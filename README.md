# TestRecycleview
1.xml引入
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.dxxx.testrecycleview.MainActivity">

    <com.dxxx.refreshloadmorerecycleview.recycleview.Refresh_Loadmore_Layout
        android:id="@+id/refresh"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</RelativeLayout>
2.设置recycleview的adapter
  final Refresh_Loadmore_Layout refresh_loadmore_layout = (Refresh_Loadmore_Layout) findViewById(R.id.refresh);
        handler = new Handler();
        all = new ArrayList<>();
        for (int i = 0; i < 5; i++) {
            all.add("");
        }
        final SimpleRecycleviewAdater simpleRecycleviewAdater = new SimpleRecycleviewAdater(all, MainActivity.this);
        refresh_loadmore_layout.setAdapter(simpleRecycleviewAdater);
        
3.刷新和加载的回调
  refresh_loadmore_layout.setRefreshTouchEvent(new RefreshRecycleview.RefreshTouchEvent() {
            @Override
            public void onRefrshStart() {
                Log.e("refreshRecycleview", "onRefrshStart");
            }

            @Override
            public void onRefreshing() {
                Log.e("refreshRecycleview", "onRefreshing");
            }

            @Override
            public void onRefresh() {

                Log.e("refreshRecycleview", "onRefresh");
                handler.postDelayed(new Runnable() {
                    @Override
                    public void run() {
                        Log.e("发送关闭", "===");
                        refresh_loadmore_layout.stopRefreshOrLoadmore();
                        all.remove(0);
                        simpleRecycleviewAdater.notifyDataSetChanged();
                    }
                }, 2000);

            }

            @Override
            public void onLoadmoreStart() {

                Log.e("refreshRecycleview", "onLoadmoreStart");

            }

            @Override
            public void onLoading() {
                Log.e("refreshRecycleview", "onLoading");

            }

            @Override
            public void onLoadmore() {
                Log.e("refreshRecycleview", "onLoadmore");


                handler.postDelayed(new Runnable() {
                    @Override
                    public void run() {
                        refresh_loadmore_layout.stopRefreshOrLoadmore();
                        all.add("");
                        simpleRecycleviewAdater.notifyDataSetChanged();
                    }
                }, 2000);

            }
        });
