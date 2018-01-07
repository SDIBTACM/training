# 优先队列
```cpp 
头文件:   #include<functional>
         #include<queue>

```

```cpp
实例化:   priority_queue<int,vector<int>,less<int> >que;
         priority_queue<int,vector<int>,greater<int> >que;
         
```

```cpp
自定义优先级：(1)struct cmp{
                bool operater()(int &a,int &b){
                  return a>b; //最小值优先,若改成a<b，则是最大值优先
               }
               priority_queue<int,vector<int>,cmp>que;
               
            (2)struct qu{
                int x;
                bool operater < (const qu &a) const{
                      return x>a.x;//最小值优先，反之最大值优先
                      }
                }
                priorith_queue<qu> que;
```



# 创建简单服务器和客户端
```py


（1）服务器
    import socket   #嵌套字模块
    HOST=''   #IP
    PORT=10888  #端口
    s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)    #IPv4方式和TCP协议，SOCK_DGRAM为UDP
    s.bind((HOST,PORT))   #关联服务器
    s.listen(1)     #最大连接数
    conn,addr=s.accept()  #接收
    print('Client\'s Address:',addr)
    while True:
        data = conn.recv(1024)  #接收信息，开辟缓冲区
        if not data:
            break
        print("Receive Data:",data.decode('utf-8'))   #编码方式
        conn.send(data)     #发送信息
    conn.close()      #关闭

（2）客户端

    import socket
    HOST='localhost' #要连接的服务器地址
    PORT=10888
    s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
    s.connect((HOST,PORT))    #连接
    data="你好!"
    while data:
        s.sendall(data.encode('utf-8'))   #发送全部数据
        data=s.recv(512)
        print("Receive from server:\n",data.decode('utf-8'))    #解码
        data=input('please input a info:\n')
    s.close()

```
