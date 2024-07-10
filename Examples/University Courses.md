
The purpose of [Henshin](Home "wikilink")\'s **University Courses
example** is to assign university courses to lecturers and students
while avoiding time conflicts. It is also meant to be an accessible
example for the usage of [Units](Units "wikilink"). This example
wants to showcase as many units as possible. Therefore please pardon
that some units\' usage may seem unnecessarily complicated. The
transformation model, example input models and source code can be found
[here](http://git.eclipse.org/c/henshin/org.eclipse.emft.henshin.git/tree/plugins/org.eclipse.emf.henshin.examples/src/org/eclipse/emf/henshin/examples/universitycourses).

## Metamodel

[[/images/henshin-universityCourses-model.png]]}

The metamodel describes a *University* which contains *Courses* and
*Persons*.. A *Course* has a name, belongs to a university and can be of
two types. An *OfferedCourse* represents a course which can be staged in
the next lecture period. A *ScheduledCourse* represents a course which
is staged in the next lecture period with the hour it is starting. For
the sake of simplicity, we consider one generic weekday only, moreover,
every course duration is one hour. Furthermore it is required to have at
least one lecturer. A *Lecturer* is a person who can teach offered
courses and teaches scheduled courses. A *Student* is a person who is
interested in courses. The *Temp* object contained in a university is
defined to save transient information of course scheduling. The
metamodel is defined in the file *universityCourses.ecore*.

### Instance Model Restrictions

Instance models - which should be transformed with the transformation
rules below - have to meet the following requirements:

-   The instance model should contain at least a *University* object.
-   The name of a *Course* is considered to be unique. There must not be
    any pair of *OfferedCourse/ScheduledCourse* objects which share the
    same name.
-   There must not be any time conflicts for a *Lecturer* or *Student*
    and their associated *ScheduledCourse*s.

## Henshin Rules and Units

[[/images/henshin-universityCourses-planAllCoursesOrFail.png]]}

The *SequentialUnit* **planAllCoursesOrFail** (*strict=true*,
*rollback=false*) executes the rule *existsUnscheduledInterestingCourse*
first. The rule **existsUnscheduledInterestingCourse** checks the
existence of an *OfferedCourse* having an interested *Student* as well
as a *Lecturer* capable of teaching the *Course*. If this application is
successful, *planCourseOrIncrement* is executed.

The *ConditionalUnit* **planCourseOrIncrement** tries to apply the
sub-unit *planOneCourse*. If it is successfully applicable,
*planCourseOrIncrement* basically calls itself recursively to attempt
*planOneCourse* again by calling the *ConditionalUnit*
**planUnscheduledInterestingCourses**. This unit uses
*existsUnscheduledInterestingCourse* so that the application terminates
as soon as all interesting courses are scheduled. In contrast to the
very similar (*strict*) *SequentialUnit* *planAllCoursesOrFail*, this
*ConditionalUnit* does not fail if no unscheduled courses exist. If it
failed after all courses have been scheduled, the previous execution of
*planCourseOrIncrement* would also be unsuccessful and would lead to the
rescission of the last *planOneCourse* execution. In consequence there
would always remain one unscheduled but interesting course after the
successful application of *planAllCoursesOrFail*. Therefore the
*ConditionalUnit* *planUnscheduledInterestingCourses* - which never
fails after an unsuccessful application of its *if*-sub-unit - is used
inside *planCourseOrIncrement*.

[[/images/henshin-universityCourses-incrementHour.png]]}

If *planOneCourse* is not applicable, the *ConditionalUnit*
**incrementIfPossible** first checks whether incrementing the current
hour is possible by applying the rule **incrementPossible**. After a
successful application of *incrementPossible* the *SequentialUnit*
*incrementHour* is applied in the *SequentialUnit*
**incrementAndContinue**. The unit **incrementHour** raises the
passed-in *hour* parameter by one and returns the result via the
parameter *oneMore*. After successfully incrementing the hour value
*planCourseOrIncrement* is called again. In the end
*planCourseOrIncrement* should only terminate with the maximum number of
courses (with prospective students) scheduled between the starting time
and the end of the day.

[[/images/henshin-universityCourses-planOneCourse.png]]}

**planOneCourse** is a *SequentialUnit* with the flags *strict=true* and
*rollback=true*. First, it schedules a course which prevents time
conflicts for lecturers by fixing the time using the rule
*scheduleOfferedCourse*. Furthermore it attempts to associate all
interested students without time conflicts with the scheduled course.
Afterwards *planOneCourse* checks the absence of interested students
with time conflicts by applying the rule
*isScheduledCourseConflictFree*. At last, the according *OfferedCourse*
is removed by an application of *removeOfferedCourseAfterScheduling*.
This unit may fail in *scheduleOfferedCourse* if there is no lecturer
associable without time conflict or in *isScheduledCourseConflictFree*
if there is an interested student with time conflict. Due to the flag
*strict=true*, the *SequentialUnit* application stopps at the failed
rule and due to the flag *rollback=true* it reverts all applied
transformations. This means that *planOneCourse* is applied only if
there is no time conflict for students or lecturers.

The rule **scheduleOfferedCourse** tries to find an *OfferedCourse* in
which at least one *Student* is interested in and which can be taught by
at least one *Lecturer*. The *in*-parameter *hour* defines the hour to
which the course can be scheduled. The *Lecturer* must not teach another
*ScheduledCourse* at the same time (here called *startingHour*). If
those requirements are met, a new *ScheduledCourse* with the same name
as the *OfferedCourse* is created at the given time. At the same time
the rule attempts to move students to the ScheduledCourse. If a
*Student* is not interested in another conflicting *ScheduledCourse*
he/she will get associated with the *ScheduledCourse* instead of the
*OfferedCourse*.

The rule **isScheduledCourseConflictFree** checks whether there is no
student interested in an *OfferedCourse* with corresponding
*ScheduledCourse* and who is interested in a *ScheduledCourse* at the
same time. This rule basically detects *Student*s who cannot be treated
by *moveStudentsToScheduledCourse*. The rule
**removeOfferedCourseAfterScheduling** (*checkDangling=false*) deletes
an *OfferedCourse* that is scheduled.

[[/images/henshin-universityCourses-manageCourses.png]]}

The starting point for the application of this example is the
*IteratedUnit* **manageCourses** with the flag *strict* set to false.
The *in*-parameter *startHour* in this and all other units sets the
earliest hour to which courses can be scheduled. This unit tries to
apply its sub-unit *planOrCleanup* two times which shall ensure that
each of the two sub-units of *planOrCleanup* is applied at least once.
**planOrCleanup** is a *PriorityUnit* with the sub-units
*planAllCoursesOrFail* and *cleanupUninterestingCourses*. The former
sub-unit can be applied *successfully* at most once. This means that the
rule **cleanupUninterestingCourses** which removes *OfferedCourse*s
without prospective students (*checkDangling=false*) is applied in the
second iteration at latest.

### Loop Unit vs. Nested Rule

[[/images/henshin-universityCourses-cleanupUninterestingCoursesUnit.png]]}

The rule *cleanupUninterestingCourses*, which is realised using a nested
rule, can alternatively be replaced by a *LoopUnit*
*cleanupUninterestingCoursesUnit*. This unit contains the multi-rule of
the formerly nested rule and is named *cleanupUninterestingCourse* here
(singular!). The expression with a nested rule is preferreable in this
case because of its greater simplicity. But for a unit (instead of a
rule) which has to be executed as many times as possible a *LoopUnit*
has to be used necessarily.

*contributed by Benjamin Heidelmeier and Gabriele Taentzer*

*PLEASE NOTE: This example does not work reliably with the Henshin 1.4
release. Try a newer release or the current nightly build.*


