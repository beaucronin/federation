# Federated websites

The web uses one-directional links: any website can link into any other, and the target of the link does not (and, barring a complete crawl of the web, cannot) know which other pages link to it. The advantage of this system is that it is permissionless and low-effort; no communication or agreement is needed between the operator of the source website and the operator of the target.

While this may have been the best choice for the web, there are disadvantages to this architecture, and therefore some situations in which bidirectional links would be advantageous. Bidirectional links are particularly useful when there is loose coupling between web pages - a level of connection that is somewhere between the tight relationships between pages on a single site, and the complete independence of separate websites.

## Centralized vs decentralized systems

Unlike existing one-way links, bidirectional links require some degree of communication between websites: when site A links to site B, site B will need to update its records so that it can display the link back to site A as needed.

Or we should say, _some system_ needs to update its records: we could imagine an implementation in which a central system tracks the link graph between websites and made these incoming-link records available to, say, the web browser on page load. In fact, such systems already exist: this is exactly the information collected by web crawlers. Why not use this same, well-tested design here?

There are a few reasons, and they are at different levels of analysis:

A centralized system naturally becomes a center of power that follows its own financial and ideological interests; there are many who believe that the internet is currently skewed toward a small number of such large, self-interested entities. As we bring new capabilities to the web, we might want to do so in a way that does not further centralize control and influence.

A centralized link graph would also operate on a nonconsensual basis; put differently, it would not reflect the value (positive or negativez) that the website operators placed on the links. Not only might this infringe on the rights - formal or not - of the website operators, it would result in a less useful graph. Just as there is strong signal in the existence of a one-way link from one website to another, there will be (even more?) signal in a bidirectional link between two sites. Links are, in many cases, acts of human judgment; inferring bidirectional links by inverting unidirectional ones does not capture any of this judgment.

## Requirements for a decentralized solution

The core challenge of a decentralized two-way linking scheme is that the required communication and coordination must be supported through mechanisms that are scalable and robust.
