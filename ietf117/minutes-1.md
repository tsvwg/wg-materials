# IETF-117 TSVWG Meeting Session 1

Tuesday, 25 July 2023, Session IV 1700-1800

Chairs - Gorry Fairhurst and Marten Seemann

Scribes - Marco Munizaga and Spencer Dawkins

The chairs began by reviewing the Notewell and talked briefly about how the TSVWG working group does its work.

## 1: WG Status and draft updates - Chairs 
RFC's completed: draft-tsvwg-dscp-considerations
        
Refer to the slides for bullet 1: https://datatracker.ietf.org/meeting/117/materials/slides-117-tsvwg-1-chairs-slides
    
With IESG: None.

Drafts beyond WGLC:
* draft-ietf-tsvwg-ecn-encap-guidelines (Writeup Needed) Document Shepherd: David
* draft-ietf-tsvwg-rfc6040update-shim (Writeup Needed) Document Shepherd: David

Both are expected to progress shortly after this meeting and a writeup will be completed,
  
Chairs: Status updates on other WG drafts.

Errata processing: RFC8325
  
Chairs: Milestone updated - see datatracker.
Notices and Related Drafts 
Liaison notices, if any:
* 3GPP Liaison Request - (SCTP & DTLS) Gorry
* https://datatracker.ietf.org/liaison/1847/
* GSMA Liaison Request - (Multi-Path DCCP) Gorry

   Chairs: Announcements and Heads-Up

## 2. Chairs: Using GitHub for tracking adopted WG items

* Marten: Chairs convinced the WG should embrace GitHub. github.com/tsvwg
    * Offer for editors to move drafts to the tsvwg GitHub org.
    * That org will be used for issue tracking whether or not a draft is managed using GitHub.
    * Are there any objections?
        * Magnus Westerlund: No objection, this is a great idea.
        * Bob Briscoe: If GitHub was ever compromised, the world as we know it will end. TSVWG drafts and issues will be the least of our problems!
        * Spencer Dawkins: I am just observing: A lot of objections in other WG are a forced march to manage draft. I would expect very few objections to the style we propose here,
    * Gorry: The way I see this working:
        * We discuss drafts on the mailing list as normal.
        * The chairs or editors create a GitHub Issue, and use that to track the issue.

## 3. Individual Drafts

### 3.1 John Kaippallimalil: Metadata for a Media Packet

draft-kaippallimalil-tsvwg-media-hdr-wireless
       
* https://datatracker.ietf.org/meeting/117/materials/slides-117-tsvwg-31-metadata-for-a-media-packet

* Gorry (as an individual): You do not include IPv6 Destination Options, rather than hop-by-hop options. If the metadata is not modified on the path and passed end-to-end, have you any thoughts on whether you could alternatively use Dest Opt?
    * John: We breifly looked at Hop-by-Hop Options, but did not look at Dest Opt. 
* Gorry: As I understand, the Flow Label is immutable for a flow - it defines what a flow is, if you change that, then you would also make a different flow that is independently forwarded.
    * John: From the wireless node, that could be ideal - but from the perspective of the network (or firewall) this is an issue.

* Tom Herbert: You cannot use dest opts, since acrhitecturely dest opts cannot be processed by intermediate nodes. But, that is also true of UDP Options. One point on the use of IPv6 Flow Label, I would be careful to use just a single mechanism. If you combine two then you create more opportunities for things to go wrong.

* Matt Mathis: There seems to be a huge opportunity and a  lots of allegators. Good luck.

* Magnus Westerlund: Another option exists when yoy are addressing a single node on the path. You could explicitly talk to this. 
* John: I think you refer to using the control plane to signal data for the user plane. I think the preference is to use the user plane.
* Magnus: I think we could use the user plane by addressing the node and linkage it to the data, using a method similar to that being proposed in the SADCDN. This might be a suitable solution also for this?

* Gorry: How many have read this draft (or a recent version)?
    * 14 hands raised.
* Gorry: How many people are interested in work in this space? (Not necessarily using this draft, but interested in this topic as an area of future work).
  
    * Tom Herbert: Clarifying question: what topic?
    * Gorry: Explicit signal infomation to help devices process packets in the network.
    * Bob Briscoe: does this include not talking to the network at all? Yes.
    * -: Could this ne on other layers?
    * 27 noted interest.
    * Gorry: It sounds like this will come up at the next IETF meeting. Use the list to discuss.

John (final comments): We are not thinking of one  solution, but would like a timely solution that we could use, and would think this is a topic the IETF could develop specification.

## 4. Transport: 

### 4.1 UDP Options (15 mins)
#### 4.1.1 Joe Touch*: UDP Options
draft-ietf-tsvwg-udp-options 
   	  - Security Options,  Post WGLC Issues
      
#### 4.1.2 Gorry Fairhurst/Tom Jones: DPLPMTUD for UDP Options
draft-ietf-tsvwg-udp-options-dplpmtud
https://datatracker.ietf.org/meeting/117/materials/slides-117-tsvwg-sessb-412-dplpmtud-for-udp-options

* Tom just got married, and has not seen these slides. The editors think this is ready and waiting for UDP Options to complete. Please send comments to the mailing list!

### 4.2 Other drafts related to UDP Options (if needed). 

* Gorry presented a list of current drafts that rely on UDP options, and the list is growing...

* Martin Duke (AD): There is a class of UDP options that seem like: let the application take some things from the encrypted payload, and put it in the header for other to read and perform some optimizations for performance. We should think hard about that and bring some skeptics before we adopt it. Just because the room thinks this is a valid use, this might not survive last call. We need to think hard about this, and bring in some skeptics to discuss these tradeoffs. This is a general comment.

* Mike Heard: presenting Joe's proposed five basic tenents for UDP options
    * Agree with all except E. Incomplete frameworks could lead to issues. Worthwhile to ask the WF to reply to Joe's message and see if we're on the right track.

* Christian Huitema: I think there should a 6th tenet that UDP options should never be attached to UDP encrypted protocols.
        * Gorry: is there already text in the security considerations that warns of the care needed when the datagram is encrypted?
        * Christian: There was text proposed, but this is not in the current version of the draft. We dio not want tro add information to UDP packets.
        * Mirja : I agree at a higher level, but does it make it stateful.
    * Christian: No, if any router knows the packet is QUIC then it would then drop packets with UDP Options.
    * Mirja: That's even worse, as a security vulnerability: IF an attacker adds UDP Options that would make sure that somebody drops it. I am unsure how it helps.
    * Christian: This is abuse. We should create a blocking point to deployment, to prevent the abuse being engrained in the network and hard to remove.
    * Mirja: Do you wnat a middlebox to figure out that whatever is above the UDP header is encrypted and then to figure-out that this packet is illegal, and at least remove the option.
    * Christian: If it is not the middlebox, then the endpoint should do this.
    * Mirja: The endpoint can always ignore this.
    * Christian: I think the endpoint should destroy the packet.
    * Marten: If you can modify the packet, you can destroy it yourself.
    * Martin Duke (individual): There are two things going on here. Shoiuld we say "don't do this", and should we  also require nodes to enforce that statement. I am little skeptical of the second, I think we should say MUST NOT. 
    * Igor Lubashev: A network could be made to remove the option, if you make it choose that, and the receiver endpoint will not know the option was ever there, so it would not drop anything.
    * Christian: Networks can attach data. If they can remove it, we are in a better place than if they don't. That is better than building an end-to-end communications channel. If you add any clear text to UDP you increase the attack surface. It's an additional weakness that you attach to the protocol. We should not deploy in a secure protocol.
    * Mike Heard: The attack surface already exists. Teh surplus area can already be used, it has existed since UDP was created. There is no new threat.
    * Christian: True, you can add data at the tail of a packet. But now you ask that the endpoint has to process it.
    * Tom Herbert: If we deploy UDP Options and there's ever a DOS attack because of them, there is a risk of network providers dropping all packets with UDP Options. The risk is high for a new protocol that is yet to prove its usefulness and will be cut by operators.

Slide: Basic Three Issues from IETF 116

* Fragmentation, Encryption, Authentication
* Mike: Fragmentation specification is not coherent on these topics, so this needs to be checked, we do not seem to have much feedback on this topic. This is central and needs to be right.
* We do not what the working group thinks on Encryption and Authentication, people have commented on the maturity on the list.
* We need more review on these three topics before the document can move forward.

Gorry: How many people want to work to complete the encryption part of this draft? 
    Raised: 1, didn't raise: 16

Gorry: Should we complete authentication work as part of this document (if not, we could progress as a separate draft)?
    Raised: 3, didn't raise: 11
    
Gorry: We are going to continue this on the mailing list, who had objections?

Martin Duke: Clarifying the question on the encryption topic - it seemed pretty clear that people did not want to do this particular method?

Gorry: I interpret this as we do not have a complete spec for doing this, to make this an interoperable specification. If we complete UDP Options, we could do it later. We only had one person raise their hand.

Martin Duke: That was me, and I wanted to see work on this topic in the future.

Gorry: Please let the WG know your thoughts and try to complete this work. 

End of Session 1

# IETF-117 TSVWG Meeting Session 2

Thursday, 27 July 2023, 9:00 AM PST

Chairs - Altanai B

Scribes - Marco Munizaga and Spencer Dawkins

## 5. Agenda recap and notices 

The chairs began by reviewing the Notewell and talked briefly about the agenda for the TSVWG working group.

Please look at the other individual drafts and discuss on the list.

## 6. Gorry Fairhurst: Careful Convergence of Congestion Control from Retained State

draft-ietf-tsvwg-careful-resume-01
Nicolas Kuhn, Stephan Emile, Gorry Fairhurst, Christian Huitema

    update
    State of CR Implementation
    Simulated performance graphs
        State of CR Implementation
        Gain in RTTs vs transfer size
        Pacing during Reconnaissance

Q Martin Duke: When do you cache the cwnd?
A Gorry: Our draft talks about this.

Q Matt Mathis : Double window problem?
A : tbd

Q Kazuho Oku: Why is the Reconnaissance phase needed in QUIC?
A : To not jump immediately if path changes. Determine its not congested and RTT is same before the jump.

Q Michael Tüxen: How long in the past can this info be?
A : Control block sharing is simmilar.

    Flow control
        rwnd/flow credit is set by the receiver

Q Matt Mathis : Tradefoff in design problem such as how to decay information has been an open issue in ML? Should be learned fro own traffic
A : More work needed.

    Planned next steps IETF118
        Safe retreat

### 6.2 Markus Ahmed: MP DCCP progress

draft-ietf-tsvwg-multipath-dccp-10

    Draft status
        review
        status changes EXP to PS
        Linux kernel ref implementation
    Optimized handshaking procedure
        limitations of MP TCP
        call flow
        consequences
        Question to community

Gorry Fairhurst : I like this approach. Is it simmilar to MP QUIC?
A : Yes, basic idea is same.

    summary
        pending issues
        roadmap

Q chair : How long to resolve pending issues?
A : ~1.5 months. Nothing in issue tracked yet. Confident in interop test.

    hum call to audience to indicate if its ready.
        less participation in hum
        outcome :
            more revisions needed before Working group can adopt
            milestone to be updated
            Will do last call on working group list before next IETF

## 7. SCTP Drafts
  
### 7.1 Magnus Westerlund: DTLS in SCTP
https://datatracker.ietf.org/doc/draft-ietf-tsvwg-dtls-over-sctp-bis/

https://datatracker.ietf.org/doc/draft-westerlund-tsvwg-sctp-crypto-chunk/
https://datatracker.ietf.org/doc/draft-westerlund-tsvwg-sctp-crypto-dtls/

Magnus Westerlund 
John Preuß Mattsson 
Claudio Porfiri

    goals
        meet 3GPP request
        DTLS 1.3 - draft-tuexen-tsvwg-rfc6083-bis
    IPR Declarations
    Background
    Liaison statement from 3GPP SA3

Q Hannes Tschofenig : What is 3GPP trying to constrain regarding message size ?
A : Wait for coming slide

    Requirnments
    Q Hannes Tschofenig : usecase from Networking euipment manufacturer. More expectation from the specification.
    A : Individual message could grow rather than theoritical maximum message size. ( check with speaker later )

Q Michael Tuxen : Practical limts needs like Diameter has 24 bit len.
A : Solution needs to be analysed …

Q Hannes Tschofenig : Rekeying protocols like DTLS and TLS been used in industrial application. Requirnemnts should not be solutions.
A : it is easy to tear down and restart for handshakes in other usecases but not here. Security considerations in document mention the requirnments such as priotity rekey and priority authentication.

    DTLs over SCTP
        packet structure
    Proposal of DTLs in crypto Chunk
    DTLS implementation requirments
        Connection IDs

Q Hannes Tschofeing : what does stack refer to ? There is no off-the-shelf stack for features presented.
A : That supports feature needed based on available such as WolfSSL, MbedTLS. Example presented such as turning off replay protection.

Q Michael Tuxen : Both solution have IPR which have limited use. Ask to ficus on one DTLS version instead of 2 which complicated either.
A : Already considered. No.

    Rekeying Robustness
        DTLS in SCTP chunks
            reordering and losses of packets

Q Michael Tuxen : API exist in document for Socket API.
A : Agree. Requires tracking.

    conclusion
        way forward : interim meeting in September
        Send liaison statement to 3GPP SA3 and RAN3

Q Hannes Tschofeing : SCTP is not only used by 3GPP but other. Get more poeple on board.
A : Cant find more memebers who are relevant.
A (chair) : Should be useful for most people.

### 7.2 Michael Tuexen: Zero Checksum for SCTPdraft-ietf-tsvwg-sctp-zero-checksum

    motivation
        SCTP over DTLS as an error detection method
    changes
    implementation status
    Next steps
        revision 02 with feedbacks
        Last call request

Q Gorry Fairhurst : How does this do IPv6 ? handle extra requirnemtns from IPv6. Should be clear such as transport checksum.
A : Doc says 2 conditions must be fulfilled which includes alternate methods and no middle boxes which ensure non zero checksum. Thus this is not applicable to IPv6.

    Poll: How many people read the document ?
    Y : 7, N : 24

    Hum for working group Last call
    Low hum

    Hum for major items to be addreessed ie its not ready
    No hum. Thus this is in a good position for a new revision.
### 7.3 Michael Tuexen: SCTP/UDP  6951.bis (5 mins)RFC 6951bis: SCTP/UDP
draft-tuexen-tsvwg-rfc6951-bis-03

    motivation

## 8. ECN & L4S Drafts
   
### 8.1 Jason Livingood: L4S Experimental Deployment
draft-livingood-low-latency-deployment

    draft update
    update on experimental deployment such as ECN marking problem
    deployment in virtual CMTS (vCMTS)
    preparing test assignments for diagnostics
    read out test results in IETF118
    No discussion on WGLC

##   8.2 Greg White: L4S Operational Guidance on Coexistence with Classic ECN during L4S Deployment

draft-ietf-tsvwg-l4sops-05

    scope and status
        adresses possible rate-imbalance in shared- queue RFC3168 bottlenecks
        living document to capture experiences for initial L4S deployment
        draft 05 changes

Q Tim Chown : Measurement and impact is ongoing in other groups. What are your thoughts on general guidance on measuring impact such as responsiveness such as latency?
A : Experiments were done in interops and have a straw-man test plan for lab test. But could be used in field test. Open area for further L4S deployment in equipment using CE marking and CC.
A chair : Tools avaiable for testing should be mentioned. Continue on mailing list.

    L4S interops activities
        invitation to join

## 8.3 Greg White : Non Queue Building (NQB) Per Hop Behavior
draft-ietf-tsvwg-nqb-19

    status draft 17
    screen shot of many issues
    post WGLC comment
    draft 18 highlights
    draft 19 highlights
    status of issues
    Next steps

Q Gorry : appreciate support for RFC 8325 Mapping Diffserv to IEEE 802.11
A : _

    Hum if if you have read this or recent version of this daft.
    Low hum

    Hum if this document is not ready to be received ie not start a working group last call
    No hum
    (remote participants could unfortunately not participate in the hums)

Mikhel Abrahamsson : Example configs to implement this in common network are needed.
Martin Duke : This should not be published (relayed from chat by remote participant, not Martin’s own position)

    To be put on agenda in IETF118

## 9. Individual Drafts

### 9.1 Dan Wing : Encrypted Transport Protocol Path Explicit Signals
draft-reddy-tsvwg-explcit-signal-01

    problem statement
    Network-Layer verses Transport-Layer signal
    solution overview
        UDP option for “tag”
    Design principle
        encrypted and obfuscated
    system diagram

Matt Joras: Secure information exchange is presented before but it was not accpeted. BoF was the dispatch feedback.
TSV accepts

Tom Herbert: Already discussed in one to one wireless media header. not architectural correct. contradicts core architecture. Practical issues with middleboxes. IPv6 extension headers has been seen before. Hop by hop processing draft is also not applicable.
Network signalling from host is a common problem and should find a common generic solution. Maybe an INT area subject.

Chair : QoS aspects fall in this group such as queues. INT area defines encapsulation.

Christian : This is not a UDP option function which is end to end and supposed to be used by transport and not be intermediries. A new mechanism should be diff and not be part of UDP options.

Tianji Jiang : I like this idea as we have a real case. Media header IP draft could be used. 3GPP could be mapped to this header.

Q C.Heard : agree with earlier comments from Tom and Christian. Oauth applicability. Networking is modifying thiese option in transit ?
A : no modification of tags in transit.

Kazuho Oku : closed information should have better treatment. Packets should not be dropped in transit. Sender should decide.

Tim Chown : Common requirements for marking such as hop by hop.

Marco Munizaga: Extension headers seem to be useful. Should focus on better support rather than a new thing.

Chair : intention for next meeting
A : particiapte in BoF for refinement.
