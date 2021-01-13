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
  
* **ART vs Dalvik**

  Dalvik behaves even worse in terms of memory management and Garbage Collection. 
  
* **What is Manifest file?** 

  It is an application level configuration file. All the metadata of the application like appName, launch icon, activities, services, and permissions are found here.
  
* **What is res folder?** 

  Keep all the xml files needded of your application which are used for designing or for storing data  
    - Drawables: Include static contents (images)
    - Layout: xml files that represents the UI
    - Mipmap: launch icons
    - Values: data files - colors.xml, strings.xml, styles.xml, dimens.xml
    
* **What are launch modes?**
  
  Launch mode is an instruction denoting how an activity should be launched. There are 4 modes:-
    1. standard: This is the default mode where the system creates a new instance of the activity in the target task or routes intent to it.
    2. singleTop: Activity will be created once and will be on top. If an instance of the activity already exists at the top of the target task, the system routes the intent to that instance through a call to its onNewIntent() method. 
    3. singleTask: Here, no multiple instances are created. The system creates the activity at the root of a new task and routes the intent to it. 
 
* **What is Activity?** 

  Activities are the main UI component of the application. Whatever is visible to user's eye is coming from the activity. Each activity contains 2 main part - one is the business logic (kotlin/java class) and the other is the view/ui (layout/xml files)
 
* **What is Activity life cycle?**

  Contains various methods that we can use to track the state of the activity. If we wish to execute some logic then we can override these methods and we can have our own implementation. There are 7 lifecycle methods - onCreate(), onStart(), onResume(), onPause(), onStop(), onRestart(), onDestroy()
  
* **What is a service?**
  A service is one of the Android component that performs long running task in the background, <ini>even when the user is not interacting with the application</ini>. By default, service runs on the main thread and should create a new thread if performing intensive or blocking operations to avoid ANR errors. The service lifecycle—from when it's created to when it's destroyed—can follow either of these two paths: started service, bound service. 
  Note: if you want to run a task outside of the main thread, but only while the application is running, use a thread. 
  
* **Types of Services**
  - Foreground Services:  A foreground service performs operation that is noticeable to the user. It must provide a notification, which is placed under the "Ongoing" heading, which means that the notification cannot be dismissed unless the service is either stopped or removed from the foreground. (e.g. audio app using foreground service to play audio track).
  - Background Services: A background service performs an operation that isn't directly noticed by the user. (e.g. app using background service to compact its storage)
  - Bound Services:  better choice for more complex two-way interactions between activities and services. It allows the launching component to interact with, and receive results from the service. Started service does not generally return results or permit interaction with the component that launched it (requires complex programming).
  
* **Ways to create bound services**

  There are 3 ways you can define the interface
    1. Extending the binder class - If your service is private to your own application and runs in the same process as the client 
    2. Using a Messenger - If you want to work across different processes, use Messenger. It creates a queue of all the client requests in a single thread, so the service receives requests one at a time.
    3. Using AIDL - If you want your service to handle multiple requests simultaneously, then use AIDL. Make sure your service is thread-safe and capable of multi-threading.

* **IntentServices**

  The IntentService is used to perform a certain task in the background. Once done, the instance of IntentService terminates itself automatically. An example for its usage would be downloading certain resources from the internet. It offers onHandleIntent() method which will be asynchronously called by the Android system.
  
* **Services**

  Service is a app component that runs on the background. 
  Bound service - Allows other components to bind it to the bound service to get some functionalities. One service can have multiple clients. Whenever the last component unbounds, that service is stopped. Local bound - client and service in same application, remote bound - AIDL - to connect to the client. When implementing AIDL we want to keep some steps.
  1. Create an interface  - Implement the remote service
  2. class extending service - 
  
* **Internal and External storage**
  
  
* **What is content provider?**

  Content providers are used when there is a need to share data between multiple applications as Android-databases (Sqlite) created in Android are visible only to the application that created them. It let you centralize content in one place and have different applications access it. When you want to access data in a content provider, you use the ContentResolver object in your application's Context to communicate with the provider as a client. The ContentResolver methods provide the basic "CRUD" (create, retrieve, update, and delete) functions of persistent storage. In most cases this data is stored in an SQlite database. The CursorLoader objects rely on content providers to run asynchronous queries and then return the results to the UI layer in your application.
  Actvity/Fragment <---> CursorLoader <---> ContentResolver <---> ContentProvider <---> DataStorage
  
* **Advantages of content providers**
  - Content providers offer granular control over the permissions for accessing data.
  - extra level of abstraction over your data to make it easier to change internally (changing underlying database structure).
  
* **What is broadcast receiver?**[*](https://developer.android.com/guide/components/broadcasts)
  
  BroadcastReceiver is a dormant component of android that listens to system-wide broadcast events or intents. Some examples of system wide generated intents are:- 
    - android.intent.action.BATTERY_LOW
    - android.intent.action.BOOT_COMPLETED 
    - android.intent.action.CALL
    - android.intent.action.DATE_CHANGED
    - android.intent.action.REBOOT
    - android.net.conn.CONNECTIVITY_CHANGE
    
* **Setting up Broadcast Receiver**
  1. Creating a broadcast receiver - extend the BroadcastReceiver abstract class and override the onReceive()
  2. Registering a broadcast receiver - There are 2 ways to do it.
      1. By defining it in the AndroidManifest.xml  
      2. By defining it programmatically - Create an `IntentFilter` and register the receiver by calling `registerReceiver(BroadcastReceiver, IntentFilter)`
      
* **What is an Intent?**

  `Intent` is a messaging object that carries information which the Android system uses to determine which component to start plus the information that the recipient component uses in order to function properly. Through intent, one can start a new actvity, start a service, deliver a broadcast, open a web page, camera.
  1. Explicit intent - Know both the action and the target component
  2. Implicit inetnt - Know the action (opening 3rd party components) but don't know the target component
  
* **Types of data that can pass in intent** 
  - Primitive types - we can pass primitive types via event but have to use serializable or parcelable to pass objects.
  - Serializables - We have to make the object's class implement Serializable. It converts the object into byte streams and java has it's on implementation for serializable. Unfortunately, this approach is slow as we use reflection. This method creates a lot of temporary objects and causes quite a bit of garbage collection.
  - Parcelable - Parcelable give better performance as we are being explicit about the serialization process instead of using reflection to infer it. We have to implement parcelablel interface. `writeToParce` responsible for serializing the data, and `Parcelable.Creator` is responsible for desirializing.

* **What is Intent Filter?**
  
  Intent filters specifies the type of the intent that the component would like to receive. When you create an implicit intent, the Android system finds the appropriate component to start by comparing the contents of the intent to the intent filters declared in the manifest file of other apps on the device.
  
* **What is ViewGroup?**

  A `ViewGroup` is a special view that can contain other views (called children). The view group is the base class for layouts and views containers.<br>
  The `View` class is a superclass for all GUI components in Android.
  
* **Custom ViewGroup**
  Sometimes you want to group some views into one component to allow them to deal with each other easily through writing some specific code or business logic. You can call that a “compound view”. Compound views give you reusability and modularity.
  
* **Why use Custom Views?**
  - Innovative UI design or animation
  - Different user interaction
  - Performance optimization
  - Reusability
  
* **Custom View methods**

  Android draws the layout hierarchy in three stages:
  - Measuring stage: `onMeasure()` - Each view must measure itself
  - Layout stage: each ViewGroup finds the right position for its children on the screen by using the child size and also by following the layout rules.
  - Drawing stage: `onDraw()` - after measuring and positioning all of the views, each view happily draws itself.l This method provides a canvas for that. `invalidate()`: makes Android redraw the view by calling onDraw()
 
* **Fragment**
  Fragment segments your app into multiple, independent screens that are hosted within an Activity.
  - modularity - divide UI to subsections 
  - reusability - same fragment can be used in other activities
  - adaptability -  easily adjust your UI to the screen it is running on.
  
* **Fragment lifecycle**
  - onAttach() this is the method which make sure the fragment is attached with the activity and Where fragment can access the context of the activity
  - onCreate() - initial creation of the fragment
  - onCreateView() - setup the layout of the fragment
  - onActivityCreated() - creates and returns the view hierarchy associated with the fragment.
  - onStart() - makes the fragment visible to the user
  - onResume() - makes the fragment begin interacting with the user
  - onPause() - fragment is no longer interacting with the user either because its activity is being paused or a fragment operation is modifying it in the activity
  - onStop() - fragment is no longer visible to the user either because its activity is being stopped or a fragment operation is modifying it in the activity
  - onDestroyView() - allows the fragment to clean up resources associated with its View
  - onDestroy() - called to do final cleanup of the fragment's state
  - onDetach() - when fragment loose the context of the activity
  
* **Passing data between fragments**
  
* **Ways to add fragment**
  1. Manually - <fragment id="" name="fully qualified name of the class"/>; name and id attributes are mandatory if using <fragment> tag
  2. Dynamically add fragment - In the activity and use fragment manager


* **Linear layout vs relative layout vs constraint layout** 
  - LinearLayout - used when we want to arrange the views/widgets in a linear fashion either horizontal or vertical
  - constraint layout - optimize and flatten the view hierarchy - avoid nesting of the views to get better performance. 
  - Relative layout - nested and have to position it, 
  - Framelayout - container as a placeholder as it's an empty layout
  
* **When to use coordinate layout?**
  
  Coordinate layout position top-level application widgets, such as AppBarLayout and FloatingActionButton. 

* **List view vs recycler view**

  * <ini>ListView</ini> - displays all the items vertically in a scrollable fashion and creates view for each items.
  * <ini>RecyclerView</ini> - It's an advanced form of list view.
    1. Reuses cells while scrolling up/down - this is possible with implementing View Holder in the ListView adapter, but it was an optional thing, while in the RecycleView it's the default way of writing adapter. Therefore, memory efficient because it keeps on recycling the view as the view is gotten from the view holder. ViewHolder holds the view and when the recycler view recycles it gets the view from the ViewHolder.
    2. Decouples list from its container - so you can put list items easily at run time in the different containers (linearLayout, gridLayout, staggeredLayout) with setting LayoutManager.
    3. Animates common list actions - Animations are decoupled and delegated to ItemAnimator.
    4. Item decoration - gives us huge control to develop and have custom divider, space, borders etc
    5. OnItemTocuhListener - handle the user interaction and to enhance user experience. Useful for gestural manipulation of item views (on slide delete)

* **Adapetrs**

  Adapters acts like a bridge that connects two incompatible classes to work together. Some types of adapters are 
    - BaseAdapter: abstract class which implements ListAdapter and SpinnerAdapter Interface. Hence, we may use it for implementing both ListView and Spinner.
    - CursorAdapter: more appropriate when there is a database because it does not load all the records as ArrayAdapter. It loads only the visible records, or the records you are querying,
    - Array Adapter - Simple adapter where we pass context, layout, data 
    - FragmentPageAdapter - ViewPager 
    - RecyclerView adapter - 

* **Shared Preference and Sqlite**
  
  There are 4 ways to store data locally - Shared preference, Sqlite, Cotent Providers, External/Internal storage.<br>
  &nbsp;&nbsp;&nbsp;We use shared preference for saving small amount of data like app setting (theme), login status, user preference (notification status). It saves the data in form of key value pair and hence we can save only primitive type data like string, boolean, integers etc but we cannot store an object. Whereas, Sqlite is a relational database where we can store huge amount of data.
  

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
  
* **Data Binding**
  
  Bind UI components in your layouts to data sources in your app using a declarative format. Data Binding is the process that establishes a connection between the UI and business logic. When the data changes it's vlaue, the elements that are bound to the data reflect changes automatically.  
  
* **MVP vs MVVM**
  MVP - Model View Presenter: Presenter is like a bridge between the view and the model. There is one to one relation between the view and the presenter and therefore is tightly coupled. And is more verbose as each view needs a presenter. 
  MVVM - Model View ViewModel: Each of the component is loosely coupled and follows event-driven pattern. VM holds the interaction between the view and the model. And all the business logic goes here. VM is lifecycle aware in which we can use live data and databinding and it is also recommended by Google.ViewModel doesn’t have to know anything about the View and has no reference to View classes
  
* **Rules for View-ViewModel communication**
  1. View should not have any logic in it. All logic for the view happens in ViewModel.
  2. In response to events view does nothing except notifying view-model by calling a method. ViewModel doesn’t have to know anything about the View and has no reference to View classes. View does not pass any view related classes to view model.
  3. ViewModel uses live data as the main way to communicate to view.
  4. View can call view-model whenever it needs something. ViewModel can provide helper methods for the view.
 
 * **Login uisng MVVM steps**

  Two-way Data Binding allows us to bind objects in the XML layouts such that the object can send data to the layout, and vice versa.
  
  1. Enable databinding to true in Gradle file and also need to add dependency for lifecycle components. 
  2. We will have a Model would hold the user’s email and password
  2. Create a class that extends ViewModel. And we create LiveData object in the ViewModel and after that, connect your View to this LiveData
  3. XML file: Databinding - the layout will be wrapped inside <layout> tag and inside the <data> tag will provide the viewmodel path for "type". It takes input from the UI using DataBinding “@=”, stores it in LiveData and displays back on the UI. ViewModel binds the data to the View.
  4. Activity - With DataBinding, the ActivityMainBinding class is auto-generated from the layout. We get the view model by doing something like ViewModelProviders.of(this).get(LoginViewModel.class); in the Observer we can override the onChanged() method
  
* **How to use Retrofit and coroutines to make network calls and how we use it in MVVM
**Model** All your data goes in this layer; **Viewmodel** contains the business logic; **View** - Activities, fragments, layouts
Model - Repository and data lives here. Inside the repository we make retrofit calls. Set the data in LiveData. In ViewModel - we get the data from the repository. 
View - call the viewmodel to get the data from the livedata which you display in your view

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
    
* **What is Dagger2?**

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

* **Handler, Thread, Looper, and MessageQueue**

  The main thread is nothing but just a handler thread. Main thread is responsible for handling events from all over the app like callbacks associated with the lifecycle

JVM will first compile the code 
to .class file using the compiler
dex code to machine code by dvm
dvm convert the compile code along with converting to machine code at the same time - ART
ART is responsible for allocating memory to the application as well as deallocate the memory. Whenever the activity get destroyed, ART tell the garbage collector to deallocate the memory. 
There is a problem. There are few instance where garbage collector will try to collect and want get to collect an object thinking that the object is live and shouldn't collect. Garbage collector is not able to collect objects and the memory get collected and we finally get out of memory. This is called memory leaks. 

* **What is work manager?**
  
  Defferable, asynchronous task that one can schedule when the work's constraints are satisfied. 
  
* **What is the problem with AsyncTask?**
  It's goal was to make background Threads which could interact with Main(UI) thread. The most common use of async task is to have it runs a time-consuming operation that updates a portion of the UI when it's completed (in AsyncTask.onPostExecute()).
  Async task is the major cause of memory leaks. Instead of using this, developers prefer using Coroutines and RxJava with schedulers. 
  Below are some of the problems:
  - **Rotation** - When the app is rotated, the activity is destroyed and recreated. When the activity is restarted your AsyncTask's reference to the activity is invalid, so onPostExecute() will have no effect on the new activity. This can be confusing if you are implicitly referencing the current Activity by having AsyncTask as an inner class of the Activity (memory leaks).The usual solution to this problem is to hold onto a reference to AsyncTask that lasts between configuration changes, which updates the target Activity as it restarts. There are a variety of ways to do this, though they either boil down to using a global holder (such as in the Application object) or passing it through Activity.onRetainNonConfigurationInstance(). For a Fragment-based system, you could use a retained Fragment (via Fragment.setRetainedInstance(true)) to store running AsyncTasks.
  - **Memory/context leaks** - Even when the actviity that spawned the AsyncTask is dead, still the AsyncTask will continue to run even after exiting the entire application. The only way that an AsyncTask finishes early is if it is canceled via AsyncTask.cancel(). Also, any object references held by the AsyncTask will not be eligible for garbage collection until the task explicitly nulls those references or completes and is itself eligible for GC (garbage collection).If not cancelled it will bog down your app with unnecessary background tasks, or of leaking memory. 
  - **Error Handling** - No out of the box solution for this. 
  - **Concurrent AsyncTasks** - If you queue up more than 138 tasks before they can complete, your app will crash. Usually happen when loading bitmaps from net.


* **RxJava and AsyncTask**

  In android, all the long running tasks are performed in the background and the result is updated in the main thread. Typically, such tasks are handled by `AsyncTask`. But with this we had to maintain the Threads and Handlers. However, RxJava takes care of threading, synchronizations and thread-safety.<br>
   `RxJava` is a java based implementation of reactive extensions - a library that follows Reactive programming principles to compose asynchronous and event-based programs by following Observer pattern. Observables and Subscribers are the main building blocks. Observables for emmitting items and Subscribers for consuming the items. 
   
* **What is Reactive Programming?**

  It is an event based asynchronous event-based programming. Everything you see is an asynchronous data stream, which can be observed and an action will be taken place when it emits values. You can create data stream out of anything; variable changes, click events, http calls, data storage, errors and what not. When it says asynchronous, that means every code module runs on its own thread thus executing multiple code blocks simultaneously. Therfeore, the amount of time taken to complete all the tasks is equivalent to the longer task in the list.
  
* **How RxJava works**
    * Subscriber subscribes to Observable, then Observable calls Subscriber.onNext() for any number of items, if something goes wrong Subsciber.onError() is called and when the task is finished Subscriber.onCompleted() is called. 
    * Operators are methods created for solving transformations and handling API calls problems. Some of the common operators - Observable, Flowable, Single.
  <ini>Observable</ini> : Let us consider that We are making an API call and receiving the response. And that response is wrapped inside Observable type so that it can be processed by RxJava operators.
  <ini>Flowable<ini> : Each operator solves different problem, Flowable solving Backpressure ( Backpressure happens when there are multiple responses come at a speed that observers cannot keep up ). In Flowable, BackPressure is handled using BackPressure Strategies — MISSING, ERROR, BUFFER, DROP, LATEST. BackPressure is Handled by anyone of the mentioned strategies. Flowable is useful when we are making pagination call.
  <ini>Single</ini> : Single always either emits one value or an error. It returns Latest response for all requests. It is ideal for making search call.
  
* **Key components of RxJava**
  - Observable: Observable is a data stream that do some work and emits data.
  - Observer: OnContrary to Observable, Observer receives the data emitted by the Observable.
  - Subscription: The bonding between Observable and Observer is called as Subscription. There can be multiple Observers subscribed to a single Observable.
  - Operator / Transformation: Operators modifies the data emitted by Observable before an observer receives them.
  - Schedulers: Schedulers decides the thread on which Observable should emit the data and on which Observer should receives the data i.e background thread, main thread etc.,

* **Schedulers**
  Schedulers basically decides on which thread a task runs, whether main thread or backgournd thread. They are introduced in RxAndroid (AndroidSchedulers.mainThread()) which plays major role in supporting multithreading concept in android applications. List of schedulers: 
  - Schedulers.io() - Used for CPU-intensive works like network calls, database operations, reading disks/files. Maintaings pool of threads
  - AndroidSchedulers.mainThread() - provides access to the main thread. Jobs like updating UI, user interaction.
  
  Disposables - Used to dispose the subscription when Observer no longer wants to listen to Observable. It avoids memory leaks. 
  
  Strong reference - 
  Weak reference - inner class, bitmap, unregistered 
  Leak cannary library - Android profiler
  
* **Converting RxJava to LiveData**
* **Advantages and Disadvantages of RxJava**
* **Types of observables**
* **Types of operators**

  - Filter: 
  - Buffer: Buffer gathers items emitted by an Observable into batches and emit the batch instead of emitting one item at a time.
  - Debounce: Debounce operators emits items only when a specified timespan is passed. It's usually used when the Observable is rapidly emitting items but you are only interested in receiving them in timely manner (e.g. Instant search).
  
* **RxJava map opeartors** [*](https://www.androidhive.info/RxJava/map-flatmap-switchmap-concatmap/)
  FlatMap, ConcatMap, SwitchMap - instead of returning the modified item, it returns the Observable itself which can emit data again
  - Map: Used to alter the emitted data. It modifies each item emitted by a source Observable and emits the modified item.
  - FlatMap:  Used when the order is not important and want to send all the network calls simultaneously
  - ConcatMap: Unlike FlatMap, maintains the order of items and waits for the current Observable to complete its job before emitting the next one. more suitable when you want to maintain the order of execution.
  - SwitchMap: Used when you want to discard the response and consider the latest one. It oneunsubscribe from previous source Observable whenever new item started emitting, thus always emitting the items from current Observable (e.g. Instant search).

* **Firebase**
  
  Firebase is a toolset to buil, improve and grow applications (ios, android, web) cross platform and gives you services that normally a developer would have to build themselves. This includes authentication, databases, analytics, push notifications. The services are hosted in cloud, and scale with little to no effort.
  
* **Firebase Cloud messaging**

  Firebase Cloud Messaging (FCM) is a set of tools that sends push notifications and small messages upto 4kb in various platforms: Android, iOS, and web. Firebase is one of the simplest method to get notification. We as android developers prefer this method as this is very efficient and will not drain the battery of the device like polling (constantly requesting backend service for updates). 
  Following is the architecture of the FCM:-
    1. A service, API or console that sends messages to targeted devices.
    2. The Firebase Cloud Messaging back end, where all the processing happens.
    3. A transport layer that’s specific to each platform. In Android’s case, this is called the Android Transport Layer.
    4. The SDK on the device where you’ll receive the messages. In this case, called the Android Firebase Cloud Messaging SDK.
  
**Performance Issues** - memory, UI, thread, battery
moved to dvm to art
    
* **What is TDD?**

  Test Driven Development - Developing something that is driven by test. We describe each unit of the system through a failing test and make it pass with an implementation. The test will serve as a guidline of what to build
  
* **Why TDD?**
  - TDD can help reduce bugs (Reliable)
  - TDD can be our code guard: For instance, when somebody working on other feature unintentionally changes the behavior of the previously written features, will cause our test to fail as it is not working as intended. This is a red light to go on to production and this will let the other developers to fix the error. This will also give more confidence to change the software without any fear.
  - TDD can act as code documentation (Readable):  In TDD, test are written step by step. If someone is unsure of how the code works, one can look over the tests written and understand the functionality of each unit. It gives insight of how a particular unit should work with both pass and fail tests (assertions). 
    
* **Mockito**
 
   Used in Junit testing. 
   
* **Mock vs. Stub vs. Spy**

  - Mock: A mock is a dummy class replacing a real one, returning something like null or 0 for each method call.
  - Stub: A stub is a dummy class providing some more specific, prepared or pre-recorded, replayed results to certain requests under test. 
  - Spy: A spy is kind of a hybrid between real object and stub, i.e. it is basically the real object with some (not all) methods shadowed by stub methods.
  
* **What is REST API**
  REST stands for REpresentational State Transfer. It is an architectural style that defines constraints for creating web services. It is stateless, i.e, no client session data is stored on the server (eg. Once you make an authentication, you have to repeat every time you make a call).
  Every data is an entity. 

* **Code Review**
   
   Every alternate days we do code review. We check our colleagues code. Peer to Peer review.
   * Proper namingCovnetion, JavaDocs, Comments
   * Followed Desisign pattern
   * Written enough test cases
   * Any security issues - 
   Any performance issues - are there any memory leaks, threads, battery, UI
   
* **Memory Management**
  
  In Android, bitmaps represent the largest contiguous blocks of memory. They occupy heaps, which results in lots of contention to find free space to allocate new bitmaps as we scroll.This then results in more GC events so it can free up memory to provide the necessary space.
  
* **Lint Tool**
  
  The lint tool checks your Android project source files for potential bugs and optimization improvements for correctness, security, performance, usability, accessibility, and internationalization. When using Android Studio, configured lint and IDE inspections run whenever you build your app
  
* **How does you take updated with latest technology**
  
  Google IO conference - 2019: Introduced dark theme, smart replies - , video captioning in video calls
  News letters, website - ProAndroid, 
  
* **Github vs Git**

  Git is a distributed version control system or source code management system. Whereas, gitHub is a hosting service for Git repositories. In short - Git is the tool, GitHub is the service for projects that use Git.
 
  
* **Rebase and merge**

  Rebase - Moving or combining a sequence of commits to a new base commit. For instance, if you started doing some development and then another developer made an unrelated change. You probably want to pull and then rebase to base your changes from the current version from the repository.
  Merge - Merging two different branches. For example, let's say you have created a branch for the purpose of developing a single feature. When you want to bring those changes back to master, you probably want merge 
  
  Differences:
    - Merge preserves the branches while Rebase won't. 
    
* **What is Accessibility Service?**
  
  An Accessibility Service assists users with disabilities in using Android devices and apps. It is a long-running privileged service that helps users process information on the screen and lets them to interact meaningfully with a device.
  - Switch Access: allows to interact with devices using one or more switches.
  - Voice Access (beta): allows to control a device with spoken commands.
  - Talkback: a screen reader commonly used by visually impaired or blind users.
  
* **Interceptors**

   Interceptors are a powerful mechanism that can monitor, rewrite, and retry the API call. So basically, when we do some API call, we can monitor the call or perform some tasks (watch between the client and server). The common use-cases are Logging the errors centrally, Caching the response

* **Caching mechanism**
  - Memory Cache: It keeps the data in the memory of the application. If the application gets killed, the data is removed. Useful only in the same session of application usage. Memory cache is the fastest cache to get the data as this is in memory.
  - Disk Cache: It saves the data to the disk. If the application gets killed, the data is retained. Useful even after the application restarts. Slower than memory cache, as this is I/O operation.
  
* **Advantages of Caching**
  - Reduce network calls: We can reduce the network calls by caching the network response.
  - Fetch the data very fast

attching viewmodel to actvity or fragment?
Fragment - 


