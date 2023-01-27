# General Principles

1. **Respect user privacy**: All users have the right to privacy and control over
   their own data. Any data processing or analysis must be done with explicit
   user consent.

2. **Transparency**: Users have the right to know what data is being collected and
   how it is being used. Any data processing or analysis must be fully
   transparent to the user.

3. **Data security**: All user data must be kept secure and protected from
   unauthorized access.

4. **Fair usage**: The SSB protocol must be used for lawful and fair purposes only.
   Any attempts to manipulate or exploit the system for personal gain or
   malicious intent will not be tolerated.

5. **Open-source**: The SSB protocol and any associated software must be
   open-source and freely available for examination and modification by the
   community.

6. **User control**: Users must have full control over their own data and the
   ability to request replicator to delete it at any time.

7. **Respect for identity**: The SSB protocol must respect the subjectivity and
   diversity of identities and not discriminate based on any protected
   characteristic.

8. **Community collaboration**: The SSB community must work together to address
   any issues or concerns that may arise with the protocol.

# Code of Conduct for Developers and Applications on the Secure Scuttlebutt Mainnet

## Introduction

The Secure Scuttlebutt (SSB) protocol is a decentralized, peer-to-peer
communication network that allows users to share information and interact with
one another in a secure and private manner. The SSB mainnet is the backbone of
this network, and it is essential that developers and applications that connect
to it adhere to a set of guidelines to ensure the security and privacy of users,
as well as the integrity of the network. This code of conduct is intended to
provide developers and applications with a clear understanding of their
obligations and the expectations for their use of the SSB mainnet. By adhering
to these guidelines, developers and applications can help to maintain the health
and security of the SSB network and ensure a positive experience for all users.

It is important to note that the SSB protocol is open-source and can be reused
to create other independent networks. If you are planning to create an
alternative network using the SSB protocol, it is requested that you clearly
specify that it is not using the SSB mainnet and if it complies with this code
of conduct. Additionally, we ask that proper references are made to the SSB
community and that the SSB protocol is used in this alternative network in
accordance with the same values and principles upheld by the SSB mainnet.

## Definitions

### Developer:

An individual or organization that creates and maintains an application that is
intended to be used on the SSB mainnet. A developer is identified by their Feed
ID and is expected to publish and maintain a DID document (ssb-did) on their
feed. If the developer is an individual, they may provide personal information
in the DID document such as their name and contact information. If the developer
is a company, they must provide information such as the company name, contact
information, and any relevant registration or incorporation details.

Example DID Document for a Developer feed:

```` json
{
  "@context": "https://w3id.org/did/v1",
  "id": "did:ssb:ed25519:aaTWXmj9rNHWl3_Z_uomAkf_5WzjLhZPSmAtFrgE20o",
  "publicKey": [
    {
      "id": "did:ssb:ed25519:aaTWXmj9rNHWl3_Z_uomAkf_5WzjLhZPSmAtFrgE20o#keys-1",
      "type": "Ed25519VerificationKey2018",
      "publicKeyBase64": "aaTWXmj9rNHWl3_Z_uomAkf_5WzjLhZPSmAtFrgE20o"
    }
  ],
  "service": [
    {
      "id": "did:ssb:ed25519:aaTWXmj9rNHWl3_Z_uomAkf_5WzjLhZPSmAtFrgE20o#services-1",
      "type": "SSBDeveloper",
      "serviceEndpoint": {
        "name": "John Doe",
        "email": "johndoe@example.com",
        "companyName": "Acme Inc.",
        "url": "https://acmeinc.com"
      }
    }
  ]
}
````

### Application:

An Application is a computer program that utilizes the Secure Scuttlebutt (SSB)
protocol and potentially interacts with other applications connected to the SSB 
mainnet.
This means that these applications can interoperate and potentially replicate
data owned by end-users. An application is declared by the Developer by creating
a new Feed for the application, which is then identified by its FeedID. It is
expected that a DID (Decentralized Identifier) document describing the
application will be published and maintained at each release of the
application (ssb-did). The DID controllers will be the Developer's DID and the
Application's DID itself, and both must sign the DID document. Additional
information contained in the document includes the hash of the binary
distributed to allow for verification of integrity, the version of the
application, and a description of the features and access rights of the
application.

By providing this information, users will be able to make an informed decision
about whether to use the application and trust it with their data. 
Additionally, this will help to build trust in the SSB ecosystem by promoting 
transparency and accountability among application developers.

```` json
{
  "@context": "https://w3id.org/did/v1",
  "id": "did:ssb:ed25519:J4OW0YVV+OpyZUOzrD+oLC1jpnaIOWjjEPVL/mhaVjE",
  "publicKey": [
    {
      "id": "did:ssb:ed25519:J4OW0YVV+OpyZUOzrD+oLC1jpnaIOWjjEPVL/mhaVjE#keys-1",
      "type": "Ed25519VerificationKey2018",
      "publicKeyBase64": "J4OW0YVV+OpyZUOzrD+oLC1jpnaIOWjjEPVL/mhaVjE"
    }
  ],
  "service": [
    {
      "id": "did:ssb:ed25519:J4OW0YVV+OpyZUOzrD+oLC1jpnaIOWjjEPVL/mhaVjE#services-1",
      "type": "SSBApplication",
      "serviceEndpoint": {
        "name": "Manyverse",
        "version": "1.0.0",
        "description": "A mobile-first, offline-first, secure, private, and decentralized social network.",
        "release-notes": "describe here the changes between versions, especially meaning full changes in requested data processing",
        "hashMultibase": "0x1234567890abcdef",
        "data-processing": [  // describes the data processing requested by the application when following
           "publish post on UI",
           "index using ssb-search2"
        ]
      }
    }
  ]
}
````

### User

A user is an individual who uses an application on the SSB mainnet. A user is
identified by their Feed ID and is expected to publish and maintain a DID
document (ssb-did) on its feed.  Using that DID document, the user can express
which applications (and version) they trust to process their data (todo: 
check if Key Agreement Relationship is appropriate or if another 
verification method should be used).  

In this DID document, the user can also provide a blacklist of FeedID that he 
explicitly does not want to replicate its data. (Note: idea here is to use a ZKP 
scheme that permit anybody to check to check that a given ID is part of the 
blacklist without being able to list all the IDs in the blacklist.  Note: unsure 
how to include that in the DID specification, maybe using Assertion Verification 
Method)

## General description of the protocol

An SSB application that want to replicate a given feed from another peer of the 
network must first provide to the peer its latest DID document. The peer can 
then check if the feed requested is part of the approved application by the 
owner and if the requesting peer is not part of the blacklist of the owner.

An SSB Application that receive an unknown Application DID document from a peer
must resolve the DID document of the developer and present to the User all 
the information contained in the DID document of the application and the
developer. The user can then decide to trust the application and the developer
and add the application to its list of trusted application (and update and 
publish its own DID document accordingly).  This permit to 
progressively disseminate the knowledge of new release through the network 
and to make the consent to accept new features in applications controlled by 
the User.

The User must be able to revoke the trust in an application version at any 
point in time (and update and publish its own DID document accordingly).

The User may also decide to blacklist a given application version (and 
update and publish its own DID document accordingly).  When doing so, it is 
expected to provide a reason for the blacklist. This reason will be gathered by 
the User application from his followed feeds and will be presented to the User 
when he will be asked to accept the new version of the application.  If the 
ratio of blacklist commitment is reaching a configuration high percentage (e.g. 
50%) of its first circle of trust, the User application should warn the User 
that the application is not trusted by a significant number of its followers.
This mechanism will help to build trust in the SSB ecosystem by allowing a rapid
propagation of information about malicious application while leaving the 
User the ultimate decider of which application he trusts.

## Data privacy and security

One of the key principles of the SSB protocol is the protection of data privacy
and security. Therefore, all applications and developers utilizing the SSB
mainnet are expected to implement appropriate measures to safeguard user data.
This includes, but is not limited to, implementing secure storage and
transmission of user data, ensuring that user data is only accessed with
explicit consent, and implementing regular audits and security testing to
identify and address potential vulnerabilities.

Developers and applications are also expected to fully disclose any data
collection and usage practices to users and obtain explicit consent for such
practices. Users should also have the ability to revoke trust in an Application
and the Application is expected delete the data of the user.

In the event of a data breach, developers and applications are expected to
quickly notify users and take appropriate measures to mitigate the impact of the
breach. They should also report the incident to the SSB community and work with
them to resolve the issue.

Formally, an SSB Developer discovering a security vulnerability in a version 
of the Application must notify the SSB community by updating the DID 
document and publishing an assertion that the version is compromised.

Overall, the protection of user data is of the utmost importance and developers
and applications utilizing the SSB mainnet are expected to prioritize and
consistently maintain the security and privacy of their users' data.

## Compliance with SSB protocols

A description of the protocols and standards
that must be followed when developing and deploying applications on the SSB
mainnet, including message and feed replication, peer-to-peer communication, and
data integrity.

The mechanism proposed implies that all Application connecting to the SSB
mainnet must implement the SSB-DID protocol. An additional protocol may be
needed or included in the SSB-DID protocol to implement the communication by 
the connecting peers of their SSB-DID documents at the beginning of the 
communication session after the SHS handshake.
Once the SSB-DID document is exchanged, the connecting peers must ensure to not
provide any data to the other peer until an explicit consent was given by the 
data owner (the latest DID Document that it is aware of is the only relevant 
for that perform that checks).

## User control and transparency

TODO: A description of the obligations of
developers and applications to provide users with control over their data and to
be transparent about data usage and sharing.

## Community guidelines

TODO: A description of the guidelines for appropriate
behavior and engagement within the SSB community, including guidelines for
communication and collaboration with other developers and users.

## Dispute resolution

TODO: A description of the process for resolving disputes or
violations of the specification, including the roles and responsibilities of
developers, applications, and the SSB community.

## Conclusion: 

TODO: A summary of the key points of the specification and a call to action for 
developers and applications to comply with the guidelines to ensure a
healthy and secure SSB mainnet.
