# IETF-117 TSVWG Meeting 1

Tuesday, 25 July 2023, Session IV 1700-1800
Room Name: Continental 5 

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
