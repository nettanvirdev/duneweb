# EnhancedWebView

A powerful and customizable WebView component for Android with built-in ad blocking, popup blocking, and download management capabilities.

## Features

- 🚫 Ad blocking with customizable blocklist
- 🔒 Popup blocking
- 🔄 Redirect protection
- ⬇️ Flexible download handling
- 📊 Progress tracking
- 🎯 Dynamic overlay blocking
- ⚡ Optimized performance
- 🛠️ Highly customizable

## Installation

1. Copy the `EnhancedWebView.java` file to your project's package directory.
2. Create a raw resource file `adblockserverlist` in `res/raw/` directory (optional, for ad blocking).

## Basic Usage

### XML Layout

Add the EnhancedWebView to your layout:

```xml
<com.youpackage.EnhancedWebView
    android:id="@+id/webView"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

### Activity/Fragment Setup

Basic implementation:

```java
EnhancedWebView webView = findViewById(R.id.webView);
webView.loadUrl("https://example.com");
```

## Advanced Configuration

### Enable/Disable Features

```java
// Configure security features
webView.setAdBlockEnabled(true);       // Enable ad blocking
webView.setPopupBlockEnabled(true);    // Enable popup blocking
webView.setRedirectBlockEnabled(true); // Enable redirect protection
webView.setUseSystemDownloader(true);  // Use system download manager
```

### Ad Blocking

```java
// Load blocklist from resource file
webView.loadAdBlockListFromResource(R.raw.adblockserverlist);

// Add custom domains to block
webView.addCustomBlockedDomain("ads.example.com");
webView.addCustomBlockedDomain("trackers.example.com");

// Remove domains from blocklist
webView.removeBlockedDomain("ads.example.com");

// Clear entire blocklist
webView.clearBlocklist();
```

### Progress Tracking

```java
webView.setProgressListener(progress -> {
    progressBar.setProgress(progress);
    if (progress == 100) {
        progressBar.setVisibility(View.GONE);
    }
});
```

### Custom Download Handling

```java
// Use custom download handler
webView.setCustomDownloadListener((url, userAgent, contentDisposition, mimeType, contentLength) -> {
    // Implement your custom download logic here
    // Example:
    DownloadManager.Request request = new DownloadManager.Request(Uri.parse(url));
    request.setTitle("My Custom Download");
    // ... configure download request
    downloadManager.enqueue(request);
});

// Or use the default system downloader
webView.setUseSystemDownloader(true);
```

## Example Implementation

Complete example of setting up EnhancedWebView with all features:

```java
public class MainActivity extends AppCompatActivity {
    private EnhancedWebView webView;
    private ProgressBar progressBar;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        webView = findViewById(R.id.webView);
        progressBar = findViewById(R.id.progressBar);

        // Configure features
        webView.setAdBlockEnabled(true);
        webView.setPopupBlockEnabled(true);
        webView.setRedirectBlockEnabled(true);
        webView.setUseSystemDownloader(true);

        // Load ad blocklist
        webView.loadAdBlockListFromResource(R.raw.adblockserverlist);

        // Add custom blocked domains
        webView.addCustomBlockedDomain("ads.example.com");

        // Setup progress tracking
        webView.setProgressListener(progress -> {
            progressBar.setProgress(progress);
            progressBar.setVisibility(progress == 100 ? View.GONE : View.VISIBLE);
        });

        // Load initial URL
        webView.loadUrl("https://example.com");
    }

    @Override
    public void onBackPressed() {
        if (webView.canGoBack()) {
            webView.goBack();
        } else {
            super.onBackPressed();
        }
    }
}
```

## Permissions

Add these permissions to your AndroidManifest.xml:

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" 
    android:maxSdkVersion="28"/>
<uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />
```

## Contributing

Feel free to submit issues and enhancement requests!

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Credits

Developed by LevelPixel

---

For more information or support, please open an issue in the repository.
