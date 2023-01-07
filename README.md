- This Git serves https://github.com/oracle/graalvm-reachability-metadata/issues/163
  and https://github.com/apache/zookeeper/pull/1942

- Execute the following command in Linux to see the error. Make sure SdkMan and Git are installed.

```shell
sdk install java 22.3.r17-grl
sdk use java 22.3.r17-grl
gu install native-image
sudo apt-get install build-essential libz-dev zlib1g-dev -y

git clone git@github.com:linghengqian/zookeeper-jdk17-nativetest.git
cd ./zookeeper-jdk17-nativetest/
./gradlew -Pagent clean test
./gradlew metadataCopy --task test
./gradlew clean nativeTest
```
- The error log is as follows.
```shell
Error: Found 1 violations of @Uninterruptible usage:
method java.lang.Thread.getId() is annotated but org.apache.zookeeper.server.quorum.QuorumPeer.getId() is not
com.oracle.svm.core.util.UserError$UserException: Found 1 violations of @Uninterruptible usage:
method java.lang.Thread.getId() is annotated but org.apache.zookeeper.server.quorum.QuorumPeer.getId() is not
        at org.graalvm.nativeimage.builder/com.oracle.svm.core.util.UserError.abort(UserError.java:73)
        at org.graalvm.nativeimage.builder/com.oracle.svm.hosted.code.UninterruptibleAnnotationChecker.reportViolations(UninterruptibleAnnotationChecker.java:100)
        at org.graalvm.nativeimage.builder/com.oracle.svm.hosted.code.UninterruptibleAnnotationChecker.checkBeforeCompilation(UninterruptibleAnnotationChecker.java:91)
        at org.graalvm.nativeimage.builder/com.oracle.svm.hosted.code.CompileQueue.finish(CompileQueue.java:433)
        at org.graalvm.nativeimage.builder/com.oracle.svm.hosted.NativeImageGenerator.doRun(NativeImageGenerator.java:651)
        at org.graalvm.nativeimage.builder/com.oracle.svm.hosted.NativeImageGenerator.run(NativeImageGenerator.java:535)
        at org.graalvm.nativeimage.builder/com.oracle.svm.hosted.NativeImageGeneratorRunner.buildImage(NativeImageGeneratorRunner.java:403)
        at org.graalvm.nativeimage.builder/com.oracle.svm.hosted.NativeImageGeneratorRunner.build(NativeImageGeneratorRunner.java:580)
        at org.graalvm.nativeimage.builder/com.oracle.svm.hosted.NativeImageGeneratorRunner.main(NativeImageGeneratorRunner.java:128)
------------------------------------------------------------------------------------------------------------------------
                        3.8s (4.9% of total time) in 25 GCs | Peak RSS: 4.57GB | CPU load: 4.34
========================================================================================================================
Failed generating 'curator-client-tests' after 1m 17s.
    Error: Image build request failed with exit status 1

> Task :nativeTestCompile FAILED

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':nativeTestCompile'.
> Process 'command '/home/linghengqian/.sdkman/candidates/java/22.3.r17-grl/bin/native-image'' finished with non-zero exit value 1

* Try:
> Run with --stacktrace option to get the stack trace.
> Run with --info or --debug option to get more log output.
> Run with --scan to get full insights.

* Get more help at https://help.gradle.org

BUILD FAILED in 1m 38s
7 actionable tasks: 7 executed
```