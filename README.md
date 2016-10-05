# Rondevu

Rondevu is transforming your physical world. It is doing so by enabling your app to exchange meaningful information with other smart devices closeby which contain your app.

## The Basics

* Each device in the Rondevu world is a *Node*. Nodes of the same app can interact with other nodes which are closeby. 
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