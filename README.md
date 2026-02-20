# CYBER-FORENSIC-DARK-WEB-TRACE-ENGINE
MONGODB
# CREATE DATABASE
use CyberTraceDB
# CREATE COLLECTIONS
db.createCollection("hackers")
db.createCollection("ipLogs")
db.createCollection("malwareDB")
db.createCollection("cryptoTransfers")
db.createCollection("threatReports")
# INSERTS HACKERS PROFILE
db.hackers.insertMany([
{
    hackerId: "HX001",
    alias: "ShadowByte",
    threatLevel: 9,
    skillSet: ["Reverse Engineering", "Zero-Day Exploit", "Crypto Laundering"],
    active: true,
    riskScore: 87.4,
    knownIPs: [
        { ip: "192.168.4.12", country: "Russia" },
        { ip: "10.44.21.8", country: "Germany" }
    ],
    cryptoWallets: [
        { type: "Bitcoin", wallet: "bc1x9shadow001", balance: 14.3 },
        { type: "Monero", wallet: "4xmrShadow001", balance: 230 }
    ]
},
{
    hackerId: "HX002",
    alias: "NullGhost",
    threatLevel: 7,
    skillSet: ["Phishing", "Botnet", "SQL Injection"],
    active: false,
    riskScore: 65.2,
    knownIPs: [
        { ip: "172.20.11.9", country: "Brazil" }
    ],
    cryptoWallets: [
        { type: "Ethereum", wallet: "0xGhost002", balance: 120 }
    ]
}
])
#
