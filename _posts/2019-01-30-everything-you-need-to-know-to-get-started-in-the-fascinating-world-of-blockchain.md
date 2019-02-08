---
date: 2019-01-30
title: Everything you need to know to get started in the fascinating world of Blockchain
description: Absolute truth does not exist … or does it?
categories:
  - Blockchain
resources:
  - name: "What is 'Faas'?"
    link: https://medium.com/@Raquel_Teresa/everything-you-need-to-know-to-get-started-in-the-fascinating-world-of-blockchain-a3f8190d1cf8
  - name: "Source code"
    link: https://github.com/oci-tutorials
type: Post
set: Blockchain
---

### Absolute truth does not exist … or does it?

You are probably wondering if you have finally found an article in which the basic concepts of this technological trend are explained in a simple way. Usually, we find terms such as “Proof of Work”, “Smart Contract”, or “Hyperledger”. However, do we know what they really mean? And above all, what paradigm changes they will bring?

>  In our day to day we usually read quotes such as: “What the internet did for communications, blockchain will do for trusted transactions” — Forbes, or “The technology most likely to change the next decade of business is not the social web, big data, the cloud, robotics or even artificial intelligence. It is the Blockchain “- Harvard Business Review. But, will Blockchain really impact in our lives as Internet did?

After some time researching around the ins and outs of Blockchain through different sources, I think I have concentrated the most important points to understand the big possibilities of this technology, which will transform sectors such as financial, energy or security. It will stir up any organization in which transactions are usually used, ultimately.

**First things first … What is Blockchain?**

Blockchain, also known as DLT (Distributed Ledger Technology) is a decentralized mechanism that keeps track of all transactions made between two or more nodes in a network, in a secure and distributed way. It is nothing more than a great accounting book that is replicated in the machines of all the participants of a network, making impossible to modify the information contained in it, and which only users with permission can access, turning it into a shared source of truth.

![Image](https://cdn-images-1.medium.com/max/800/1*iOe5bg-HGlo94D9iTbZzCA.png)

**The mystery … Who created Blockchain?**

All great stories hide mysteries, conspiracy theories, and different enigmas. Blockchain has all the necessary ingredients to turn its birth into a legend.

On Halloween 2008, several hundred people belonging to different cryptographic communities received an email with the following sender: Satoshi Nakamoto. He informed them that he had been working on a system that eliminated the need for trust intermediaries when making transactions, and solved the problem of double spending. All this was explained in the paper: [“Bitcoin: A Peer-to-Peer Electronic Cash System”](https://bitcoin.org/bitcoin.pdf).

The first cryptographic currency based on Blockchain was created. And unlike most entrepreneurs and CxOs of the world’s largest multinationals, which feed the dreamers with their success stories in their search of success, its creator decided to remain anonymous.

**But … What did Satoshi Nakamoto explain in his paper?**

Until recently, whenever we needed to make a transaction, we had some central authority or third party that acted as an intermediary. Whether it was in the payment of an asset online, in the change of a property’s name, or when patenting a product, we relied on the truthfulness of banks, notaries or any other organization.

Blockchain involves a paradigm shift in this system, by introducing a peer-to-peer exchange mechanism in a distributed and secure way, thanks to the cryptographic algorithm on which it is based.

When several actors that are part of a Blockchain distributed network decide to exchange value (cryptographic currency, property title, etc.), a new block composed of a set of transactions is sent to all the nodes that participate in the network. At that moment, those nodes initiate the verification process, which consists in verifying that the information contained in the block is valid and that the bases of the agreed terms have not been altered. Once the majority of the nodes have validated it, the block is sealed with a cryptographic hash (algorithm) based on the information that contains and the previous block hash. Thus, the manipulation or alteration of the data contained in each of the blocks that are part of the chain is fairly impossible, and an immutable record is obtained, a source of truth.

In an effort to get a large number of nodes involved in the verification process, Satoshi Nakamoto proposed a system of miners. These users keep a copy of the chain in their machines, from where they use its compute capacity to calculate the complex mathematical operations necessary to incorporate a new block in the chain. Then, those miners who get the solution, obtain a great monetary reward. This process is called *Proof of Work*.

![Image](https://cdn-images-1.medium.com/max/800/1*-elE9DQRg3E9S1SglRCEwQ.png)

Moreover, in most data centers around the world, the databases are replicated to prevent a single point of failure and to provide high availability to the service. It is an expensive solution, but essential for any organization that handles sensitive data. In the case of Blockchain and due to its distributed nature, all the nodes that are part of the network have a copy of the blocks, avoiding a single point of failure and making insignificant the probability of data loss.

**“Permissionless” and “Permissioned” Blockchain**

Even though one of the first applications of Blockchain was Bitcoin, many other implementations of this technology have already been found. In the case of cryptographic currencies, the Blockchain network is public. Any user can join and obtain a copy of all transactions in the chain and participate in the verification process. On the other hand, it is also possible to create a private Blockchain network, in which only users with an invitation have access to the information contained in it and can participate in the validation process.

Currently, several open source initiatives such as Ethereum, Corda or *Hyperledger* Fabric, have developed their own implementations of Blockchain, making life easier for developers by providing different tools and frameworks. Other private companies, such as Oracle, have also put in place solutions based on this powerful technology.

**Let’s give an example**

Once we have an understanding of how a Blockchain public network such as Bitcoin works, let’s see one practical application of a private network. On this [link](https://www.youtube.com/watch?v=NGk9k7uP4F0), you will find the complete analysis of this case, which was modeled using the Blockchain service in the Oracle Cloud.

To improve the components’ traceability of their vehicles, a car manufacturer has integrated Blockchain with four of its dealers, creating the following network:

![Image](https://cdn-images-1.medium.com/max/800/1*Sn9Jok9e_lLD4Gp47XqdAw.png)

The car manufacturer is the founder of the Blockchain network, and the four dealers participate in it:

![Image](https://cdn-images-1.medium.com/max/800/1*fdBIPOQfvn9OAFqIKS55ow.gif)

Whenever a vehicle’s component is sold by one of the suppliers to the manufacturer, and once the transaction is approved by both parties, the transaction is registered in the Blockchain network. Thus, a large database shared between the parties is obtained, in a secure and unalterable way. Moreover, the dealers only have access to the information of their components, but not to those of other dealers.

![Image](https://cdn-images-1.medium.com/max/800/1*fPBQs0X3RtFyy9YICWGAcw.png)

This is just a simple practical example, that helps to understand better the countless applications that Blockchain has. For example, this solution in place would have avoided the scandal over the illegal installation of software that Volkswagen carried out to eleven million vehicles, in order to manipulate the results of the technical controls of polluting emissions. With a source of truth like Blockchain, it would have been possible to track the supply chain of all the cars, from the manufacturer to the customer, avoiding the manipulation in any of its phases.

**Last but not least … What are the Smart Contracts?**

Conventionally, contracts are used to close agreements between two or more parties. In Blockchain’s world, the Smart Contracts define the rules of the agreed terms and execute the outcomes automatically, without the need for an intermediary institution. The *Smart Contracts* are scripts that can be programmed in different programming languages such as Java or Go, and that verify that when any of the conditions described in the contract is satisfied, the transaction is made.

Imagine that you decide to bet five euros on the fact that Real Madrid will win the next game against Barcelona, and another person bets the same amount to the opposite case. With a Smart Contract in place between the two parts, the result of the game would be verified automatically, and the money would be sent to the winner. In this sense, no intermediaries neither a third party would be needed to act as an arbitrator.

**Absolute truth does not exist … or does it?**

Blockchain is the first source of distributed and inalterable truth. A large public database globally. Secure and collaborative.

There is no doubt that it is time to get on board with this amazing technology, in which the developer’s community will have a decisive role, not only in its growth but also in its transformation and evolution.

**This sounds great, but … How can I get started and try it?**

Oracle’s Blockchain service is available within the *free 30-days Trial*. Furthermore, there are many other solutions available to test. Why not try them?

[*Sign Up for Free!*](https://myservices.us.oraclecloud.com/mycloud/signup?sourceType=:ex:tb:::RC_WWSA180813P00054:Q3_MM_RT_BC&SC=:ex:tb:::RC_WWSA180813P00054:Q3_MM_RT_BC&pcode=WWSA180813P00054:Q3_MM_RT_BC)

![Image](https://cdn-images-1.medium.com/max/800/1*wGVDR9h6t7tW7cnd5dWSDw.png)

***

> Raquel Teresa [@Raquel_Teresa](https://twitter.com/Raquel_Teresa)
>
> Cloud Sales Consultant and Developer Advocate @ Oracle Digital.
> Passionate about technology and people.
