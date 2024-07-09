\_\_NOTOC\_\_

This page contains **installation instructions** for
[Henshin](Henshin "wikilink"). Henshin is a plug-in for recent versions
of the Eclipse Modeling Tools (last tested version: 2022-12). To obtain
the most recent Eclipse version, go to the [download
page](https://www.eclipse.org/downloads/packages/) and download the
Eclipse Modeling Tools.

Henshin is generally installed via update site. Hence, to install
Henshin, open Eclipse, go to *Help -\> Install New Software\...* and use
one of the following update site URLs:

-   **Current release**:
    <http://download.eclipse.org/modeling/emft/henshin/updates/release>
    (1.8.0)
-   **Nightly build**:
    <http://download.eclipse.org/modeling/emft/henshin/updates/nightly>\

**Features** At a minimum, install the Henshin SDK feature. Version
1.6.0 and 1.8.0 introduced the additional features Henshin Variability
SDK and OCL2AG. For all features, a source version is provided.

**JDK Compatibility** From release 1.8.0, Henshin is compatible with JDK
versions from 11.0 and higher. The release variant *1.8.0-legacyjdk*
(see below) and previous releases are compatible with JDK 8.0 to JDK
14.0.

## Legacy versions {#legacy_versions}

Permanent URLs of current and previous releases:

-   <http://download.eclipse.org/modeling/emft/henshin/updates/1.8.0>
-   <http://download.eclipse.org/modeling/emft/henshin/updates/1.8.0-legacyjdk>
-   <http://download.eclipse.org/modeling/emft/henshin/updates/1.6.0>
-   <http://download.eclipse.org/modeling/emft/henshin/updates/1.4.0>
-   <http://download.eclipse.org/modeling/emft/henshin/updates/1.2.0>
-   <http://download.eclipse.org/modeling/emft/henshin/updates/1.0.0>
-   <http://download.eclipse.org/modeling/emft/henshin/updates/0.9.2>

\
If you want to install a legacy version of Henshin (\<= 1.4.0) in a
newer Eclipse version (\>= Eclipse Oxygen), please use the following
instructions (thanks to [Joel Greenyer](http://jgreen.de/) for providing
these instructions):

-   Install GMF Tooling v3.2.1 via [Update
    site](http://download.eclipse.org/modeling/gmp/gmf-tooling/updates/releases-3.2.1/)
    *(right click -\> copy\...)*
-   Install M2Eclipse via [Update
    site](http://download.eclipse.org/technology/m2e/releases) *(right
    click -\> copy\...)*
-   Install the required Henshin version.

## Sources

You can also get the latest version of the source code directly from our
Git repository (weblink):

-   <http://git.eclipse.org/c/henshin/org.eclipse.emft.henshin.git/>

## Build Jobs {#build_jobs}

Release and nighly builds are compiled using Hudson jobs:

-   Releases: <https://ci.eclipse.org/henshin/job/cbi_henshin_release/>
-   Nightly Builds:
    <https://ci.eclipse.org/henshin/job/cbi_henshin_nightly/>

[category:Henshin](category:Henshin "wikilink")
