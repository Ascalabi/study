# Android Dev


**WorkManager** performs background task without UI.

## App components

### Activities
Entry point for interacting with the user. A single screen with a UI

**MainActivity** is the first activity the user sees when they launch the app for the first time.

Each time a new activity starts, the previous activity is stoppped (activity lifecycle callback methods). The system preserves the state of the activity in a stack (**base stack**)

Getting data back from and activity: Call startActivityForResults() instead of startActivity()

**Up navigation** need to add `android:parentActivityName=".MainActivity">` in Android Manifest

### States
![[Pasted image 20220104203254.png]]


#### Intents
Activities are started or activated with an **Intent**. In addition to starting an activity, an intent can also be used to pass data between two activities.

> **Explicit intent:** you specify the receiving activity (or component) using the fully qualified class name.

> **Implicit intent:**  You declare a general action to perform, and the android system matches your request to an activity or component.

```java
//Start the  ShowMessageActivity to show a specific message
Intent messageIntent = new Intent(this, ShowMessageActivity.class);
startActivity(messageIntent);
```

### Services
A general-purpose entry point for keeping app running in the background. It does not provide a UI.

> **Started services** keep running untill their work is completed.

> **Bound Services** run because some other app (or the system) has said that it needs them

### Broadcast receivers
For notifications???

### Content providers
Something something databases? something


## Components 

### MainActivity
**./app/java/com.example.app/MainActivity**

is the entry point for your app

./app/res/layout/**activity_main.xml** defines the layout 

### Android Manifest
./app/manifest/**AndroidManifest.xml**

The manifest file describes the fundamental characteristics of the app and declears each of its components. As well as identifies user permissions the app requires


### Gradle Scirpts
./Gradle Scripts/..
for building projects
`run ./gradlew task` 

## Threads

### UI Threads
Also reffered to as the Main Thread.

Two rules:
 - Do not block the UI thread
 - Do UI work only on the UI thread

So basically: Complete all work in less than 16ms for each UI screen, don't run asynchronous tasks and other long-running tasks on the UI thread

### Worker Thread
Is any thread which is not the UI thread. Use `AsynchTask` which allows you to perform background operations on a worker thread and publish results on the UI thread. 

![[Pasted image 20220104213232.png]]

Uses of `AsyncTask` (It can be impractical to use sometimes):
- Short or interruptible tasks
- Tasks that don't need to report back to UI or user
- Low-priority tasks that can be left unfinished

Use `AsyncTaskLoader` for everything else


## Storage

