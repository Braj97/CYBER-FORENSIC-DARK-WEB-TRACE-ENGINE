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
# INSERT MALWARE SIGNATURES
db.malwareDB.insertMany([
{
    malwareId: "MW001",
    name: "BlackHydra",
    type: "Ransomware",
    severity: 10,
    hashSignature: "89XJ23KL90AD12",
    relatedHackers: ["HX001"]
},
{
    malwareId: "MW002",
    name: "PhantomWorm",
    type: "Worm",
    severity: 8,
    hashSignature: "88GH21ZX88PL91",
    relatedHackers: ["HX002"]
}
])
# INSERT IP LOGS WITH GEOJSONS
db.ipLogs.insertMany([
{
    hackerId: "HX001",
    timestamp: new Date(),
    location: {
        type: "Point",
        coordinates: [37.6173, 55.7558]
    },
    attackType: "Ransomware Deployment",
    dataExfiltratedGB: 120
}
])
# CREATE INDEXES
db.hackers.createIndex({ alias: 1 }, { unique: true })
db.malwareDB.createIndex({ hashSignature: 1 })
db.ipLogs.createIndex({ location: "2dsphere" })
# THREAT ANALYSIS
db.hackers.aggregate([
{
    $lookup: {
        from: "malwareDB",
        localField: "hackerId",
        foreignField: "relatedHackers",
        as: "malwareLinked"
    }
},
{
    $project: {
        alias: 1,
        threatLevel: 1,
        totalMalwareLinked: { $size: "$malwareLinked" },
        combinedRisk: {
            $multiply: ["$threatLevel", "$riskScore"]
        }
    }
},
{
    $sort: { combinedRisk: -1 }
}
])
# NETWORK MAPPING
db.hackers.aggregate([
{
    $graphLookup: {
        from: "hackers",
        startWith: "$alias",
        connectFromField: "alias",
        connectToField: "alias",
        as: "networkChain",
        maxDepth: 2
    }
}
])
# BUCKET OPERATION
db.hackers.aggregate([
{
    $bucket: {
        groupBy: "$riskScore",
        boundaries: [0, 50, 70, 90, 100],
        default: "Extreme",
        output: {
            count: { $sum: 1 }
        }
    }
}
])
# EXAMPLE
session = db.getMongo().startSession()

session.startTransaction()

db.hackers.updateOne(
    { hackerId: "HX001" },
    { $inc: { riskScore: 5 } }
)

db.threatReports.insertOne({
    hackerId: "HX001",
    reportDate: new Date(),
    status: "Risk Increased",
    analyst: "Cyber AI Engine"
})

session.commitTransaction()
session.endSession()
# REGEX DETECTION QUERY
db.malwareDB.find({
    name: { $regex: "Hydra", $options: "i" }
})
# MULTI FACET AI REPORT
db.hackers.aggregate([
{
    $facet: {
        highThreat: [
            { $match: { threatLevel: { $gte: 8 } } }
        ],
        inactiveHackers: [
            { $match: { active: false } }
        ],
        averageRisk: [
            {
                $group: {
                    _id: null,
                    avgRisk: { $avg: "$riskScore" }
                }
            }
        ]
    }
}
])

#OUTPUT


