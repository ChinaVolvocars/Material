
### 不同布局在GridLayoutManager中占不同的列

- 实现方法1，在调用处设置item占几列

 ```java
mLayoutManager = new GridLayoutManager(this, 2);
mLayoutManager.setSpanSizeLookup(new GridLayoutManager.SpanSizeLookup() {
   @Override
   public int getSpanSize(int position) {
     return mCustomAdapter.getItemViewType(position) == 1 ? 2 : 1;
   }
});
mRecyclerView.setLayoutManager(mLayoutManager);
```

- 实现方法2，在适配器中设置
```java
    public void onAttachedToRecyclerView(RecyclerView recyclerView) {
        super.onAttachedToRecyclerView(recyclerView);
        RecyclerView.LayoutManager manager = recyclerView.getLayoutManager();
        if(manager instanceof GridLayoutManager) {
            final GridLayoutManager gridManager = ((GridLayoutManager) manager);
            gridManager.setSpanSizeLookup(new GridLayoutManager.SpanSizeLookup() {
                @Override
                public int getSpanSize(int position) {
                    Log.d(TAG, "getSpanSize: type = " + getItemViewType(position));
                    return getItemViewType(position) == 1 ? 2 : 1;
                }
            });
        }
    }
```


### 使用过程
- 在activity中
```java
listRV.setLayoutManager(new GridLayoutManager(mActivity, 3));
```
- 在Adapter中
```java
  @Override
  public void onAttachedToRecyclerView(RecyclerView recyclerView) {
    super.onAttachedToRecyclerView(recyclerView);

    RecyclerView.LayoutManager manager = recyclerView.getLayoutManager();
    if (manager instanceof GridLayoutManager) {

      GridLayoutManager gridLayoutManager = (GridLayoutManager) manager;
      gridLayoutManager.setSpanSizeLookup(new GridLayoutManager.SpanSizeLookup() {
        @Override
        public int getSpanSize(int position) {
          int itemViewTypeCustom = getItemViewTypeCustom(position);
          return itemViewTypeCustom == ITEM_TITTLE ? 3 : 1;
        }
      });

    }

  }
```
