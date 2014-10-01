async
----

```C++
using namespace choco;
```


async를 이용한 비동기 작업 처리.
```C++
class async_test : public intf::handler{
public:
    asnyc_test(){
        /* async작업을 사용하는 핸들러는 반드시
           route_async메소드를 이용해야 한다. */
        route_async<dmy_pkt>(
            __BIND(async_test::on_dmy));
    }
    
    async void on_dmy(
        session::conn *client,
        dmy_pkt *pkt){
        
        int n;
        
        printf("hello world\n");
        
        /* 작업을 비동기로 처리시킨다. */
        n = mt::async<int>(
            [](){
                Sleep(1000);
                return 1234;
            });
            
        /* 이 코드는 위의 작업이 끝나면 실행된다. */
        printf("task done. n is %d\n", n);
    }
};
```

async를 지원하는 기능들
```C++
string body;
body = open_uri_async(
    "http://www.naver.com");

printf("%s\n", body.c_str());
```
```C++
auto result = orm::from("account")
    ->where("id", "pjc0247")
    ->where("password", "asdf")
    ->find_one_async();
```
