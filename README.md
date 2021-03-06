# rx-scheduler-transformer
Rxjava2 scheduler transformer tools for [RxLife](https://github.com/trello/RxLifecycle) in android.

[ ![Download](https://api.bintray.com/packages/thepacific/maven/transformer/images/download.svg) ](https://bintray.com/thepacific/maven/transformer/_latestVersion)

One nice aspect of RxJava is that you can see how data is transformed through a series of operators:
```java
Observable
        .fromCallable(someSource)
        .map(data -> manipulate(data))
        .subscribeOn(Schedulers.io())
        .compose(bindUntilEvent(ActivityEvent.DESTROY))
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(data -> doSomething(data));
```
We frequently use subscribeOn(), compose() and observeOn() because we want to process data in a worker thread then subscribe to it on the main thread and look up the Activity or Fragment lifecycle. Now, this tiny library get you rid of it!

# Setup
```groovy
compile 'com.github.thepacific:transformer:0.0.1'
```

# Support
Flowable, Observable, Maybe, Single, and Completable are all supported. Implementation is solely based on their Observer types, so conceivably any type that uses those for subscription should work.
```java
CompletableTransformerUtil for Completable
FlowableTransformerUtil for Flowable
MaybeTransformerUtil for Maybe
ObservableTransformerUtil for Observable
SingleTransformerUtil for Single

// Transform schedulers
// from   Schedulers.io()             to   AndroidSchedulers.mainThread()
// from   Schedulers.computation()    to   AndroidSchedulers.mainThread()
// from   Schedulers.trampoline()     to   AndroidSchedulers.mainThread()
// from   Schedulers.newThread()      to   AndroidSchedulers.mainThread()
// from   Schedulers.single()         to   AndroidSchedulers.mainThread()
// from   Schedulers.from(executor)   to   AndroidSchedulers.mainThread()

```

# Usage
```java
// ObservableTransformerUtil.io(lifecycle(), ActivityEvent.DESTROY))
// ObservableTransformerUtil.computation(lifecycle(), ActivityEvent.DESTROY))
// ObservableTransformerUtil.trampoline(lifecycle(), ActivityEvent.DESTROY))
// ObservableTransformerUtil.newThrea(lifecycle(), ActivityEvent.DESTROY))
// ObservableTransformerUtil.single(lifecycle(), ActivityEvent.DESTROY))
// ObservableTransformerUtil.from(executor, lifecycle(), ActivityEvent.DESTROY))

Observable
        .fromCallable(someSource)
        .map(data -> manipulate(data))
        .compose(ObservableTransformerUtil.io(lifecycle(), ActivityEvent.DESTROY))
        .subscribe(data -> doSomething(data));
```

# License  
[The MIT License ](https://opensource.org/licenses/MIT)
