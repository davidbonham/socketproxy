# socketproxy

Sit on a socket connection between a client and a server tracing the
the contents of the packets they contain. For example, if we are running
an HTTP server on port 8000 on the local host and we type
 
$ socketproxy --port 5000 localhost:8000

we can then connect to port 5000 from another terminal window and perform 
a GET like this:

$ telnet localhost 5000
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
GET /
(HTTP response here)
Connection closed by foreign host
$ 

and the socketproxy terminal will display the traffic that was exchanged.

If we capture the packet exchange like this:

$ socketproxy --port 5000 localhost:8000 --record packets.cap

We can then replay the exchange taking the part of one of the participants.
To replay the client's packets to a live server:

$ socketproxy --asclient=packets.cap localhost:8080

To replay the server's responses to a live client:

$ socketproxy --asserver=packets.cap --port=5000 dummy
