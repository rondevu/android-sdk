# Rondevu

Rondevu is transforming your physical world. It is doing so by enabling your app to exchange meaningful information with other smart devices closeby which contain your app.

## The Basics

The Rondevu world is comprised of *Nodes* and *Actions*.

### Nodes

Rondevu converts smart devices into proximity-aware entities called *Nodes*. Nodes of an app can interact with other nodes which are closeby. 

### Actions

*Actions* are the medium using which *Nodes* interact. Actions are like functions, using which you can request an operation to be performed, and other Node(s) listening for that Action respond. 

```
     perform Action
Node ------------------> Node'
     (Request Object)

       respond
Node <------------------ Node'
       (Response Object)

```

Actions would be instantiated via:
```java
Action action = new Action("UUID"); // Example UUID: e3e28095-28b9-48dd-ae14-abc737eb0b09
```

To trigger an Action:
```java
Rondevu.perform(action);
```

Remember that:

* Each smart device in the Rondevu world is a `Node`
* A smart device of a unique app and a unique device-id is a unique Node
* Nodes interact with other nodes via an `Action`
* An Action is instantiated via a `UUID`, an universally unique identifier. Same UUID implied same Action and vice-versa
* Nodes can either
 * Perform Actions
 * Listen and respond to Actions
 * Or do both
* To listen and process Actions of other Nodes, a Node would need to do so via `listenFor(action)` during instantiation
* To be visible to other nodes in the proximity, Nodes would declare that they are available to a certain *Action*. 
* To interact with other nodes, Nodes would *perform an Action* on nearby nodes and wait for a *response*.

## Getting Started

To install Rondevu, in your `build.gradle`:

```gradle
 dependencies {
   compile 'com.rondevu.android:android-sdk:0.1'
 }
```


In your `Activity` or `Fragment` class where you want to listen to or perform `Action`:

```java
public class YourActivity implements Node {

  @Override public void onCreate() {
    super.onCreate();

    // Declare the interested Action
    Action interestedAction = Action.create("Unique-ID");

    // Instantiate Rondevu according to requirement
    Rondevu.startNode(this)
    	.listenFor(interestedAction) // optional
    	.initialize();
    
    // Remaining boot code
  }
}
```


Nodes listening for `Action` would extend the following method:

```java
@Override
public onRequest(Action relevantAction, Object requestObject) {
	// Do the necessary processing for this action and respond
	Rondevu.respond(releavantAction, responseObject);
}
```


Nodes wanting to perform `Action` would extend the following method:

```java
Rondevu.perform(relevantAction);

@Override
public onResponse(Action relevantAction, Object responseObject) {
	// Acknowledge response
	Rondevu.ack(releavantAction);
}
```