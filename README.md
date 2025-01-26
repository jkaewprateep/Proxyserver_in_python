# Proxyserver_in_python
Proxy server in Python

<p align="center" width="100%">
    <img width="30%" src="https://github.com/jkaewprateep/Proxyserver_in_python/blob/main/python-logo.jpg">
    <img width="25%" src="https://github.com/jkaewprateep/Proxyserver_in_python/blob/main/proxy.jpg">
    <img width="13%" src="https://github.com/jkaewprateep/Proxyserver_in_python/blob/main/girl-with-shirt-that-says-shes-girl_911060-82180.png">
    <img width="16%" src="https://github.com/jkaewprateep/Proxyserver_in_python/blob/main/image14.jpg"> </br>
    <b> Proxy server in Python mixro-services communications </b> </br>
    <b> ( Picture from Internet ) </b> </br>
</p>

🧸💬 Communication development in micro-services allowed the deployment of a proxy-load balance mechanism that creates an efficient way of communication, all information needs to route to a specific endpoint with correct order and monitoring, in our example, it implements an access list by their request allowance to access the correct service without know of path and IP-address when implementing translators as commercial proxy server. <br>

## Codings exameple
```
import socket

import threading

def access_list( host, port ):

    if host == "172.17.100.56" and port == 8080 :
        return True;
    elif host == "172.17.100.56" and port == 8081 :
        return True;
    elif host == "172.17.100.56" and port == 8082 :
        return True;
    elif host == "172.17.100.56" and port == 8083 :
        return True;
    elif host == "172.17.100.56" and port == 8084 :
        return True;
    elif host == "172.17.100.56" and port == 8085 :
        return True;

    elif host == "robottrading-backend" and port == 8080 :
        return True;
    elif host == "robottrading-backend" and port == 8081 :
        return True;
    elif host == "robottrading-backend" and port == 8082 :
        return True;
    elif host == "robottrading-backend" and port == 8083 :
        return True;
    elif host == "robottrading-backend" and port == 8084 :
        return True;
    elif host == "robottrading-backend" and port == 8085 :
        return True;

    

    elif host == "paper-api.alpaca.markets" and port == 443 :
        return True;

    return False;

def handle_client_request(client_socket):

    print("Received request:\n")

    # read the data sent by the client in the request

    request = b''

    client_socket.setblocking(False)

    while True:

        try:

            # receive data from web server

            data = client_socket.recv(1024)

            request = request + data

            # Receive data from the original destination server

            print(f"{data.decode('utf-8')}")

        except:

            break

    # extract the webserver's host and port from the request

    host, port = extract_host_port_from_request(request)

    print("==============================>", host, port)

    ## add
    if access_list(host, port) :

        # create a socket to connect to the original destination server

        destination_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

        try :
            # connect to the destination server

            destination_socket.connect((host, port))

            # send the original request

            destination_socket.sendall(request)

            # read the data received from the server

            # once chunk at a time and send it to the client

            print("Received response:\n")

        except :
            pass;

        while True:

            try :
                # receive data from web server

                data = destination_socket.recv(1024)

                # Receive data from the original destination server

                print(f"{data.decode('utf-8')}")

                # no more data to send

                if len(data) > 0:

                    # send back to the client

                    client_socket.sendall(data)

                else:

                    break
            except :
                break;

        # close the sockets

        destination_socket.close()

        client_socket.close()

def extract_host_port_from_request(request):

    # get the value after the "Host:" string

    host_string_start = request.find(b'Host: ') + len(b'Host: ')

    host_string_end = request.find(b'\r\n', host_string_start)

    host_string = request[host_string_start:host_string_end].decode('utf-8')

    webserver_pos = host_string.find("/")

    if webserver_pos == -1:

        webserver_pos = len(host_string)

    # if there is a specific port

    port_pos = host_string.find(":")

    # no port specified

    if port_pos == -1 or webserver_pos < port_pos:

        # default port

        port = 80

        host = host_string[:webserver_pos]

    else:

        # extract the specific port from the host string

        port = (host_string[(port_pos + 1):])[:webserver_pos - port_pos - 1]
        # invalid literal for int() with base 10: '443 HTTP'
        port = port.replace(" HTTP", "");
        port = int(port);

        host = host_string[:port_pos]
        host = host.replace("CT ", "");

    return host, port

def start_proxy_server():

    port = 8888

    # bind the proxy server to a specific address and port

    server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

    server.bind(('127.0.0.1', port))

    # accept up to 10 simultaneous connections

    server.listen(10)

    print(f"Proxy server listening on port {port}...")

    # listen for incoming requests

    while True:

        client_socket, addr = server.accept()

        print(f"Accepted connection from {addr[0]}:{addr[1]}")

        # create a thread to handle the client request

        client_handler = threading.Thread(target=handle_client_request, args=(client_socket,))

        client_handler.start()

if __name__ == "__main__":

    start_proxy_server()
```
