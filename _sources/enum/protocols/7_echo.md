# Echo

This is a fun one and as far as I can tell, other than causing DoS (denial of service) no real way to exploit it to do anything other than what it is intended for.

It's quite unique in that it runs on both TCP and UDP, it's basic functionality is exactly what it sounds like; you send it something, and the server will `echo` it back at you. It's designed for testing and measuring packet loss and other such things.

```bash
nc -uvn <IP> 7
Hello echo # our command
Hello echo # servers response
```

If we want to cause the DoS then we have to connect the Echo service to another Echo service and this will create an excessively high number of packets and likely take out one or both of the machines. Not something we really want to do on a pentest, but it's worth noting so we don't ever do it by mistake.
 