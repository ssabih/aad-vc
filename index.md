# Fundamentals of AAD Verifiable Credentials
<!-- wp:paragraph -->
<p>In this article I will explain the core concepts involved in AAD Verifiable Credentials (AAD VC) and how they come together in the big picture. Although there is plenty of information available on these concepts already, I will try to simplify it as much as possible in an order that is easy to grasp.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3>DID</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Its very important to understand Decentralized Identifier (DID) as it enables Verifiable Credential (VC).  </p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>did:ion:EiD1yi78SVbtXwTvIb21HM-OxkrnV_8LyMvb3RE9AFKoXQ</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Above is an example of a DID. In short DID is an address that resolves to a DID document which contains useful information about the subject DID refers to. Subject can be an organization, person, device etc. DID has three parts: scheme which is denoted by did:, method identifier which in my case is ION and a unique identifier. If I know a DID, I can resolve it to get the information available in the DID document. DID is immutable and cannot be transferred throughout its life but a DID document can be updated after its creation.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3>DID document</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>DID document in simple words is just a nice a way of storing the public key along with other useful information in a DID document. DID does not require a validator such as Certificate Authority (CA) or intermediate CAs, that terminology does not apply instead there is a concept of decentralized PKI (discussed under ION section).</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>You can resolve the above DID through an <a href="https://identity.foundation/ion/explorer/">ION network explorer</a>. Why ION because that's the DID method as visible in the DID. Below is an example of a DID document, it has a public key, two endpoints and a Linked Domain.  This is what I get when I resolve the above DID.</p>
<!-- /wp:paragraph -->

<!-- wp:code {"style":{"typography":{"fontSize":"12px"}}} -->
<pre class="wp-block-code" style="font-size:12px"><code>{
  "@context": "https://w3id.org/did-resolution/v1",
  "didDocument": {
    "id": "did:ion:EiD1yi78SVbtXwTvIb21HM-OxkrnV_8LyMvb3RE9AFKoXQ",
    "@context": &#91;
      "https://www.w3.org/ns/did/v1",
      {
        "@base": "did:ion:EiD1yi78SVbtXwTvIb21HM-OxkrnV_8LyMvb3RE9AFKoXQ"
      }
    ],
    "service": &#91;
      {
        "id": "#linkeddomains",
        "type": "LinkedDomains",
        "serviceEndpoint": {
          "origins": &#91;
            "https://custom-domain.net/"
          ]
        }
      },
      {
        "id": "#hub",
        "type": "IdentityHub",
        "serviceEndpoint": {
          "instances": &#91;
            "https://beta.hub.msidentity.com/v1.0/5962e0c0-b6f1-451b-b9d1-ecb9e43d1f55"
          ],
          "origins": &#91;]
        }
      }
    ],
    "verificationMethod": &#91;
      {
        "id": "#sig_41c58922",
        "controller": "did:ion:EiD1yi78SVbtXwTvIb21HM-OxkrnV_8LyMvb3RE9AFKoXQ",
        "type": "EcdsaSecp256k1VerificationKey2019",
        "publicKeyJwk": {
          "kty": "EC",
          "crv": "secp256k1",
          "x": "Nt0BjmRrNyox9T_sjmQUMqGYhZr1udZnz8OPh4CwkJo",
          "y": "mILYNrdpMewmT2k27CdYecJ6givLrlNtwdBL8yVu2Hg"
        }
      }
    ],
    "authentication": &#91;
      "#sig_41c58922"
    ],
    "assertionMethod": &#91;
      "#sig_41c58922"
    ]
  },
  "didDocumentMetadata": {
    "method": {
      "published": true,
      "recoveryCommitment": "EiCjD37DPA8VQzCFFHZDiHrrbLdUi-JC7Apa_sWWuyoFTA",
      "updateCommitment": "EiCida7lsqIxZbtOP8Utqcs15XmPbCkUk7CQSUbLFXW0nw"
    },
    "canonicalId": "did:ion:EiD1yi78SVbtXwTvIb21HM-OxkrnV_8LyMvb3RE9AFKoXQ"
  }
}</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3>Linked Domain</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>I mentioned in previous section that my DID document contains a Linked Domain and we can see that its called "https://custom-domain.net". In short, its an SSL binding which proves that the subject of the DID controls this domain.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Linked Domain in the context of AAD VC represents issuer and the user agent which is Microsoft Authenticator confirms if the Linked Domain is verified or not once it receives the VC.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":71,"width":514,"height":98,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large is-resized"><img src="https://sabih114253105.files.wordpress.com/2022/03/linked-domain.png?w=762" alt="" class="wp-image-71" width="514" height="98"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Verifying a domain requires that you host a did-configuration.json file on your server, you are provided this file when you try to verify a domain to be used as Linked Domain in your AAD VC. In my example its hosted at following path:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>https:&#47;&#47;custom-domain.net/.well-known/did-configuration.json</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3>ION</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Now that I have mentioned ION in my previous section, let me explain it briefly. Identity Overlay Network (ION) in simple words is a storage mechanism for DID documents. In the world of DID, everything needs to be decentralized. ION nodes are not owned by any single entity and anyone can spin up an ION node and become part of the ION network. ION provides a permissionless system for DID</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>After a DID document is created, it will change over time due to operations such as keys rollover. All of these operations have to be tracked, ION provides this capability through Sidetree protocol which runs on all ION nodes.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>All the signed Sidetree transactions are put in a Batch file which is connected to an Anchor file that has the Merkle Root. What's written to the ledger (Bitcoin) is the single IPFS hash of the singed Anchor file that can represent thousands of DID operations in a specific Batch. IPFS is used as P2P file replication protocol in ION</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":100,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://sabih114253105.files.wordpress.com/2022/03/sidetree-2.png?w=1024" alt="" class="wp-image-100"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Other ION nodes detects the ledger transaction, it connects to its peer node and retrieves the Batch file to maintain the state of the DID. When it receives a request for DID document, ION looks through the Batch operations in chronological order to produce a DID document to be returned for a specific DID. </p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3>AAD VC Service</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>AAD VC service can be seen as an API server which serves the API calls for two main purposes: Issuance and Verification of the VC.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>VC is a digital credential which has claims about the subject of the VC and its signed by private key of an issuer's DID. Below is an example of an AAD VC.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":66,"width":288,"height":536,"sizeSlug":"large","linkDestination":"none","style":{"color":[]},"className":"is-style-default"} -->
<figure class="wp-block-image size-large is-resized is-style-default"><img src="https://sabih114253105.files.wordpress.com/2022/03/aad-vc.jpeg?w=549" alt="" class="wp-image-66" width="288" height="536"/></figure>
<!-- /wp:image -->

<!-- wp:group -->
<div class="wp-block-group"><!-- wp:paragraph -->
<p>This VC is signed by the private key of an issuer's DID. Microsoft Authenticator, you can also call it user agent or wallet was able to resolve it to a DID document. In the DID document it was able to find the public key and was able to verify the signature. It was also able to check the Linked Domain as well.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>In a typical AAD VC issuance scenario</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>Microsoft Authenticator will download a VC issuance request after scanning a QR code.</li><li>After downloading the issuance request, Microsoft Authenticator will process the request which involves resolving the DID to a DID document and checking the Linked Domain</li><li>Depending on VC issuance contract requirement, Microsoft Authenticator might request additional information from the user in the form of an ID token or self-asserted information</li><li>In the end AAD VC service will provide a VC that contains claims about the subject made by the issuer. Microsoft Authenticator will store this VC which can later be used to prove the claims to a VC verifier</li></ul>
<!-- /wp:list --></div>
<!-- /wp:group -->

<!-- wp:paragraph -->
<p>In a typical AAD VC verification scenario</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>Microsoft Authenticator will download a VC presentation request after scanning a QR code.</li><li>After downloading the presentation request, Microsoft Authenticator validates the DID of the verifier and gains the consent to present the matching VC to verifier.</li><li>Once AAD VC service receives the VC information presented by Microsoft Authenticator, it will check if the VC is valid and provides the overall result to the verifier.</li></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3>Conclusion</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>I hope, I was able to simplify the very complex working of DID and VC to some extent. There is a complete guide by Microsoft on AAD VC, available <a href="https://docs.microsoft.com/en-us/azure/active-directory/verifiable-credentials/">here</a>. If you want to dive deeper into the mechanics and open standards then have a look at this <a href="https://www.w3.org/TR/did-core/">documentation</a>.</p>
<!-- /wp:paragraph -->
