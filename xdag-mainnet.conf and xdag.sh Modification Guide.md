# xdag-mainnet.conf and xdag.sh Modification Guide

### Note: The following steps need to be performed after pulling the latest node code.



## 1. xdag-mainnet.conf

### Step 1. Modify the node RPC configuration

Change **`rpc.enabled = true`** to **`rpc.http.enabled = true`**



### Step 2. Update the whitelist of nodes

During the upgrade, the community will update the nodes in the whitelist. Please refer to the node whitelist released by the community.



### Step 3. Modify the generate block settings (Exchanges only)

Change **`node.generate.block.enable = true`** to **`node.generate.block.enable = false`**



## 2. xdag.sh

### Step 1. Modify the version in the xdag.sh file

```
cd xdagj/script
sudo vim xdag.sh
```

Change **`XDAG_VERSION="${project.version}"`** to **`XDAG_VERSION="0.8.0"`**



### Step 2. Copy the new version of xdag.sh file to the /run directory

```bash
cp xdag.sh /run
```
