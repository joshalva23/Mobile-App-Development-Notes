# **Activities and Implicit Intents in Android**

Activities and intents are fundamental components of Android development, allowing users to navigate between screens and interact with different parts of the system or other applications.

---

## **1. Activities in Android**
An **Activity** represents a **single screen** in an Android app. It provides a user interface and handles user interactions.

### **Example of a Basic Activity**
```java
import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main); // Set UI layout
        Toast.makeText(this, "Activity Created", Toast.LENGTH_SHORT).show();
    }
}
```

---

## **2. Navigating Between Activities**
### **Explicit Intent (Navigating to a Specific Activity)**
An **explicit intent** specifies the **exact** activity to open.

#### **Example: Opening Another Activity**
```java
Intent intent = new Intent(MainActivity.this, SecondActivity.class);
startActivity(intent);
```

#### **Example: Passing Data Between Activities**
```java
Intent intent = new Intent(MainActivity.this, SecondActivity.class);
intent.putExtra("USERNAME", "JohnDoe");
startActivity(intent);
```

#### **Receiving Data in SecondActivity**
```java
String username = getIntent().getStringExtra("USERNAME");
textView.setText(username);
```

---

## **3. Implicit Intents**
An **implicit intent** does not specify a particular activity but instead requests an action, allowing other apps to handle it.

### **Common Use Cases for Implicit Intents**
| **Intent Action** | **Purpose** |
|------------------|------------|
| `Intent.ACTION_VIEW` | Open a URL in a web browser |
| `Intent.ACTION_DIAL` | Open the dialer with a phone number |
| `Intent.ACTION_SEND` | Share content via email, social media, etc. |
| `Intent.ACTION_PICK` | Pick a contact, image, or file from storage |
| `Intent.ACTION_CAMERA_BUTTON` | Open the camera |

---

### **Example 1: Opening a Web Page**
```java
Intent intent = new Intent(Intent.ACTION_VIEW);
intent.setData(Uri.parse("https://www.google.com"));
startActivity(intent);
```

### **Example 2: Making a Phone Call**
```java
Intent intent = new Intent(Intent.ACTION_DIAL);
intent.setData(Uri.parse("tel:+1234567890"));
startActivity(intent);
```

### **Example 3: Sending an Email**
```java
Intent intent = new Intent(Intent.ACTION_SEND);
intent.setType("text/plain");
intent.putExtra(Intent.EXTRA_EMAIL, new String[]{"example@example.com"});
intent.putExtra(Intent.EXTRA_SUBJECT, "Hello");
intent.putExtra(Intent.EXTRA_TEXT, "This is a test email.");
startActivity(Intent.createChooser(intent, "Send Email"));
```

### **Example 4: Capturing a Photo**
```java
Intent intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
startActivity(intent);
```

---

## **4. Handling Implicit Intents in Your App**
If you want your app to **respond** to an implicit intent, declare an **intent filter** in the `AndroidManifest.xml` file.

### **Example: Handling Web Links**
```xml
<activity android:name=".WebActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <category android:name="android.intent.category.BROWSABLE"/>
        <data android:scheme="https" android:host="www.example.com"/>
    </intent-filter>
</activity>
```
This allows the app to open `https://www.example.com` links.

---

## **5. Checking If an App Can Handle an Intent**
Before starting an implicit intent, check if there’s an app to handle it:
```java
if (intent.resolveActivity(getPackageManager()) != null) {
    startActivity(intent);
} else {
    Toast.makeText(this, "No app found to handle this request", Toast.LENGTH_SHORT).show();
}
```

---

## **6. Summary**
- **Activities** are the building blocks of an Android app's UI.
- **Explicit intents** are used to navigate between activities within the same app.
- **Implicit intents** allow apps to request actions without specifying a target activity.
- **Intent filters** define how an app responds to implicit intents.
- **Checking for available apps** prevents crashes when using implicit intents.