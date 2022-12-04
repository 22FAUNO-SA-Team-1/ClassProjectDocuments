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
	This CWE calls out the risks involved with adding user input into a URL redirect without validating that input, potentially facilitating phishing attacks. In all six instances of CWE-601, the flagged line of code was either capturing session information, or location information from the user's cookies. At no point does the code truly reference direct user input into the construction of the URL redirect. Additionally, should a Keycloak administrator ever need it, the Keycloak admin console includes the capability to set URL redirect rules within the application itself. 



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
	There were ten CVEs that referenced CWE-79. The concern being taking user input as an HTTP request parameter. Similar to CWE-601, six of these CVEs were specific to obtaining current user session information. Two others flagged lines of code establishing an HTTP connection, and the final two lines of code were reading the contents of an HTTP response. None of the code references direct user input being incorporated into HTTP requests. One risk mitigation for cross-site scripting is that Keycloak uses a REST API with calls that require bearer token authentication.



### [CWE-918](https://cwe.mitre.org/data/definitions/918.html): Server-side Request Forgery

Files Analyzed:<br/>
    [FolderTheme.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/theme/FolderTheme.java)<br/>
    [QuarkusWelcomResource.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/quarkus/runtime/services/resources/QuarkusWelcomeResource.java)<br/>
    [ThemeResource.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/services/resources/ThemeResource.java)<br/>
    [WelcomeResource.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/services/resources/WelcomeResource.java)<br/>
Automated Scan Issues: Scan told of a user-provided value being the cause of the vulnerability. No user-provided values were found within the code.<br/>
Code Review Summary:<br/>
	Server-side request forgery had eight hits in the code scanning report. The main reason why these hits were made was due to a "user-provided value". In a server-side request forgery attack, the server may be tricked into interacting with a malicious external server. Upon further review of the code, there were no instances of any user-provided values. The reason for looking into this CWE is due to the heavy use Keycloak utilizes servers and "realms". 

### [CWE-614](https://cwe.mitre.org/data/definitions/614.html): Failure to Use Secure Cookies

Files Analyzed:<br/>
    [WrappedHttpServletResponse.java](https://github.com/keycloak/keycloak/blob/main/adapters/oidc/spring-security/src/main/java/org/keycloak/adapters/springsecurity/facade/WrappedHttpServletResponse.java)<br/>
Automated Scan Issues: No related automated scan issues encountered.<br/>
Code Review Summary:<br/>
	This CWE was flagged five times throughout the code. The reason for this item being flagged is due to the 'secure' flag not being set on a cookie. This can be harmful as the cookie can be easily intercepted. Upon further review, the secure flag is set three lines above. The scanner did not pick up on this as the 'secure' parameter was not set on the actual method flagged, but rather in a separate method. The reason for looking into this CWE is due to how cookies are needed to verify users, and failure to have secure cookies could allow an attacker to pass as an administrator. 

### [CWE-327](https://cwe.mitre.org/data/definitions/327.html): Use of a Broken or Risky Cryptographic Algorithm

Files Analyzed:<br/>
    [FIPS1402Provider.java](https://github.com/keycloak/keycloak/blob/main/crypto/fips1402/src/main/java/org/keycloak/crypto/fips/FIPS1402Provider.java)<br/>
Automated Scan Issues: No related automated scan issues encountered.<br/>
Code Review Summary:<br/>
	This CWE appeared three times throughout the code. The code was flagged because "AES/CVC/PKCS7Padding" is a weak cryptographic algorithm. This raises concerns and should be looked at further in detail as Keycloak is a manager for applications and good cryptographic algorithms are needed. 

### [CWE-113](https://cwe.mitre.org/data/definitions/113.html): HTTP Response Splitting

Files Analyzed:<br/>
    [CatalinaHttpFacade.java](https://github.com/keycloak/keycloak/blob/main/adapters/spi/tomcat-adapter-spi/src/main/java/org/keycloak/adapters/tomcat/CatalinaHttpFacade.java)<br/>
    [JaxrsHttpFacade.java](https://github.com/keycloak/keycloak/blob/main/adapters/oidc/jaxrs-oauth-client/src/main/java/org/keycloak/jaxrs/JaxrsHttpFacade.java)<br/>
    [WrappedHttpServletRequest.java](https://github.com/keycloak/keycloak/blob/main/adapters/oidc/spring-security/src/main/java/org/keycloak/adapters/springsecurity/facade/WrappedHttpServletRequest.java)<br/>
    [ServletHttpFacade.java](https://github.com/keycloak/keycloak/blob/main/adapters/spi/servlet-adapter-spi/src/main/java/org/keycloak/adapters/servlet/ServletHttpFacade.java)<br/>
Automated Scan Issues: No related automated scan issues encountered.<br/>
Code Review Summary:<br/>
	This CWE was flagged seven times. The reason why this weakness was flagged in the code was due to a "user-provided" value. The weakness involves writing a user input to a HTTP header, which can lead to HTTP response splitting. Upon further review of the code, no user-provided value was found; however, that does not mean that there is none. The code is shown to append a query string. Due to the complexity of the code and time restraints, no further evidence of an "user-provided" value was found. The reason why this CWE was looked into is due to the heavy usage Keycloak makes to API and HTTP requests. A vulnerability there would not be good. 


## OSS Contributions
As laid out in the summary most CWE's flagged by automated scanning and reviewed by the team turned out to be false positives, however there were some notable exceptions that warrent additional investigation. After implementing automated scanning, Keycloak's vulnerability disclosure policy was re-reviewed. As part of the policy we had to notify the Keycloak Security group via email and made the alerts private, we did this by implementing a Beta feature "Private vulnerability reporting" in our fork (because we couldn't make the fork private). There was some non-CWE's that our scanning flagged primarily due to depreciated packages and libraries. We are still waiting on Keycloaks response about how to proceed. According to the disclosure policy, they will create a private fork to fix and add us if warrented (i.e. at their discretion).
## Reflection/Collaboration


## Misc
[Back to Table of Contents](./README.md)

[Code Scanning Alerts](https://github.com/22FAUNO-SA-Team-1/keycloak/security/code-scanning) (available to our organization and keycloak maintainers)

[Project Board for this section](https://github.com/orgs/22FAUNO-SA-Team-1/projects/6)
