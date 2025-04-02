# **The Activity Lifecycle in Android 📱**  

In Android, an **Activity** represents a single screen in an app. Since Android apps run on **a variety of devices** with different system constraints, **activities go through a lifecycle** managed by the Android system.  

Understanding the **activity lifecycle** helps developers manage resources **efficiently**, handle **configuration changes**, and create a **smooth user experience**.  

---

## **1. Overview of the Activity Lifecycle**  

An **activity lifecycle** consists of different **stages (states)** that define how an activity behaves when the user **opens, interacts with, leaves, or returns** to it.  

### **Lifecycle States:**
1️⃣ **Created** – The activity is first created.  
2️⃣ **Started** – The activity becomes visible to the user.  
3️⃣ **Resumed** – The activity is in the foreground and ready for interaction.  
4️⃣ **Paused** – Another activity is covering part of this activity.  
5️⃣ **Stopped** – The activity is no longer visible to the user.  
6️⃣ **Destroyed** – The activity is being removed from memory.  

---

## **2. Activity Lifecycle Callbacks (Methods)**
Android provides **lifecycle methods** to handle each stage of the activity's lifecycle.  

### **Full Lifecycle Flow:**
```mermaid
graph TD;
    A[onCreate()] --> B[onStart()];
    B --> C[onResume()];
    C -->|User interacts| C;
    C --> D[onPause()];
    D --> E[onStop()];
    E --> F[onDestroy()];
    D -->|User returns| C;
    E -->|User restarts activity| B;
```

### **Lifecycle Methods and Their Purpose**
| **Method** | **When is it called?** | **Purpose** |
|------------|-----------------|----------------------|
| `onCreate()` | When activity is first created. | Initialize resources like UI components, variables, and set content views. |
| `onStart()` | When activity becomes visible. | Prepare the app to be visible to the user. |
| `onResume()` | When activity moves to the foreground. | Start animations, listeners, or interactions. |
| `onPause()` | When another activity is covering it. | Pause animations, release resources, save user input. |
| `onStop()` | When activity is no longer visible. | Save app data, stop heavy operations. |
| `onDestroy()` | Before activity is destroyed. | Clean up resources, unregister listeners. |

---

## **3. Understanding Each Lifecycle Stage in Detail**

### **1️⃣ onCreate() – Activity Creation**
- Called **only once** when the activity is created.  
- Used for **initial setup** like setting layouts, initializing variables, and creating UI components.  

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main); // Set the UI layout
    Log.d("Lifecycle", "onCreate called");
}
```
✔ **Best for:** Initializing UI, setting up database connections, creating ViewModels.  

---

### **2️⃣ onStart() – Activity Becomes Visible**
- Called when the activity is becoming **visible to the user**.  
- Can be called **multiple times** if the activity is stopped and restarted.  

```java
@Override
protected void onStart() {
    super.onStart();
    Log.d("Lifecycle", "onStart called");
}
```
✔ **Best for:** Restoring UI state, setting up event listeners.  

---

### **3️⃣ onResume() – Activity in Foreground (Active)**
- Called when the user can **interact** with the activity.  
- Activity is in the **foreground** (user can click buttons, enter text, etc.).  

```java
@Override
protected void onResume() {
    super.onResume();
    Log.d("Lifecycle", "onResume called");
}
```
✔ **Best for:** Start animations, register listeners (e.g., GPS, sensors).  

---

### **4️⃣ onPause() – Activity Losing Focus**
- Called when the activity is **partially visible** (e.g., a dialog appears).  
- **Avoid running heavy operations** here, as the activity can still be resumed.  

```java
@Override
protected void onPause() {
    super.onPause();
    Log.d("Lifecycle", "onPause called");
}
```
✔ **Best for:** Pause animations, release resources (e.g., camera, microphone).  

---

### **5️⃣ onStop() – Activity Completely Hidden**
- Called when the activity is **no longer visible** to the user.  
- Activity can be **restarted** later.  

```java
@Override
protected void onStop() {
    super.onStop();
    Log.d("Lifecycle", "onStop called");
}
```
✔ **Best for:** Stop background tasks, release unnecessary resources.  

---

### **6️⃣ onDestroy() – Activity is Destroyed**
- Called **before the activity is removed from memory**.  
- This is **not always called** (e.g., system kills activity due to low memory).  

```java
@Override
protected void onDestroy() {
    super.onDestroy();
    Log.d("Lifecycle", "onDestroy called");
}
```
✔ **Best for:** Cleanup operations, unregister listeners, releasing memory-heavy objects.  

---

## **4. Handling Configuration Changes (e.g., Screen Rotation)**
When the device **rotates** (portrait ↔ landscape), the activity is **destroyed and recreated**.  

### **Solution 1: Retain Data Using ViewModel**
```java
public class MyViewModel extends ViewModel {
    private MutableLiveData<String> myData = new MutableLiveData<>();
    
    public LiveData<String> getData() {
        return myData;
    }

    public void setData(String data) {
        myData.setValue(data);
    }
}
```
✔ Prevents data loss when activity **recreates after rotation**.  

---

### **Solution 2: Save Data Using onSaveInstanceState()**
```java
@Override
protected void onSaveInstanceState(Bundle outState) {
    super.onSaveInstanceState(outState);
    outState.putString("user_input", editText.getText().toString()); 
}
```
✔ Saves **user input** when the screen rotates.  

---

## **5. Real-World Example (Using Logs to Track Lifecycle)**
```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Log.d("Lifecycle", "onCreate called");
    }

    @Override
    protected void onStart() {
        super.onStart();
        Log.d("Lifecycle", "onStart called");
    }

    @Override
    protected void onResume() {
        super.onResume();
        Log.d("Lifecycle", "onResume called");
    }

    @Override
    protected void onPause() {
        super.onPause();
        Log.d("Lifecycle", "onPause called");
    }

    @Override
    protected void onStop() {
        super.onStop();
        Log.d("Lifecycle", "onStop called");
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        Log.d("Lifecycle", "onDestroy called");
    }
}
```
✔ This will **print logs** when the activity transitions between **states**, making debugging easier.  

---

## **6. Summary of Best Practices**  

✅ **Use ViewModel** to **retain data** across configuration changes.  
✅ **Release heavy resources** (e.g., camera, GPS) in `onPause()` or `onStop()`.  
✅ **Save small UI state changes** using `onSaveInstanceState()`.  
✅ **Avoid long-running tasks** in lifecycle methods (use `WorkManager` for background tasks).  
✅ **Monitor lifecycle changes** with `Log.d()` for better debugging.  
