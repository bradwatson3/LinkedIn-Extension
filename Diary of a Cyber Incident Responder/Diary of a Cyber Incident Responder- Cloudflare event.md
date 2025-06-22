# DFIR Notes & Reflections

## Walkaways
- Breaches can't be removed completely. Most frameworks assume already breached, behave as such.
- Have incident response plan, regularly validate it. Should go into IR assuming every decision you do is heavily scrutinised.
- Test, detect, respond and recover processes for critical systems.
- Have a defendable process for handling breaches.
- Learn from front line – use a 'risk-based prioritisation' approach to defending against risks.

> Belief that we need to learn with each other. Government departments have legislation that means they can't share info with us, so companies should work together more to better defend against criminals.

---
## Cyber Resilience

> Withstand, respond and recover. Some companies are too large to fail (like Optus). It's about continuously delivering outcomes despite adverse cyber events. Despite what's going on, we need to keep going.

---
## Example Diary Day
- Tweak new script to compare phone company data against CRM data to identify stolen IP, swear affidavit.
- Finalise containment report for large corporate ransom strike incident.
- Write up evidence collection plan for suspect IT manager.
- Finalise instructions for consented search of an office for stolen IP.
- Onboard new customer for corporate network breach. Initial access is not known by local IT team, containment is unknown.

---
## What is DFIR?
- NIST Computer Science Incident Handling Guide, Guide to Integrating Forensic Techniques into IR.
- Cyber incident response is the process of responding to cyber attack, aim of minimising damage.
- Digital forensics is the act of collecting data for use later.
- **Contain → Eradicate → Recover → Report** (NIST 800-6r12)
- **Collect → Examine → Analyze → Report** (NIST 800-86)

> At each level, need to think of defensibility. Everything you do as a responder will be closely examined. You don't want regret when your 2 hours of response will be examined 2 years later in court.

---
## Matt's 3 C's
- **Compliance** – What standards it meets  
- **Culture** – Want to have a culture of data safety  
- **Complexity**

> Personal cyber ≠ Organisational cyber

---
## Matt's 3 Rules
1. Use only trusted devices  
2. Do everything that you can to protect that trust  
3. Do everything that you can to protect your most valuable accounts

---
## Attributes of a Great Cyber Emergency Responder
- Calm under pressure  
- High level of technical skills  
- Clear communication  
- Confident in their decisions  
- Having investigative mindset

> In cyber, we can have an overwhelming sense of panic because there can be too many incidents etc – Have hope.

---
## Kill Chain – Military Term
We use kill chains to decide how we should invest our time, money and other resources to build our capabilities and gain an advantage over our adversaries:
1. Find the target  
2. Determine target's location, course and speed  
3. Communicate information coherently to the platform launching the weapon  
4. Launch the attack using anything from a kinetic weapon to electromagnetic systems to cyber

> To defeat adversaries' attacks we look for links where the adversary has a vulnerability and we have an advantage.  
> **Defender's advantage** – We just need to break one chain of attacker to defend.

---
## Cyber Kill Chain
In cyber world, there's a similar process attackers use:
- Recon  
- Weaponization  
- Delivery  
- Exploitation  
- Installation  
- Command and Control  
- Exfiltration  

> Stop the attack at any point on CKC and you're successful against adversary.  
> **CKC is theory, MITRE is practical.**

---
## MITRE Framework
Always helpful to find initial access. Only so many ways to gain initial access, go through list to find it.

---
## Activity
- Two minute presentation  
- Describe the group  
- One tactic, technique or procedure

---
## APT29 (aka Cozy Bear)
Advanced 'criminal group' allegedly acting on behalf of the Russian Foreign Intelligence. They're believed to have been operating since 2008, and have interacted in two major campaigns:
- **Operation Ghost** (started in 2013): Targeted various ministries of foreign affairs across Europe, including the American embassy in Europe  
- **SolarWinds** (2019): Conducted a highly sophisticated supply chain cyber operation  

> In 2021, US and UK governments collectively attributed the SolarWinds compromise to this group.

---
## The 'Assume Breach' Model
Modern cybersecurity frameworks assume you will be breached. Emphasis is on early detection and recovery:
- **Identify** – Good documentation  
- **Protect** – Strong passwords, MFA, cyber training, firewalls  
- **Detect** – Detect early  
- **Respond**  
- **Recover**  
- **Govern**

---
## Today – DFIR Steps
We want to do a **CSF-Identify**, so we know what evidence to collect.

### Evidence Collection Targets
- **Cloud**: Logs (best evidence)  
- **Servers**: Memory (last few minutes/hours), logs (last few hours/days), snapshots (last few days/months)  
- **Computers**: Memory, logs, web records, and forensics

> Avoid collecting everything. Be targeted. Provide reasons for what you collect and why.  
> If we're going into forensics, it means monitoring tools have failed.

---
## Case 1: Don't Cross the Wires

**Timeline of events leading to breach:**
- Factory planned firewall change from telco to MSP
- Cutover day set for Mon 32 Feb 2022 (date likely fictitious)
- Testing done on Fri 29 Feb – server couldn’t receive data via SFTP
- Team opened all ports >1023 for testing
- Same day, server ORDER01 appeared on Shodan with RDP open
- 260k+ failed login attempts recorded
- At 9:16 PM, correct password guessed
- By 9:20 PM, account on machine changed
- By 1:35 AM, suspicious software placed:
  - ADVANCED_IP_SCANNER
  - SOFTWARE_REPORTED_TOOL
- 2:10 AM – Mimikatz found:
  - BulletsPassView, Dialupass, MailPV, RDPV
- 2:23 AM – Malware executed
- 4:55 AM – Alert triggered, IT texted
- 5:05 AM – Staff unable to access system
- 5:30 AM – Factory manager’s password changed
- 9:30 AM – Cyber incident declared
- 11:30 AM – Firewall ports >1023 closed
- 1:30 PM – Matt engaged for IR
- ORDER01 connected to 50 servers – isolation not viable
- Velociraptor installed on 100 servers, threat hunting begins
- Deadbox images collected for overnight scans
- Audit for non-2FA accounts initiated

**Lessons Learned:**
- Firewall should’ve been locked down after testing
- Rename admin accounts to reduce brute-force risk
- Use stronger passwords + enforce MFA
- Lock out accounts after repeated failures

---
## Case 2: It's Not Hackers
**Event context – 43 Nov 2023:**
- Mid-sized CEO suspicious of IT provider
- Large unexplained ATO payment
- Claims of spying and unauthorized access

**Investigation Steps:**
- Refer tax issues to forensic accountant
- Plan evidence collection
- Consider new MSP
- Conduct interviews

**Findings:**
- 37 Nov – Successful login from overseas VPN
- 4 days later – Fake invoice sent from CEO’s account
- 10 days later – 2 fake admin accounts created
- Criminals renamed company to real estate entity in Office 365
- Phishing was initial access vector
- IT provider unaware; CEO confused by fake accounts
- Password sharing limited forensic ability

**Lessons Learned:**
- One account per person
- Enable logging
- Protect against phishing
- Use professional monitoring
- Consolidate authentication
- Reduce internet exposure

---
## Case 3: Cold Ransom in Azure

**Timeline:**
- 9:30 AM – Company contacts Matt, ransom detected in Azure
- 10:30 AM – Cold customer; no prior data on file
- 11:30 AM – Instructions + standard terms sent

**Context:**
- Migrating accounting system to Azure for 100 franchise stores
- 1.8TB data migrated
- Configuration outsourced twice
- Ransom note found; data encrypted

**Investigation Focus:**
- Pay ransom or investigate?
- Identify initial access vector

**Potential Vectors:**
- Lateral movement from MSP’s other clients
- Phished MSP employee
- Admin creds leaked or weak passwords
- Vulnerable app or open network exposure

**Decision Point:**
- Investigate MSP only (not wider cloud or other IT vendors)

**Actions Taken:**
- Captured memory of UserServer01
- Full disk image captured

**Findings:**
- Nothing useful in memory
- Massive password guess attempts → password guessed
- Logs cleared on AppServer01
- No logging in place
- Files heavily accessed

**Outcome:**
- Initial access confirmed through brute-force
- No proof of exfiltration; assumed due to bandwidth use
- Containment achieved
- Post-incident recommendations made
