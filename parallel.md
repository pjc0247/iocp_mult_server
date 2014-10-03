parallel
----
```C++
using namespace choco;
```

parallel::async를 이용하여 작업을 스레드 풀에서 실행시킬 수 있다.
```C++
parallel::async(
  [](){
    printf("async task\n");
  });
```
```C++
auto f = parallel::async<int>(
  [](){
    Sleep(1000);
    return 1234;
  });
  
printf("result : %d\n", f.get());
```
