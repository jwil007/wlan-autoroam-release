# Mobility Score

The **Mobility Score** is a comprehensive 0-100 rating that evaluates how well a WiFi network is configured for seamless roaming. It analyzes real roaming test data and assigns points across six key categories.

## Score Breakdown (115 points → normalized to 100)

### 🚀 Performance (25 points)
Measures actual roaming success and speed.

- **Success Rate** (13 points): Percentage of successful roams
  - 100% success = 13 points
  - Linear scaling down to 0 points
  
- **Roam Speed** (12 points): Average roam duration
  - ≤100ms = 12 points (Excellent)
  - ≤200ms = 9 points (Good)
  - ≤400ms = 6 points (Fair)
  - ≤800ms = 3 points (Poor)
  - >800ms = 0 points

### 📡 Roaming Amendments (15 points)
Evaluates support for 802.11k/v/r roaming standards. **Gradient scoring** rewards partial adoption.

**Enterprise Networks** (802.1X authentication):
- **802.11k** (Neighbor Reports): 5 points × coverage %
- **802.11v** (BSS Transition): 5 points × coverage %
- **802.11r** (Fast Transition): 5 points × coverage %

**Personal Networks** (PSK/SAE authentication):
- **802.11k**: 7.5 points × coverage %
- **802.11v**: 7.5 points × coverage %
- **802.11r**: 0 points (not beneficial without 802.1X key management)

*Example: 67% of APs support 802.11r on enterprise = 3.35 points (67% × 5)*

### 🔄 Configuration Consistency (20 points)
Networks should be uniformly configured across all APs.

- **Protocol Consistency** (8 points): All APs use the same 802.11k/v/r features
- **Security Consistency** (7 points): All APs use identical security settings
- **Data Rate Consistency** (5 points): All APs advertise the same supported rates

Deductions are made for configuration mismatches between APs.

### 📻 RF Health (20 points)
Evaluates channel planning and radio frequency best practices.

- **Clean Channels** (10 points):
  - 2.4GHz: Deduct points for non-standard channels (not 1, 6, or 11)
  - 2.4GHz: Deduct points for overlapping channels
  - All bands: Reward proper channel separation

- **Channel Widths** (6 points):
  - Penalize 40MHz on 2.4GHz (causes interference)
  - Penalize 80MHz on 5GHz (limits capacity in dense deployments)
  - Penalize 160MHz or 320MHz on 6GHz (unnecessary in most environments)

- **Data Rates** (4 points):
  - Penalize 802.11b legacy rates (1, 2, 5.5, 11 Mbps)

### 🔧 Technology (15 points)
Rewards adoption of modern WiFi standards and WPA3 security.

**WiFi Generation** (10 points max):
- **WiFi 7** (802.11be/EHT): 10 points if any 6GHz APs detected
- **WiFi 6E** (802.11ax on 6GHz): 10 points if any 6GHz APs detected
- **WiFi 6** (802.11ax): Up to 5 points (scaled by % coverage across all bands)
- **WiFi 5** (802.11ac): Up to 1 point (scaled by % coverage)
- **WiFi 4 or older**: 0 points

**WPA3 Bonus** (5 points):
- +5 points if **any** WPA3 standard is in use (Enterprise, SAE, or OWE)
- Encourages modern security adoption

*Example 1: Network has 6GHz APs + WPA3 = 10 points (6E) + 5 points (WPA3) = 15 points total*  
*Example 2: Network is 100% WiFi 6 (no 6GHz) + WPA3 = 5 points (WiFi 6) + 5 points (WPA3) = 10 points total*  
*Example 3: Network is 80% WiFi 6 + WPA3 = 4 points (80% × 5) + 5 points (WPA3) = 9 points total*

### 🔒 Security (20 points)
Evaluates authentication strength. Points based on **strongest** security suite detected.

- **802.1X** (WPA3/WPA2 Enterprise): 20 points
- **SAE** (WPA3-Personal): 13 points
- **PSK** (WPA2-Personal): 10 points
- **OWE** (Enhanced Open): 7 points
- **Open** (No encryption): 0 points

*WPA3-Enterprise detection: Looks for SHA-256 or SHA-384 in authentication suites*

---

## Letter Grades

| Score | Grade | Rating |
|-------|-------|--------|
| 90-100 | A | Excellent |
| 80-89 | B | Good |
| 70-79 | C+ | Fair |
| 60-69 | C | Adequate |
| 50-59 | D | Poor |
| 0-49 | F | Critical Issues |

## Color Coding

- 🟢 **Green** (≥80): Well-optimized network
- 🟡 **Yellow** (60-79): Room for improvement
- 🔴 **Red** (<60): Significant issues affecting roaming

---


