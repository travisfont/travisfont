# Security Tokens

A **security token** is a physical or digital device used in authentication to verify a user's identity, typically as part of **multi-factor authentication (MFA)** or **two-factor authentication (2FA)**. 

## Function and Types of Security Tokens

### 1. **Authentication**  
   - Provides an additional layer of security beyond just a username and password.
   - Generates a **one-time password (OTP)**, uses **public-key cryptography (PKI)**, or relies on **biometric verification** to confirm identity.

### 2. **Types of Security Tokens**  
   - **Hardware Tokens**: Physical devices (e.g., USB tokens, smart cards, key fobs).  
     - Example: **YubiKey**, **RSA SecurID**.  
   - **Software Tokens**: Virtual tokens (e.g., mobile apps like **Google Authenticator**, **Microsoft Authenticator**).  
   - **Disconnected Tokens**: Generate OTPs without direct system connection.  
   - **Connected Tokens**: Require physical connection (e.g., USB, NFC).  

### 3. **Key Benefits**  
   - **Prevents Unauthorized Access**: Even if a password is stolen, attackers need the token.  
   - **Mitigates Phishing & Man-in-the-Middle Attacks**: Dynamic codes expire quickly.  
   - **Compliance**: Meets regulatory requirements (e.g., **PCI-DSS, GDPR, HIPAA**).  

### 4. **Common Use Cases**  
   - Online banking, corporate VPN access, cryptocurrency wallets (e.g., **Ledger, Trezor**), and cloud services.  
