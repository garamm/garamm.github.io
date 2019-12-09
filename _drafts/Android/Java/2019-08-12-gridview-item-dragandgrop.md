---
title: "[Android/Java] GridView에서 item Drag and Drop 하기"
categories:
  - Android/Java
---

Data.java
```java
import java.io.Serializable;
import java.util.Date;

public class Data implements Serializable {
    String code;

    public Data(String code) {
        this.code = code;
    }

    public String getCode() {
        return code;
    }

    public void setCode(String code) {
        this.code = code;
    }
}
```

list_item.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:padding="3dp">

    <RelativeLayout
        android:layout_width="wrap_content"
        android:id="@+id/itemBack"
        android:layout_height="wrap_content"
        android:gravity="center">

        <ImageView
            android:id="@+id/backgroundImage"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_centerInParent="true"
            android:scaleType="fitXY"
            android:src="@drawable/kitchen_order_full" />

        <TextView
            android:id="@+id/numberText"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_centerInParent="true" />

    </RelativeLayout>
</RelativeLayout>
```

DataAdapter.java
```java
import android.content.Context;
import android.os.Handler;
import android.os.Message;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.BaseAdapter;
import android.widget.ImageView;
import android.widget.RelativeLayout;
import android.widget.TextView;

import java.io.Serializable;
import java.util.ArrayList;

public class DataAdapter extends BaseAdapter {
    private Context mContext;
    private ArrayList<Data> arrayList;
    private LayoutInflater mInflater;
    public ViewHolder mViewHolder;
    int height;
    public Handler longClickHandler;

    public DataAdapter(Context context, ArrayList<Data> kitchenOrders, int height, Handler longClickHandler) {
        mContext = context;
        arrayList = kitchenOrders;
        this.height = height;
        this.longClickHandler = longClickHandler;
        mInflater = (LayoutInflater) mContext.getSystemService(Context.LAYOUT_INFLATER_SERVICE);
    }

    public void setArrayList(ArrayList<Data> kitchenOrders) {
        this.arrayList = kitchenOrders;
    }

    @Override
    public int getCount() {
        return arrayList.size();
    }

    @Override
    public Object getItem(int position) {
        return mViewHolder;
    }

    @Override
    public long getItemId(int position) {
        return 0;
    }

    @Override
    public View getView(final int position, View convertView, ViewGroup parent) {

        if (convertView == null) {
            mViewHolder = new ViewHolder();
            convertView = mInflater.inflate(R.layout.list_item, null);
            convertView.setTag(mViewHolder);
        }

        mViewHolder = (ViewHolder) convertView.getTag();
        mViewHolder.backgroundImage = (ImageView) convertView.findViewById(R.id.backgroundImage);
        mViewHolder.numberText = (TextView) convertView.findViewById(R.id.numberText);
        mViewHolder.itemBack = (RelativeLayout) convertView.findViewById(R.id.itemBack);
        mViewHolder.itemBack.setTag(position);

        ViewGroup.LayoutParams params = mViewHolder.backgroundImage.getLayoutParams();
        params.height = height / 2 - 10;
        mViewHolder.backgroundImage.setLayoutParams(params);
        mViewHolder.numberText.setText(String.valueOf(arrayList.get(position).getCode()));

        mViewHolder.itemBack.setOnLongClickListener(new View.OnLongClickListener() {
            @Override
            public boolean onLongClick(View v) {
                Message msg = new Message();
                msg.what = (int) v.getTag();
                msg.obj = v;
                longClickHandler.sendMessage(msg);
                return false;
            }
        });


        return convertView;
    }

    public class ViewHolder {
        TextView numberText;
        ImageView backgroundImage;
        RelativeLayout itemBack;
    }
}
```

activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.hanul.myapplication.MainActivity">

    <GridView
        android:id="@+id/gridView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_above="@+id/removeBar"
        android:numColumns="5" />

    <TextView
        android:id="@+id/removeBar"
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:layout_alignParentBottom="true"
        android:background="#000"
        android:gravity="center"
        android:text="remove"
        android:textColor="#fff"
        android:visibility="gone" />

</RelativeLayout>
```

MainActivity.java
```java
import android.content.ClipData;
import android.content.ClipDescription;
import android.content.Context;
import android.graphics.Color;
import android.os.Handler;
import android.os.Message;
import android.support.annotation.NonNull;
import android.support.annotation.Nullable;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.support.v7.view.ActionMode;
import android.util.Log;
import android.view.DragEvent;
import android.view.View;
import android.view.ViewGroup;
import android.widget.AdapterView;
import android.widget.Button;
import android.widget.GridView;
import android.widget.LinearLayout;
import android.widget.TextView;
import android.widget.Toast;

import java.util.ArrayList;

public class MainActivity extends AppCompatActivity {

    ArrayList<Data> datas;
    DataAdapter adapter;
    int height;
    GridView gridView;
    TextView removeBar;
    View longClickView;
    int position;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        gridView = (GridView) findViewById(R.id.gridView);
        removeBar = (TextView) findViewById(R.id.removeBar);
        longClickView = new View(getApplicationContext());

        datas = new ArrayList<Data>();
        for (int i = 0; i < 10; i++) {
            datas.add(new Data(String.valueOf(i)));
        }

        /*longClickView.setOnDragListener(new View.OnDragListener() {
            @Override
            public boolean onDrag(View v, DragEvent event) {
                switch (event.getAction()) {
                    case DragEvent.ACTION_DROP:
                        Toast.makeText(getApplicationContext(), "DROP!", Toast.LENGTH_SHORT).show();
                        removeBar.setVisibility(View.GONE);
                        break;
                }
                return false;
            }
        });*/


        removeBar.setOnDragListener(new View.OnDragListener() {
            @Override
            public boolean onDrag(View v, DragEvent event) {
                switch (event.getAction()) {

                    // 이미지를 드래그 시작될때
                    case DragEvent.ACTION_DRAG_STARTED:
                        Log.d("DragClickListener", "ACTION_DRAG_STARTED");
                        removeBar.setVisibility(View.VISIBLE);
                        removeBar.setBackgroundColor(Color.parseColor("#000000"));
                        break;

                    // 드래그한 이미지를 옮길려는 지역으로 들어왔을때
                    case DragEvent.ACTION_DRAG_ENTERED:
                        Log.d("DragClickListener", "ACTION_DRAG_ENTERED");
                        // 이미지가 들어왔다는 것을 알려주기 위해 배경이미지 변경
                        removeBar.setBackgroundColor(Color.parseColor("#FF919191"));
                        break;

                    // 드래그한 이미지가 영역을 빠져 나갈때
                    case DragEvent.ACTION_DRAG_EXITED:
                        Log.d("DragClickListener", "ACTION_DRAG_EXITED");
                        removeBar.setBackgroundColor(Color.parseColor("#000000"));
                        break;

                    // 이미지를 드래그해서 드랍시켰을때
                    case DragEvent.ACTION_DROP:
                        Log.d("DragClickListener", "ACTION_DROP");

                        if (v == findViewById(R.id.removeBar)) {
                            //View view = (View) event.getLocalState();
                            /*ViewGroup viewgroup = (ViewGroup) view.getParent();
                            viewgroup.removeView(view);*/

                           /* LinearLayout containView = (LinearLayout) v;
                            containView.addView(view);
                            view.setVisibility(View.VISIBLE);*/
                            v.setVisibility(View.GONE);
                            longClickView.setVisibility(View.VISIBLE);
                            datas.remove(position);
                            adapter.notifyDataSetChanged();
                            Toast.makeText(getApplicationContext(), "DROP FOR REMOVE!!", Toast.LENGTH_SHORT).show();

                        } else {
                            Toast.makeText(getApplicationContext(), "DROP!!", Toast.LENGTH_SHORT).show();
                            longClickView.setVisibility(View.VISIBLE);
                            v.setVisibility(View.GONE);
                        }
                        break;

                    case DragEvent.ACTION_DRAG_ENDED:
                        Log.d("DragClickListener", "ACTION_DRAG_ENDED");
                        longClickView.setVisibility(View.VISIBLE);
                        removeBar.setVisibility(View.GONE);
                    default:
                        break;
                }
                return true;
            }
        });


    }
	// 핸들러를 사용하거나 gridView.setOnItemLongClickListener 사용하기
    Handler longClickHandler = new Handler() {
        @Override
        public void handleMessage(Message msg) {
            super.handleMessage(msg);
            longClickView = (View) msg.obj;
            position = msg.what;

            // 태그 생성
            ClipData clip = ClipData.newPlainText("dtagtext", "dtagtext");
            longClickView.startDrag(clip, new View.DragShadowBuilder(longClickView), null, 0);
            longClickView.setVisibility(View.INVISIBLE);
            removeBar.setVisibility(View.VISIBLE);
        }
    };

    @Override
    public void onWindowFocusChanged(boolean hasFocus) {
        super.onWindowFocusChanged(hasFocus);
        height = gridView.getHeight();
        adapter = new DataAdapter(this, datas, height, longClickHandler);
        gridView.setAdapter(adapter);
    }
}
```

kitchen_order_full.png
![img1](/img/2019-08-12-gridview-item-dragandgrop-1.png)

실행 결과(태블릿)
![img2](/img/2019-08-12-gridview-item-dragandgrop-2.png)
![img3](/img/2019-08-12-gridview-item-dragandgrop-3.png)
![img4](/img/2019-08-12-gridview-item-dragandgrop-4.png)
![img5](/img/2019-08-12-gridview-item-dragandgrop-5.png)