# Gonka

Gonka is a decentralized AI infrastructure designed to optimize computational power for AI model training and inference, offering an alternative to monopolistic, high-cost, centralized cloud providers. As AI models become increasingly complex, their computational demands surge, presenting significant challenges for developers and businesses that rely on costly, centralized resources.


#### Download Docker

```bash
sudo apt update -y
sudo apt upgrade -y
sudo apt install git docker.io -y
sudo apt install docker-compose -y
```
Check Docker version

```bash
docker --version
```


#### download inference

```bash
curl -L "https://github.com/gonka-ai/gonka/releases/download/release%2Fv0.2.5/inferenced-linux-amd64.zip" -o inferenced-linux-amd64.zip
apt update && apt install -y unzip
unzip inferenced-linux-amd64.zip
```

Give Permission

```bash
chmod +x inferenced
./inferenced --help
```

#### Create Gonka Wallet

```bash
./inferenced keys add gonka-account-key --keyring-backend file
```

You'll be asked for password, create your password and re enter.
and then you'll see your gonka address and 24 memonic phrase

#### Clone repositori
```
git clone https://github.com/gonka-ai/gonka.git -b main && \
cd gonka/deploy/join
```

mkdir -p /mnt/shared

Copy Template

```bash
cp config.env.template config.env
```

```bash
nano config.env
```

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

docker compose -f docker-compose.yml -f docker-compose.mlnode.yml pull




