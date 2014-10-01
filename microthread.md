microthread
----

```C++
using namespace choco;
```


microthread인스턴스 생성
```C++
void task(
    mt::microthread *m){
    
    printf("hello world\n");
    
    m->yield();
}

auto m =
    mt::microthread(task);
```


schedule
```C++
m.schedule();
```


yield
```
m.yield();
```
