# Rondevu

A proximity information exchange library for your smartphone.

## The Basics

Rondevu transforms your physical world by allowing closeby devices to exhange meaningful information.

## Getting Started

To install Rondevu, in your `build.gradle`:

```gradle
 dependencies {
   compile 'com.rondevu.android:android-sdk:0.1'
 }
``` 

In your `Application` class:

```java
public class YourApplication extends Application {

  @Override public void onCreate() {
    super.onCreate();
    Rondevu.install(this);
    // Normal app init code...
  }
}
```

In your `Activity` or `Fragment` class where you want to listen to updates from nearby nodes, instantiate Rondevu:

```java
public class YourActivity implements Node {

  @Override public void onCreate() {
    super.onCreate();

    // Instantiate Rondevu according to requirement
    Rondevu.startNode(this)
    	.alwaysOn(false)
    	.listenForAction("relevant-action")
    	.initialize();
    
    // Remaining boot code
  }
}
```

Nodes listening for `Action`s would extend the following method:

```java
@Override
public onRequest(Action relevantAction, Object requestObject) {
	// Do the necessary processing for this action and respond
	Rondevu.respond(releavantAction, responseObject);
}
```

Nodes wanting to perform `Action`s would extend the following method:

```java
Rondevu.perform(relevantAction);

@Override
public onResponse(Action relevantAction, Object responseObject) {
	// Acknowledge response
	Rondevu.ack(releavantAction);
}
```