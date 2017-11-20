# Synchronize with Firebase

##同步 Firebase

## This lesson teaches you to

## 这节课你将学习

1.  Initialize Firebase Realtime Database

2.  初始化 Firebase 实时数据库

3.  Write data into a shared database

4.  写入数据到一个共享数据库

## Try it out

##试一试

*   [Doorbell sample app](https://github.com/androidthings/doorbell)
*   [门铃例子应用](https://github.com/androidthings/doorbell)

A smart doorbell should publish the captured events and images to the cloud to allow other clients to connect and display the data, or to enable further analysis.

智能门铃应该将拍照事件和图像发布到云端，以便允许其他客户端进行连接并展示数据，或者进一步的分析。

In this lesson, you will use Firebase Realtime Database to persist and publish events that are consumed by a mobile companion app.

本节课中，你将使用 Firebase 实时数据库保存和发布配套应用所使用的门铃事件。

## Add Firebase to your project

##在你的工程中添加 Firebase

* * *

To enable Firebase Realtime Database for your project:

为了让你的工程支持 Firebase 实时数据库：

1.  Install the [Firebase Android SDK](https://firebase.google.cn/docs/android/setup) into your app project.


1.  在你的工程中安装 [Firebase Android SDK](https://firebase.google.cn/docs/android/setup) 
2.  In the [Firebase console](https://firebase.google.cn/console/), select _Import Google Project_ to import the Google Cloud project you created for Cloud Vision into Firebase.


2. 在 [Firebase 控制台](https://firebase.google.cn/console/) 选择 _Import Google Project_  来将你为 Cloud Vision 创建的 Google Cloud 工程导入 Firebase 中。
3. Download and install the `google-services.json` file as described in the instructions.


3. 按说明下载并安装   `google-services.json` 文件。
4. Add the Firebase Realtime Database dependency to your app-level `build.gradle` file:


4. 在你应用的 `build.gradle` 文件中添加  Firebase 实时数据库依赖：

   ```groovy
   dependencies {
       ...

       compile 'com.google.firebase:firebase-core:9.4.0'
       compile 'com.google.firebase:firebase-database:9.4.0'
   }
   ```

## Configure database rules

##配置数据库规则

* * *

You need to specify who can read and write to your Firebase Realtime Database. To configure your Firebase database access rules:

你需要指定谁可以读和写你的 Firebase 实时数据库。使用下面的规则来配置你的数据库：

1.  In the [Firebase console](https://firebase.google.cn/console/), on the page for your project, click **Database**.


1.  在 [Firebase 控制台](https://firebase.google.cn/console/) ，在你的工程页，点击  **Database**。
2.  Click **Rules**, and update the database rules to allow public read/write access:


2. 点击 **Rules** ，然后更新数据库规则为所有人拥有读写权限：

   ```json
   {
     "rules": {
       ".read": true,
       ".write": true
     }
   }
   ```

3. Click **Publish**.


3. 点击 **发布**。

For more information on setting database rules, see [Getting Started with Database Rules](https://firebase.google.cn/docs/database/security/quickstart).

更多设置数据库规则的信息，请查看 [数据库规则入门指南](https://firebase.google.cn/docs/database/security/quickstart)。

## Write captured data to database

##将拍摄数据写入到数据库

* * *

Upon each doorbell event, the app captures the following pieces of data and stores them in Firebase:

应用将会有如下一系列数据并将他们存入 Firebase：

*   Camera image data
*   相机图像数据
*   Event timestamp
*   事件发生时的时间戳
*   Cloud Vision annotations
*   Cloud Vision 标注

The following JSON schema describes how the data is organized in Firebase for each event:

下面的 JSON 概要描述了每次事件的数据在 Firebase 中是如何存储的：

```json
<doorbell-entry> {
    "image": <Base64 image data>,
    "timestamp": <event timestamp>,
    "annotations": {
        <label>: <score>,
        <label>: <score>,
        ...
    }
}
```
To write the captured data into a Firebase database:

将拍摄的数据存入 Firebase 数据库中：

1.  Initialize an instance of the database with `FirebaseDatabase.getInstance()`:


1. 使用 `FirebaseDatabase.getInstance()` 初始化一个数据库实例：

   ```java
   import com.google.firebase.database.FirebaseDatabase;
   ...

   public class DoorbellActivity extends Activity {
       ...

       private FirebaseDatabase mDatabase;

       @Override
       protected void onCreate(Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
           ...

           mDatabase = FirebaseDatabase.getInstance();
       }
   }
   ```


2. Write the captured image data, timestamp, and Cloud Vision annotations to a new `DatabaseReference` in the database.


2. 使用一个新的  `DatabaseReference`  将拍摄的图像数据，时间戳，以及 Cloud Vision 标注写入数据库。

   ```java
   import com.google.firebase.database.DatabaseReference;
   import com.google.firebase.database.FirebaseDatabase;
   import com.google.firebase.database.ServerValue;
   ...

   public class DoorbellActivity extends Activity {
       ...

       private FirebaseDatabase mDatabase;

       private void onPictureTaken(final byte[] imageBytes) {
           if (imageBytes != null) {
               final DatabaseReference log = mDatabase.getReference("logs").push();
               String imageStr = Base64.encodeToString(imageBytes, Base64.NO_WRAP | Base64.URL_SAFE);
               // upload image to firebase
               log.child("timestamp").setValue(ServerValue.TIMESTAMP);
               log.child("image").setValue(imageStr);

               mCloudHandler.post(new Runnable() {
                   @Override
                   public void run() {
                       Log.d(TAG, "sending image to cloud vision");
                       // annotate image by uploading to Cloud Vision API
                       try {
                           Map<String, Float> annotations = CloudVisionUtils.annotateImage(imageBytes);
                           Log.d(TAG, "cloud vision annotations:" + annotations);
                           if (annotations != null) {
                               log.child("annotations").setValue(annotations);
                           }
                       } catch (IOException e) {
                           Log.e(TAG, "Cloud Vison API error: ", e);
                       }
                   }
               });
           }
       }
   }
   ```

> **Note:** The `push()` method creates a new node referenced by a unique key.

> 注意：`push()` 方法将会根据唯一的键值创建一个新的节点。

Your app will now write each captured image and the associated image analysis to a Firebase database! Go ahead and try it with the sample code.

你的应用将每次拍摄的图像和对应的图像标记数据写入到 Firebase 数据库中！继续下去并尝试使用示例代码。

## Verify the synchronized data

##验证同步的数据

* * *

Firebase automatically synchronizes changes to the local database with the cloud. To verify that the data your app wrote was synchronized:

Firebase 会自动将更改同步到本地数据库中。下面来验证你的应用已经被自动同步：

1.  In the [Firebase console](https://firebase.google.cn/console/), on the page for your project, click **Database**.


1. 在 [Firebase 控制台](https://firebase.google.cn/console/)，在你工程中，点击 **Database**。
2. Click **Data**.


2. 点击 **Data**。
3. Observe that a new entry was created under the "logs" node.


3. 观察一个新的实体在 “logs” 节点中被创建。