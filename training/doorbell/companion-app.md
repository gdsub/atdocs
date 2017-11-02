# Implement a Companion App

## This lesson teaches you to

1.  Create a companion app module
2.  Display database entries with Firebase UI

## Try it out

*   [Doorbell sample app](https://github.com/androidthings/doorbell)

A smart doorbell might or might not have a local display. A companion app for mobile devices allows the user to remotely and automatically see new doorbell ring events and images as they are inserted by the embedded device.

In this lesson, you will build an Android mobile app containing a Firebase Realtime Database that is synchronized with the doorbell database.

## Create a companion app module

* * *

Create a second module in your project alongside the embedded doorbell app. To create a new project module:

1.  Follow the instructions to [Create a New Module](https://developer.android.google.cn/studio/projects/add-app-module.html) in Android Studio.
2.  Create a new **Phone & Tablet Module**.

## Add Firebase to your module

* * *

To enable Firebase Realtime Database for your companion app module:

1.  Install the [Firebase Android SDK](https://firebase.google.cn/docs/android/setup) into your app module.
2.  Download and install the `google-services.json` file as described in the instructions.

    <aside class="note">**Note:** <span>For this code sample, you will use the same `google-services.json` file for both the app module (writes data) and the companion app module (reads the data). In a production environment, each module should have its own `google-services.json`, and they should not replace one another.</span></aside>

3.  Add the Firebase Realtime Database and Firebase UI dependencies to your app-level `build.gradle` file:

        dependencies {    ...    compile 'com.google.firebase:firebase-core:9.4.0'    compile 'com.google.firebase:firebase-database:9.4.0'    compile 'com.firebaseui:firebase-ui-database:0.5.3'}

## Display doorbell events with Firebase UI

* * *

To simplify the interaction with the Firebase data model, create a `DoorbellEntry` model class that contains the same properties described in the JSON schema.

    public class DoorbellEntry {    Long timestamp;    String image;    Map<String, Float> annotations;    public DoorbellEntry() {    }    public DoorbellEntry(Long timestamp, String image, Map<String, Float> annotations) {        this.timestamp = timestamp;        this.image = image;        this.annotations = annotations;    }    public Long getTimestamp() {        return timestamp;    }    public String getImage() {        return image;    }    public Map<String, Float> getAnnotations() {        return annotations;    }}

The `FirebaseRecyclerAdapter` from the [FirebaseUI](https://github.com/firebase/FirebaseUI-Android) library simplifies binding Firebase data to a `Recyclerview` for display. Subclasses of the adapter must override `populateViewHolder()` and provide the logic to bind the data from the model into the provided `ViewHolder`.

The following `DoorbellEntryAdapter` binds data from a `DoorbellEntry` instance into a `DoorbellEntryViewHolder`:

    public class DoorbellEntryAdapter extends FirebaseRecyclerAdapter<DoorbellEntry, DoorbellEntryAdapter.DoorbellEntryViewHolder> {    /**     * ViewHolder for each doorbell entry     */    static class DoorbellEntryViewHolder extends RecyclerView.ViewHolder {        public final ImageView image;        public final TextView time;        public final TextView metadata;        public DoorbellEntryViewHolder(View itemView) {            super(itemView);            this.image = (ImageView) itemView.findViewById(R.id.imageView1);            this.time = (TextView) itemView.findViewById(R.id.textView1);            this.metadata = (TextView) itemView.findViewById(R.id.textView2);        }    }    private Context mApplicationContext;    public DoorbellEntryAdapter(Context context, DatabaseReference ref) {        super(DoorbellEntry.class, R.layout.doorbell_entry, DoorbellEntryViewHolder.class, ref);        mApplicationContext = context.getApplicationContext();    }    @Override    protected void populateViewHolder(DoorbellEntryViewHolder viewHolder, DoorbellEntry model, int position) {        // Display the timestamp        CharSequence prettyTime = DateUtils.getRelativeDateTimeString(mApplicationContext,                model.getTimestamp(), DateUtils.SECOND_IN_MILLIS, DateUtils.WEEK_IN_MILLIS, 0);        viewHolder.time.setText(prettyTime);        // Display the image        if (model.getImage() != null) {            // Decode image data encoded by the Cloud Vision library            byte[] imageBytes = Base64.decode(model.getImage(), Base64.NO_WRAP | Base64.URL_SAFE);            Bitmap bitmap = BitmapFactory.decodeByteArray(imageBytes, 0, imageBytes.length);            if (bitmap != null) {                viewHolder.image.setImageBitmap(bitmap);            } else {                Drawable placeholder =                        ContextCompat.getDrawable(mApplicationContext, R.drawable.ic_placeholder);                viewHolder.image.setImageDrawable(placeholder);            }        }        // Display the metadata        if (model.getAnnotations() != null) {            ArrayList<String> keywords = new ArrayList<>(model.getAnnotations().keySet());            int limit = Math.min(keywords.size(), 3);            viewHolder.metadata.setText(TextUtils.join("\n", keywords.subList(0, limit)));        } else {            viewHolder.metadata.setText("no annotations yet");        }    }}

Create an instance of the `DoorbellEntryAdapter` and attach it to a `RecyclerView` in your activity. The FirebaseUI adapters automatically register event listeners with the database. To avoid memory leaks, initialize the adapter in `onStart()` and call its `cleanup()` method in `onStop()`.

To ensure that each new doorbell event is visible to the user, scroll the list to the latest item on each data set change. You can listen for changes events using an `AdapterDataObserver` attached to your adapter.

    public class MainActivity extends AppCompatActivity {    private DatabaseReference mDatabaseRef;    private RecyclerView mRecyclerView;    private DoorbellEntryAdapter mAdapter;    @Override    protected void onCreate(Bundle savedInstanceState) {        super.onCreate(savedInstanceState);        setContentView(R.layout.activity_main);        mRecyclerView = (RecyclerView) findViewById(R.id.doorbellView);        // Show most recent items at the top        LinearLayoutManager layoutManager =                new LinearLayoutManager(this, LinearLayoutManager.VERTICAL, true);        mRecyclerView.setLayoutManager(layoutManager);        // Reference for doorbell events from embedded device        mDatabaseRef = FirebaseDatabase.getInstance().getReference().child("logs");    }    @Override    protected void onStart() {        super.onStart();        mAdapter = new DoorbellEntryAdapter(this, mDatabaseRef);        mRecyclerView.setAdapter(mAdapter);        // Make sure new events are visible        mAdapter.registerAdapterDataObserver(new RecyclerView.AdapterDataObserver() {            @Override            public void onItemRangeChanged(int positionStart, int itemCount) {                mRecyclerView.smoothScrollToPosition(mAdapter.getItemCount());            }        });    }    @Override    protected void onStop() {        super.onStop();        // Tear down Firebase listeners in adapter        if (mAdapter != null) {            mAdapter.cleanup();            mAdapter = null;        }    }}

Congratulations! You have built a cloud-enabled doorbell for the connected home using Android Things!

