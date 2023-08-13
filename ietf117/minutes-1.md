# IETF-117 TSVWG Meeting Session 1

Tuesday, 25 July 2023, Session IV 1700-1800

Chairs - Gorry Fairhurst and Marten Seemann

Scribes - Marco Munizag* and Spencer Dawkins

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
        * Spencer Dawkins: I a observe a lot of objections in other WG are a forced march to manage draft. I would expect very few objections to the style we propose here,
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

* Matt Mathis: There seems to be a huge opportunity and there are lots of allegators. Good luck.

* Magnus Westerlund: Another option exists when yoy are addressing a single node on the path. You could explicitly talk to this. 
* John: I think you refer to using the control plane to signal data for the user plane. I think the preference is to use the user plane.
* Magnus: I think we could use the user plane by addressing the node and linkage it to the data, using a method similar to that being proposed in the SADCDN. This might be a suitable solution also for this?

* Gorry: How many have read this draft (or a recent version)?
    * Have read: 14 hands raised.
* Gorry: How many people are interested in work in this space? (Not necessarily using this draft, but interested in this topic as an area of future work).
  
    * Tom Herbert: Clarifying question: what topic?
    * Gorry: Explicit signal infomation to help devices process packets in the network.
    * Bob Briscoe: Does this include not talking to the network at all? Yes.
    * -: Could this be on other layers?
    * 27 noted interest.
    * Gorry: It sounds like this will come up at the next IETF meeting. Use the list to discuss.

John (final comments): We are not thinking of one  solution, but would like a timely solution that we could use, and would think this is a topic the IETF could develop specification.

## 4. Transport: 

### 4.1 UDP Options (15 mins)
#### 4.1.1 Joe Touch : UDP Options
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
        * Christian: There was text proposed, but this is not in the current version of the draft. We dio not want to add information to UDP packets.
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
    * Mike Heard: The attack surface already exists. The surplus area can already be used, it has existed since UDP was created. There is no new threat.
    * Christian: True, you can add data at the tail of a packet. But now you ask that the endpoint has to process it.
    * Tom Herbert: If we deploy UDP Options and there's ever a DOS attack because of them, there is a risk of network providers dropping all packets with UDP Options. The risk is high for a new protocol that is yet to prove its usefulness and will be cut by operators.

Slide: Basic Three Issues from IETF 116

* Fragmentation, Encryption, Authentication
* Mike: Fragmentation specification is not coherent on these topics, so this needs to be checked, we do not seem to have much feedback on this topic. This is central and needs to be right.
* We do not what the working group thinks on Encryption and Authentication, people have commented on the maturity on the list.
* We need more review on these three topics before the document can move forward.

* Gorry: How many people want to work to complete the encryption part of this draft? 
    * Raised: 1, didn't raise: 16
* Gorry: Should we complete authentication work as part of this document (if not, we could progress as a separate draft)?
    * Raised: 3, didn't raise: 11
    
        * Gorry: We are going to continue this on the mailing list, who had objections?

        * Martin Duke: Clarifying the question on the encryption topic - it seemed pretty clear that people did not want to do this particular method?
        * Gorry: I interpret this as we do not have a complete spec for doing this, to make this an interoperable specification. If we complete UDP Options, we could do it later. We only had one person raise their hand.
        * Martin Duke: That was me, and I wanted to see work on this topic in the future.
        * Gorry: Please let the WG know your thoughts and try to complete this work. 

End of Session 1

# IETF-117 TSVWG Meeting Session 2

Thursday, 27 July 2023, 9:00 AM PST

Chairs - Gorry Fairhurst and Martin Seeman

Scribes - Altanai B

## 5. Agenda recap and Notices 

The chairs began by reviewing the Notewell and talked briefly about the agenda for the TSVWG working group.

Please look at the other individual drafts and discuss on the list.

## 6. Gorry Fairhurst: Careful Convergence of Congestion Control from Retained State

draft-ietf-tsvwg-careful-resume-01
Nicolas Kuhn, Stephan Emile, Gorry Fairhurst, Christian Huitema

    The draft was adopted, and has been updated to harmonise language and improve the spec.
    State of expermintal CR implementation
    Simulated performance graphs
        State of CR Implementation
        Gain in RTTs vs Transfer Size
        Pacing during Unvalidated Phase is Needed, permitted during the Reconaisance Phase

        * Martin Duke: is the 4xRTT improvement relative to a jump to the BDP or to doing nothing.
        * Gorry: It is relative to slow start, if you jump to the BDP you can win or loose, depending on whether it proved safe to do this.
  
        * Martin: There is more work: The cwnd resumption is communication from the sender and communicate to itself. The naive way is to cache the BDP at some random time on the sawtooth, burt this is onlty a part of the problem. When do you cache the cwnd?
        * Gorry: That'a na Elephant in the Room I was avoiding, because it is work to come: Our draft talks about this as the observation phase, this needs more work, please help.
        * Christian: If you look at a random time, you do get a random result. We should add some guidance to this draft. What is done in the PicoQUIC implemenation is that PicoQUIC waits for the slow start to complete, and then the connection to stabilise and then saves the BDP and RTT. That's one method, there may be others.
        * Gorry: Quite so. It also can be very wrong if you measure flight size at the end of the connection, where the packets in flight can be near zero.

        * Matt Mathis : Is this all with QUIC. Did you look at TCP? have you dealt with the receiver window (rwnd) problem?
        * Gorry: Yes, this is important, more later.
        * Kazuho Oku: I understand the validation phase (reconaissance) is needed for TCP. Why is the Reconnaissance Phase needed in QUIC, because there is accurate ACK information?
        * Gorry: The Reconnaissance Phase is primarily (i) to not jump immediately if path changes and (ii) confirm the path is teh same - that needs multiple RTT samples. Determine its not congested and RTT is same before the jump. We should discuss how this is done for QUIC.

        * Michael Tüxen: How long in the past can this information remain valid?
        * Gorry: Yes, another thing to resolve: How long in the past ought you to believe the infomation remains valid. TCP Control Block Sharing has similar concerns, which we hope to have more startiung point.

    Flow Control
        rwnd/flow credit is set by the receiver, and would constrain the benfit, unless we update this.

        * Matt Mathis : Receiver window tuning is to control memory usage and is a tradefoff in the design. There are various methods, and QUIC, like TCP, needs to deal witrh this problem.
        * Gorry: Yes.
        * Matt: The problem (earlier) of how to decay information has been an open issue. I'd like to encourage someone to do resaerch into ML strategiesfor this. It should be learned for a connection's own traffic.
        * Gorry : If I understand correctly, the Observation Phase is something that you need to learn in different environments?
        * Matt: Yes, from your own traffic and mix - different cases will have different results.
        * Gorry: Agree, more work needed, please help decide what we should write.

    Planned next steps for IETF-118
    Safe Retreat, and share results (possibly in ICCRG).

### 6.2 Markus Ahmed: MP DCCP Progress

draft-ietf-tsvwg-multipath-dccp-10

    Draft status
        Review
        Status Changed from EXP to PS
        Linux kernel refernce Implementation
    Optimized Handshaking Procedure
        Will consider a change from the MP-TCP handhsake, because DCCP is not constrained in the same way.
        Similarly the key exchnage procedure could be updated after review comments.

        * Gorry (individual) : I like this approach, it is a good call to do this, if we do not need to do this. Is it  simmilar to MP QUIC?
        * Markus : Yes. I think they use a CID, the basic idea is same.
        * Gorry : Please confirm with someone to check if this new part is similar to MP-QUIC.

        * Chairs : How long do you need to resolve the pending issues?
        * Markus : ~1.5 months. We have PRs to merge, but no pending issues at the moment. We are confident in other aspects from the interop tests that we presented at the last meeting.

        * Chairs: Please hum if you think this document will be ready to proceed when the current issued are addressed !
         *  Ready: Some people;  Not Ready (or may have issues): No/few people.
  
        * Chairs: We do not see opposition to a Working Group Last Call.
Another revision(s) will be needed, and the 
milestone is updated to show intended publication as a PS. The Chairs plan to start
        a WG last call on working group list before the next IETF meeting.

## 7. SCTP Drafts
  
### 7.1 Magnus Westerlund: DTLS for SCTP
draft-ietf-tsvwg-dtls-over-sctp-bis
draft-westerlund-tsvwg-sctp-crypto-chunk
draft-westerlund-tsvwg-sctp-crypto-dtls

Magnus Westerlund 
John Preuß Mattsson 
Claudio Porfiri

There is liaison statement from 3GPP SA3, and we plan to update 3GPP on progress.
IPR declarations for various options.

* Hannes Tschofenig : What is 3GPP trying to constrain regarding the message size ? I am uncertain about this.
   * Magnus : The message size (see slide) is s1-AP - 142 KB and Xn - 500 KB.
   * Hannes : Where do these actual numbers come from?
   * Magnus : These come from the layers built on the 3GPP stack that generates these messages.
   * Hannes  : I always concerned when requirements are generated in one organisation and then passed-on to another and the same actors are in both parts of the process. I was expecting more description about why this is needed.
   * Magnus : The messages need to be more than 16 KB. The more UEs in a cell, the larger the message. This is a real problem. Individual message could grow rather than theoritical maximum message size. ( check with speaker later )
   * Michael Tuxen : Can we get some practical limts, for what we need to deal with. For instance, the need in Diameter comes from a 24 bit length.
   * Hannes : We could argue that DTLS 1.3 is a modern protocol, and we do not need this rekey.
   * Magnus : We need long-lived sessions, or thus will need to tear down and restart a DF handhake. This is different.
   * Hannes : Some industrial IoT applications are also long-lived. I am suspcious whether the requirements came first or the solution?
   * Magnus : There are references in the 3GPP litterature to the requirements. The Security Considerations in document mention the requirnments such as priotity rekey and priority authentication.

* Hannes : What does the stack availability refer to ? Is there an off-the-shelf stack for the DTLS features presented? WolfSSL has a TLS 1.3 and DTLS 1.3 stack... There is a 1.2 stad in MbedTLS.
   * Magnus: That supports feature needed based on available such as WolfSSL, MbedTLS. I have not looked at whether these stack support turning off the replay protection. At this point in time this is what is available, it is not available off the shelf.
   * Hannes : The new stuff is not implemented anyway. The TLS WG has 20+ years of features, there is no such thing as an of-the-stack shelf. Many things are not implemented in any stack.
  
* Michael : Both of the solutions have IPR, which make this of limited use in open source stacks. Can we focus on one DTLS version instead of two?  Can we use just 1.2 or 1.3, we need to choose.
   * Magnus : Already considered. The previous answer was "no", but we should bring this to the next meeting to see if this is now possible.

* Michael : We have for many years defined socket API recommendations in the RFC. There is a way to do what is proposed by specifying the API. We have not done this for DTLS, but we could add an informative API.
   * Magnus : I agree. This requires draining at the end of the epoch.
   * Michael : You need to drain, unless you keep the keys from the previous epoch.
   * Magnus : Yes, and this is possible.

The conclusion is we need to find a way forward: We have talked to the chairs and expect we could make progress using a WG interim meeting on 19th September 2023 (16:00 CET). Before that, the Chairs plan to send a liaison statement to 3GPP SA3 and RAN3 toi inform about this meeting and ask for clarification.


* Hannes  : SCTP is not only used by 3GPP, but also be good to get other uses on these topics. We need to get more people on board.
 
Chairs: We plan to schedule the interim that was mentioned, and this would be a good time to look at the various aspects: including open source usages, and to answer this 3GPP liasion. This would be an ideal time for other people to come and help talk about other uses and needs. We could finally end up with 1,2, or 3 documents that meet the needs of users. Please think and prepare for that meeting and think about whether TLS 1.2 is still needed. By the Pargue IETF, we would like to update the milestone.
  
### 7.2 Michael Tuexen: Zero Checksum for SCTPdraft-ietf-tsvwg-sctp-zero-checksum

* Michael : This document seems, to the authors, to be ready for last call. 
* Gorry (individual) : How does this relate to IPv6 requirements? There are additional requirnemtns from IPv6 relating to UDP checksums and these ought to apply. We should be clear about the usage of the transport checksum with IPv6.
* Michael : The draft says two conditions must be fulfilled which includes alternate methods and no middle boxes which ensure non zero checksum. Thus, this is not applicable to IPv6. I can add a sentence.

* Chairs : How many people read this document (rev-01) or (rev-02) ?
           Have read : 7, Have not Read : 24

* Chairs : Some people have read, and we have a set of reviewers.
  
* Chairs : Please now hum if you think the next revision is ready for a Working Group Last Call.
           Support and ready: Some, Would not support WGLC (or major items to address): few/none.

Please read the document and comment on list. The chairs think this is in a good position for a new revision, and we plan to start a WGLC before the next IETF meeting.
  
### 7.3 Michael Tuexen: RFC 6951bis: SCTP/UDP

draft-tuexen-tsvwg-rfc6951-bis-03

This is an individual draft, Michael plans to address any new comments, but has no pending issues.

No questions.

## 8. ECN & L4S Drafts
   
### 8.1 Jason Livingood: L4S Experimental Deployment

draft-livingood-low-latency-deployment (Individual submission to Independent Stream Editor).

The draft is mainly ready. This described an update on experimental deployment in virtual CMTS (vCMTS) such as ECN marking problems.
The author is looking for whether this should proceed as an ISE or as a WG draft, and is looking for feedback from TSVWG on which way to proceed. 

* Chairs : Please continue discussion on the mailing list. The Chairs plan to discuss the document status with our AD, please renmind us.

##   8.2 Greg White: L4S Operational Guidance on Coexistence with Classic ECN during L4S Deployment

draft-ietf-tsvwg-l4sops-05

Documents will be updated.

* Tim Chown : Measurement of the impact is ongoing in other groups (e.g. ippm). What are your thoughts on general guidance on measuring impact such as responsiveness such as latency?
* Greg : Experiments were done in the interops and have developed a straw-man test plan for lab testing. Some could be used in field testing. This is an open area for further L4S work as equipment using CE marking and CC, it is important there are good ways of CE-marking; and knowing the CC performs correctlty.
* Tim : Many games come with a responsiveness number.
* Greg : We also have interops, and the next will be at cablelabs in september and then at the IETF meeting in Prague.

* Chair : Please continue discussion on the mailing list, and please also consider joining the L4S inerop at the next IETF meeting.

## 8.3 Greg White : Non Queue Building (NQB) Per Hop Behavior

draft-ietf-tsvwg-nqb-19

A WGLC was made on draft -14, and there have been updates based on comments linked to 13 issues, and believes all of these are addressed in the new revision. There was a new queation relating the UP in a 802.11 context, which is expected to be closed soon. The plan is to submit as PS by Sept 2023.

* Gorry : Thanks for responding to my issues. If other people raised issued, please check the new text. I appreciate trying to align with RFC 8325 Mapping Diffserv to IEEE 802.11. When will the next revision be addressed?
* Greg : I hope this will be ready in 2 weeks.

* Chairs: Please hum if if you have read this (or a recent version of the draft).
    Read: Some.

* Chairs: Please hum if this document is not ready to proceed (i.e. ought not start a working group last call).
  Has concerns with WGLC : Few/none. (Note: remote participants could unfortunately not participate in the hums, and should ask to have their position noted if needed).
        * Mikhel Abrahamsson : Example configs to implement this in access network are needed, this would be of interest.
        * Sebastien Moeller (relayed from chat by remote participant) : This should not be published.

The purpose of a WGLC is to gather review and indicate support or not, and we therefore plan to start a  WGLC before the next IETF meeting and report on this at TSVWG for IETF-118.

## 9. Individual Drafts

### 9.1 Dan Wing : Encrypted Transport Protocol Path Explicit Signals
draft-reddy-tsvwg-explcit-signal-01

The draft protects and authenticates the signals. We do not discuss key negoatiation in this draft.

* Matt Joras : Secure information exchange is presented before, but it was not accepted. This was discussed at dispatch. I do not think it is appropriate to adopt this in TSVWG. The feedback from dispatch was to hold a BoF.
* Chairs : I can confirm that this topic has been on the Agenda in various forms, including RSVP extensioms, Diffserv and other signaling methods. This topic can lie here, and we do not expect any adoption call today.
* Dan : We would like to collaborate on a BoF proposal.

Tom Herbert: Already discussed in one to one wireless media header. I do not feel this is architecturally correct. There are potemtial practical issues with middleboxes. IPv6 extension headers has been seen before. Hop by hop processing draft is also not applicable. This signalling from host is a common problem and should find a common generic solution. Maybe this an INT Area subject.
Gorry : The QoS aspects of this fall in this group (scheduling, configuring queues and signalling). INT area defines encapsulation, and headers. We can debate this here.
Christian : I agree with the previous spekaer. This is not a UDP option function, which is end-to-end and supposed to be used by transport and not be intermediaries. A new mechanism should be a part of UDP options, and should not be used with encrypted transports.
Tianji Jiang : I like this idea. We have a real case, that relates to the 3GPP downstream, where the media header could be used. The receiver is a UE and often a destination, there needs to be a way that works withg encrypted traffic this is a potential solution.
* Mike Heard : I agree with earlier comments from Tom and Christian, which I largely agree with. I see a proposal to use auth and uenc - please keep in mind that these parts might not make this to the final draft. Does the network modify or strip these options in transit ?
* Dan : There is no modification of the tags in transit, they are end-to-end.
* Dan : I do not have time to answer all questions, but I would love to hear from more people.
* Kazuho Oku : This seems like a way to ask the netrwork for better treatment, and that this seems a slipperly sliope to go into. Packets should not be dropped in transit, we should use ECN, the sender should decide.
Tim Chown : Common requirements for marking such as hop by hop.
Marco Munizaga: Extension headers seem to be useful. We should focus on better support rather than on a new thing.
Dan: The waist of the Internet is UDP, NATs partly made it this way. This is why we brought this here. This does move things higher in the stack and this is uncomfortable, but we would dearly love to have a solution to this long-standing problem that we have been attempting to tackle for a few decades.
Chairs : What is your intention for next meeting?
Dan : We would like to particiapte in the BoF for refinement. We will also update the draft, starting with comments that we received here.

Chairs : Please discuss on the list and join to the next interim if possible. We look forward to seeing people in Prague - where we shall request two sessions.

End of Session 2
