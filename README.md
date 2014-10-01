iocp_mult_server
================

asdfsadfs
	
```C++
#include <choco/choco.h>
	
using namespace choco;
```

```C++
enum pkt_id{
    id_keepalive,
  
    id_c2s_login_req,
    id_s2c_login_resp
};

/* KEEPALIVE */
struct c2s_keepalive : packet::header{
    int timestamp;
  
    c2s_keepalive(){
        id = id_keepalive;
    }
};

/* LOGIN */
struct c2s_login_req : packet::header{
    char id[MAX_ID+1];
    char pw[MAX_PW+1];
    
    c2s_login_req(){
        id = id_c2s_login_req;
    }
};
struct s2c_login_resp : packet::header{
    int result;
    
    char nickname[MAX_NICKNAME+1];
    
    s2c_login_resp(){
        id = id_s2c_login_resp;
    }
};
```

```C++
class my_server : public intf::handler{
public:
    my_server(){
        route<c2s_keepalive>(
            _BIND(my_server::on_keepalive));
        
        route_async<c2s_login_req>(
            _BIND(my_server::on_login_req));
    }
    
    virtual void on_connected(
        session::conn *client){
        handler::on_connected(client);
      
        printf("new connection %x\n", client);
    }
    virtual void on_disconnected(
        session::conn *client){
        handler::on_disconnected(client);
      
        printf("disconnected %x\n", client);
    }
    
    /* packet handlers */
    void on_keepalive(
        session::conn *client,
        c2s_keepalive *pkt){
      
    }
    async void on_login_req(
        session::conn *client,
        c2s_login_req *pkt){
        
        auto result = orm::from("account")
            ->where("id", pkt->id)
            ->where("pw", pkt->pw)
            ->find_one_async();
          
        if(result != nullptr){
            send_login_resp(
                client,
                result->get("nickname"),
                true);
        }
        else{
            send_login_resp(
                client,
                "", false);
        }
    }
    
    void send_login_resp(
        intf::sendable *to,
        const std::string &id,
        int result){
        
        s2c_login_resp resp;
        strncpy(resp.id, id.c_str(), MAX_ID);
        resp.result = result;
        to->send(&resp, sizeof(s2c_login_resp));
    }
};
```

```C++
int main(int argc,char **argv){
    auto handler = 
        new my_server();
    auto server =
        new server(handler);
      
    server->open(
        "0.0.0.0", 9916);
    
    while(true){
        printf("press \'q\' to close server.\n");
      
        int ch = getch();
      
        if(ch == 'q')
            server->close();
    }
    
    return 0;
}
```
