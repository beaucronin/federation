# Federated websites

The web uses one-directional links: any website can link into any other, and the target of the link does not (and, barring a complete crawl of the web, cannot) know which other pages link to it. The advantage of this system is that it is permissionless and low-effort; no communication or agreement is needed between the operator of the source website and the operator of the target.

While this may have been the best choice for the web, there are disadvantages to this architecture, and therefore some situations in which bidirectional links would be advantageous. Bidirectional links are particularly useful when there is loose coupling between web pages - a level of connection that is somewhere between the tight relationships between pages on a single site, and the complete independence of separate websites.

## Centralized vs decentralized systems

Unlike existing one-way links, bidirectional links require some degree of communication between websites: when site A links to site B, site B will need to update its records so that it can display the link back to site A as needed.

Or we should say, _some system_ needs to update its records: we could imagine an implementation in which a central system tracks the link graph between websites and made these incoming-link records available to, say, the web browser on page load. In fact, such systems already exist: this is exactly the information collected by web crawlers. Why not use this same, well-tested design here?

There are a few reasons, and they are at different levels of analysis:

**A centralized system naturally becomes a center of power** that follows its own financial and ideological interests; there are many who believe that the internet is currently skewed toward a small number of such large, self-interested entities. As we bring new capabilities to the web, we might want to do so in a way that does not further centralize control and influence.

**A centralized link graph would operate on a nonconsensual basis**; put differently, it would not reflect the value (positive or negativez) that the website operators placed on the links. Not only might this infringe on the rights - formal or not - of the website operators, it would result in a less useful graph. Just as there is strong signal in the existence of a one-way link from one website to another, there will be (even more?) signal in a bidirectional link between two sites. Links are, in many cases, acts of human judgment; inferring bidirectional links by inverting unidirectional ones does not capture any of this judgment.

## Requirements for a decentralized solution

The core challenge in designing a decentralized two-way linking scheme is that the required communication and coordination must be supported through mechanisms that are scalable to the size of the internet, robust in the face of failures and attacks, and accessible to a very wide range of developers and site creators.

* The system must function in "steady state" without server-side processes. Periodic processing can be used to update the link records, but those link records, as well as the federation graph, must be accessible as static assets.

* Website operators must retain control over which sites they establish two-way links with. The granularity of this control is not immediately obvious (subdomain-level policies, rather than page- or address-level policies are proposed below).

* Links must remain valid once established, and in particular must not be broken by the removal or movement of one "end" of the link.

## Terminology

The `requester` is the website which asks to establish a federal relationship, or to create a bi-directional link within an already-established relationship.

The `responder` is the website that receives the initiator's request.

## Proposed Solution

### Creating and Destroying Federal Relationships

Federal relationships are always consensual - each domain is free to agree to or refuse relationships with any other site/domain. These decisions _may_ follow a published policy, or they may not. Once a relationship is established, it is expected to be permanent, revoked only in cases where the other party operates in a manner at odds with the original expectations.

#### The `federation_request` 

A request to establish a federation relationship is initiated by an HTTP POST to a known address. 

The request must be made via https, and the `Origin` header must contain a host within the domain for which the federation request is being made.

The body of the request must contain a JSON document with the following structure:

```json
{
  'federation_type': 'RELATIONSHIP_REQUEST',
  'requester_domain': ['subA.example.com', '*.subB.example.com']
}
```

This POST should be directed at `test.com/_federation/peers`.

| Status Code | Meaning
| --- | ---
| 201 | The relationship has been established, and may be used immediately
| 202 | The relationship request has been received, but will not be acted upon synchronously
| 403 | The relationship has been declined by the responder
| 5xx | The responder was unable to process the request because it is not functioning properly, and no decision was made or will be made

#### Federation catalogue

Websites that participate in bi-directional linking under this protocol provide their current federation relationships at a known address. The presence or absence of a domain in this listing is the source of truth for these relationships.

If an originating domain is allowed, then the target website commits to 

1. accepting incoming links from the given domains to specific addresses under its control, 
2. maintaining these link records permanently, and 
3. making them available to subsequent requesters alongside the content (web pages and other resource types) which resides at those addresses

### Creating Bi-directional Links Between Federated Websites

### Browsing and Traversing Bi-directional Links
