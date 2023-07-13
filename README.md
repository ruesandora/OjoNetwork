<h1 align="center"> Ojo Network </h1>

![image](https://github.com/ruesandora/OjoNetwork/assets/101149671/ce77e3bf-5d7c-4208-86cd-266aec55ef0c)

<h1 align="center"> About Ojo Network incubated by Umee </h1>

<h1 align="center"> Updates and required packages </h1>

```sh
# Update system and install build tools
sudo apt update
sudo apt-get install git curl build-essential make jq gcc snapd chrony lz4 tmux unzip bc -y

# Install Go
rm -rf $HOME/go
sudo rm -rf /usr/local/go
cd $HOME

curl https://dl.google.com/go/go1.20.1.linux-amd64.tar.gz | sudo tar -C/usr/local -zxvf -
cat <<'EOF' >>$HOME/.profile
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
EOF

source $HOME/.profile
go version
```

<h1 align="center"> Installing the node </h1>

```sh
cd $HOME
rm -rf ojo
git clone https://github.com/ojo-network/ojo.git

cd ojo
git checkout v0.1.2

make install
ojod versionon
```

<h1 align="center"> Initalization processes </h1>

```sh
# Replace monikerName with your own nickname.
ojod init monikerName --chain-id=ojo-devnet

# Genesis
curl -Ls https://ss-t.ojo.nodestake.top/genesis.json > $HOME/.ojo/config/genesis.json 

# Addrbook
curl -Ls https://ss-t.ojo.nodestake.top/addrbook.json > $HOME/.ojo/config/addrbook.json
```

<h1 align="center"> Service file creation </h1>

```sh
sudo tee /etc/systemd/system/ojod.service > /dev/null <<EOF
[Unit]
Description=ojod Daemon
After=network-online.target
[Service]
User=$USER
ExecStart=$(which ojod) start
Restart=always
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable ojod
```

<h1 align="center"> Start and monitor your node </h1>

```sh
sudo systemctl restart ojod
journalctl -u ojod -f
```
