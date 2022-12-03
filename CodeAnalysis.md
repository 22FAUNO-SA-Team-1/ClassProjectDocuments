### Introduction

### Code Review 

### Selected CWEs

#### [CWE-73](https://cwe.mitre.org/data/definitions/73.html): External Control of File Name or Path

Files Analyzed:<br/>
    [FolderTheme.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/theme/FolderTheme.java)<br/>
    [FolderThemeProvider.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/theme/FolderThemeProvider.java)<br/>
    [JavaKeystoreKeyProvider.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/keys/JavaKeystoreKeyProvider.java)<br/>
    [GzipResourceEncodingProvider.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/encoding/GzipResourceEncodingProvider.java)<br/>
    [BlacklistPasswordPolicyProvider.java](https://github.com/keycloak/keycloak/blob/main/server-spi-private/src/main/java/org/keycloak/policy/BlacklistPasswordPolicyProvider.java)<br/>
Automated Scan Issues: No related automated scan issues encountered.
Code Review Summary:
    Each entry for this CWE relies on a PATH variable which can result in information leakage if not santized.  Characters such as ".." or directory separaters "/" should not be allowed for entry.  Keycloak's data validation and sanitation techniques must be relied upon again to protect against weaknesses like these.


#### [CWE-90](https://cwe.mitre.org/data/definitions/90.html): Improper Neutralization of Special Elements used in an LDAP Query ('LDAP Injection')

Files Analyzed:
    [LdapMapEscapeStrategyTest.java](https://github.com/keycloak/keycloak/blob/main/model/map-ldap/src/test/java/org/keycloak/models/map/storage/ldap/store/LdapMapEscapeStrategyTest.java)
Automated Scan Issues: No related automated scan issues encountered.
Code Review Summary:
    This code is flagged nine times within the LdapMapEscapeStrategyTest.java file with each entry using a LDAP query.  The risk originates with a concatenated search filter which is provided by the user.  As CodeQL states:  If an LDAP query is built using string concatenation, and the components of the concatenation include user input, a user is likely to be able to run malicious LDAP queries.  The risk can be mitigated using Keycloak's input validation tools.



#### [CWE-807](https://cwe.mitre.org/data/definitions/807.html): Reliance on Untrusted Inputs in a Security Decision

Files Analyzed:
	[DefaultClientSessionContext.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/services/util/DefaultClientSessionContext.java)
Automated Scan Issues: No related automated scan issues encountered.
Code Review Summary:
	This code is flagged twice within the DefaultClientSessionContext.java, and all instances are querying to determine user scope based on a user's role within a realm.  A custom data type of "ClientScopeModel" holds the entitlement after a series of get() commands where no direct user input is accepted.  As no direct user input is accepted, the risk is modification of user data within the Keycloak realm.

### OSS Contributions

### Reflection/Collaboration


## Misc
[Back to Table of Contents](./README.md)

[Project Board for this section](https://github.com/orgs/22FAUNO-SA-Team-1/projects/6)
