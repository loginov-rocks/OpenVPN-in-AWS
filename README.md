# OpenVPN in AWS

1. Run CloudFormation stack
2. Go to the stack *Outputs*, note `PrivateKeyFileName`, `PrivateKeyParameterName`, and `PublicDnsName`
3. Go to *Systems Manager* -> *Parameter Store* -> `PrivateKeyParameterName`
4. Click *Show* under *Value* and copy contents to a locale file named `PrivateKeyFileName`
5. Run from a local terminal:

```sh
chmod 400 <PrivateKeyFileName>.pem
ssh -i "<PrivateKeyFileName>.pem" ubuntu@<PublicDnsName>
```

6. This will log into the EC2 instance, then:

```sh
sudo apt update
sudo apt upgrade -y
mkdir openvpn
cd openvpn
wget https://git.io/vpn -O openvpn-install.sh
chmod +x openvpn-install.sh
sudo ./openvpn-install.sh
sudo mv /root/<ClientName>.ovpn ./
exit
```

When installing openvpn, choose
1) protocol: `UDP` or `TCP`, should match what's in the template
2) Port: `1194` by default, should match what's in the template
3) DNS servers: leave default
4) client name: `client` by default

5. And again from the local terminal:

```sh
scp -i "<PrivateKeyFileName>.pem" ubuntu@<PublicDnsName>:~/openvpn/<ClientName>.ovpn ./
```

8. This will copy the `<ClientName>.ovpn` file to the local computer so it can be used by a OpenVPN client.

To add new client use `sudo ./openvpn-install.sh` and follow instructions, then copy file to the local computer
as in the last step.

## Reference

* https://github.com/Nyr/openvpn-install
