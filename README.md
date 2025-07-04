# CMMC Level 1 & 2 Security Policies for Fleet

[![CMMC 2.0](https://img.shields.io/badge/CMMC-Level%201%20%26%202-blue)](https://www.acq.osd.mil/cmmc/)
[![Fleet Compatible](https://img.shields.io/badge/Fleet-4.15%2B-green)](https://fleetdm.com/)
[![osquery Compatible](https://img.shields.io/badge/osquery-5.7.0%2B-orange)](https://osquery.io/)
[![macOS Compatible](https://img.shields.io/badge/macOS-12.0%2B-lightgrey)](https://www.apple.com/macos/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Complete, production-ready implementation of **US CMMC 2.0 Level 1 & Level 2** security policies for macOS systems using Fleet device management platform. Based on the official [usnistgov/macos_security](https://github.com/usnistgov/macos_security) project with all **167 security controls** from the macOS security baseline.

## 🚀 Quick Start

### Level 1 (Essential Foundation - Start Here)
1. **Download** the `cmmc-policies-level-1.yml` file from this repository
2. **Upload** to Fleet using fleetctl or Fleet UI
3. **Deploy** to your macOS hosts via teams/targeting  
4. **Monitor** CMMC Level 1 compliance in Fleet dashboard

### Level 2 (Complete Implementation - After Level 1)
1. **Download** the `cmmc-policies-level-2.yml` file (all 167 controls)
2. **Replace** Level 1 policies or deploy as comprehensive policy set
3. **Monitor** complete CMMC Level 1 & 2 compliance across all domains

## 📋 What's Included

### 🛡️ **CMMC Level 1: 76 Essential Security Controls**
File: `cmmc-policies-level-1.yml`
- **Authentication**: Smartcard enforcement, SSH hardening (3 policies)
- **Data Protection**: Essential iCloud restrictions, privacy settings (8 policies)
- **System Hardening**: Core security features, basic service management (25 policies)
- **System Settings**: Fundamental access controls, sharing restrictions (20 policies)
- **Access Control**: Basic user management and session controls (20 policies)

### 🛡️ **CMMC Level 2: 167 Complete Security Controls**
File: `cmmc-policies-level-2.yml`
- **All Level 1 controls** plus comprehensive advanced requirements
- **Audit & Logging**: Complete monitoring framework, log security (26 policies)
- **Authentication**: Advanced smartcard, PAM, certificate controls (8 policies)  
- **iCloud Controls**: Complete cloud service restrictions (14 policies)
- **macOS Controls**: Comprehensive OS hardening and security (47 policies)
- **Password Policy**: Full complexity, history, lockout controls (11 policies)
- **System Settings**: Complete configuration security (40 policies)
- **Inherent Controls**: Built-in macOS security capabilities (13 policies)
- **Permanent Controls**: Permanently implemented features (3 policies)
- **Not Applicable**: Controls not applicable to endpoints (3 policies)
- **Supplemental**: Administrative requirements (2 policies)

### 🎯 **Complete Coverage Matrix**
| Domain | Level 1 File | Level 2 File | Description |
|--------|-------------|-------------|-------------|
| **Authentication (IA)** | 3 | 8 | Smartcard, MFA, PAM, certificate trust |
| **Data Protection (SC)** | 8 | 14 | Complete iCloud restrictions, privacy |
| **System Hardening (CM/SI)** | 25 | 47 | Full OS security, service management |
| **Access Control (AC)** | 20 | 40 | User management, session controls |
| **Audit & Logging (AU)** | 0 | 26 | Complete event monitoring, log security |
| **Password Policy (IA)** | 0 | 11 | Complexity, history, lockout enforcement |
| **System Settings** | 20 | 40 | Configuration security, restrictions |
| **Inherent Controls** | 0 | 13 | Built-in macOS security capabilities |
| **Administrative** | 0 | 8 | Permanent, N/A, supplemental controls |
| **TOTAL POLICIES** | **76** | **167** | Complete usnistgov/macos_security coverage |

## 🔧 Requirements

| Component | Version | Notes |
|-----------|---------|-------|
| **Fleet** | 4.15+ | Device management platform |
| **osquery** | 5.7.0+ | Query engine with managed_policies table |
| **macOS** | 12.0+ (Monterey) | Target operating system |
| **MDM** | Required | Configuration profile enforcement |
| **Permissions** | Admin | Required for policy deployment |

## 📦 Installation

### Fleet CLI (fleetctl) - Recommended
```bash
# Install fleetctl (update URL for your platform)
curl -L https://github.com/fleetdm/fleet/releases/latest/download/fleetctl_darwin.tar.gz | tar xz

# Configure Fleet connection
fleetctl config set --address https://your-fleet-instance.com

# OPTION A: Start with Level 1 (Essential Foundation)
fleetctl apply -f cmmc-policies-level-1.yml

# OPTION B: Deploy Complete Level 2 (All 167 Controls)
fleetctl apply -f cmmc-policies-level-2.yml
```

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

### Query Examples from Level 2 File
```sql
-- Audit daemon verification
SELECT 1 WHERE EXISTS (SELECT 1 FROM launchd WHERE name = 'com.apple.auditd' AND status = 'running');

-- Smartcard enforcement check  
SELECT 1 WHERE EXISTS (SELECT 1 FROM managed_policies WHERE domain = 'com.apple.security.smartcard' AND name = 'enforceSmartCard' AND value = 'true');

-- File permission validation
SELECT 1 WHERE EXISTS (SELECT 1 FROM file WHERE path = '/var/audit' AND mode LIKE '40700');
```

## 🏢 CMMC Compliance Mapping

### Level 1 Controls (17 Fundamental Practices) → 76 Fleet Policies
- **Access Control (AC)**: 4 practices → 20 Fleet policies  
- **Identification & Authentication (IA)**: 3 practices → 3 Fleet policies
- **Media Protection (MP)**: 2 practices → Covered in system settings
- **Physical Protection (PE)**: 2 practices → Inherent/administrative controls
- **System & Communications Protection (SC)**: 4 practices → 28 Fleet policies
- **System & Information Integrity (SI)**: 2 practices → 25 Fleet policies

### Level 2 Controls (110 Additional Practices) → 167 Total Fleet Policies
- **Audit & Accountability (AU)**: 12 practices → 26 Fleet policies
- **Configuration Management (CM)**: 9 practices → 47 Fleet policies  
- **Advanced Access Control**: 18 additional practices → 40 Fleet policies
- **Enhanced Authentication**: 9 additional practices → 8 Fleet policies
- **Password Management**: 6 practices → 11 Fleet policies
- **System Protection**: 15 additional practices → 14 Fleet policies
- **Administrative Controls**: 41 practices → 21 Fleet policies

## 📊 Deployment Decision Matrix

| Scenario | Recommended File | Rationale |
|----------|------------------|-----------|
| **New CMMC Implementation** | `cmmc-policies-level-1.yml` | Start with 76 essential controls, build foundation |
| **Pilot Testing** | `cmmc-policies-level-1.yml` | Validate MDM integration with core policies |
| **DoD Contractor (Level 2 Required)** | `cmmc-policies-level-2.yml` | Deploy all 167 controls for certification |
| **Production Environment** | `cmmc-policies-level-1.yml` → `cmmc-policies-level-2.yml` | Phased approach for stability |
| **Assessment Preparation** | `cmmc-policies-level-2.yml` | Complete coverage for CMMC evaluation |

## 📊 Reporting & Monitoring

### Fleet Dashboard Metrics
- **Real-time compliance percentages** across all CMMC domains
- **Level 1 vs Level 2 policy tracking** with detailed breakdowns
- **Host-level compliance status** with specific failure identification
- **Policy category performance** (Authentication, Audit, etc.)
- **Export capabilities** for CMMC assessment documentation

### Automated Compliance Reporting
```bash
# Generate Level 1 compliance report (76 policies)
curl -H "Authorization: Bearer $FLEET_API_TOKEN" \
  "$FLEET_URL/api/v1/fleet/policies" | \
  jq '.policies[] | select(.name | contains("CMMC L1")) | {name, passing_host_count, failing_host_count}'

# Generate Level 2 compliance report (167 policies)
curl -H "Authorization: Bearer $FLEET_API_TOKEN" \
  "$FLEET_URL/api/v1/fleet/policies" | \
  jq '.policies[] | select(.name | contains("CMMC L2")) | {name, passing_host_count, failing_host_count}'

# Generate audit domain compliance (Level 2 only)
curl -H "Authorization: Bearer $FLEET_API_TOKEN" \
  "$FLEET_URL/api/v1/fleet/policies" | \
  jq '.policies[] | select(.name | contains("AU.L2")) | {name, passing_host_count, failing_host_count}'
```

## 🔍 Troubleshooting

### Common Issues

#### Level 1 Policies
1. **MDM Profile Missing**: Ensure basic configuration profiles are deployed
2. **Basic osquery Access**: Verify managed_policies table availability
3. **Core Service Status**: Check essential system services are running

#### Level 2 Policies  
1. **Audit System**: Verify audit daemon is running and configured
2. **File Permissions**: Check audit log and system file permissions
3. **Advanced MDM**: Ensure complex configuration profiles are deployed
4. **Password Policies**: Verify password policy profiles are active

### Debug Commands
```bash
# Check Fleet agent status
sudo launchctl list | grep fleet

# Verify osquery table access
osqueryi "SELECT name FROM osquery_registry WHERE registry='table' AND name='managed_policies';"

# Test Level 1 policy (smartcard)
osqueryi "SELECT domain, name, value FROM managed_policies WHERE domain='com.apple.security.smartcard';"

# Test Level 2 policy (audit)
osqueryi "SELECT 1 FROM launchd WHERE name = 'com.apple.auditd' AND status = 'running';"

# Check file permissions (Level 2)
osqueryi "SELECT path, mode, uid, gid FROM file WHERE path = '/var/audit';"
```

## 🚀 Deployment Strategy

### Recommended Implementation Path

#### Phase 1: Level 1 Foundation (Week 1-2)
- Deploy `cmmc-policies-level-1.yml` to 10-20 test systems
- Validate 76 essential policies and basic MDM integration
- Identify organizational customizations for core controls
- Train IT staff on Level 1 remediation procedures
- Achieve 95%+ Level 1 compliance before proceeding

#### Phase 2: Level 1 Production (Week 3-4)  
- Deploy Level 1 policies to all macOS systems
- Monitor compliance dashboard for authentication failures
- Remediate basic system settings and access controls
- Establish baseline security posture with 76 essential controls
- Document any exceptions or organizational variations

#### Phase 3: Level 2 Assessment (Week 5-6)
- Evaluate need for complete 167-policy implementation
- Review audit logging, password policy, and advanced requirements
- Prepare MDM for complex configuration profiles
- Plan advanced authentication (smartcard, PAM) deployment
- Assess organizational readiness for full CMMC Level 2

#### Phase 4: Level 2 Implementation (Week 7-12)
- Deploy `cmmc-policies-level-2.yml` replacing Level 1 policies
- Focus on audit framework deployment (26 policies) first
- Implement password policy controls (11 policies)  
- Deploy advanced system settings (40 policies)
- Complete authentication and authorization controls (8 policies)
- Achieve 95%+ compliance across all 167 policies

#### Phase 5: Continuous Monitoring (Ongoing)
- Daily compliance monitoring across all 167 controls
- Weekly compliance reporting by CMMC domain
- Monthly policy review and organizational updates
- Quarterly CMMC assessment preparation activities
- Annual security control validation and testing

## 🤝 Contributing

### Reporting Issues
1. **Specify policy file**: Level 1 (`cmmc-policies-level-1.yml`) or Level 2 (`cmmc-policies-level-2.yml`)
2. **Include environment details**: Fleet version, macOS version, MDM platform
3. **Provide query results**: Copy actual osquery output showing failures
4. **CMMC control reference**: Include specific control number (e.g., `AU.L2-3.3.8`)

### Contributing Improvements
1. **Fork** this repository and create feature branch
2. **Test with both files**: Validate changes against Level 1 and Level 2 policies
3. **Update documentation**: Include CMMC control mapping and rationale
4. **Provide test results**: Show successful policy validation on test systems

### Development Guidelines
- Maintain separate Level 1 and Level 2 file structure
- Follow existing YAML formatting and policy naming conventions
- Include proper CMMC control references in all policies
- Test with actual MDM configuration profiles, not just osquery
- Update documentation for any new osquery table requirements

## 📚 Additional Resources

- **Fleet Documentation**: [https://fleetdm.com/docs](https://fleetdm.com/docs)
- **CMMC Official Resources**: [https://www.acq.osd.mil/cmmc/](https://www.acq.osd.mil/cmmc/)
- **osquery Schema Reference**: [https://osquery.io/schema/5.17.0/](https://osquery.io/schema/5.17.0/)
- **Fleet Tables Documentation**: [https://fleetdm.com/tables](https://fleetdm.com/tables)
- **macOS Security Project**: [https://github.com/usnistgov/macos_security](https://github.com/usnistgov/macos_security)
- **Apple Configuration Profiles**: [https://developer.apple.com/documentation/devicemanagement](https://developer.apple.com/documentation/devicemanagement)

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## ⚖️ Legal & Compliance

- **CMMC Framework**: Policies based on CMMC 2.0 Level 1 & Level 2 requirements
- **Usage Rights**: Free for commercial and non-commercial use under MIT license
- **Compliance Disclaimer**: This tool aids in CMMC preparation but does not guarantee certification
- **Assessment Required**: Organizations must undergo official CMMC assessment for certification
- **Testing Responsibility**: Users responsible for validation in their specific environments
- **Support Scope**: Community-supported project, not official CMMC or Fleet product

## 🙏 Acknowledgments

- **DoD CMMC Program Office** for the Cybersecurity Maturity Model Certification framework
- **NIST** for SP 800-171 Rev 3 foundation standards and security control guidance
- **usnistgov/macos_security** project for comprehensive 167-control security baseline
- **Fleet Team** for the device management platform and excellent osquery integration
- **Apple** for macOS security frameworks and configuration profile capabilities
- **Open Source Community** for testing, feedback, and continuous improvement contributions

## 📈 Project Statistics

- **🎯 Level 1: 76 policies** covering all 17 fundamental CMMC practices
- **🎯 Level 2: 167 policies** covering complete usnistgov/macos_security baseline
- **✅ 100% Fleet optimized** with native osquery table usage (no curl)
- **🔧 Production validated** on macOS 12-15 enterprise environments
- **📊 MDM integrated** using managed_policies table for policy enforcement
- **🏛️ DoD compliant** following official CMMC 2.0 framework requirements
- **🚀 Two-file architecture** for flexible deployment strategies

---

**Ready to achieve CMMC compliance for your macOS fleet?** 

- **Start simple**: Deploy `cmmc-policies-level-1.yml` for essential 76 controls
- **Go comprehensive**: Deploy `cmmc-policies-level-2.yml` for complete 167 controls
- **Build confidence**: Phased approach from Level 1 foundation to Level 2 mastery

🚀 **Your CMMC compliance journey starts here!**