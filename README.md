''BLOCK ATTACKER TRAFFIC USING VIRTUAL FIREWALL''
# The-Digital-Shield
The defense side of the "Vulnerable-Lab-Exploitation" A virtual Firewall should be installed in between kali and the metasploitable machine and set up a rule that specifically blocks the door(port 80) that I used to get in the victim machine in the 'exploitation' project.
---

## 🛡️ Phase 6: Defensive Mitigation (Blue Team)
After successfully exploiting the target, I transitioned to a **Defensive role** to mitigate the vulnerability using host-based security controls.

### 1. The Strategy: Default Deny
Instead of just patching the software, I implemented an **Access Control List (ACL)** using `iptables` to block the attacker's IP address.

### 2. Technical Implementation (Rule Priority)
I identified that the system had existing permissive rules. To ensure my security policy was enforced, I used the **INSERT** command rather than **APPEND**. 

**Command executed on Metasploitable:**
`sudo iptables -I INPUT -s 'Kali  ip' -j DROP`<img width="919" height="869" alt="Rule" src="https://github.com/user-attachments/assets/3607a31c-8547-4835-867f-3739e77cead6" />


> **Why -I (Insert)?** Firewalls read rules from the top down. By using `-I`, I placed the `DROP` rule at position #1, ensuring the attack was blocked before any `ACCEPT` rules could be processed.

### 3. Verification & Results
* **Attack Attempt:** After applying the rule, I attempted to re-run the Metasploit exploit.
* **Outcome:** The connection timed out. The firewall successfully "dropped" all packets from the attacker, effectively neutralizing the RCE path.<img width="914" height="1034" alt="successful block" src="https://github.com/user-attachments/assets/23a9f370-e204-4954-bdc9-8ae43ac6fce3" />

* **Granular Control:** Verified that while the attacker was blocked, other authorized network traffic could still flow, maintaining system availability.

**Challenges encountered:I used append at fisrt so the rule didn't work as I expected since the traffic was accepted first.'ACCEPT' rule was above so 'DROP' was ignored.
