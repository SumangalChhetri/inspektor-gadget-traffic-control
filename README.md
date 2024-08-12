# Ingress Packet Filtering Gadget

Filter and drop ingress network packets based on destination IP and port number using eBPF.

## How to Use

- Clone the repository and change directory inside the project:

    ```bash
    $ git clone https://github.com/SumangalChhetri/ingress_packet_filtering_gadget.git
    $ cd ingress_packet_filtering_gadget/
    ```

- Build the image after setting the experimental tag:

    ```bash
    $ export IG_EXPERIMENTAL=true
    $ sudo -E ig image build -t ingress_pkt_filter .
    ```

- Run the image:

    ```bash
    $ sudo -E ig run ghcr.io/inspektor-gadget/gadget/ingress_pkt_filter:latest --public-keys=""
    ```

## Working

This program filters and drops ingress packets at the network interface based on the destination IP address and port number. If not specified, all packets will be dropped.

We also track the number of packets dropped per `<destination IP, port number>` pair.

## Approach

The eBPF program uses the specified destination IP address and port to filter incoming packets. The address is provided as separate byte values for IPv4 addresses.

## Future Work
- Create a suite of automated tests to verify the correctness and performance of the eBPF program and the overall gadget.
- Improve the input flags format for more user-friendly IP and port specification.
- Decouple IP address and port number in the comparison logic.
- Explore filtering based on container mount ID.
- Implement support for additional protocols and packet types beyond TCP, such as UDP or ICMP.

## Flags

#### `--a`
- **Description:** The first 8 bits of the filter IPv4 address in Integer Format (e.g., 127)
- **Default value:** `0`

#### `--b`
- **Description:** The second 8 bits of the filter IPv4 address in Integer Format (e.g., 0)
- **Default value:** `0`

#### `--c`
- **Description:** The third 8 bits of the filter IPv4 address in Integer Format (e.g., 0)
- **Default value:** `0`

#### `--d`
- **Description:** The last 8 bits of the filter IPv4 address in Integer Format (e.g., 1)
- **Default value:** `0`

#### `--p`
- **Description:** Port number (e.g., 443)
- **Default value:** `0`

#### `--iface`
- **Description:** Network interface to attach to
- **Default value:** `""`

### Example Command:

```bash
sudo -E ig run ghcr.io/inspektor-gadget/gadget/ingress_pkt_filter:latest --public-keys="" --a 192 --b 168 --c 1 --d 10 --p 80

This will filter and drop the ingress packets with destination address, port pair 192.168.1.10:80

Requirements
ig v0.26.0 (CHANGEME)
Linux v5.15 (CHANGEME)
"# ingress_packet_filtering_gadget" 
