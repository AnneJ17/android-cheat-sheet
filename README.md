# Android Cheat Sheet

## Table of contents
1. [App Basics](#basics)
2. [Activities](#activities)
3. [Intent & Intent Filters](#intents)
4. [Services](#services)
5. [Architecture Components](#components)
    * [DataStore](#datastore)
    * [LiveData](#livedata)
    * [ViewModel](#viewmodel)
    * [ViewBinding](#viewbinding)
    * [DataBinding](#databinding)
    * [Paging](#paging)
    * [Navigation](#navigation)
    * [WorkManager](#workmanager)
6. [Design Patterns](#patterns)
7. [Background Tasks](#tasks)
    *  [Broadcast](#broadcast)
    *  [RxJava](#rxjava)
    *  [Coroutines](#coroutines)
8. [App Data](#data)
9. [Firebase](#firebase)
10. [Best Practices](#practices)
    *  [Dependency Injection](#DI)
    *  [Testing](#testing)
    *  [Performance](#performance)
    *  [Accessibility](#accessibility)
11. [Miscellaneous](#miscellaneous)

## App Basics <a name="basics"></a>

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

  A developer writes code for an android app and must compile the code to run on an android device. The Java files are compiled by a Java compiler into Java bytecode files. The bytecode is compiled by the R8 compiler into Dex bytecode files. When the app is installed on an android device, an AOT compiler compiles the Dex bytecode into .oat files. When the app is executed, the ART virtual machine runs the optimized code.	ART is responsible for allocating memory to the application as well as deallocate the memory. Whenever the activity get destroyed, ART tell the garbage collector to deallocate the memory.<br>
  Dalvik behaves even worse in terms of memory management and Garbage Collection.
 
* **APK file**
  
  An android project is composed of Java code, app resources, and the manifest file. Gradle starts the build by compiling an app's Java files into Dex bytecode and resources folder into single class file called R.java using the aapt (android asset packaging tool) tool. Gradle zips the Dex bytecode, the app resources, and manifest into a single APK file. The APK file can be installed on an android emulator or an android device, or the APK can be published in Google Play Store where others can download the app.
  
* **Android SDK**
  
  The Android SDK is a collection of software development tools and libraries required to develop Android applications. It comprises all the tools necessary to code programs from scratch and even test them.<br>
  Components of Android SDK:
    - Android SDK Tools: includes a complete set of development and debugging tools for Android, and is included with Android Studio
    - SDK Build Tools: for building components for your Android app
    - SDK Platform-Tools: Android Debug Bridge (adb), fastboot (flash a device with a new system image), systrace (collect and inspect timing information of processes running on your device at the system level which is crucial for debugging app performance)
    - SDK Platform: For each version of Android, there's one SDK Platform available (eg. Android 11 (API level 30))
    - Google APIs: Google provides Google APIs to make developing your app easier. They also offer a system image for the emulator so you can test your app using the Google APIs.
    - Android Emulator: QEMU-based device-emulation tool that simulates Android devices
    
* **What is ADB tool used for?**

  Android Debug Bridge (ADB) is a command line tool that lets you communicate with an Android device that is connected over USB, or with an emulator. It allows you to pull data from the device such as application log files, memory usage data, and push and pull applications.
  
* **What is Manifest file?** 

  It is an application level configuration file. All the metadata of the application like appName, launch icon, activities, services, and permissions are found here.
 
* **Scope functions**
  
  The Kotlin standard library contains several functions whose sole purpose is to execute a block of code within the context of an object. 
When you call such a function on an object with a lambda expression provided, it forms a temporary scope. In this scope, you can access the object without its name. Such functions are called scope functions. There are five of them: let, run, with, apply, and also. apply and also return the context object.let, run, and with return the lambda result.

   | Function       | Object reference     | Return Value   |
   | :------------- | :------------------: | :------------- |
   | let            | it                   | Lambda result  |
   | run            | this                 | Lambda result  |
   | apply          | this                 | the object     |
   | with           | this                 | Lambda result  |
   | also           | it                   | the object     |

* **Lazy vs lateinit**
  - `lazy { ... }` delegate can only be used for `val` properties, whereas `lateinit` can only be applied to `var`s, because it can't be compiled to a `field`, thus no immutability can be guaranteed
  - lateinit var has a backing field which stores the value, and by lazy { ... } creates a delegate object in which the value is stored once calculated, stores the reference to the delegate instance in the class object and generates the getter for the property that works with the delegate instance. So if you need the backing field present in the class, use lateinit;
  
* **Inline function**

  So, calling non-inline function and passing a lambda to it will always create an instance of an object. But when we use the keyword 'inline', no new instance is created, instead, the code around the invocation of block inside the inlined function will be copied to the call site. Inlining works best for functions with parameters of functional types or lambdas(Higher-order functions). You wouldn't want to inline by default: Inlining may cause the generated code to grow; however, if we do it in a reasonable way (i.e. avoiding inlining large functions), it will pay off in performance

## Activities <a name="activities"></a>
 
* **What is Activity?** 

  Activities are the main UI component of the application. Whatever is visible to user's eye is coming from the activity. Each activity contains 2 main part - one is the business logic (kotlin/java class) and the other is the view/ui (layout/xml files)
 
* **What is Activity life cycle?**

  Contains various methods that we can use to track the state of the activity. If we wish to execute some logic then we can override these methods and we can have our own implementation. There are 7 lifecycle methods - onCreate(), onStart(), onResume(), onPause(), onStop(), onRestart(), onDestroy()
  
* **What are launch modes?**
  
  Launch mode is an instruction denoting how an activity should be launched. There are 4 modes:-
    1. standard: This is the default mode where the system creates a new instance of the activity in the target task or routes intent to it.
      - Example: Suppose there is an activity stack of A -> B -> C. Now again launching activity B with “standard” launch mode, the new stack will be A -> B -> C -> B.
    2. singleTop: Activity will be created once and will be on top. If an instance of the activity already exists at the top of the target task, the system routes the intent to that instance through a call to its onNewIntent() method. 
      - Example: Suppose we have an activity stack of A -> B -> C -> D and C is launched. As it is not on top, a new instance of C will be created. So it will look like A -> B -> C -> D -> C. But if D was launced instead, a new instance of D will not be created rather we will receive the callback on onNewIntent() method.
    3. singleTask: Here, no multiple instances are created. The system creates the activity at the root of a new task and routes the intent to it. 
      - Example: Suppose there is an activity stack of A -> B -> C and activity D is launched. Then the stack will be A -> B -> C -> D (Here D launch as usual)
      - Suppose there is an activity stack of A -> B -> C -> D. State of Activity Stack after launch B activity will be A -> B (Here old instance gets called and intent data route through onNewIntent() callback)
    4. singleInstance: No multiple instances, same as "singleTask", except that the system doesn't launch any other activities into the task holding the instance.
      - Suppose state of activity stack is A -> B -> C. After launch D activity of "launch mode = singleInstance", the state will be Task1 — A -> B -> C, Task2 — D (here D will be in different task).
      - Now if you continue this and start E and D then Stack will look like -> Task1 — A -> B -> C -> E, Task2 — D
    
* **singleTask vs singleInstance**
  
  The "singleTask" and "singleInstance" modes also differ from each other in only one respect: A "singleTask" activity allows other activities to be part of its task. It's always at the root of its task, but other activities (necessarily "standard" and "singleTop" activities) can be launched into that task. A "singleInstance" activity, on the other hand, permits no other activities to be part of its task.

  
* **Fragment**
  Fragments can be used as part of your application's layout, allowing you to better modularize your code and more easily adjust your user interface to the screen it is running on. In short, fragment segments your app into multiple, independent screens that are hosted within an Activity.
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
  
* **Passing data between fragments** [*](https://heartbeat.fritz.ai/passing-data-between-fragments-on-android-using-viewmodel-d47fa6773f9c)

	One can implement interfaces or use view model to share data between the fragments. It's not adviced to communicate directly with other fragments as this makes the classes tightly coupled. Using `ViewModel` and `LiveData` to pass data between fragments has a number of advantages, such as separation of controllers from data handling and avoiding repeated data fetching due to configuration changes like screen rotation. This is because `ViewModel` is tied to the activity lifecycle.
  
* **Ways to add fragment**
  1. Manually - `<fragment id="" name="fully qualified name of the class"/>` Name and id attributes are mandatory if using `<fragment>` tag
  2. Dynamically - Add fragment in the activity and use fragment manager
  
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
  - Drawing stage: `onDraw()` - after measuring and positioning all of the views, each view happily draws itself. This method provides a canvas for that. `invalidate()`: makes Android redraw the view by calling onDraw()
  
* **What is res folder?** 

  Keep all the xml files needded of your application which are used for designing or for storing data  
    - Drawables: Include static contents (images)
    - Layout: xml files that represents the UI
    - Mipmap: launch icons
    - Values: data files - colors.xml, strings.xml, styles.xml, dimens.xml
 
* **Linear layout vs relative layout vs constraint layout** 
  - LinearLayout - used when we want to arrange the views/widgets in a linear fashion either horizontal or vertical
  - constraint layout - optimize and flatten the view hierarchy - avoid nesting of the views to get better performance. 
  - Relative layout - nested and have to position it, 
  - Framelayout - container as a placeholder as it's an empty layout
  
* **When to use coordinate layout?**
  
  Coordinate layout position top-level application widgets, such as AppBarLayout and FloatingActionButton. 

* **List view vs recycler view**

  * <ini>ListView</ini> - displays all the items vertically in a scrollable fashion and creates view for each items. List View being rebuilt/redrawn too many times as the user scrolls result in performance overhead on the CPU.
  * <ini>RecyclerView</ini> - It's an advanced form of list view.
    1. Reuses cells while scrolling up/down - this is possible with implementing View Holder in the ListView adapter, but it was an optional thing, while in the RecycleView it's the default way of writing adapter. Therefore, memory efficient because it keeps on recycling the view as the view is gotten from the view holder. ViewHolder holds the view and when the recycler view recycles it gets the view from the ViewHolder.
    2. Decouples list from its container - so you can put list items easily at run time in the different containers (linearLayout, gridLayout, staggeredLayout) with setting LayoutManager.
    3. Animates common list actions - Animations are decoupled and delegated to ItemAnimator.
    4. Item decoration - gives us huge control to develop and have custom divider, space, borders etc
    5. OnItemTocuhListener - handle the user interaction and to enhance user experience. Useful for gestural manipulation of item views (on slide delete)

* **Adapters**

  Adapters acts like a bridge that connects two incompatible classes to work together. Some types of adapters are 
    - BaseAdapter: abstract class which implements ListAdapter and SpinnerAdapter Interface. Hence, we may use it for implementing both ListView and Spinner.
    - CursorAdapter: more appropriate when there is a database because it does not load all the records as ArrayAdapter. It loads only the visible records, or the records you are querying
    - Array Adapter - Simple adapter where we pass context, layout, data 
    - FragmentPageAdapter - ViewPager 
    - RecyclerView adapter
    
## Intent & Intent Filters

* **What is an Intent?** <a name="intents"></a>

  `Intent` is a messaging object that carries information that the Android system uses to determine which component to start plus the information that the recipient component uses in order to function properly. Through intent, one can start a new actvity, start a service, deliver a broadcast, open a web page, camera.
  1. Explicit intent - Know both the action and the target component
  2. Implicit inetnt - Know the action (opening 3rd party components) but don't know the target component
  
* **What is sticky intent?**
  
  - Sticky intent: Sticks with Android, for future broadcast listeners. It is a broadcast from sendStickyBroadcast() method such that the intent floats around even after the broadcast, allowing others to collect data from it. The Android system uses sticky broadcast for certain system information. For example if BATTERY_LOW event occurs then that Intent will stick with Android so that any future requests for BATTERY_LOW, will return the Intent.
  
* **What is PendingIntent?**

  A pending intent is a token that you give to another application. That is, If you want some one to perform any Intent operation at future point of time on behalf of you, then we will use Pending Intent. For example, the notification manager, alarm manager or other 3rd party applications. This allows the other application to restore the permissions of your application to execute a predefined piece of code.
  
* **Types of data that can pass in intent** 
  - Primitive types - we can pass primitive types via event but have to use serializable or parcelable to pass objects.
  - Serializables - We have to make the object's class implement Serializable. It converts the object into byte streams and java has it's on implementation for serializable. Unfortunately, this approach is slow as we use reflection. This method creates a lot of temporary objects and causes quite a bit of garbage collection.
  - Parcelable - Parcelable give better performance as we are being explicit about the serialization process instead of using reflection to infer it. We have to implement parcelablel interface. `writeToParce` responsible for serializing the data, and `Parcelable.Creator` is responsible for desirializing.

* **What is Intent Filter?**
  
  Intent filters specifies the types of intents that an activity, service, or broadcast receiver can respond to. It filter out intents that these components are willing to receive. When you create an implicit intent, the Android system finds the appropriate component to start by comparing the contents of the intent to the intent filters declared in the manifest file of other apps on the device.<br>
  <action> -> generic action to perform (eg. ACTION_MAIN - main entry point)
  <category> -> what kind of component that should handle the intent (eg. CATEGORY_DEFAULT, CATEGORY_BROWSABLE, CATEGORY_LAUNCHER)
  <data> -> Declaes the data type accepted to be acted on. Can be just a URI, or both a data type(MIME type) and a URI.

## Services <a name="services"></a>

* **What is a service?**

  A service is one of the Android component that performs long running task in the background, <ini>even when the user is not interacting with the application</ini>. By default, service runs on the main thread and should create a new thread if performing intensive or blocking operations to avoid ANR errors. The service lifecycle —from when it's created to when it's destroyed— can follow either of these two paths: started service, 
  service. <br>
  Note: if you want to run a task outside of the main thread, but only while the application is running, use a thread. 
  
* **When to use services?**
  
  We cannot use main thread for long running tasks as the android system uses it to update the UI and if it's blocked for more than 5 sec, we will receive an ANR error. So, to avoid these we can use background thread which can be achieved by using a Thread or an executor. But using a thread or executor still not ideal since a screen rotation can disrupt things, since actvity will not be there when the task is done. You could use AsyncTask to handle this, but what if your app needs this Background Thread to be started from not just an Activity, but a notification or another component?

* **Creating a service**
  
  If the service is started by the command `startService()`, the service continues to run until it stops itself with stopSelf() or another component stops it by calling stopService(). In this case, onStartCommand() is called. If the service is started by `bindService()`, onStartCommand() is not called and the service runs only as long as the component is bound to it.<br>
  Below are the callback methods that need to be overridden:- 
  - onStartCommand(): The service receives the intent here. 
  - onBind()
  - onCreate()
  - onDestroy()
 
* **IntentServices**

  The IntentService is used to perform a certain task in the background. Once done, the instance of IntentService terminates itself automatically. An example for its usage would be downloading certain resources from the internet. It offers onHandleIntent() method which will be asynchronously called by the Android system.
  
* **Creating an IntentService**
  - define a class within your application that extends IntentService and defines the onHandleIntent
  - Register the intent service in the app manifest file
  -You can start the IntentService from any Activity or Fragment by calling startService(), the IntentService does the work defined in its onHandleIntent() method, and then stops itself.
  - To communicate back to the application, there is two approches - ResultReceiver, BroadcastReceiver
    - ResultReceiver - Generic callback interface used If your service only needs to connect with its parent application in a single place.
    - BroadcastReceiver - Generic broadcast event which can then be picked up by any application. If your service needs to communicate with multiple components that want to listen for communication
  
* **JobIntentService vs IntentService**

  JobIntentService is not recommended for new apps as it will not work well starting with Android 8 Oreo, due to the introduction of Background execution limits. 
  - IntentService: Creates a new thread to perform a task and the given job is done on it's onHandleIntent(). The job given to the IntentService would get lost when the application is killed. 
  - JobIntentService: The application can kill this job at any time and it can start the job from the beginning once the application gets recreated/up.
  
* **Service Category**
  - StartService
  - BoundService
  
* **Types of Services**
  - Foreground Services:  A foreground service performs operation that is noticeable to the user. It must provide a notification, which is placed under the "Ongoing" heading, which means that the notification cannot be dismissed unless the service is either stopped or removed from the foreground. (e.g. audio app using foreground service to play audio track). Use `startForegroundService()` class to start this service. 
  - Background Services: A background service performs an operation that isn't directly noticed by the user. (e.g. app using background service to compact its storage)
  - Bound Services:  better choice for more complex two-way interactions between activities and services. It allows the launching component to interact with, and receive results from the service. Started service does not generally return results or permit interaction with the component that launched it (requires complex programming).
  
* **Bound Service**

  Bound service - Allows other components to bind it to the bound service to get some functionalities. One service can have multiple clients. Only when the last component unbounds, the service is stopped. So, it's important to stop the service by overriding the `onTaskRemoved()` and then calling the `stopSelf()`.<br>
  Local bound - client and service in same application. There is 2 way you can bound a service. One is after starting a service and then binding it or by just binding it directly. If you do allow your service to be started and bound, then when the service has been started, the system does not destroy the service when all clients unbind. Instead, you must explicitly stop the service by calling stopSelf() or stopService().<br>
  remote bound - AIDL - to connect to the client.
  
* **Why use Bound Service?**
  
  It allows components (such as activities) to bind to the service, send requests, receive responses, and perform interprocess communication (IPC). A bound service typically lives only while it serves another application component and does not run in the background indefinitely.
  
* **Ways to create bound services**

  There are 3 ways you can define the interface
    1. Extending the binder class - If your service is private to your own application and runs in the same process as the client 
    2. Using a Messenger - If you want to work across different processes, use Messenger. It creates a queue of all the client requests in a single thread, so the service receives requests one at a time.
    3. Using AIDL - If you want your service to handle multiple requests simultaneously, then use AIDL. Make sure your service is thread-safe and capable of multi-threading.
    
* **Steps for implementing AIDL**

  When implementing AIDL we want to keep some steps.
  1. Create an interface  - Implement the remote service
  2. class extending service
  
## Architecture Components <a name="components"></a>

* **Data Store** <a name="datastore"></a>
  
  Jetpack DataStore is a data storage solution that allows you to store key-value pairs (like `SharedPreferences`) or typed objects with protocol buffers. DataStore uses Kotlin coroutines and Flow to store data asynchronously, consistently, and transactionally. DataStore is ideal for small, simple datasets and is a replacement for SharedPreferences.

* **DataStore vs SharedPreferences** [*](https://blog.mindorks.com/jetpack-datastore-preferences)
  - `SharedPreference` has some drawbacks like it provided synchronous APIs -but it’s not MAIN-thread-safe! whereas DataStore is safe to use in UI thread because it uses `Dispatchers.IO` under the hood
  - DataStore Preferences support Kotlin Coroutines Flow API by default.
  - It’s safe from runtime exceptions
  - It also provides a way to migrate from `SharedPreferences`
  - It provides Type safety (Using Protocol buffers)
  
* **LiveData, MutableLiveData and MediatorLiveData** <a name="livedata"></a>

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
  
  
* **ViewModel** <a name="viewmodel"></a>

	ViewModel is one of the major component of MVVM pattern. ViewModel class is designed to store and manage UI-related data in a lifecycle conscious way. The ViewModel class allows data to survive device configuration changes(screen rotations, changes to keyboard availability). ViewModel is responsible for wrapping the model and preparing observable data needed by the view. It also provides hooks for the view to pass events to the model. The ViewModel is not tied to the view or Lifecycle objects which makes testing much easier.
	
* **Implementing ViewModel** [*](https://medium.com/androiddevelopers/viewmodels-a-simple-example-ed5ac416317e)
  1. Separate out your data from your UI controller(activity/fragment) by creating a class that extends ViewModel
  2. Set up communications between your ViewModel and your UI controller - create a member variable for your ViewModel in the UI Controller (ViewModelProviders.of(<Your UI controller>).get(<Your ViewModel>.class)).
  3. Use your ViewModel in your UI controller - To access or change UI data, you can now use the data in your ViewModel.ViewModel also works very nicely with another Architecture Component, LiveData whic is observable: it can trigger UI updates when the data changes.
	
* **How ViewModel retains data?**
  If the activity is re-created, it receives the same MyViewModel instance that was created by the first activity. When the owner activity is finished, the framework calls the ViewModel objects's onCleared() method so that it can clean up resources. ViewModel does not survive low memory or finish() scenarios as clear() method in ViewModelStore get called in onDestroy() of activity or fragment which clears the HashMap except during configuration changes.<br>
  Check [this article](https://proandroiddev.com/the-curious-case-of-survival-of-viewmodel-afe074992fbc#:~:text=According%20to%20the%20official%20documentation,in%20a%20lifecycle%20conscious%20way.&text=ViewModel%20objects%20are%20automatically%20retained,next%20activity%20or%20fragment%20instance.) for more details.
	
* **How does LiveData in the ViewModel update the Activity?**

  When we register the Observer in our Activity, we need to override the method onChanged(). The method onChanged() would get trigger whenever the LiveData is changed. Thus in the onChanged(), we can update the changed LiveData onto the View.
  
* **Benefits of ViewModel**
  - ViewModel survives rotation and other configuration changes
  - ViewModel keeps running while the activity is on the backstack
  - ViewModel is lifecycle agnostic. You can `override onCleared` (to clear disposables for example) just before the ViewModel is about to get killed.
  - ViewModel promotes reactiveness based on state propagated down to the view by letting the view observe the live data inside the ViewModel
  - ViewModel includes support for Coroutines
  - ViewModel works with Room and LiveData to replace the loader. Room informs the LiveData when the database changes, and the LiveData, in turn, updates the UI.
  
* **ViewBinding** <a name="viewbinding"></a>

  View binding works with your existing XML, and will generate a binding object for each layout in a module.
  1. Create binding reference inside your class: `private lateinit var binding YourClassBinding`
  2. Inflate your binding `binding = YourClassBinding.inflate(layoutInflater)` inside Activity's `onCreate` and call `setContentView(binding.root)`, or inflate it in Fragment's `onCreateView` then return it: `return binding.root`
  3. Reference views in code via binding using their ids `binding.textView.text = "Hello, world!"`
  
* **Data Binding** <a name="databinding"></a> [*](https://medium.com/androiddevelopers/data-binding-lessons-learnt-4fd16576b719)
  
  Bind UI components in your layouts to data sources in your app using a declarative format. Data Binding is the process that establishes a connection between the UI and business logic. When the data changes it's vlaue, the elements that are bound to the data reflect changes automatically.<br>
  Binding components in the layout file lets you remove many UI framework calls in your activities, making them simpler and easier to maintain. This can also improve your app's performance and help prevent memory leaks and null pointer exceptions.<br>
  - One-way binding:  there’s only one communication way: from observable to view.
  - Two-way binding: When properties in the model get updated, so does the UI. When UI elements get updated, the changes get propagated back to the model.
  
* **Paging** <a name="paging"></a>
  - Don't require the consumer to download all the data at once. Loading partial data on demand reduces usage of network bandwidth and system resources.
  
* **Navigation** <a name="navigation"></a>
  
  The navigation component helps you to manage navigations, fragment transactions, backstack, animations, most importantly deeplinking(don't have to use intent-filters). A navigation graph is a resource file that represents all of your app's navigation paths.
  
  
* **What is work manager?** <a name="workmanager"></a>
  
  WorkManager is intended for work that is deferrable—that is, not required to run immediately—and required to run reliably even if the app exits or the device restarts. For example:
  - Sending logs or analytics to backend services
  - Periodically syncing application data with a server
  
## Design Patterns <a name="patterns"></a>

* **MVP vs MVVM**

  - MVP - Model View Presenter: Presenter is like a bridge between the view and the model. There is one to one relation between the view and the presenter and therefore is tightly coupled. And is more verbose as each view needs a presenter.<br>
  - MVVM - Model View ViewModel: Each of the component is loosely coupled and follows event-driven pattern. VM holds the interaction between the view and the model. And all the business logic goes here. VM is lifecycle aware in which we can use live data and databinding and it is also recommended by Google.ViewModel doesn’t have to know anything about the View and has no reference to View classes
  
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
  
* **MVVM Architectural Pattern**

	**Model** - All your data goes in this layer; **Viewmodel** contains the business logic; **View** - Activities, fragments, layouts<br>
	Model - Repository and data lives here. Inside the repository we make retrofit calls. Set the data in LiveData. In ViewModel - we get the data from the repository.<br>
	View - call the viewmodel to get the data from the livedata which you display in your view

## Background Tasks <a name="tasks"></a>

* **Handler, Thread, Looper, and MessageQueue**

  - Main thread is responsible for handling events from all over the app like callbacks associated with the lifecycle
  - Handlers: For comomunicating between the threads. It enqueues task in the MessageQueue using Looper and also executes them when the task comes out of the MessageQueue.
  - Looper: It is a worker that keeps a thread alive, loops through MessageQueue and sends messages to the corresponding handler to process.
  - Message Queue: It is a queue that has tasks called messages which should be processed
  Using these handler and Main thread looper works fine, but not good solution as it allocates lot of memory. 
    1. we need to create a new thread every time we send data to the background
    2. Every time we need to post data back to the main thread using the handler 

* **What is broadcast receiver?** <a name="broadcast"></a> [*](https://developer.android.com/guide/components/broadcasts)
  
  BroadcastReceiver is a dormant component of android that listens to system-wide broadcast events or intents. That is, it allows you to register for system or application events. Some examples of system wide generated intents are:- 
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
   `RxJava` is a java based implementation of reactive extensions - a library that follows Reactive programming principles to compose asynchronous and event-based programs by following Observer pattern. Observables and Subscribers are the main building blocks. Observables for emmitting items and Subscribers(observer) for consuming the items. 
   
* **What is Reactive Programming?**

  It is an event based asynchronous event-based programming. Everything you see is an asynchronous data stream, which can be observed and an action will be taken place when it emits values. You can create data stream out of anything; variable changes, click events, http calls, data storage, errors and what not. When it says asynchronous, that means every code module runs on its own thread thus executing multiple code blocks simultaneously. Therfeore, the amount of time taken to complete all the tasks is equivalent to the longer task in the list.
  
* **How RxJava works** <a name="rxjava"></a>
    * Subscriber subscribes to Observable, then Observable calls Subscriber.onNext() for any number of items, if something goes wrong Subsciber.onError() is called and when the task is finished Subscriber.onCompleted() is called. 
    * Operators are methods created for solving transformations and handling API calls problems. Some of the **common operators** - Observable, Flowable, Single, Completable.
  <ini>Observable</ini> : Let us consider that We are making an API call and receiving the response. And that response is wrapped inside Observable type so that it can be processed by RxJava operators.
  <ini>Flowable<ini> : Each operator solves different problem, Flowable solving Backpressure ( Backpressure happens when there are multiple responses come at a speed that observers cannot keep up ). In Flowable, BackPressure is handled using BackPressure Strategies — MISSING, ERROR, BUFFER, DROP, LATEST. BackPressure is Handled by anyone of the mentioned strategies. Flowable is useful when we are making pagination call.
  <ini>Single</ini> : Single always either emits one value or an error. It returns Latest response for all requests. It is ideal for making search call.
<ini>Completable</ini> : Saving data to db. No return.
  
* **Key components of RxJava**
  - Observable: Observable is a data stream that do some work and emits data.
  - Observer: OnContrary to Observable, Observer receives the data emitted by the Observable.
  - Subscription: The bonding between Observable and Observer is called as Subscription. There can be multiple Observers subscribed to a single Observable. Example, login screen (checking all the fields are entered, each field act as a observable; enabling the button)
  - Operator / Transformation: Operators modifies the data emitted by Observable before an observer receives them.
  - Schedulers: Schedulers decides the thread on which Observable should emit the data and on which Observer should receives the data i.e background thread, main thread etc.,

* **Schedulers**

  Schedulers basically decides on which thread a task runs, whether main thread or backgournd thread. They are introduced in RxAndroid (AndroidSchedulers.mainThread()) which plays major role in supporting multithreading concept in android applications. List of schedulers: 
  - Schedulers.io() - Used for CPU-intensive works like network calls, database operations, reading disks/files. Maintaings pool of threads
  - AndroidSchedulers.mainThread() - provides access to the main thread. Jobs like updating UI, user interaction.
  
* **Disposables** - Used to dispose the subscription when Observer no longer wants to listen to Observable. It avoids memory leaks. 
  
* **RxJava Notes**
  - Observer pattern handles event-basd code but it doesn't have the notion of `onComplete` or `onError`. Developers have to remember to cancel the event listener, which otherwise leads to memory leak. Obsesrvable pattern can handle both async and event-based code. In addition to disposing on lifecycle termination event, there are many operators that can cancel an event-based observable. 
  - What is an Observable?
  	- Simply a collection that arrives over time
	- Can be finitie or infinite
	- OnNext: push (emit) the next value
	- OnCompleted: No more values to push
	- OnError: error occured when trying to push
   - Cancelling an infinite Observable
  	- TakeUntil: discard any items emitted by an Observable after a second Observable emits an item or terminates
	- Take: emit only the first n items emitted by an Observable
	- TakeWhile: discard items emitted by an Observable after a specified condition becomes false
	- Amb: given two or more source Observables, emit all of the items from only the first of these Observables to emit an item
  
  
* **Converting RxJava to LiveData**
* **Advantages and Disadvantages of RxJava**

* **Types of observables**
   - Observable: emit a stream elements (endlessly)
   - Flowable: emit a stream of elements (endlessly, with backpressure)
   - Single: emits exactly one element and then completes. Useful when we want to ensure we haven’t got empty outputs.
   - Maybe: emits zero or one elements. Unlike Single, it can complete without emitting a value. Useful when we have optional emissions (eg. getting a logged user but not signed in yet).
   - Completable: emits a “complete” event, without emitting any data type, just a success/failure. Useful when carrying out actions that don’t require a specific output (eg. when we make a login or send data).
   
* **Types of operators**

  - Filter: Filter items emitted by the source Observable by only emitting those that satisfy a specified predicate
  - Buffer: Buffer gathers items emitted by an Observable into batches and emit the batch instead of emitting one item at a time.
  - Debounce: Debounce operators emits items only when a specified timespan is passed. It's usually used when the Observable is rapidly emitting items but you are only interested in receiving them in timely manner (e.g. Instant search).
  - Zip: combine the values of multiple Observable together through a specific function (eg. attaching a picture to an API result, such as avatar to name).
  
* **RxJava map opeartors** [*](https://www.androidhive.info/RxJava/map-flatmap-switchmap-concatmap/)<br>
  FlatMap, ConcatMap, SwitchMap - instead of returning the modified item, it returns the Observable itself which can emit data again
  - Map: Used to alter the emitted data. It modifies each item emitted by a source Observable and emits the modified item.
  - FlatMap:  Used when the order is not important and want to send all the network calls simultaneously
  - ConcatMap: Unlike FlatMap, maintains the order of items and waits for the current Observable to complete its job before emitting the next one. more suitable when you want to maintain the order of execution.
  - SwitchMap: Used when you want to discard the response and consider the latest one. It unsubscribe from previous source Observable whenever new item started emitting, thus always emitting the items from current Observable (e.g. Instant search).
  
* **What is backpressure?**

	Basically a backpressure strategy indicates what to do with emitted items if they can’t be processed as fast as they are received. We can imagine, for instance, a flowable that sends gyroscope data with a really fast frequency and we need to apply a strong computation algorithm over each emitted item.<br>
	If the type spend for the algorithm is considerably higher than the time between each item’s emission, then backpressure strategy is applied. (If we use an Observable instead of a Flowable, then we will have a backpressure exception. The available options are drop, buffer, latest.
	
* **Coroutines** <a name="coroutines"></a>

  Coroutines = Co + Routines. Here, **Co** means cooperation and **Routines** means functions. It means that when functions cooperate with each other, we call it as Coroutines. It's an optimized framework written over the actual threading by taking advantage of the cooperative nature of functions to make it light and yet powerful. So, we can say Coroutines are lightweight threads. A lightweight thread means it doesn't map on native thread (stackless), so it doesn't require context switching on the processor, so they are faster. Coroutines do not replace threads, it’s more like a framework to manage it.<br>
  Coroutines are able to perform long-running and intensive tasks by suspending execution without blocking the thread and then resuming the execution at some time later. It allows the creation of non-blocking asynchronous code that appears to be synchronous.<br>
 Creating coroutines doesn’t allocate new threads (no memory allocated). Instead, they use predefined thread pools and smart scheduling for the purpose of which task to execute next and which tasks later.
 
* **Why we need coroutines?**
  
  We have many async tools like RxJava, AsyncTasks, Jobs, Threads, but why there is a need to learn something new?<br>
  While Using Rx, it requires a lot of effort to get it enough, to use it safely. On the Other hand, AsyncTasks and threads can easily introduce leaks and memory overhead. Even using these tools after so many disadvantages, the code can suffer from callbacks, which can introduce tons of extra code. Not only that, but the code also becomes unreadable as it has many callbacks which ultimately slow down or hang the device leading to poor user experience.<br>
  With coroutines we can launch thousandes of coroutine jobs and we can also suspend the function. We also can define scopes with the coroutine job.
  
* **Coroutine Features**

  - Lightweight: You can run many coroutines on a single thread due to support for suspension, which doesn't block the thread where the coroutine is running. Suspending saves memory over blocking while supporting many concurrent operations.
  - Fewer memory leaks: Use structured concurrency to run operations within a scope.
  - Built-in cancellation support: Cancellation is propagated automatically through the running coroutine hierarchy.
  - Jetpack integration: Many Jetpack libraries include extensions that provide full coroutines support. Some libraries also provide their own coroutine scope that you can use for structured concurrency.
  - RxJava works on android and Java. Coroutines are based on Kotlin. 
  - RXjava based on Observer pattern and well designed library and more complex. Greater learning curve. 
  
* **Coroutines vs Thread**
  - Fetching the data from one thread and passing it to another thread takes a lot of time. It also introduces lots of callbacks, leading to less readability of code. On the other hand, coroutines eliminate callbacks.
  - Creating and stopping a thread is an expensive job, as it involves creating their own stacks.,whereas creating coroutines is very cheap when compared to the performance it offers. coroutines do not have their own stack.
  - Threads are blocking, whereas coroutines are suspendable. 
  - Coroutines offer a very high level of concurrency. When a coroutine reaches a suspension point, the thread is returned back to its pool, so it can be used by another coroutine or by another process. When the suspension is over, the coroutine resumes on a free thread in the pool. At the moment when a coroutine suspends, the Kotlin runtime finds another coroutine to resume its execution.
  - Coroutines, unlike threads, also don’t need a lot of memory, just some bytes.Because of this, you can start many more coroutines than threadsBecause of this, you can start many more coroutines than threads. Each thread on a JVM consumes about 1MB of memory.
  
* **Coroutine Scopes**
  - @CoroutineScope
  - @GlobalScope
  - @LifecycleScope
  - @ViewModelScope
  
* **Dispatchers**

  Dispatchers help coroutines in deciding the thread on which the work has to be done. Dispatchers are passed as the arguments to the GlobalScope by mentioning which type of dispatchers we can use depending on the work that we want the coroutine to do. If not mentioned in the GlobalScope, then it cannot be predicted, sometimes it is DefaultDispatcher-worker-1, or DefaultDispatcher-worker-2 or DefaultDispatcher-worker-3.
  - Dispatchers.Main: Starts the coroutine in the main thread. for example when you update the UI.
  - Dispatchers.IO: Starts the coroutine in the IO thread. Used for input/output work (e.g. network call, reading from or wrtiing to database, writing to file).
  - Dispatchers.Default: Starts the coroutine in the Default Thread. Used to do Complex and long-running calculations and uses the same dispatcher as GlobalScope.launch { … }
  - Dispatchers.Unconfined:  not confined to any specific thread. It executes the initial continuation of a coroutine in the current call-frame and lets the coroutine resume in whatever thread that is used by the corresponding suspending function
  
* **Difference b/w launch/join and async/await in Kotlin Coroutines**
  - `launch` do not return any object and is used to fire and forget coroutine. It's like starting a new thread. If the code inside the `launch` terminates with exception, then it is treated like uncaught exception in a thread.`join` is used to wait for completion of the launched coroutine and it does not propagate its exception. However, a crashed child coroutine cancels its parent with the corresponding exception, too. async retruns the result.

  
## App Data <a name="data"></a>

* **Shared Preference and Sqlite**
  
  There are 4 ways to store data locally - Shared preference, Sqlite, Cotent Providers, External/Internal storage.<br>
  &nbsp;&nbsp;&nbsp;We use shared preference for saving small amount of data like app setting (theme), login status, user preference (notification status). It saves the data in form of key value pair and hence we can save only primitive type data like string, boolean, integers etc but we cannot store an object. Whereas, Sqlite is a relational database where we can store huge amount of data.
  
* **Components of RoomDB**

  - The database class that holds the database and serves as the main access point for the underlying connection to your app's persisted data.
  - Data entities that represent tables in your app's database.
  - Data access objects (DAOs) that provide methods that your app can use to query, update, insert, and delete data in the database.


* **RoomDB vs Sqlite**
  1. In the room there is sql verification done at compile time
  2. When schema changes you need to upgrade the affected sql queries Room solve this problem very easily
  3. We have to write lot of boiler plate code to convert sql queries and java object but room maps our database object to java object without any boiler plate code
  4. Room is built to work with live data and Rxjava for data observation while sqlite does not
  
  
* **What is content provider?**

  Content providers are used when there is a need to share data between multiple applications as Android-databases (Sqlite) created in Android are visible only to the application that created them.It let you centralize content in one place and different applications can access it. When you want to access data in a content provider, you use the ContentResolver object in your application's Context to communicate with the provider as a client. The ContentResolver methods provide the basic "CRUD" (create, retrieve, update, and delete) functions of persistent storage. In most cases this data is stored in an SQlite database. The CursorLoader objects rely on content providers to run asynchronous queries and then return the results to the UI layer in your application.<br>
  Actvity/Fragment <---> CursorLoader <---> ContentResolver <---> ContentProvider <---> DataStorage
  
* **Advantages of content providers**
  - Content providers offer granular control over the permissions for accessing data.
  - extra level of abstraction over your data to make it easier to change internally (changing underlying database structure).

## Firebase <a name="firebase"></a>

* **Firebase**
  
  Firebase is a toolset to buil, improve and grow applications (ios, android, web) cross platform and gives you services that normally a developer would have to build themselves. This includes authentication, databases, analytics, push notifications. The services are hosted in cloud, and scale with little to no effort.
  
* **Firebase Cloud messaging**

  Firebase Cloud Messaging (FCM) is a set of tools that sends push notifications and small messages upto 4kb in various platforms: Android, iOS, and web. Firebase is one of the simplest method to get notification. We as android developers prefer this method as this is very efficient and will not drain the battery of the device like polling (constantly requesting backend service for updates). 
  Following is the architecture of the FCM:-
    1. A service, API or console that sends messages to targeted devices.
    2. The Firebase Cloud Messaging back end, where all the processing happens.
    3. A transport layer that’s specific to each platform. In Android’s case, this is called the Android Transport Layer.
    4. The SDK on the device where you’ll receive the messages. In this case, called the Android Firebase Cloud Messaging SDK.
    
* **Push Notification using FCM** [*](https://blog.mindorks.com/pushing-notifications-in-android-using-fcm)
	
	So, when a device is registered to FCM server (on initial startup), we receive a registration token that is used to establish a connection with the FCM server. We access this token by creating a service class which extends `FirebaseMessagingService` and adds it to the Manifest file. To generate the token we need to call 
```
FirebaseInstanceId.getInstance().instanceId
      .addOnCompleteListener(OnCompleteListener { task ->
val token = task.result?.token
})
```
These unique tokens change when the user  uninstalls/reinstall s,... So we need to generate a new token which is done by overriding `onNewToken()`. Then, we can send it to the backend server where we can save it  and use it to send a notification later. `onMessageReceived()` is called when a message is received from FCM and we can wirite all the logic like generating & handling notification inside this method. 

## Best Practices <a name="practices"></a>

### Dependency Injection <a name="DI"></a>

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

  When using Singletons in codebase, you loose the insight of what methods are doing. For instance, when class ```Foo``` depends on ```Logger.getInstance()```, ```Foo``` is effectively hiding it's dependencies from consumers. This means you can't fully understand ```Foo``` or use it with confidence unless you read it's source and uncover the fact that it depends on Logger. 
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
  - @Qualifier - If different object of the same type are necessary (@Named is pre-defined method but @Qualifier can be used it to make a cleaner code)
  
* **DaggerHilt** [*](https://proandroiddev.com/exploring-dagger-hilt-and-whats-main-differences-with-dagger-android-c8c54cd92f18)

  Dagger can create a graph of the dependencies in your project that it can use to find out where it should get those dependencies when they are needed. To make Dagger do this, you need to create an interface and annotate it with @Component. Dagger creates a container as you would have done with manual dependency injection.<br>
  In Dagger-Android, we have to create a component class with a builder/factory, includes every module and we should inject the application context in the Application class after building our project. Here is the Dagger-Android way to construct a component.<br>
 ```
@Singleton
@Component(modules = [AndroidInjectionModule::class,ActivityModule::class,ViewModelModule::class,NetworkModule::class])
interface AppComponent : AndroidInjector<DaggerApplication> {

  @Component.Factory
  interface Factory {
    fun create(@BindsInstance application: Application): AppComponent
  }
}
class TheMoviesApplication : DaggerApplication() {

  override fun applicationInjector() = DaggerAppComponent.factory().create(this)
  
}
```
But in Hilt, we don’t need to create a component, include every module, and build for generating DaggerAppComponent class.
```
@HiltAndroidApp
class PokedexApp : Application()
```
Also, the instance of an App can be injected into other modules by the Hilt<br>
```
/** Provides a binding for an Android BinderFragment Context. */
@Module
@InstallIn(ApplicationComponent.class)
public final class ApplicationContextModule {
  private final Context applicationContext;

  public ApplicationContextModule(Context applicationContext) {
    this.applicationContext = applicationContext;
  }

  @Provides
  @ApplicationContext
  Context provideContext() {
    return applicationContext;
  }

  @Provides
  Application provideApplication() {
    return (Application) applicationContext.getApplicationContext();
  }

}
```
Hilt provides the ApplicationContextModule by default and it is followed by the whole application’s lifecycle. By annotating the `@HiltAndroidApp` annotation, an instance of the App will be injected into that module internally. So we don’t need to inject the instance of the application in the App class.

### Testing <a name="testing"></a>

* **What is TDD?**

  Test Driven Development - Developing something that is driven by test. We describe each unit of the system through a failing test and make it pass with an implementation. The test will serve as a guidline of what to build
  
* **Why TDD?**
  - TDD can help reduce bugs (Reliable)
  - TDD can be our code guard: For instance, when somebody working on other feature unintentionally changes the behavior of the previously written features, will cause our test to fail as it is not working as intended. This is a red light to go on to production and this will let the other developers to fix the error. This will also give more confidence to change the software without any fear.
  - TDD can act as code documentation (Readable):  In TDD, test are written step by step. If someone is unsure of how the code works, one can look over the tests written and understand the functionality of each unit. It gives insight of how a particular unit should work with both pass and fail tests (assertions). 
    
* **Mockito**
 
   An open source testing framework for Java. Used in Junit testing. 
   
* **Mock vs Stub vs Spy**

  - Mock: A mock is a dummy class replacing a real one, returning something like null or 0 for each method call.
  - Stub: A stub is a dummy class providing some more specific, prepared or pre-recorded, replayed results to certain requests under test. 
  - Spy: A spy is kind of a hybrid between real object and stub, i.e. it is basically the real object with some (not all) methods shadowed by stub methods.
  
* **PowerMock**
  - static, final and private methods
  - remove static initializers
  - allow mocking without dependency injection

* **Continuous Integration/ Continuous Deployment (CI/CD)**
  - Pipeline: CI and CD are often represented as a pipeline, where new code enters on one end, flows through a series of stages (build, test, staging, production), and published as a new production release to end users on the other end.
  - CI: It is a process where developers integrate their code into the master/main branch. Each merge triggers an automated code build and test sequence, which ideally runs in less than 10 minutes. A successful CI build may lead to further stages of continuous delivery.
  - CD: every change in the source code is deployed to production automatically
  
* **Lint Tool**
  
  The lint tool checks your Android project source files for potential bugs and optimization improvements for correctness, security, performance, usability, accessibility, and internationalization. When using Android Studio, configured lint and IDE inspections run whenever you build your app

  
### Performance <a name="performance"></a>

* **Performance Issues** - memory, UI, thread, battery
    
* **Memory Management**
  
  In Android, bitmaps represent the largest contiguous blocks of memory. They occupy heaps, which results in lots of contention to find free space to allocate new bitmaps as we scroll.This then results in more GC events so it can free up memory to provide the necessary space.
  
* **Memory Leak**

  There are few instance where garbage collector will try to collect and want get to collect an object thinking that the object is live and shouldn't collect. Garbage collector is not able to collect objects and the memory get collected with unused objects leads application crash with OutOfMemoryError. This is called memory leaks.
  
* **Common causes of memory leaks**

	Most memory leaks are caused by bugs related to the lifecycle of objects.
	- Adding a `Fragment` instance to the backstack without clearing that Fragment’s view fields in `Fragment.onDestroyView()`[*](https://stackoverflow.com/questions/59503689/could-navigation-arch-component-create-a-false-positive-memory-leak/59504797#59504797)
	- Storing an `Activity` instance as a `Context` field in an object that survives activity recreation due to configuration changes.
	- Registering a listener, broadcast receiver or RxJava subscription which references an object with lifecycle, and forgetting to unregister when the lifecycle reaches its end.

* **Types of reference** [*](https://medium.com/google-developer-experts/finally-understanding-how-references-work-in-android-and-java-26a0d9c92f83)

  A Reference consist addresses and class information about the object being referenced. Assigning Reference will not create distinct copies of Objects. All reference variables are referring to same Object.Depending upon how objects are garbage collected, references to those objects are grouped into 4 types.
  - Strong reference - Anytime we create a new object, a strong reference is by default created. This kind of reference makes the referenced object not eligible for Garbage Collection
  - Weak reference - They are likely to be garbage collected when JVM runs garbage collector thread (inner class, bitmap, unregistered)
  - Soft references - a SoftReference will beg to the GC to stay in memory unless there is no other option. That is, it can be deleted from a container if the clients are no longer referencing them and memory is tight.
  - Phantom references - It points to objects that are already dead and have been finalised.
  
* **Leak cannary library** - LeakCanary is a memory leak detection library for Android which help you find and fix these memory leaks during development.

* **Android profiler** 
	The Android Profiler tools provide real-time data to help you to understand how your app uses CPU, memory, network, and battery resources.
	- Inspect CPU activity and traces with CPU Profiler
	- Inspect the Java heap and memory allocations with Memory Profiler
	- Inspect network traffic with Network Profiler
	- Inspect energy usage with Energy Profiler
	
* **Caching mechanism**
  - Memory Cache: It keeps the data in the memory of the application. If the application gets killed, the data is removed. Useful only in the same session of application usage. Memory cache is the fastest cache to get the data as this is in memory.
  - Disk Cache: It saves the data to the disk. If the application gets killed, the data is retained. Useful even after the application restarts. Slower than memory cache, as this is I/O operation.
	
## Accessibility <a name="accessibility"></a>

    
* **What is Accessibility Service?**
  
  An Accessibility Service assists users with disabilities in using Android devices and apps. It is a long-running privileged service that helps users process information on the screen and lets them to interact meaningfully with a device.
  - Switch Access: allows to interact with devices using one or more switches.
  - Voice Access (beta): allows to control a device with spoken commands.
  - Talkback: a screen reader commonly used by visually impaired or blind users.
  
## Miscellaneous <a name="miscellaneous"></a>

* **Declarative and Imperative programming**
  - Declarative programming is a programming paradigm that expresses the logic of a computation without describing its control flow.
  - Imperative programming is a programming paradigm that uses statements that change a program’s state.

* **Github vs Git**

  Git is a distributed version control system or source code management system. Whereas, gitHub is a hosting service for Git repositories. In short - Git is the tool, GitHub is the service for projects that use Git.
 
* **Rebase and merge**

  Rebase - Moving or combining a sequence of commits to a new base commit. For instance, if you started doing some development and then another developer made an unrelated change. You probably want to pull and then rebase to base your changes from the current version from the repository.<br>
  Merge - Merging two different branches. For example, let's say you have created a branch for the purpose of developing a single feature. When you want to bring those changes back to master, you probably want merge.<br>
  
  Differences:
    - Merge preserves the branches while Rebase won't. 
  
* **Interceptors**

   Interceptors are a powerful mechanism that can monitor, rewrite, and retry the API call. So basically, when we do some API call, we can monitor the call or perform some tasks (watch between the client and server). The common use-cases are Logging the errors centrally, Caching the response
  
* **Product Flavor**

  Feature of Gradle plugin in Android Studio. (eg paid and free app). Both share common source code and resources. At the same time they both can have different features, device requirements and resources. 
  
  Advantages of Product Flavor
  - Single android studio project
  - Single repo in version control
  - common features and bug fixes in one go
  
  ```
  android {
    ...
     defaultConfig {...}
     buildTypes {
        debug{...}
        release{...}
     }
     flavorDimensions "version"
     productFlavors {
       basic {
       }
       premium {
       }
     }
  }
  ```
  
* **What is REST API?**

  REST stands for REpresentational State Transfer. It is an architectural style that defines constraints for creating web services. It is stateless, i.e, no client session data is stored on the server (eg. Once you make an authentication, you have to repeat every time you make a call).
  Every data is an entity. 

* **Code Review**
   
   Every alternate days we do code review. We check our colleagues code. Peer to Peer review.
   * Proper namingCovnetion, JavaDocs, Comments
   * Followed Desisign pattern
   * Written enough test cases
   * Any security issues
   * Any performance issues - are there any memory leaks, threads, battery, UI	
____________________________________________________________________________
<p align="center">:octocat: If you would like to contribute to the Android Cheat Sheet, just make a pull request!</p>
