# **Activities and Intents in Android**  

Activities and intents are core components in **Android development** that facilitate **navigation** and **interaction** between different screens and applications.  

---

## **1. Activities**  
An **Activity** represents a **single screen** in an Android app. It acts as the **entry point** for user interaction. Each activity is a subclass of **`Activity`** or **`AppCompatActivity`**.  

### **Key Lifecycle Methods:**  
- `onCreate()`: Initializes the activity.  
- `onStart()`: Prepares the UI for interaction.  
- `onResume()`: Activity becomes visible and interactive.  
- `onPause()`: Pause ongoing operations.  
- `onStop()`: Free up resources when activity is no longer visible.  
- `onDestroy()`: Cleans up before the activity is destroyed.  

### **Basic Activity Example:**  
```java
package com.example.myapp;

import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main); // Sets the UI layout
    }
}
```

### **Defining an Activity in AndroidManifest.xml**  
Every activity **must be declared** in `AndroidManifest.xml`:  
```xml
<activity android:name=".MainActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```
- The **intent-filter** makes `MainActivity` the **launcher activity** (entry point).  

---

## **2. Intents**  
An **Intent** is a messaging object used to **request an action** from another component. It enables communication within and between apps.  

### **Types of Intents:**  
1. **Explicit Intent**: Specifies the **target activity** (used **within the same app**).  
2. **Implicit Intent**: Does **not** specify the target; the system determines the best match.  

---

### **Explicit Intent Example (Launching Another Activity):**  
#### **MainActivity.java (Sending Data)**  
```java
Intent intent = new Intent(MainActivity.this, SecondActivity.class);
intent.putExtra("message", "Hello, Second Activity!"); // Passing data
startActivity(intent);
```

#### **SecondActivity.java (Receiving Data)**  
```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_second);

    // Retrieve data
    String message = getIntent().getStringExtra("message");
    TextView textView = findViewById(R.id.textView);
    textView.setText(message);
}
```

---

### **Implicit Intent Example (Opening External Apps):**  
#### **Example - Open a Web Page:**  
```java
Intent intent = new Intent(Intent.ACTION_VIEW);
intent.setData(Uri.parse("https://www.example.com"));
startActivity(intent);
```

#### **Example - Dial a Phone Number:**  
```java
Intent intent = new Intent(Intent.ACTION_DIAL);
intent.setData(Uri.parse("tel:1234567890"));
startActivity(intent);
```

---

## **3. Activity Lifecycle**  
Understanding the **activity lifecycle** is crucial for managing resources and ensuring a smooth user experience.  

### **Lifecycle States:**  
| **Method**     | **Description** |
|---------------|----------------|
| `onCreate()`  | Called when the activity is first created. |
| `onStart()`   | Activity becomes visible to the user. |
| `onResume()`  | Activity is now interactive. |
| `onPause()`   | Activity is partially visible (e.g., a dialog appears). |
| `onStop()`    | Activity is no longer visible. |
| `onDestroy()` | Activity is being destroyed. |

### **Lifecycle Example (Logging State Changes):**  
```java
@Override
protected void onStart() {
    super.onStart();
    Log.d("ActivityLifecycle", "Activity Started");
}

@Override
protected void onStop() {
    super.onStop();
    Log.d("ActivityLifecycle", "Activity Stopped");
}
```

---

## **4. Passing Data Between Activities**  
You can pass data using **`Intent` and `Bundle`**.  

### **Using Bundles:**  
#### **Sending Data (MainActivity.java)**  
```java
Intent intent = new Intent(MainActivity.this, SecondActivity.class);
Bundle bundle = new Bundle();
bundle.putString("key", "value");
intent.putExtras(bundle);
startActivity(intent);
```

#### **Receiving Data (SecondActivity.java)**  
```java
Bundle bundle = getIntent().getExtras();
if (bundle != null) {
    String value = bundle.getString("key");
}
```

---

## **5. Managing State (Handling Configuration Changes)**  
Use `onSaveInstanceState()` to preserve **UI state** when an activity is **recreated** (e.g., screen rotation).  

#### **Example:**  
```java
@Override
protected void onSaveInstanceState(Bundle outState) {
    super.onSaveInstanceState(outState);
    outState.putString("key", "value");
}

@Override
protected void onRestoreInstanceState(Bundle savedInstanceState) {
    super.onRestoreInstanceState(savedInstanceState);
    String value = savedInstanceState.getString("key");
}
```

---

## **6. Activities with Implicit Intents**  
### **Example - Share Text via Other Apps (e.g., Messaging, Email, etc.):**  
```java
Intent intent = new Intent(Intent.ACTION_SEND);
intent.setType("text/plain");
intent.putExtra(Intent.EXTRA_TEXT, "This is a sample text.");
startActivity(Intent.createChooser(intent, "Share via"));
```
- **`Intent.createChooser()`** prompts the user to select an app to handle the action.  

---

## **Summary**  
✔ **Activities** define individual screens in an app.  
✔ **Explicit Intents** navigate between activities within the same app.  
✔ **Implicit Intents** interact with external apps (e.g., open a browser, call a number).  
✔ **Lifecycle methods** help manage state and resources efficiently.  
✔ **Data can be passed** between activities using `Intent.putExtra()` and `Bundle`.  
✔ **Implicit intents** allow users to share content or interact with system apps.  
