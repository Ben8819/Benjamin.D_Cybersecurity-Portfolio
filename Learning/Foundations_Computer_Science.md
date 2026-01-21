# 🧠 Computer Science Foundations for Cybersecurity

Understanding how computers work under the hood helps me analyze system behaviour, investigate anomalies, and interpret low-level indicators during SOC work.

This document summarizes the essential CS knowledge supporting my defensive skills.

---

# 1. Binary, Hex & Data Representation

Everything on a computer ultimately becomes **bits** (0 or 1).

### Why this matters in cyber
- Logs often contain hex/binary elements  
- Malware frequently uses XOR and bitwise operations  
- Memory dumps require hex/binary interpretation  

### Key Concepts
- **Bit / Byte**  
- **Two’s complement** (negative numbers)  
- **Hexadecimal** (`0xFF` = 255)  
- **ASCII / Unicode encoding**  

Example:   
"A" = 65 decimal = 0x41 = 01000001


---

# 2. Logic Gates & Boolean Operations

The physical building blocks of processors.

| Gate | Meaning | Cyber Relevance |
|------|---------|------------------|
| AND | Both inputs must be 1 | signature matching logic |
| OR | Either input can be 1 | alert conditions |
| XOR | Inputs must differ | malware obfuscation |
| NOT | Flips bit | low-level anti-analysis |

**XOR encryption example:**<br>
10101010<br>
XOR<br>
11110000<br>
01011010


---

# 3. CPU Architecture & Instruction Flow

### CPU cycle  
1. **Fetch** instruction  
2. **Decode** it  
3. **Execute** with ALU  
4. **Write back** result  

### Components  
- Registers  
- ALU  
- Cache levels  
- Control unit  

### Why this helps SOC work
- CPU spikes → cryptomining/malware  
- Knowing instruction flow clarifies exploit behavior  
- Memory operations explain buffer overflows  

---

# 4. Memory Architecture

### Types
- **Registers** (fastest)  
- **Cache (L1/L2/L3)**  
- **RAM**  
- **Disk / NVMe**  

### Memory Layout (process-level)
[ Stack ]<br>
[ Heap ]<br>
[ BSS ]<br>
[ Data ]<br>
[ Text ]

### Why cyber analysts care
- Malware hides in memory  
- Sysmon Event ID 10 relates to memory access  
- Forensics tools like Volatility operate on RAM structures  

---

# 5. Buses & Component Communication

### Major buses
- Address bus  
- Data bus  
- Control bus  
- PCIe  
- USB  
- NVMe protocol  

### Security Angle
- PCIe devices can perform DMA attacks  
- USB is a common attack vector (BadUSB)  
- Bus sniffing is used in hardware implants  

---

# 6. C++ Foundation

C++ is common in malware, exploits, agents, and performance-heavy applications.

### Key C++ Concepts I learned
- Variables & types  
- Control flow  
- Functions  
- Pointers & references  
- Memory allocation (`new`, `delete`)  
- Basic classes & OOP  
- Compilation pipeline (preprocessor → compile → link)  

### Why SOC analysts benefit from C++
- Many malware samples are compiled C++ binaries  
- Helps understand memory corruption vulnerabilities  
- Improves process-level analysis skills  
- Assists in interpreting disassembly or API calls  

---

# 7. How These Fundamentals Improve My Cyber Skills

### In investigation:
- CPU anomalies → process spikes  
- Memory patterns → injection or shellcode  
- Hex data → indicators of compromise  

### In SIEM analysis:
- Understanding system behaviour = fewer false positives  
- Better log interpretation  
- Ability to identify abnormal execution sequences  

### In threat hunting:
- Detect subtle attacker techniques  
- Spot malware persistence mechanisms  
- Understand how programs communicate internally  

---

# Summary

Low-level computer science knowledge helps me become a more effective SOC Analyst by improving:

- Situational awareness  
- Technical depth  
- Investigation accuracy  
- Understanding of how attacks really work  
- Ability to interpret complex telemetry
