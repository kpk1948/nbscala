NetBeans Plugin for Scala
=========================

## Index
1. Common Informations
 1. Where to start?
 2. Community
 3. Snapshot Builds
2. Build Instructions
3. Project Details

## Common Informations
This is a plugin of the Netbeans Platform for the [scala language](http://http://www.scala-lang.org/). In the case of a maven scala project, no local installation of scala is needed. It enables syntax checking, syntax highlighting, auto-completion, pretty formatter, occurrences mark, brace matching, indentation, code folding, function navigator, go to declaration, project management and a shell console. It's is specially useful if you are a maven user.

### Where to start ?
The project central point is [https://github.com/dcaoyuan/nbscala](). There are some other places, where the project was hosted before, but they are abandoned now.

### Community
Until recently, this has mostly been a one man project. Some patches were supplied by different people, but I'm still convinced that it will gather a community soon. For questions and bug reports use the [issue tracker](https://github.com/dcaoyuan/nbscala/issues). If interested in joining the project, you can write me directly or send patches/pull requests.

### Where to download
Released builds can be found here [https://sourceforge.net/projects/erlybird/files/nb-scala/](https://sourceforge.net/projects/erlybird/files/nb-scala/). Choose proper folder which begins with NetBeans version number then followed supported Scala version. If I have time, the plugins will also be uploaded to [http://plugins.netbeans.org](plugins.netbeans.org), and may be avaliable via the NetBeans Update Center automatically when it passed verification by NetBeans staffs.

### Installation
Make sure you don't have an old version installed. (Check your netbeans installation for a 'nbscala' directory: if it exists, delete it.)

1. Download the latest release at sourceforge.
2. Extract all files into a directory.
3. Start Netbeans.
4. Select Tools -> Plugins -> Downloaded -> Add Plugins...
5. Select all extracted files.
6. Accept the license and the installation of unsigned plugins. 

## Build Instructions
Cause of the small group of people involved in the project we only supply updates for the latest netbeans version (7.2 at the moment).

### Requirement - Run:
* Java 1.6+
* NetBeans 7.2

### Requirement - Build:
* Java 1.7 (for master branch)
* Java 1.6 (for 2.9.x branch)
* Maven 2.x/3.x 
* NetBeans 7.2

### Branches:
* master -- tracking Scala 2.10.x currently
* 2.9.x  -- for Scala 2.9.x

### Setting nb.installation property for maven
Hint: This is going to be removed in the future. There is already a nbm-application based subproject which can be used to run all modules of the plugin. See the scala.app/pom.xml for more information, what is still missing.

Make a new copy of your installed NetBeans (which will be used to run 'mvn nbm:run-ide' goal), check if there is a directory 'nbscala' under this copy, if yes, delete it. Then set 'nb.installation' property in your maven settings.xml (.m2/settings.xml) to point to this copy:

    <profiles>
        <profile>
            <id>netbeans</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <properties>
                <nb.installation>${user.home}/myapps/netbeans-71</nb.installation>
            </properties>
        </profile>
    </profiles>

### Set system environment variable for building.

    MAVEN_OPTS=-Xss8M

or even more:

    MAVEN_OPTS=-Xss8M -Xmx1024M

### Build all nbms

    cd nbscala
    mvn clean install

### Generate auto-update site:

    cd nbscala
    mvn nbm:autoupdate

the nbms and update site can be found at nbscala/target/netbeans_site

### Run/Debug ide:

    cd nbscala
    mvn nbm:cluster

To run:

    mvn nbm:run-ide

To debug:

    mvn nbm:run-ide -Pdebug-ide

Build-Run-Cycle: (after changed module was successfuly built)
	
    mvn nbm:cluster nbm:run-ide

Build-Debug-Cycle: (after changed module was successfuly built)

    mvn nbm:cluster nbm:run-ide -Pdebug-ide

### Installation Notes:

 * After installation, it's always better to restart NetBeans
 * You may need to delete NetBeans' old cache to get improved features working. To find the cache location, read the netbeans.conf at:

        NetBeansInstallationPlace/etc/netbeans.conf

## Project Details

The Project targets version 2.10.x of the scala release.



## Scala Console Integration

### A new Scala shell console was implemented recently (since Feb 27, 2013)

### To open it, right click on project, and choose "Open Scala Console"

### Features:

* Be aware of project's classpath that could be imported, new, run under console
* Popup auto-completion when press \<tab\>
* Applied also to Java SE projects and Maven projects



## Sbt Integration

### Only Scala-2.10+ is supported under NetBeans

* That is, always try to set your project's Scala version to 2.10+ in Build.scala or build.sbt: 

        scalaVersion := "2.10.0"

### Supported features

* Recognize sbt project and open in NetBeans
* Open sbt console in NetBeans (Right click on sbt project, choose "Open Sbt")
* Jump to/Open compile error lines

### How to

* Install the newest nbscala plugins, [download directly](https://sourceforge.net/projects/erlybird/files/nb-scala) or [build by yourself](https://github.com/dcaoyuan/nbscala) on NetBeans 7.2+.
* Git clone, build and publish-local a NetBeans special sbt plugin <https://github.com/dcaoyuan/nbsbt>:

        git clone git@github.com:dcaoyuan/nbsbt.git
        cd nbsbt
        sbt clean compile publish-local

* Add nbsbt to your plugin definition file. You can use either the global one at  **~/.sbt/plugins/plugins.sbt** or the project-specific one at **PROJECT_DIR/project/plugins.sbt**

        addSbtPlugin("org.netbeans.nbsbt" % "nbsbt-plugin" % "1.0.2")


## FAQ

**Q**: NetBeans' response becomes slower after a while.

**A**: Edit your NetBeans configuration file (NetBeansInstallationPlace/etc/netbeans.conf), add -J-Xmx1024M (or bigger)

**Q**: I got:

    [error] sbt.IncompatiblePluginsException: Binary incompatibility in plugins detected.

**A**: Try to remove published nbsbt plugin from your local .ivy2 repository and sbt plugins cache:

    rm -r ~/.ivy2/local/org.netbeans.nbsbt
    rm -r ~/.sbt/plugins/target

and redo 'publish-local' for the NetBeans sbt plugin <https://github.com/dcaoyuan/nbsbt>.


**Q**: I got:

    [error] Not a valid command: netbeans
    [error] Expected '/'
    [error] Expected ':'
    [error] Not a valid key: netbeans (similar: test, tags, streams)
    [error] netbeans
    [error]         ^

**A**: Try to remove the project/target folder under your project base directory, there may be something cached here, and was not reflected to the newest condition.


**Q**: What will this plugin do upon my project?

**A**: It will generate a NetBeans project definition file ".classpath_nb" for each project.


**Q**: It seems there are some suspicious error hints displayed on the edited source file, how can I do?

**A**: There may be varies causes, you can try open another source file, then switch back to this one, the error hints may have disappeared. If not, right click in editing window, choose 'Reset Scala Parser', and try the steps mentioned previous again.


**Q**: My project's definition was changed, how to reflect these changes to NetBeans.

**A**: Right click on the root project, choose "Reload Project".


**Q**: Exiting from Scala console leaves terminal unusable.

**A**: Under some unix-like environment, scala interactive console started with some stty setting, but not for NetBeans's integrated one. You can try 'reset' after quit from NetBeans.
