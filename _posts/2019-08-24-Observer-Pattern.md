---
layout:	post
title: 	"Observer Design Pattern"
date:	2019-08-20
excerpt:	"Observer Design Pattern in Java”
tag:
- Java
- Observer
- Design Pattern
comments:	true
---

## 1. 디자인 패턴
(뉴비인 내가 느끼기에) 디자인 패턴은 코딩에 도움이 되는 프로그램 설계 구조인 것 같다. 코딩을 시작할 때, 막연하게 느껴지는 부분을 적절한 디자인 패턴을 사용함으로써 쉽게 할 수 있게 도움을 주는 것 같다. 디자인 패턴에 대해 부정적인 사람도 있고, 긍정적인 사람도 있는데 확실한 것은 뉴비들에게는 도움이 된다는 것이다. 
<br>
 디자인 패턴을 너무 과용한다면, 생각없이 짠 코드가 되기 싶다. 반대로 디자인 패턴을 아예 무시하고 코딩한다면, 다른 사람과의 협업에 이점이 적을 것 같다.
<br>

## 2. 옵저버 패턴
옵저버 패턴은 간단하게 설명하자면, 한 객체의 변화하는 값을 다른 여러 객체가 쉽게 받게 하는 데에 있다. 즉, 한 객체의 상태 변화에 따라 다른 객체의 상태도 연동되도록 하는 일대다의 객체 의존관계를 구성하는 패턴이다. 
<br>
그런데 옵저버 패턴은 의존관계를 구성하는 객체들이 *간접적인* 연관관계를 갖게 하는 데에 의미가 있다. 다시 말해서, 다른 객체의 영향을 받는다고 해서 직접적으로 영향을 받는 게 아니라 인터페이스를 통한 간접적인 영향을 받는다는 말이다.
<br>

## 3. 예시
옵저버 패턴은 크게 데이터의 변경을 통보해주는 인터페이스와 데이터의 변경을 통보받는 인터페이스로 구성된다. 여기서는 **Subject** 인터페이스가 데이터의 변경을 통보하는 인터페이스이고, **Observer** 인터페이스가 데이터의 변경을 통보받는 인터페이스이다. 
그리고 Subject 인터페이스를 구현하는 **Memo** 클래스와 Observer 인터페이스를 구현하는 **GUI**, **Notepad** 클래스를 정의하였다.
<br>
Memo의 데이터 변경을 GUI, Notepad 클래스가 통보를 받는데,  Memo와 GUI, Notepad 는 인터페이스를 통해 데이터의 변경을 통보하고 받기 때문에, **직접적인 의존관계를 갖지 않는다**.
<br>
아래의 코드는 App클래스에서 System.in으로 입력받은 값을 Memo 클래스의 message에 저장하게 되고, 변경되는 message의 값들을 GUI, Notepad클래스로 통보해준다. 데이터의 변경을 통보받은 GUI에서는 JFrame으로 출력해주고, Notepad에서는 txt파일로 저장해준다.
<br>

## 4. UML

### 4.1 Subject 인터페이스

![](/images/observer/Subject.png)
<br>

### 4.2 Observer 인터페이스

![](/images/observer/Observer.png)
<br>

## 5. 코드

### 5.1 Subject 인터페이스

~~~ java
public interface Subject {
	public void addObserver(Observer observer);
	public void removeObserver(Observer observer);
	public void notifyObserver();
	public void write(String message);
}
~~~
<br>

### 5.2 Observer 인터페이스

~~~ java
public interface Observer {
        public void update(String message);
}
~~~
<br>

### 5.3 Memo 클래스

~~~ java
import java.util.LinkedList;
import java.util.List;

public class Memo implements Subject{
        private List<Observer> observerList;
        private String message;

        public Memo()
        {
                this.init();
        }
        private void init()
        {
                this.observerList = new LinkedList<Observer>();
        }
        public void write(String message)
        {
                if(message == null)
                        throw new java.lang.NullPointerException();
                this.message = message;
        }
        public void addObserver(Observer observer)
        {
                if(observer == null)
                        throw new java.lang.NullPointerException();
                if(!this.observerList.contains(observer))
                        this.observerList.add(observer);
        }
        public void removeObserver(Observer observer)
        {
                if(observer == null)
                        throw new java.lang.NullPointerException();
                if(this.observerList.contains(observer))
                        this.observerList.remove(observer);
        }
        public void notifyObserver()
        {
                int length = this.observerList.size();

                for(int I = 0; I < length; I++)
                {
                        this.observerList.get(i).update(this.message);
                }
        }
}
~~~
<br>

### 5.4 GUI 클래스

~~~ java
import javax.swing.JFrame;
import javax.swing.JScrollPane;
import javax.swing.JTextArea;

public class GUI implements Observer{
        private JFrame jFrame;
        private JScrollPane scroll;
        private JTextArea messageArea;

        public GUI()
        {
                this.init();
        }
        private void init()
        {
                this.showFrame();
        }
        private void showFrame()
        {
                this.jFrame = new JFrame(“GUI”);
                this.jFrame.setSize(300, 300);
                this.messageArea = new JTextArea();
                this.messageArea.setEditable(false);
                this.scroll = new JScrollPane(this.messageArea);
                this.jFrame.add(this.scroll);
                this.jFrame.setVisible(true);
                //this.jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        }
        public void update(String message)
        {
                if(message == null)
                        throw new java.lang.NullPointerException();
                if(“exit”.equals(message))
                {
                        this.closeGUI();
                        return;
                }
                this.appendMessage(message);
        }
        private void closeGUI()
        {
                this.jFrame.dispose();
        }
        private void appendMessage(String message)
        {
                this.messageArea.append(message + “\n”);
        }
}
~~~
<br>

### 5.5 Notepad 클래스

~~~ java
import java.io.FileWriter;
import java.io.IOException;

public class Notepad implements Observer{
        private FileWriter fw;
        private static final String FILE_NAME = “memo.txt”;

        public Notepad()
        {
                this.init();
        }
        private void init()
        {
                this.openFile();
        }
        private void openFile()
        {
                try
                {
                        this.fw = new FileWriter(FILE_NAME, true);
                }
                catch(IOException ioe)
                {
                        ioe.printStackTrace(System.out);
                }
        }
        public void update(String message)
        {
                if(message == null)
                        throw new java.lang.NullPointerException();
                if(message.equals(“exit”))
                {
                        this.closeFile();
                        return;
                }
                this.writeFile(message);
        }
        private void writeFile(String message)
        {
                try
                {
                        this.fw.write(message + “\r\n”);
                }
                catch(IOException ioe)
                {
                        ioe.printStackTrace(System.out);
                }
        }
        private void closeFile()
        {
                try
                {
                        this.fw.close();
                }
                catch(IOException ioe)
                {
                        ioe.printStackTrace(System.out);
                }
                finally
                {
                        try
                        {
                                if(this.fw != null)
                                        this.fw = null;
                        }
                        catch(Exception e)
                        {
                                e.printStackTrace(System.out);
                        }
                }
        }
}
~~~
<br>

### 5.6 App 클래스(main)

~~~ java
import java.util.Scanner;
  
public class App {
        public static void main(String[] args)
        {
                Subject memo = new Memo();
                Observer guiViewer = new GUI();
                Observer notepad = new Notepad();

                memo.addObserver(guiViewer);
                memo.addObserver(notepad);

                Scanner scanner = new Scanner(System.in);
                String message = null;

                while(!Thread.currentThread().interrupted())
                {
                        message = scanner.nextLine();
                        memo.write(message);
                        memo.notifyObserver();

                        if(“exit”.equals(message))
                        {
                                scanner.close();
                                break;
                        }
                }
        }
}
~~~
<br>

## 6. 구현 

터미널에 입력한 값이 각각 Java Application 창으로 뜨는 것과 memo.txt 파일로 저장되는 것을 확인할 수 있다.

![](/images/observer/ex1.png)
<br>
![](/images/observer/ex2.png)

