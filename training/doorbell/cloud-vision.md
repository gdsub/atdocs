# Analyze the Image Data

## This lesson teaches you to

1.  Initialize Google Cloud Vision
2.  Annotate a captured image

## Try it out

*   [Doorbell sample app](https://github.com/androidthings/doorbell)

A smart doorbell should be able to automatically extract useful data from an image before sending it to the companion app.

Useful data could include how many people are at your door (perhaps zero…it's a prank!) and their emotional state. In this lesson, you will leverage the power of Google’s Cloud Vision API to do image analysis.

## Set up the Cloud Vision API

* * *

To enable Google Cloud Vision for your project:

1.  Create a Google Cloud project and enable the API, as described in the [Cloud Vision Quickstart](https://cloud.google.com/vision/docs/quickstart) guide.
2.  Generate a new Android API key for your project as described in the [Authenticating to a Cloud API Service](https://cloud.google.com/vision/docs/common/auth) guide.
3.  Add the Cloud Vision Java client library dependencies to your app-level `build.gradle` file:

        dependencies {    ...    compile 'com.google.api-client:google-api-client-android:1.22.0' exclude module: 'httpclient'    compile 'com.google.http-client:google-http-client-gson:1.22.0' exclude module: 'httpclient'    compile 'com.google.apis:google-api-services-vision:v1-rev22-1.22.0'}

4.  Add the required permissions to your app's manifest file:

        <uses-permission android:name="android.permission.INTERNET" />

## Upload the image for processing

* * *

The Cloud Vision API annotates image data with objects detected in the image, the coordinates of the discovered object, and a score indicating how confident the algorithm is in that object discovery.

To send the data to Cloud Vision for processing:

1.  Create a new `VisionRequestInitializer` with your cloud project's API key.
2.  Construct a new `Vision` instance using the `Vision.Builder` and the proper HTTP and JSON instances for the Android platform.

    <aside class="note">**Note:** <span>The `Vision` object represents the API endpoint and internally handles the HTTP transport and JSON parsing logic for each request and response.</span></aside>

3.  Encode the image data into an `Image` instance. Pass that to an `AnnotateImageRequest` and activate the `LABEL_DETECTION` request feature.

4.  Execute the request as part of a `BatchAnnotateImagesRequest` and process the response. This is a blocking method call that will take some time to complete, depending on the network conditions.

    import com.google.api.client.extensions.android.http.AndroidHttp;import com.google.api.client.http.HttpTransport;import com.google.api.client.json.JsonFactory;import com.google.api.client.json.gson.GsonFactory;import com.google.api.services.vision.v1.Vision;import com.google.api.services.vision.v1.VisionRequestInitializer;import com.google.api.services.vision.v1.model.AnnotateImageRequest;import com.google.api.services.vision.v1.model.BatchAnnotateImagesRequest;import com.google.api.services.vision.v1.model.BatchAnnotateImagesResponse;import com.google.api.services.vision.v1.model.EntityAnnotation;import com.google.api.services.vision.v1.model.Feature;import com.google.api.services.vision.v1.model.Image;...public class CloudVisionUtils {...private static final String CLOUD_VISION_API_KEY = "...";...public Map<String, Float> annotateImage(byte[] imageBytes) throws IOException {    // Construct the Vision API instance    HttpTransport httpTransport = AndroidHttp.newCompatibleTransport();    JsonFactory jsonFactory = GsonFactory.getDefaultInstance();    VisionRequestInitializer initializer = new VisionRequestInitializer(CLOUD_VISION_API_KEY);    Vision vision = new Vision.Builder(httpTransport, jsonFactory, null)            .setVisionRequestInitializer(initializer)            .build();    // Create the image request    AnnotateImageRequest imageRequest = new AnnotateImageRequest();    Image image = new Image();    image.encodeContent(imageBytes);    imageRequest.setImage(image);    // Add the features we want    Feature labelDetection = new Feature();    labelDetection.setType("LABEL_DETECTION");    labelDetection.setMaxResults(MAX_LABEL_RESULTS);    imageRequest.setFeatures(Collections.singletonList(labelDetection));    // Batch and execute the request    BatchAnnotateImagesRequest requestBatch = new BatchAnnotateImagesRequest();    requestBatch.setRequests(Collections.singletonList(imageRequest));    BatchAnnotateImagesResponse response = vision.images()            .annotate(requestBatch)            .setDisableGZipContent(true)            .execute();    return convertResponseToMap(response);}

The `BatchAnnotateImagesResponse` returned from the API wraps the annotation data in a few layers. You may wish to simplify the result by extracting the annotation labels and scores into a simpler collection.

    private Map<String, Float> convertResponseToMap(BatchAnnotateImagesResponse response) {    Map<String, Float> annotations = new HashMap<>();    // Convert response into a readable collection of annotations    List<EntityAnnotation> labels = response.getResponses().get(0).getLabelAnnotations();    if (labels != null) {        for (EntityAnnotation label : labels) {            annotations.put(label.getDescription(), label.getScore());        }    }    return annotations;}

You can now upload the image data from the camera and examine the annotations returned by the Cloud Vision API.

    public class DoorbellActivity extends Activity {    ...    private void onPictureTaken(final byte[] imageBytes) {        if (imageBytes != null) {            ...            try {                // Process the image using Cloud Vision                Map<String, Float> annotations = annotateImage(imageBytes);                Log.d(TAG, "cloud vision annotations:" + annotations);            } catch (IOException e) {                Log.e(TAG, "Cloud Vison API error: ", e);            }        }    }}

