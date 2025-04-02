
# Screen Navigation in Android  

Screen navigation refers to the process of moving between different **activities** or **fragments** in an Android application. The user interacts with the app in various ways, and as a result, navigation between screens becomes an essential part of the app's user experience. Android provides multiple methods for screen navigation, such as **Activity navigation, Fragment transactions, and Navigation Component**.  

---

## 1. Activity Navigation  

In Android, an **Activity** represents a single screen with a user interface. To navigate from one activity to another, we use **Intents**.  

### **Starting a New Activity**  

To start a new activity, you create an **Intent** object, which specifies the current activity and the target activity to be launched.  

### **Example - Starting a New Activity:**  

#### **MainActivity (Current Activity):**  
```java
Button navigateButton = findViewById(R.id.navigate_button); // Assume a Button is in the layout
navigateButton.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        Intent intent = new Intent(MainActivity.this, SecondActivity.class);
        startActivity(intent);
    }
});
```

#### **SecondActivity (Target Activity):**  
```java
public class SecondActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);  // Layout for the second activity
    }
}
```

---

### **Passing Data Between Activities**  

You can also pass data between activities using **Intent extras**. This allows you to send information (such as strings, integers, or custom objects) to the target activity.  

#### **Passing Data:**  
```java
Intent intent = new Intent(MainActivity.this, SecondActivity.class);
intent.putExtra("message", "Hello from MainActivity!");
startActivity(intent);
```

#### **Retrieving Data in SecondActivity:**  
```java
String message = getIntent().getStringExtra("message");
Toast.makeText(this, message, Toast.LENGTH_SHORT).show();
```

---

## 2. Fragment Navigation  

A **Fragment** is a reusable UI component that can be added to or replaced within an activity. Fragment navigation refers to **adding, replacing, or removing fragments** within the activity.  

### **Adding a Fragment**  

You can add a fragment to an activity dynamically using `FragmentTransaction`.  

### **Example - Adding a Fragment:**  

#### **MainActivity (Host Activity):**  
```java
Fragment fragment = new MyFragment();
FragmentTransaction transaction = getSupportFragmentManager().beginTransaction();
transaction.replace(R.id.fragment_container, fragment); // Replace the container with the fragment
transaction.addToBackStack(null); // Optional: Add to back stack to allow back navigation
transaction.commit();
```

#### **MyFragment (Fragment):**  
```java
public class MyFragment extends Fragment {
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        return inflater.inflate(R.layout.fragment_my, container, false);
    }
}
```

---

### **Fragment Navigation with Back Stack**  

By adding fragments to the **back stack**, you allow the user to navigate back to the previous fragment or activity by pressing the **back button**.  

```java
transaction.addToBackStack(null); // Ensures that the fragment is placed in the back stack
```

---

## 3. Navigation Component (Jetpack)  

The **Navigation Component** is a part of Android’s **Jetpack** libraries and provides a more structured way to handle navigation between activities and fragments. It simplifies the process of navigating and passing data between screens.  

### **Setting Up Navigation Component**  

#### **Add Dependencies in `build.gradle`:**  
```gradle
implementation "androidx.navigation:navigation-fragment-ktx:2.5.0"
implementation "androidx.navigation:navigation-ui-ktx:2.5.0"
```

#### **Create a Navigation Graph (`res/navigation/nav_graph.xml`):**  
```xml
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/nav_graph"
    android:label="app_name"
    app:startDestination="@id/firstFragment">

    <fragment
        android:id="@+id/firstFragment"
        android:name="com.example.myapp.FirstFragment"
        android:label="First Fragment">
        <action
            android:id="@+id/action_firstFragment_to_secondFragment"
            app:destination="@id/secondFragment" />
    </fragment>

    <fragment
        android:id="@+id/secondFragment"
        android:name="com.example.myapp.SecondFragment"
        android:label="Second Fragment" />
</navigation>
```

#### **Navigate Between Fragments (`FirstFragment.java`):**  
```java
NavController navController = Navigation.findNavController(view);
navController.navigate(R.id.action_firstFragment_to_secondFragment);
```

---

### **Advantages of Navigation Component**  

- ✅ **Centralized Navigation Logic**: Navigation actions are centralized in the navigation graph.  
- ✅ **Back Stack Management**: Automatically handles fragment transactions and back stack management.  
- ✅ **Type-Safe**: Ensures safe navigation using IDs and actions.  

---

## 4. Intent Flags for Special Navigation  

You can modify how activities are launched using **Intent Flags**. These flags allow you to **customize the behavior** of how activities are started, especially in cases where you want to clear the activity stack or handle specific behaviors on activity launch.  

### **Common Intent Flags:**  

- `Intent.FLAG_ACTIVITY_NEW_TASK`: Starts a new task, or if the activity is already running in the current task, brings it to the foreground.  
- `Intent.FLAG_ACTIVITY_CLEAR_TOP`: Clears all activities above the target activity in the stack.  

### **Example - Using Intent Flags:**  
```java
Intent intent = new Intent(MainActivity.this, SecondActivity.class);
intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP | Intent.FLAG_ACTIVITY_NEW_TASK);
startActivity(intent);
```

This example **clears all activities above SecondActivity** in the stack and starts a **new task**.  

