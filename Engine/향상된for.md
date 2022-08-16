# 향상된 for문 （；´д｀）ゞ
    for(자료형 변수명 : 배열명){

    }

```java
String[] arr = {"a","b","c","d"};
for(String s : arr){
     System.out.println(s);
}

-------------------------------------------
console
a
b
c
d
```

```java
ArrayList<String> arrlist = new ArrayList<String>();
arrlist.add("1-1");
arrlist.add("1-2");
arrlist.add("1-3");
		
for(String s : arrlist) {
			  
  System.out.println(s);
			  
}

-----------------------------------------
console
1-1
1-2
1-3
```
### 클래스 호출
```java
public class Ex001 {

	private String a;
	private int b;
	private int c;
	
	public String getA() {
		return a;
	}
	public void setA(String a) {
		this.a = a;
	}
	public int getB() {
		return b;
	}
	public void setB(int b) {
		this.b = b;
	}
	public int getC() {
		return c;
	}
	public void setC(int c) {
		this.c = c;
	}
}


ArrayList<Ex001> arrlist = new ArrayList<Ex001>();

Ex001 ex01 = new Ex001();
ex01.setA("a");
ex01.setB(1);
ex01.setC(2);
arrlist.add(ex01);
		
 for(Ex001 s : arrlist) {
			  
 System.out.println(s.getA());
 System.out.println(s.getB());
 System.out.println(s.getC());
			  
}
```
```
console
a
1
2
```