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
mkdir ovpn
cd ovpn
wget https://git.io/vpn -O openvpn-install.sh
chmod +x openvpn-install.sh
sudo ./openvpn-install.sh
sudo mv /root/client.ovpn ./
exit
```

7. And again from the local terminal:

```sh
scp -i "<PrivateKeyFileName>.pem" ubuntu@<PublicDnsName>:~/ovpn/client.ovpn ./
```

8. This will copy the `client.ovpn` file to the local computer so it can be used by a OpenVPN client.

## Reference

* https://github.com/Nyr/openvpn-install
