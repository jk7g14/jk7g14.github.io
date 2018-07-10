---
layout: post
title:  "EtherMembership, Decentralized Application for Membership Plans"
subtitle: "sdfsdf"
comments: true
---

These days, many big internet companies including Google, Facebook, Microsoft and Amazon are controlling the internet. For Starups, in my opinion, using Blockchain technology is the best way to compete with those giant internet companies because the major companies have the most money, computing power and all the data, and the Blockchain technology can be the solutions for all these problems. This is why I started to look up this technology and this may help the next generation of the Artificial Intelligenc. 

## Problem Statement
This project, EtherMembership, is the solution for the membership plans on Ethereum Blockchain. Currently, many websites are getting donations through PayPal and the websites' mananger should check and manually give the contributors some benefits. With this Ethereum Smart Contract, the manager do not need to do that and even though the contributors do not have bank accounts and live in non-PayPal service contries, they can contribute ethereum to the owner. Therefore, the mananger could monetize their values easily. Furthermore, they do not need to make their own website again because this is just going to be the second database for their website. 

## A brief explanation of EtherMembership

This Ethermembership would be the third-party app for anyone's membership plans. The managers of their websites just need to make the link to our app. This means that even if they do not know about web3.js and solidity, they can manange the ethereum based membership. Only they need to do is to make API to retrieve the data from ethermembership. The data would contain members' wallet address, their email or id in the website and the amount of ether they contributed so far. After they retrieve the data, the members of their website will get the grade according to the minimum contribution for membership plans.

![Sample Images](/img/ethermembership/architecture.png){:width="600"}

## Smart Contract

* **MembershipFactory Contract**

This factory contract helps to make a memebership contract and the creator of the membership contract gets control of the contract.

![Sample Images](/img/ethermembership/Factory.png){:width="600"}

* **Membership Contract** 

Each memebership contract will store the members' information, transection history and the url of the manager's website.

![Sample Images](/img/ethermembership/membership.png){:width="600"}

* **How these two contracts are connected** 

Our site would be the main page of EtherMembership app and in this manin site, the managers can make their own contract with the url of their site. 

![Sample Images](/img/ethermembership/cycle.png){:width="600"}

## Front-End

**1) Main Page**

This page retrieves all the deployed memebership contracts from the factory contract. In here, you can creat a new memebership contract and browse existing membership contracts.

![Sample Images](/img/ethermembership/root.png){:width="600"}

**2) Create a new membership contract**

The owner of sites can make their own membership contracts here and they need to pay some gas for this. The url should be the root of their REST API and the contract address of their membership will be sent to "url/new" as "cid".

For the manager of the contract now have their own contract address and their users can contribute some ether through "Main/memberships/:address". In this example, it is "http://localhost:8000/memberships/:address".

![Sample Images](/img/ethermembership/new.png){:width="600"}


**3) Register, Contribute, and Collect**

* Register

When users register their id or email of the site, they also can contribute their ether with some gas, so that users do not need to waste their gas. 

![Sample Images](/img/ethermembership/register2.png){:width="600"}

* Contribute more

After users registered their id in the contract, they can contribute more if they want to. Moreover, users can see how much ether they contributed as well when they come to the this page again according to their metamask account.

![Sample Images](/img/ethermembership/user.png){:width="600"}

* Collect

The manager will have a different view here with contract balance and collect button. When they click the collect button, all the registered user information will be sent to the website server. As a result, the server of their website will update their database according to the minimum ether for the memebership plans. 

![Sample Images](/img/ethermembership/manager.png){:width="600"}

## More information on Ethermembership

All the installation process and more information will be in [ethermembership](https://github.com/jk7g14/ethermembership).

<br />

---

{% if page.comments %}
<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
  /*
  var disqus_config = function () {
  this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
  this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
  };
  */
  (function() { // DON'T EDIT BELOW THIS LINE
  var d = document, s = d.createElement('script');
  s.src = 'https://jk7g14-github-io.disqus.com/embed.js';
  s.setAttribute('data-timestamp', +new Date());
  (d.head || d.body).appendChild(s);
  })();
  </script>
  <noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
  {% endif %}
