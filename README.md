# Side-Protocol

Website - https://side.one/

Twitter - https://twitter.com/SideProtocol

Telegram - https://t.me/SideProtocolOfficial

Discord - https://discord.gg/sideprotocol

Github https://github.com/sideprotocol

# Auto install
```
sudo apt install curl && source <(curl -s https://nodesync.top/side_protocol_auto)
```
# Create a wallet
```
sided keys add wallet
```
# Create Validator
```
sided tx staking create-validator \
  --amount=1000000uside \
  --pubkey=$(sided tendermint show-validator) \
  --moniker="<moniker>" \
  --identity="<identity>" \
  --website="<website>" \
  --details="<details>" \
  --security-contact="<contact>" \
  --chain-id="side-testnet-1" \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1" \
  --fees="200uside" \
  --from=wallet
```
