Group
----

```C++
using namespace choco;
```


group 인스턴스의 생성
```C++
group *room = new group();
group *room2 = new group();
```


그룹에 다른 intf::sendable객체를 추가.
( group의 하위에 group도 추가할 수 있다. )
```C++
room->add(client);
room->add(room2);
```


그룹에서 intf::sendable객체 제거
```C++
room->remove(client);
room->remove(room2);
```


브로드캐스팅.
```C++
room->send(
    &pkt, sizeof(dummy_pkt));
```
