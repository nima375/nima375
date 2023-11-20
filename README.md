{
  "dns": {
    "independent_cache": true,
    "rules": [
      {
        "domain": [
          "qweasd.ferekans365.cfd",
          "dns.google"
        ],
        "server": "dns-direct"
      },
      {
        "geosite": [
          "ir"
        ],
        "server": "dns-direct"
      }
    ],
    "servers": [
      {
        "address": "https://dns.google/dns-query",
        "address_resolver": "dns-direct",
        "strategy": "prefer_ipv6",
        "tag": "dns-remote"
      },
      {
        "address": "local",
        "address_resolver": "dns-local",
        "detour": "direct",
        "strategy": "prefer_ipv6",
        "tag": "dns-direct"
      },
      {
        "address": "local",
        "detour": "direct",
        "tag": "dns-local"
      },
      {
        "address": "rcode://success",
        "tag": "dns-block"
      }
    ]
  },
  "inbounds": [
    {
      "listen": "127.0.0.1",
      "listen_port": 6450,
      "override_address": "8.8.8.8",
      "override_port": 53,
      "tag": "dns-in",
      "type": "direct"
    },
    {
      "domain_strategy": "",
      "endpoint_independent_nat": true,
      "inet4_address": [
        "172.19.0.1/28"
      ],
      "inet6_address": [
        "fdfe:dcba:9876::1/126"
      ],
      "mtu": 9000,
      "sniff": true,
      "sniff_override_destination": false,
      "stack": "mixed",
      "tag": "tun-in",
      "type": "tun"
    },
    {
      "domain_strategy": "",
      "listen": "127.0.0.1",
      "listen_port": 2080,
      "sniff": true,
      "sniff_override_destination": false,
      "tag": "mixed-in",
      "type": "mixed"
    }
  ],
  "log": {
    "level": "panic"
  },
  "outbounds": [
    {
      "flow": "xtls-rprx-vision",
      "packet_encoding": "",
      "server": "qweasd.ferekans365.cfd",
      "server_port": 443,
      "tls": {
        "enabled": true,
        "insecure": false,
        "reality": {
          "enabled": true,
          "public_key": "b6HLwZctCXbgNN5r2yuCH7vrBSrNGTRYL9PttGpqHXQ",
          "short_id": "932346af"
        },
        "server_name": "sourceforge.net",
        "utls": {
          "enabled": true,
          "fingerprint": "firefox"
        }
      },
      "uuid": "eae0a510-5606-46f4-bf1d-00cc25256e71",
      "type": "vless",
      "domain_strategy": "",
      "tag": "proxy"
    },
    {
      "tag": "direct",
      "type": "direct"
    },
    {
      "tag": "bypass",
      "type": "direct"
    },
    {
      "tag": "block",
      "type": "block"
    },
    {
      "tag": "dns-out",
      "type": "dns"
    }
  ],
  "route": {
    "auto_detect_interface": true,
    "rules": [
      {
        "outbound": "dns-out",
        "port": [
          53
        ]
      },
      {
        "inbound": [
          "dns-in"
        ],
        "outbound": "dns-out"
      },
      {
        "geosite": [
          "ir"
        ],
        "outbound": "bypass"
      },
      {
        "geoip": [
          "ir"
        ],
        "outbound": "bypass"
      },
      {
        "ip_cidr": [
          "224.0.0.0/3",
          "ff00::/8"
        ],
        "outbound": "block",
        "source_ip_cidr": [
          "224.0.0.0/3",
          "ff00::/8"
        ]
      }
    ]
  }
}
