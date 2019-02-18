# Fragment 통신방법

Last Edited: Feb 16, 2019 5:21 PM
Tags: Android

프레그먼트 통신방법

1. 공용 ViewModel
2. interface 구현
3. 이벤트 버스 ( RxJava, Otto(Deprecated))

번호순으로 추천.

AAC ViewModel 이용하면 액티비티를 통해 공통된 ViewModel을 

ViewModelProviders로부터 얻을수 있음.

뷰모델내에 LiveData등을 이용해서 프레그먼트간 싱크를 맞출수 있음.