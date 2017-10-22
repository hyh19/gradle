```
省略...
// 注册的生命周期钩子在 task 图生成后被调用
gradle.taskGraph.whenReady { TaskExecutionGraph taskGraph ->
    // 查看执行图中是否包含 task release
    if (taskGraph.hasTask(release)) {
        if (!version.release) {
            logger.quiet "-- Hello --"
            version.release = true
            ant.propertyfile(file: versionFile) {
                entry(key: 'release', type: 'string', operation: '=', value: 'true')
            }
        }
    }
}
省略...
```

结果:
```
$ gradle release
Reading the version file.
-- Hello --
:compileJava UP-TO-DATE
:processResources UP-TO-DATE
:classes UP-TO-DATE
:war UP-TO-DATE
:createDistribution
:backupReleaseDistribution
:release
Releasing the project...
```

API:

https://docs.gradle.org/1.7/javadoc/org/gradle/api/execution/TaskExecutionGraph.html
