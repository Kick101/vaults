
__Processes listening on network ports__

```sql
select distinct processes.name, listening_ports.port, listening_ports.address, processes.pid from processes join listening_ports 
on processes.pid = listening_ports.pid;
```

__All processes with outbound connections to nonstandard ports__

```sql
select pos.pid, p.name, pos.local_address, pos.remote_address, pos.local_port, pos.remote_port from process_open_sockets pos 
join processes p on pos.pid = p.pid 
where pos.remote_port not in (80, 443) 
and pos.family = 2 
and pos.local_address not in ("0.0.0.0", "127.0.0.1");
```



