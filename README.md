# Android Cheat Sheet

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

  LiveData is somewhat similar to RxJava except that LiveData is lifecycle aware. LiveData excel on ViewModel layer, with its tight integration with Android lifecycles and ViewModel. RxJava provide more capabilities in transformations. We use RxJava in data source and repository layers, and it's transformed into LiveData (using LiveDataReactiveStreams) in ViewModels (before exposing data to activities/fragments).
  
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
  
* **Singleton s DI**

  <ini>Singleton</ini>: 
    - When using Singletons in codebase, you loose the insight of what methods are doing? For instance, when class ```Foo``` depends on ```Logger.getInstance()```, ```Foo``` is effectively hiding it's dependencies from consumers. This means you can't fully understand ```Foo``` or use it with confidence unless you read it's source and uncover the fact that it depends on Logger. 
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
