# Side-Protocol

Website - https://side.one/

Twitter - https://twitter.com/SideProtocol

Telegram - https://t.me/SideProtocolOfficial

Discord - https://discord.gg/sideprotocol

Github - https://github.com/sideprotocol

Explorer - https://explorer.side.exchange/testnet/staking

# Auto install
```
sudo apt install curl -y && source <(curl -s https://raw.githubusercontent.com/lthuan2011/Side-Protocol/main/side_auto)
```
# Create a wallet
```
sided keys add wallet
```
# Query Wallet Balance
```
sided q bank balances $(sided keys show wallet -a)
```
# Check sync status
```
sided status 2>&1 | jq .SyncInfo.catching_up
```

# Create Validator
```
sided tx staking create-validator \
  --amount=1000000uside \
  --pubkey=$(sided tendermint show-validator) \
  --moniker="Your_Moniker" \
  --chain-id="side-testnet-1" \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1" \
  --fees="200uside" \
  --from=wallet
```
# Services Management
```
# Reload Service
sudo systemctl daemon-reload

# Enable Service
sudo systemctl enable sided

# Disable Service
sudo systemctl disable sided

# Start Service
sudo systemctl start sided

# Stop Service
sudo systemctl stop sided

# Restart Service
sudo systemctl restart sided

# Check Service Status
sudo systemctl status sided

# Check Service Logs
sudo journalctl -u sided -f --no-hostname -o cat
```
#  Fix err NOT SYNCED
```
cd $HOME
sudo systemctl stop sided
cp $HOME/.sidechain/data/priv_validator_state.json $HOME/.sidechain/priv_validator_state.json.backup
sided tendermint unsafe-reset-all --home $HOME/.sidechain --keep-addr-book
SNAP_RPC="https://testnet-rpc.side.one:443"
              
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 2000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)
                          
sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
              s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
              s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
              s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" $HOME/.sidechain/config/config.toml
                          
mv $HOME/.sidechain/priv_validator_state.json.backup $HOME/.sidechain/data/priv_validator_state.json
peers=$(curl -s https://raw.githubusercontent.com/lthuan2011/Side-Protocol/main/peers.txt)
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" ~/.sidechain/config/config.toml
sudo systemctl restart sided && sudo journalctl -u sided -f --no-hostname -o cat
```
