# JAVA I/O

Last Edited: Feb 12, 2019 2:39 AM
Tags: JAVA

Java I/O는 자바에서의 입출력 프로그래밍을 말하는데 입력은 키보드 뿐만 아니라 네트워크, 파일 등의 다양한 형태가 될 수 있으며 출력도 마찬가지로 모니터 뿐만 아니라 네트워크, 파일 등으로 출력 될 수 있다.

InpuStreamReader의 생성자에 필요한 인자 = 표준 입력을 통해 획득

BufferedReader의 생성자에 필요한 인자 = InputStreamReader

    Keyboard buffer -> inputStream -> InputStreamReader -> 
    BufferedReader -> br.readLine()

InputStream , OutputStream

하지만 Byte 단위라서 영어나 숫자 등은 잘 출력되는데,

단위가 2바이트인 한글은 깨져서 출력된다.

그래서 char 단위인 InputStreamReader를 쓴다.

해당 클래스를 쓰면 한글도 잘 출력되지만, 한글자씩 받아와야 하는

상황이 아니면 버퍼에 저장하여 한꺼번에 받는 방식을 많이 사용한다.

    InputStream is = // inputStream 초기화는 상황마다 틀리므로, 초기화됬다 가정한다.
    String temp;
    
    BufferedReader buffer = new BufferReader(new InputStreamReader(is,"UTF-8");
    
    while((temp = buffer.readLine() ) != null) {
    
    //........
    
    }

readLine() 은 한 줄씩 읽어온다. "\n", "\r"을 만날 때까지 읽어온다.

InputStream에 개행문자 포함되어야 한다. 스트림의 개행은 "\r\n".

보내는 쪽에선 반드시 "\r\n"을 붙여야 한다.

    String tempData = "아야어여오유";
    
    Byte[] bytes = (tempData+"\r\n").getBytes();

그러면 BufferedReader vs Scanner 차이는?

Scanner의 버퍼 사이즈는 1024 chars 이고, BufferedReader의 버퍼사이즈는 8192 chars이기 때문에 많은 입력이 있다면 BufferedReader가 성능 상 우위를 가질 수 밖에 없다.

Scanner는 내부적으로 regex를 매우 많이 이용하기 때문인 것으로 보인다.