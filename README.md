## Actinium-seeder


Actinium-seeder is a crawler for the Actinium network, which exposes a list
of reliable nodes via a built-in DNS server.

### Features

* regularly revisits known nodes to check their availability
* bans nodes after enough failures, or bad behaviour
* accepts nodes down to v0.5.0 to request new IP addresses from,
  but only reports good post-v0.6.9 nodes.
* keeps statistics over (exponential) windows of 2 hours, 8 hours,
  1 day and 1 week, to base decisions on.
* very low memory (a few tens of megabytes) and cpu requirements.
* crawlers run in parallel (by default 96 threads simultaneously).

## Requirements


`sudo apt-get install build-essential libboost-all-dev libssl-dev`

## Usage


Assuming you want to run a dns seed on `dnsseed.actinium.org`, you will
need an authorative **NS record** in the domain record of `actinium.org`, pointing
to for example `ns.actinium.org`:

`dig -t NS dnsseed.actinium.org`

```shell
;; ANSWER SECTION
dnsseed.actinium.org.   86400    IN      NS     ns.actinium.org.
```

On the system `ns.actinium.org`, you can now run **dnsseed**:

`./dnsseed -h dnsseed.actinum.org -n ns.actinium.org`

If you want the DNS server to report SOA records, please provide an
e-mail address (with the @ part replaced by .) using -m.

## Compiling

Compiling will require boost and ssl.  On debian systems, these are provided
by `libboost-dev` and `libssl-dev` respectively.

`$ make`

This will produce the `dnsseed` binary.


## Running as non-root

Typically, you'll need root privileges to listen to port 53 (name service).

One solution is using an iptables rule (Linux only) to redirect it to
a non-privileged port:

$ iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-port 5353

If properly configured, this will allow you to run dnsseed in userspace, using
the -p 5353 option.
