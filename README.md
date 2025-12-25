# Gonka

Gonka is a decentralized AI infrastructure designed to optimize computational power for AI model training and inference, offering an alternative to monopolistic, high-cost, centralized cloud providers. As AI models become increasingly complex, their computational demands surge, presenting significant challenges for developers and businesses that rely on costly, centralized resources.

Make sure to rent Device with VM OS, you can rent on with this link.

- Vast.ai : select ubuntu 22 VM.
- Spheron.
- RunPod.
- TensorDock.

Specifications for this model :

- Storage : 200 GB recommended.

- 1X RTX 3090 (2X RTX 3090 recommended).
- 1X RTX 4090 (2X RTX 4090 recommended).
- 1X RTX 5090 (2X RTX 5090 recommended).
- 1X A100.
- 1X H100.

## Installation

### Install Dependencies

```bash
sudo apt update && sudo apt upgrade -y
```

```bash
sudo apt install screen curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```

### Install Docker
```bash
sudo apt update -y && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo docker run hello-world
```

### Install Nvidia

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

### Download inference

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

### Create Gonka Wallet

Note : If you allready create wallet no need to create again next time, you can skip this and go to next step.

```sh
./inferenced keys add gonka-account-key --keyring-backend file
```

You'll be asked for password, create your password and re enter.
and then you'll see your gonka address, public key, and 24 memonic phrase.

![Image](https://drive.google.com/uc?export=view&id=1iJT2W3V0wdbbrwoRfsOlMb3je03PaZoQ)

### Clone repositori
```sh
git clone https://github.com/gonka-ai/gonka.git -b main && \
cd gonka/deploy/join
```

```sh
mkdir -p /mnt/shared
```

Copy Template

```sh
cp config.env.template config.env
```

Edit template

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

### Download model

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

### Reload Shell

```
source config.env
```

### Run Node

1. Run full node

```bash
docker compose -f docker-compose.yml -f docker-compose.mlnode.yml pull
```

Wait untill you see all marked, like this image

![Image](https://drive.google.com/uc?export=view&id=1IoMwtvEIDHcEnD5ZE6FUHdWu8U1GW9_d)

Then press CTRL + C, then run next command...

2. Start Initial Server

```bash
source config.env && docker compose up tmkms node -d --no-deps
```


3. Register your host

Run container
```bash
docker compose run --rm --no-deps -it api /bin/sh
```
Paste this inside container

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

after register your host :

- Open your browser.
- Visit : http://node2.gonka.ai:8000/v1/participants/your-gonka-address
- Replace your-gonka-address with yours.
- make sure to see on site "{"pubkey":"your-pubkey"}"

Check your address transaction on Gonka block explorer

- visit : http://node2.gonka.ai:8000/dashboard/gonka/
- paste your gonka address.
- then check your tx.

![Image](https://drive.google.com/uc?export=view&id=1MZzI_80cairjwQAdjv9YoZQtkOjpp7DE)

### Key command
Check node logs

```
docker compose logs tmkms node -f
```

Stop node

```
docker compose down
```



