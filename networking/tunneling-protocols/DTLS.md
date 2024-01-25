##### ***What is DTLS?***
The Datagram Transport Layer Security (DTLS) protocol is designed to secure real-time communication applications. It is a derivative of the TLS (Transport Layer Security) protocol but optimized for use with datagram protocols, like UDP (User Datagram Protocol). DTLS helps to prevent eavesdropping, tampering, and message forgery.

DTLS ensures the confidentiality and integrity of data between the client and server. DTLS has its own handshake protocol to allow the client and server to authenticate each other and to negotiate encryption algorithms. It also provides mechanisms for resuming sessions to minimize the overhead of repeated handshakes.

1. Client and server initiate communication and perform a handshake.
2. The handshake process involves client and server exchanging cryptographic parameters, including agreeing on a pre-shared key or exchanging public keys.
3. Once the handshake is complete, application data can be exchanged. The data packets are encrypted and optionally compressed.
4. Either party can initiate a closure of the session, or it can remain open for ongoing communication.

There are a few differences from TLS:
- Packet Re-Ordering: Since UDP doesn't guarantee the order of packets, DTLS has to manage the sequence control at the application layer.
- Packet Loss: DTLS has built-in mechanisms for dealing with packet loss, which is common in datagram-based networks.
- Stateful: DTLS is stateful, meaning both the client and server maintain information about the session.

One use case would be using DTLS with RTP for VoIP communication. Using DTLS with VoIP often involves securing the Real-time Transport Protocol (RTP) streams that carry the actual voice data. The combination is often referred to as DTLS-SRTP (Datagram Transport Layer Security - Secure Real-Time Transport Protocol).

These would be the steps for DTLS encryption over a VoIP system.
1. **SIP Negotiation**: The Session Initiation Protocol (SIP) is used to initiate a call between the client and the server. The client sends a SIP INVITE, indicating support for DTLS-SRTP.
2. **SDP Exchange**: Within the SIP INVITE, the Session Description Protocol (SDP) payload specifies that DTLS will be used for securing RTP streams.
3. **Server Response**: The server replies with a SIP 200 OK message, also indicating via SDP that it supports DTLS-SRTP.
4. **DTLS Handshake**:
- Client and server exchange DTLS handshake messages over the same port that RTP will use.
- Cryptographic parameters are exchanged, allowing both parties to validate certificates and agree on keys.

After the DTLS handshake is successful, both parties have the cryptographic keys needed for SRTP. At the end of the call, a SIP BYE message is sent, and the DTLS session is closed.