# TSVWG Meeting 1

Tuesday, 25 July 2023, Session IV 1700-1800
Room Name: Continental 5 

Scribess - Marco Munizaga and Spencer Dawkins

The chairs began by reviewing the Notewell and talked briefly about how the TSVWG working group does its work.

## 1: WG Status and draft updates - chairs (15 minutes)
RFC's completed: draft-tsvwg-dscp-considerations
        
Refer to the slides for bullet 1: https://datatracker.ietf.org/meeting/117/materials/slides-117-tsvwg-1-chairs-slides
    
With IESG: None.

Drafts beyond WGLC:
* draft-ietf-tsvwg-ecn-encap-guidelines (Writeup Needed) Document Shepherd: David
* draft-ietf-tsvwg-rfc6040update-shim (Writeup Needed) Document Shepherd: David
  
Chairs: Status updates on other WG drafts:

Errata processing: RFC8325
  
Chairs: Milestone updates - see tracker
Notices and Related Drafts 
Liaison notices, if any:
* 3GPP Liaison Request - (SCTP & DTLS) Gorry
* https://datatracker.ietf.org/liaison/1847/
* GSMA Liaison Request - (Multi-Path DCCP) Gorry

   Chairs: Announcements and Heads-Up (10 Mins) 

## 2. Chairs: Using GitHub for tracking adopted WG items (10 mins)

* Marten: Charis convinced we should embrace GitHub. github.com/tsvwg
    * Offer for editors to move drafts to the tsvwg GitHub org.
    * That org will be used for issue tracking whether or not the draft is managed using GitHub.
    * Objections?
        * Magnus Westerlund: No Objection, great idea
        * Bob Briscoe: if GitHub ever goes away, the world as we know it will end. TSVWG drafts and issues will be the least of our problems!
        * Spencer Dawkins: Just observing. A lot of objections in other wg are a forced march to manage draft. I would expect few objections
    * Gorry: The way I see this working:
        * We discuss on the mailing list
        * Create a GitHub Issue, and use that to track it.

## 3. Individual Drafts

### 3.1 John Kaippallimalil: Metadata for a Media Packet (15 mins) - new work

draft-kaippallimalil-tsvwg-media-hdr-wireless
       
* https://datatracker.ietf.org/meeting/117/materials/slides-117-tsvwg-31-metadata-for-a-media-packet

* Gorry: As an individual. You don't include destination options, rather than hop by hop. Any htoughts on usion dest opts
    * John:  We breifly looked at hop-by-hop, but didn't look at dest opts. 
* Gorry: (note taker missed this) Something about Flow label...
    * John:

* Tom Herbert: You can't use destination opts, they can't be modified by intermediate nodes. But that's also true of UDP options. One point on the use of flow label, I would be careful to use a single mechanism. Hoping most of the traffic uses flow label, except... The problem is when you fallback to something else, that something else becomes the bottle neck and slows down the network. Prefer a single thing, not two or more.

* Matt Mathis: Lots of aggregators. Hard problem, good luck.

* Magnus Westerlund: You are processing this at a single node. Processing this at the user plane will be easier

* Gorry: Interested to know how many have read this draft?
    * 14 hands raised.
* Gorry: How many people are interested in work in this space? Not necessarily using this draft, but interested in this topic as an area of future work.
    * Tom Herbert: Clarifying q, what area?
    * Gorry: Explicit infomation to let devices signal information to the network.
    * Bob Briscoe: does this include not talking to the network at all? Yes.
    * 27 noted an interest.
    * Gorry: Sounds like this will come up at the next IETF meeting. Use the list to discuss.

John (final comments): Not picky on any solution, but would like a timely solution that we could u

## 4. Transport: 

### 4.1 UDP Options (15 mins)
#### 4.1.1 Joe Touch*: UDP Options
draft-ietf-tsvwg-udp-options 
   	  - Security Options,  Post WGLC Issues
      
#### 4.1.2 Gorry Fairhurst/Tom Jones: DPLPMTUD for UDP Options
draft-ietf-tsvwg-udp-options-dplpmtud
https://datatracker.ietf.org/meeting/117/materials/slides-117-tsvwg-sessb-412-dplpmtud-for-udp-options

* Tom just got married, and hasn't seen the slides. Please send comments to the mailing list!

### 4.2 Other drafts related to UDP Options (if needed). 

* Gorry presented a list og drafts that rely onUDP options, and the list is growing

* Martin Duke: Applications of UDP options. There is a class of UDP options that is let me take some things from the encrypted payload, and put it in the header for some optimizations. We should think hard about that and bring some skeptics before we adopt it. It may not survive last call. A general comment.

* Mike Heard: presenting Joe's proposed five basic tenents for UDP options
    * Agree with all except E. Incomplete frameworks could lead to issues. Worthwhile to ask the wg to reply to Joe's message and see if we're on the right track.

* Christian Huitema: I think there should a 6th tenet that UDP options should not attach to encrypted protocols.
* Mirja : I agree at a higher level, but doesn't it make it stateful?
    * CH: No
    * Mirja: That's even worse I add UDP options and make somebody drop it
    * CH: 
    * Christian: the endpoint should do that
    * Mirja: but this looks like an attack.
    * Marten: If you can modify the packet, you can destroy it yourself
    * Martin Duke: There are two things going on here. We can say "don't do this", and we can also require nodes to enforce that statement. I think we should say MUST NOT 
    * Igor Lubashev: Saying MUST NOT attach to encrypted protocols we can do.  But if the option is used to signal something to the co-operating network, the network can remove the option as it forwards the packet, and the receiver endpoint will not know the option was ever there, so it would not drop anything.
    * CH: Networks can attach data. If they do remove, we are in a better place than if they don't. That's better than building an end to end communications channel. If you add anything to UDP you increase the attack surface. It's an additional weakness that you attach to the protocol. We should not deploy in a secure context.
    * Mike Heard: The attack surface already exists. There is no new threat.
    * CH: True you can add data at the end of the packet. But now if you ask the endpoint to process it.
    * Tom Herbert: If we deploy UDP options and there's a DOS attack because of them, there is a risk of network operators dropping all packets with UDP options. The risk is high for a new protocol that's yet to prove its usefulness and will be cut by operators.

Slide: Basic Three Issues from IETF 116

* Fragmentation, Encryption, Authentication
* Specification isn't coherent on these topics, so needs to be fixed
* The editors don't know what the working group thinks, so we need more review before the document can move forward

Gorry: How many people want to work on the encryption part of this draft? 
    Raise 1, didn't raise 16

Gorry: Should we complete authentication work as part of this document or separate draft?
    Raise 3, didn't raise 11
    
Gorry: We are going to progress on the mailing list

Martin Duke: Clarifying q on the encryption q

Does it mean we don't want to improve...

Gorry: If we complete UDP Options, we could do it later. We only had one person raise their hand.

Martin Duke: That was me.
