
# **Firebase and AdMob in Android**

Firebase and AdMob are powerful tools provided by Google to enhance the functionality and monetization of Android applications. Firebase offers a suite of services like real-time databases, authentication, and analytics, while AdMob enables app developers to monetize their applications through ads.

This guide will cover how to integrate Firebase and AdMob into your Android app and explain how to use their key features.

---

### **1. Firebase Integration**

Firebase is a comprehensive suite of cloud-based tools that helps you manage your app's backend, analytics, authentication, and real-time databases. Here's how to get started with Firebase in your Android project.

#### **a) Setting Up Firebase in Your Android Project**

1. **Add Firebase to Your Android Project**:
    - Go to the Firebase Console (https://console.firebase.google.com/).
    - Create a new project or select an existing one.
    - Add your Android app by providing your app's package name.
    - Download the `google-services.json` file and add it to your app's `app/` directory.
    
2. **Add Firebase SDK dependencies**:
    - Open the `build.gradle` (project-level) file and add the following classpath to the `dependencies` section:
    ```gradle
    classpath 'com.google.gms:google-services:4.3.10'
    ```
    - In the `build.gradle` (app-level) file, apply the following plugin and add necessary dependencies:
    ```gradle
    apply plugin: 'com.google.gms.google-services'
    
    dependencies {
        implementation 'com.google.firebase:firebase-analytics:21.0.0'
        // Add other Firebase dependencies as needed
    }
    ```

3. **Initialize Firebase**:
    - In your app’s `MainActivity` or `Application` class, initialize Firebase:
    ```java
    import com.google.firebase.analytics.FirebaseAnalytics;

    public class MainActivity extends AppCompatActivity {
        private FirebaseAnalytics mFirebaseAnalytics;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            // Initialize Firebase Analytics
            mFirebaseAnalytics = FirebaseAnalytics.getInstance(this);
        }
    }
    ```

#### **b) Firebase Authentication**

Firebase Authentication provides backend services to help authenticate users, including simple pass-through methods for email/password, phone authentication, and even third-party sign-ins like Google and Facebook.

1. **Set Up Email/Password Authentication**:
    - Add authentication dependency:
    ```gradle
    implementation 'com.google.firebase:firebase-auth:21.0.1'
    ```
    - Example for Email and Password Authentication:
    ```java
    FirebaseAuth mAuth = FirebaseAuth.getInstance();

    // Register a new user
    mAuth.createUserWithEmailAndPassword(email, password)
        .addOnCompleteListener(this, task -> {
            if (task.isSuccessful()) {
                FirebaseUser user = mAuth.getCurrentUser();
                // User is signed in
            } else {
                // Handle failure
            }
        });

    // Sign in with email and password
    mAuth.signInWithEmailAndPassword(email, password)
        .addOnCompleteListener(this, task -> {
            if (task.isSuccessful()) {
                FirebaseUser user = mAuth.getCurrentUser();
                // User signed in
            }
        });
    ```

#### **c) Firebase Firestore/Realtime Database**

Firebase Firestore and Realtime Database are cloud-hosted databases that allow real-time synchronization of data. Both offer similar features but differ in use cases and data structuring.

1. **Firestore Example**:
    - Add Firestore dependency:
    ```gradle
    implementation 'com.google.firebase:firebase-firestore:24.0.0'
    ```
    - Example to store and retrieve data from Firestore:
    ```java
    FirebaseFirestore db = FirebaseFirestore.getInstance();

    // Create a new document
    Map<String, Object> user = new HashMap<>();
    user.put("first", "Ada");
    user.put("last", "Lovelace");

    db.collection("users").document("user_id")
        .set(user)
        .addOnSuccessListener(aVoid -> Log.d("Firestore", "DocumentSnapshot successfully written!"))
        .addOnFailureListener(e -> Log.w("Firestore", "Error writing document", e));
    
    // Retrieve data from Firestore
    db.collection("users").document("user_id")
        .get()
        .addOnSuccessListener(documentSnapshot -> {
            if (documentSnapshot.exists()) {
                Log.d("Firestore", "DocumentSnapshot data: " + documentSnapshot.getData());
            } else {
                Log.d("Firestore", "No such document");
            }
        });
    ```

---

### **2. AdMob Integration**

AdMob is Google’s mobile advertising platform that allows developers to monetize their apps by displaying ads. You can use AdMob to serve banner ads, interstitial ads, and rewarded ads.

#### **a) Setting Up AdMob in Your Android Project**

1. **Add AdMob to Your Project**:
    - Go to the AdMob website (https://admob.google.com/) and create an account or sign in.
    - Add your app and get the **App ID** and **Ad Unit IDs** for the ads you want to display (banner, interstitial, etc.).
    - Add the `google-services.json` file you get from AdMob to your app's `app/` directory.
    
2. **Add AdMob SDK Dependencies**:
    - In your `build.gradle` (project-level), make sure the Google services classpath is added:
    ```gradle
    classpath 'com.google.gms:google-services:4.3.10'
    ```
    - In the `build.gradle` (app-level) file, add the following AdMob SDK dependency:
    ```gradle
    apply plugin: 'com.google.gms.google-services'

    dependencies {
        implementation 'com.google.android.gms:play-services-ads:21.0.0'
    }
    ```

3. **Initialize AdMob**:
    - Initialize AdMob in the `MainActivity` or `Application` class:
    ```java
    import com.google.android.gms.ads.MobileAds;

    public class MainActivity extends AppCompatActivity {
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            // Initialize AdMob with your app ID
            MobileAds.initialize(this, initializationStatus -> {});
        }
    }
    ```

#### **b) Displaying Banner Ads**

1. **Add Banner Ad View in Layout**:
    - In your `activity_main.xml`, add a `AdView` element:
    ```xml
    <com.google.android.gms.ads.AdView
        android:id="@+id/adView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        ads:adUnitId="YOUR_AD_UNIT_ID"
        android:layout_alignParentBottom="true" />
    ```

2. **Load a Banner Ad**:
    - In your `MainActivity.java`, load the banner ad:
    ```java
    AdView mAdView = findViewById(R.id.adView);
    AdRequest adRequest = new AdRequest.Builder().build();
    mAdView.loadAd(adRequest);
    ```

#### **c) Displaying Interstitial Ads**

1. **Load Interstitial Ad**:
    - In your `MainActivity.java` or another activity, load the interstitial ad:
    ```java
    InterstitialAd interstitialAd = new InterstitialAd(this);
    interstitialAd.setAdUnitId("YOUR_AD_UNIT_ID");

    AdRequest adRequest = new AdRequest.Builder().build();
    interstitialAd.loadAd(adRequest);

    interstitialAd.setAdListener(new AdListener() {
        @Override
        public void onAdLoaded() {
            super.onAdLoaded();
            // Show interstitial ad
            if (interstitialAd.isLoaded()) {
                interstitialAd.show();
            }
        }
    });
    ```

#### **d) Displaying Rewarded Ads**

1. **Load Rewarded Ad**:
    ```java
    RewardedAd rewardedAd = new RewardedAd(this, "YOUR_AD_UNIT_ID");

    RewardedAdLoadCallback adLoadCallback = new RewardedAdLoadCallback() {
        @Override
        public void onAdLoaded() {
            // Ad loaded
        }

        @Override
        public void onAdFailedToLoad(LoadAdError adError) {
            // Handle the error
        }
    };

    rewardedAd.loadAd(new AdRequest.Builder().build(), adLoadCallback);
    ```

2. **Show Rewarded Ad**:
    ```java
    if (rewardedAd.isLoaded()) {
        rewardedAd.show(this, new OnUserEarnedRewardListener() {
            @Override
            public void onUserEarnedReward(@NonNull RewardItem rewardItem) {
                // Reward user
            }
        });
    }
    ```

---

### **3. Conclusion**

Integrating **Firebase** and **AdMob** into your Android application allows you to enhance app functionality and monetize your app effectively.

- **Firebase** provides services like authentication, cloud storage, and analytics, which are essential for modern Android apps.
- **AdMob** offers a wide range of ad formats, including banner ads, interstitial ads, and rewarded ads, enabling you to monetize your app while providing users with relevant content.

By following the steps outlined in this guide, you can quickly integrate these services into your app and start enhancing its functionality and revenue potential.
