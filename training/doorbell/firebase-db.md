# Synchronize with Firebase

## This lesson teaches you to

1.  Initialize Firebase Realtime Database
2.  Write data into a shared database

## Try it out

*   [Doorbell sample app](https://github.com/androidthings/doorbell)

A smart doorbell should publish the captured events and images to the cloud to allow other clients to connect and display the data, or to enable further analysis.

In this lesson, you will use Firebase Realtime Database to persist and publish events that are consumed by a mobile companion app.

## Add Firebase to your project

* * *

To enable Firebase Realtime Database for your project:

1.  Install the [Firebase Android SDK](https://firebase.google.cn/docs/android/setup) into your app project.
2.  In the [Firebase console](https://firebase.google.cn/console/), select _Import Google Project_ to import the Google Cloud project you created for Cloud Vision into Firebase.
3.  Download and install the `google-services.json` file as described in the instructions.
4.  Add the Firebase Realtime Database dependency to your app-level `build.gradle` file:

        dependencies {    ...    compile 'com.google.firebase:firebase-core:9.4.0'    compile 'com.google.firebase:firebase-database:9.4.0'}

## Configure database rules

* * *

You need to specify who can read and write to your Firebase Realtime Database. To configure your Firebase database access rules:

1.  In the [Firebase console](https://firebase.google.cn/console/), on the page for your project, click **Database**.
2.  Click **Rules**, and update the database rules to allow public read/write access:

        {  "rules": {    ".read": true,    ".write": true  }}

3.  Click **Publish**.

For more information on setting database rules, see [Getting Started with Database Rules](https://firebase.google.cn/docs/database/security/quickstart).

## Write captured data to database

* * *

Upon each doorbell event, the app captures the following pieces of data and stores them in Firebase:

*   Camera image data
*   Event timestamp
*   Cloud Vision annotations

The following JSON schema describes how the data is organized in Firebase for each event:

    <doorbell-entry> {    "image": <Base64 image data>,    "timestamp": <event timestamp>,    "annotations": {        <label>: <score>,        <label>: <score>,        ...    }}

To write the captured data into a Firebase database:

1.  Initialize an instance of the database with `FirebaseDatabase.getInstance()`:

        import com.google.firebase.database.FirebaseDatabase;...public class DoorbellActivity extends Activity {    ...    private FirebaseDatabase mDatabase;    @Override    protected void onCreate(Bundle savedInstanceState) {        super.onCreate(savedInstanceState);        ...        mDatabase = FirebaseDatabase.getInstance();    }}

2.  Write the captured image data, timestamp, and Cloud Vision annotations to a new `DatabaseReference` in the database.

        import com.google.firebase.database.DatabaseReference;import com.google.firebase.database.FirebaseDatabase;import com.google.firebase.database.ServerValue;...public class DoorbellActivity extends Activity {    ...    private FirebaseDatabase mDatabase;    private void onPictureTaken(final byte[] imageBytes) {        if (imageBytes != null) {            final DatabaseReference log = mDatabase.getReference("logs").push();            String imageStr = Base64.encodeToString(imageBytes, Base64.NO_WRAP | Base64.URL_SAFE);            // upload image to firebase            log.child("timestamp").setValue(ServerValue.TIMESTAMP);            log.child("image").setValue(imageStr);            mCloudHandler.post(new Runnable() {                @Override                public void run() {                    Log.d(TAG, "sending image to cloud vision");                    // annotate image by uploading to Cloud Vision API                    try {                        Map<String, Float> annotations = CloudVisionUtils.annotateImage(imageBytes);                        Log.d(TAG, "cloud vision annotations:" + annotations);                        if (annotations != null) {                            log.child("annotations").setValue(annotations);                        }                    } catch (IOException e) {                        Log.e(TAG, "Cloud Vison API error: ", e);                    }                }            });        }    }}

    <aside class="note">**Note:** <span>The `push()` method creates a new node referenced by a unique key.</span></aside>

Your app will now write each captured image and the associated image analysis to a Firebase database! Go ahead and try it with the sample code.

## Verify the synchronized data

* * *

Firebase automatically synchronizes changes to the local database with the cloud. To verify that the data your app wrote was synchronized:

1.  In the [Firebase console](https://firebase.google.cn/console/), on the page for your project, click **Database**.
2.  Click **Data**.
3.  Observe that a new entry was created under the "logs" node.

