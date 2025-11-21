🚀 Gensyn Node Quick-Start Scripts
Run these four blocks sequentially in your VPS terminal.

Block 1: Install Dependencies & Start Screen (Linux/WSL)

This block updates your system, installs Python, tools, Node.js, Yarn, and starts a screen session named gensyn to keep the node running in the background.


# Update system and install base tools
```Bash
sudo apt update && sudo apt install -y python3 python3-venv python3-pip curl wget screen git lsof
```
# Install Node.js v20.x
```Bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash - && sudo apt update && sudo apt install -y nodejs
```
# Install Yarn
```Bash
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list > /dev/null
sudo apt update && sudo apt install -y yarn
```
# Start a persistent screen session
```Bash
screen -S gensyn
```
Block 2: Clone Repository & Initial Run

This block clones the Gensyn code, navigates into the directory, sets up the Python environment, and executes the main run script.

Paste this block into the gensyn screen session you just created.


```Bash
git clone https://github.com/gensyn-ai/rl-swarm.git
cd rl-swarm
python3 -m venv .venv
source .venv/bin/activate
./run_rl_swarm.sh
```
❗ STOP HERE: After pasting Block 2, the script will pause and prompt you to log in via a web browser. Proceed to Block 3 in a new terminal window.

Block 3: Setup Cloudflare Tunnel (For Web Login)

This block is run in a separate terminal window to create a secure tunnel so you can access the node's web interface (http://localhost:3000/) from your local machine to complete the login.


# Install UFW Firewall and allow ports
```Bash
sudo apt install ufw -y
sudo ufw allow 22
sudo ufw allow 3000/tcp
sudo ufw enable
```
# Install Cloudflared
```Bash
wget -q https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared-linux-amd64.deb
```
# Run the tunnel (Copy the resulting URL and paste it into your browser)
```Bash
cloudflared tunnel --url http://localhost:3000
```
ACTION REQUIRED: Copy the resulting public URL from the tunnel command and log in to the Gensyn dashboard. Once logged in, go back to the original gensyn screen session to complete the configuration prompts (Hugging Face, Model Name, AI Market).

Block 4: Install GSwarm and Go (For role)

This block installs Go and the GSwarm utility for Telegram notifications and Discord role linking.


# Install Go 1.24.0
```Bash
cd ~
wget https://go.dev/dl/go1.24.0.linux-amd64.tar.gz
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf go1.24.0.linux-amd64.tar.gz
```
# Configure environment variables
```Bash
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
echo 'export GOPATH=$HOME/go' >> ~/.bashrc
echo 'export PATH=$PATH:$GOPATH/bin' >> ~/.bashrc
source ~/.bashrc
```
# Install GSwarm
```Bash
go install github.com/Deep-Commit/gswarm/cmd/gswarm@latest
```
ACTION REQUIRED: After running this block, run the gswarm command to configure your Telegram token and Chat ID.
