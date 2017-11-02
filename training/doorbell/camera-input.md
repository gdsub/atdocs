# Add Camera Support

## This lesson teaches you to

1.  Connect a CSI-2 peripheral
2.  Add camera features to an app
3.  Handle I/O threading
4.  Trigger still image capture
5.  Receive image data from the camera

## Try it out

*   [Doorbell sample app](https://github.com/androidthings/doorbell)

A smart doorbell should capture an image of who (or what) is at the door, allowing the owner to remotely make a decision about whether or not to answer.

Because Android Things builds on the Android framework, you can access the same robust camera APIs that an Android mobile developer would use. In this lesson, you’ll access the camera peripheral using Android camera APIs and capture an image for later processing.

## Connect the camera

* * *

Connect a supported camera module to the CSI-2 camera port on your board. Ensure that the cable is fully inserted and sits evenly before closing the connector latch.

![""](https://developer.android.google.cn/things/images/doorbell-camera-wiring.png)

## Add permissions and required features

* * *

Add the required permissions to your app's manifest file:

        <uses-permission android:name="android.permission.CAMERA" />

## Set up an I/O thread

* * *

Communicating with peripheral hardware introduces blocking operations into the flow of your app. To avoid blocking the app's main thread, and thus delaying framework events, create a background worker thread to process input and handle commands. The `HandlerThread` works very nicely for this purpose.

    public class DoorbellActivity extends Activity {    /**     * A Handler for running tasks in the background.     */    private Handler mCameraHandler;    /**     * An additional thread for running tasks that shouldn't block the UI.     */    private HandlerThread mCameraThread;    @Override    protected void onCreate(Bundle savedInstanceState) {        super.onCreate(savedInstanceState);        // Creates new handlers and associated threads for camera and networking operations.        mCameraThread = new HandlerThread("CameraBackground");        mCameraThread.start();        mCameraHandler = new Handler(mCameraThread.getLooper());    }    @Override    protected void onDestroy() {        super.onDestroy();        mCameraThread.quitSafely();    }}

## Initialize the camera session

* * *

The first step to capture a camera image is to discover the hardware and open a device connection. To connect to a camera device:

1.  Use the `CameraManager` system service to discover the list of available camera devices with `getCameraIdList()`.
2.  Create an `ImageReader` instance to process the raw camera data and produce a JPEG-encoded image to your app. The reader processes data asynchronously, and invokes the provided `OnImageAvailableListener` when the image is ready.
3.  Open a connection to the appropriate camera device using `openCamera()`.
4.  The `CameraDevice.StateCallback` reports that the camera was opened successfully through the `onOpened()` callback method.
5.  Close the `CameraDevice` when it is not in use to free the system resources:

    public class DoorbellCamera {    // Camera image parameters (device-specific)    private static final int IMAGE_WIDTH  = ...;    private static final int IMAGE_HEIGHT = ...;    private static final int MAX_IMAGES   = ...;    // Image result processor    private ImageReader mImageReader;    // Active camera device connection    private CameraDevice mCameraDevice;    // Active camera capture session    private CameraCaptureSession mCaptureSession;    // Initialize a new camera device connection    public void initializeCamera(Context context,                                 Handler backgroundHandler,                                 ImageReader.OnImageAvailableListener imageAvailableListener) {        // Discover the camera instance        CameraManager manager = (CameraManager) context.getSystemService(CAMERA_SERVICE);        String[] camIds = {};        try {            camIds = manager.getCameraIdList();        } catch (CameraAccessException e) {            Log.d(TAG, "Cam access exception getting IDs", e);        }        if (camIds.length < 1) {            Log.d(TAG, "No cameras found");            return;        }        String id = camIds[0];        Log.d(TAG, "Using camera id " + id);        // Initialize image processor        mImageReader = ImageReader.newInstance(IMAGE_WIDTH, IMAGE_HEIGHT,                ImageFormat.JPEG, MAX_IMAGES);        mImageReader.setOnImageAvailableListener(imageAvailableListener, backgroundHandler);        // Open the camera resource        try {            manager.openCamera(id, mStateCallback, backgroundHandler);        } catch (CameraAccessException cae) {            Log.d(TAG, "Camera access exception", cae);        }    }    // Callback handling devices state changes    private final CameraDevice.StateCallback mStateCallback =            new CameraDevice.StateCallback() {        @Override        public void onOpened(CameraDevice cameraDevice) {            mCameraDevice = cameraDevice;        }        ...    };    // Close the camera resources    public void shutDown() {        if (mCameraDevice != null) {            mCameraDevice.close();        }    }}

## Trigger an image capture

* * *

Once the camera device connection is active, create a `CameraCaptureSession` to facilitate image requests from the hardware. To open a capture session:

1.  Build a new `CameraCaptureSession` instance with the `createCaptureSession()` method.
2.  Pass the method a list of potential target surfaces for individual image requests. For this example, pass the surface connected to the `ImageReader` constructed previously.
3.  Attach a `CameraCaptureSession.StateCallback` to report when the session is configured and active. The callback invokes the `onConfigured()` method if everything is successful.

    public class DoorbellCamera {    ...    public void takePicture() {        if (mCameraDevice == null) {            Log.w(TAG, "Cannot capture image. Camera not initialized.");            return;        }        // Here, we create a CameraCaptureSession for capturing still images.        try {            mCameraDevice.createCaptureSession(                    Collections.singletonList(mImageReader.getSurface()),                    mSessionCallback,                    null);        } catch (CameraAccessException cae) {            Log.d(TAG, "access exception while preparing pic", cae);        }    }    // Callback handling session state changes    private CameraCaptureSession.StateCallback mSessionCallback =            new CameraCaptureSession.StateCallback() {            @Override            public void onConfigured(CameraCaptureSession cameraCaptureSession) {                // The camera is already closed                if (mCameraDevice == null) {                    return;                }                // When the session is ready, we start capture.                mCaptureSession = cameraCaptureSession;                triggerImageCapture();            }            @Override            public void onConfigureFailed(CameraCaptureSession cameraCaptureSession) {                Log.w(TAG, "Failed to configure camera");            }    };}

Your app can now use the active `CameraCaptureSession` to request image data from the camera hardware. To begin an image capture request inside the capture session:

1.  Initialize a new `CaptureRequest` using the builder interface. To capture a single still image, use the `TEMPLATE_STILL_CAPTURE` parameter.
2.  Indicate the target surface for the request. For this example, this is the same `ImageReader` surface provided to the capture session.
3.  Set any additional capture parameters, such as auto-focus and auto-exposure, using the request builder.
4.  Initiate the capture request on the `CameraCaptureSession` using the `capture()` method.
5.  When the capture is complete, close the active session.

    public class DoorbellCamera {    // Active camera device connection    private CameraDevice mCameraDevice;    // Active camera capture session    private CameraCaptureSession mCaptureSession;    // Image result processor    private ImageReader mImageReader;    ...    private void triggerImageCapture() {        try {            final CaptureRequest.Builder captureBuilder =                    mCameraDevice.createCaptureRequest(CameraDevice.TEMPLATE_STILL_CAPTURE);            captureBuilder.addTarget(mImageReader.getSurface());            captureBuilder.set(CaptureRequest.CONTROL_AE_MODE, CaptureRequest.CONTROL_AE_MODE_ON);            Log.d(TAG, "Session initialized.");            mCaptureSession.capture(captureBuilder.build(), mCaptureCallback, null);        } catch (CameraAccessException cae) {            Log.d(TAG, "camera capture exception");        }    }    // Callback handling capture progress events    private final CameraCaptureSession.CaptureCallback mCaptureCallback =        new CameraCaptureSession.CaptureCallback() {            ...            @Override            public void onCaptureCompleted(CameraCaptureSession session,                                           CaptureRequest request,                                           TotalCaptureResult result) {                if (session != null) {                    session.close();                    mCaptureSession = null;                    Log.d(TAG, "CaptureSession closed");                }            }        };}

## Handle the image result

* * *

From your activity, initialize the camera and invoke the `takePicture()` method when the doorbell button is pressed. During capture, the camera hardware streams the image data to the provided `ImageReader` surface and invokes the `OnImageAvailableListener` with the result.

To obtain the image after capture completes:

1.  Obtain the latest `Image` from the `ImageReader` provided to the `onImageAvailable()` method.
2.  Retrieve the JPEG-encoded image as a `byte[]` from the buffer returned by the `getBuffer()` method:

    public class DoorbellActivity extends Activity {    /**     *  Camera capture device wrapper     */    private DoorbellCamera mCamera;    @Override    protected void onCreate(Bundle savedInstanceState) {        super.onCreate(savedInstanceState);        ...        mCamera = DoorbellCamera.getInstance();        mCamera.initializeCamera(this, mCameraHandler, mOnImageAvailableListener);    }    @Override    protected void onDestroy() {        super.onDestroy();        ...        mCamera.shutDown();    }    @Override    public boolean onKeyUp(int keyCode, KeyEvent event) {        if (keyCode == KeyEvent.KEYCODE_ENTER) {            // Doorbell rang!            Log.d(TAG, "button pressed");            mCamera.takePicture();            return true;        }        return super.onKeyUp(keyCode, event);    }    // Callback to receive captured camera image data    private ImageReader.OnImageAvailableListener mOnImageAvailableListener =            new ImageReader.OnImageAvailableListener() {        @Override        public void onImageAvailable(ImageReader reader) {            // Get the raw image bytes            Image image = reader.acquireLatestImage();            ByteBuffer imageBuf = image.getPlanes()[0].getBuffer();            final byte[] imageBytes = new byte[imageBuf.remaining()];            imageBuf.get(imageBytes);            image.close();            onPictureTaken(imageBytes);        }    };    private void onPictureTaken(final byte[] imageBytes) {        if (imageBytes != null) {            // ...process the captured image...        }    }}

