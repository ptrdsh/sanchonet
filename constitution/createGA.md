- create governance Action
'''
cardano-cli conway governance action create-constitution \
  --testnet \
  --governance-action-deposit 0 \
  --stake-verification-key-file ../key/stake.vkey \
  --proposal-url "https://shorturl.at/gLPW7" \
  --proposal-hash "d52578a05ee1fb507d0fc66d11a23776767ffa79dee24373d641ca57be3ce6c2" \
  --constitution-url "https://shorturl.at/aoFG0" \
  --constitution-file governance/constitution.txt \
  --out-file constitution.action
'''
- cat constitution.action
- build tx
'''
  cardano-cli transaction build --testnet-magic 4 --conway-era \
  --tx-in $(cardano-cli query utxo --address $(cat ../key/payment_address.txt) --testnet-magic 4 
--out-file /dev/stdout | jq -r 'keys[0]') \
  --change-address $(cat ../key/payment_address.txt) \
  --proposal-file constitution.action \
  --witness-override 2 \
  --out-file tx.raw
'''
- sign tx
'''
cardano-cli transaction sign \
  --tx-body-file tx.raw \
  --signing-key-file ../key/cold.skey \
  --signing-key-file ../key/payment.skey \
  --testnet-magic 4 \
  --out-file tx.signed
'''
- send
'''
cardano-cli transaction submit \
  --testnet-magic 4 \
  --tx-file tx.signed
'''
- get GA ID for sharing it on socials by querying UTXOset of payment address
'''
cardano-cli query utxo --address $(cat ../key/payment_address.txt) --testnet-magic 4 --out-file 
/dev/stdout | jq -r 'keys[0]'
'''
