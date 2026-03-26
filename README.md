# Basil Plant Tracking System - Hyperledger Fabric Implementation

A comprehensive blockchain-based basil plant tracking system built on Hyperledger Fabric for **Pittaluga & fratelli** greenhouse operations. This system enables end-to-end traceability of basil plants from greenhouse cultivation to supermarket delivery.

## 🌿 **Project Overview**

This project implements a complete supply chain tracking solution for basil plants using:
- **Smart Contract (Chaincode)**: Java-based contract for basil lifecycle management
- **Client Application**: Console-based Java application for plant operations
- **Multi-Organization Support**: Org1MSP (Pittaluga & fratelli) and Org2MSP (Supermarket)
- **Complete Audit Trail**: Full history tracking with GPS positioning

## 🏗️ **Architecture**

### Data Model
- **Basil**: Main entity with QR code, extra info, and owner
- **Owner**: Organization and user identification  
- **BasilLeg**: Timestamp and GPS position tracking

### Smart Contract Functions
- `createTracking` - Initialize basil plant tracking
- `stopTracking` - Terminate plant tracking
- `updateTracking` - Update GPS position and timestamp
- `getActualTracking` - Retrieve current plant state
- `getHistory` - Get complete audit trail
- `transferTracking` - Transfer ownership between organizations
- `queryAllBasil` - List all tracked plants

## 📋 **Prerequisites**

### System Requirements
- **Java 11+** (OpenJDK or Oracle JDK)
- **Docker & Docker Compose**
- **Node.js 14+** (for Fabric binaries)
- **Git**
- **curl**

### Hyperledger Fabric Setup
1. **Download Fabric Samples and Binaries**:
   ```bash
   curl -sSLO https://raw.githubusercontent.com/hyperledger/fabric/main/scripts/install-fabric.sh && chmod +x install-fabric.sh
   ./install-fabric.sh docker samples binary
   ```

2. **Navigate to Fabric Samples**:
   ```bash
   cd fabric-samples
   ```

## 🚀 **Deployment Guide**

### Step 1: Start the Fabric Network

```bash
# Navigate to test-network directory
cd fabric-samples/test-network

# Clean any existing network
./network.sh down

# Start the network with certificate authorities
./network.sh up createChannel -ca

# Verify network is running
docker ps
```

Expected containers:
- `peer0.org1.example.com`
- `peer0.org2.example.com`
- `orderer.example.com`
- `ca.org1.example.com`
- `ca.org2.example.com`

### Step 2: Deploy the Basil Chaincode

```bash
# Navigate to your project directory
cd /path/to/Pittaluga-fratelli/basilChainCode

# Build the chaincode
./gradlew clean
./gradlew installDist

# Deploy to the network
cd fabric-samples/test-network
./network.sh deployCC -ccn basil -ccp /path/to/Pittaluga-fratelli/basilChainCode -ccl java
```

**Successful deployment shows**:
```
Chaincode definition committed on channel 'mychannel'
Version: 1.0, Sequence: 1, Endorsement Plugin: escc, Validation Plugin: vscc
Approvals: [Org1MSP: true, Org2MSP: true]
```

### Step 3: Update Network Configuration

Ensure your `App.java` points to the correct network path:

```java
private static final Path PATH_TO_TEST_NETWORK = Paths.get("/path/to/fabric-samples/test-network");
private static final String CHAINCODE_NAME = "basil";
```

## 🖥️ **Running the Client Application**

### Method 1: Direct Execution (Recommended)
```bash
cd /path/to/basilChainCode
./gradlew installDist
./build/install/basil/bin/basil
```

### Method 2: Gradle Run
```bash
./gradlew run --console=plain
```

## 📱 **Usage Guide**

### Application Menu Options

```
==================================================
BASIL PLANT TRACKING - MAIN MENU
==================================================
1.  Create new plant tracking
2.  Stop plant tracking
3.  Update plant state
4.  Get plant current state
5.  Get plant history
6.  Transfer plant ownership
7.  Query all plants
8.  Switch organization (Org1/Org2)
9.  Show connection info
0.  Exit
==================================================
```

### Complete Workflow Example

#### 1. Create Plant Tracking
```
Choice: 1
QR code: BASIL001
Extra info: Premium basil from greenhouse A
Owner organization: Org1MSP
Owner user: User1@org1.example.com
```

#### 2. Update Plant Location
```
Choice: 3
QR code: BASIL001
Timestamp: 1672531200000
GPS position: 44.4056,8.9463
Requester org: Org1MSP
Requester user: User1@org1.example.com
```

#### 3. Transfer to Supermarket
```
Choice: 6
QR code: BASIL001
New owner org: Org2MSP
New owner user: User1@org2.example.com
```

#### 4. View Complete History
```
Choice: 5
QR code: BASIL001
```

## 🔄 **Organization Switching**

### Switch to Org2MSP (Supermarket)
```
Choice: 8
Select organization: 2
```

The application will reconnect using Org2MSP certificates, allowing supermarket operations.

## 🧪 **Testing Scenarios**

### Test 1: Basic Plant Lifecycle
1. Create plant as Org1MSP
2. Update GPS position
3. Transfer to Org2MSP
4. View history

### Test 2: Multi-Organization Access
1. Create plant as Org1MSP
2. Switch to Org2MSP
3. Try to update plant (should fail - unauthorized)
4. Switch back to Org1MSP
5. Transfer ownership to Org2MSP
6. Switch to Org2MSP
7. Update plant (should succeed)

### Test 3: Complete Audit Trail
1. Create multiple plants
2. Perform various operations
3. Query all plants
4. Review individual histories

## 🛠️ **Troubleshooting**

### Common Issues

#### 1. Network Connection Errors
```bash
# Check if network is running
docker ps

# Restart network if needed
./network.sh down
./network.sh up createChannel -ca
```

#### 2. Chaincode Deployment Failures
```bash
# Check if build directory exists
ls -la build/install/

# Rebuild if needed
./gradlew clean installDist

# Redeploy with new version
./network.sh deployCC -ccn basil -ccp /path/to/basilChainCode -ccl java -ccv 2.0 -ccs 2
```
### Debug Commands

```bash
# Check chaincode status
peer lifecycle chaincode querycommitted --channelID mychannel --name basil

# View peer logs
docker logs peer0.org1.example.com -f

# Check network connectivity
docker network ls
```


- **Deterministic Execution**: Uses transaction timestamps for consistency
- **Efficient Queries**: Optimized state database operations
- **Minimal Data Storage**: Compact JSON serialization

## 🤝 **Contributing**

1. Fork the repository
2. Create feature branch
3. Implement changes
4. Test thoroughly
5. Submit pull request

## 📄 **License**

This project is licensed under the Apache License 2.0 - see the LICENSE file for details.

## 🆘 **Support**

For issues and questions:
- Check troubleshooting section
- Review Hyperledger Fabric documentation
- Open GitHub issue with detailed error logs

---
