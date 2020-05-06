# NMAP Scanning Techniques

## Internal Host Discovery

!!! tip
    A few small tips about the following nmap scanning string:

    * 255 min host group is recommended.
    * min rate 6000 is recommended to keep kernel pumping packets.

```bash
nmap -Pn -n -sS -vvv -p 21-23,25,53,111,137,139,445,80,443,8443,8080 \
    --min-hostgroup 255 \
    --min-rtt-timeout 0ms \
    --max-rtt-timeout 100ms \
    --max-retries 1 \
    --max-scan-delay 0 \
    --min-rate 6000 \
    --open \
    -oA CLIENT-# \
    -iL <IPLIST>
```

## Internal Full Scope Hit and Run String using Syn Half Scan

!!! tip
    A few small tips about the following nmap scanning string:

    * 255 min host group is recommended.
    * min rate 1000 should be fine for internal scanning with decent accuracy of results.
    * Full Port Scan / --open is used for further parsing.

```bash
nmap -Pn -n -sS -p- -sV -vvv --min-hostgroup 255 \
    --min-rtt-timeout 25ms \
    --max-rtt-timeout 100ms \
    --max-retries 1 \
    --max-scan-delay 0 \
    --min-rate 1000 \
    --open \
    -oA <customer-#> \
    -iL <IPLIST>
```