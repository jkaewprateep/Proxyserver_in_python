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
🐑💬 ➰ This code supports many protocols because it works atthe  socket layer, HTTP, Winsock, wscp, TCP, UDP, and printout log for monitoring. By request you can filter of incoming messages by rules as desired from your Python programming language and map to the target destination, moreover, you can evaluate incoming messages by statistics and treads them with suitable action performed. One method to find third-party communication messages and deny or block them without access to the  server in the backend system, access list, and dynamic access list can be developed from the endpoint and table access list because even if they use the same IP address they will need intentional action such as gathering service information should perform by sequence and some information is unique identification.<br>   
🦭💬 Unique identification information such as client id, robot id, and asset ID cannot be gathered in one place but a sequence of information requests and correct response order need to be performed. You can develop a dynamic access list at the proxy gateway. Still, it needs to be a powerful machine with advanced searching algorithms where information is ready development can be performed even AI from data input. </br>

🐯💬 Culture-INFO, in the implementation and development plan administrator can provide full access to development servers with list and authentication but in UAT and production server implementer can only access to gateway server with full authorization to create and update the access with no reason to block or limit the access because it can be dedicated server with load-balancing and project dedication server in the virtual machine environment. </br>
🦁💬 In This implementing scenario in preventive maintenance you need to respond to the gateway server too but it is a fast implementation a method with provided log monitoring and alert tool same as the dinosaur or 3D log daemon tool or propritary log can installed on this server it will require access right permission if contain sensitive data. There are internal and external log in practicals but we see they are the same logging to export them should be performed extraction information that will replace the address and sensitive data fromthe  exporting log. In the support side they only require network topology does not require real-ip address even it has but by priority of customer sensitive data information all transferring or communication is on secured FTP communication only to show the intention of the administrator with manual or automatic task but the company supports this methodology will provide you disk space and long encryption password do not forget required only network tropology you can replace all IP-address information. </br>

## Codings exameple
🐐💬 It is easy to trap one message and repeating in the network but it is not easy to have all the information requirements from the network when you do not know their IP address or simulation of the message events because the IP address here, port and sequence can development dynamic or Python codes. Once I had a request for the same information I can have some service output created but it will leaved remain inside queue because it is not created by the service order or does not contain valid information for the target service and development will clear out this from queue and develop new access list rules. <//br>
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
