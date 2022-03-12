# Mechanics of AAD Verifiable Credentials
<!-- wp:paragraph -->
<p>In this article I will explain the basic concepts involved in AAD Verifiable Credentials (AAD VC).</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>To Understand AAD VC, there are some core concepts we have to be clear about. Although there is plenty of information available on these concepts already, I will try to simplify it as much as I can in the context of this article and keep the explanation brief. </p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3>DID</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If you are reading this article then you already have an idea about a DID (Decentralized Identifier). Its very important to understand DID as it enables Verifiable Credential (VC). In other words DID makes VC possible. </p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>did:ion:EiD1yi78SVbtXwTvIb21HM-OxkrnV_8LyMvb3RE9AFKoXQ</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Above is an example of a DID. In short DID is a pointer to a DID document which contains useful information about the subject and subject is an entity DID refers to. Subject can be an organization, person, device etc. If I know a DID, I can resolve it to get the information available in the DID document.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3>DID document</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>DID document is a nice a way of storing the public key along with other information but public key is the most important information contained in the DID document. DID does not require PKI or any of its component. </p>
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
<h3>Verifiable Credentials</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Finally into Verifiable Credentials (VC). In short VC is a digital credential which has claims about the subject of the VC and its signed by private key of an issuer's DID. Below is an example of an AAD VC.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":66,"width":257,"height":478,"sizeSlug":"large","linkDestination":"none","style":{"color":{}},"className":"is-style-default"} -->
<figure class="wp-block-image size-large is-resized is-style-default"><img src="https://sabih114253105.files.wordpress.com/2022/03/aad-vc.jpeg?w=549" alt="" class="wp-image-66" width="257" height="478"/></figure>
<!-- /wp:image -->

<!-- wp:group -->
<div class="wp-block-group"><!-- wp:paragraph -->
<p>This VC is signed by the private key of an issuer's DID. Microsoft Authenticator, you can also call it user agent or wallet was able to resolve it to a DID document. In the DID document it was able to find the public key and was able to verify the signature. It was also able to check the Linked Domain as well.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>In a typical AAD VC issuance scenario, Microsoft Authenticator will authenticate a user using Open ID Connect to get the VC from AAD VC service by making API calls. AAD VC service will provide a VC which will contain claims about the subject of the VC. On receiving the VC, Microsoft Authenticator will resolve the DID to a DID document, check the Linked Domain and store the VC which can later be used to prove the claims to a VC verifier.</p>
<!-- /wp:paragraph --></div>
<!-- /wp:group -->

<!-- wp:heading {"level":3} -->
<h3>Conclusion</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>I hope, I was able to simplify the very complex working of DID and VC to some extent. There is a complete guide by Microsoft on AAD VC, available <a href="https://docs.microsoft.com/en-us/azure/active-directory/verifiable-credentials/" data-type="URL" data-id="https://docs.microsoft.com/en-us/azure/active-directory/verifiable-credentials/">here</a>. If you want to dive deeper into the mechanics and open standards then have a look at this <a href="https://www.w3.org/TR/did-core/" data-type="URL" data-id="https://www.w3.org/TR/did-core/">documentation</a>.</p>
<!-- /wp:paragraph -->
