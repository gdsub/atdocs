# Analyze the Image Data

##分析图像数据

## This lesson teaches you to

##本节课你将学习

1.  Initialize Google Cloud Vision

2.  初始化 Google Cloud Vision
3.  Annotate a captured image


2. 标记拍摄的图像

## Try it out

##试一试

*   [Doorbell sample app](https://github.com/androidthings/doorbell)
*   [门铃示例应用](https://github.com/androidthings/doorbell)

A smart doorbell should be able to automatically extract useful data from an image before sending it to the companion app.

一个智能的门铃在发送数据到配套的应用之前应该能从图像中自动提取有用的数据。

Useful data could include how many people are at your door (perhaps zero…it's a prank!) and their emotional state. In this lesson, you will leverage the power of Google’s Cloud Vision API to do image analysis.

有用的数据应该包括几个人在门前（或许没人…这是恶作剧！）和他们对应的情绪状态。这节课中，你将利用 Google’s Cloud Vision API 的力量去分析图像。

## Set up the Cloud Vision API

##配置 Cloud Vision API

* * *

To enable Google Cloud Vision for your project:

在你的工程使用 Google Cloud Vision：

1.  Create a Google Cloud project and enable the API, as described in the [Cloud Vision Quickstart](https://cloud.google.com/vision/docs/quickstart) guide.

2.  创建一个 Google Cloud 工程以便使用 API，查看  [Cloud Vision 快速入门](https://cloud.google.com/vision/docs/quickstart)  指南的介绍。
3.  Generate a new Android API key for your project as described in the [Authenticating to a Cloud API Service](https://cloud.google.com/vision/docs/common/auth) guide.


2. 根据  [验证 Cloud API 服务](https://cloud.google.com/vision/docs/common/auth) 指南来为你的工程生成一个新的 Android API 密钥。
3. Add the Cloud Vision Java client library dependencies to your app-level `build.gradle` file:


3. 在你应用的 `build.gradle`  文件中添加  Cloud Vision Java 客户端的库依赖：

```groovy
  dependencies {
    ...

    compile 'com.google.api-client:google-api-client-android:1.22.0' exclude module: 'httpclient'
    compile 'com.google.http-client:google-http-client-gson:1.22.0' exclude module: 'httpclient'

    compile 'com.google.apis:google-api-services-vision:v1-rev22-1.22.0'
}
```

4. Add the required permissions to your app's manifest file:


4. 在应用的 manifest 文件中添加必要的权限：

```html
  <uses-permission android:name="android.permission.INTERNET" />
```

## Upload the image for processing

##上传图像并处理

* * *

The Cloud Vision API annotates image data with objects detected in the image, the coordinates of the discovered object, and a score indicating how confident the algorithm is in that object discovery.

Cloud Vision API 利用图像中检测到的对象，对象的坐标，以及对于对象的评分标记算法来标记图像数据。

To send the data to Cloud Vision for processing:

发送图像到 Cloud Vision 进行相关处理，你需要：

1.  Create a new `VisionRequestInitializer` with your cloud project's API key.


1.  使用你的云工程的 API 密钥创建一个  `VisionRequestInitializer`  对象。
2.  Construct a new `Vision` instance using the `Vision.Builder` and the proper HTTP and JSON instances for the Android platform.


2. 使用 `Vision.Builder` 创建一个新的 `Vision` 实例并且为支持 Android 平台提供正确的 HTTP 和 JSON 实例。

> **Note:** The `Vision` object represents the API endpoint and internally handles the HTTP transport and JSON parsing logic for each request and response.

> **注意:**  `Vision` 对象象征 API 端对每次请求和响应中的 HTTP 传输和 JSON 解析等相关内部处理逻辑。

3. Encode the image data into an `Image` instance. Pass that to an `AnnotateImageRequest` and activate the `LABEL_DETECTION` request feature.


3. 将图像数据转码并传递给  `Image` 实例。然后将这个实例传递给一个 `AnnotateImageRequest` 实例并痛殴设置 `LABEL_DETECTION` 参数来请求相关特征。
4. Execute the request as part of a `BatchAnnotateImagesRequest` and process the response. This is a blocking method call that will take some time to complete, depending on the network conditions.


4. 使用 `BatchAnnotateImagesRequest` 执行请求并处理结果。这是一个耗时的、依赖网络条件的阻塞请求。

```java
import com.google.api.client.extensions.android.http.AndroidHttp;
import com.google.api.client.http.HttpTransport;
import com.google.api.client.json.JsonFactory;
import com.google.api.client.json.gson.GsonFactory;
import com.google.api.services.vision.v1.Vision;
import com.google.api.services.vision.v1.VisionRequestInitializer;
import com.google.api.services.vision.v1.model.AnnotateImageRequest;
import com.google.api.services.vision.v1.model.BatchAnnotateImagesRequest;
import com.google.api.services.vision.v1.model.BatchAnnotateImagesResponse;
import com.google.api.services.vision.v1.model.EntityAnnotation;
import com.google.api.services.vision.v1.model.Feature;
import com.google.api.services.vision.v1.model.Image;
...
public class CloudVisionUtils {
...
private static final String CLOUD_VISION_API_KEY = "...";
...

public Map<String, Float> annotateImage(byte[] imageBytes) throws IOException {
    // Construct the Vision API instance
    HttpTransport httpTransport = AndroidHttp.newCompatibleTransport();
    JsonFactory jsonFactory = GsonFactory.getDefaultInstance();
    VisionRequestInitializer initializer = new VisionRequestInitializer(CLOUD_VISION_API_KEY);
    Vision vision = new Vision.Builder(httpTransport, jsonFactory, null)
            .setVisionRequestInitializer(initializer)
            .build();

    // Create the image request
    AnnotateImageRequest imageRequest = new AnnotateImageRequest();
    Image image = new Image();
    image.encodeContent(imageBytes);
    imageRequest.setImage(image);

    // Add the features we want
    Feature labelDetection = new Feature();
    labelDetection.setType("LABEL_DETECTION");
    labelDetection.setMaxResults(MAX_LABEL_RESULTS);
    imageRequest.setFeatures(Collections.singletonList(labelDetection));

    // Batch and execute the request
    BatchAnnotateImagesRequest requestBatch = new BatchAnnotateImagesRequest();
    requestBatch.setRequests(Collections.singletonList(imageRequest));
    BatchAnnotateImagesResponse response = vision.images()
            .annotate(requestBatch)
            .setDisableGZipContent(true)
            .execute();

    return convertResponseToMap(response);
}
```

The `BatchAnnotateImagesResponse` returned from the API wraps the annotation data in a few layers. You may wish to simplify the result by extracting the annotation labels and scores into a simpler collection.

API 所返回的 `BatchAnnotateImagesResponse` 将标记数据进行了若干层封装。你可能希望将注释标签和分数抽取到一个简单的集合来简化相关操作。

```java
private Map<String, Float> convertResponseToMap(BatchAnnotateImagesResponse response) {
    Map<String, Float> annotations = new HashMap<>();

    // Convert response into a readable collection of annotations
    List<EntityAnnotation> labels = response.getResponses().get(0).getLabelAnnotations();
    if (labels != null) {
        for (EntityAnnotation label : labels) {
            annotations.put(label.getDescription(), label.getScore());
        }
    }

    return annotations;
}
```
You can now upload the image data from the camera and examine the annotations returned by the Cloud Vision API.

现在你可以上传相机的图像数据然后使用  Cloud Vision API 检查返回的结果。

```java
public class DoorbellActivity extends Activity {
    ...

    private void onPictureTaken(final byte[] imageBytes) {
        if (imageBytes != null) {
            ...
            try {
                // Process the image using Cloud Vision
                Map<String, Float> annotations = annotateImage(imageBytes);
                Log.d(TAG, "cloud vision annotations:" + annotations);
            } catch (IOException e) {
                Log.e(TAG, "Cloud Vison API error: ", e);
            }
        }
    }
}
```

