This page lists all changes since the first released version.

[![Maven Central](https://img.shields.io/maven-central/v/de.thetaphi/forbiddenapis.svg)](https://search.maven.org/#search%7Cga%7C1%7Cg%3A%22de.thetaphi%22%20AND%20a%3A%22forbiddenapis%22)

# Version 3.0.1 (released 2020-06-03) #

**Bug fixes:**
  * Fix `Method not found while parsing signature: org.apache.commons.io.output.ByteArrayOutputStream#toString()`
    happening when using `commons-io-unsafe` (any version) with version 2.7 of the library on classpath.
    Due to a (still binary compatible) change (move of method to abstract superclass), signatures parsing
    was broken ([issue #168](../issues/168), [pull #169](../pull/169)), thanks to Quentin Caillard.
    A later version will have a better fix for this. This bugfix release adds support for the
    new library version by adding bundled signatures `commons-io-unsafe-2.7`.

**Maintenance:**
  * Upgrade ASM 8.0 to ASM 8.0.1

# Version 3.0 (released 2020-04-27) #

This is the major 3.0 release of the forbidden-apis plugin. The main new feature is
support for the Gradle build cache and support for task configuration avoidance.
The new version also supports Java 14.

**Breaking changes:**
  * Update to Java 7 as minimum requirement ([pull #160](../pull/160)).
  * Update to Gradle 3.2 as minimum requirement ([pull #155](../pull/155)),
    thanks `mkmaier`.
  * Removal of all deprecated task settings/properties ([pull #161](../pull/161)).

**New features:**

  * Add Java 14 support by upgrading to ASM 8.0. This includes updated signatures files
    and build system changes.
  * Add support for lazy tasks API / task configuration avoidance. Starting with
    Gradle 4.9, forbiddenapis will use the new lazy tasks API, so tasks are not
    generated in the configuration phase unless they are needed in the task graph.
    Older Gradle versions still use the old API ([issue #145](../issues/145),
    [pull #162](../pull/162)), thanks to Ryan Ernst and Thomas Broyer.
  * Allow to silently ignore signatures if a class was not found. This allows
    to silence multi-module Maven/Gradle builds by adding a new setting to Gradle,
    Maven, Ant tasks/mojos: `ignoreSignaturesOfMissingClasses`. In contrast to the now
    deprecated `failOnUnresolvableSignatures`, it prints no warnings and at the same
    time it enforces that method signatures are correct (if the class was found).
    The previous setting had the big problem that it was often used for multi-module
    builds only to workaround that some classes are missing in submodules, but the
    (now-deprecated) setting `failOnUnresolvableSignatures=false` disabled *all* signatures
    checking, so incorrect method descriptors (wrong parameters,...) were hidden on
    parsing. This lead to unseen violations, just because some method parameter was
    mistyped ([pull #164](../pull/164)). See also [issue #83](../issues/83) for
    details. Thanks to Joep Weijers, Matthew Janssen, Dawid Weiss.
  * Maven task now hides the warning if the classes directory/fileset is missing
    ([pull #163](../pull/163)), thanks Joep Weijers

**Bug fixes:**
  * Fix some Gradle task annotations, so the build cache works correctly
    ([pull #159](../pull/159), [pull #155](../pull/155)),
    thanks `mkmaier` and Vladimir Sitnikov.
    
**Internals:**
  * Use `https` for `repo.gradle.org` ([pull #154](../pull/154)), thanks `mkmaier`.
  * Add some hacks to bootstrap the Maven 2 build parts (which uses `http` instead of `https`
    to bootstrap itsself, which is no longer supported by Maven Central).

_Please note:_ `repo.gradle.org` now requires TLS 1.2, which is not included in Java 7.
The same applies for `repo1.maven.org`. To build with Java 7, the system downgrades all
URLs to `http` and uses a separate replacement server for Maven Central. Unfortunately
this does not work with Gradle's repository. For safety reasons and also to actually
bootstrap the build, run everything one time with at least Java 8+. This will download
all artifacts through a secure TLS 1.2 connection and cache them locally. To build a 
release, Java 7 can then be used to run the build with recommended minimum version.

# Version 2.7 (released 2019-10-12) #

**New features:**

  * Add Java 12 and 13 support by upgrading to ASM 7.2. This includes updated signatures files
    and build system changes (shipped documentation is currently missing reference to
    Java 12 and 13, sorry).
  * Replace jdk-unsafe class signatures of `java.io.FileReader` and `java.io.FileWriter`
    by explicit constructor signatures since there are now safe methods with `Charset` that
    can be used since Java 11 ([pull #152](../pull/152)), thanks Andreas Asplund. This allows
    to use those classes in checked code starting from Java 11.

**Bug fixes:**
  * Do not emit "and 0 more" in ignored signatures message
    ([pull #148](../pull/148)), thanks Dawid Weiss.
    
**Internals:**
  * Add `InputStream#skip(long)` to forbiddenapi's own checks because it is flaky.
    ([pull #150](../pull/150)), thanks Tim Allison.
  * Update Groovy to latest 2.4.17 bugfix release.

_Please note:_ There is Java 12 and Java 13 support, but as of limitations (no Java 6 compile support anymore),
it can not be compiled/built with those Java versions. Creating deprecated/internal signatures files
needs to bootstrap with earlier Java version. Once the tools classes are built, run them in a second
step with targeted Java version.

# Version 2.6 (released 2018-09-17) #

**New features:**
  * Add Java 11 support by upgrading to ASM 6.2.1. This includes updated signatures files
    and build system changes (shipped documentation is currently missing reference to
    Java 11, sorry).
  * The plugin was published on the [Gradle Plugin portal](https://plugins.gradle.org/plugin/de.thetaphi.forbiddenapis).
    Due to a bug in plugin initialization ([issue #144](../issues/144)), older versions may not fully work.
    Thanks to Sebastian Davids and the Gradle team for help and pushing this forward!
  * When signatures are using classes that are not found on classpath, the option to ignore
    those warnings is no longer so noisy: It only lists all failed signatures separately
    where methods/fields do not exit, but the missing classes are reported only with a
    single line ([issue #83](../issues/83), [pull #140](../pull/140)), thanks ghost,
    Tim Allison, Dawid Weiss.
  * Report more details about class that failed to load while scanning the classes
    to be checked ([issue #142](../issues/142)), thanks Trejkaz.
  * Only print warning about Gradle daemon once.

**Bug fixes:**
  * Gradle: Fix SourceSets added after plugin loading don't get tasks
    ([issue #138](../issues/138), [pull #139](../pull/139)).
  * Add a hack for the broken JavaVersion enum in Gradle. It returns version "1.9" and "1.10"
    (which are invalid targetCompatibility versions) and this problem is only fixed after Java 11.
    Before Java 11 we switch to plain "majorVersion" property
    ([issue #143](../issues/143)), thanks Sebastian Davids.
  * Gradle: Always apply JavaBasePlugin, otherwise startup may fail with new plugin DSL,
    as initialization order is undefined ([issue #144](../issues/144)), thanks Sebastian
    Davids.

**Internals:**
  * Refactor signatures file parsing and lookup of signatures (remove from Checker).
  * Workaround for Java 1.6 (minimum version) builds with Maven Central TLS settings.
  * Cleanup Gradle plugin initialization exception handling, check minimal Gradle version.

# Version 2.5 (released 2018-03-28) #

**New features:**
  * Add Java 10 support by upgrading to ASM 6.1.1 ([issue #136](../issues/136)).
    This includes updated signatures files and build system changes.
  * Add signatures for `commons-io-unsafe-2.6` ([pull #133](../pull/133)), thanks to Jochen Schalanda.
  * Improve documentation about custom signatures files ([issue #135](../issues/135)), thanks
    Roman Leventov.

**Internals:**
  * Add local signatures file to check ourselves (e.g., forbid direct use of ASM's `ClassReader` constructors).
  * Simplify class file patching by loading bytecode of class files into byte arrays. This also works
    around a slowness bug in ASM 6.1.
  * Upgrade to Groovy 2.4.15.

# Version 2.4.1 (released 2017-10-04) #

**Bug fixes:**
  * Fix regression in Gradle Task: Dependencies on `SourceSet` outputs were not correctly setup
    ([issue #131](../issues/131)).

# Version 2.4 (released 2017-10-03) #

*Warning:* This version is buggy, please use bugfix release 2.4.1!

**New features:**
  * Fix deprecation warning with Gradle 4. This requires changes in the Gradle task:
    `classesDir` property  was **deprecated** and replaced by `classesDirs`. Most users
    will not see a change, as this property is only used in advanced setups.
    The new property is now a `FileCollection` containing all classes output dirs
    of the task's `SourceSet` ([issue #120](../issues/120), [pull #124](../pull/124)),
    thanks to Jörn Huxhorn for reporting. See [potential breaking changes in
    Gradle 4](https://docs.gradle.org/4.0/release-notes.html#potential-breaking-changes).
  * Update ASM to version 6.0 and remove class file patching. This adds full Java 9 support.
  * Change source/target Java version normalization to respect new Java version numbering scheme.
    `targetVersion` config property can now be `'1.6', '6', '1.7', '7', '1.8', '8', '9'`, while
    the bundled signatures files now use official version numbers ([issue #130](../issues/130)).
  * Improve usage of Module API in Java 9 as fallback for future Java versions
    ([issue #118](../issues/118)). This allows usage in newer Java runtimes without
    upgrading ASM.
  * Add documentation about custom signatures files ([issue #128](../issues/128)), thanks
    Roman Leventov.

# Version 2.3 (released 2017-02-13) #

**New features / bug fixes:**
  * Improve Gradle startup time: `apply plugin: 'de.thetaphi.forbiddenapis'` was
    changed to compile `plugin-init.groovy` only once ([issue #116](../issues/116),
    [pull #117](../pull/117)), thanks to Jörn Huxhorn.
  * Update Groovy to version 2.4.8 for Java 9 compatibility.
  * Update ASM to version 5.2.
  * Update Java 9 bundled signatures files.

# Version 2.2 (released 2016-06-19) #

**New features / bug fixes:**
  * Add support for signature polymorphic methods, like `MethodHandle.invoke()`
    ([issue #105](../issues/105), [pull #106](../pull/106)), thanks to Robert.
  * Add signatures for `commons-io-unsafe-2.5` ([issue #102](../issues/102)).
  * Add some missing methods to `commons-io-unsafe` signatures after another review ([pull #104](../pull/104)).
  * Add new setting to disable the classloading cache. This is a workarounds for build systems that change JAR files,
    but don't close their classloaders, leading to `FileNotFoundException` when trying to load class files from
    the changed JAR file. This affects especially the Gradle Daemon ([issue #75](../issues/75), [pull #76](../pull/76)).

# Version 2.1 (released 2016-05-22) #

**New features:**
  * Add targetVersion support to Ant task ([issue #101](../issues/101)).
  * Add initial support for new Java 9 class file format ([pull #97](../pull/97)).
  * On Java 9 Jigsaw, don't use ASM to analyze system classes. This change makes it
    more unlikely that the tool will exit with "Bundled version of ASM cannot parse bytecode
    of java.lang.Object class; marking runtime as not suppported" on Java 9+
    ([commit](../commit/95d9d55ca49f78e9f2a0da824a10ddd07b6d151f)).
  * Deprecate `internalRuntimeForbidden` attribute and add a new bundles signatures `jdk-non-portable`
    to and make heuristics reliable ([pull #95](../pull/95), [issue #54](../issues/54)).
  * Add new bundled signatures `jdk-internal` for disallowing internal runtime APIs ([issue #91](../issues/91), [pull #95](../pull/95)).
  * Add unsafe signatures for ResourceBundle ([issue #89](../issues/89)), thanks to Trejkaz.
  * Add a bundle `jdk-reflection` that contains methods that bypass Java security ([pull #86](../pull/86)), thanks to Dominik Stadler.
  * Add support for new Java version style "6.0" instead of "1.6" ([pull #81](../pull/81)).

**Bug fixes:**
  * Fix method checks to also look into superclasses if method was overridden ([issue #100](../issues/100)).
  * Fix class loading order bugs ([issue #91](../issues/91)).
  * Allow referencing non-jarred artefacts ([pull #87](../pull/87)), thanks to Dawid Weiss.
  * Hide warnings about missing classes/methods/fields in deprecated signatures ([pull #84](../pull/84))

**Internals:**
  * Update to [ASM 5.1](../commit/a235498503d01dfda8030dd978eb278a693d84c0)

# Version 2.0 (released 2015-09-30) #

This is the major 2.0 release of the forbidden-apis plugin. The main new
feature is native support for the [Gradle](https://gradle.org/) build system (minimum requirement is Gradle 2.3).
But also Apache Ant and Apache Maven build systems got improved support: Ant can now load signatures from
arbitrary resources by using a new XML element `<signatures></signatures>` that may contain
any valid ANT resource, e.g., ivy's cache-filesets or plain URLs. Apache Maven now supports
to load signatures files as artifacts from your repository or Maven Central (new `signaturesArtifacts`
Mojo property).

**Breaking changes:**
  * Update to Java 6 as minimum requirement ([pull #71](../pull/71)).
  * Switch default Maven lifecycle phase to `verify` ([pull #72](../pull/72)).

**New features:**
  * Add support for Gradle ([issue #68](../issues/68), [pull #70](../pull/70), [pull #73](../pull/73), thanks to Ryan Ernst & Chris Earle for help and suggestions).
  * Allow Maven artifacts to be loaded as signatures ([issue #13](../issues/13), [pull #79](../pull/79)).
  * Allow arbitrary Ant resources to be loaded as signatures ([pull #78](../pull/78)).
  * Add `failOnViolation` setting to optionally fail builds ([pull #62](../pull/62), thanks to Jochen Schalanda).
  * Cleanup unsafe JDK signatures for Java 8 (mostly `java.time` API) ([issue #19](../issues/19), [pull #57](../pull/57)).
  * Support for Java 9 Jigsaw preview builds ([pull #74](../pull/74)).

**Bug fixes:**
  * Add automatic plugin execution override for M2E. It is no longer needed to add a lifecycle mapping to exclude forbiddenapis to execute inside Eclipse's M2E ([issue #60](../issues/60)).

**Internals:**
  * Package refactoring of Cli, Ant, Maven ([pull #69](../pull/69)).
  * Refactor loggers and make Checker a final, non abstract class ([pull #67](../pull/67)).
  * Use EnumSet for the checker options ([pull #64](../pull/64)).
  * Add support for signature file URLs ([pull #77](../pull/77)).

# Version 1.8 (released 2015-04-13) #

**New features:**
  * Initial Java 9 support (JIGSAW modules, signatures, deprecations) ([issue #39](../issues/39), [pull #50](../pull/50)).
  * Add annotation support (`@SuppressForbidden`) to suppress errors for classes/methods/fields. Annotations can be defined in config, also using glob patterns (e.g., `**.MySuppressForbidden`) ([issue #34](../issues/34), [issue #53](../issues/53)).
  * Forbid `MessageFormat.format(String,Object[])` because it uses default locale ([pull #48](../pull/48), thanks to Shalin Shekhar Mangar).
  * Add support for forbidding java packages using class name globs (e.g., `sun.misc.**`) ([issue #40](../issues/40)).
  * Sort error messages by source code line number. Also show failures in synthetic methods and lambdas where they belong ([issue #12](../issues/12)).

**Bug fixes:**
  * Forbidden `@java.lang.Deprecated` is not always detected ([issue #45](../issues/45)).
  * Re-enable class-only, non-runtime annotation checking ([issue #46](../issues/46)).

# Version 1.7 (released 2014-11-24) #

**New features:**
  * Auto-generate HTML documentation for Ant Task, Maven Mojo, and CLI ([issue #32](../issues/32), [issue #37](../issues/37)).
  * Add a new documentation ZIP file to release ([issue #32](../issues/32), [issue #37](../issues/37)).
  * Add help-mojo ([issue #32](../issues/32), [issue #37](../issues/37)).
  * Add support for signaturesFilelist and signaturesFile elements ([issue #36](../issues/36)).
  * Add option to also ignore unresolvable signatures in Ant and CLI ([issue #42](../issues/42)).
  * Allow option to only print warning if Ant fileset of classes to scan is empty (like Maven) ([issue #35](../issues/35)).

**Bug fixes:**
  * Fix bug that deprecated signatures of Java 8 fail to load on Java 9 ([issue #41](../issues/41)).

**Backwards compatibility:**
  * Remove deprecated Mojo of version 1.0 ([issue #33](../issues/33)).
  * Rename some CLI options to be consistent with others ([issue #42](../issues/42)).

# Version 1.6.1 (released 2014-08-05) #

**Bug fixes:**
  * Fix wrong plugin descriptor in Maven artifact. No code change, just new binary artifact.

# Version 1.6 (released 2014-08-04) #

**New features:**
  * Option to skip the execution of the plugin (pass `mvn -Dforbiddenapis.skip=true`) ([issue #29](../issues/29)).
  * Maven plugin should log warning if target version is not set ([issue #28](../issues/28)).

**Other changes:**
  * Upgrade ASM to bugfix release 5.0.3.

# Version 1.5.1 (released 2014-04-17) #

**Bug fixes:**
  * Fix regression caused by [issue #8](../issues/8) with non-runtime visible annotations (e.g. `java.lang.Synthetic`) which are not in classpath ([issue #27](../issues/27)). This hotfix disables detection of class-file only annotations. Annotations that need to be detected must have `RetentionPolicy.RUNTIME` to be visible to forbidden-apis.
  * Improve logging when no line numbers are available.

# Version 1.5 (released 2014-04-16) #

**New features:**
  * Make it possible to ban annotations ([issue #8](../issues/8)).

**Bug fixes:**
  * Upgrade ASM to bugfix release 5.0.1.
  * Forbidden class use does not work in field declarations and method declarations ([issue #25](../issues/25)).
  * Fix lookup of class references in `checkType()` / `checkDescriptor()` to also inspect superclasses and interfaces ([issue #26](../issues/26)).

# Version 1.4.1 (released 2014-03-19) #

**New features:**
  * Upgrade to ASM 5.0 ([issue #24](../issues/24)).
  * Full support for Java 8: Update deprecated signatures with final version of JDK 1.8.0; recompile and verify test classes.

**Bug fixes:**
  * Add some missing unsafe signatures ([issue #22](../issues/22)).

# Version 1.4 (released 2013-11-21) #

**New features:**
  * Upgrade to ASM 5.0 BETA	([issue #18](../issues/18)).
  * Add Java 8 deprecated + unsafe signatures ([issue #16](../issues/16)).
  * Detect references to invokeDynamic using method handles to forbidden methods ([issue #11](../issues/11)).
  * Skip execution for Maven projects with packaging "pom" (Maven only, [issue #10](../issues/10)).
  * Add an option to ignore unresolvable signatures (Maven only, [issue #14](../issues/14)).
  * Enhance the target parameter to also support testTarget like maven-compiler-plugin (Maven only, [issue #15](../issues/15)).

**Bug fixes:**
  * Fix missing methods in commons-io ([issue #9](../issues/9)).

**Optimizations:**
  * Improve memory usage ([issue #20](../issues/20)).

# Version 1.3 (released 2013-04-28) #

**New features:**
  * Preliminary support for Java 8 ([issue #7](../issues/7), tested with preview build 86 of the Oracle Java 8 JDK): The tool can now read Java 8 class files and detects usage of forbidden APIs in default interface methods and closures. It does not yet ship with signature files for Java 8, as the API is not yet official.

**Optimizations:**
  * Reduced binary JAR size by using non-debug ASM version.

# Version 1.2 (released 2013-02-16) #

**New features:**
  * Validating test classes is now supported by the Maven Mojo. The goals were renamed to "check" and "testCheck" ([issue #4](../issues/4)).

**Bug fixes:**
  * fixed [issue #5](../issues/5) (Apple-provided JDK 1.6 on MacOSX was detected as "unsupported"). The algorithm to get the bootclasspath was improved to support those JDK versions.

# Version 1.1 (released 2013-02-11) #

**New features:**
  * added a Command Line Interface (CLI, [issue #3](../issues/3)).

**Bug fixes:**
  * fixed [issue #1](../issues/1) (the bundled signature `jdk-system-out` was not working in Maven).
  * fixed [issue #2](../issues/2) (the Ant task was incorrectly failing to execute on empty task tag `<forbiddenapis internalRuntimeForbidden="true" dir="..."/>`, although the checks for internal api calls are enabled).

# Version 1.0 (released 2013-02-04) #

Initial release, including support for Apache Ant, Apache Maven.