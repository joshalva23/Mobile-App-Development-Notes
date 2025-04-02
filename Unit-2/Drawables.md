# **Drawables in Android**  

Drawables are **resource files** that define the **visual appearance** of UI elements in Android. They are used for **backgrounds, icons, buttons, and other UI components**. Drawables can be **static images, shapes, gradients, or even animations**.  

---

## **Types of Drawables**  

- **Bitmap Drawable**: Displays an image resource like **.png** or **.jpg**.  
- **Shape Drawable**: Used to create **shapes** like rectangles, ovals, or custom gradients.  
- **Layer Drawable**: Combines **multiple drawables** into a single output.  
- **State List Drawable**: Changes appearance based on the **widget's state** (e.g., pressed, focused).  
- **Animation Drawable**: Defines **frame-by-frame animations**.  

---

## **Examples of Drawables**  

### **1. Shape Drawable (XML-based Shapes)**  

To create a **colored rounded rectangle**:  

📂 **File:** `res/drawable/rounded_button.xml`  

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <solid android:color="#6200EE" />
    <corners android:radius="16dp" />
    <stroke
        android:width="2dp"
        android:color="#3700B3" />
</shape>
```

#### **Usage in Layout XML**  

```xml
<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Rounded Button"
    android:background="@drawable/rounded_button"
    android:padding="16dp"
    android:textColor="@android:color/white" />
```

---

### **2. State List Drawable (Change Button Appearance on Press)**  

To create a button with **different colors** for **pressed and normal states**:  

📂 **File:** `res/drawable/button_states.xml`  

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_pressed="true" android:drawable="@color/pressed_color" />
    <item android:drawable="@color/default_color" />
</selector>
```

#### **Usage in Layout XML**  

```xml
<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="State Button"
    android:background="@drawable/button_states"
    android:padding="16dp"
    android:textColor="@android:color/white" />
```

📂 **File:** `res/values/colors.xml`  

```xml
<color name="pressed_color">#FF6200EE</color>
<color name="default_color">#6200EE</color>
```

---

### **3. Layer Drawable (Combining Multiple Drawables)**  

To create a **layered background** with an **image and a color overlay**:  

📂 **File:** `res/drawable/layered_background.xml`  

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item>
        <color android:color="#6200EE" />
    </item>
    <item>
        <bitmap
            android:src="@drawable/sample_image"
            android:gravity="center" />
    </item>
</layer-list>
```

#### **Usage in Layout XML**  

```xml
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@drawable/layered_background"
    android:orientation="vertical">
</LinearLayout>
```

---

### **4. Bitmap Drawable (Using an Image)**  

To display an **image as a drawable**:  

📂 **File:** `res/drawable/sample_bitmap.xml`  

```xml
<?xml version="1.0" encoding="utf-8"?>
<bitmap xmlns:android="http://schemas.android.com/apk/res/android"
    android:src="@drawable/sample_image"
    android:gravity="center" />
```

#### **Usage in Layout XML**  

```xml
<ImageView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:src="@drawable/sample_bitmap" />
```

---

### **5. Animation Drawable (Frame-by-Frame Animation)**  

To create an **animation using multiple images**:  

📂 **File:** `res/drawable/animation_drawable.xml`  

```xml
<?xml version="1.0" encoding="utf-8"?>
<animation-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/frame1" android:duration="100" />
    <item android:drawable="@drawable/frame2" android:duration="100" />
    <item android:drawable="@drawable/frame3" android:duration="100" />
</animation-list>
```

#### **Usage in Java Code**  

📂 **File:** `MainActivity.java`  

```java
import android.graphics.drawable.AnimationDrawable;
import android.os.Bundle;
import android.widget.ImageView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        ImageView imageView = findViewById(R.id.animatedImageView);
        imageView.setBackgroundResource(R.drawable.animation_drawable);

        AnimationDrawable animationDrawable = (AnimationDrawable) imageView.getBackground();
        animationDrawable.start();
    }
}
```

#### **Layout XML (`res/layout/activity_main.xml`)**  

```xml
<ImageView
    android:id="@+id/animatedImageView"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_centerInParent="true" />
```
