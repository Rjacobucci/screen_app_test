# screen_test Android App

This repo contains the Android Application used in the screen_test study. The application allows participants to record and upload screenshots taken every X number of seconds. The general layout of the code is explained below.

### Features added by Univeristy of Notre Dame, Center for Research Computing

1.  GPS coordinate logging:  When the app is actively recording the screen images, it will also write JSON files of the user's location as \<hash\>_\<datetime\>_gps.json.  

2.  Foreground app info:  The app will use the OS process list to find the top application running, and record it to a JSON file, \<hash\>_\<datetime\>_foreground.json.  



### Activities

| Activity Name    | Purpose                                                      |
| ---------------- | ------------------------------------------------------------ |
| RegisterActivity | Handles registration for new users.                          |
| MainActivity     | Contains the main interface of the app, allowing participants to start/stop the screen capture. |
| DevToolsActivity | Contains tools to tweak how the app works, including the number of images to send per batch, the number of batches to send in parallel etc. |

### Service

| Service Name   | Purpose                                                      |
| -------------- | ------------------------------------------------------------ |
| CaptureService | Responsible for capturing screenshots and foreground app name every X number of seconds. Runs continously throughout the duration of the study. |
| UploadService  | Responsible for uploading screenshots to the configured API endpoint (AWS API Gateway in this version). Triggered at certain times by `UploadScheduler` |
| LocationService| Responsible for capturing GPS coordinates every 10 seconds, recording them in JSON files.  |

### Other Files

| File Name          | Purpose                                                      |
| ------------------ | ------------------------------------------------------------ |
| Constants          | Contains the constants used throughout the application.      |
| Batch              | Contains a "batch" of files, used by `UploadService`.        |
| Encryptor          | Used during the encryption process by `CaptureService`.      |
| InfoDialog         | The dialog that is shown when the "information" button is pressed on the main activity. |
| InternetConnection | A set of functions to check if the device is connected to the internet, and through what type of connection (WiFi vs mobile data). |
| Logger             | Utility functions to save logs to SharedPreferences.         |
| UploadScheduler    | Schedules the `UploadService` at certain times a day.        |



## Constants

Most app-related constants are located in the `Constants` file. The constants are explained below.

| Constant Name       | Explanation                                   |
| ------------------- | --------------------------------------------- |
| REGISTER_ADDRESS    | The address of the "register" cloud function. |
| UPLOAD_ADDRESS      | The address of the upload API endpoint. Currently `https://39u9e4x1c9.execute-api.us-east-2.amazonaws.com/default/upload-function` |
| COUNT_ADDRESS       | The address of the "count" cloud function.    |
| BATCH_SIZE_DEFAULT  | TODO                                          |
| MAX_TO_SEND_DEFAULT | TODO                                          |
| MAX_BATCHES_TO_SEND | TODO                                          |
| REQ_TIMEOUT         | TODO                                          |

### Updating endpoint URLs

`Constants.java` holds the API endpoints. Update these strings with your own URLs:

```
public class Constants {
    public static final String REGISTER_ADDRESS = "<your register function URL>";
    public static final String UPLOAD_ADDRESS = "<your upload endpoint URL>";
    public static final String COUNT_ADDRESS = "<your count function URL>";
    ...
}
```

Rebuild the app after modifying the values so the new endpoints are used.

