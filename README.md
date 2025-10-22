# 🔐 EclipseProof - Integrated Full Stack Application

> 🏆 **Built for MLH Midnight Hackathon 2025 - Proof of concept**
>
> Our full-fledge app - under development - https://github.com/Ei-Sandi/EclipseProof-Pro 

**Privacy-Preserving Income Verification Using Zero-Knowledge Proofs**

EclipseProof revolutionizes income verification by allowing individuals to prove they meet income requirements without revealing their exact salary, employer details, or other sensitive financial information.

## 🏗️ Architecture

This integrated application consists of:

- **Backend (`eclipseproof-cli/`)**: Express.js API server with Midnight Network contract integration
- **Frontend (`frontend-vite-react/`)**: React application with modern UI for proof generation and verification

## 🚀 Quick Start Guide

> ⚡ **Quick Note**: First-time setup takes 3-6 minutes due to zero-knowledge circuit compilation. Subsequent starts are much faster!

### Prerequisites
- Node.js 18+ 
- npm 10+
- Docker (for proof server)
- [Compact Tools](https://docs.midnight.network/relnotes/compact-tools) (Midnight developer tools)

### Installation and Setup

1. **Clone and Install Dependencies**

   ```bash
   git clone <repository-url>
   cd EclipseProof
   npm install
   npm run setup
   ```

2. **Start the Integrated Application**

   ```bash
   # Start both backend API and frontend
   npm run start-app
   ```

   **⏱️ Important:** The first startup takes **3-6 minutes** due to:
   - Contract compilation (16 zero-knowledge circuits)
   - Provider initialization
   - Blockchain connection setup
   
   **Wait for this message before testing:**
   ```
   [INFO] EclipseProof API server running on port 3001
   ```

   **OR for full testnet integration:**

   ```bash
   # Start proof server, backend API, and frontend
   npm run start-app-testnet
   ```

   **⚠️ If compilation fails with zkir errors, try:**
   ```bash
   npm run start-app:simple
   ```

3. **Access the Application**
   - Frontend: http://localhost:5173
   - Backend API: http://localhost:3001
   - API Health Check: http://localhost:3001/health

4. **Verify Everything is Working**
   ```bash
   # Test backend health
   curl http://localhost:3001/health
   
   # Test proof generation
   curl -X POST \
     -H "Content-Type: application/json" \
     -d '{
       "name": "Test User",
       "dateOfBirth": "1990-01-01", 
       "payslipJson": "{\"netPay\":3500,\"grossPay\":4200}",
       "amountToProve": 3000
     }' \
     http://localhost:3001/api/proof/generate-from-json
   ```

## � Troubleshooting

### Common Issues

**❌ "Failed to fetch" Error**
- **Cause**: Backend not fully started yet
- **Solution**: Wait for the log message `"EclipseProof API server running on port 3001"`
- **Time**: Initial startup takes 3-6 minutes for contract compilation

**❌ "zkir returned a non-zero exit status -2"**
- **Cause**: Contract compilation failed
- **Solution**: Use simple mode: `npm run start-app:simple`
- **Alternative**: Restart the compilation process

**❌ "Could not connect to server"**
- **Cause**: Backend process stopped or didn't start
- **Solution**: Check if port 3001 is available: `ss -tlnp | grep :3001`
- **Fix**: Restart with `npm run start-app`

**❌ Wallet Connection Issues**
- **Cause**: Midnight Lace wallet not properly connected
- **Solution**: Click "Connect Wallet" button in the UI
- **Requirements**: Chrome 119+ with Midnight Lace extension

### Performance Notes

- **First startup**: 3-6 minutes (contract compilation)
- **Subsequent starts**: 30-60 seconds (cached contracts)
- **Proof generation**: 1-3 seconds
- **Proof verification**: <1 second

### API Testing

Use these commands to test your backend directly:

```bash
# Health check
curl http://localhost:3001/health

# Contract status
curl http://localhost:3001/api/contract/status

# Generate proof
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Alice Smith",
    "dateOfBirth": "1985-03-22",
    "payslipJson": "{\"netPay\":4500,\"grossPay\":5200}",
    "amountToProve": 4000
  }' \
  http://localhost:3001/api/proof/generate-from-json

# Verify proof (replace with actual proofKey from generation)
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "proofKey": "PROOF_HASH_FROM_GENERATION",
    "verifierName": "Alice Smith",
    "verifierDateOfBirth": "1985-03-22",
    "requiredAmount": 3500
  }' \
  http://localhost:3001/api/proof/verify
```

## �🙏 Acknowledgments

- **MLH (Major League Hacking)**: For organizing the Midnight Hackathon
- **Midnight Network**: For providing privacy-focused blockchain infrastructure
- **Template Provider**: Built on the Midnight Network starter template
- **Zero-Knowledge Community**: For advancing privacy-preserving technologies

## 📞 Support & Contact

- **GitHub Issues**: [Report bugs or request features](https://github.com/Utpal-Kalita/EclipseProof/issues)
- **Email**: Contact via GitHub profile

---

## 🌟 The Problem We Solve

### Traditional Income Verification Issues:

- 📄 **Over-sharing**: Applicants must share complete payslips with sensitive details
- 🔍 **Privacy Invasion**: Verifiers see exact salaries, employer names, and personal data
- � **Security Risk**: Sensitive documents can be misused or data breached
- ⏱️ **Manual Process**: Time-consuming document review and verification

### EclipseProof Solution:

- 🔒 **Zero-Knowledge Proofs**: Prove income thresholds without revealing exact amounts
- 🛡️ **Privacy-First**: Only the minimum necessary information is shared
- ⚡ **Instant Verification**: Cryptographic proofs verified in seconds
- 🌐 **Blockchain Security**: Powered by Midnight Network for trust and transparency

## 🎯 How It Works

### Simple Analogy

Imagine you want to get into a club that only allows people over 21:

**❌ Old Way:** Show your entire driver's license (revealing name, address, exact birthday)  
**✅ EclipseProof Way:** Get a cryptographic "stamp" that only proves "Yes, I'm over 21"

### For Income Verification:

1. **Prover** uploads their payslip privately
2. **Prover** enters their name and date of birth for identity verification
3. System extracts income data (never shared) and creates identity hashes
4. Prover specifies minimum income to prove (e.g., "I earn at least £2,500")
5. System generates a **Proof Key** using zero-knowledge cryptography with identity verification
6. **Verifier** receives the Proof Key and enters applicant's details for matching
7. System verifies both income threshold AND identity match
8. Result: ✅ "Identity verified and meets requirement" or ❌ "Verification failed"

**The verifier learns ONLY whether the identity matches and requirement is met - nothing else!**

## � Features

### 🔐 For Provers (Income Holders)

- **Document Upload**: Secure payslip processing with OCR simulation
- **Identity Verification**: Enter name and date of birth for cryptographic hashing
- **Income Detection**: Automatic extraction of salary information
- **Flexible Thresholds**: Prove any minimum income level
- **Privacy Protection**: Your exact income and identity details stay completely private
- **Proof Generation**: Create shareable cryptographic proofs with identity verification

### ✅ For Verifiers (Landlords, Lenders, etc.)

- **Identity Matching**: Verify proofs against specific applicant details
- **Dual Verification**: Check both income requirements and identity authenticity
- **Instant Results**: Get yes/no answers with identity confirmation in seconds
- **Privacy Compliant**: No access to sensitive personal or financial data
- **Fraud Prevention**: Cryptographic proof prevents identity and income fraud
- **Audit Trail**: Blockchain-backed verification history

## 🛠️ Technology Stack

- **Frontend**: React + TypeScript + Vite
- **Blockchain**: Midnight Network (Privacy-focused blockchain)
- **Cryptography**: Zero-Knowledge Proofs via Compact language
- **UI/UX**: Tailwind CSS with dark theme
- **State Management**: React Hooks + RxJS
- **Testing**: Vitest + Docker containers

## 📦 Installation & Setup

### Prerequisites

- Node.js 20.16.0+
- npm or yarn
- Git
- Docker (for local development)
- [Compact Tools](https://docs.midnight.network/relnotes/compact-tools) (Midnight developer tools)

### Project Structure

```
EclipseProof/
├── eclipseproof-cli/             # Backend API server (Express.js)
│   ├── src/
│   │   ├── server.ts            # Main API server
│   │   ├── api.ts               # API endpoints & blockchain integration
│   │   ├── config.ts            # Provider configuration
│   │   ├── contract-types.ts    # Smart contract type definitions
│   │   └── test/                # API tests
│   ├── package.json
│   └── proof-server*.yml        # Docker compose configs
├── eclipseproof-contract/        # Smart contract (Compact language)
│   ├── src/
│   │   ├── eclipseproof.compact # Main contract code
│   │   ├── witnesses.ts         # Contract witnesses
│   │   └── managed/             # Compiled contract artifacts
│   └── package.json
├── frontend-vite-react/          # React web application
│   ├── src/
│   │   ├── components/          # Reusable UI components
│   │   ├── pages/               # Application pages (home, wallet-ui)
│   │   ├── services/            # IncomeVerificationService
│   │   ├── types/               # TypeScript interfaces
│   │   └── App.tsx              # Main application component
│   ├── public/                  # Static assets & contract artifacts
│   └── package.json
├── package.json                  # Root package.json (workspaces)
├── turbo.json                   # Turborepo configuration
└── README.md                    # This file
```

## 🔑 Midnight Lace Wallet Setup

- Install the **Midnight Lace (testnet)** extension in Chrome 119 or newer. Load the unpacked build if you are using the current alpha.
- Create a Midnight testnet profile and configure the node, indexer, and local proof-server URIs during onboarding.
- Ensure you are running versions that match: Lace ≥ 2.0.0 works best with `midnight.js` ≥ 1.0.0 (this starter already meets the requirement).
- Brave/Edge users may need to disable shields for localhost so the wallet can talk to the proof server.
- After the page loads, the dApp looks for the injected provider at `window.midnight` (and `window.cardano.midnight`). If it is not found, the UI prompts you to connect/install.
- When you press **Connect Wallet**, the app now calls `provider.enable()` before delegating to the Mesh SDK connect flow, so Lace can show the authorization prompt and cache the session.

## 🎮 Usage Guide

> 🔑 **Before you begin:** Click the **Connect Wallet** button in the EclipseProof header to sign in with your Midnight wallet. Generating or verifying proofs now requires an active wallet connection so actions are tied to your Midnight identity.

### 👤 As a Prover (Proving Your Income)

1. **Navigate to the Prover Tab** 🔐
2. **Upload Your Payslip**: Click "Choose a payslip file" and select your document
3. **Review Detected Income**: The system will show the extracted income amount
4. **Enter Identity Details**: Provide your full name and date of birth
5. **Set Proof Amount**: Enter the minimum income you want to prove (e.g., £2,500)
6. **Generate Proof**: Click "Generate Privacy Proof"
7. **Share Proof Key**: Copy the generated proof key and share it with your verifier

### 🏢 As a Verifier (Checking Income Proof)

1. **Navigate to the Verifier Tab** ✅
2. **Paste Proof Key**: Enter the proof key shared by the prover
3. **Enter Applicant Details**: Input the applicant's exact name and date of birth
4. **Set Income Requirement**: Enter your minimum income requirement
5. **Verify**: Click "Verify Income Proof"
6. **Review Result**: Get instant ✅ or ❌ with identity and income verification

## 🔒 Privacy & Security

### What Provers Keep Private:

### What Provers Keep Private:

- ❌ Exact salary amount
- ❌ Employer name and details
- ❌ Bank account information
- ❌ Personal identifiers on payslips
- ❌ Other income sources
- ❌ Raw personal details (only cryptographic hashes are used)

### What Verifiers Learn:

- ✅ Whether income meets their minimum requirement
- ✅ Whether identity details match the proof (via hash comparison)
- ✅ Timestamp of verification
- ✅ Currency used (e.g., GBP)
- ❌ Nothing else! No exact income, names, or dates

### Security Features:

- 🔐 **End-to-End Encryption**: All sensitive data encrypted
- 🧬 **Cryptographic Hashing**: Personal details converted to irreversible hashes
- 🌐 **Blockchain Security**: Proofs stored on decentralized network
- 🛡️ **Zero-Knowledge**: Cryptographically impossible to extract private data
- 🔍 **Audit Trail**: All verifications logged for transparency
- 🚫 **No Data Storage**: Original documents never permanently stored
- 🛡️ **Identity Verification**: Prevents proof sharing/fraud while protecting privacy

## 🎯 Use Cases

### 🏠 **Rental Applications**

- Landlords verify both income and identity without seeing sensitive details
- Prevents application fraud with identity matching
- Faster processing with instant verification
- Enhanced privacy protection for tenants

### 🏦 **Loan Applications**

- Banks verify income thresholds with identity confirmation
- Prevents identity fraud in financial applications
- Streamlined underwriting with cryptographic verification
- Enhanced data protection compliance (GDPR, CCPA)

### 💼 **Employment Verification**

- Employers verify candidate income claims with identity matching
- Background checks without salary or personal detail exposure
- Prevents resume fraud while protecting privacy
- Reduced administrative overhead with automated verification

### 🎓 **Scholarship Applications**

- Educational institutions verify income-based eligibility securely
- Identity matching prevents application fraud
- Simplified verification process for students
- Privacy-compliant financial assessment

## 🔮 Future Roadmap

### Phase 1: Core Platform ✅

- [x] Basic proof generation and verification
- [x] Web interface for provers and verifiers
- [x] Privacy-preserving architecture

### Phase 2: Enhanced Features 🚧

- [ ] Real OCR integration for document processing
- [ ] Multi-currency support
- [ ] Mobile application
- [ ] API for third-party integrations

### Phase 3: Enterprise Ready 📋

- [ ] Bulk verification tools
- [ ] Advanced analytics dashboard
- [ ] Compliance reporting
- [ ] Enterprise SSO integration

### Phase 4: Ecosystem Expansion 🌐

- [ ] Integration with major property platforms
- [ ] Banking API partnerships
- [ ] Government verification systems
- [ ] International market expansion

## 🔧 Development

### Network Configuration

#### Testnet Network

1. **Set Network ID** - Already configured in `frontend-vite-react/src/App.tsx`
2. **Contract Address** - Currently using Counter contract for demo
3. **Start Development**:
   ```bash
   npm run dev:frontend
   ```

#### Local Development

1. **Configure wallet address** in `counter-cli/src/scripts/prepare-standalone.test.ts`
2. **Start local instances**:
   ```bash
   npm run dev:undeployed-instances
   ```
3. **Deploy and configure contract address**

### Available Scripts

```bash
npm run build          # Build all packages
npm run test           # Run tests
npm run lint           # Lint code
npm run dev:frontend   # Start frontend dev server
npm run compact        # Compile smart contracts
```

## 🤝 Contributing

We welcome contributions from the community! Here's how you can help:

### Development Setup

```bash
# Fork the repository
# Clone your fork
git clone https://github.com/YOUR_USERNAME/EclipseProof.git

# Create a feature branch
git checkout -b feature/amazing-feature

# Make your changes
# Test your changes
npm test

# Commit with clear message
git commit -m "Add amazing feature"

# Push to your fork
git push origin feature/amazing-feature

# Create a Pull Request
```

### Areas for Contribution:

- 🐛 Bug fixes and improvements
- ✨ New features and enhancements
- 📚 Documentation improvements
- 🧪 Test coverage expansion
- 🎨 UI/UX enhancements
- 🔒 Security audits

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
