# Gonka

Gonka is a decentralized AI infrastructure designed to optimize computational power for AI model training and inference, offering an alternative to monopolistic, high-cost, centralized cloud providers. As AI models become increasingly complex, their computational demands surge, presenting significant challenges for developers and businesses that rely on costly, centralized resources.


#### Install Docker

```sh

```

#### Install Nvidia

```sh
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | \
sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg
```

```sh
curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```

```sh
sudo apt update
sudo apt install -y nvidia-container-toolkit
sudo nvidia-ctk runtime configure --runtime=docker
```

Restart docker

```sh
sudo systemctl restart docker
```

#### download inference

```sh
curl -L "https://github.com/gonka-ai/gonka/releases/download/release%2Fv0.2.5/inferenced-linux-amd64.zip" -o inferenced-linux-amd64.zip
apt update && apt install -y unzip
unzip inferenced-linux-amd64.zip
```

Give Permission

```sh
chmod +x inferenced
./inferenced --help
```

#### Create Gonka Wallet

```sh
./inferenced keys add gonka-account-key --keyring-backend file
```

You'll be asked for password, create your password and re enter.
and then you'll see your gonka address and 24 memonic phrase

#### Clone repositori
```sh
git clone https://github.com/gonka-ai/gonka.git -b main && \
cd gonka/deploy/join
```

mkdir -p /mnt/shared

Copy Template

```sh
cp config.env.template config.env
```

```sh
nano config.env
```

Then you'll see...

```bash
export KEY_NAME=nodename
export KEYRING_PASSWORD=your-password
export PUBLIC_URL=http://your-ip:8000
export P2P_EXTERNAL_ADDRESS=tcp://your-ip:5000
export ACCOUNT_PUBKEY=your-pubkey
```

and only change this part : 

- nodename
- your-password
- your-ip
- your-pubkey

eddit cencored with yours

Reload shell

```
source config.env
```

#### Download model

```sh
mkdir -p $HF_HOME
sudo apt update && sudo apt install -y python3-pip
sudo apt install -y pipx
pipx install huggingface_hub[cli]
pipx ensurepath
pipx install --force huggingface_hub[cli]
export PATH="$HOME/.local/bin:$PATH" && which hf
export PATH="$HOME/.local/bin:$PATH"
hf download Qwen/Qwen2.5-7B-Instruct
```

#### Reload Shell

```
source config.env
```

#### Run Node

1. Run full node

```bash
docker compose -f docker-compose.yml -f docker-compose.mlnode.yml pull
```

2. Start Initial Server

```bash
source config.env && docker compose up tmkms node -d --no-deps
```

3. Register your host

```bash
inferenced register-new-participant \
    $DAPI_API__PUBLIC_URL \
    $ACCOUNT_PUBKEY \
    --node-address $DAPI_CHAIN_NODE__SEED_API_URL
```

Enter

```
exit
```

after register
