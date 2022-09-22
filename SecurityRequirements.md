# Security Requirements Report
Team OneWon
## Essential Interactions, Diagrams, and Alignment Analysis

### Use Case 1: Implementing a Truststore

![Keycloak System Engineering Diagram]( ./Diagrams/Truststore.png "Keycloak Trust Diagram")

#### Description

The truststore is a foundational part of the Keycloak mechanism for remote connections.  Any Keycloak implementation that will connect to external systems will need the certificate of any external server to establish a secure SSL connection.  Remote server or Certificate Authority certificates must be stored in order to validate identity.  This validation is needed to prevent man-in-the-middle attacks.


#### Use Case

In this case, a non-privledged user will connect to an external server using Keycloak.  The user expects to connect to the external server without issue and without risk of credential theft.  The user believes proper care has been taken to configure the system and underlying security in order to connect to the expected endpoint.  Server configuration should be for maximum security to prevent any security breach from external entities.



#### Misuse Case

In reality, a hacker may attempt to infiltrate the Keycloak server and the hacker's goals may not be immediately apparent.  For example, curiousity may be a motivating factor which leads to an opportunistic security breach.  Should the Keycloak server be attacked, layers of security should be in place to maintain the integrity of the truststore.  Using a strong truststore password will eliminate the possibly of the bad actor altering the truststore file.  Similarly, a security certificate must be swiftly revoked from the truststore should a remote server's certificate become compromised.  Regular logging and auditing will eliminate these potential security risks.  The hacker may attempt to alter or delete logs to cover any trace of their presence in the system.  An alerting feature may be configured to notify administrators of mass file changes within the system.



#### Security Requirement

The Keycloak truststore is created and maintained using the pre-built Java "keytool" utility, therefore the truststore utilizes the underlying security of keytool.  The truststore password will be embedded into web applications that make use of the truststore for connection purposes.  Only administrators should have access to the embedded password, and robust system security is necessary to prevent external hackers from accessing the password.  Regular system logging and auditing should be conducted to monitor for any truststore changes.  Additionally, a file hash fingerprint should be recorded before and after any truststore changes to audit for any unrecorded changes to the file.


### Use Case 2

### Use Case 3

### Use Case 4

### Use Case 5

### Reflection on Security Requirements

## OSS Project Documention Review


## Overall Planning and Reflection
