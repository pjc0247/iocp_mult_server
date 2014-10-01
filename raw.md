raw
----


```C++
using namespace choco;
```


intf::raw를 이용한 에코 서버
```C++
class raw_test : public intf::raw{
public:
    virtual void on_connected(
        session::conn *client){
    }
    virtual void on_disconnected(
        session::conn *client){
    }
    
    virtual void on_data(
        session::conn *client,
        void *data, int len){
        
        printf("%dbyte(s) recved\n", len);
        printf("%s\n", data);
        
        client->send(data, len);
    }
};
```
