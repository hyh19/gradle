Gradle 在 `buildSrc` 目录下使源文件结构标准化。Java 代码需要放在 `src/main/java` 目录下，Groovy 代码则放在 `src/main/groovy` 目录下。位于这些目录下的代码都会被自动编译，并且被加入到 Gradle 构建脚本的 `classpath` 中。

```
.
|____build.gradle
|____buildSrc
| |____src
| | |____main
| | | |____groovy
| | | | |____com
| | | | | |____manning
| | | | | | |____gia
| | | | | | | |____ProjectVersion.groovy
| | | | | | | |____ReleaseVersionTask.groovy
|____version.properties
```

**ReleaseVersionTask.groovy**
```groovy
package com.manning.gia

// 需要从 Gradle API 导入类
import org.gradle.api.DefaultTask
import org.gradle.api.tasks.Input
import org.gradle.api.tasks.OutputFile
import org.gradle.api.tasks.TaskAction

class ReleaseVersionTask extends DefaultTask {
    @Input Boolean release
    @OutputFile File destFile

    ReleaseVersionTask() {
        group = 'versioning'
        description = 'Makes project a release version.'
    }

    @TaskAction
    void start() {
        project.version.release = true
        ant.propertyfile(file: destFile) {
            entry(key: 'release', type: 'string', operation: '=', value: 'true')
        }
    }
}
```

**ProjectVersion.groovy**
```groovy
package com.manning.gia

class ProjectVersion {
    Integer major
    Integer minor
    Boolean release

    ProjectVersion(Integer major, Integer minor) {
        this.major = major
        this.minor = minor
        this.release = Boolean.FALSE
    }

    ProjectVersion(Integer major, Integer minor, Boolean release) {
        this(major, minor)
        this.release = release
    }

    @Override
    String toString() {
        "$major.$minor${release ? '' : '-SNAPSHOT'}"
    }
}
```

**build.gradle**
```gradle
// 构建脚本需要从 buildSrc 导入编译过的类
import com.manning.gia.ProjectVersion
import com.manning.gia.ReleaseVersionTask

ext.versionFile = file('version.properties')
project.version = readVersion()

ProjectVersion readVersion() {
    logger.quiet 'Reading the version file.'

    if (!versionFile.exists()) {
        throw new GradleException("Required version file does not exit: $versionFile.canonicalPath")
    }

    Properties versionProps = new Properties()

    versionFile.withInputStream { stream ->
        versionProps.load(stream)
    }

    new ProjectVersion(versionProps.major.toInteger(), versionProps.minor.toInteger(), versionProps.release.toBoolean())
}

task makeReleaseVersion(type: ReleaseVersionTask) {
    release = version.release
    destFile = versionFile
}
```

结果：
```
$ gradle makeReleaseVersion
:buildSrc:clean UP-TO-DATE
:buildSrc:compileJava UP-TO-DATE
:buildSrc:compileGroovy
:buildSrc:processResources UP-TO-DATE
:buildSrc:classes
:buildSrc:jar
:buildSrc:assemble
:buildSrc:compileTestJava UP-TO-DATE
:buildSrc:compileTestGroovy UP-TO-DATE
:buildSrc:processTestResources UP-TO-DATE
:buildSrc:testClasses UP-TO-DATE
:buildSrc:test UP-TO-DATE
:buildSrc:check UP-TO-DATE
:buildSrc:build
Reading the version file.
:makeReleaseVersion
```
