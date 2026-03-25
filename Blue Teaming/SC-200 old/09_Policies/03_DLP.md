1. Go to **Defender for Cloud Apps > Control > Policies > Create policy > File policy**
2. Configure:
   - **Severity**: Critical/High/Medium/Low
   - **Category**: "DLP"
   - **Filters**: Define scope (files, folders, apps)
   - **Users**:
     - All file owners
     - From selected groups
     - Exclude selected groups
   - **Inspection method**:
     - Built-in DLP
     - Data Classification Services (recommended)
   - **Governance Actions**: Select desired enforcement steps
3. Select **Create**