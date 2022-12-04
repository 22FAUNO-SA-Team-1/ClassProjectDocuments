# Code Analysis Report
Team OneWon
## Introduction

## Code Review 

### Automated Scan Strategy
Our Automated scan strategy, was decided in part to what we learned in class and what Keycloaks main repository had pre-made. We made minor modifications to the CodeQL yaml file from class and ran it. While doing so, we noted that Keycloak had some other code scanning, that we utilized. This included a Trivy scanner, and their own CodeQL Java scanner. We tried to implement SonarCloud but recieved an [error](https://cwiki.apache.org/confluence/display/MAVEN/MojoFailureException).
## Selected CWEs

### [CWE-73](https://cwe.mitre.org/data/definitions/73.html): External Control of File Name or Path

Files Analyzed:<br/>
    [FolderTheme.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/theme/FolderTheme.java)<br/>
    [FolderThemeProvider.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/theme/FolderThemeProvider.java)<br/>
    [JavaKeystoreKeyProvider.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/keys/JavaKeystoreKeyProvider.java)<br/>
    [GzipResourceEncodingProvider.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/encoding/GzipResourceEncodingProvider.java)<br/>
    [BlacklistPasswordPolicyProvider.java](https://github.com/keycloak/keycloak/blob/main/server-spi-private/src/main/java/org/keycloak/policy/BlacklistPasswordPolicyProvider.java)<br/>
Automated Scan Issues: No related automated scan issues encountered.<br/>
Code Review Summary:<br/>
    Each entry for this CWE relies on a PATH variable which can result in information leakage if not santized.  Characters such as ".." or directory separaters "/" should not be allowed for entry.  Keycloak's data validation and sanitation techniques must be relied upon again to protect against weaknesses like these.



### [CWE-90](https://cwe.mitre.org/data/definitions/90.html): Improper Neutralization of Special Elements used in an LDAP Query ('LDAP Injection')

Files Analyzed:<br/>
    [LdapMapEscapeStrategyTest.java](https://github.com/keycloak/keycloak/blob/main/model/map-ldap/src/test/java/org/keycloak/models/map/storage/ldap/store/LdapMapEscapeStrategyTest.java)<br/>
Automated Scan Issues: No related automated scan issues encountered.<br/>
Code Review Summary:<br/>
    This code is flagged nine times within the LdapMapEscapeStrategyTest.java file with each entry using a LDAP query.  The risk originates with a concatenated search filter which is provided by the user.  As CodeQL states:  If an LDAP query is built using string concatenation, and the components of the concatenation include user input, a user is likely to be able to run malicious LDAP queries.  The risk can be mitigated using Keycloak's input validation tools.



### [CWE-807](https://cwe.mitre.org/data/definitions/807.html): Reliance on Untrusted Inputs in a Security Decision

Files Analyzed:<br/>
	[DefaultClientSessionContext.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/services/util/DefaultClientSessionContext.java)<br/>
Automated Scan Issues: No related automated scan issues encountered.<br/>
Code Review Summary:<br/>
	This code is flagged twice within the DefaultClientSessionContext.java, and all instances are querying to determine user scope based on a user's role within a realm.  A custom data type of "ClientScopeModel" holds the entitlement after a series of get() commands where no direct user input is accepted.  As no direct user input is accepted, the risk is modification of user data within the Keycloak realm.


    
### [CWE-601](https://cwe.mitre.org/data/definitions/601.html): URL Redirection to Untrusted Site ('Open Redirect')

Files Analyzed:<br/>
	[WelcomeResource.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/services/resources/WelcomeResource.java)<br/>
    [RealmsResource.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/services/resources/RealmsResource.java)<br/>
    [AbstractOAuth2IdentityProvider.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/broker/oidc/AbstractOAuth2IdentityProvider.java)<br/>
    [QuarkusWelcomeResource.java](https://github.com/keycloak/keycloak/blob/main/quarkus/runtime/src/main/java/org/keycloak/quarkus/runtime/services/resources/QuarkusWelcomeResource.java)<br/>
    [KeycloakAuthenticationSuccessHandler.java](https://github.com/keycloak/keycloak/blob/main/adapters/oidc/spring-security/src/main/java/org/keycloak/adapters/springsecurity/authentication/KeycloakAuthenticationSuccessHandler.java)<br/>
    [KeycloakAuthenticationEntryPoint.java](https://github.com/keycloak/keycloak/blob/main/adapters/oidc/spring-security/src/main/java/org/keycloak/adapters/springsecurity/authentication/KeycloakAuthenticationEntryPoint.java)<br/>
    
Automated Scan Issues: No related automated scan issues encountered.<br/>
Code Review Summary:<br/>
	TODO



### [CWE-79](https://cwe.mitre.org/data/definitions/79.html): Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')

Files Analyzed:<br/>
	[WelcomeResource.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/services/resources/WelcomeResource.java)<br/>
    [ThemeResource.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/services/resources/ThemeResource.java)<br/>
    [LocationActionsService.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/services/resources/LoginActionsService.java)<br/>
    [SamlProtocol.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/protocol/saml/SamlProtocol.java)<br/>
    [QuarkusWelcomeResource.java](https://github.com/keycloak/keycloak/blob/main/quarkus/runtime/src/main/java/org/keycloak/quarkus/runtime/services/resources/QuarkusWelcomeResource.java)<br/>
    [SamlUtil.java](https://github.com/keycloak/keycloak/blob/main/adapters/saml/core/src/main/java/org/keycloak/adapters/saml/SamlUtil.java)<br/>
    [AuthenticatedActionsHandler.java](https://github.com/keycloak/keycloak/blob/main/adapters/oidc/adapter-core/src/main/java/org/keycloak/adapters/AuthenticatedActionsHandler.java)<br/>
Automated Scan Issues: No related automated scan issues encountered.<br/>
Code Review Summary:<br/>
	TODO

## OSS Contributions

## Reflection/Collaboration


## Misc
[Back to Table of Contents](./README.md)

[Project Board for this section](https://github.com/orgs/22FAUNO-SA-Team-1/projects/6)
