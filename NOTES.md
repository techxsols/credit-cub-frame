Project Description
Credit Cub's Credit Club is a bear themed farcaster frame app that is able to extend credit when a user applies for it using only information pulled from their connected addresses. We can check if the address is a sybil or not, and gives an amount of credit based on the that addresses Assets, Associations, and Attestation.

First Sybil - We only want to extend credit to addresses with some social gravity not throwaway addresses or bots. So we query a bunch of attributes of the wallet, and give a +1 for each of the conditions met such as if address has an ENS a primary address and pfp set, if ENS register >1yr ago and >1yr left till expiry, if fid isbadgeholder, if fid <1500, if coinbase verified, if Nouns Balance >1. If score is 0, we deny the application.

Then we determine trust amount. This is a toy model since revealing it makes it very game-able. But in short it scores addresses by giving points based on meeting attributes, and then it gives $credit based on range buckets. You can get very clever, but in short we just need to know "Will this person repay, if they had the money?" and "do they have the ability to repay?".

We reuse the sybil checks as a larger social presence means a social cost to defect.
Then we have Associations, being part of a dao like Nouns means .
Get ($Balance of ETH, USDC, DAI, APECOIN)
Union vouches: if vouch count ≥ 3: get median vouch => give +1-4 pts based on amount.
After it determines a score, it than it gives an amount of $4,$42,$420, $4269 based on which risk bucket it falls in. And triggers a safe("creditcub.eth") via relay to call updateTrust on Union Finance. And that address now has a credit line underwritten by CreditCub.eth.

Lastly the user can activate their Union account on Optimism by minting an NFT on Base. We chose base for the mint because mint with warps isn't supported on OP, and OP_Union for the credit because they're only on Base Sepolia, and it's alot more fun of a demo when you get real $ in credit from effectively a program embedded in a tweet.

How it's Made
We used frog.fm and neynar to build the farcaster frame app. The artwork was a combo of gpt to get the characters and cleaning up in photoshop.

For the data inputs for the antisybil and underwriting models we used airstacks api to get usd/eth balances, ENS records and age data, coinbase verifications we pulled those from base directly via infura, and union vouch data we used their /data library. For credit you're partly underwriting a persons willingness to repay so beyond the BotorNot question social tech like names(ens) and faces(nouns), allows for reputation to aggregate.

For the wallet that would vouch for folks, we set up a 1/3 safe multisig and registered it on Union. To trigger the transactions assigning credit to applicants we used defenders relay api to sign and execute via the frame app. This is where we ran into some trouble where the frames kept timing out before the safe tx could be sent. Found a lifesaver of a tool (defer.fun) for anyone doing safe tx with frames.

We created an NFT on base where if someone mints it to an address, defender monitor will pick it up and register them.
