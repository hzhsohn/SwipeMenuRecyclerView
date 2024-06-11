# SwipeMenuRecyclerView
左右滑出按钮的RecyclerView  侧滑删除添加等功能


Add it in your root build.gradle at the end of repositories:
	allprojects {
		repositories {
			...
			maven { url 'https://jitpack.io' }
		}
	}
  
  	dependencies {
	        compile 'com.github.Myspace01:SwipeMenuRecyclerView:-SNAPSHOT'
	}





     <com.umeng.myrecyclerview.SwipeMenuRecyclerView
        android:id="@+id/recyclerView2"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_below="@+id/swipeLayout"
        tools:listitem="@layout/item_ble_led"
        android:paddingBottom="100dp"
        android:clipToPadding="false"
        />





 
        //列表左滑显示删除
        recyclerView2.setSwipeMenuCreator(swipeMenuCreator);
        recyclerView2.setSwipeMenuItemClickListener(mMenuItemClickListener);


 
    /**
     * 菜单创建器，在Item要创建菜单的时候调用。
     */
    private SwipeMenuCreator swipeMenuCreator = new SwipeMenuCreator() {
        @Override
        public void onCreateMenu(SwipeMenu swipeLeftMenu, SwipeMenu swipeRightMenu, int viewType) {
            int width = 200;

            // 1. MATCH_PARENT 自适应高度，保持和Item一样高;
            // 2. 指定具体的高，比如80;
            // 3. WRAP_CONTENT，自身高度，不推荐;
            int height = ViewGroup.LayoutParams.MATCH_PARENT;

            // 根据ViewType来决定哪一个item该如何添加菜单。
            // 这里模拟业务，实际开发根据自己的业务计算。
            SwipeMenuItem addItem = new SwipeMenuItem(getApplicationContext())
                    //.setBackground(R.drawable.picture_check_green)
                    .setBackgroundColor(Color.RED)
                    .setText(R.string.delete_button)
                    .setTextColor(Color.WHITE)
                    .setWidth(width)
                    .setHeight(height);
            swipeRightMenu.addMenuItem(addItem); // 添加菜单到右侧。
        }
    };

    /**
     * RecyclerView的Item的Menu点击监听。
     */
    private SwipeMenuItemClickListener mMenuItemClickListener = new SwipeMenuItemClickListener() {
        @Override
        public void onItemClick(SwipeMenuBridge menuBridge) {
            menuBridge.closeMenu();

            int direction = menuBridge.getDirection(); // 左侧还是右侧菜单。
            int adapterPosition = menuBridge.getAdapterPosition(); // RecyclerView的Item的position。
            int menuPosition = menuBridge.getPosition(); // 菜单在RecyclerView的Item中的Position。

            if (direction == SwipeMenuRecyclerView.RIGHT_DIRECTION) {
                bleDatabaseDevices.remove(adapterPosition);
                adapter2.notifyDataSetChanged();
                Toast.makeText(getApplicationContext(), "list第" + adapterPosition + "; 右侧菜单第" + menuPosition, Toast.LENGTH_SHORT).show();

            } else if (direction == SwipeMenuRecyclerView.LEFT_DIRECTION) {
                Toast.makeText(getApplicationContext(), "list第" + adapterPosition + "; 左侧菜单第" + menuPosition, Toast.LENGTH_SHORT).show();
            }
        }
    };



 
