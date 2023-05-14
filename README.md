# errorprone-disabled-and-patch-issue
Public repro of an Error Prone bug introduced in 2.19.0

When a check is both disabled and passed to the -XepPatchChecks argument, a stacktrace like the following occurs:

```
/home/alandau/code/gradle-disabled-and-patch-issue/lib/src/main/java/com/github/alexlandau/repro/Library.java:5: error: An unhandled exception was thrown by the Error Prone static analysis plugin.
    public String toString() {
                  ^
     Please report this at https://github.com/google/error-prone/issues/new and include the following:

     error-prone version: 2.19.1
     BugPattern: MissingOverride
     Stack Trace:
     java.util.NoSuchElementException: No value present
        at java.base/java.util.Optional.get(Optional.java:143)
        at com.google.errorprone.matchers.Description.severity(Description.java:78)
        at com.google.errorprone.ErrorProneAnalyzer.lambda$finished$1(ErrorProneAnalyzer.java:138)
        at com.google.errorprone.VisitorState.reportMatch(VisitorState.java:301)
        at com.google.errorprone.scanner.Scanner.reportMatch(Scanner.java:127)
        at com.google.errorprone.scanner.ErrorProneScanner.processMatchers(ErrorProneScanner.java:448)
        at com.google.errorprone.scanner.ErrorProneScanner.visitMethod(ErrorProneScanner.java:739)
        at com.google.errorprone.scanner.ErrorProneScanner.visitMethod(ErrorProneScanner.java:150)
        at jdk.compiler/com.sun.tools.javac.tree.JCTree$JCMethodDecl.accept(JCTree.java:953)
        at jdk.compiler/com.sun.source.util.TreePathScanner.scan(TreePathScanner.java:86)
        at com.google.errorprone.scanner.Scanner.scan(Scanner.java:74)
        at com.google.errorprone.scanner.Scanner.scan(Scanner.java:48)
        at jdk.compiler/com.sun.source.util.TreeScanner.scanAndReduce(TreeScanner.java:96)
        at jdk.compiler/com.sun.source.util.TreeScanner.scan(TreeScanner.java:111)
        at jdk.compiler/com.sun.source.util.TreeScanner.scanAndReduce(TreeScanner.java:119)
        at jdk.compiler/com.sun.source.util.TreeScanner.visitClass(TreeScanner.java:203)
        at com.google.errorprone.scanner.ErrorProneScanner.visitClass(ErrorProneScanner.java:548)
        at com.google.errorprone.scanner.ErrorProneScanner.visitClass(ErrorProneScanner.java:150)
        at jdk.compiler/com.sun.tools.javac.tree.JCTree$JCClassDecl.accept(JCTree.java:860)
        at jdk.compiler/com.sun.source.util.TreePathScanner.scan(TreePathScanner.java:86)
        at com.google.errorprone.scanner.Scanner.scan(Scanner.java:74)
        at com.google.errorprone.scanner.Scanner.scan(Scanner.java:48)
        at jdk.compiler/com.sun.source.util.TreeScanner.scan(TreeScanner.java:111)
        at jdk.compiler/com.sun.source.util.TreeScanner.scanAndReduce(TreeScanner.java:119)
        at jdk.compiler/com.sun.source.util.TreeScanner.visitCompilationUnit(TreeScanner.java:152)
        at com.google.errorprone.scanner.ErrorProneScanner.visitCompilationUnit(ErrorProneScanner.java:560)
        at com.google.errorprone.scanner.ErrorProneScanner.visitCompilationUnit(ErrorProneScanner.java:150)
        at jdk.compiler/com.sun.tools.javac.tree.JCTree$JCCompilationUnit.accept(JCTree.java:614)
        at jdk.compiler/com.sun.source.util.TreePathScanner.scan(TreePathScanner.java:60)
        at com.google.errorprone.scanner.Scanner.scan(Scanner.java:58)
        at com.google.errorprone.scanner.ErrorProneScannerTransformer.apply(ErrorProneScannerTransformer.java:43)
        at com.google.errorprone.ErrorProneAnalyzer.finished(ErrorProneAnalyzer.java:156)
        at jdk.compiler/com.sun.tools.javac.api.MultiTaskListener.finished(MultiTaskListener.java:132)
        at jdk.compiler/com.sun.tools.javac.main.JavaCompiler.flow(JavaCompiler.java:1394)
        at jdk.compiler/com.sun.tools.javac.main.JavaCompiler.flow(JavaCompiler.java:1341)
        at jdk.compiler/com.sun.tools.javac.main.JavaCompiler.compile(JavaCompiler.java:933)
        at jdk.compiler/com.sun.tools.javac.api.JavacTaskImpl.lambda$doCall$0(JavacTaskImpl.java:104)
        at jdk.compiler/com.sun.tools.javac.api.JavacTaskImpl.invocationHelper(JavacTaskImpl.java:152)
        at jdk.compiler/com.sun.tools.javac.api.JavacTaskImpl.doCall(JavacTaskImpl.java:100)
        at jdk.compiler/com.sun.tools.javac.api.JavacTaskImpl.call(JavacTaskImpl.java:94)
        at org.gradle.internal.compiler.java.IncrementalCompileTask.call(IncrementalCompileTask.java:92)
        at org.gradle.api.internal.tasks.compile.AnnotationProcessingCompileTask.call(AnnotationProcessingCompileTask.java:94)
        at org.gradle.api.internal.tasks.compile.ResourceCleaningCompilationTask.call(ResourceCleaningCompilationTask.java:57)
        at org.gradle.api.internal.tasks.compile.JdkJavaCompiler.execute(JdkJavaCompiler.java:55)
        at org.gradle.api.internal.tasks.compile.JdkJavaCompiler.execute(JdkJavaCompiler.java:39)
        at org.gradle.api.internal.tasks.compile.daemon.AbstractIsolatedCompilerWorkerExecutor$CompilerWorkAction.execute(AbstractIsolatedCompilerWorkerExecutor.java:78)
        at org.gradle.workers.internal.DefaultWorkerServer.execute(DefaultWorkerServer.java:63)
        at org.gradle.workers.internal.AbstractClassLoaderWorker$1.create(AbstractClassLoaderWorker.java:54)
        at org.gradle.workers.internal.AbstractClassLoaderWorker$1.create(AbstractClassLoaderWorker.java:48)
        at org.gradle.internal.classloader.ClassLoaderUtils.executeInClassloader(ClassLoaderUtils.java:100)
        at org.gradle.workers.internal.AbstractClassLoaderWorker.executeInClassLoader(AbstractClassLoaderWorker.java:48)
        at org.gradle.workers.internal.FlatClassLoaderWorker.run(FlatClassLoaderWorker.java:32)
        at org.gradle.workers.internal.FlatClassLoaderWorker.run(FlatClassLoaderWorker.java:22)
        at org.gradle.workers.internal.WorkerDaemonServer.run(WorkerDaemonServer.java:96)
        at org.gradle.workers.internal.WorkerDaemonServer.run(WorkerDaemonServer.java:65)
        at org.gradle.process.internal.worker.request.WorkerAction$1.call(WorkerAction.java:138)
        at org.gradle.process.internal.worker.child.WorkerLogEventListener.withWorkerLoggingProtocol(WorkerLogEventListener.java:41)
        at org.gradle.process.internal.worker.request.WorkerAction.run(WorkerAction.java:135)
        at java.base/jdk.internal.reflect.DirectMethodHandleAccessor.invoke(DirectMethodHandleAccessor.java:104)
        at java.base/java.lang.reflect.Method.invoke(Method.java:577)
        at org.gradle.internal.dispatch.ReflectionDispatch.dispatch(ReflectionDispatch.java:36)
        at org.gradle.internal.dispatch.ReflectionDispatch.dispatch(ReflectionDispatch.java:24)
        at org.gradle.internal.remote.internal.hub.MessageHubBackedObjectConnection$DispatchWrapper.dispatch(MessageHubBackedObjectConnection.java:182)
        at org.gradle.internal.remote.internal.hub.MessageHubBackedObjectConnection$DispatchWrapper.dispatch(MessageHubBackedObjectConnection.java:164)
        at org.gradle.internal.remote.internal.hub.MessageHub$Handler.run(MessageHub.java:414)
        at org.gradle.internal.concurrent.ExecutorPolicy$CatchAndRecordFailures.onExecute(ExecutorPolicy.java:64)
        at org.gradle.internal.concurrent.ManagedExecutorImpl$1.run(ManagedExecutorImpl.java:49)
        at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1136)
        at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:635)
        at java.base/java.lang.Thread.run(Thread.java:833)
```

This is not a common situation, but it can come up in some contexts related to automated tooling (e.g. an automated tool runs compilation with -XepPatchChecks against a repo in which Gradle build configuration disables the check).

To reproduce, run `./gradlew compileJava` in this repo. The Error Prone version can be adjusted in the lib/build.gradle file.

In 2.18.0, there is no stacktrace and the patch is created. In 2.19.0 and 2.19.1 (the latest at time of writing), the stacktrace occurs and the compilation fails.
