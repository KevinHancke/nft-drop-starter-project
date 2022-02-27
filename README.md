# buildspace Solana NFT Drop Project - Edited by Kevin Hancke
### Welcome 👋
Instructions:

## Clone repository 
1. Run `git clone https://github.com/KevinHancke/nft-drop-starter-project`
2. Run `cd app`
3. Run `npm install` at the root of your directory
4. Run `npm run start` to start the project

## Setup your local Solana environment
This part will get tricky... So lets make sure we log the exact command and paths used I am using Windows OS Ubuntu Terminal
1. Requirements
    Run `git --version`
    > git version 2.31.1 (or higher!)

    Run `node --version`
    > v14.17.0 (or higher, below v17 -- we found that node v16 works best)

    Run `yarn --version`
    > 1.22.11 (or higher!)

    Run `ts-node --version`
    > v10.2.1 (or higher!)

2. Solana install: https://docs.solana.com/cli/install-solana-cli-tools#use-solanas-install-tool
    
    Run `solana --version`

    Run `solana config set --url devnet`

    Run `solana config get`

3. Install Metaplex to the home folder of your user (*NB* Pay attention here)

-------------------------------------------------------------------------------------------------------------------------------------
TEST OUTPUTS:

1. From /mnt/c/pr0 > Run `git clone -b v1.1.1 https://github.com/metaplex-foundation/metaplex.git`
 > Metaplex installed to mnt/c/pr0/metaplex
2. From /mnt/c/pr0 > Run `yarn install --cwd ~/metaplex/js/`
 > error Directory "/home/thekev0dev/metaplex/js" doesn't exist
-----> Try next:

This is the version I used and proceeded with:
1. From /mnt/c/pr0 > Run `git clone -b v1.1.1 https://github.com/metaplex-foundation/metaplex.git`
 > Metaplex installed to mnt/c/pr0/metaplex
2. From /mnt/c/pr0 > Run `yarn install --cwd metaplex/js/`
 > Success - many warnings about packages none ciritical
3. From /mnt/c/pr0 > Run `ts-node metaplex/js/packages/cli/src/candy-machine-v2-cli.ts --version`
 > 0.0.2
-----> Proceeding from here

------------------------------------------------------------------------------------------------------------------------------------

## Setup your Assets + Solana Keypair
1. Create folder 'assets' in root of nft-drop-starter-project
2. Run `solana-keygen new --outfile ~/.config/solana/devnet.json`
3. Run `solana config set --keypair ~/.config/solana/devnet.json` to set this as default keypair
4. Run `solana airdrop 2` or `solana balance`

## Configure the Candy Machine
1. Create/Edit file 'config.json' in the root of the project.
2. Upload the NFT and create candy machine run this command from one level outside the assets folder
    From your project folder Run `ts-node /mnt/c/pr0/metaplex/js/packages/cli/src/candy-machine-v2-cli.ts upload -e devnet -k /home/thekev0dev/.config/solana/devnet.json -cp config.json ./assets` 
3. Remember to save your key from the output of step 2:
    > initialized config for a candy machine with publickey: "<YOUR_KEY>"
4. Verify that your NFT's were uploaded:
    From your project folder Run `ts-node /mnt/c/pr0/metaplex/js/packages/cli/src/candy-machine-v2-cli.ts verify_upload -e devnet -k /home/thekev0dev/.config/solana/devnet.json`
5. *NB* Remember to delete the cache folder should you wish to make updates to your NFTs then rerun the upload command; Note that .png is the only supported filetype for the time being unless you use a script; I experienced difficulty deploying with the larger images, 1mb appears to be a decent size. 500 x 500px was used for 10 images.

# Call your Candy Machine from Web App
1. Setup your .env variable from nft-drop-starter-project/app/.env:
    REACT_APP_CANDY_MACHINE_ID= "<YOUR_KEY>" found in output of step 3 from configure your candy machine above
    REACT_APP_SOLANA_NETWORK= devnet
    REACT_APP_SOLANA_RPC_HOST= https://explorer-api.devnet.solana.com
    # *NB* As a general rule never commit your .env file 

# Update Candy Machine //have not tested this yet check similar commands for path
1. `ts-node ~/metaplex/js/packages/cli/src/candy-machine-v2-cli.ts update_candy_machine -e devnet  -k ~/.config/solana/devnet.json -cp config.json`