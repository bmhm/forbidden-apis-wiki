# Gradle Usage Instructions #

_(since version 2.0)_ The plugin registers a separate task for each defined sourceSet using the default task naming convention.
For default Java projects, two tasks are created: `forbiddenApisMain` and `forbiddenApisTest`.
Additional source sets will produce a task with similar names (`'forbiddenApis' + nameOfSourceSet`).
All tasks are added as dependencies to the check default Gradle task. For convenience, the plugin
also defines an additional task forbiddenApis that runs checks on all source sets.

Installation can be done from your `build.gradle` file:

```gradle
buildscript {
  repositories {
    mavenCentral()
  }
  dependencies {
    classpath 'de.thetaphi:forbiddenapis:2.2'
  }
}

apply plugin: 'java'
apply plugin: 'de.thetaphi.forbiddenapis'
```

After that you can add the following task configuration closures:

```gradle
forbiddenApisMain {
  bundledSignatures += 'jdk-system-out'
}
```

(using the `+=` notation, you can add additional bundled signatures to the defaults).
To define those defaults, which are used by all source sets, you can use the extension / convention mapping provided by the corresponding extension:

```gradle
forbiddenApis {
  bundledSignatures = [ 'jdk-unsafe', 'jdk-deprecated', 'jdk-non-portable' ]
  signaturesFiles = files('path/to/my/signatures.txt')
  ignoreFailures = false
}
```

The possible `bundledSignatures` can be found on a [separate page](BundledSignatures). You can also give your own signatures in separate files from your project directory.

The detailed documentation (based on nightly snapshots) can be found here: http://jenkins.thetaphi.de/job/Forbidden-APIs/javadoc/
