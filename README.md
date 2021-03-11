# DDog
DDog is yet another CLI tool written in Java to check connectivity towards the Datadog SaaS.

## Commands
Those are the commands that can be used:
* ping
* send-metric
* send-trace
* send-log

There is also a CLI help online:
```bash
./ddog
Missing required subcommand
Usage: ddog [COMMAND]
Used to check connectivity between servers.
Commands:
  send-metric  Send a metric.
  send-trace   Send a trace.
  send-log     Send a log event.
  ping         Used to check connectivity between servers.
```


## Ping
This command should be used to check in the on-premise environment if there are connectivity problems between servers (firewalls, etc).
The Ping command behaves like an echo server, where the _client-ping_ sends a text message to the _server-ping_ which in turn responds by reversing the text message.

### Hep online
```bash
./ddog ping -h
Usage: ddog ping [-hV] [--verbose] [--bind-address=<bindAddress>]
                 [--bind-port=<bindPort>] --message=<message> --mode=<mode>
                 [--target-host=<targetHost>] [--target-port=<targetPort>]
Used to check connectivity between servers.
      --bind-address=<bindAddress>
                            Bind address. Default is 0.0.0.0.
      --bind-port=<bindPort>
                            Server port. Default is 4405.
  -h, --help                Show this help message and exit.
      --message=<message>   The message to send over the wire.
      --mode=<mode>         The mode to be used: client or server.
      --target-host=<targetHost>
                            Target host:port pairs. Default is localhost.
      --target-port=<targetPort>
                            Target port. Default is 4405.
  -V, --version             Print version information and exit.
      --verbose             Verbose output of the command. Default is false.
```

### Client mode
The command for the Client mode is the following:
```bash
./ddog ping --mode=client --target-host=127.0.0.1 --target-port=4405 --message=ciao --verbose=true
```
Where:
* target-host must match the interface bound by the _server-ping_ instance.
* target-port must match the bind port of the _server-ping_ instance.

### Server mode
The command for the Server mode is the following:
```bash
./ddog ping --mode=server --bind-address=0.0.0.0 --bind-port=4405 --verbose=true
```
Where:
* bind-address matches an IP of one the available interface. Default is AnyInterface, which isolved by the IP 0.0.0.0.
* bind-port matches the port opened by the UDP socket.

### Example
#### Server instance
The command for the Server mode is the following:
```bash
./ddog ping --mode=server --bind-address=0.0.0.0 --bind-port=4405 --verbose=true
Parameters
        message: null
        mode: server
        bind-address: 0.0.0.0
        bind-port: 4405
        target-host: localhost
        target-port: 4405
        verbose: true
Server received message ciao
Server sending reverse response oaic
```

#### Client instance
```bash
./ddog ping --mode=client --target-host=127.0.0.1 --target-port=4405 --message=ciao --verbose=true
Parameters
        message: ciao
        mode: client
        bind-address: 0.0.0.0
        bind-port: 4405
        target-host: 127.0.0.1
        target-port: 4405
        verbose: true
Client sending message ciao to server
Client received response oaic
```

