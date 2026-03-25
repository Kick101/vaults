Prevent data breaches and leaks **in real time**, while enabling **BYOD (Bring Your Own Device)** securely.

---
### 🔗 Integration
- Works with **Microsoft Entra ID** (formerly Azure AD).
- Integrated into **Microsoft Defender for Cloud Apps**.
- Uses **Conditional Access policies** to apply access and session controls.

---
### ⚙️ Configuration Steps
1. In **Microsoft Entra ID**, create a Conditional Access policy.
2. Under **Access controls > Session**, select:
   - `Use Conditional Access App Control`.
   - Optionally use **custom controls** via Defender for Cloud Apps portal.
3. Define:
   - **Who** (users/groups)
   - **What** (apps)
   - **Where** (locations/networks)

---
### 📌 Use Cases via Access & Session Policies

**🛑 Prevent Data Exfiltration**
  - Block downloads, cut/copy/paste/print on unmanaged devices.

**🔐 Protect on Download**
  - Force Azure Information Protection labeling on download.

**🏷️ Enforce File Labeling**
  - Block upload of unlabeled sensitive files.

**🔍 Monitor User Sessions**
  - Track risky user behavior and refine future session policies.

**🚫 Block Access**
  - Deny app access based on risk (e.g., lack of device certificate).

**⚠️ Block Custom Activities**
  - E.g., prevent sending sensitive content in Teams/Slack messages.

---
### 📄 Example: Block Sensitive IM in Teams

1. Create a **Conditional Access policy** (with custom controls).
2. In **Defender for Cloud Apps**, create a **Session Policy**:
   - Use `Block sending of messages based on real-time content inspection` template.
3. Under **Activity Source**, select: `Send Teams message`.
4. Enable **Content Inspection**:
   - Define sensitive content via:
     - Built-in expressions
     - Custom regex
5. Under **Actions**, select:
   - `Block` the message
   - Create alert for admin

**📝 Result**: Users are notified when blocked for sending sensitive content.

