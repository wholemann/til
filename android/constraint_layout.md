- constraint 제약 조건 이용시 match_parent보단 left/right 또는 top/bottom 제약 조건과 함께 match_constraint(=0dp)를 이용한다.
- TextView는 wrap_content일 때 길이가 가변적이므로 옆이나 밑에 배치된 View가 화면 밖으로 밀려나는 경우가 있다. 아래 속성을 이용하면 된다.
    - app:layout_constrainedWidth=”true|false”
    - app:layout_constrainedHeight=”true|false”

### match_constraint
- 기본적으로 match_parent처럼 공간을 부모뷰에 맞게 꽉 채우게 된다.
    - layout_constraintWidth_min and layout_constraintHeight_min : WRAP_CONTENT처럼 동작하나 최소값을 가짐
    - layout_constraintWidth_max and layout_constraintHeight_max : WRAP_CONTENT처럼 동작하나 최대값을 가짐
    - layout_constraintWidth_percent and layout_constraintHeight_percent : 0에서 1까지 float 값을 입력하여 비율적으로 길이를 결정
- 만약 뷰의 가로와 세로의 비율을 결정하고 싶다면 아래의 속성을 이용하시면 된다.
    - layout_constraintDimensionRatio
```
<Button android:layout_width="wrap_content"
        android:layout_height="0dp" 
        app:layout_constraintDimensionRatio="1:1"/>
```
```
<Button android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintDimensionRatio="H,16:9"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintTop_toTopOf="parent"/>
```
### chains
- chain속성을 통해 연결 할 때 수평 기준 가장 왼쪽 or 수직 기준 가장 상단 View가 기준(Head)가 된다.
- chainStyle은 head 뷰에만 속성을 적어주면 된다. 기본 style은 CHAIN_SPREAD.
    - CHAIN_SPREAD 뷰들을 골고루 펼쳐 여백을 같게 한다.(기본값)
    - CHAIN_SPREAD에서의 Weighted chain은 만약 뷰의 길이가 0dp로 지정되어있다면 남은 공간을 수치만큼 비율적으로 나눠갔는다.
    - CHAIN_SPREAD_INSIDE CHAIN_SPREAD와 비슷하지만 가장 외곽에 있는 뷰들은 부모 뷰와 여백이 없는 상태로 골고루 펼쳐진다.
    - CHAIN_PACKED뷰들이 똘똘 뭉치게 되고 부모뷰로부터의 여백을 같게 한다. 여백을 조정하고 싶다면 bias조정을 통해 한쪽으로 치우치게 만들 수 있다.

참고
- https://www.charlezz.com/?p=669