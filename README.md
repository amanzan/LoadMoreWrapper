# LoadMoreWrapper

[![](https://jitpack.io/v/amanzan/LoadMoreWrapper.svg)](https://jitpack.io/#amanzan/LoadMoreWrapper)

Fork from https://github.com/nukc/LoadMoreWrapper, migrated to AndroidX

- Load more support for RecyclerView
- Footer view customizable
- No changes to the original RecyclerView adapter

<img src="https://raw.githubusercontent.com/nukc/LoadMoreWrapper/master/images/item.gif"><img src="https://raw.githubusercontent.com/nukc/LoadMoreWrapper/master/images/grid_custom.gif"><img src="https://raw.githubusercontent.com/nukc/LoadMoreWrapper/master/images/staggeredgrid.gif">

## Installation

It's on jitpack, so you'll need to add this to your root build.gradle:
```
maven { url 'https://jitpack.io' }
```

And then in your app/build.gradle:

````
implementation 'com.github.amanzan:LoadMoreWrapper:2.0.0'
````

## Usage

```java
    //the adapter is the original (这个 adapter 是原有的, 不改动它)
    LoadMoreWrapper.with(adapter)
        .setFooterView(...) // view or layout resource
        .setShowNoMoreEnabled(true) // enable show NoMoreView，default false
        .setListener(new LoadMoreAdapter.OnLoadMoreListener() {
             @Override
             public void onLoadMore(LoadMoreAdapter.Enabled enabled) {
                 // do something
                 // you can enabled.setLoadMoreEnabled(false) when do not need load more
                 // you can enabled.setLoadFailed(true) when load failed
             })
        .into(recyclerView);
```

or

in the original adapter: [demo](https://github.com/nukc/LoadMoreWrapper/blob/master/app/src/main/java/com/github/nukc/sample/AnotherActivity.java)
```java
    private static class AnotherAdapter extends RecyclerView.Adapter<RecyclerView.ViewHolder> {

        private LoadMoreWrapper mWrapper;

        // code..

        @Override
        public void onAttachedToRecyclerView(RecyclerView recyclerView) {
            super.onAttachedToRecyclerView(recyclerView);

            mWrapper = LoadMoreWrapper.with(this);
            mWrapper.setListener(
                    new LoadMoreAdapter.OnLoadMoreListener() {
                        @Override
                        public void onLoadMore(LoadMoreAdapter.Enabled enabled) {
                            // do something
                        }
                    })
                    .setShowNoMoreEnabled(true)
                    .into(recyclerView);
        }

    }

```

方法名 | 备注
:------------- | :-------------
setLoadMoreEnabled(boolean enabled) | 设置是否启用加载更多，默认 true
setShowNoMoreEnabled(boolean enabled) | 设置全部加载完后是否显示没有更多视图，默认 false
setLoadFailed(boolean isLoadFailed) | 设置是否加载失败，默认 false
setNotShowFooterWhenNotCoveredScreen(boolean notShow) | 设置在 item 未铺满屏幕的时候是否不显示底部视图，默认 false
getOriginalAdapter() | 获取原来的 adapter
getFooterView | 获取加载更多视图
getNoMoreView | 获取没有更多视图
getLoadFailedView | 获取加载失败视图

注意：当加载完全部后且已 setLoadMoreEnabled(false)，但如果由于生命周期或其他问题而导致 View 重建，mLoadMoreEnabled 依然为 true。
这时候应该需要保存 mLoadMoreEnabled 的状态，如果是 ViewPager + Fragment，可以使用 setOffscreenPageLimit 进行解决。

## License

    Copyright 2016 nukc

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
