# **UI Design: Building a Layout with UI Elements**  

This subtopic focuses on designing layouts and incorporating UI elements in an **Android app** using **XML** and **Android Studio**. You will learn how to create layouts, add widgets, and customize them effectively.  

---

## **1. Understanding Layouts with Code Examples**  

### **LinearLayout Example**  
- Arranges UI elements in a **linear direction** (**horizontal** or **vertical**).  

```xml
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Welcome to My App"
        android:textSize="24sp"
        android:textColor="#000000"
        android:layout_gravity="center" />

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Click Me"
        android:onClick="onButtonClick" />
</LinearLayout>
```

---

### **ConstraintLayout Example**  
- Allows **precise positioning** of UI elements using **constraints**.  

```xml
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello, ConstraintLayout!"
        android:textSize="18sp"
        android:layout_marginTop="32dp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Press Me"
        app:layout_constraintTop_toBottomOf="@id/textView"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

---

### **ScrollView Example**  
- Used to **enable scrolling** for content that **exceeds** the screen size.  

```xml
<ScrollView
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="This is a very long text that demonstrates scrolling..."
            android:textSize="16sp"
            android:padding="8dp" />

        <!-- Add more elements as needed -->

    </LinearLayout>
</ScrollView>
```

---

## **2. Adding Common UI Elements**  

### **TextView Example**  
- Displays **static or dynamic** text.  

```xml
<TextView
    android:id="@+id/welcomeText"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Welcome to Android Development!"
    android:textSize="20sp"
    android:textStyle="bold"
    android:padding="8dp" />
```

---

### **EditText Example**  
- Accepts **user input**.  

```xml
<EditText
    android:id="@+id/inputField"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:hint="Enter your name"
    android:inputType="textPersonName"
    android:padding="8dp" />
```

---

### **Button Example**  
- Triggers **actions** when clicked.  

```xml
<Button
    android:id="@+id/submitButton"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Submit"
    android:onClick="onSubmitButtonClick" />
```

---

### **ImageView Example**  
- Displays an **image**.  

```xml
<ImageView
    android:id="@+id/sampleImage"
    android:layout_width="200dp"
    android:layout_height="200dp"
    android:src="@drawable/sample_image"
    android:contentDescription="Sample image description"
    android:layout_gravity="center" />
```

---

## **3. Event Handling in Activity**  

### **Java Code for Button Click Handling**  

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    // Method to handle button click
    public void onSubmitButtonClick(View view) {
        Toast.makeText(this, "Button Clicked!", Toast.LENGTH_SHORT).show();
    }
}
```

---

## **4. Organizing Resources**  

### **Strings in `res/values/strings.xml`**  

```xml
<resources>
    <string name="app_name">My App</string>
    <string name="welcome_message">Welcome to My App</string>
    <string name="button_label">Click Me</string>
</resources>
```

---

### **Colors in `res/values/colors.xml`**  

```xml
<resources>
    <color name="primaryColor">#6200EE</color>
    <color name="textColor">#000000</color>
</resources>
```

---

### **Images in `res/drawable/`**  
- Place **image files** (e.g., `logo.png`) in the `drawable` folder and reference them using `@drawable/logo`.  

---

## **5. Full Example Layout**  

```xml
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp">

    <TextView
        android:id="@+id/welcomeText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Welcome to My App"
        android:textSize="24sp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        android:layout_marginTop="16dp" />

    <EditText
        android:id="@+id/inputField"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:hint="Enter your name"
        android:padding="8dp"
        app:layout_constraintTop_toBottomOf="@id/welcomeText"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        android:layout_marginTop="16dp" />

    <Button
        android:id="@+id/submitButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Submit"
        app:layout_constraintTop_toBottomOf="@id/inputField"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        android:layout_marginTop="16dp" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

