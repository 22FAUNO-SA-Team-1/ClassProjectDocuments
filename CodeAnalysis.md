# Code Analysis Report
Team OneWon
## Introduction
In the process of evaluating and ensuring proper software assurance, we began with planning and analyzing hypothetical scenarios in which attackers might attempt to misuse the software and expanded those scenarios to determine what is required to ensure the software is secure and works as advertised.  With potential threats addressed and secure planning complete, we now must turn our attention to reviewing the source code to either determine whether the code offers proper assurance as is, or what must be changed in order to meet assurance demands.  We will accomplish this through a combination of automated code scanning tools, manual code review, and cross-referencing both methods with our previous findings of potential threats and weaknesses.

## Code Review 
Our code review strategy process will proceed as follows:

1.	 Begin by deciding which code scanning tools we should use (automated scanning strategy covered in the following subsection) and using those tools to scan the Keycloak source code

2.	Each member covering the CWEs will then use a combination of the following strategies:

    a.	Refer to previous project findings in order to determine critical CWEs

    b.	Browse through Keycloak directories targeting folders dealing with critical interactions with potential threats or weaknesses

    c.	Browse through the list of CVE alerts found by automated scanning tools and cross-referencing them with appropriate CWEs

3.	Once we narrowed the list down to 5-10 CWEs with appropriate findings in the source code, any issues encountered are documented, and a summary of the code review findings is documented as well



### Automated Scan Strategy
Our Automated scan strategy, was decided in part to what we learned in class and what Keycloak's main repository had pre-made. We made minor modifications to the CodeQL yaml file from class and ran it. While doing so, we noted that Keycloak had some other code scanning, that we utilized. This included a Trivy scanner, and their own CodeQL Java scanner. We tried to implement SonarCloud but received an [error](https://cwiki.apache.org/confluence/display/MAVEN/MojoFailureException).

## Selected CWEs

### [CWE-73](https://cwe.mitre.org/data/definitions/73.html): External Control of File Name or Path

* **Files Analyzed:**
    * [FolderTheme.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/theme/FolderTheme.java) 
    * [FolderThemeProvider.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/theme/FolderThemeProvider.java) 
    * [JavaKeystoreKeyProvider.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/keys/JavaKeystoreKeyProvider.java)
    * [GzipResourceEncodingProvider.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/encoding/GzipResourceEncodingProvider.java) 
    * [BlacklistPasswordPolicyProvider.java](https://github.com/keycloak/keycloak/blob/main/server-spi-private/src/main/java/org/keycloak/policy/BlacklistPasswordPolicyProvider.java)
* **Automated Scan Issues:** No related automated scan issues encountered.
* **Code Review Summary:**
    Each entry for this CWE relies on a PATH variable which can result in information leakage if not sanitized.  Characters such as ".." or directory separators "/" should not be allowed for entry.  Keycloak's data validation and sanitation techniques must be relied upon again to protect against weaknesses like these.



### [CWE-90](https://cwe.mitre.org/data/definitions/90.html): Improper Neutralization of Special Elements used in an LDAP Query ('LDAP Injection')

* **Files Analyzed:**
    * [LdapMapEscapeStrategyTest.java](https://github.com/keycloak/keycloak/blob/main/model/map-ldap/src/test/java/org/keycloak/models/map/storage/ldap/store/LdapMapEscapeStrategyTest.java) 
* **Automated Scan Issues:** No related automated scan issues encountered. 
* **Code Review Summary:**
    CWE-90 is flagged nine times within the LdapMapEscapeStrategyTest.java file with each entry using a LDAP query.  The risk originates with a concatenated search filter which is provided by the user.  As CodeQL states:  If an LDAP query is built using string concatenation, and the components of the concatenation include user input, a user is likely to be able to run malicious LDAP queries.  The risk can be mitigated using Keycloak's input validation tools.



### [CWE-807](https://cwe.mitre.org/data/definitions/807.html): Reliance on Untrusted Inputs in a Security Decision

* **Files Analyzed:**
	* [DefaultClientSessionContext.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/services/util/DefaultClientSessionContext.java) 
* **Automated Scan Issues:** No related automated scan issues encountered. 
* **Code Review Summary:**
	This CWE is flagged twice within the DefaultClientSessionContext.java, and all instances are querying to determine user scope based on a user's role within a realm.  A custom data type of "ClientScopeModel" holds the entitlement after a series of get() commands where no direct user input is accepted.  As no direct user input is accepted, the risk is modification of user data within the Keycloak realm.


    
### [CWE-601](https://cwe.mitre.org/data/definitions/601.html): URL Redirection to Untrusted Site ('Open Redirect')

* **Files Analyzed:**
	* [WelcomeResource.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/services/resources/WelcomeResource.java) 
    * [RealmsResource.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/services/resources/RealmsResource.java) 
    * [AbstractOAuth2IdentityProvider.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/broker/oidc/AbstractOAuth2IdentityProvider.java) 
    * [QuarkusWelcomeResource.java](https://github.com/keycloak/keycloak/blob/main/quarkus/runtime/src/main/java/org/keycloak/quarkus/runtime/services/resources/QuarkusWelcomeResource.java) 
    * [KeycloakAuthenticationSuccessHandler.java](https://github.com/keycloak/keycloak/blob/main/adapters/oidc/spring-security/src/main/java/org/keycloak/adapters/springsecurity/authentication/KeycloakAuthenticationSuccessHandler.java) 
    * [KeycloakAuthenticationEntryPoint.java](https://github.com/keycloak/keycloak/blob/main/adapters/oidc/spring-security/src/main/java/org/keycloak/adapters/springsecurity/authentication/KeycloakAuthenticationEntryPoint.java)   
* **Automated Scan Issues:** No related automated scan issues encountered. 
* **Code Review Summary:**
	This CWE calls out the risks involved with adding user input into a URL redirect without validating that input, potentially facilitating phishing attacks. In all six instances of CWE-601, the flagged line of code was either capturing session information, or location information from the user's cookies. At no point does the code truly reference direct user input into the construction of the URL redirect. Additionally, should a Keycloak administrator ever need it, the Keycloak admin console includes the capability to set URL redirect rules within the application itself. 



### [CWE-79](https://cwe.mitre.org/data/definitions/79.html): Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')

* **Files Analyzed:**
	* [WelcomeResource.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/services/resources/WelcomeResource.java) 
    * [ThemeResource.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/services/resources/ThemeResource.java) 
    * [LocationActionsService.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/services/resources/LoginActionsService.java) 
    * [SamlProtocol.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/protocol/saml/SamlProtocol.java) 
    * [QuarkusWelcomeResource.java](https://github.com/keycloak/keycloak/blob/main/quarkus/runtime/src/main/java/org/keycloak/quarkus/runtime/services/resources/QuarkusWelcomeResource.java) 
    * [SamlUtil.java](https://github.com/keycloak/keycloak/blob/main/adapters/saml/core/src/main/java/org/keycloak/adapters/saml/SamlUtil.java) 
    * [AuthenticatedActionsHandler.java](https://github.com/keycloak/keycloak/blob/main/adapters/oidc/adapter-core/src/main/java/org/keycloak/adapters/AuthenticatedActionsHandler.java) 
* **Automated Scan Issues:** No related automated scan issues encountered. 
* **Code Review Summary:**
	There were ten CVEs that referenced CWE-79. The concern being taking user input as an HTTP request parameter. Similar to CWE-601, six of these CVEs were specific to obtaining current user session information. Two others flagged lines of code establishing an HTTP connection, and the final two lines of code were reading the contents of an HTTP response. None of the code references direct user input being incorporated into HTTP requests. One risk mitigation for cross-site scripting is that Keycloak uses a REST API with calls that require bearer token authentication.



### [CWE-918](https://cwe.mitre.org/data/definitions/918.html): Server-side Request Forgery

* **Files Analyzed:**
    * [FolderTheme.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/theme/FolderTheme.java) 
    * [QuarkusWelcomResource.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/quarkus/runtime/services/resources/QuarkusWelcomeResource.java) 
    * [ThemeResource.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/services/resources/ThemeResource.java) 
    * [WelcomeResource.java](https://github.com/keycloak/keycloak/blob/main/services/src/main/java/org/keycloak/services/resources/WelcomeResource.java) 
* **Automated Scan Issues:** Scan told of a user-provided value being the cause of the vulnerability. No user-provided values were found within the code. 
* **Code Review Summary:**
	Server-side request forgery had eight hits in the code scanning report. The main reason why these hits were made was due to a "user-provided value". In a server-side request forgery attack, the server may be tricked into interacting with a malicious external server. Upon further review of the code, there were no instances of any user-provided values. The reason for looking into this CWE is due to the heavy use Keycloak utilizes servers and "realms". 



### [CWE-614](https://cwe.mitre.org/data/definitions/614.html): Failure to Use Secure Cookies

* **Files Analyzed:**
    * [WrappedHttpServletResponse.java](https://github.com/keycloak/keycloak/blob/main/adapters/oidc/spring-security/src/main/java/org/keycloak/adapters/springsecurity/facade/WrappedHttpServletResponse.java) 
* **Automated Scan Issues:** No related automated scan issues encountered. 
* **Code Review Summary:**
	This CWE was flagged five times throughout the code. The reason for this item being flagged is due to the 'secure' flag not being set on a cookie. This can be harmful as the cookie can be easily intercepted. Upon further review, the secure flag is set three lines above. The scanner did not pick up on this as the 'secure' parameter was not set on the actual method flagged, but rather in a separate method. The reason for looking into this CWE is due to how cookies are needed to verify users, and failure to have secure cookies could allow an attacker to pass as an administrator. 



### [CWE-327](https://cwe.mitre.org/data/definitions/327.html): Use of a Broken or Risky Cryptographic Algorithm

* **Files Analyzed:**
    * [FIPS1402Provider.java](https://github.com/keycloak/keycloak/blob/main/crypto/fips1402/src/main/java/org/keycloak/crypto/fips/FIPS1402Provider.java)
* **Automated Scan Issues:** No related automated scan issues encountered.
* **Code Review Summary:** This CWE appeared three times throughout the code. The code was flagged because "AES/CVC/PKCS7Padding" is a weak cryptographic algorithm. This raises concerns and should be looked at further in detail as Keycloak is a manager for applications and good cryptographic algorithms are needed. 



### [CWE-113](https://cwe.mitre.org/data/definitions/113.html): HTTP Response Splitting

* **Files Analyzed:**
    * [CatalinaHttpFacade.java](https://github.com/keycloak/keycloak/blob/main/adapters/spi/tomcat-adapter-spi/src/main/java/org/keycloak/adapters/tomcat/CatalinaHttpFacade.java) 
    * [JaxrsHttpFacade.java](https://github.com/keycloak/keycloak/blob/main/adapters/oidc/jaxrs-oauth-client/src/main/java/org/keycloak/jaxrs/JaxrsHttpFacade.java) 
    * [WrappedHttpServletRequest.java](https://github.com/keycloak/keycloak/blob/main/adapters/oidc/spring-security/src/main/java/org/keycloak/adapters/springsecurity/facade/WrappedHttpServletRequest.java) 
    * [ServletHttpFacade.java](https://github.com/keycloak/keycloak/blob/main/adapters/spi/servlet-adapter-spi/src/main/java/org/keycloak/adapters/servlet/ServletHttpFacade.java) 
* **Automated Scan Issues:** No related automated scan issues encountered.
* **Code Review Summary:**
	This CWE was flagged seven times. The reason why this weakness was flagged in the code was due to a "user-provided" value. The weakness involves writing a user input to a HTTP header, which can lead to HTTP response splitting. Upon further review of the code, no user-provided value was found; however, that does not mean that there is none. The code is shown to append a query string. Due to the complexity of the code and time restraints, no further evidence of an "user-provided" value was found. The reason why this CWE was looked into is due to the heavy usage Keycloak makes to API and HTTP requests. A vulnerability there would not be good.



### **CWE Findings Summary**

Our findings from automated scanning and subsequent manual code review are that none of the CVEs we evaluated raised any major concerns.  Most of the CWEs encountered in this review have corresponding mitigations built into Keycloak that have been covered in other sections of our security analysis. We do, however, believe that some of the CWE's concerning HTTP and encryption may warrant additional investigation by someone with more than a superficial understanding of Keycloak's codebase.



## OSS Contributions
As laid out in the summary most CWE's flagged by automated scanning and reviewed by the team turned out to be false positives, however there were some notable exceptions that warrant additional investigation. After implementing automated scanning, Keycloak's vulnerability disclosure policy was re-reviewed. As part of the policy we had to notify the Keycloak Security group via email and made the alerts private, we did this by implementing a Beta feature "Private vulnerability reporting" in our fork (because we couldn't make the fork private). There was some non-CWE's that our scanning flagged primarily due to depreciated packages and libraries. We are still waiting on Keycloak's response about how to proceed. According to the disclosure policy, they will create a private fork to fix and add us if warranted (i.e. at their discretion).



## Reflection/Collaboration
This portion of the project was the first time in the software assurance review process where diving into the actual source code is required.  This, we believe, can be a more labor and time intensive process, which leads to the first hurdle that we met and that I imagine many groups may also encounter, a good portion of the time allotted fell around a holiday which can make it difficult to focus at times.  We countered this “hurdle” of sorts by keeping group discussions going on discord and selecting and running our initial automated scanning tools during the holiday week/weekend.  The next speed bump we encountered was a slight mix-up in procedure and definition.  We initially conflated CWEs and CVEs together and ran the code scanning tools to review the results before narrowing down what we were looking for based on previous project submissions.  After some encouragement on some of our selections during the professor check-in meeting, we were able to buckle down and proceed rapidly from that point.  In regards to group contributions, John, Zach, and Ethen primarily covered selecting and expanding on the CWEs themselves while Trenton continued to serve as the github administrator and greatly aided in keeping the group on track for this submission.  Trenton also discovered a vulnerability disclosure issue with the OSSP (covered in the OSS contribution section).  Chris covered the same topics he has throughout the project, primarily organizing and managing meeting agendas and the meetings themselves, providing as much feedback through discord as possible, and covering the planning and group organizational sections.


## Misc
[Back to Table of Contents](./README.md)

[Code Scanning Alerts](https://github.com/22FAUNO-SA-Team-1/keycloak/security/code-scanning) (available to our organization and Keycloak maintainers)

[Project Board for this section](https://github.com/orgs/22FAUNO-SA-Team-1/projects/6)
