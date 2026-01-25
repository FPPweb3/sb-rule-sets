# sb-rule-sets

```json
{
  "route": {
    "rules": [
      {
        "rule_set": "torrent-clients",
        "outbound": "direct"
      },
      {
        "rule_set": "encrypted-dns",
        "action": "reject"
      },
      {
        "rule_set": "itdog-combined",
        "outbound": "proxy"
      }
    ],
    "rule_set": [
      {
        "type": "remote",
        "tag": "torrent-clients",
        "format": "source",
        "url": "https://raw.githubusercontent.com/FPPweb3/sb-rule-sets/main/torrent-clients.json"
      },
      {
        "type": "remote",
        "tag": "encrypted-dns",
        "format": "source",
        "url": "https://raw.githubusercontent.com/FPPweb3/sb-rule-sets/main/encrypted-dns.json"
      },
      {
        "type": "remote",
        "tag": "itdog-combined",
        "format": "binary",
        "url": "https://raw.githubusercontent.com/FPPweb3/sb-rule-sets/main/itdog-combined.srs"
      }
    ]
  }
}
```
# Usage example
```json
{
  "log": {
    "level": "fatal",
    "timestamp": true
  },
  "dns": {
    "servers": [
      {
        "tag": "dns-local",
        "type": "udp",
        "server": "127.0.0.1"
      },
      {
        "tag": "dns-vpn",
        "type": "udp",
        "server": "10.10.50.1"
      }
    ],
    "rules": [
      {
        "query_type": [ "SVCB", "HTTPS" ],
        "action": "reject"
      },
      {
        "rule_set": "itdog-combined",
        "server": "dns-vpn"
      },
      {
        "disable_cache": true,
        "server": "dns-local"
      }
    ]
  },
  "inbounds": [
    {
      "type": "tproxy",
      "tag": "tproxy-in",
      "listen": "::",
      "listen_port": 25000
    },
    {
      "type": "direct",
      "tag": "dns-in",
      "listen": "::",
      "listen_port": 5353
    }
  ],
  "outbounds": [
    {
      "type": "direct",
      "tag": "direct-out",
      "domain_resolver": {
        "server": "dns-local",
        "disable_cache": true
      }
    },
    {
      "type": "direct",
      "tag": "vpn-out",
      "bind_interface": "vpn-vpn",
      "domain_resolver": {
        "server": "dns-vpn"
      }
    }
  ],
  "route": {
    "rules": [
      {
        "type": "logical",
        "mode": "or",
        "rules": [
          {
            "inbound": "dns-in",
          },
          {
            "port": 53
          }
        ],
        "action": "hijack-dns"
      },
      {
        "ip_is_private": true,
        "outbound": "direct-out"
      },
      {
        "network": "udp",
        "port": 443,
        "action": "reject"
      },
      {
        "action": "sniff",
        "sniffer": [ "http", "tls" ]
      },
      {
        "rule_set": "encrypted-dns",
        "action": "reject"
      },
      {
        "rule_set": "itdog-combined",
        "outbound": "vpn-out"
      }
    ],
    "rule_set": [
      {
        "type": "remote",
        "tag": "encrypted-dns",
        "format": "binary",
        "url": "https://raw.githubusercontent.com/FPPweb3/sb-rule-sets/main/encrypted-dns.srs",
        "download_detour": "vpn-out"
      },
      {
        "type": "remote",
        "tag": "itdog-combined",
        "format": "binary",
        "url": "https://raw.githubusercontent.com/FPPweb3/sb-rule-sets/main/itdog-combined.srs",
        "download_detour": "vpn-out"
      }
    ]
  }
}
```
