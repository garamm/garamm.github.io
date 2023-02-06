---
layout: post
title: "[Android/Java] Horizontal ListView 수평 리스트뷰(with CardView)"
category: android
date: "2019-07-26"
---

build.gradle(Module: app)
```java
dependencies {
    implementation 'com.android.support:recyclerview-v7:28.0.0' // 추가
    implementation 'com.android.support:cardview-v7:28.0.0' // 추가
}
```


activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
 
    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
 
</LinearLayout>
```


layout_item.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/activity_main"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:orientation="horizontal">
 
    <androidx.cardview.widget.CardView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="8dp"
        app:cardCornerRadius="3dp"
        app:cardElevation="8dp">
 
        <RelativeLayout
            android:layout_width="200dp"
            android:layout_height="wrap_content">
 
            <ImageView
                android:id="@+id/cardImg"
                android:layout_width="200dp"
                android:layout_height="200dp"
                android:layout_centerHorizontal="true"
                android:layout_marginBottom="10dp"
                android:src="@mipmap/ic_launcher" />
 
            <TextView
                android:id="@+id/cardText"
                android:text="카드명"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_below="@+id/cardImg"
                android:layout_centerHorizontal="true"
                android:layout_centerVertical="true"
                android:textSize="18dp" />
        </RelativeLayout>
    </androidx.cardview.widget.CardView>
 
</LinearLayout>
```


HorizontalData.java
```java
class HorizontalData {
 
    private int img;
    private String text;
 
    public HorizontalData(int img, String text){
        this.img = img;
        this.text = text;
    }
 
    public String getText() {
        return this.text;
    }
 
    public int getImg() {
        return this.img;
    }
}
```


HorizontalAdapter.java
```java
class HorizontalAdapter extends RecyclerView.Adapter<HorizontalViewHolder> {
 
    private ArrayList<HorizontalData> HorizontalDatas;
     
    public void setData(ArrayList<HorizontalData> list) {
        HorizontalDatas = list;
    }
     
    @Override
    public HorizontalViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.layout_item, parent, false);
        HorizontalViewHolder holder = new HorizontalViewHolder(view);
        return holder;
    }
     
    @Override
    public void onBindViewHolder(HorizontalViewHolder holder, int position) {
        HorizontalData data = HorizontalDatas.get(position);
        holder.cardImg.setImageResource(data.getImg());
        holder.cardText.setText(data.getText());
    }
     
    @Override
    public int getItemCount() {
        return HorizontalDatas.size();
    }
}
     
class HorizontalViewHolder extends RecyclerView.ViewHolder {
     
    public ImageView cardImg;
    public TextView cardText;
     
    public HorizontalViewHolder(View itemView) {
        super(itemView);
        cardImg = itemView.findViewById(R.id.cardImg);
        cardText = itemView.findViewById(R.id.cardText);
    }
}
```


MainActivity.java
```java
public class MainActivity extends AppCompatActivity {
 
    private RecyclerView mHorizontalView;
    private HorizontalAdapter mAdapter;
    private LinearLayoutManager mLayoutManager;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        mHorizontalView = findViewById(R.id.recyclerView);
 
        mLayoutManager = new LinearLayoutManager(this);
        mLayoutManager.setOrientation(LinearLayoutManager.HORIZONTAL);
 
        mHorizontalView.setLayoutManager(mLayoutManager);
        mAdapter = new HorizontalAdapter();
 
        ArrayList<HorizontalData> data = new ArrayList<>();
 
        for (int i=0; i<20; i++) {
            data.add(new HorizontalData(R.mipmap.ic_launcher, "카드: " + i));
            i++;
        }
 
        mAdapter.setData(data);
        mHorizontalView.setAdapter(mAdapter);
    }
}
```

![img1](/img/2019-07-26-horizontal-listview-1.png)