# NFCSample
## Manifest
* add permission
```xml
<uses-permission android:name="android.permission.NFC" />
<uses-feature android:name="android.hardware.nfc" android:required="true" />
```

* add filter
```xml
            </intent-filter>
            <!-- NFC-related intent filter. Allows application to handle messages from any
                 NFC-A devices discovered. Other Android devices are required to support NFC-A.
                 See: res/xml/nfc_tech_filter.xml -->
            <intent-filter>
                <action android:name="android.nfc.action.TECH_DISCOVERED" />
            </intent-filter>
            <meta-data
                android:name="android.nfc.action.TECH_DISCOVERED"
                android:resource="@xml/nfc_tech_filter" />
```

* add nfc filter configure /res/xml/nfc_tech_filter.xml

Because we only want tag id (serial number), I does not specify any teck type in teck-list
```xml
<?xml version="1.0" encoding="utf-8"?>
<!-- This file is used as part of the filter for incoming NFC TECH_DISCOVERED intents. -->
<resources xmlns:android="http://schemas.android.com/apk/res/android">
    <!-- Android's host card emulation feature only supports the IsoDep protocol. -->
    <tech-list>
        <!--<tech>android.nfc.tech.IsoDep</tech>-->
    </tech-list>
</resources>
```


## Code
* CardReader implement androdid NFC callback function. Main activity need initial it and set up.
```java
        Activity activity = this;//getActivity();
        NfcAdapter nfc = NfcAdapter.getDefaultAdapter(activity);
        if (nfc != null) {
            nfc.enableReaderMode(activity, mCardReader, READER_FLAGS, null);
        }
```

* Main activity also need implement a callback function `CardReader.TagCallback` te receive NFC data.
```java
    @Override
    public void onSerialNumberReceived(final String serialNumber) {
        // This callback is run on a background thread, but updates to UI elements must be performed
        // on the UI thread.
        //getActivity().runOnUiThread(new Runnable() {
        this.runOnUiThread(new Runnable() {
            @Override
            public void run() {
                mSerialNumView.setText(serialNumber);
            }
        });
    }
```
