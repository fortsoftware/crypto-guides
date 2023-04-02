# Running SUI full node with docker-compose on testnet

## Pre-requisites

1. [Hardware Requirements](https://docs.sui.io/build/fullnode#hardware-requirements)
2. Install [docker](https://docs.docker.com/get-docker/)
3. Install [docker compose](https://docs.docker.com/compose/install/)
4. If you have firewall, make sure to at least have the p2p port (default 8080) open. You might need to open other ports depending on the monitoring tools you want to use.

## Instructions

1. Create the home directory for your sui node. This is where the node's configuration and db files will live. 
   We are going to assume `/var/sui` in this document, but you can choose any directory that you prefer.
   ```
   mkdir -p /var/sui
   ```
2. Download the `fullnode.yaml` file. This is the file that holds the node configuration.
   ```
   wget -O fullnode.yaml https://github.com/MystenLabs/sui/raw/testnet/crates/sui-config/data/fullnode-template.yaml
   ```
3. Add the following config to your `fullnode.yaml` file. These are peers so that your node can start syncing faster.
   ```
   p2p-config:
     seed-peers:
       - address: "/dns/sui-rpc-pt.testnet-pride.com/udp/8084"
         peer-id: 0b10182585ae4a305721b1823ea5a9c3ce7d6ac4b4a8ce35fe96d9914c8fcb73
       - address: "/dns/sui-rpc-pt2.testnet-pride.com/udp/8084"
         peer-id: bf45f2bd2bbc4c2d53d10c05c96085d4ef18688af04649d6e65e1ebad1716804
       - address: "/dns/sui-rpc-testnet.bartestnet.com/udp/8084"
       - address: "/ip4/38.242.197.20/udp/8080"
       - address: "/ip4/178.18.250.62/udp/8080"
       - address: "/ip4/162.55.84.47/udp/8084"
       - address: "/dns/wave-3.testnet.n1stake.com/udp/8084"
   ```
4. Download the testnet genesis file.
   ```
   wget https://github.com/MystenLabs/sui-genesis/raw/main/testnet/genesis.blob
   ```
5. Copy the `env` file to `.env`. Use your favorite editor to edit the file.
   ```
   cp env .env
   ```
6. Update the variables in the `.env` file.
   * `SUI_IMAGE_GIT_BRANCH_OR_COMMIT_HASH` -  This is the docker image version, which corresponds to a git branch or a commit hash.
     The default value will be updated on a best-effort basis, but you might need to look in the official Sui
     communication channels (discord) to confirm this is the right commit or branch you should be using.

   * `SUI_HOME_DIRECTORY` - This is the sui node home directory. `/var/sui` is the default location, but you can choose
     a different location, as mentioned in step 1 of this document.

7. Run docker compose
   ```
   docker compose up -d
   ```

8. After a couple of minutes, check if your node is syncing. You can follow [this guide](https://github.com/testnet-pride/Node-manuals/blob/main/Testnets/Sui/guidePT.md#monitor-fullnode-performance)
   to check if your node is syncing.