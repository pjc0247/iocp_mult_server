Group
----

```C++
using namespace choco;

group *room = new group();
group *room2 = new group();

/* join */
room->add(client);
room->add(room2);

/* leave */
room->remove(client);
room->remove(room2);

/* broadcast */
room->send(
    &pkt, sizeof(dummy_pkt));
```
