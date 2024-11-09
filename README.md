# DuneWebView

🚀 A modern, secure, and feature-rich WebView component for Android that enhances the browsing experience with built-in protection and advanced customization options.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Platform](https://img.shields.io/badge/platform-Android-green.svg)
![API](https://img.shields.io/badge/API-21%2B-brightgreen.svg)

## ✨ Key Features

- 🛡️ **Security First**
  - Ad blocking with customizable rules
  - Popup blocking and redirect protection
  - Overlay detection and removal
  - Safe browsing environment

- 📱 **Enhanced User Experience**
  - Smart download management
  - Progress tracking
  - Gesture support with SwipeRefreshLayout
  - URL processing and search handling
  - Navigation controls

- ⚙️ **Developer Friendly**
  - Highly customizable
  - Easy integration
  - Extensive configuration options
  - Both Java and Kotlin support

## 📦 Installation

### Step 1: Download the Module
1. Download the DuneWeb example project
2. Extract the `duneweb` module from the root folder

### Step 2: Add Module to Your Project
1. Open your project in Android Studio
2. Go to File → New → Import Module
3. Navigate to the extracted `duneweb` module directory
4. Click 'Finish' to import the module

### Step 3: Add Module Dependency
Add the following to your app's `build.gradle`:

```gradle
dependencies {
    implementation project(':duneweb')
}
```

### Step 4: Sync Project
Click 'Sync Now' in the Gradle notification bar or go to File → Sync Project with Gradle Files

## 🎯 Basic Usage

### XML Layout

```xml
<androidx.swiperefreshlayout.widget.SwipeRefreshLayout
    android:id="@+id/swipeRefreshLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <com.levelpixel.DuneWebView
        android:id="@+id/duneWebView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</androidx.swiperefreshlayout.widget.SwipeRefreshLayout>
```

### Basic Implementation (Java)

```java
public class MainActivity extends AppCompatActivity {
    private DuneWebView duneWebView;
    private ProgressBar progressBar;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        duneWebView = findViewById(R.id.duneWebView);
        progressBar = findViewById(R.id.progressBar);
        
        // Configure basic security features
        duneWebView.setAdBlockEnabled(true);  // Block ads and trackers
        duneWebView.setPopupBlockEnabled(true); // Block popups
        duneWebView.setRedirectBlockEnabled(true); // Block unwanted redirects
        duneWebView.setUseSystemDownloader(true); // Use system download manager
        
        // Load default URL
        duneWebView.loadUrl("https://www.google.com");
        
        // Set up progress tracking
        duneWebView.setProgressListener(progress -> {
            progressBar.setVisibility(View.VISIBLE);
            progressBar.setProgress(progress);
            if (progress == 100) {
                progressBar.setVisibility(View.INVISIBLE);
            }
        });
    }
}
```

## 🛠️ Advanced Configuration

### Ad Blocking and Security

```java
// Load default ad blocklist
duneWebView.loadAdBlockListFromResource(true, null);

// Add custom blocked domains
duneWebView.addCustomBlockedDomain("ads.example.com");
duneWebView.addCustomBlockedDomain("trackers.example.com");

// Remove specific domain from blocklist
duneWebView.removeBlockedDomain("ads.example.com");

// Clear entire blocklist
duneWebView.clearBlocklist();

// Check if domain is blocked
boolean isBlocked = duneWebView.isBlockedDomain("facebook.com");

// Get total number of blocked hosts
int blockedHostsSize = duneWebView.getBlocklistSize();
```

### SwipeRefresh Implementation

```java
private void setupSwipeRefresh() {
    SwipeRefreshLayout swipeRefreshLayout = findViewById(R.id.swipeRefreshLayout);
    
    swipeRefreshLayout.setOnRefreshListener(() -> {
        duneWebView.reload();
        new Handler().postDelayed(() -> 
            swipeRefreshLayout.setRefreshing(false), 500);
    });

    // Set custom colors for refresh indicator
    swipeRefreshLayout.setColorSchemeColors(
        getColorFromAttr(this, R.attr.colorPrimary),
        getColorFromAttr(this, R.attr.colorSecondary)
    );
}
```

### Navigation Controls

```java
// Back navigation
backButton.setOnClickListener(v -> {
    if (duneWebView.canGoBack()) {
        duneWebView.goBack();
    } else {
        duneWebView.loadUrl(DEFAULT_URL);
    }
});

// Forward navigation
forwardButton.setOnClickListener(v -> {
    if (duneWebView.canGoForward()) {
        duneWebView.goForward();
    }
});

// Refresh page
refreshButton.setOnClickListener(v -> duneWebView.reload());
```

### URL Processing

```java
private String processInput(String input) {
    String cleanInput = input.trim().toLowerCase();

    // Check for valid protocol
    if (hasValidProtocol(cleanInput)) {
        return input;
    }

    // Check if input looks like URL
    if (looksLikeUrl(cleanInput)) {
        return "https://" + input;
    }

    // Convert to search query
    return buildSearchUrl(input);
}

private String buildSearchUrl(String query) {
    try {
        return "https://www.google.com/search?q=" + 
            URLEncoder.encode(query, "UTF-8");
    } catch (UnsupportedEncodingException e) {
        return "https://www.google.com/search?q=" + 
            query.replace(" ", "+");
    }
}
```

## 📱 Required Permissions

Add these to your `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" 
    android:maxSdkVersion="28"/>
<uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />
```

## 🔧 Customization Options

| Feature | Method | Description |
|---------|---------|-------------|
| Ad Blocking | `setAdBlockEnabled(Boolean)` | Enable/disable ad blocking |
| Popup Blocking | `setPopupBlockEnabled(Boolean)` | Enable/disable popup blocking |
| Redirect Protection | `setRedirectBlockEnabled(Boolean)` | Enable/disable redirect protection |
| Download Handler | `setUseSystemDownloader(Boolean)` | Toggle system download manager |
| Progress Tracking | `setProgressListener(listener)` | Set progress callback |
| Blocklist Management | `addCustomBlockedDomain(String)` | Add custom domain to blocklist |
| Blocklist Clearing | `clearBlocklist()` | Clear entire blocklist |
| Domain Check | `isBlockedDomain(String)` | Check if domain is blocked |

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👨‍💻 Author

Created by LevelPixel, Tanvir Ahamed

## 📞 Support

For support, please open an issue in the repository.

---

⭐ Don't forget to star the repo if you find it useful!
