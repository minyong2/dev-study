##### 파일 읽기,파일 쓰기
---
## JAVA 파일생성/읽기/쓰기

1. 파일 유무 확인 및 생성

@ 파일 객체 안에는 여러가지 함수가 있는데 가장 요긴하게 쓰이는 함수는 exists()함수이다.

==> 주로 파일 생성,사용 전 해당 위치에 파일 또는 폴더가 이미 존재하는지 아닌지 확인할 때 사용
```java
    public static void main(String[] args) {
        File file = new File(File.pathSeparator);
        if (!file.exists()){
            try {
                file.createNewFile();

            }catch (Exception e){
                e.printStackTrace();
            }
        }}
```
2. 파일 읽기

- 파일에 값을 읽을때는 FileInputStream, InputStreamReader,BufferedReader 객체를 사용한다.
- 예제코드를 보면 세 번의 객체 생성 작업이 있는 것을 볼 수 있는데 모두 방금 전에 생성한 객체를 인자로 사용한다.

    ==> 이 방법은 객체가 가진 데이터 값을 변환하는 과정이다.
```
FileInputStream은 시스템에 존재하는 파일의 byte를 획득
InputStreamReader는 InputStream 획득한 복수개의 byte를 해석해 특정한 문자열 집합인 charset으로 만든다.
최종적으로 BufferedReader에서는 charset값을 읽어서 string으로 변환한다.
```
```java
try {
    FileInputStream fIn = new FileInputStream(file);
    InputStreamReader isr = new InputStreamReader(fIn);
    BufferedReader inBuff = new BufferedReader(isr);

    while (true) {
        try {
            inputLine = inBuff.readLine();
        } catch(IOException e) {
            e.printStackTrace();
        }

        if (inputLine == null)
            break;

        outStringBuf.append(inputLine);
    }
} catch(FileNotFoundException e) {
    e.printStackTrace();
}
```

3. 파일 쓰기
- 파일에 값을 쓸 때는 FileOutputStream,OutputStreamWriter 객체를 사용
- OutputStreamWriter는 Character형태의 Stream을 byte형태로 전환해 값을 입력 할 수 있는 객체.
- OutputStreamWriter생성 시 변환한 값을 입력할 Stream 객체를 전달 받는데 이 때 File 객체를 품은 FileOutputStream 객체를 사용하면 File에 값을 사용하도록 할 수 있다.

- 파일 쓰기 끝나면 반드시 close() 메서드로 닫아야 함 ★

```java
final String welcomeString = new String(initialValue);
FileOutputStream fOut = new FileOutputStream(file);
OutputStreamWriter osw = new OutputStreamWriter(fOut);

osw.write(welcomeString);
osw.flush();
osw.close();
```


---
# `Oracle Version`

![image](https://user-images.githubusercontent.com/97263974/187319551-42eaf466-21cf-49c1-bdb0-e17e564b672a.png)

---

![image](https://user-images.githubusercontent.com/97263974/187319639-c097278e-127e-44ad-a77d-18b3a9dbf5f6.png)

---

# `Postgresql Version`
```sql
private void fileUploadWithThumbnail(String attach_file_seq, String file_path, String user_id, String thumbnail) {
    File uploadFile = new File(file_path);
    MultipartEntityBuilder builder = MultipartEntityBuilder.create() //객체 생성
            .setCharset(Charset.forName("UTF-8")) //인코딩을 UTF-8로
            .setMode(HttpMultipartMode.BROWSER_COMPATIBLE);
    builder.addPart("file", new FileBody(uploadFile)); //빌더에 FileBody 객체에 인자로 File 객체를 넣어준다.
    builder.addTextBody("attach_file_seq", attach_file_seq, ContentType.create("Multipart/related", "UTF-8"));
    builder.addTextBody("user_id", user_id, ContentType.create("Multipart/related", "UTF-8"));
    builder.addTextBody("thumbnail", thumbnail, ContentType.create("Multipart/related", "UTF-8"));

    try {
        InputStream inputStream = null;
        HttpClient httpClient = new DefaultHttpClient();
        HttpPost post = new HttpPost(server_domain + "/file/fileUploadDisk.json"); //전송할 URL
        HttpEntity multipart = builder.build();
        post.setEntity(multipart);
        HttpResponse httpResponse = httpClient.execute(post);
        HttpEntity httpEntity = httpResponse.getEntity();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

## 참고링크
- [PostgredSQL 파일전송 링크](https://jacegem.github.io/blog/2018/Spring-PostgreSQL%EC%97%90-%ED%8C%8C%EC%9D%BC-%EC%A0%80%EC%9E%A5%ED%95%98%EA%B3%A0-%EB%B6%88%EB%9F%AC%EC%98%A4%EA%B8%B0/)

- [Create/Write 쉬운 예제](https://homzzang.com/b/java-51)

- [File 제어 클래스&메소드](https://homzzang.com/b/java-50)

- [쿼리로 VO 생성](https://devel-lee.tistory.com/46)
---


### File 클래스
    java.io 패키지 안 File 클래스 가져오면 파일 제어 가능
    import java.io.File; // File 클래스 가져오기
    File myObj = new File("filename.txt"); //파일명지정

### File 클래스 소속 메소드
```
canRead()
=> Boolean 반환 / 파일 읽기 가능 여부 체크
canWrite()
=> Boolean 반환 / 파일이 쓰기 가능 여부 체크
createNewFile()
=> Boolean 반환 / 빈 파일 생성
delete()
=> Boolean 반환 / 파일 삭제
exist()
=> Boolean 반환 / 파일 존재 여부 체크
getName()
=> String 반환 / 파일명 변환
getAbsolutePath()
=> String 반환 / 파일의 절대 경로 반환
length()
=> Long 반환 / 바이트 단위로 파일 크기 반환
list()
=> String[] 반환 / 디렉토리 안 파일을 배열로 반환
mkdir()
=> Boolean 반환 / 디렉토리 생성
```

### Java API의 파일 제어 클래스 종류

 ```
FileReader
BufferedReader
Files
Scanner
FileInputStream
FileWriter
BufferedWriter
FileOutputStream
etc..
```


#### flush()에 대하여...
```
Campbell Ritchie 
Sheriff  


It probably varies from class to class. Here is an example of flush(). What happens is that a buffer might be full, with unread data still in it Flushing that moves all the data out into its destination, emptying the stream or whatever. 

You don't usually have to call flush() if you call close().


Stephan van Hulst 
Bartender  

Here's one concrete example. 
Let's say you have a BufferedOutputStream wrapped around a Socket's OutputStream. You want to send a load of data, but you don't want to close the connection yet. 
After you're done writing everything you want to send, some data may still be in the buffer. Now you will have to flush the buffer, to force it to send all the data.

===============================================================================================

라는데 flush를 호출하면 `강제적`으로 파일에 입력하는 느낌 ...

flush()와 close()를 같이 쓴다고 하는데
사실 close함수가 내부적으로 flush함수를 호출한다고 함.


===> 사용자가 원할 때 버퍼를 비워주는 것으로 버퍼를 비우는 것은 IO에서는 출력하는 것 이겠고, 네트워크에서는 버퍼의 내용을 클라이언트나 서버로 보내는 것이겠지. 여기서 사용자가 원할 때라는 말이 포인트인 것 같다.
```