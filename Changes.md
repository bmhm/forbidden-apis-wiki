This page lists all changes since the first released version.

# Version 1.8 (released 2015-04-13) #

**New features:**
  * Initial Java 9 support (JIGSAW modules, signatures, deprecations) ([issue #39](../issues/39), [pull #50](../pull/50)).
  * Add annotation support (`@SuppressForbidden`) to suppress errors for classes/methods/fields. Annotations can be defined in config, also using glob patterns (e.g., `**.MySuppressForbidden`) ([issue #34](../issues/34), [issue #53](../issues/53)).
  * Forbid `MessageFormat.format(String,Object[])` because it uses default locale ([pull #48](../pull/48), thanks to Shalin Shekhar Mangar).
  * Add support for forbidding java packages using class name globs (e.g., `sun.misc.**`). ([issue #40](../issues/40)).
  * Sort error messages by source code line number. Also add synthetic methods and lambdas where they belong. ([issue #12](../issues/12)).

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