### Commands
- [git](#git)
- [postgres](#postgres)
- [scp](#scp)
- [vim](#vim)

### Recipes
- [Safely Remove USB-Attached Storage Device](#safely-remove-usb-attached-storage-device)
- [Generate a self-signed CA and an SSL certificate](#generate-a-self-signed-ca-and-an-ssl-certificate)
- [Setup a Wireguard node](#setup-a-wireguard-node)
- [Setup Wireguard interface as a system service](#setup-wireguard-interface-as-a-system-service)

<br />

## Commands
### git
Create a tag in the current commit:
```shell
git tag -a tag-name -m "Tag description"
```

Move existing tag to current commit (description will not be carried over; must be explicitly included in the command):
```shell
git tag -f tag-name -m "Tag description"
```

Delete a tag:
```shell
get tag -d tag-name
```

Show tag details:
```shell
git show tag-name
```


Push a tag to the remote repository (include `-f` option if the tag already exists):
```shell
git push origin tag-name
```

</br>

### postgres
List all databases:
```shell
\list
```
Use a database for all subsequent queries:
```shell
\c database-name
```
List all tables:
```shell
\dt
```

</br>

### scp
Copy file to remote location:
```shell
scp -i ~/.ssh/id_rsa ~/test.txt remote-user@127.0.0.1:/home/user/test.txt
```

</br>

### vim
Find out which plugin was the last to assign a value to a setting

```vim
:verbose set setting-name?
```

</br>

## Recipes
### Safely Remove USB-Attached Storage Device

```bash
# 1. Identify the device
lsblk

# 2. Unmount the device
udisksctl unmount -b /dev/sda2

# 3. Power-off the device
udisksctl power-off -b /dev/sda2
```

</br>

### Generate a self-signed CA and an SSL certificate

```shell
# 1. Generate a root CA certificate and private key
openssl req -x509 -new – key rsa:2048 -sha256 -nodes -out rootca.crt -keyout rootca.key -days 3650 -subj “/CN=Local CA

# 2. Install the root CA certificate on your system by copying the 'rootca.crt' file to the '/usr/local/share/ca-certificates/' directory and running:
sudo update-ca-certificates

# 3. Generate a certificate signing request (CSR) for your project
openssl req -new -nodes -newkey rsa:2048 -sha256 -keyout project.key -out project.csr -subj "/CN=localhost"

# 4. Sign the CSR with your local CA. This command will sign the CSR (`project.csr`) with the root CA certificate (`rootca.crt`) and private key (`rootca.key`) and generate a signed certificate (`project.crt`) valid for 1 year.
openssl x509 -req -in project.csr -CA rootca.crt -CAkey rootca.key -CAcreateserial -out project.crt -days 365 -sha256
```

</br>

### Setup a Wireguard node
1. Create Wireguard keys
```shell
# Create a new private key
wg genkey >> wg0-private.key

# Create a new public key
wg pubkey < wg0-private.key >> wg0-public.key
```

2. Populate the `/etc/wireguard/wg0.conf` file
```init
[Interface]
PrivateKey = <contents of wg0-private.key>
Address    = 10.0.0.1/32
ListenPort = 51820

PreUp    = sysctl -w net.ipv4.ip_forward=1
PostDown = sysctl -w net.ipv4.ip_forward=0

[Peer]
PublicKey  = <The peer's public key>
AllowedIps = <The peer's Wireguard IP address>
Endpoint   = <The peer's public IP address and port number>
```

3. Start the Wireguard interface
`wg-quick up wg0`

</br>

### Setup Wireguard interface as a system service
Execute the following command:
`systemctl enable --now wg-quick@wg0`

If you have started Wireguard with this config manually before, you need to shut it down first or systemd will not be able to start it!

`wg-quick@wg0` is the name of the systemd unit being enabled and started. It refers to a templated unit called `wg-quick@.service`, which is used by WireGuard's helper script `wg-quick`.

You can find the templated unit in one of the systemd unit directories by executing:
`systemctl cat wg-quick@.service`

You can check the service's status by executing:
`systemctl status wg-quick@wg0`
