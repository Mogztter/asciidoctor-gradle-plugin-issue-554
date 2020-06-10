# asciidoctor-gradle-plugin-issue-554

## Reproduction steps

Run the task for the first time (or use `./gradlew clean` to start on a clean workspace):

```console
$ ./gradlew convert
> Task :asciidoctorGemsPrepare
Successfully installed rouge-3.18.0
1 gem installed

BUILD SUCCESSFUL in 19s
2 actionable tasks: 2 executed
```

Then, build again:

```console
$ ./gradlew convert

BUILD SUCCESSFUL in 479ms
2 actionable tasks: 2 up-to-date
```

Nothing changed, everything is up-to-date, so far so good!

Then edit the `build.gradle` and update the version of the Rouge dependency to 3.19.0.
Once it's done you should get the following output:

```console
$ grep rouge build.gradle
  asciidoctorGems 'rubygems:rouge:3.19.0'
```

Then, build again:

```console
$ ./gradlew convert

BUILD SUCCESSFUL in 504ms
2 actionable tasks: 2 up-to-date
```

This is wrong! Let's take a look:


```console
$ ./gradlew convert --info
```

The task `:asciidoctorGemsPrepare` is up-to-date even though the version has changed!

```
> Task :asciidoctorGemsPrepare UP-TO-DATE
Caching disabled for task ':asciidoctorGemsPrepare' because:
  Build cache is disabled
Skipping task ':asciidoctorGemsPrepare' as it is up-to-date.
:asciidoctorGemsPrepare (Thread[Execution worker for ':',5,main]) completed. Took 0.008 secs.
:convert (Thread[Execution worker for ':',5,main]) started.
```

See https://github.com/asciidoctor/asciidoctor-gradle-plugin/issues/554
