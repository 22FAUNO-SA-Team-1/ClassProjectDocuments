# Assurance Cases Report
Team OneWon
## Essential Interactions, Diagrams, and Alignment Analysis

### Assurance Case 1: Keycloak Prevents Man-In-The-Middle Attacks

![Keycloak Assurance Diagram]( ./Diagrams/jk_assurance_case1.png "Keycloak Assurance Diagram")

Keycloak allows for integration between multiple platforms which can require credentials to be passed between endpoints.  Credential theft through network eavesdropping is a risk which can be eliminated using a properly configured truststore.  The truststore allows for encrypted SSL connections in order to protect user credentials.

E1:  Regular auditing of system logs are needed to protect the integrity of the truststore.  Should the truststore become compromised, the security of any SSL connection cannot be reliable.  A fraudulent or compromised certificate can allow for a rogue entity to impersonate an endpoint and gather user credentials.

E2:  A poor certificate using weak encryption can allow for easier credential interception.  SSL certicates should never be self-signed and always acquired from a trusted Certificate Authority using industry-standard RSA 2048-bit or higher key length.

### Assurance Case 2:  Keycloak ensures all users are authenticated before being allowed access to applications or other resources

![Keycloak Assurance Diagram]( ./Diagrams/cs_assurance_claim.png "Keycloak Assurance Diagram")

Keycloak has many features, among which is the ability for an organization to utilize keycloak and offload much of the overhead and resources required for properly secure user authentication.  If, however, an organization is going to entrust authentication to a 3rd party like Keycloak, there must exist sufficient assurance that their applications and resources will be safe from unauthenticated user interactions.  In direct response to this, Keycloak has a plethora of settings which, when properly utilized, will offer that assurance to an organization.

E1:  2FA Policy Review Report - An organization utilizing Keycloak must regularly conduct a review of the Two-Factor Authentication Policy in order to ensure that all users, existing and new, have the proper settings in the Keycloak dashboard in order to enforce the usage of 2FA for their accounts.  This will help mitigate phishing attempts and minimize damage caused by user credentials being gained externally to the system.

E2:  Passwords are stored quite safely in a hashed format utilizing a strong hashing algorithm (PBKDF2) with a default of 27,500 hashing iterations per hashed password.  Admins can tweak these settings if needed as this is can be resource intensive when adding many users at once but the default settings are best kept for security.  A combination of reviewing the source code and reviewing the settings in the Keycloak dashboard will ensure passwords are stored in a safe hashed format.

E3:  Regular reviews of Password Policy are strongly recommended in order to ensure users are using safe and strong passwords for their accounts.

E4:  Regular reviews are recommended of both the source code and the Keycloak dashboard settings in order to ensure that brute force detection is always in place and enforced at all times.

Conclusion and Gaps:  After thorough review of Keycloak documentation, there were **NO** gaps found in regards to user authentication.  Each of the pathways of attack has an answer provided by Keycloak accessed by an Aministrator through the dashboard.  Passwords are stored in hashed format using a strong hashing algorithm with many hashing iterations per hashed password.  Proper password use can be hard-enforced by Keycloak rather than recommended by administrators or organization management.  User passwords can be required to have a set minimum of characters (recommended minimum is 8, and a minimum greater than 14 is overkill) and can require special characters, numbers, capitals and lowercase characters.  Keycloak even stores a history of old passwords for each user and can prevent the reuse of those passwords within a determined time period.  Even brute force detection and 2FA have settings in the Keycloak dashboard that can mitigate and prevent unauthenticated access.

### Assurance Case 3: Keycloak Prevents Unauthorized Access to Clients

![Keycloak Assurance Diagram]( ./Diagrams/AssuranceCase3.png "Keycloak Assurance Diagram")

### Assurance Case 4:

### Assurance Case 5:KeyCloak is sufficiently Available
![Keycloak Assurance Diagram]( ./Diagrams/AssuranceCase5.drawio.png "Keycloak Assurance Diagram")

## Reflection
