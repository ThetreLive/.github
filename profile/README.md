<div align="center">
    <img src="https://data.thetaedgestore.com/api/v2/data/0x5fe701fdc26e01f8698cd546ee16a7920df1a537322cb631eb08d76e5aa0162d" alt="Thetre">
    <h2>A Decentralised Movie Streaming Experience, powered by Theta EdgeCloud Video Services, Theta Edgestore and Libp2p</h2>
</div>

### Features
- **A DAO for Trustless Movie Proposals and Listing:** Thetre DAO is a fully transparent and trustless system that delegates token power based on members' past history of providing movie reviews. This system adds value to the opinions of movie critics by recognising their contributions. Currently, the system is closed for testing, but ZK Snark-based smart contracts have been implemented for Thetre DAO elections. Token power is delegated based on these elections, allowing critics to vote For or Against a movie proposal.

     DAO members are allowed to watch parts of the movie along with all other relevant details to make informed decisions. Members receive a Thetre DAO Pass for DRM Sign-in to watch proposal movies.## How I built it

- **A Hybrid Business Model That Accounts for All Viewers:** Thetre uses a hybrid model to ensure that no viewer pays more than they should. This model includes two options: Subscription-based and Pay-per-movie.

     Subscription-based Model: Ideal for "binge-watchers," this model mints an expiring NFT for the viewer based on the subscription period, which is used for DRM sign-in.

     Pay-per-movie Model: Designed for occasional viewers, this model mints an NFT specific to the movie, allowing access via Theta DRM.

- **Watch Movies Together With Friends and Family:** Thetre makes it easy to enjoy movies with your loved ones using Libp2p and gossipsub technology. With just one click, you can start a watch party that includes:

     Real-time Chat: Chat with your friends and family as you watch.
     Synchronised Viewing: Watch the movie together in perfect sync.
     TFUEL Transactions: Easily request and send TFUEL within the chat.

     Even if only one person has TFUEL, the whole group can join in and watch the movie. This makes Thetre accessible and enjoyable for everyone, even those who aren’t familiar with blockchain technology.

- **Passkey Based Wallets:** Thetre uses passkey-based wallets to greatly enhance the user experience. This eliminates the hassle of setting up a wallet application like Metamask. Passkeys also ensure the safety of users' funds. Thetre includes a built-in wallet manager, making it a one-stop application without the need for any other apps. During the testing phase, we’ve included Metamask login, but this will soon be replaced with ERC 4337-based smart accounts.

- **Different kinds of movie streaming:** Thetre offers two different streaming options(decided by the movie proposer) -

     Recorded Movies: Once the proposer submits a movie, their role is complete. The movie is stored in Thetre's S3 bucket and delivered using Theta EdgeStore. For DRM-protected content, it’s securely stored in Theta EdgeCloud. Otherwise, viewers get direct access via EdgeStore.

     Livestream Based Movies: Proposers stream the movie according to a schedule they set when creating the proposal. This approach is highly cost-efficient as it doesn't require permanent storage. Plus, it brings back the nostalgic theatrical experience that we’ve lost over time.

## Technical Aspects
Thetre's main focus is to provide an excellent user experience while being cost-effective.

### DAO & Proposals
![Proposals](https://data.thetaedgestore.com/api/v2/data/0x450f37edfc0a91e1df612883c30a1c64a49c71af9706934f841e3da72d9a3536/)

1. ZK Snarks - Zero-Knowledge Snark is used for the election of DAO members, ensuring the process is completely anonymous and decentralised. The DAO election occurs monthly, allowing anyone to apply and share their "love for movies." During the testing phase, this process is internal but will be open to all on the mainnet. After a certain threshold of votes is achieved, the applicant receives a DAO Pass which is used for DRM Sign-in while viewing proposals. 
    Check out the contracts [here](https://github.com/ThetreLive/thetre-contracts/tree/master/contracts/ThetreDAOElection)

2. tDAO Token - tDAO is a TNT20 token which is delegated to the DAO members according to the votes they receive, it represents the voting power of an individual member.

3. Openzeppelin Governance Contracts - Thetre uses Openzeppelin Governance for allowing anyone to create a movie proposal. The voting period lasts for 7 days after the delay of 1 day where the proposer has a chance to withdraw their application before the voting begins. The Governance uses tDAO as the vote token.

4. Validation & Listing - Once a proposal is accepted, it undergoes a final validation before the movie is listed on Thetre according to its type (recorded or livestream).

| Contract Name        | Address                                    |
|----------------------|--------------------------------------------|
| [TIMELOCK_CONTROLLER](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/governance/TimelockController.sol)  | [0xB4e7d6cF228a6cB316FeEcf4700BE133257eFF47](https://testnet-explorer.thetatoken.org/account/0xB4e7d6cF228a6cB316FeEcf4700BE133257eFF47) |
| [DAO_TOKEN](https://github.com/ThetreLive/thetre-contracts/blob/master/contracts/ThetreListings/DAOToken.sol)            | [0xb0c3b5778436737c1fe7ac554d2ea9180A620574](https://testnet-explorer.thetatoken.org/account/0xb0c3b5778436737c1fe7ac554d2ea9180A620574) |
| [LISTING_GOVERNER](https://github.com/ThetreLive/thetre-contracts/blob/master/contracts/ThetreListings/ListingGoverner.sol)     | [0x7c12bcf3154DA003D7e79875707cC3aa393a3646](https://testnet-explorer.thetatoken.org/account/0x7c12bcf3154DA003D7e79875707cC3aa393a3646) |
| [THETRE](https://github.com/ThetreLive/thetre-contracts/blob/master/contracts/Thetre.sol)               | [0x19648bD235C758C7a54BC4B7e4d8Faf67a8a44EE](https://testnet-explorer.thetatoken.org/account/0x19648bD235C758C7a54BC4B7e4d8Faf67a8a44EE) |
| [GOVERNANCE_PASS](https://github.com/ThetreLive/thetre-contracts/blob/master/contracts/ThetreTicket.sol)      | [0x1a1d19fe31197e49ffcc292ff6a23c4fefb3ff39](https://testnet-explorer.thetatoken.org/account/0x1a1d19fe31197e49ffcc292ff6a23c4fefb3ff39) |


### Backend Architecture
![Microservices](https://data.thetaedgestore.com/api/v2/data/0x2c8aa4e185b1b735b9f5143a508d494f637cfbb0327c2542fcc95a0c85369e44/)

1. AWS S3 - While creating movie proposal, the images and videos are saved in Thetre S3 bucket using API Gateway.

2. Elastic Block Storage - S3 Bucket is mounted to the EBS, which creates a virtual filesystem.

3. Theta EdgeStore - There are currently two Theta EdgeStore nodes running on AWS EC2 and GCP VM each in the ap-south-1 zone. Theta EdgeStore Network acts as a dCDN for the data stored in AWS S3 bucket. It serves the purpose of Cloudfront along with the added benefits such as customisability and more control.

     ![Theta EdgeStore](https://data.thetaedgestore.com/api/v2/data/0x4686ab03d5b71f07ec3dad077be8e8bccf07e7b03a9db16d1710ee94d587f714/)

4. Theta EdgeCloud - The usage of Theta EdgeCloud depends on the type of movie. Storing data in multiple places can be costly, so I offer filmmakers the option to choose between recorded or livestream-based movies. Initially, the movie is temporarily stored in Theta EdgeCloud with DRM protection, accessible only to DAO members during voting via the DAO Pass NFT Collection.

     Once a proposal is approved:

     Free Movies: They can be directly listed using the movie URL provided by Theta EdgeStore.

     DRM-Based Movies:

      &nbsp;Livestream: If the filmmaker chooses the livestream option, a livestream is created for live screenings according to the schedule specified by the proposer.

      &nbsp;Recorded: If it is a DRM-based recorded movie, it is uploaded to Theta EdgeCloud for movie ticket-based access.

5. Libp2p - Thetre uses libp2p under-the-hood for allowing users to create chatrooms where they can Chat, Watch Movies together in Sync, and Request/Send Funds. Libp2p circuit relayer is deployed in an EC2 instance, it is used to establish connection while creating a chatroom. 

      ![Libp2p](https://data.thetaedgestore.com/api/v2/data/0x0a0d993408be00ff0d3fb6c14b2fddb7cf13392848e0ec44f359a9ab235cfec9/)

6. Redis - Redis is used as a cache layer for storing data related to ```ProposalCreated``` event. It is directly implemented inside NextJS api.

### Frontend Architecture

![Frontend](https://data.thetaedgestore.com/api/v2/data/0xf4a6d9bf7d11914bc877ef34969fb798878ed61fe98f300a9a3d763af2caa446)

1. Tools - Next.JS, Tailwind.Css, Ethers.js, Turnkey SDK, TVA.js

2. Login/Signup - Onboarding users is one of the key aspects for an application like Thetre, I wanted to ensure that even the viewers who aren't aware of web3 wallets can use the application seemlessly. Hence, I used Turnkey SDK for allowing passkey based wallet creation. There's also an option to use metamask(which could be removed in the mainnet launch). 

3. Free Movies - Free movies are directly accessed via theta edgestore using a specific key and relpath.

4. DRM Based Recorded Movies - These movies require the viewer to own an NFT from the movie's collection. It is stored in Theta EdgeCloud for DRM Based Access. These movies can be watched at any time, since they're always available. [/utils/theta.ts](https://github.com/ThetreLive/thetre-website/blob/master/src/utils/theta.ts)

5. DRM Based Livestreamed Movies - These movies also require an NFT from the movie's collection. But they're not stored anywhere, which saves alot of backend cost. Such movies are directly live streamed(just like they do in movie theatres) by the proposer by following the schedule they specify. Therefore, they are not always available to be watched. Proposer can find a button to start livestreaming in the watch page, where they can generate stream url and key.

    Check out the [thetaPlayer.tsx](https://github.com/ThetreLive/thetre-website/blob/master/src/components/thetaPlayer.tsx) component that allows to run Theta EdgeCloud Videos/Streams seemlessly.

6. Chatrooms - Thetre allows viewers to libp2p based chatrooms where they can do the following operations - 

    ![Chat](https://d112y698adiu2z.cloudfront.net/photos/production/software_photos/002/962/151/datas/original.png)

- Chat
- Watch Movies together in sync
- Request/Send TFUEL in chatroom - This is a really important feature. This allows a group of people to watch movies even if none of them except one, knows about blockchain. Anyone in the chatroom can request TFUEL to buy movie tickets