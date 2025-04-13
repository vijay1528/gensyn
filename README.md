# Gensyn

[ If you are running node only with CPU ]

python3 -m venv .venv
source .venv/bin/activate
CPU_ONLY=true ./run_rl_swarm.sh

[ If you want to push models to huggingface ]

Use this code before node startup:-

git config --global credential.helper store

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

