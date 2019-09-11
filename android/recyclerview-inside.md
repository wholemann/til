### 구성 요소

- LayoutManager: RecyclerView 내에 item view들의 크기를 측정하고, 위치를 지정하는 역할. 언제 item view들을 재사용해야하는지에 대한 정책을 결정하는 역할.
- Adapter: RecyclerView 내에 보여지는 view들에 대한 data set을 binding 시켜주는 역할.
- ViewHolder: RecyclerView 내 위치하는 곳의 item view와 metadata 정보를 갖고 있는 역할.

### 호출순서

getItemCount -> getItemViewType -> onCreateViewHolder -> onBindViewHolder

#

### Reference
- [RecyclerView 내부 동작 원리](http://blog.naver.com/PostView.nhn?blogId=mail1001&logNo=220682221473&parentCategoryNo=&categoryNo=&viewDate=&isShowPopularPosts=false&from=postList)
