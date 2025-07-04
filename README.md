# CMMC Level 1 & 2 Security Policies for Fleet

[![CMMC 2.0](https://img.shields.io/badge/CMMC-Level%201%20%26%202-blue)](https://www.acq.osd.mil/cmmc/)
[![Fleet Compatible](https://img.shields.io/badge/Fleet-4.15%2B-green)](https://fleetdm.com/)
[![osquery Compatible](https://img.shields.io/badge/osquery-5.7.0%2B-orange)](https://osquery.io/)
[![macOS Compatible](https://img.shields.io/badge/macOS-12.0%2B-lightgrey)](https://www.apple.com/macos/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Complete, production-ready implementation of **US CMMC 2.0 Level 1 & Level 2** security policies for macOS systems using Fleet. Based on the official [usnistgov/macos_security](https://github.com/usnistgov/macos_security) project with all **167 security controls** from the macOS security baseline.

## 🚀 Quick Start

### Level 1 (Essential Foundation - Start Here)
1. **Download** the `cmmc-policies-level-1.yml` file from this repository
2. **Upload** to Fleet using fleetctl
3. **Deploy** to your macOS hosts via teams/targeting  
4. **Monitor** CMMC Level 1 compliance in Fleet dashboard

### Level 2 (Complete Implementation - After Level 1)
1. **Download** the `cmmc-policies-level-2.yml` file
2. **Replace** Level 1 policies or deploy as comprehensive policy set
3. **Monitor** full CMMC Level 2 compliance status

## 📋 What's Included

### 🛡️ **CMMC Level 1 (76 Essential Policies)**
- **Authentication Controls**: Smartcard authentication, SSH hardening
- **Data Protection**: iCloud restrictions, sync controls
- **System Hardening**: Service disabling, file permissions
- **Access Control**: User restrictions, system preferences
- **System Settings**: Security configurations, privacy controls

### 🔒 **CMMC Level 2 (167 Complete Policies)**
- **Everything from Level 1** plus:
- **Auditing Controls**: Comprehensive logging and monitoring
- **Advanced Authentication**: Password policies, certificate controls
- **Extended iCloud Controls**: Full cloud service restrictions
- **Complete macOS Controls**: Full usnistgov/macos_security implementation
- **Inherent/Permanent Controls**: Built-in macOS security features

### 🎯 **Coverage Areas**
- **CMMC 2.0 Compliance**: Maps to all CMMC Level 1 & 2 requirements
- **NIST 800-171**: Full alignment with DoD cybersecurity standards
- **FCI/CUI Protection**: Safeguards for Federal Contract/Controlled Unclassified Information
- **MDM Integration**: Configuration profile-based enforcement
- **Native osquery**: No external dependencies or curl commands

## 🔧 Requirements

| Component | Version | Notes |
|-----------|---------|-------|
| **Fleet** | 4.15+ | Device management platform |
| **osquery** | 5.7.0+ | Query engine with managed_policies table |
| **macOS** | 15.0+ (Sequoia) | Target operating system |
| **MDM** | Required | Configuration profile enforcement |
| **Permissions** | Admin | Required for policy deployment |

## 📦 Installation

### Fleet CLI (fleetctl) - Recommended
```bash
# Install fleetctl (update URL for your platform)
curl -L https://github.com/fleetdm/fleet/releases/latest/download/fleetctl_darwin.tar.gz | tar xz

# Configure Fleet connection
fleetctl config set --address https://your-fleet-instance.com

# Step 1: Start with Level 1 (Essential Foundation)
fleetctl apply -f cmmc-policies-level-1.yml

# Step 2: Deploy Complete Level 2 (All 167 Controls)
fleetctl apply -f cmmc-policies-level-2.yml
```

### Direct osquery Testing
```bash
# Test individual policies with osqueryi
osqueryi "SELECT 1 FROM managed_policies WHERE domain='com.apple.security.smartcard';"

# Verify file permissions
osqueryi "SELECT 1 FROM file WHERE path='/var/audit' AND mode LIKE '40700';"
```

## ✅ Validation & Testing

### Pre-Deployment Testing
```bash
# Validate YAML syntax
yamllint cmmc-policies-level-1.yml
yamllint cmmc-policies-level-2.yml

# Test queries locally on macOS (Level 2 example)
osqueryi "SELECT 1 FROM managed_policies WHERE domain='com.apple.security.smartcard';"
osqueryi "SELECT 1 FROM file WHERE path='/var/audit' AND mode LIKE '40700';"
```

### Policy Verification
Each policy returns:
- **1** = **PASSING/COMPLIANT** (CMMC requirement satisfied)
- **0** = **FAILING/NON-COMPLIANT** (CMMC violation detected)

### Configuration Profile Validation
```bash
# Check MDM profile deployment
profiles -P | grep "com.apple.security.smartcard"

# Verify managed policies table access
osqueryi "SELECT domain, name, value FROM managed_policies LIMIT 5;"

# Check audit configuration (Level 2)
osqueryi "SELECT 1 FROM file WHERE path='/etc/security/audit_control' AND content LIKE '%flags:%aa%';"
```

## 📊 Policy Categories & File Breakdown

### CMMC Level 1 File (76 Essential Policies)
| Section | Policies | CMMC Domains | File Location |
|---------|----------|-------------|---------------|
| **Authentication** | 3 | IA.L1-3.5.x | cmmc-policies-level-1.yml |
| **Data Protection** | 8 | SC.L1-3.13.x | cmmc-policies-level-1.yml |
| **System Hardening** | 25 | CM.L1-3.4.x, SI.L1-3.14.x | cmmc-policies-level-1.yml |
| **Access Control** | 20 | AC.L1-3.1.x | cmmc-policies-level-1.yml |
| **System Settings** | 20 | Various Level 1 | cmmc-policies-level-1.yml |

### CMMC Level 2 File (167 Complete Policies)
| Section | Policies | CMMC Domains | File Location |
|---------|----------|-------------|---------------|
| **Auditing Controls** | 26 | AU.L2-3.3.x | cmmc-policies-level-2.yml |
| **Authentication** | 8 | IA.L2-3.5.x | cmmc-policies-level-2.yml |
| **iCloud Controls** | 14 | SC.L2-3.13.x | cmmc-policies-level-2.yml |
| **macOS Controls** | 47 | CM.L2-3.4.x, SI.L2-3.14.x | cmmc-policies-level-2.yml |
| **Password Policy** | 11 | IA.L2-3.5.x | cmmc-policies-level-2.yml |
| **System Settings** | 40 | AC.L2-3.1.x, SC.L2-3.13.x | cmmc-policies-level-2.yml |
| **Inherent Controls** | 13 | Various | cmmc-policies-level-2.yml |
| **Permanent Controls** | 3 | SC.L2-3.13.x | cmmc-policies-level-2.yml |
| **Not Applicable** | 3 | N/A | cmmc-policies-level-2.yml |
| **Supplemental** | 2 | Administrative | cmmc-policies-level-2.yml |

## 🛠️ Fleet & osquery Optimization

### Native Table Usage (No curl Required)
Both policy files use only direct, relevant osquery tables:
- **`managed_policies`**: MDM configuration profile verification
- **`file`**: File system permissions, ownership, content checks
- **`launchd`**: Service status and daemon verification  
- **`gatekeeper`**: Application security status
- **`disk_encryption`**: FileVault compliance checking
- **`processes`**: Running process validation
- **`users`**: User account verification
- **`sip_config`**: System Integrity Protection status

## 🎯 Implementation Strategy

### Phased Deployment (Recommended)
1. **Phase 1**: Deploy Level 1 policies (76 controls)
   - Monitor for 1-2 weeks
   - Remediate critical failures
   - Document exceptions

2. **Phase 2**: Deploy Level 2 (167 controls)
   - Deploy to pilot group first
   - Address additional audit findings
   - Full production rollout

### MDM Configuration Profile Requirements
Many controls require MDM profiles. Key domains include:
- `com.apple.security.smartcard`
- `com.apple.applicationaccess`  
- `com.apple.SetupAssistant`
- `com.apple.security.firewall`
- `com.apple.finder`

## 📈 Monitoring & Compliance

### Fleet Dashboard Metrics
- **Policy Pass Rate**: Target >95% for Level 1, >90% for Level 2
- **Critical Failures**: Focus on authentication and audit controls
- **Trending**: Monitor compliance drift over time

### Remediation Priority
1. **Critical**: Authentication, audit daemon, FileVault
2. **High**: Password policy, SSH configuration, firewall
3. **Medium**: iCloud restrictions, service hardening
4. **Low**: Cosmetic settings, user preferences

## 🚨 Common Issues & Solutions

### Managed Policies Table Empty
```bash
# Verify MDM enrollment
profiles status -type enrollment
```

### False Positives
- Review query logic carefully
- Some controls may need environment-specific adjustments
- Document accepted risks and compensating controls

## 🤝 Contributing

### Reporting Issues
1. **Check existing issues** before creating new ones
2. **Provide details**: macOS version, Fleet version, MDM solution
3. **Include examples**: Failing queries, profile configurations

### Contributing Improvements
1. **Fork** this repository
2. **Create feature branch**: `git checkout -b feature/new-control`
3. **Test thoroughly**
4. **Submit pull request** with detailed description

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## ⚖️ Legal & Compliance

- **CMMC Compliance**: Policies align with CMMC 2.0 Level 1 & 2 requirements
- **NIST 800-171**: Supports compliance with DoD cybersecurity standards
- **No Warranty**: Provided "as-is" - validate in your environment
- **Official Sources**: Based on [usnistgov/macos_security](https://github.com/usnistgov/macos_security)

## 🙏 Acknowledgments

- **NIST/usnistgov** for the macOS Security Compliance Project
- **CMMC Accreditation Body** for CMMC 2.0 framework
- **Fleet** for the device management platform
- **osquery Community** for the query engine
- **Contributors** who helped test and improve these policies

## 📈 Compliance Summary

| Metric | Level 1 | Level 2 | 
|--------|---------|---------|
| **Total Policies** | 76 | 167 |
| **MDM Required** | ~60% | ~70% |
| **Complexity** | Basic | Advanced |

---

**Ready to achieve CMMC compliance?** Start with Level 1 for essential security, then progress to Level 2 for complete DoD contractor requirements! 🚀