# SignSure — Digital Signature Creation & Verification

An educational implementation of the workflow in the supplied diagrams:
**sender message → signature creation → transfer message/signature → receiver verification**.

It supports two algorithms:
- **RSA-PSS** using RSA-2048 and SHA-256 (traditional cryptography)
- **ML-DSA-65** (post-quantum digital-signature algorithm standardized in NIST FIPS 204)

## Requirements
- Java 21 or newer
- Maven 3.9+
- A browser

## Run locally
From this folder:

```bash
mvn spring-boot:run
```

Open <http://localhost:8080>.

To create a runnable jar:

```bash
mvn clean package
java -jar target/digital-signature-system-1.0.0.jar
```

## Demo walkthrough
1. Choose RSA-PSS or ML-DSA-65.
2. Click **Generate keys**.
3. Enter a message and click **Sign message**.
4. Download the signed package, or use the receiver panel.
5. Click **Verify signature**. A valid signature should pass.
6. Click **Tamper with message**, then verify again; verification should fail.
7. Repeat with the other algorithm and compare key/signature byte sizes.

## How it works
- The signing algorithm signs the message bytes; the standard library/provider handles its required hashing/encoding.
- The page also calculates and displays a SHA-256 fingerprint of the received message for learning/inspection.
- RSA-PSS uses SHA-256, MGF1-SHA-256, a 32-byte salt and trailer field 1.
- ML-DSA-65 is provided by Bouncy Castle. Its internal signing procedure is not RSA and should not be described as “RSA with a different key size”.
- Public keys are X.509 SubjectPublicKeyInfo encoded, and private keys are PKCS#8 encoded, both represented as Base64.

## Notes and limitations
- This is a local educational prototype, not a production-ready signing service.
- The private key is shown in the page and sent to the local backend for signing. Never use real or confidential private keys.
- No account identity, certificate authority, trusted timestamp, revocation checking, encryption, or secure key vault is implemented.
- Timing results include HTTP/local processing overhead and are indicative only. For a rigorous performance study, run many trials, control the environment, and report averages/percentiles.
- The browser does not automatically trust a public key: the receiver must obtain the correct public key through a trusted channel.
- Keep this service on localhost while experimenting. Do not expose it to the public internet without security review.

## Project structure
```text
src/main/java/edu/project/signature/
  SignatureApplication.java
  CryptoService.java
  SignatureController.java
src/main/resources/
  application.properties
  static/index.html
pom.xml
```
