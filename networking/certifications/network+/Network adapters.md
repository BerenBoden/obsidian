
![image](https://beren-obsidian-images.imgix.net/f2cd6f785cde7061d4ffb45ca3589c90.png)
### *Facts about network adapters*
A network interface card (NIC), also called a network adapter, connects a host to the network medium.

This lesson covers the following topics:

- Network interface card features
- Network interface card components
#### Network Interface Card Features
The following are a few features of network interface cards.

- The network interface card is responsible for converting binary data into a format that can be sent on the network medium.
    - A transceiver is responsible for converting digital data into digital signals to be sent on the medium.
        - The type of signal the transceiver sends depends on the type of network. A fiber optic NIC sends light signals, a wired NIC sends electronic signals on a wire, and a wireless NIC sends radio signals.
        - To receive signals, the transceiver converts digital signals from the network to digital data for the PC.
    - A modem converts binary data to analog waves on the sending end (modulation) and then converts the analog waves back to binary data on the receiving end (demodulation).
- Some computers, such as laptops, come with built-in NICs. Other computers use NICs that plug in to the system's expansion slots or are external to the computer and connect through an existing computer port.
- Network interface cards are Layer 1 devices because they send and receive signals on the network medium. They are also Layer 2 devices because they must follow the rules for media access and because they read the physical address in a frame.
- The type of network interface card you choose must match the network architecture you are connecting to.
- Older network interface cards used an external transceiver to connect to the network media. However, all modern network interface cards use a built-in transceiver.

#### Network Interface Card Components
The following table describes various components used by a network interface card.

|   |   |
|---|---|
|Component|Description|
|Transceiver module|A transceiver module is used to change the media type of a port on a network device, such as a switch or a router. The following are the most common types of transceiver modules:<br><br>- A gigabit interface converter (GBIC) is a large transceiver that fits in a port slot and is used for gigabit media, including copper and fiber optic.<br>- A small form-factor pluggable (SFP) is similar to a GBIC, but is a smaller size. An SFP is sometimes called a mini-GBIC.<br>- SFP+ is a newer version of the SFP. SFP+ supports data rates as high as 10 Gbit/s, 8 Gbit/s Fiber Channel, 10 gigabit Ethernet, and the Optical Transport Network (OTU2 standard).<br>- A 10 gigabit small form-factor pluggable (XFP) transceiver is similar to an SFP in size, but is used for 10 gigabit networking.<br>- Quad (4-channel) SFP is a compact hot-pluggable transceiver that is also used for data communication applications.|
|Media converter|You use a media converter to connect network interface cards that are using different media types. For example, a media converter could be used to connect a server with a fiber optic Ethernet NIC to a copper Ethernet cable.<br><br>- Media converters work at the Physical layer (Layer 1). Media converters do not read or modify the MAC address in any way.<br>- Media converters convert one media type to another within the same architecture (such as Ethernet). A media converter cannot translate between two different architectures. This must be done using a bridge or a router. Converting from one architecture to another requires modifying the frame contents to modify the Data Link layer address.|
|Media access control (MAC) address|A MAC address is a unique identifier burned into the ROM of every Ethernet NIC.<br><br>- The MAC address is a 12-digit (48-bit) hexadecimal number (each number ranges from 0–9 or A–F).<br>- The address is often written as 00-B0-D0-06-BC-AC or 00B0.D006.BCAC. Dashes, periods, and colons can be used to divide the MAC address parts.<br>- The MAC address is globally unique by design. The first half of the MAC address, the first six digits, is assigned to each manufacturer. The manufacturer determines the rest of the address, assigning a unique value that identifies the host address. A manufacturer that uses all the addresses in the original assignment can apply for a new MAC address assignment.<br>- Devices use the MAC address to send frames to other devices on the same subnet.<br><br>Some network cards allow you to change the MAC address through jumpers, switches, or software, but there are few legitimate reasons for doing so.|
|Address Resolution Protocol  <br>(ARP)|Hosts use ARP to discover the MAC address of a device from its IP address. Before two devices can communicate, the MAC address of the receiving device must be known. If the MAC address isn't known, ARP does the following to find it:<br><br>1. The sending device sends out a broadcast frame.<br>    - The destination MAC address is all Fs (FFFF:FFFF:FFFF).<br>    - The sending MAC address is its own MAC address.<br>    - The destination IP address is the known IP address of the destination host.<br>    - The sending IP address is its own IP address.<br>2. All hosts on the subnet process the broadcast frame, looking at the destination IP address.<br>3. If the destination IP address matches its own address, the host responds with a frame that includes its own MAC address as the sending MAC address.<br>4. The original sender reads the MAC address from the frame and associates the IP address with the MAC address, saving it in its cache.<br><br>Once the sender knows the MAC address of the receiver, it sends data in frames addressed to the destination device. These frames include a cyclic redundancy check (CRC) that is used to detect frames that have been corrupted during transmission.<br><br>Hosts use the reverse address resolution protocol (RARP) to find the IP address of a host with a known MAC address.|
### *Twisted pair facts*
Twisted pair network cabling is widely used because it's supported by many networking standards, it's inexpensive, and it's relatively easy to work with.

This lesson covers the following topics:

- Twisted pair standards
- Twisted pair advantages
- Twisted pair disadvantages
- Twisted pair abbreviations
- Plenum and riser
- Solid vs. stranded
- UTP cable types
- Connector types

#### Twisted Pair Standards

Twisted pair cables support a wide variety of fast modern network standards. Key points about twisted pair standards are:

- Two copper conductors form a path for an electrical signal with each wire carrying an equal but opposite signal.
- The wires are twisted to reduce crosstalk (the absorbed signals from another pair).
- The conductors are 22- to 24-gauge in thickness and are covered in plastic insulation.
- Pairs are color coded, bundled together, and covered in a plastic jacket or sheath.
- Most cables contain four twisted pairs.
- Cables may contain 25 or 100 pairs when used in larger wiring applications.
- Each pair within a length of cable is given a different number of twists to further reduce the effects of crosstalk.

#### Twisted Pair Advantages

Advantages of twisted pair cabling include:

- Flexibility - You can install twisted pair cabling around tight corners and places where other types of network cable cannot go without being damaged.
- Cost - Twisted pair cabling is less expensive than other types of network cabling.
- Ease of use - Twisted pair cabling is easy to work with. It's much easier to install compared to other types of network cabling.
- Support for newer protocols - newer, faster network protocols and standards have been designed to run on twisted pair cabling.

#### Twisted Pair Disadvantages
Disadvantages of twisted pair cabling include that it is:

- Susceptible to interference - The sheath around twisted pair cable is relatively thin, making it susceptible to EMI.
- Susceptible to eavesdropping - With the right equipment, anyone can pick up signals emanating from the wire.

#### Twisted Pair Abbreviations
There are two common abbreviations for twisted pair cables.

- Unshielded twisted pair (UTP). UTP cables are easy to work with and less expensive than shielded cables.
- Shielded twisted pair (STP):
    - Shielding is electrically conductive foil or braided material that is wrapped around pairs of wires, around the overall cable, or both.
    - Shielding helps to minimize crosstalk.
    - The main purpose of shielding is to minimize the effects of EMI from external sources, such as fluorescent light ballasts.
    - The shielding can be used as a ground. However, most shielded cables have a special grounding wire called a drain wire.

#### Plenum and Riser
Specially manufactured twisted pair cables are used in plenum and riser spaces. Key points are:

- A plenum space is a part of a building that provides a pathway for the airflow needed by heating and air conditioning systems, such as above a dropped ceiling or below a raised floor.
- Plenum rated cables use insulation that is fire resistant and non-toxic when burned.
- You must use plenum rated cables in plenum spaces.
- Riser rated cables are designed for installations that run between floors.
- Riser requirements are not as strict as plenum requirements.
    - You can use plenum rated cables in riser spaces.
    - You should never use riser rated cables in plenum spaces.

#### Solid vs. Stranded
Twisted pair cables can be solid or stranded. Be aware that:

- Solid wires conduct electrical signals better, but are prone to break when they are repeatedly bent.
- Stranded cables are more flexible, but don't carry signals as well.
- You should use solid cables in permanent and semi-permanent installations.
- You should use stranded cables for patch cords and frequently moved cables.

#### UTP Cable Types
The following table describes the types of unshielded twisted pair cable,

|   |   |   |
|---|---|---|
|Type|Connector|Description|
|Phone cable|RJ11|A phone cable is used to connect a PC to a phone jack in a wall outlet to establish a dial-up internet connection. It is also used to connect a DSL modem to a telephone network. It has two pairs of twisted cable (a total of 4 wires).|
|Cat 5|RJ45|Cat 5 supports 100-Mb Ethernet (100BASE-TX) and ATM networking. Cat 5 specifications also support gigabit (1000 Mb) Ethernet.|
|Cat 5e|RJ45|Cat 5e is similar to Cat 5 but provides better EMI protection. It supports 100-Mb (100BASE-T) and gigabit (1000BASE-T) Ethernet.|
|Cat 6|RJ45|Cat 6 supports 10-Gbps Ethernet (10GBASE-T) and high-bandwidth broadband communications. In most cases, Cat 6 cables include a solid plastic core that keeps the twisted pairs separate and prevents the cable from being bent too tightly.|
|Cat 6a|RJ45|Cat 6a is designed to provide better protection against EMI and crosstalk than Cat 6 cabling. Cat 6a provides better performance than Cat 6, especially when used with 10-Gbps Ethernet (10GBASE-T).|
|Cat 7|GG45  <br>TERA|The Cat 7 standard was ratified years before the Cat 6a standard to support 10-Gbps Ethernet (10GBASE-T). It requires shielding on each twisted pair and the cable as a whole. It also specifies the GG45 or TERA connectors.|
|Cat 8|RJ45|This is the fastest Ethernet cable with data transfer speed of up to 40 Gb and bandwidth support of up to 2 GHz. Cat8 uses shielded foiled twisted pair construction that includes shielding around each pair of wires within the cable to reduce near-end crosstalk.  <br>  <br>It also uses braiding around the group of pairs to minimize electromagnetic interference in crowded network installations.|

You can substitute each type of UTP cable for any category below it, but never for a category above. For example:

- You can substitute Cat 6 for a standard requiring Cat 5e.
- You can't use Cat 5 in a situation where Cat 6 is required.
- The exception is Cat 7 cabling, and then only when Cat 7 is terminated with TERA connectors.

#### Connector Types
The following table describes the two types of connectors used with twisted pair cables.

|   |   |
|---|---|
|Connector|Description|
|RJ11<br><br>![Connector used with twisted pair cables RJ11](https://cdn.testout.com/_version_6025/netpro2021v6-en-us/en-us/resources/text/t_twisted_pair_np6/fb_cab90c.jpg)|The RJ11 connector:<br><br>- Has four connectors.<br>- Supports up to two pairs of wires.<br>- Uses a locking tab to keep the connector secure in an outlet.<br>- Is used primarily for telephone wiring.|
|RJ45<br><br>![Connector used with twisted pair cables RJ45](https://cdn.testout.com/_version_6025/netpro2021v6-en-us/en-us/resources/text/t_twisted_pair_np6/fb_cab91c.jpg)|The RJ45 connector:<br><br>- Has eight connectors.<br>- Supports up to four pairs of wires.<br>- Uses a locking tab to keep the connector secure in an outlet.<br>- Is used for Ethernet and some Token Ring connections.<br><br>The RJ48c connector type is almost identical to RJ45. It uses the same connector as an RJ45, but it is wired differently. It is used for specific WAN connections, such as a T1 line.|
|GG45<br><br>![Connector used with twisted pair cables GG45](https://cdn.testout.com/_version_6025/netpro2021v6-en-us/en-us/resources/text/t_twisted_pair_np6/gg45.jpg)|The GG45 connector:<br><br>- Has eight connectors.<br>- Supports four pairs of wires.<br>- Is backwards compatible with RJ45.<br>- Has four additional conductors in the corners of the connector that duplicate and replace the four inner pins on the RJ45.|
|TERA<br><br>![Connector used with twisted pair cables TERA](https://cdn.testout.com/_version_6025/netpro2021v6-en-us/en-us/resources/text/t_twisted_pair_np6/tera.jpg)|The TERA connector:<br><br>- Has eight connectors.<br>- Supports four pairs of wires.<br>- Is incompatible with RJ45 and GG45.<br>- Does not require special tools to install.|

When connecting a modem to a phone line, connect the modem to the wall jack using the line port on the modem. Then connect the phone to the phone port on the modem. This allows you to use the phone or the modem through a single connection.
### *Coaxial cable facts*
Coaxial cable is a relatively old technology that is usually implemented with a bus topology. It is not suitable for ring or star topologies because the ends of the cable must be terminated.

This lesson covers the following topics:

- Coaxial cable components
- Coaxial cable advantages and disadvantages
- Coaxial cable grades
- Coaxial cable connectors
#### Coaxial Cable Components
Coaxial cable has the following components:

- Two concentric metallic conductors.
    - The inner conductor is a solid wire made of copper or copper-coated tin.
    - The outer mesh conductor (shield) is made of aluminum or tin-coated copper.
- A PVC plastic insulator that surrounds the inner conductor and insulates the signal from the mesh conductor.
- A PVC plastic cable sheath (jacket) that surrounds and protects the wire.

## Coaxial Cable Advantages and Disadvantages
Coaxial cable has the following advantages and disadvantages.

|  |  |
| ---- | ---- |
| Advantages | Disadvantages |
| Advantages of coaxial cable include:<br><br>- Highly resistant to electromagnetic interference (EMI).<br>- Highly resistant to physical damage. | Disadvantages of coaxial cable include:<br><br>- More expensive than UTP.<br>- Inflexible construction (more difficult to install).<br>- Not supported by newer networking standards. |
|  |  |
#### Coaxial Cable Grades
Coaxial cable grades include:

|   |   |   |
|---|---|---|
|Grade|Uses|Resistance Rating|
|RG-58|10Base2 Ethernet networking (also called thinnet)|50 ohms|
|RG-6|Cable TV, satellite TV, and cable networking|75 ohms|

When using coaxial cables, it is important to use cables with the same resistance (impedance) rating.
#### Coaxial Cable Connector
Coaxial cable uses the F-type connector. The F-type connector is:

- Twisted onto the cable or installed using a compression tool.
- Used to create cable and satellite TV connections.
- Used to connect a cable modem to a broadband cable connection.

Twinaxial cable (twinax) is similar to a coaxial cable but it has two inner conductors instead of one. It was initially created and used by IBM but it's becoming more popular in short-range scenarios due to its high-speed differential signaling applications (1 Mbit/s).
To connect computers using fiber optic cables, you need two fiber strands. One strand transmits signals; the other strand receives signals. Long-haul runs sometimes need only one fiber. The send and receive signals are transmitted over the same fiber.

This lesson covers the following topics:

- Fiber optic components
- Multi-mode vs. single-mode
- Connector types
- Fiber optic cable facts
- Wavelength-division multiplexing (WDM)
- Media converters
### *Fibre optic facts*
To connect computers using fiber optic cables, you need two fiber strands. One strand transmits signals; the other strand receives signals. Long-haul runs sometimes need only one fiber. The send and receive signals are transmitted over the same fiber.

This lesson covers the following topics:

- Fiber optic components
- Multi-mode vs. single-mode
- Connector types
- Fiber optic cable facts
- Wavelength-division multiplexing (WDM)
- Media converters
#### Fiber Optic Components
Fiber optic cabling uses the following components:

- The _core_ carries the signal. It is made of glass or plastic.
- The _cladding_ maintains the signal in the center of the core as the cable bends.
- The _sheathing_ protects the cladding and the core.

Advantages and Disadvantages

Fiber optic cabling offers the following advantages and disadvantages:

|   |   |
|---|---|
|Advantages|Disadvantages|
|Advantages of fiber optic cable include:<br><br>- Immune to electromagnetic interference (EMI).<br>- Highly resistant to eavesdropping.<br>- Supports extremely high data transmission rates.<br>- Allows long cable distances without a repeater.|Disadvantages of fiber optic cable include:<br><br>- Is very expensive to install.<br>- Is difficult to work with.<br>- Requires special training to attach connectors to cables.|

#### Multi-Mode vs. Single-Mode
Multi-mode and single-mode fiber cables are not interchangeable. The following table describes multi-mode and single-mode fiber cables.

|   |   |
|---|---|
|Type|Description|
|Single-mode|Characteristics of single-mode cables include:<br><br>- Data transfers through the core using a single light path.<br>- The core diameter is approximately 8 - 10.5 microns.<br>- Cable lengths can extend a great distance.<br>- There is less modal dispersion, so bandwidths can be higher.<br>- Higher-cost electronics are required to send signals down a single path.<br>- It is optimized for 1310 nanometer (nm) and 1550 nm light sources.|
|Multi-mode|Characteristics of multi-mode cables include:<br><br>- Data transfers through the core using multiple light paths.<br>- The core diameter is approximately 50 to 100 microns.<br>- There is more modal dispersion due to the multiple paths.<br>- Cable lengths are limited in distance and are dependent on bandwidth.<br>- Higher light gathering capacity simplifies connections and allows lower-cost electronics.<br>- It is optimized for 850 nm and 1300 nm light sources.|

#### Connector Types
Fiber optic cabling uses the following connector types:

|   |   |
|---|---|
|Type|Description|
|ST connector  <br><br>![Fiber optic cabling ST connector type](https://cdn.testout.com/_version_6025/netpro2021v6-en-us/en-us/resources/text/t_fiber_cables_np6/fb_cab180c.jpg)|The ST connector:<br><br>- Is used with single-mode and multi-mode cabling.<br>- Has a keyed bayonet-type connector.<br>- Is also called a push-in and twist connector.<br>- Has a separate connector for each wire.<br>- Is nickel plated with a ceramic ferrule to ensure proper core alignment and prevent light ray deflection.<br>- Gets its mnemonics from set-and-twist or straight tip.|
|SC connector  <br><br>![Fiber optic cabling SC Connector type](https://cdn.testout.com/_version_6025/netpro2021v6-en-us/en-us/resources/text/t_fiber_cables_np6/fb_cab183c.jpg)|The SC connector:<br><br>- Is used with single-mode and multi-mode cabling.<br>- Has a push-on/pull-off connector that uses a locking tab to maintain connection.<br>- Has a separate connector for each wire.<br>- Uses a ceramic ferrule to ensure proper core alignment and prevent light ray deflection.<br>- Gets its mnemonics from set-and-click or square connector.|
|LC connector  <br><br>![Fiber optic cabling LC Connector type](https://cdn.testout.com/_version_6025/netpro2021v6-en-us/en-us/resources/text/t_fiber_cables_np6/fb_cab184c.jpg)|The LC connector:<br><br>- Is used with single-mode and multi-mode cabling.<br>- Is composed of a plastic connector with a locking tab that is similar to a RJ45 connector.<br>- Has a single connector with two ends to keep the two cables in place.<br>- Uses a ceramic ferrule to ensure proper core alignment and to prevent light ray deflection.<br>- Is half the size of other fiber optic connectors.<br>- Gets its mnemonics from lift-and-click or little connector.|
|MTRJ connector  <br><br>![Fiber optic cabling MTRJ Connector type](https://cdn.testout.com/_version_6025/netpro2021v6-en-us/en-us/resources/text/t_fiber_cables_np6/fb_cab185c.jpg)|The MTRJ connector:<br><br>- Is used with single-mode and multi-mode cabling.<br>- Is composed of a plastic connector with a locking tab.<br>- Uses metal guide pins to ensure that it is properly aligned.<br>- Has a single connector with one end that holds both cables.<br>- Uses a ceramic ferrule to ensure proper core alignment and prevent light ray deflection.<br>- Gets its mnemonics from mechanical transfer registered jack.|
|FC connector  <br><br>![Fiber optic cabling FC Connector type](https://cdn.testout.com/_version_6025/netpro2021v6-en-us/en-us/resources/text/t_fiber_cables_np6/fb_cab186c.jpg)|The FC connector:<br><br>- Is typically used with single-mode cabling.<br>- Has a separate connector for each wire.<br>- Uses a threaded connector.<br>- Is designed to stay securely connected in environments where it may experience physical shock or intense vibration.<br>- Gets its mnemonics from ferrule connector or fiber channel.|

Additional Tips

Adding connectors onto a fiber optic cable takes some practice. For long cables running between floors or overhead, you might hire an experienced contractor to install the cable and the necessary connectors. Remember to complete the following when installing fiber optic.

- Keep the area as clean as possible.
- Cut the cable with a clean 90-degree cut.
- Polish the end of the cable prior to adding the connector. Use special polishing film and tools for polishing the cable ends.
- Glue or crimp the connector onto the cable.
- Cover or cap any connectors that won't be hooked up immediately.
- If necessary, directly splice two cable ends together. This requires expensive and specialized equipment.

#### Fiber Optic Cable Facts
There are two light source technologies prevalent in fiber optic communications, diode laser and high-radiance light-emitting diode (LED). The light produced by these technologies is in the infrared region of the light spectrum.

- The most common wavelengths used in fiber optics are 850 nm, 1300 nm, 1310 nm and 1550 nm.
    - In glass, these longer wavelengths have lower attenuation or signal loss due to scattering.
    - Attenuation in glass due to light absorption for these wavelength is almost zero.
- Multi-mode fiber is designed to operate at 850 nm and 1300 nm.
- Single-mode fiber is optimized for 1310 nm and 1550 nm.

#### Wavelength-Division Multiplexing
WDM joins several light wavelengths (colors) onto a single strand of fiber.

- This enables light signals in both directions across a single fiber.
- Today's systems can easily multiplex 160 signals.
- WDM is mostly used by long-haul and high-speed providers.
- Most WDM systems are designed to be used with single-mode fiber.

#### Media Converters
When working with fiber optic cabling, you can use media converters to switch between different network media.

For example, you can convert:

- Single-mode fiber to copper Ethernet wiring.
- Multi-mode fiber to copper Ethernet wiring.
- Single-mode or multi-mode fiber to coaxial wiring.
- Single-mode fiber to multi-mode fiber.
### *Cable construction and wiring implementation facts*
Twisted pair cabling is one of the primary ways that computers connect to networks.

This lesson covers the following topics:

- RJ45 wiring conventions
- Straight-through and crossover cables
- Constructing cables with RJ45 connectors
- Gigabit Ethernet cabling
- Power over Ethernet (PoE) cabling

#### RJ45 Wiring Conventions

There are two Telecommunications Industry Association (TIA) standards for creating straight-through cables. It doesn't matter which standard you use, T568A or T568B. But once you choose a standard, you should use the same one for all cables to avoid confusion when troubleshooting.

|   |   |
|---|---|
|Standard|Pinout Order|
|![Standards for straight-through cables T568A](https://cdn.testout.com/_version_6025/netpro2021v6-en-us/en-us/resources/text/t_twisted_pair_cons_np6/t568a.png)  <br>T568A|Pin 1: GW = White with green stripe  <br>Pin 2: G = Green  <br>Pin 3: OW = White with orange stripe  <br>Pin 4: B = Blue  <br>Pin 5: BW = White with blue stripe  <br>Pin 6: O = Orange  <br>Pin 7: BrW = White with brown stripe  <br>Pin 8: Br = Brown|
|![Standards for straight-through cables T568B](https://cdn.testout.com/_version_6025/netpro2021v6-en-us/en-us/resources/text/t_twisted_pair_cons_np6/t568b.png)  <br>T568B|Pin 1: OW = White with orange stripe  <br>Pin 2: O = Orange  <br>Pin 3: GW = White with green stripe  <br>Pin 4: B = Blue  <br>Pin 5: BW = White with blue stripe  <br>Pin 6: G = Green  <br>Pin 7: BrW = White with brown stripe  <br>Pin 8: Br = Brown|

#### Straight-Through and Crossover Cables

Using the TIA wiring standards, twisted pair cables are constructed as either straight-though or crossover cables.

|   |   |
|---|---|
|Cable|Description|
|![Twisted pair straight-though cable](https://cdn.testout.com/_version_6025/netpro2021v6-en-us/en-us/resources/text/t_twisted_pair_cons_np6/st.png)  <br>Straight-Through|Most twisted pair cables are manufactured as straight-through cables  <br>  <br>Connect computers to a hub or switch with a straight-through cable.  <br>  <br>The pinout order on a computer's network interface card (NIC) is different than the pinout order on the hub or switch port.  <br>  <br>Transmit pins of the NIC map to the receive pins on hub or switch and vice versa.|
|![Twisted pair Crossover cable](https://cdn.testout.com/_version_6025/netpro2021v6-en-us/en-us/resources/text/t_twisted_pair_cons_np6/co.png)  <br>Crossover|If a network device such as a hub or switch does not have an uplink port, use a crossover cable when connecting it to another network device.  <br>  <br>You can also connect one computer directly to another using a crossover cable.  <br>  <br>A crossover cable maps the transmit pins on one end of the cable with the receive pins on the other end.  <br>  <br>You can easily create a crossover cable as follows:<br><br>- Use the T568A standard to attach an RJ45 connector to one end.<br>- Use the T568B standard to attach an RJ45 to the other end.|

#### Constructing Cables with RJ45 Connectors

When creating a twisted pair cable using RJ45 connectors:

- Use a crimping tool designed for RJ45 connectors.  
    ![Crimping tool designed for RJ45 connectors](https://cdn.testout.com/_version_6025/netpro2021v6-en-us/en-us/resources/text/t_twisted_pair_cons_np6/crimp.png)
- Determine whether the cable wires have solid cores or stranded cores.
    - Be sure to use the correct RJ45 connector type, solid or stranded.
    - Use solid core cables for longer runs inside walls or the ceiling.
    - Use stranded core wires for patch cables and for drop cables where frequent movement occurs and flexibility is needed.
- To reduce crosstalk, keep the pairs twisted as much as possible right up to the connector. Be aware that Cat 6- and Cat 6a-compliant cables may not perform at 10 Gbps if the pairs are not twisted right up to the connector.

#### Gigabit Ethernet Cabling

For 10-Mbps Ethernet over twisted pair (10BASE-T) and 100-Mbps Ethernet over twisted pair (100BASE-TX), you use only two of the twisted pairs of wires in a Cat 3, Cat 5, or Cat 5e cable. The following pins are used for a computer NIC:

- Pin 1: Transmit +  
    Pin 2: Transmit -  
    Pin 3: Receive +  
    Pin 6: Receive -
- Pins 4, 5, 7, and 8 are unused.

1000BASE-T or Gigabit Ethernet uses all four pairs of wires in Cat 5e or above cables.

- There are positive (+) and negative (-) pins for each pair of wires.
- Signals go in both directions over each pair of wires, so there are no dedicated transmit and receive pins.
- The T568A and T568B wiring standards are still used.

If Cat 7 cabling is used for 10 Gigabit Ethernet, the cables are terminated with GG45 or TERA connectors.

- GG45 connectors require a special set of tools that are different from the RJ45 crimping tool.
- TERA connectors can be installed without any special tools.

#### Power over Ethernet Cabling

PoE technologies allow network cables to carry electrical power. This is helpful for remote devices where no external power is available. Another notable use is digital telephone systems where handsets are powered through the Ethernet cable, eliminating the need for a separate power adapter.

- Power can be supplied through one of the unused pairs of wires in 10- and 100-Megabit Ethernet.
- Power can also be supplied using one of the data wires.
- Many network switches have the option to supply PoE.
- PoE injection devices can be added to the middle of the cable span.
Network administrators are often responsible for data and telephone wiring.

This lesson covers the following topics:

- Demarcation points
- Main distribution frames (MDFs), intermediate distribution frames (IDFs), and building industry cross-connect (BIX)
- Punch down blocks
- Patch or distribution panels
- Documenting MDF, IDF, and patch panels

#### Demarcation Points

When you contract with a local exchange carrier (LEC) for data, internet, or telephone services, they install a physical cable and a termination jack on your premises. The demarc (short for demarcation point) is the line that marks the boundary between the telco equipment or cable and your private network or telephone system. The demarc is also called the minimum point of entry (MPOE) or the end user point of termination (EU-POT). In businesses, the demarc is typically located on the bottom floor of a building, just inside a door, and identified by an orange plastic cover on the wiring component. In residential buildings, the demarc is often a small box on the outside of the house.

- The demarc may be:
    - A box on the wall with a simple RJ45 connection
    - A 50-pin RJ21 connector
    - A fiber optic connection
    - A port on a network interface device (NID

If needed, a demarc extension can be used to move the demarc to another location in a building. For example, if your organization uses only one floor of a building, you will want the demarc where it is not exposed to other organizations. You are responsible for installing the demarc extension, but the LEC might do it for an additional charge. Normally, the LEC is responsible for all equipment on one side of the demarc, and the customer is responsible for all equipment on the other side of the demarc.

While a NID can be a passive demarc that organizes the cable and connections, a more intelligent NID, known as smartjack, may be provided by the LEC. Smartjacks:

- Are maintained by the LEC.
- Are typically used for more complex services, such as a T1 line.
- Can provide signal conversions, buffer signals, and regenerate signals.
- May provide diagnostic capabilities for the LEC.
    - The loopback capability can be used to test signals by transmitting them back to the LEC.
    - Alarm indicators can report trouble to the LEC.
    - Indicator lights can show the configuration and status of the Smartjack.

Main Distribution Frames (MDFs), Intermediate Distribution Frames (IDFs), and Building Industry Cross-Connect (BIX)

Strictly speaking, a main distribution frame (MDF) is a frame or rack that is used to interconnect and manage telecommunication wiring in a building. It functions like an old-time telephone switchboard, where operators used connecting wires to route telephone calls. Today's MDF describes the room that houses the traditional MDF along with networking patch panels. Often, rack-mounted equipment is also housed in an MDF.

- A traditional MDF may exist in a dedicated room or a within rack space in a datacenter.
- An MDF is usually located on the bottom floor or basement of a building.
- All internet or WAN demarcation points are normally near or within the MDF.

A traditional intermediate distribution frame (IDF) is a smaller wiring distribution frame or rack within a building. Like an MDF, the room that houses the IDF along with networking patch panels and rack-mounted equipment is called an IDF.

- IDFs are typically located on each floor directly above the MDF, although additional IDFs can be added on each floor as necessary.
- IDFs located above the MDF are connected using a vertical cross connect (VCC) or wire bundles that run vertically between the MDF and an IDF.
- If a floor has more than one IDF, the IDFs are connected with a horizontal cross connect (HCC).

BIX is a cross-connect system made of different sizes of punch down blocks, cable distribution accessories (such as moulded rings and strips), and a punch down tool. The BIX cross-connect system is certified for Category-5e and is primarily composed of two parts, the mounts and the connectors.

#### Punch Down Blocks

Punch down blocks are the predecessors to patch panels. They are commonly used to support low-bandwidth Ethernet and telephony wiring.

|   |   |
|---|---|
|Block Type|Description|
|![A 66 block is a punch down block used to connect individual copper wires together](https://cdn.testout.com/_version_6025/netpro2021v6-en-us/en-us/resources/text/t_wire_distribution_np6/66.png)  <br>66 block|A _66 block_ is a punch down block used to connect individual copper wires together.<br><br>- The 66 block has 25 rows of four metal pins. Pushing a wire into a pin pierces the plastic sheath on the wire, making contact with the metal pin.<br>- There are two different 66 block configurations:<br>    - In the 25-pair block (also called a non-split block), all four pins are bonded (electrically connected). Use the 25-pair block to connect a single wire with up to three other wires.<br>    - With the 50-pair block (also called a split block), each set of two pins in a row are bonded. Use the 50-pair block to connect a single wire to one other wire.<br>- With a 50-pair block, use a bridge clip to connect the left two pins to the right two pins. Adding or removing the bridge clip is an easy way to connect wires within the row for easy testing purposes.<br><br>66 blocks are used primarily for telephone applications. When used for data applications:<br><br>- Be sure to purchase 66 blocks rated for Cat 5.<br>- When inserting wires in the block, place both wires in a pair through the same slot to preserve the twist as much as possible.|
|![A 110 block is a punch down block used to connect individual wires together](https://cdn.testout.com/_version_6025/netpro2021v6-en-us/en-us/resources/text/t_wire_distribution_np6/110.png)  <br>110 block|A _110 block_ is a punch down block used to connect individual wires together.<br><br>- The 110 block comes in various sizes for connecting pairs of wires (for example 50, 100, or 300 pair).<br>- The 110 block has rows of plastic slots. Each plastic slot connects two wires together.<br>    - Place the first wire into the plastic slot on the 110 block.<br>    - Insert a connecting block over the wire and slot. The connecting block has metal connectors that pierce the plastic cable sheath.<br>    - Place the second wire into the slot on the connecting block.<br>- C4 connectors connect four pairs of wires; C5 connectors connect five pairs of wires.<br>- When connecting data wires on a 110 block, you typically connect wires in the following order:<br>    - White wire with a blue stripe followed by the solid blue wire.<br>    - White wire with an orange stripe followed by the solid orange wire.<br>    - White wire with a green stripe followed by the solid green wire.<br>    - White wire with a brown stripe followed by the solid brown wire.<br><br>110 blocks are used primarily for telephone applications. They are preferable over 66 blocks in high-speed networks because the introduce less crosstalk. When used for data applications:<br><br>- Be sure to purchase 110 blocks that are certified for Cat 5, Cat 6 and Cat 6a.<br>- When inserting wires, preserve the twist as much as possible.|
|![A krone connector is a punch down block used for telecommunications. It's an European alternative to the 110 block.](https://cdn.testout.com/_version_6025/netpro2021v6-en-us/en-us/resources/text/t_wire_distribution_np6/krone.jpg)  <br>Krone LSA-PLUS|Krone is an insulation-displacement connector for telecommunications. It's a European alternative to the 110 block. The Krone system is also used in broadcast systems with audio interconnections. The krone wiring can be used with associated control systems as well. Multipair audio cables are specifically designed for the krone system.|

Use a punch down tool to insert wires into 66, 110, or krone blocks.

![Punch down tool to insert wires into blocks](https://cdn.testout.com/_version_6025/netpro2021v6-en-us/en-us/resources/text/t_wire_distribution_np6/pdtool.jpg)

- The punch down tool pushes the wire into the block and cuts off the excess wire.
- Be sure to position the blade on the side of the clip toward the end of the wire.
- The blade for a 66 block is straight, while the blade for a 110 block is notched.

#### Patch or Distribution Panels
In an MDF or IDF, punch down blocks are rarely used for network cabling. Instead, twisted pair cables are terminated at a patch panel.

- Typically, individual four-pair cables are used rather than 25-pair or 100-pair cables. This takes advantage of cable shielding and minimizes cross-talk.
    - In large applications, bundles of 25- and 100-pair cables can be used for VCCs and HCCs. However, they should be certified to support the desired network speed.
- Twisted pairs are connected at the rear of the panel with connections similar to punch down blocks. A special tool is usually required.  
      
    ![Patch or distribution panel](https://cdn.testout.com/_version_6025/netpro2021v6-en-us/en-us/resources/text/t_wire_distribution_np6/pprear.png)  
      
    
- At the front of the panel, patch cables are used between the patch panel and network devices.  
      
    ![Fornt of Patch panel](https://cdn.testout.com/_version_6025/netpro2021v6-en-us/en-us/resources/text/t_wire_distribution_np6/pp.png)  
    
- A patch panel for fiber optic cabling is called a fiber distribution panel.  
      
    ![Patch panel for fiber optic cabling](https://cdn.testout.com/_version_6025/netpro2021v6-en-us/en-us/resources/text/t_wire_distribution_np6/fdp.png)

#### Documenting MDF, IDF, and Patch Panels

Keeping an MDF or IDF organized is a major challenge. One key to doing so is proper documentation. Here are some guidelines:

- Develop a naming convention and use it to label cables, wall jacks, patch panel ports, network devices, and racks.
- Record names in tables and diagrams.
    - Include location, installation dates, cable lengths, and cable grades.
    - Consider using documentation software. Perform an internet search for cable plant documentation software or cable management software to view available options.
### *Trouble shooting tools facts*
Troubleshooting physical connectivity problems is much easier if you use the appropriate troubleshooting tool.

#### Troubleshooting Tools
The table below lists and describes some of the most common troubleshooting tools.

|   |   |
|---|---|
|Tool|Description|
|Loopback plug|A loopback plug, or loopback adapter, reflects a signal from the transmit port on a device to the receive port on the same device. Use the loopback plug to verify that a device can both send and receive signals.<br><br>- There are loopback plugs for both copper and fiber connections.<br>- A failure in the loopback test indicates a faulty network card.<br>- A successful loopback test means the problem is in the network cabling or another connectivity device.<br><br>You can purchase pre-made loopback plugs, or you can make an inexpensive one by cutting the end of a cable and manually connecting the transmit wires to the receive wires. To do this, connect the wire from pin 1 to the wire at pin 3, and the wire at pin 2 to the wire at pin 6.|
|Smartjack|A _smartjack_ is an intelligent loopback device installed at the demarcation point for a WAN service. Key points are:<br><br>- Technicians at the central office can send diagnostic commands to the smartjack to test connectivity and performance between the central office and the demarc.<br>- When you contact a WAN service provider for assistance, the provider might execute a test using the smartjack.<br>- A successful test indicates that the problem is within the customer premises equipment (CPE).|
|Known good spares|One valuable troubleshooting method is to keep a set of components that you know are in proper functioning order. If you suspect a problem in a component, swap it with the known good component. If the problem is not resolved, troubleshoot other components. Examples of using this strategy include the following:<br><br>- Change the drop cable that connects a computer to the network.<br>- Replace a NIC with a verified working NIC.<br>- Move a device from one switch port to another.|
|Cable tester|A cable tester (line tester) verifies that the cable can carry a signal from one end to the other and that all wires are in the correct positions.<br><br>- High-end cable testers can check for various miswire conditions such as wire mapping, reversals, split pairs, shorts, or open circuits.<br>- You can use a cable tester to quickly identify a crossover and a straight-through cable.<br>- Most testers have a single unit that tests both ends of the cable at once.<br>- Many testers come with a second unit that can be plugged into one end of a long cable run to test the entire cable.|
|Time-domain reflectometer  <br>(TDR)|A _time-domain reflectometer_ is a special device that sends electrical pulses on a wire to discover information about the cable. The TDR measures impedance discontinuities (the echo received on wire in response to a signal on the same wire). The results of this test can be used to identify several variables:<br><br>- Estimated wire length.<br>- Cable impedance.<br>- The location of splices and connectors on the wire.<br>- The location of shorts and open circuits.|
|Optical time-domain reflectometer  <br>(OTDR)|An _optical time-domain reflector_ performs the same function as a TDR, but is used for fiber optic cables. An OTDR sends light pulses into the fiber cable and measures the light that is scattered or reflected back to the device. The information is then used to identify specifics about the cable:<br><br>- The location of a break.<br>- Estimated cable length.<br>- Signal attenuation (loss) over the length of the cable.|
|Cable certifier|A cable _certifier_ is a multi-function tool that verifies that a cable or an installation meets the requirements for a specific architecture implementation. For example, you would use a certifier to verify that a specific drop cable meets the specifications for 1000BaseT networking.<br><br>- A certifier is very important for Cat 6 cable used with bandwidths at or above 1000 Mbps. Slight errors in connectors or wires can cause the network to function at 100 Mbps instead of the desired 1000 Mbps (10 Gbps).<br>- Certifiers can also validate the bandwidth capabilities of network interface cards and switches. Many can detect the duplex settings of network devices.<br>- Most certifiers include features of a toner probe, TDR, and cable tester.<br>- Certifiers are very expensive and are typically used by organizations that specialize in wiring installations.<br><br>The following image shows some outputs you might see when using a cable certifier:<br><br>![Cable Certifier Output Example](https://cdn.testout.com/_version_6025/netpro2021v6-en-us/en-us/resources/text/t_trouble_tools_np6/cable_cert.png)|
|Toner probe|A _toner probe_ is composed of two devices used together to trace the end of a wire from a known endpoint to the termination point in the wiring closet. To use a toner probe:<br><br>- Connect the tone generator to one end of the wire. It will send a signal on the wire.<br>- In the wiring closet, touch the probe to wires or place the probe close to wires. A sound at the probe indicates that the generated tone has been detected and the wire that you are touching is the termination point for the wire you are tracing.|
|Multimeter|A _multimeter_ is a device used to test various electrical properties. A multimeter can measure several parameters:<br><br>- AC and DC voltage<br>- Current (amps)<br>- Resistance (ohms)<br>- Capacitance<br>- Frequency|
|Voltage event recorder|A voltage event recorder tracks voltage conditions on a power line. Basic recorders track only undervoltage or overvoltage conditions. More advanced devices track conditions over time and create a graph, saving data from a program running on a computer.<br><br>Some UPS systems include a simple voltage event recorder. Use a voltage event recorder to identify periods of low or high voltage that can adversely affect network components.|
|Environmental monitor|An environmental monitor does what its name implies, it monitors the environmental conditions of a specific area or device.<br><br>- Monitors are often used to track the conditions within server rooms, such as temperature, humidity, water, smoke, motion, and air flow.<br>- Typically, computers (especially servers) have an internal monitor that measures fan speed and CPU temperature.<br>- Many monitors sound an alarm if a specified temperature or other environmental condition is reached.|
|Wire stripper, snips, and crimpers|Wire strippers remove the protective sheath of a cable in order to expose the conductive wire.<br><br>- Wire strippers are rated to specific gauge (cable width) ranges.<br>- Most wire strippers are combination tools. They can strip, cut, and crimp cables.<br>- Almost all wire strippers have multiple holes or can be adjusted for specific cable sizes.<br><br>A crimping tool is used to attach connectors to wires. Some crimpers are designed for power connections. A special crimper is used to attach RJ45 connectors to twisted pair cables.<br><br>Snips are cutting tools used to cut cables or wires to a specific length or to remove damaged sections. A diagonal cutter is an example of a snip tool.<br><br>Whenever possible, use a wire stripper instead of snips to strip a cable. Wire strippers are specifically designed to cut only the protective sheath without cutting the internal wire.|
|Speed test website|A speed test website is an online tool that is used to test the bandwidth of the internet connection. There are countless speed test websites available, all of them provide essentially the same information:<br><br>- Connection latency (ping)<br>- Download speed<br>- Upload speed|
|Fusion splicer|A fusion splicer is a tool that uses heat to join two optical fibers together. It fuses them together end-to-end. This is done in situations when the cable is broken or too short for the purpose it's being used for.  <br>  <br>The fusion splicer ensures that the two cables are fused together in an accurate and precise way to eliminate (as much as possible) any light being scattered or reflected back by the splice. The source of heat used by the fusion splicer is usually an electric arc, but could also be a laser, a gas flame, or a tungsten filament through which current is passed.|
|Tap splicer|A tap splicer is a vinyl plastic wire termination device with a sharp metal insert that cuts through the plastic insulating jackets of two wires and crimps them together. You use it to make quick splices or connections using two or more pieces of wire within the recommended range of the tap splice.|