# Setup Firebase Tools with Admin Credentials

This action installs Firebase Tools from npm and initializes a credentials file for accessing Firebase using admin privileges, skipping the normal login process.  
Note: This action **does not include Node.js setup**. You need to set up Node.js separately.

---

## Features
- Installs Firebase Tools
- Sets up an admin credentials file (no login required)
- Enables various Firebase CLI operations

---

## Usage

Here is an example of how to use this action in a GitHub Actions workflow:

```yaml
jobs:
  sample:
    runs-on: ubuntu-latest
    steps:
      # Clone the repository
      - name: Clone repository
        uses: actions/checkout@v4

      # Set up Node.js
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      # Install Firebase Tools with credentials
      - name: Install Firebase Tools
        uses: DonPok/github-actions-firebase-tools@v1
        with:
          googleApplicationCredentialsContents: ${{secrets.CREDENTIAL}}  
      
      # Use Firebase Tools (example command)
      - name: Use Firebase Tools
        run: firebase projects:list # Lists Firebase projects
```

---

## Inputs

### **`googleApplicationCredentialsContents`** (*Required*)
Set the content of your admin credentials (in JSON format).  
You can configure a secret in your repository by navigating to: **`Settings > Secrets and variables`**.  
Add one of the following formats to the secret:
1. Paste the raw JSON content into the secret text area.
2. Use Base64-encoded JSON content. (If using Base64, configure the `encode` input as described below.)

---

### **`encode`** (*Optional, default: `utf8`*)
If your credentials are Base64-encoded, set this to `base64`. Otherwise, leave it empty.

---

### **`firebaseToolVersion`** (*Optional, default: `latest`*)
Specify the version of Firebase Tools to install from npm. View available versions here:  
[Firebase Tools (npm)](https://www.npmjs.com/package/firebase-tools?activeTab=versions)

---

### **`credentialPath`** (*Optional, default: `default`*)
Define where to save the credentials. The default is:  
`${{runner.temp}}/googleApplicationCredentials.json`

---

## Notes
- **Node.js setup is required** before using this action.
- Always manage credentials securely by storing them as secrets in your repository.

## configuration sample
### base64 encoded credential
```yaml
      # Install Firebase Tools with credentials
      - name: Install Firebase Tools
        uses: DonPok/github-actions-firebase-tools@v1
        with:
          googleApplicationCredentialsContents: ${{secrets.CREDENTIAL}}
          encode: base64
```


### change firebase tool version
```yaml
      # Install Firebase Tools with credentials
      - name: Install Firebase Tools
        uses: DonPok/github-actions-firebase-tools@v1
        with:
          googleApplicationCredentialsContents: ${{secrets.CREDENTIAL}}
          firebaseToolVersion: 13.33.0
```


### change credentialPath
change to working directory.
```yaml
      # Install Firebase Tools with credentials
      - name: Install Firebase Tools
        uses: DonPok/github-actions-firebase-tools@v1
        with:
          googleApplicationCredentialsContents: ${{secrets.CREDENTIAL}}
          credentialPath: ${{ github.workspace }}/.credential.json
```