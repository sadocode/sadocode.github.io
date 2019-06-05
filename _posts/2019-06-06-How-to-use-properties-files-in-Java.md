---
layout: post
title: "Java properties 사용 예제"
date: 2019-06-06
excerpt: "How to use properties files in Java"
tag:
- Java
- properties
comments: true
---

## properties

.properties는 Java에서 설정값 저장용으로 주로 쓰이는 파일 확장자이다.
.properties 파일의 인코딩은 ISO-8859-1이며, 유니코드로 표현하기 위해서는 \u를 앞에 표기해줘야 한다.
key, value 쌍으로 문자열을 작성하게 되며, 포맷은 다음과 같다.

{% raw %}
key = value
key=value
key:value
key value
# comments
! comments
\ space character
{% endraw %}

## Properties class
Properties 클래스는 Hashtable의 서브클래스이다. key와 value 모두 String 형태로 저장되며, 다음과 같이 사용할 수 있다.

### info.properties 파일
PORT=8070
WEBROOT=C:/webserver
HOME=index.html

### 사용 예제
아래는 webserver 구현을 위한 코드의 일부이다. FileInputStream을 이용해 *info.properties*파일을 읽었고, Properties 클래스를 선언하여 설정값들을 가져왔다. info.properties에서 port, webroot, home의 값을 가져온 예제이다.

~~~ java
import java.io.FileInputStream;
import java.util.Properties;

public class Server{
	private Properties info;
	private int port;
	private String webRoot;
	private String home;

	public void setEnvironment()
	{
		try(FileInputStream fis = new FileInputStream("info.properties"))
		{
			this.info = new Properties();
			this.info.load(fis);
			this.port = Integer.parseInt(info.getProperty("PORT"));
			this.webRoot = info.getProperty("WEBROOT");
			this.home = info.getProperty("HOME");
		} catch(IOException ioe){
			ioe.printStackTrace(System.out);
		}
	}
}
~~~


