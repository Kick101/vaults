RBAC lets you define **who can do what** and **where** in the Microsoft Defender portal. It helps secure access by assigning roles and scoping permissions to device groups.

---
### 🔐 Key Concepts

- **Custom Roles**: Control *actions* users can perform (e.g., investigate, remediate).
- **Device Groups**: Scope *visibility*—users only see and act on devices in assigned groups.
- **Entra ID Groups**: Link user groups to RBAC roles and device groups.

### 🏷️ Access Levels by Default

| **Microsoft Entra Role**     | **Defender Access Level**       |
|-----------------------------|----------------------------------|
| Global Administrator         | Full Access                      |
| Security Administrator       | Full Access                      |
| Security Reader              | Read-Only Access                 |
| Defender Global Admin Role   | Full access to all devices       |

### ⚠️ Security Best Practice
> Use **least privilege** principles—assign only what is needed.  
> Avoid using **Global Administrator** unless absolutely necessary.

### ✅ Setup Steps

1. **Define Admin Roles**
2. **Assign Permissions**
3. **Create Device Groups** (by tag, domain, name, etc.)
4. **Link Roles to Entra ID Groups**

---
