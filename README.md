# Gensyn

Node's Important Dependencies:

Install sudo
`apt update && apt install -y sudo`

Install Dependencies
`sudo apt update && sudo apt install -y python3 python3-venv python3-pip curl wget screen git lsof nano unzip`

Nodejs And NPM (thanks to zunxbt)
`curl -sSL https://raw.githubusercontent.com/zunxbt/installation/main/node.sh | bash`

[ If you want to push models to huggingface, Use this code before node startup:- ]
```bash
git config --global credential.helper store
```
[ What to do after: Waiting for userData.json to be created... ]

**If running locally (i.e. on a macbook): **

1. Go to your browser and open http://localhost:3000 - follow the sign-in instructions
2. Go back to the terminal and wait for it to carry on
3. You can now close the browser tab 

**if running on a VPS (remote server):**

OPTION 1
Create an SSH tunnel

1. On your local machine, run `ssh -L 3000:localhost:3000 <your_username@your_server_ip>` and leave it running
2. Run `./run_rl_swarm.sh` and wait for it to reach the `Userdata.json` step
3. Open http://localhost:3000 on your local browser and login
4. Go back to the terminal you ran `./run_rl_swarm.sh` from and wait for it to continue
5. You can now close the browser tab and the `ssh -L` tunnel

OPTION 2 (EASIER)
More easily create an SSH tunnel

1. Install `cloudflared` on your machine by following these instructions for your operating system: 
    https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/downloads/ 
    (on mac it's just `brew install cloudflared`)
2. Open another terminal / ssh connection and run this command:
     `cloudflared tunnel --url http://localhost:3000`
3. Copy the url it gives you from the output message (in the screenshot)
4. Paste it into your browser on your local machine - follow the sign-in instructions
5. Go back to the terminal and wait for it to carry on
6. You can now close the cloudflared terminal / ssh connection and the browser tab

{ If you want to restore your node with `swarm.pem` & email }

Follow all the steps like installing dependencies, starting the node etc but when you reach at `userData.json` close the screen/node then copy your backed up `swarm.pem` to repo folder also change its owner permission to write and then start the node again and follow the process normally.

One more simple way:
When you clone gensyn repo, copy your backed swarm and change its owner permission to write. it shoould look like rw-r-r

{ Some imp commands }

To clear/del repo folder for starting fresh node installation
`rm -rf ~/(your repo folder name eg. rl-swarm)`

Minimize node/screen
`CTRL+A+D`

Return to node screen
`screen -r (screen name eg. rlswarm)`

To stop node or kill screen
`screen -XS (screen name eg. rlswarm) quit` or `killall screen`



ERROR: hivemind.p2p.p2p_daemon_bindings.utils.P2PDaemonError: Daemon failed to start: 2025/05/15 xxxxxx failed to connect to bootstrap peers
some specific peers are offline, we must looking other online peers

Solution: 
1. under directory rl-swarm, typing: nano hivemind_exp/runner/gensyn/testnet_grpo_runner.py 
2. see picture and modify like this
```
    def get_initial_peers(self) -> list[str]:
    # Skip chain lookup if no peers provided
        if not getattr(self, 'force_chain_lookup', True):
                return []

    # Original chain lookup
        peers = self.coordinator.get_bootnodes()
        logger.info(f"Bootnodes from chain: {peers}")

    # Filter out dead peers (optional)
        alive_peers = [p for p in peers if not p.startswith('/ip4/38.101.215.14')]
        return alive_peers if alive_peers else []
```
3. in my case (or most users maybe) 38.101.215.14 provide offline peers
3b. I forgot, nano run_rl_swarm.sh and find DEFAULT_PEER_MULTI_ADDRS="/ip4/38.101.215.14/tcp/30002/p2p/QmQ2gEXoPJg6iMBSUFWGzAabS2VhnzuS782Y637hGjfsRJ"  (could be diffirent)
and change it into: DEFAULT_PEER_MULTI_ADDRS=""
yes, let it blank
4. re run in python env
5. let me know if it worked, I tried only 1 node in my CPU mode and working, score,rewards also increased
---==  GOOD LUCK  ==--

NOTE
    # Filter out dead peers (optional)
        alive_peers = [p for p in peers if not p.startswith('/ip4/38.101.215.15')]
        return alive_peers if alive_peers else []

it could be your another IP like 38.101.215.14,etc...you must check in your CLI

