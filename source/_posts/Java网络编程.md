---
title: Java网络编程
categories:
- 技术笔记
tags:
- Java
- 网络编程
---

### URL类
1. 构造方法
>直接输入完整地址
>协议+域名+资源地址

2. 获取输入`url.openStream()`返回一个输入流，尽量放到另一个线程

---

### 套接字
-|客户端Socket|服务器端Socket   
|:---:|:---:|:---:|   
-|输入流`getInputString()`|输出流`getOutputString()`
-|输出流`getOutputString()`|输入流`getInputString()`
构造|服务器地址+端口`new Socket(String,int)`|ServerSocket对象用来注册端口号`new ServerSocket(int)` 使用ServerSocket创造Socket对象`serverSocket.accept()`
构造解读|用地址和端口号构造就会向服务器端发送申请，对方相应端口有程序监听的话就会被接收|只要有客户端希望链接就都会被端口监听的程序接收到

- **注意** 尽量把输入流读取信息`in.readUTF()`放在另一个线程中，防止**线程阻塞**
- 获取到的底层流可以用来**构造数据流**，更加方便读写
- 数据流使用方法`readUTF()`读可以方便一些，也可以构造String对象的时候指定`"utf-8"`
- 服务器端先启动，等待客户端请求，服务器端`accept()`方法也会**阻塞线程**

##### 补充
- 端口为0\~65535，其中前1024个被占用0\~1023
- InetAddress类
- - 用静态方法构造`InetAddress.getByName(String s)`
- - 传入域名，toString返回`域名/IP`
- - 传入IP，toString返回`域名/IP`或者`/IP`，因为IP有多个域名的时候不确定用户希望返回哪一个
- 客户端多线程使用Thread，这样每一个申请访问的都可以开辟一个线程，且不会互相影响，如果都使用一个Runnable来创建的话会互相影响

### UDP数据报
>UDP数据报传输更快，但不一定保证能够到达目的地主机，也不能数据保证到达目的地的顺序和发出顺序相同，是一种不可靠的协议，用于能忍受微小错误需要更快传输信息的场景

- **发送端**
  1. **打包数据**
   - `DatagramPacket(byte data[], int length, InetAddress address, int port)`
   - 设置偏移量的字节数组`DatagramPacket(byte data[], int offset, int length, InetAddress address, int port)`
  2. **发送数据包**
   - 无参数构造方法`DatagramSocket mail_out = new DatagramSocket()`
   - 发送数据包`mail_out.send(data_pack)`

- **接收端**
  1. **注册接收端口**
   - `DatagramSocket mail_in = new DataSocket(int port)`
  2. **接收数据包**
   - `DatagramPacket pack = new DatagramPacket(byte data[], int length)`
   - `mail_in.receive(pack)`


**注意事项**

- `receive()`会阻塞线程
- 数据包的数据长度最好不要超过8192KB(8MB)
- 数据包类的常用方法
```java
getPort()//获取发出数据包的远程主机的端口
getLength()//获取数据包的字节长度
getAddress()//获取发出数据包的主机地址
```

---

### 广播数据报
>DatagramSocket只允许数据报发送给指定的目标地址，而MulticastSocket可以将数据报以广播方式发送到数量不等的多个客户端。
>IP协议为多点广播提供了这批特殊的IP地址，这些IP地址的范围是
>**224.0.0.0~239.255.255.255**

关键类：组播套接字类MulticastSocket
MulticastSocket socket

|代码|目的|
|:---:|:---:|
|`int port = 5858`|int型端口|
|`group = InetAddress.getByName("239.255.8.0")`|创建group|
|`new MulticastSocket(port)`|创建组播套接字实例，设置广播端口|
|`socket.setTimeToLive(1)`|设置广播范围为只在本地网络|
|`NetworkInterface.getByInetAddress(group)`|构造NetworkInterface实例|
|`new InetSocketAddress(group,port)`|构造InetSocketAddress实例|
|`socket.joinGroup(socketAddress,networkInterface)`|加入group|
|`socket.send(packet)`|发送数据包|
|`socket.receive(packet)`|接收数据报包|


![](https://img.gejiba.com/images/57cb5b035d22ee0563784757333481c1.png)

代码如下，感兴趣的同学课后自取
**广播端**
**BroadCast.java**
```java
import java.net.*;

public class BroadCast {
    String s = "国庆放假时间是9月30日";
    int port = 5858;
    InetAddress group = null;
    MulticastSocket socket = null;
    BroadCast(){
        try {
            group = InetAddress.getByName("239.255.8.0");
            socket = new MulticastSocket(port);
            socket.setTimeToLive(1);
            InetSocketAddress socketAddress = new InetSocketAddress(group,port);
            NetworkInterface networkInterface = NetworkInterface.getByInetAddress(group);
            socket.joinGroup(socketAddress,networkInterface);
        } catch (Exception e) {
            System.out.println("Error:"+e);
        }
    }
    public void play(){
        while(true){
            try {
                DatagramPacket packet = null;
                byte data[] = s.getBytes();
                packet = new DatagramPacket(data, data.length, group, port);
                System.out.println(new String(data));
                socket.send(packet);
                Thread.sleep(2000);
            }
            catch (Exception e){
                System.out.println("Error:"+e);
            }
        }
    }
    public static void main(String args[]){
        new BroadCast().play();
    }
}
```

**接收端**
**Receiver.java**
```java
import java.net.*;

public class Receiver {
    public static void main(String[] args) {
        int port = 5858;
        InetAddress group = null;
        MulticastSocket socket = null;
        try{
            group = InetAddress.getByName("239.255.8.0");
            socket = new MulticastSocket(port);
            InetSocketAddress socketAddress = new InetSocketAddress(group,port);
            NetworkInterface networkInterface = NetworkInterface.getByInetAddress(group);
            socket.joinGroup(socketAddress,networkInterface);
        }
        catch (Exception e){}
        while(true){
            byte data[] = new byte[8192];
            DatagramPacket packet = null;
            packet = new DatagramPacket(data, data.length, group, port);
            try {
                socket.receive(packet);
                String message = new String(packet.getData(), 0, packet.getLength());
                System.out.println("接收的内容：\n"+message);
            }
            catch (Exception e){}
        }
    }
}
```