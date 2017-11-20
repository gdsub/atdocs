# Implement a Companion App

##实现一个配套的应用

## This lesson teaches you to

##本节课你将学习

1.  Create a companion app module

2.  创建一个配套应用模块
3.  Display database entries with Firebase UI


2. 使用 Firebase 可视化页面显示数据

## Try it out

##试一试

*   [Doorbell sample app](https://github.com/androidthings/doorbell)
*   [门铃例示例应用](https://github.com/androidthings/doorbell)

A smart doorbell might or might not have a local display. A companion app for mobile devices allows the user to remotely and automatically see new doorbell ring events and images as they are inserted by the embedded device.

智能的门铃可能会或可能没有本地显示设备。与之配套的移动设备应用程序允许用户远程自动查看新的门铃按铃事件和拍摄到的图片。

In this lesson, you will build an Android mobile app containing a Firebase Realtime Database that is synchronized with the doorbell database.

在本节课中，你将编译一个包含可以同步门铃数据库的 Firebase 实时库数据的 Android 移动应用。

## Create a companion app module

##创建一个配套的应用模块

* * *

Create a second module in your project alongside the embedded doorbell app. To create a new project module:

在你工程中同嵌入式门铃应用同级目录中创建另一个模块。创建一个新的工程模块，需要：

1.  Follow the instructions to [Create a New Module](https://developer.android.google.cn/studio/projects/add-app-module.html) in Android Studio.


1.  在 Android Studio 中跟随这个教程 [创建一个新的模块](https://developer.android.google.cn/studio/projects/add-app-module.html)。
2.  Create a new **Phone & Tablet Module**.


2. 创建一个新的 **手机 & 平板模块**。

## Add Firebase to your module

##在你的模块中添加 Firebase 

* * *

To enable Firebase Realtime Database for your companion app module:

为了让你的应用模块支持 Firebase 实时数据库，你需要：

1.  Install the [Firebase Android SDK](https://firebase.google.cn/docs/android/setup) into your app module.


1.  在你的应用模块中安装 [Firebase Android SDK](https://firebase.google.cn/docs/android/setup)。
2.  Download and install the `google-services.json` file as described in the instructions.


2. 根据说明和介绍下载并安装 `google-services.json` 文件。

> **Note:** For this code sample, you will use the same `google-services.json` file for both the app module (writes data) and the companion app module (reads the data). In a production environment, each module should have its own `google-services.json`, and they should not replace one another.

> 注意：在该代码示例中，你将使用同应用模块（写入数据）以及配套应用模块（读取数据）相同的 `google-services.json` 文件。在生产环境中每个模块应该有自己的  `google-services.json`文件，并且不能相互替换。

3. Add the Firebase Realtime Database and Firebase UI dependencies to your app-level `build.gradle` file:


3. 在你应用的 `build.gradle`  文件中添加 Firebase实时数据库和 Firebase 可视化界面依赖：

   ```groovy
   dependencies {
       ...

       compile 'com.google.firebase:firebase-core:9.4.0'
       compile 'com.google.firebase:firebase-database:9.4.0'
       compile 'com.firebaseui:firebase-ui-database:0.5.3'
   }
   ```

## Display doorbell events with Firebase UI

##在 Firebase 可视化界面中显示门铃事件

* * *

To simplify the interaction with the Firebase data model, create a `DoorbellEntry` model class that contains the same properties described in the JSON schema.

为了简化 Firebase 数据模型的交互，请一个  `DoorbellEntry`  模型类，其包含与之前 JSON 文件结构相同的属性。

```java
public class DoorbellEntry {

    Long timestamp;
    String image;
    Map<String, Float> annotations;

    public DoorbellEntry() {
    }

    public DoorbellEntry(Long timestamp, String image, Map<String, Float> annotations) {
        this.timestamp = timestamp;
        this.image = image;
        this.annotations = annotations;
    }

    public Long getTimestamp() {
        return timestamp;
    }

    public String getImage() {
        return image;
    }

    public Map<String, Float> getAnnotations() {
        return annotations;
    }
}
```
The `FirebaseRecyclerAdapter` from the [FirebaseUI](https://github.com/firebase/FirebaseUI-Android) library simplifies binding Firebase data to a `Recyclerview` for display. Subclasses of the adapter must override `populateViewHolder()` and provide the logic to bind the data from the model into the provided `ViewHolder`.

[FirebaseUI](https://github.com/firebase/FirebaseUI-Android)  中的 `FirebaseRecyclerAdapter` 简化了在 `Recyclerview` 中展示 Firebase 数据的操作。适配器的子类必须重写 `populateViewHolder()` 方法并且需要提供在 `ViewHolder` 中进行数据绑定的逻辑。

The following `DoorbellEntryAdapter` binds data from a `DoorbellEntry` instance into a `DoorbellEntryViewHolder`:

在下面代码中， `DoorbellEntryAdapter` 适配器将 `DoorbellEntry` 实例的数据绑定到 `DoorbellEntryViewHolder` 上：

```java
public class DoorbellEntryAdapter extends FirebaseRecyclerAdapter<DoorbellEntry, DoorbellEntryAdapter.DoorbellEntryViewHolder> {

    /**
     * ViewHolder for each doorbell entry
     */
    static class DoorbellEntryViewHolder extends RecyclerView.ViewHolder {

        public final ImageView image;
        public final TextView time;
        public final TextView metadata;

        public DoorbellEntryViewHolder(View itemView) {
            super(itemView);

            this.image = (ImageView) itemView.findViewById(R.id.imageView1);
            this.time = (TextView) itemView.findViewById(R.id.textView1);
            this.metadata = (TextView) itemView.findViewById(R.id.textView2);
        }
    }

    private Context mApplicationContext;

    public DoorbellEntryAdapter(Context context, DatabaseReference ref) {
        super(DoorbellEntry.class, R.layout.doorbell_entry, DoorbellEntryViewHolder.class, ref);

        mApplicationContext = context.getApplicationContext();
    }

    @Override
    protected void populateViewHolder(DoorbellEntryViewHolder viewHolder, DoorbellEntry model, int position) {
        // Display the timestamp
        CharSequence prettyTime = DateUtils.getRelativeDateTimeString(mApplicationContext,
                model.getTimestamp(), DateUtils.SECOND_IN_MILLIS, DateUtils.WEEK_IN_MILLIS, 0);
        viewHolder.time.setText(prettyTime);

        // Display the image
        if (model.getImage() != null) {
            // Decode image data encoded by the Cloud Vision library
            byte[] imageBytes = Base64.decode(model.getImage(), Base64.NO_WRAP | Base64.URL_SAFE);
            Bitmap bitmap = BitmapFactory.decodeByteArray(imageBytes, 0, imageBytes.length);
            if (bitmap != null) {
                viewHolder.image.setImageBitmap(bitmap);
            } else {
                Drawable placeholder =
                        ContextCompat.getDrawable(mApplicationContext, R.drawable.ic_placeholder);
                viewHolder.image.setImageDrawable(placeholder);
            }
        }

        // Display the metadata
        if (model.getAnnotations() != null) {
            ArrayList<String> keywords = new ArrayList<>(model.getAnnotations().keySet());

            int limit = Math.min(keywords.size(), 3);
            viewHolder.metadata.setText(TextUtils.join("\n", keywords.subList(0, limit)));
        } else {
            viewHolder.metadata.setText("no annotations yet");
        }
    }

}
```
Create an instance of the `DoorbellEntryAdapter` and attach it to a `RecyclerView` in your activity. The FirebaseUI adapters automatically register event listeners with the database. To avoid memory leaks, initialize the adapter in `onStart()` and call its `cleanup()` method in `onStop()`.

创建一个  `DoorbellEntryAdapter`  实例并且将它关联给你页面的  `RecyclerView`。FirebaseUI 适配器自动为数据库注册数据监听器。为了避免内存泄漏，在 `onStart()` 中初始化适配器并且在 `onStop()` 中调用适配器的 `cleanup()` 方法。

To ensure that each new doorbell event is visible to the user, scroll the list to the latest item on each data set change. You can listen for changes events using an `AdapterDataObserver` attached to your adapter.

为了确保最新的门铃事件对用户是可见的，可以在每次数据变化时滚动列表到最新的列表项。你可以为你的适配器绑定一个 `AdapterDataObserver` 方法监听每一次数据改变。

```java
public class MainActivity extends AppCompatActivity {

    private DatabaseReference mDatabaseRef;

    private RecyclerView mRecyclerView;
    private DoorbellEntryAdapter mAdapter;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mRecyclerView = (RecyclerView) findViewById(R.id.doorbellView);
        // Show most recent items at the top
        LinearLayoutManager layoutManager =
                new LinearLayoutManager(this, LinearLayoutManager.VERTICAL, true);
        mRecyclerView.setLayoutManager(layoutManager);

        // Reference for doorbell events from embedded device
        mDatabaseRef = FirebaseDatabase.getInstance().getReference().child("logs");
    }

    @Override
    protected void onStart() {
        super.onStart();

        mAdapter = new DoorbellEntryAdapter(this, mDatabaseRef);
        mRecyclerView.setAdapter(mAdapter);

        // Make sure new events are visible
        mAdapter.registerAdapterDataObserver(new RecyclerView.AdapterDataObserver() {
            @Override
            public void onItemRangeChanged(int positionStart, int itemCount) {
                mRecyclerView.smoothScrollToPosition(mAdapter.getItemCount());
            }
        });
    }

    @Override
    protected void onStop() {
        super.onStop();

        // Tear down Firebase listeners in adapter
        if (mAdapter != null) {
            mAdapter.cleanup();
            mAdapter = null;
        }
    }
}
```

Congratulations! You have built a cloud-enabled doorbell for the connected home using Android Things!

祝贺你！你已经使用 Android Things 创建了一个连接家的云门铃！