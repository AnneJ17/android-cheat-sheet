# Android Cheat Sheet

* **Kotlin vs Java**

  Kotlin is a statically typed open source programming language developed by JetBrains. It runs on JVM and so can be used anywhere we use Java. 
    - Concise - Eliminate boilerplate code
    - Null safety - Aim to eliminate null pointer exception which is common in Java
    - Interoperable - Kotlin is 100% compatible with existing Java code. Kotlin compiles to the exact same bytecode and making it interoperable in the JVM or on Android. 
    - Smart Cast - It explicit typecast the immutable values and insert the values in its safe cast automatically
    - Performance - In comparison with Java, the code compilation process is slower. For incremental compilation, Kotlin shows identical to Java or even slightly better results.
    - Extension function - Kotlin allows developers to extend a class with new functionality via extension functions. It is done by prefixing the name of the class that needs to be extended to the name of the function being created. We can even extend the library classess. 
    - Coroutines - Better way of managing intensive operations in Kotlin - coroutines. Coroutines are stackless, which means they demand lower memory usage as compared to threads. And therefore known as lightweight threads. 

* **What is Gradle?** [*](https://www.geeksforgeeks.org/android-build-gradle/)

  Gradle is a build tool to automate and manage the build process. It compiles the source code using appropriate tools, e.g., converts the kt files into dex files and compress all of them into a single file called apk. Following are the two types: 
  1. Top-level build.gradle - Located in the root project directory and it's function is to define build configurations that will be applied to all the modules in the project.
  2. Module-level build.gradle - Located in the project/module directory of the project and this is where all the needed dependencies for the application is mentioned and where the sdk versions (minimumSdk, maximumSdk, targettedSdk, versionCode, versionName) are declared.
  
* **What is Manifest file?** 

  It is an application level configuration file. All the metadata of the application like appName, launch icon, activities, services, and permissions are found here.
  
* **What is res folder?** 

  Keep all the xml files needded of your application which are used for designing or for storing data  
    - Drawables: Include static contents (images)
    - Layout: xml files that represents the UI
    - Mipmap: launch icons
    - Values: data files - colors.xml, strings.xml, styles.xml, dimens.xml
  
* **What is Activity?** 

  Activities are the main UI component of the application. Whatever is visible to user's eye is coming from the activity. Each activity contains 2 main part - one is the business logic (kotlin/java class) and the other is the view/ui (layout/xml files)
 
* **What is Activity life cycle?**

  Contains various methods that we can use to track the state of the activity. If we wish to execute some logic then we can override these methods and we can have our own implementation. There are 7 lifecycle methods - onCreate(), onStart(), onResume(), onPause(), onStop(), onRestart(), onDestroy()
  
* **Intent**

  Intent is the intention of what kind of operation we want to perform, ie., connecting with another application component from the current component. Through intent, one can start a new actvity, start a service, open a web page, camera. We can also pass data (primitive data in key value pair) between actcivities. There are two types of intent:- 
  1. Explicit intent - We know both the action and the target component
  2. Implicit inetnt - Know the action (opening 3rd party components) but don't know the target component
  
  * **Types of data that can pass in intent** 
  
  - Primitive types - we can pass primitive types via event but have to use serializable or parcelable to pass objects.
  - Serializables - have to make the object's class implement Serializable. Itconvert the object into byte streams - java has it's on implementation for serializable. Unfortunately, this approach is slow.
  - Parcelable - have to implement parcelablel interface. `writeToParce` responsible for serializing the data, and `Parcelable.Creator` is responsible for desirializing. Parcelable give better performance, however we need to manually implement it according to our needs.

* **Passing data between fragments**
  

* **Fragment**
  Fragment is a piece of an activity's UI which gives modularity to your activity. 
  - modularity - Entire ui to subsections, 
  - reusability - smae fragment can be used in , 
  - adaptability -  
manually adding fragment - <fragment tag in xml
fragment life cycle and activity
onAttach() - access the context of the activity
onDetach() - 

* **Linear layout vs relative layout vs constraint layout** 
  - LinearLayout - used when we want to arrange the views/widgets in a linear fashion either horizontal or vertical
  - constraint layout - optimize and flatten the view hierarchy - avoid nesting of the views to get better performance. 
  - Relative layout - nested and have to position 

List view vs recycler view 
list view - by default 1000 objects for every row create in, display only vertically
recycler view - viewholder mandatory, provide layout manger, item animate
* **Adapetrs**
  Adapters acts like a bridge that connects two incompatible classes. 
  Some type of adapters are Base dapter (parent), Array adapter

4 ways to store data locally - Shared preference, Sqlite, Cotent Providers, External/Internal storage.
* **Shared Preference and Sqlite**


* **LiveData, MutableLiveData and MediatorLiveData**

  The below relation is taken from [Android Developer Site](https://developer.android.com/reference/android/arch/lifecycle/MediatorLiveData)
  ```java.lang.Object
   ↳	android.arch.lifecycle.LiveData<T>
 	   ↳	android.arch.lifecycle.MutableLiveData<T>
 	 	   ↳	android.arch.lifecycle.MediatorLiveData<T> 
  ```
  * **LiveData** is an observable data holder class. Unlike regular observable, it is lifecycle-aware meaning it respects the lifecycle of activities, fragments, or services. It notifies Observer objects when the lifecycle state changes. LiveData has no public method to modify it's data. LiveData is an abstract class with protected setValue and postVlaue methods. We will have to use MutableLiveData to modify data. <br>LiveData is used to make the task of the ViewModel easier. The best part about LiveData is that the data will not be updated when the view is in background thus avoiding lot of crashes at runtime. 
  * **MutableLiveData** is a subclass of LiveData which is used for some of it's properties (setValue/postValue) and using these properties we can easily notify the UI when onChanged() is called. With MutableLiveData, setValue() and postValue() are publically available. setValue() - update the LiveData object from the main thread, postValue() - executed in the worker thread and main thread not preferred as it's a bit slower than setValue. 
  * **MediatorLiveData** is a subclass of MutableLiveData and therefore can access every property of MutableLiveData as well as LiveData. And when to use MediatorLiveData over MutableLiveData is demonstarted in this example - For instance, If we want to merge emissions of liveData1 and liveData2 instances into one object: liveDataMerger(a MediatorLivData object), then, liveData1 and liveData2 will become sources for the liveDataMerger and every time onChanged callback is called for either of them, we set a new value in liveDataMerger (by using addSource()). Another example, you want to display two SeekBars on two different fragments(Fragment1 and Fragment2) and you also want them to be synced when operating from Fragment1.
  
* **Advantages of using LiveData**
  - <ins>No memory leaks</ins>: Observers are bound to Lifecycle objects and clean up after themselves when their associated lifecycle is destroyed.
  - <ins>Views always get the upto date data</ins>: If a lifecycle becomes inactive, it receives the latest data upon becoming active again.
  - <ins>No crash due to stopped activity</ins>: If the observer's lifecycle is inactive, it doesn’t receive any LiveData events.
  
* **LiveData vs RxJava**

  - LiveData is somewhat similar to RxJava except that LiveData is lifecycle aware. LiveData excel on ViewModel layer, with its tight integration with Android lifecycles and ViewModel. Another advantage is that they automatically subscribe and unsubscribe on Activity life cycle events. LiveData does not give freedom to choose thread - only have current and UI thread and only one direction of switch "from current to UI". If a background service send a notification to a live data object it must use `postValue` method to ensure the value change is observed in the main thread.
  - RxJava provide more capabilities in transformations. We use RxJava in data source and repository layers, and it's transformed into LiveData (using LiveDataReactiveStreams) in ViewModels (before exposing data to activities/fragments). RxJava’s approach to choose a thread during the subscription, not in time of the sending is much more appropriate. Unlike LiveData, you have to explicitly subscribe and unsubscribe - in `onResume` we can do `compositeDisposable.add(...)` and `onPause` we do `CompositeDisposable.dispose`. 
  
* **How does LiveData in the ViewModel update the Activity?**

  When we register the Observer in our Activity, we need to override the method onChanged(). The method onChanged() would get trigger whenever the LiveData is changed. Thus in the onChanged(), we can update the changed LiveData onto the View.
  
* **Dependency Injection**

  Dependency Injection is a software design pattern in which one or more dependencies (or services) are injected, or passed by reference, into a dependent object (or client) and are made part of the client's state. The pattern separates the creation of a client's dependencies from it's own behavior, which allows program designs to be loosely coupled and to follow dependency inversion and single responsibility principles. It's built upon the concept of Inversion of control (a class should get its dependency from outside).
  
* **Benefits of Dependency Injection**
  - **Decoupling**: DI segregate the place where the object is created and consumed. This gives all the benefits listed below.
  - **Reusability**: Easily reuse without having to write the object creation code everytime it is required.
  - **Maintainability**: Perform any type of changes at one place and have it get reflected everywhere.
  - **Testability**: So now we have two entities - object creation and where its consumed. This gives us the ability to test the object creation and at the same time testing the object consumption code.
  - **Increased development speed** - As the application size grows, the benefits of DI start showing as the time spent for creating objects repeatedly goes away.
  
* **Run-time vs Compile-time DI**
  - **Run-time**: Usually based on reflection which is simpler to use but slower at run-time or at compile-time (eg. Spring).
  - **Compile-time**: It is based on code generation, ie., all the heavy-weight operations are performed during compilation. Add complexity but performs faster (eg. Dagger)
  
* **Singleton vs DI**

  When using Singletons in codebase, you loose the insight of what methods are doing? For instance, when class ```Foo``` depends on ```Logger.getInstance()```, ```Foo``` is effectively hiding it's dependencies from consumers. This means you can't fully understand ```Foo``` or use it with confidence unless you read it's source and uncover the fact that it depends on Logger. 
    The singleton pattern places a disproportionate emphasis on the ease of accessing objects. It completely eschews context by requiring that every consumer use an AppDomain-scoped object, leaving no options for varying implementations.
    Therefore Singleton pattern not only make the code harder to read, but also make it harder to unit test. And sometimes it promote hidden coupling, for example what if we need two objects at the complete opposite ends of your object graph to collaborate.
    
* **What is Dagger 2**

  Dagger is a fully static, compile-time dependency injection framework for both Java and Android. It is an adaptation of an earlier version created by Square and now maintained by Google. It frees you from writing tedious and error prone boilerplate code.

* **3 Questions to ask when you use Dagger**
  1. Which object do you want to inject?
use @Provides to create a method that returns this object
  2. Where do you want to inject this object?
Use @Inject in the place where you want to use this and create an interface @Component that contains all the places where you'll use this object.
  3. How will you construct this object?
Create a class @Module that contains your method with @Provides in it.

* **3 major things in Dagger**
  1. **Dependency Provider** - Dependencies are the onbjects that we need to instantiate inside a class. The class which provides the objects called dependencies are called Dependency Provider. In Dagger 2, we use @Module annotation. Inside this class, we create methods that will provide th eobjects (dependencies) and we use the @Provides annotation for this. 
@Module and @Provides - define classes and methods which provide dependencies.
  2. **Dependency Consume**r - It is a class where we need to instantiate the objects and for this we use @Inject annotation at the object declaration.
  3. **Component** - Connection between the dependency provider and dependency consumer. For this, we create an interface by annotating it with @Component.

* **Base elements used in Dagger**

  - @Inject - Base annotation whereby the dependency is requested
  - @Module - Classes with methods that provide dependencies
  - @Provide - Methods inside @Module which tell Dagger how we want to build and present a dependency
  - @Component  - Bridge between @Module and @Inject
  - @Scope - Enables to create global and local singletons
  - @Qualifier - If different object of the same type are necessary
  
* **Coroutines**

  Coroutines = Co + Routines. Here, **Co** means cooperation and **Routines** means functions. It means that when functions cooperate with each other, we call it as Coroutines. It's an optimized framework written over the actual threading by taking advantage of the cooperative nature of functions to make it light and yet powerful. So, we can say Coroutines are lightweight threads. A lightweight thread means it doesn't map on native thread (stackless), so it doesn't require context switching on the processor, so they are faster. Coroutines do not replace threads, it’s more like a framework to manage it.
  Coroutines are able to perform long-running and intensive tasks by suspending execution without blocking the thread and then resuming the execution at some later time. It allows the creation of non-blocking asynchronous code that appears to be synchronous.
  
* **Why use Coroutines?**

  - Lightweight: You can run many coroutines on a single thread due to support for suspension, which doesn't block the thread where the coroutine is running. Suspending saves memory over blocking while supporting many concurrent operations.
  - Fewer memory leaks: Use structured concurrency to run operations within a scope.
  - Built-in cancellation support: Cancellation is propagated automatically through the running coroutine hierarchy.
  - Jetpack integration: Many Jetpack libraries include extensions that provide full coroutines support. Some libraries also provide their own coroutine scope that you can use for structured concurrency.
  - RxJava works on android and Java. Coroutines are based on Kotlin. 
  - RXjava based on Observer pattern and well designed library and more complex. Greater learning curve. 
  
* **Difference b/w launch/join and async/await in Kotlin Coroutines**
  - `launch` is used to fire and forget coroutine. It's like starting a new thread. If the code inside the `launch` terminates with exception, then it is treated like uncaught exception in a thread.`join` is used to wait for completion of the launched coroutine and it does not propagate its exception. However, a crashed child coroutine cancels its parent with the corresponding exception, too.

* **Services**

  Service is a app component that runs on the background. 

* **Handler, Thread, Looper, and MessageQueue**

  The main thread is nothing but just a handler thread. Main thread is responsible for handling events from all over the app like callbacks associated with the lifecycle

JVM will first compile the code 
to .class file using the compiler
dex code to machine code by dvm
dvm convert the compile code along with converting to machine code at the same time - ART
ART is responsible for allocating memory to the application as well as deallocate the memory. Whenever the activity get destroyed, ART tell the garbage collector to deallocate the memory. 
There is a problem. There are few instance where garbage collector will try to collect and want get to collect an object thinking that the object is live and shouldn't collect. Garbage collector is not able to collect objects and the memory get collected and we finally get out of memory. This is called memory leaks. 

* **What is the problem with AsyncTask?**
  It's goal was to make background Threads which could interact with Main(UI) thread. The most common use of async task is to have it runs a time-consuming operation that updates a portion of the UI when it's completed (in AsyncTask.onPostExecute()).
  Async task is the major cause of memory leaks. Instead of using this, developers prefer using Coroutines and RxJava with schedulers. 
  Below are some of the problems:
  - **Rotation** - When the app is rotated, the activity is destroyed and recreated. When the activity is restarted your AsyncTask's reference to the activity is invalid, so onPostExecute() will have no effect on the new activity. This can be confusing if you are implicitly referencing the current Activity by having AsyncTask as an inner class of the Activity (memory leaks).The usual solution to this problem is to hold onto a reference to AsyncTask that lasts between configuration changes, which updates the target Activity as it restarts. There are a variety of ways to do this, though they either boil down to using a global holder (such as in the Application object) or passing it through Activity.onRetainNonConfigurationInstance(). For a Fragment-based system, you could use a retained Fragment (via Fragment.setRetainedInstance(true)) to store running AsyncTasks.
  - **Memory/context leaks** - Even when the actviity that spawned the AsyncTask is dead, still the AsyncTask will continue to run even after exiting the entire application. The only way that an AsyncTask finishes early is if it is canceled via AsyncTask.cancel(). Also, any object references held by the AsyncTask will not be eligible for garbage collection until the task explicitly nulls those references or completes and is itself eligible for GC (garbage collection).If not cancelled it will bog down your app with unnecessary background tasks, or of leaking memory. 
  - **Error Handling** - No out of the box solution for this. 
  - **Concurrent AsyncTasks** - If you queue up more than 138 tasks before they can complete, your app will crash. Usually happen when loading bitmaps from net.


* **RxJava and AsyncTask**

  In android, all the long running tasks are performed in the background and the result is updated in the main thread. Typically, such tasks are handled by AsyncTask. But with this we had to maintain the Threads and Handlers. However, RxJava takes care of threading, synchronizations and thread-safety.<br>
   RxJava is a java based implementation of reactive extensions - a library that follows Reactive programming principles to compose asynchronous and event-based programs by using observable sequence. Observables and Subscribers are the main building blocks. Observables for emmitting items and Subscribers for consuming the items. 
  
 * **How RxJava works**
  
    * Subscriber subscribes to Observable, then Observable calls Subscriber.onNext() for any number of items, if something goes wrong Subsciber.onError() is called and when the task is finished Subscriber.onCompleted() is called. 
    * Operators are methods created for solving transformations and handling API calls problems. Some of the common operators - Observable, Flowable, Single.
  <ini>Observable</ini> : Let us consider that We are making an API call and receiving the response. And that response is wrapped inside Observable type so that it can be processed by RxJava operators.
  <ini>Flowable<ini> : Each operator solves different problem, Flowable solving Backpressure ( Backpressure happens when there are multiple responses come at a speed that observers cannot keep up ). In Flowable, BackPressure is handled using BackPressure Strategies — MISSING, ERROR, BUFFER, DROP, LATEST. BackPressure is Handled by anyone of the mentioned strategies. Flowable is useful when we are making pagination call.
  <ini>Single</ini> : Single always either emits one value or an error. It returns Latest response for all requests. It is ideal for making search call.

  Strong reference - 
  Weak reference - inner class, bitmap, unregistered 
  Leak cannary library - Android profiler
  
**Performance Issues** - memory, UI, thread, battery
moved to dvm to art

* **Firebase Cloud messaging**

  Firebase Cloud Messaging (FCM) is a set of tools that sends push notifications and small messages upto 4kb in various platforms: Android, iOS, and web. Firebase is one of the simplest method to get notification. We as android developers prefer this method as this is very efficient and will not drain the battery of the device like polling (constantly requesting backend service for updates). 
  Following is the architecture of the FCM:-
    1. A service, API or console that sends messages to targeted devices.
    2. The Firebase Cloud Messaging back end, where all the processing happens.
    3. A transport layer that’s specific to each platform. In Android’s case, this is called the Android Transport Layer.
    4. The SDK on the device where you’ll receive the messages. In this case, called the Android Firebase Cloud Messaging SDK.
    
* **What is TDD?**

  Test Driven Development - Developing something that is driven by test. We describe each unit of the system through a failing test and make it pass with an implementation. The test will serve as a guidline of what to build
  
* **Why TDD?**
  - TDD can help reduce bugs (Reliable)
  - TDD can be our code guard: For instance, when somebody working on other feature unintentionally changes the behavior of the previously written features, will cause our test to fail as it is not working as intended. This is a red light to go on to production and this will let the other developers to fix the error. This will also give more confidence to change the software without any fear.
  - TDD can act as code documentation (Readable):  In TDD, test are written step by step. If someone is unsure of how the code works, one can look over the tests written and understand the functionality of each unit. It gives insight of how a particular unit should work with both pass and fail tests (assertions). 
    
 * **Mockito**
 
   Used in Junit testing. 
