HOST = "www.example.com" # host name of your docker server
IP = "192.168.16.38" # ip of your docker server

default:
	openssl genrsa -aes256 -out ca-key.pem 2048
	openssl req -new -x509 -days 365 -key ca-key.pem -sha256 -out ca.pem
	openssl genrsa -out server-key.pem 2048
	openssl req -subj "/CN=${HOST}" -new -key server-key.pem -out server.csr
	echo subjectAltName = IP:${IP},IP:127.0.0.1 > extfile.cnf
	openssl x509 -req -days 365 -in server.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out server-cert.pem -extfile extfile.cnf
	openssl genrsa -out key.pem 2048
	openssl req -subj '/CN=client' -new -key key.pem -out client.csr
	echo extendedKeyUsage = clientAuth > extfile.cnf
	openssl x509 -req -days 365 -in client.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out cert.pem -extfile extfile.cnf
	rm -v client.csr server.csr
	chmod -v 0444 ca.pem server-cert.pem cert.pem

# daemon flags: docker -d --tlsverify --tlscacert=ca.pem --tlscert=server-cert.pem --tlskey=server-key.pem -H=0.0.0.0:2376
# client flags: docker --tlsverify --tlscacert=ca.pem --tlscert=cert.pem --tlskey=key.pem -H=$HOST:2376 version
