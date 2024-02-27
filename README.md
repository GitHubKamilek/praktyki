VPN configurations  

---------- comands ----------------
cd C:\Program Files\OpenVPN\easy-rsa

EasyRSA-Start.bat

./easyrsa init-pki 

./easyrsa build-ca nopass

./easyrsa build-server-full server nopass

./easyrsa build-client-full client1 nopass

./easyrsa gen-dh

exit

openvpn --genkey secret ta.key


---------- server.ovpn ----------------

# Specify a port, a protocol and a device type
port 1194
proto udp
dev tun
# Specify paths to server certificates
ca "C:\\Program Files\\OpenVPN\\easy-rsa\\pki\\ca.crt"
cert "C:\\Program Files\\OpenVPN\\easy-rsa\\pki\\issued\\server.crt"
key "C:\\Program Files\\OpenVPN\\easy-rsa\\pki\\private\\server.key"
dh "C:\\Program Files\\OpenVPN\\easy-rsa\\pki\\dh.pem"
# Specify the settings of the IP network your VPN clients will get their IP addresses from
server 10.24.1.0 255.255.255.0
push "redirect-gateway def1"
# If you want to allow your clients to connect using the same key, enable the duplicate-cn option (not recommended)
# duplicate-cn
# TLS protection
tls-auth "C:\\Program Files\\OpenVPN\\easy-rsa\\pki\\ta.key" 0
cipher AES-256-GCM
# Other options
keepalive 20 60
persist-key
persist-tun
status "C:\\Program Files\\OpenVPN\\log\\status.log"
log "C:\\Program Files\\OpenVPN\\log\\openvpn.log"
verb 3

----------- IPEnableRouter in Registry -----------

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters

------------ client.ovpn --------------

client
dev tun
proto udp
remote 217.97.123.76 1194
resolv-retry infinite
nobind
persist-key
persist-tun
ca ca.crt
cert client1.crt
key client1.key
remote-cert-tls server
tls-auth ta.key 1
cipher AES-256-GCM
connect-retry-max 25
verb 3

--------- Files Directory ---------

C:\Program Files\OpenVPN\easy-rsa\pki\ca.cert
C:\Program Files\OpenVPN\easy-rsa\pki\dh.pem
C:\Program Files\OpenVPN\easy-rsa\pki\ta.key

C:\Program Files\OpenVPN\easy-rsa\pki\issued\client1.crt
C:\Program Files\OpenVPN\easy-rsa\pki\private\client1.key
