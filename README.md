# Load Balancer
> **Load balancing** refers to efficiently distributing incoming network traffic across a group of backend servers. Basically tracks amount of traffic on all servers and then decides to which server it should route the request based on traffic on each server instance. It's the middleware between server and client request.

> If a single server goes down, the load balancer redirects traffic to the remaining online servers. When a new server is added to the server group, the load balancer automatically starts to send requests to it.

![load balancing diagram NGINX](https://i.imgur.com/7GOsEui.png)
- NGINX

> I'm using round robin algorithm to create the low level version of NGINX. 


### What happens is: 
> When a user makes a request, it goes to the load balancer but not server. Thus, the load balancer makes decision. Depending upon the amount of loads, it forwards the request to the befitting server instance.

> Some of the load balancers has reverse proxy where a server could act as a client to one of the server itself. Instead of rewriting reverse proxy from scratch, I'm using a package called "http util". Reverse proxy server basically redirects the request and hides location of the server from the client.

> Here, client makes a request to reverse proxy and it'll redirect to a server.

### How the program is written?
> Load balancer is implemented as a struct containing port, servers and round robin count. The struct has the initializer function which creates new load balancer. The server struct contains an address and a proxy. It also has an initializer function which creates a new server. There are two methods: one to get address of a server and another, to get if the server is alive. Load balancer keeps rounding, checking which server is alive. The program has also server interface which contains three methods: address(), isAlive() & serve().

>The main function first creates server list and then calls a function called server proxy function. It calls get next available server function. That's it. The load balancer calls get next available server function and checks if the server is alive, jumps to next server using round robin algorithm.

***

### To run the program:
<ul>
  <li>
    <span style="color: #f03c15;"> Make sure go is installed on your machine. https://go.dev/dl/ </span> 
  </li>
</ul>

```go
$ go run main.go
```

When it runs, the http server is started at port: 8000

![http server running on port 8000 load balancer](https://i.imgur.com/AMupICT.png)

After that, run the localhost on web and each time you reload, the server keeps routing the provided url by the load balancer. Eventually it'll load one of the included url.

*** 

![request 1: http server running on port 8000 load balancer](https://i.imgur.com/MJ9BMEt.png)

***

![request 2: http server running on port 8000 load balancer](https://i.imgur.com/SPXtp2H.png)

***

![request 3: http server running on port 8000 load balancer](https://i.imgur.com/MRhPj6o.png)

***

![many request: http server running on port 8000 load balancer](https://i.imgur.com/kNEgrQy.png)

![response success https://www.google.com : (client reuqested http server at port 8000 by load balancer](https://i.imgur.com/JnbZlmV.png)

end;
