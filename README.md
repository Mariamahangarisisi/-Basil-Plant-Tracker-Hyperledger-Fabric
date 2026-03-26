# -Basil-Plant-Tracker-Hyperledger-Fabric
🌿 Basil Plant Tracker | Hyperledger Fabric Track basil from greenhouse to supermarket with blockchain-powered precision. Java-based smart contracts, GPS logging, and multi-organization support ensure full traceability and auditability. 🚀




A complete blockchain-based system for tracking basil plants, designed for Pittaluga & fratelli greenhouse operations. This platform ensures full traceability from planting to supermarket delivery.

🔹 Project Overview

This project provides a comprehensive supply chain tracking solution for basil plants:

Smart Contract (Chaincode): Java-based contract for managing basil lifecycle
Client Application: Console-based Java app for plant operations
Multi-Organization Support: Org1MSP (greenhouse) & Org2MSP (supermarket)
Full Audit Trail: Records history with GPS coordinates
🏗️ Architecture

Data Model

Entity	Description
Basil	Main entity with QR code, extra info, and owner
Owner	Organization and user identification
BasilLeg	Timestamp and GPS location tracking

Smart Contract Functions

createTracking – Start basil plant tracking
stopTracking – Stop tracking
updateTracking – Update GPS location and timestamp
getActualTracking – Get current plant status
getHistory – Retrieve complete audit trail
transferTracking – Transfer ownership
queryAllBasil – List all tracked plants
📋 Prerequisites

System Requirements:

Java 11+
Docker & Docker Compose
Node.js 14+
Git & curl

Hyperledger Fabric Setup:

curl -sSLO https://raw.githubusercontent.com/hyperledger/fabric/main/scripts/install-fabric.sh && chmod +x install-fabric.sh
./install-fabric.sh docker samples binary
cd fabric-samples
🚀 Deployment Guide

1️⃣ Start Fabric Network

cd fabric-samples/test-network
./network.sh down
./network.sh up createChannel -ca
docker ps  # check running containers

2️⃣ Deploy Basil Chaincode

cd /path/to/basilChainCode
./gradlew clean installDist
cd fabric-samples/test-network
./network.sh deployCC -ccn basil -ccp /path/to/basilChainCode -ccl java

3️⃣ Update App Configuration

private static final Path PATH_TO_TEST_NETWORK = Paths.get("/path/to/fabric-samples/test-network");
private static final String CHAINCODE_NAME = "basil";
🖥️ Running the Client

Direct Execution:

cd /path/to/basilChainCode
./gradlew installDist
./build/install/basil/bin/basil

Via Gradle Run:

./gradlew run --console=plain
📱 Application Menu
BASIL PLANT TRACKING - MAIN MENU
1. Create new plant tracking
2. Stop plant tracking
3. Update plant state
4. Get plant current state
5. Get plant history
6. Transfer plant ownership
7. Query all plants
8. Switch organization (Org1/Org2)
0. Exit

Example Workflow:

Create plant tracking: QR = BASIL001, Extra info: Premium basil, Owner: Org1MSP
Update location: GPS = 44.4056,8.9463
Transfer to supermarket: New owner = Org2MSP
View complete history
🧪 Testing Scenarios
Basic Plant Lifecycle: Create → Update GPS → Transfer → View history
Multi-Org Access: Test permission limits between Org1MSP & Org2MSP
Full Audit Trail: Create multiple plants, perform operations, review histories
🛠️ Troubleshooting
Check network status: docker ps
Rebuild chaincode: ./gradlew clean installDist
View peer logs: docker logs peer0.org1.example.com -f
🔒 Security
Identity-based access control (MSP)
Ownership validation
Immutable transaction history
Multi-signature endorsement
🤝 Contributing
Fork the repo
Create a feature branch
Implement your changes
Test thoroughly
Submit a pull request
