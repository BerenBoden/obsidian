![image](https://beren-obsidian-images.imgix.net/48c0919112dbb56ed0a2f2fe9ebddf89.svg)

### *WLAN history, frequencies & standards*
There are three main frequencies available for public use 900MHz, 2.4GHz, 5GHz and the new 60GHz which is for the new 802.11ad standard that is a very high-speed, short-range wireless technology.
- 900MHz & 2.4GHz are referred to as Industrial, Scientific, and Medical (ISM) bands.
- 5 GHz band is known as the Unlicensed National Information Infrastructure (U-NII) band.
- The 60 GHz frequency band is often referred to as the "V-band" in the radio frequency spectrum.

The notation "802.11b/g/n" generally refers to the three different Wi-Fi standards: 802.11b, 802.11g, and 802.11n. These are often grouped together in this way because many Wi-Fi devices are compatible with multiple standards.
- 802.11b: Operates in the 2.4 GHz ISM band, with a maximum data rate of 11 Mbps.
- 802.11g: Also operates in the 2.4 GHz ISM band but offers faster speeds, up to 54 Mbps.
- 802.11n: Can operate in both the 2.4 GHz ISM and 5 GHz U-NII bands, with even faster speeds up to 600 Mbps under ideal conditions.

Here is a table of WLAN history.

| Year  | Speed       | Radio Frequency | Network Standard       | Status                   |
| --- | ------------ | ----------------- | ----------------------- | ------------------- |
| 1992  | 860 Kbps    | 900 MHz         | Proprietary            | IEEE 802.11 Drafting Begins |
| 1997  | 1 and 2 Mbps| 2.4 GHz         | 802.11                 | 802.11 Ratified         |
| 1999  | 11 Mbps     | 2.4 GHz, 5 GHz  | 802.11a,b              | 802.11a,b Ratified      |
| 2003  | 54 Mbps     | 2.4 GHz         | 802.11g                | 802.11g Ratified        |
| 2007  | (up to 600 Mbps) | 2.4 GHz, 5 GHz  | 802.11n             | 802.11n Draft 2.0       |
| 2013  | Up to 1.3 Gbps | 5 GHz         | 802.11ac         | 802.11ac Ratified       |

Here is a table of all the IEEE WLAN standards.

| Standard      | Description                                                       |
|---------------|-------------------------------------------------------------------|
| IEEE 802.11a  | 54 Mbps, 5 GHz standard                                            |
| IEEE 802.11ac | 1 Gbps, 5 GHz standard                                             |
| IEEE 802.11b  | Enhancements to 802.11 to support 5.5 Mbps and 11 Mbps            |
| IEEE 802.11c  | Bridge operation procedures; included in the IEEE 802.1D standard  |
| IEEE 802.11d  | International roaming extensions                                   |
| IEEE 802.11e  | Quality of service                                                 |
| IEEE 802.11F  | Inter-Access Point Protocol                                        |
| IEEE 802.11g  | 54 Mbps, 2.4 GHz standard (backward compatible with 802.11b)       |
| IEEE 802.11h  | Dynamic Frequency Selection (DFS) and Transmit Power Control (TPC) at 5 GHz |
| IEEE 802.11i  | Enhanced security                                                  |
| IEEE 802.11j  | Extensions for Japan and U.S. public safety                        |
| IEEE 802.11k  | Radio resource measurement enhancements                            |
| IEEE 802.11m  | Maintenance of the standard; odds and ends                         |
| IEEE 802.11n  | Higher throughput improvements using multiple-input, multiple-output (MIMO) antennas |
| IEEE 802.11p  | Wireless Access for the Vehicular Environment (WAVE)               |
| IEEE 802.11r  | Fast roaming                                                       |
| IEEE 802.11s  | ESS Extended Service Set Mesh Networking                           |
| IEEE 802.11T  | Wireless Performance Prediction (WPP)                              |
| IEEE 802.11u  | Internetworking with non-802 networks (cellular, for example)      |
| IEEE 802.11v  | Wireless network management                                        |
| IEEE 802.11w  | Protected management frames                                        |
| IEEE 802.11y  | 3650–3700 operation in the US                                      |
### *Most popular & common WLAN standards*
**2.4 GHz (802.11b)**
The 802.11b standard operates in the 2.4 GHz unlicensed radio band and was once the most widely used wireless standard. It offers a maximum data rate of 11 Mbps, which was considered adequate for most applications. 

One key feature is its ability to shift data rates without losing connection (Data-Shift-Rate). This rate shifting happens on a per-transmission basis and is automatic, allowing multiple clients to be supported at varying speeds depending on their locations.

To manage Ethernet contention in the RF spectrum, 802.11b uses a protocol known as Carrier Sense Multiple Access with Collision Avoidance (CSMA/CA). CSMA/CA also has an optional Request to Send, Clear to Send (RTS/CTS) mechanism. For each packet sent, an RTS/CTS and acknowledgment must be received.

The standard is now largely considered obsolete due to newer, faster standards like 802.11g offering better performance for a similar price.

**2.4 GHz (802.11g)**
The 802.11g standard is backward compatible with 802.11b and was ratified in June 2003. It operates in the same 2.4 GHz frequency range as 802.11b but offers a higher maximum data rate of 54 Mbps, similar to 802.11a.

Although 802.11g can coexist with 802.11b, having even one 802.11b user can degrade the performance for all users on the same access point. 802.11b uses Direct Sequence Spread Spectrum (DSSS), whereas 802.11g uses more robust Orthogonal Frequency Division Multiplexing (OFDM). In the United States 11 channels are configurable within the 2.4 GHz range, with channels 1, 6, and 11 being non-overlapping to avoid interference.

As I am located in New Zealand there are 13 channels available, I could opt for channels 1, 5, 9, and 13, given that I have the option of 13 channels. This would allow for better spacing and less interference among the channels, making it a suitable setup for environments where multiple access points are needed.

**5 GHz (802.11a)**
The IEEE ratified the 802.11a standard in 1999 but was not introduced into the market until late 2001. 802.11a offers a maximum data rate of 54 Mbps and provides 12 non-overlapping frequency channels, the important thing to note about 802.11a is that it operates on 5 GHz frequencies, and does not interfere with 2.4 GHz devices. 802.11a products have the ability to data-rate-shift from 54 Mbps down to 6 Mbps.

| Band         | Frequency Range | Environment      | Frequency (GHz) | Channel |
|--------------|-----------------|------------------|-----------------|---------|
| Lower Band   | 5.15–5.25       | Indoor           | 5.15            | 36      |
|              |                 |                  | 5.180           | 40      |
|              |                 |                  | 5.220           | 44      |
|              |                 |                  | 5.240           | 48      |
| Middle Band  | 5.25–5.35       | Indoor and Outdoor | 5.260          | 52      |
|              |                 |                  | 5.280           | 56      |
|              |                 |                  | 5.300           | 60      |
|              |                 |                  | 5.320           | 64      |
| Upper Band   | 5.725–5.825     | Outdoor         | 5.745            | 149     |
|              |                 |                  | 5.765           | 153     |
|              |                 |                  | 5.785           | 157     |
|              |                 |                  | 5.805-5.825     | 161     |

**5 GHz (802.11h)**
The 5 GHz (802.11h) specification introduced two pivotal features: Dynamic Frequency Selection (DFS) and Transmit Power Control (TPC). Due to the release of more 802.11a 5 GHz products by 2008, there was access to up to 23 non-overlapping channels in the 5 GHz range.

DFS monitors for radar signals in the 5 GHz band and 802.11a before transmitting. If radar signals are detected, DFS will either leave the channel or mark it unavailable to avoid interference.

TPC allows for adjusting the transmit power of client machines and access points to cover different ranges.

### *Resources*
https://www.lairdconnect.com/support/faqs/what-channels-are-supported-both-24-ghz-and-5ghz-band-most-countries