This image has the official Bitcoin Core client installed, thus it can be used as a wallet.

## Run a Bitcoin full node

    mkdir bitcoin && cd bitcoin
    docker run -d --name bitcoin -v $(pwd):/data/bitcoin soulmachine/bitcoin bitcoind


## List all wallet addresses

    docker exec -it bitcoin bitcoin-cli listreceivedbyaddress 0 true

Or,

    docker exec -it bitcoin bitcoin-cli getaddressesbyaccount ""

When a new Bitcoin node starts, it will pre-generate a Bitcoin address, that's why you can see an address above.


## Generate a wallet address

    docker exec -it bitcoin bitcoin-cli getnewaddress

This will generate a "Bitcoin address", which is actually a public key in [Asymmetric Cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography),

* Each Bitcoin address has a corresponding private key(`bitcoin-cli dumpprivkey <btc-address>`)
* You can calculate your Bitcoin address with the private key(`bitcoin-cli importprivkey <private-key>`), not vice versa
* As long as you have the private key, you control the funds.


<!--
## Encrypt a wallet

    docker exec -it bitcoin bitcoin-cli encryptwallet YOUR_PASS_PHRASE

After you run `encryptwallet`, you need to restart `bitcoind`. 

    docker kill bitcoin
    docker start bitcoin

Note that the `encryptwallet` command will disappear in `bitcoin-cli help`'s result, instead you will get three new commands: `walletlock`, `walletpassphrase` and `walletpassphrasechange`.

Run `docker exec -it bitcoin bitcoin-cli getinfo` you will see something like `"unlocked_until" : 0`, which means the wallet is locked.

Unlock the wallet for 30 seconds,

    docker exec -it bitcoin bitcoin-cli walletpassphrase YOUR_PASS_PHRASE 30

Run `bitcoin-cli getinfo` you will see `unlocked_until` has an expiration timestamp now.
-->


## Backup your wallet


### Method 1

    docker exec -it bitcoin bitcoin-cli dumpwallet /data/bitcoin/MyBitcoinWallet.txt

This command will backup all your private keys and Bitcoin addresses to a text file. You can store this text file to anywhere you like, for example Dropbox, Google Drive, Microsoft OneDrive, Evernote, etc.

Import it,

    docker exec -it bitcoin bitcoin-cli importwallet /data/bitcoin/MyBitcoinWallet.txt

There is another command `backupwallet` that is very similar to `dumpwallet`, the only difference is that `backupwallet` generates a binary file instead of a text file. Since binary files are not friendly to other tools such as Evernote, I rarely use `backupwallet`.


### Method2

To backup your wallet, actually the only important thing is your private key, as long as you have the private key, you can restore its corresponding Bitcoin address.

Show your private key,

    docker exec -it bitcoin bitcoin-cli dumpprivkey 1BoZttJg99TjJxyYFcGvgqfCV3dnwSLsZm

This will print your private key to screen,

* You can print your private to a paper via a printer and hide this paper in a secret place, this is so called [Paper Wallet](https://en.bitcoin.it/wiki/Paper_wallet)
* You can memorize this private key in your own brain and never tell it to other people, this is so called [Brain Wallet](https://en.bitcoin.it/wiki/Brainwallet)
* You can save this private key to Evernote, OneNote, etc.

Import a private key,

    docker exec -it bitcoin bitcoin-cli importprivkey YOUR_PRIVATE_KEY

Usually I prefer the second method because it has better compatibility(compatible human brain, printers, Evernote, etc.).


<!--
## Import wallet

    docker exec -it bitcoin bitcoin-cli importwallet /data/bitcoin/MyBitcoinWallet.bak

If your wallet is encrypted, you need to unlock it before importing it,

    docker exec -it bitcoin bitcoin-cli walletpassphrase YOUR_PASS_PHRASE 30
-->


## Query balance

    docker exec -it bitcoin bitcoin-cli getbalance


For more commands please run `bitcoin-cli help`.


## Get help

    docker exec -it bitcoin bitcoind -help
    docker exec -it bitcoin bitcoin-cli help

## References

* [Running A Full Node](https://bitcoin.org/en/full-node#ubuntu-1610)
* [kylemanna/docker-bitcoind](https://github.com/kylemanna/docker-bitcoind)
* [Chapter 3. The Bitcoin Client - oreilly.com](http://chimera.labs.oreilly.com/books/1234000001802/ch03.html)
