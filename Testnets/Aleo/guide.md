[<img src='https://user-images.githubusercontent.com/80550154/227746770-2d6fa944-cfee-45c4-ab54-9b8853581251.png' alt='banner' width= '99.9%'>]()
___
[<img src='https://user-images.githubusercontent.com/83868103/227937841-6e05b933-9534-49f1-808a-efe087a4f7cd.png' alt='Twitter'  width='16.5%'>](https://twitter.com/intent/user?screen_name=TestnetPride&intent=follow)[<img src='https://user-images.githubusercontent.com/83868103/227935592-ea64badd-ceb4-4945-8dfc-f25c787fb29d.png' alt='TELEGRAM'  width='16.5%'>](https://t.me/TestnetPride)[<img src='https://user-images.githubusercontent.com/83868103/227936236-325bebfd-b287-4206-a964-dcbe67fe7ca8.png' alt='WEBSITE'  width='16.5%'>](http://testnet-pride.com/)[<img src='https://user-images.githubusercontent.com/83868103/227936479-a48e814b-3ec1-4dcb-bd44-96b02d8f55da.png' alt='MAIL'  width='16.5%'>](mailto:official@testnet-pride.com)[<img src='https://user-images.githubusercontent.com/83868103/227932993-b1e3a588-2b91-4915-854a-fa47da3b2cdb.png' alt='LINKEDIN'  width='16.5%'>](https://www.linkedin.com/company/testnet-pride/) [<img src='https://user-images.githubusercontent.com/83868103/227948915-65731f97-c406-4d2c-996c-e5440ff67584.png' alt='GITHUB'  width='16.5%'>](https://github.com/testnet-pride)
___
#### 1. Create a new Aleo wallet
* If you already have a wallet, you can use it and skip the wallet creation step ✅
* To create a new wallet, go to the [website](https://aleo.tools/) and click the "Generate" button. Save the output in a safe place 🔒 
##### After saving the keys, use them to add variables to your server using the commands below
[![Typing SVG](https://readme-typing-svg.herokuapp.com?font=Fira+Code&pause=5000&color=F73515&center=true&width=1000&lines=%E2%9B%94%EF%B8%8F+DO+NOT+GIVE+THIS+DATA+TO+ANYONE+%E2%9B%94%EF%B8%8F)](https://github.com/testnet-pride)
<img width="1781" alt="image" src="https://user-images.githubusercontent.com/83868103/227736022-1adcf1fd-4cca-4419-a823-8f859518d41e.png">
___
#### 2. Download required packages and create a tmux session
```bash
apt update && \
apt install make clang pkg-config libssl-dev build-essential gcc xz-utils git curl vim tmux ntp jq llvm ufw -y && \
tmux new -s deploy
```
##### *P.S. Creating a tmux session is required to build a binary, which takes some time. So you won't need to add variables and build a binary again if the ssh connection to your server lost. Just reconnect to the tmux session.*
___
#### 3. Add your wallet and private key as a variable. 

```bash
echo Enter your Private Key: && read PK && \
echo Enter your View Key: && read VK && \
echo Enter your Address: && read ADDRESS
```
#### 4. Make sure the data is correct. If not, you can do step 3 again.
```bash
echo Private Key: $PK && \
echo View Key: $VK && \
echo Address: $ADDRESS
```
___
#### 5. Generate a tweet with your wallet to get tokens
``` bash
echo "https://twitter.com/intent/tweet?text=@AleoFaucet%20send%2010%20credits%20to%20$ADDRESS"
```
##### *P.S. You need to past the output of the command above into your browser and publish a tweet and wait for the response form a bot. It will take about 30-40 minutes. The bot will send you a link which you will need to use in the step 7.*

#### **Do not wait for the response. Feel free to do the next step (6) in the meantime**
<img width= 43% alt=image src=https://user-images.githubusercontent.com/80550154/227748074-45a0004b-e131-4f58-9469-0591797c025d.png> <img width="43%" alt="image" src=https://user-images.githubusercontent.com/80550154/227748072-161f0fe6-21db-412e-be3b-e8d5707ad64f.png>

___
#### 6. Install required software
```bash
cd $HOME
git clone https://github.com/AleoHQ/snarkOS.git --depth 1
cd snarkOS
bash ./build_ubuntu.sh
source $HOME/.bashrc
source $HOME/.cargo/env
```

```bash
cd $HOME
git clone https://github.com/AleoHQ/leo.git
cd leo
cargo install --path .
```
___
#### 7. Deploy a contract
```bash
echo Enter the Name of your contract "(any)": && read NAME
```
```bash
cd $HOME && mkdir leo_deploy && cd leo_deploy
leo new $NAME
```
##### *P.S In the command below past the link which you got from a bot on twitter at [the previous step](https://github.com/testnet-pride/Node-manuals/blob/main/Testnets/Aleo/guide.md#5-generate-a-tweet-with-your-wallet-to-get-tokens).*
```bash
echo Paste the link: && read QUOTE_LINK && \
CIPHERTEXT=$(curl -s $QUOTE_LINK | jq -r '.execution.transitions[0].outputs[0].value')
```

```bash
RECORD=$(snarkos developer decrypt --ciphertext $CIPHERTEXT --view-key $VK)
```
```bash
snarkos developer deploy "$NAME.aleo" \
--private-key "$PK" \
--query "https://vm.aleo.org/api" \
--path "$HOME/leo_deploy/$NAME/build/" \
--broadcast "https://vm.aleo.org/api/testnet3/transaction/broadcast" \
--fee 600000 \
--record "$RECORD"
```
#### That is it!
___
#### 8. Useful commands

##### Add a new tmux session
```
ctrl+b c
```
##### Show all sessions
```
ctrl+b w
```
##### Detach from tmux session
```
ctrl+b d
```
##### Return to a tmux session

```bash
tmux attach -t deploy
```
___
[<img src='https://user-images.githubusercontent.com/83868103/227937841-6e05b933-9534-49f1-808a-efe087a4f7cd.png' alt='Twitter'  width='16.5%'>](https://twitter.com/intent/user?screen_name=TestnetPride&intent=follow)[<img src='https://user-images.githubusercontent.com/83868103/227935592-ea64badd-ceb4-4945-8dfc-f25c787fb29d.png' alt='TELEGRAM'  width='16.5%'>](https://t.me/TestnetPride)[<img src='https://user-images.githubusercontent.com/83868103/227936236-325bebfd-b287-4206-a964-dcbe67fe7ca8.png' alt='WEBSITE'  width='16.5%'>](http://testnet-pride.com/)[<img src='https://user-images.githubusercontent.com/83868103/227936479-a48e814b-3ec1-4dcb-bd44-96b02d8f55da.png' alt='MAIL'  width='16.5%'>](mailto:official@testnet-pride.com)[<img src='https://user-images.githubusercontent.com/83868103/227932993-b1e3a588-2b91-4915-854a-fa47da3b2cdb.png' alt='LINKEDIN'  width='16.5%'>](https://www.linkedin.com/company/testnet-pride/) [<img src='https://user-images.githubusercontent.com/83868103/227948915-65731f97-c406-4d2c-996c-e5440ff67584.png' alt='GITHUB'  width='16.5%'>](https://github.com/testnet-pride)
___
