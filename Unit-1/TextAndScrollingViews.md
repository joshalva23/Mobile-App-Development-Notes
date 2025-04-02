# **Text and Scrolling Views in Android**  

This guide focuses on how to **display text** and **handle long content** that requires scrolling in Android applications.  

---

## **1. TextView**  
`TextView` is a basic UI element used to **display text** on the screen. It supports **plain text, styled text, and HTML-formatted content**.  

### **Basic TextView Example:**  
```xml
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Welcome to the Android App!"
    android:textSize="18sp"
    android:textColor="#000000" />
```

### **Attributes:**  
- `android:text`: Sets the text content.  
- `android:textSize`: Defines the size of the text.  
- `android:textColor`: Changes the text color.  
- `android:gravity`: Aligns the text (e.g., `center`, `left`).  
- `android:maxLines`: Limits the number of lines displayed.  

---

### **Styled Text Example:**  
```xml
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Hello, <b>World!</b>"
    android:textColor="#FF5722"
    android:lineSpacingExtra="4dp"
    android:autoLink="web"
    android:textStyle="italic" />
```
- `<b>` makes text **bold**.  
- `android:lineSpacingExtra`: Adds extra spacing between lines.  
- `android:autoLink="web"`: Automatically links web URLs.  

---

## **2. EditText**  
`EditText` allows users to **input and edit text**. It extends `TextView` with **interactive functionality**.  

### **Basic EditText Example:**  
```xml
<EditText
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:hint="Enter your name"
    android:inputType="textPersonName"
    android:maxLength="20" />
```

### **Attributes:**  
- `android:hint`: Placeholder text.  
- `android:inputType`: Defines the type of input (`text`, `number`, `password`, etc.).  
- `android:maxLength`: Limits the number of characters.  
- `android:lines`: Sets the number of visible lines.  

---

## **3. ScrollView**  
`ScrollView` is a layout that allows **vertical scrolling** for content that exceeds the screen size.  

### **Basic ScrollView Example:**  
```xml
<ScrollView
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="This is a long text that requires scrolling."
            android:padding="16dp" />

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="More content here."
            android:padding="16dp" />
    </LinearLayout>
</ScrollView>
```

### **Attributes:**  
- `android:fillViewport="true"`: Forces content to fill the `ScrollView`.  

---

## **4. NestedScrollView**  
`NestedScrollView` is an advanced version of `ScrollView` that supports **nested scrolling**. It is useful when working with components like `RecyclerView`.  

```xml
<androidx.core.widget.NestedScrollView
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">
        
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Nested scrolling example." />
        
    </LinearLayout>

</androidx.core.widget.NestedScrollView>
```

---

## **5. Combining Text and Scrolling Views**  
Here’s a **complete example** that combines `TextView`, `EditText`, and `ScrollView` to create a **form-like interface**.  

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
            android:text="User Information"
            android:textSize="20sp"
            android:textStyle="bold"
            android:paddingBottom="8dp" />

        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Enter your name"
            android:inputType="textPersonName" />

        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Enter your email"
            android:inputType="textEmailAddress"
            android:layout_marginTop="8dp" />

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Comments"
            android:layout_marginTop="16dp" />

        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Write your comments here"
            android:inputType="textMultiLine"
            android:minLines="5"
            android:gravity="top"
            android:background="#DDDDDD"
            android:padding="8dp" />

    </LinearLayout>
</ScrollView>
```

---

## **6. Adding Scrolling Behavior Programmatically**  
You can programmatically scroll to **the bottom** of a `ScrollView`.  

### **Java Example:**  
```java
ScrollView scrollView = findViewById(R.id.scrollView);
scrollView.post(() -> scrollView.fullScroll(View.FOCUS_DOWN));
```

