
# **Communicating with SMS and Emails in Android**

This section covers how to send SMS messages and emails from an Android application. Communication via SMS and email is essential for many applications, whether for notifications, reminders, or user interaction.

### **1. Sending SMS Messages**

In Android, you can send SMS messages using the **SmsManager** class. Alternatively, you can also open the default SMS app with pre-filled data to allow users to send SMS manually.

#### **a) Sending SMS using SmsManager**

1. **Add the required permission** in your `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.SEND_SMS" />
```

2. **Send SMS using SmsManager**:

```java
import android.telephony.SmsManager;

public void sendSMS(String phoneNumber, String message) {
    SmsManager smsManager = SmsManager.getDefault();
    smsManager.sendTextMessage(phoneNumber, null, message, null, null);
}
```

3. **Handling SMS Sending Result**

To handle the result of the SMS sending process, you can use `PendingIntent` and `BroadcastReceiver`.

```java
public void sendSMSWithResult(String phoneNumber, String message) {
    SmsManager smsManager = SmsManager.getDefault();
    PendingIntent sentIntent = PendingIntent.getBroadcast(this, 0, new Intent("SMS_SENT"), 0);
    PendingIntent deliveredIntent = PendingIntent.getBroadcast(this, 0, new Intent("SMS_DELIVERED"), 0);

    smsManager.sendTextMessage(phoneNumber, null, message, sentIntent, deliveredIntent);
}
```

- **Broadcast Receivers for SMS Sent and Delivered**

In your `BroadcastReceiver`:

```java
public class SmsReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        switch (getResultCode()) {
            case Activity.RESULT_OK:
                Toast.makeText(context, "SMS sent", Toast.LENGTH_SHORT).show();
                break;
            case SmsManager.RESULT_ERROR_GENERIC_FAILURE:
                Toast.makeText(context, "Error sending SMS", Toast.LENGTH_SHORT).show();
                break;
        }
    }
}
```

- **Explanation**: You can send an SMS directly through `SmsManager` or open the default SMS app with prefilled data by using `Intent.ACTION_VIEW` with the `sms:` URI.

#### **b) Opening Default SMS App with Pre-filled Data**

```java
Intent sendIntent = new Intent(Intent.ACTION_VIEW, Uri.parse("sms:" + phoneNumber));
sendIntent.putExtra("sms_body", message);
startActivity(sendIntent);
```

- **Explanation**: This approach opens the user's SMS app with the provided phone number and message pre-filled, allowing the user to review and send it manually.

---

### **2. Sending Emails**

Sending emails in Android can be done using **Intent** to launch the user's email app with pre-filled data. Alternatively, you can use external libraries to send emails directly without the need for user interaction.

#### **a) Sending Email using Intent**

1. **Create an Intent to launch the email app**:

```java
Intent emailIntent = new Intent(Intent.ACTION_SEND);
emailIntent.putExtra(Intent.EXTRA_EMAIL, new String[] {"recipient@example.com"});
emailIntent.putExtra(Intent.EXTRA_SUBJECT, "Subject of the email");
emailIntent.putExtra(Intent.EXTRA_TEXT, "Body of the email");

emailIntent.setType("message/rfc822"); // Specify email mime type
startActivity(Intent.createChooser(emailIntent, "Send Email"));
```

- **Explanation**: This approach launches the default email app, pre-fills the recipient, subject, and body, and allows the user to send the email manually.

#### **b) Sending Email Programmatically**

To send email programmatically, without user interaction, you can use external libraries such as **JavaMail API**. Here’s an example using JavaMail:

1. **Add the necessary dependency to your `build.gradle` file**:

```gradle
dependencies {
    implementation 'com.sun.mail:android-mail:1.6.0'
    implementation 'com.sun.mail:android-activation:1.6.0'
}
```

2. **Use JavaMail API to send an email**:

```java
import javax.mail.*;
import javax.mail.internet.*;
import java.util.Properties;

public void sendEmail() {
    String host = "smtp.gmail.com";
    String user = "your-email@gmail.com";
    String pass = "your-password";
    String to = "recipient@example.com";
    String subject = "Subject";
    String messageText = "Body of the email";

    Properties properties = System.getProperties();
    properties.put("mail.smtp.host", host);
    properties.put("mail.smtp.auth", "true");
    properties.put("mail.smtp.port", "587");
    properties.put("mail.smtp.starttls.enable", "true");

    Session session = Session.getInstance(properties, new javax.mail.Authenticator() {
        protected PasswordAuthentication getPasswordAuthentication() {
            return new PasswordAuthentication(user, pass);
        }
    });

    try {
        MimeMessage message = new MimeMessage(session);
        message.setFrom(new InternetAddress(user));
        message.addRecipient(Message.RecipientType.TO, new InternetAddress(to));
        message.setSubject(subject);
        message.setText(messageText);

        Transport.send(message);
        Toast.makeText(this, "Email Sent", Toast.LENGTH_SHORT).show();
    } catch (MessagingException e) {
        e.printStackTrace();
        Toast.makeText(this, "Error Sending Email", Toast.LENGTH_SHORT).show();
    }
}
```

- **Explanation**: The **JavaMail API** enables you to send emails programmatically by configuring the SMTP server, authentication credentials, and other email settings.

---

### **3. Conclusion**

- **Sending SMS**: You can use the `SmsManager` class for direct SMS sending or an `Intent` to open the default SMS app with prefilled data. To receive results, you can use a `BroadcastReceiver`.
  
- **Sending Emails**: The easiest way to send an email is by using an `Intent` to open the email app. If you need to send emails programmatically, external libraries such as **JavaMail** can be used to send emails without user interaction.

By using these methods, your Android application can integrate both SMS and email communication, allowing for better user interaction and notification handling.