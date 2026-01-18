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
      }
    ],
    "rule_set": [
      {
        "type": "remote",
        "tag": "torrent-clients",
        "format": "source",
        "url": "https://raw.githubusercontent.com/FPPweb3/sb-rule-sets/main/torrent-clients.json"
      }
      {
        "type": "remote",
        "tag": "encrypted-dns",
        "format": "source",
        "url": "https://raw.githubusercontent.com/FPPweb3/sb-rule-sets/main/encrypted-dns.json"
      }
    ]
  }
}
```
