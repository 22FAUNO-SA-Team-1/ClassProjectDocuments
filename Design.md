



### 1. Data Flow Diagram and Threat Modeling

1. [Keycloak DFD](https://github.com/22FAUNO-SA-Team-1/ClassProjectDocuments/blob/70ef72df39501e67f1419819c7d63b5194a6ec71/Diagrams/Part%204/DFD%20Model%20Screenshot.PNG)
2. [Threat Modeling Report](https://htmlpreview.github.io/?https://github.com/22FAUNO-SA-Team-1/ClassProjectDocuments/blob/39a5ba295d804d09f1305d362b79e30f07344579/Diagrams/Part%204/Keycloak%20Threat%20Report.htm)

### 2. Introduction

The scenario we have based our data flow diagram on, and by extension generated and addressed the various threats existing in said scenario, revolves around what we believe is the primary purpose and most common use of Keycloak, namely authenticating a valid user and providing secure access to an application.  In this scenario an external interactor representing a user wishing to access protected assets provided by a web application must be properly authenticated by Keycloak before gaining said access.  This user will be prompted by Keycloak to enter their credentials as a data flow source into the Keycloak process.  Keycloak will then, based on the Keycloak server settings, determine how that user should be authenticated, and will send an authentication request to the internal truststore data source, or in the case where the application relies on an external LDAP server for authentication, that authentication request is sent out to an external interactor representing the LDAP server as that server is outside the control of the Keycloak codebase.  In the case of the internal truststore, the truststore data source sends back the required data for the Keycloak process to make an authentication decision, whereas when using the LDAP server, the external interactor LDAP server sends the authentication response back to the Keycloak process.  If the external interactor user is authenticated properly, their input data will then be forwarded to the web application external interactor and any UI or otherwise requested data while using the application will be sent back to the user through the Keycloak process.  Threat boundaries exist between the Keycloak Process and any external interactor where the codebase does not have direct control.


### 3. Individual Threat Review
<!--Start of Chris' Threats -->
- *Threat ID*: 1.
    - Threat Name:  Potential Excessive Resource Consumption for Keycloak or Truststore
    - Threat Category:  Denial of Service
    - Justification:  If Keycloak process or the Truststore data store get out of control with resource consumption, the machine (virtual or physical) where they run will be excessively bogged down
    - Existing Mitigations:  Keycloak proper user role control and the ability to allow/deny certain data flows can prevent excessive resource consumption on the application to which Keycloak grants users access, there isn't anything that directly prevents the Keycloak server itself from excess resource consumption
    - Notable Gap:  There have been some known issues with memory leaks Keycloak servers in the past and its unsure whether these have been addressed at this point or not

- *Threat ID*: 2.
    - Threat Name:  Spoofing of Destination Data Store Truststore
    - Threat Category:  Spoofing
    - Justification:  Not applicable in our scenario as the truststore data store is an internal data store running on the keycloak server and an external actor wouldn't have access 
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 3.
    - Threat Name:  Spoofing of Source Data Store Truststore
    - Threat Category:  Spoofing
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 4.
    - Threat Name:  Weak Access Control for a Resource
    - Threat Category:  Information Disclosure
    - Justification:  If access to resources isn't properly controlled, an attacker can access/read/manipulate resources
    - Existing Mitigations:  Keycloak doesn't facilitate users accessing truststores data stores.  To be provided any control over the truststore and data flowing to/from it, a user must be authenticated to a master or administrator realm on the Keycloak server itself.  Non administrative role users have authentication requests handled directly by the Keycloak process itself.
    - Notable Gap: None

- *Threat ID*: 5.
    - Threat Name:  Data Flow LDAP Authentication Request Is Potentially Interrupted
    - Threat Category:  Denial of Service
     - Justification:  Keycloak cannot authenticate users if LDAP service is interrupted.
	- Existing Mitigations:  None
    - Notable Gap:  Difficult to mitigate.  Could be mitigated with a distributed network connection between Keycloak and LDAP server.

- *Threat ID*: 6.
    - Threat Name:  External Entity LDAP Server Potentially Denies Receiving Data
    - Threat Category:  Repudiation
    - Justification:  If LDAP server denies receiving authentication data then a user would fail authentication even when they are a valid user
    - Existing Mitigations:  Keycloak allows logging of events like denied data transmission with relevant data such as time/date of the event and summary of the data that failed or was denied to flow.
    - Notable Gap:  Logging is the only answer Keycloak has to this threat, it doesn't directly address the issue when it occurs but it does allow fixing the issue after the fact as needed.

- *Threat ID*: 7.
    - Threat Name:  Spoofing of the LDAP Server External Destination Entity
    - Threat Category:  Spoofing
    - Justification:  An attacker could spoof an LDAP server and hijack data or requests meant to be sent to the LDAP server
    - Existing Mitigations:  Spoofing the LDAP Server is prevented by authentication via the truststore.  A truststore for trusted LDAP entities is stored on the Keycloak server and ensuring the LDAP server is authentic is required before authentication requests are sent to an LDAP server.
    - Notable Gap: None

- *Threat ID*: 8.
    - Threat Name:  Cross Site Request Forgery
    - Threat Category:  Elevation of Privilege
    - Justification: Data parsing is utilized to prevent CSRF or XSRF
    - Existing Mitigations: All input data is sanitized before being processed
    - Notable Gap: None

- *Threat ID*: 9.
    - Threat Name:  Elevation by Changing the Execution Flow in Keycloak
    - Threat Category:  Elevation of Privilege
    - Justification: All data is sanitized
    - Existing Mitigations: Keycloak sanitizes all data to ensure no execution flows can be altered
    - Notable Gap: None

<!--Start of John's Threats -->
- *Threat ID*: 10.
    - Threat Name:  Keycloak May be Subject to Elevation of Privilege Using Remote Code Execution
    - Threat Category:  Elevation Of Privilege
    - Justification:  Remote Code Execution is a common attack vector so care must be taken when accepting input from remote systems.
    - Existing Mitigations:  Keycloak features attribute validation when configuring user profiles within a realm.
    - Notable Gap:  None

- *Threat ID*: 11.
    - Threat Name:  Data Flow LDAP Authentication Response Is Potentially Interrupted
    - Threat Category:  Denial Of Service
    - Justification:  Keycloak cannot authenticate users if LDAP service is interrupted.
	- Existing Mitigations:  None
    - Notable Gap:  Difficult to mitigate.  Could be mitigated with a distributed network connection between Keycloak and LDAP server.

- *Threat ID*: 12.
    - Threat Name:  Potential Process Crash or Stop for Keycloak
    - Threat Category:  Denial Of Service
    - Justification:  Application availability is critical for system use.
    - Existing Mitigations:  Keycloak can be configured using clustering to improve performance and availability.
    - Notable Gap:  None

- *Threat ID*: 13.
    - Threat Name:  Data Flow Sniffing
    - Threat Category:  Information Disclosure
    - Justification:  Credential theft is a risk if transmitted in plaintext.
    - Existing Mitigations:  Mitigated by using LDAPS protocol for encrypted data transmission.
    - Notable Gap:  None

- *Threat ID*: 14.
    - Threat Name:  Potential Data Repudiation by Keycloak
    - Threat Category:  Repudiation
    - Justification:  Keycloak claims that it did not receive data from a source outside the trust boundary. Consider using logging or auditing to record the source, time, and summary of the received data.
    - Existing Mitigations:  Keycloak allows for eight levels of system logging.
    - Notable Gap:  None

- *Threat ID*: 15.
    - Threat Name:  Potential Lack of Input Validation for Keycloak
    - Threat Category:  Tampering
    - Justification:  Data integrity is crucial for trusted operation.
    - Existing Mitigations:  Mitigated using Input Validation or Sanitization, similar to Threat ID 10.
    - Notable Gap:  None

- *Threat ID*: 16.
    - Threat Name:  Spoofing the Keycloak Process
    - Threat Category:  Spoofing
    - Justification:  Keycloak may be spoofed by an attacker and this may lead to information disclosure by LDAP Server.
    - Existing Mitigations:  Mitigated via Keycloak truststore which uses certificate-based authentication.
    - Notable Gap:  None

- *Threat ID*: 17.
    - Threat Name:  Spoofing the LDAP Server External Entity
    - Threat Category:  Spoofing
    - Justification:  LDAP Server may be spoofed by an attacker and this may lead to unauthorized access to Keycloak.
    - Existing Mitigations:  Mitigated via Keycloak trust store which uses certificate-based authentication, similar to Threat ID 16.
    - Notable Gap:  None

- *Threat ID*: 18.
    - Threat Name:  Elevation Using Impersonation
    - Threat Category:  Elevation Of Privilege
    - Justification:  Keycloak may be able to impersonate the context of LDAP Server in order to gain additional privilege.
    - Existing Mitigations:  Configure Keycloak with read-only LDAP integration.
    - Notable Gap:  None

<!--Start of Ethen's Threats -->
- *Threat ID*: 19.
    - Threat Name: Spoofing the User External Entity
    - Threat Category: Spoofing
    - Justification: Keycloak uses standard authorization
    - Existing Mitigations: Keycloak utilizes standard authorization protocols to prevent spoofing
    - Notable Gap: None

- *Threat ID*: 20.
    - Threat Name: Elevation Using Impersonation
    - Threat Category: Elevation Of Privilege
    - Justification: Impersonation can be used to gain access to higher elevation thus login function has to validate the person
    - Existing Mitigations: Dual factor authentication can be utilized to mitigate the possibility of impersonation
    - Notable Gap: None

- *Threat ID*: 21.
    - Threat Name: Cross Site Request Forgery
    - Threat Category: Elevation Of Privilege
    - Justification: Data parsing is utilized to prevent CSRF or XSRF
    - Existing Mitigations: All input data is sanitized before being processed
    - Notable Gap: None

- *Threat ID*: 22.
    - Threat Name: Elevation by Changing the Execution Flow in Keycloak
    - Threat Category: Elevation Of Privilege
    - Justification: All data is sanitized
    - Existing Mitigations: Keycloak sanitizes all data to ensure no execution flows can be altered
    - Notable Gap: None

- *Threat ID*: 23.
    - Threat Name: Keycloak May be Subject to Elevation of Privilege Using Remote Code Execution
    - Threat Category: Elevation Of Privilege
    - Justification: Remote Code Execution is a common attack vector so care must be taken when accepting input from remote systems.
    - Existing Mitigations: Keycloak features attribute validation when configuring user profiles within a realm.
    - Notable Gap: None

- *Threat ID*: 24.
    - Threat Name: Data Flow User Input Is Potentially Interrupted
    - Threat Category: Denial Of Service
    - Justification: Keycloak can not guarantee uninterrupted data flow to web applications
    - Existing Mitigations: None
    - Notable Gap: Keycloak can not guarantee uninterrupted data flow to web applications
  
- *Threat ID*: 25.
    - Threat Name: Potential Process Crash or Stop for Keycloak
    - Threat Category: Denial Of Service
     - Justification:  Application availability is critical for system use.
    - Existing Mitigations:  Keycloak can be configured using clustering to improve performance and availability.
    - Notable Gap:  None

- *Threat ID*: 26.
    - Threat Name: Data Flow Sniffing
    - Threat Category: Information Disclosure
    - Justification: Data flow is encrypted
    - Existing Mitigations: Keycloak encrypts the User Input data flow with standard encryption policies
    - Notable Gap: None

- *Threat ID*: 27.
    - Threat Name: Potential Data Repudiation by Keycloak
    - Threat Category: Repudiation
    - Justification: Keycloak audits all actions
    - Existing Mitigations: Auditing is a critical part of Keycloak's architecture
    - Notable Gap: None

<!--Start of Zach's Threats -->
- *Threat ID*: 28.
    - Threat Name: Potential Lack of Input Validation for Keycloak (User to Keycloak)
    - Threat Category: Tampering
    - Justification: Keycloak provides administrators with an option to add custom validators as needed
    - Existing Mitigations: Keycloak includes eight validators by default: Length, integer, double, URI, pattern, email. local-date, person name-prohibited-characters, username prohibited characters, and options
    - Notable Gap: None

- *Threat ID*: 29.
    - Threat Name: Spoofing the Keycloak Process
    - Threat Category: Spoofing
    - Justification: Keycloak allows Open Redirects
    - Existing Mitigations: Written configuration recommendation
    - Notable Gap: If an administrator does not make a redirect URI sufficiently specific, or if a user is already authenticated and redirect URIs have not been configured, spoofing Keycloak is possible. 

- *Threat ID*: 30.
    - Threat Name: Data Flow User Input Is Potentially Interrupted
    - Threat Category: Denial Of Service
    - Justification: Keycloak can not guarantee uninterrupted data flow to web applications
    - Existing Mitigations: None
    - Notable Gap: Keycloak can not guarantee uninterrupted data flow to web applications

- *Threat ID*: 31.
    - Threat Name: External Entity Web Application Potentially Denies Receiving Data
    - Threat Category: Repudiation
    - Justification: Keycloak audits all actions
    - Existing Mitigations: Auditing is a critical part of Keycloak's architecture
    - Notable Gap: None

- *Threat ID*: 32.
    - Threat Name: Spoofing of the Web Application External Destination Entity
    - Threat Category: Spoofing
    - Justification: Keycloak is configurable for SSL/HTTPS
    - Existing Mitigations: The SSL trust manager ensures the web application's identity is valid and ensures the DNS domain name against the server's certificate
    - Notable Gap: None

- *Threat ID*: 33.
    - Threat Name: Spoofing the Web Application External Entity
    - Threat Category: Spoofing
    - Justification: Not applicable - Web Applications do not have permissions to exploit in Keycloak
    - Existing Mitigations: Users have permissions to access web applications.
    - Notable Gap: None 
  
- *Threat ID*: 34.
    - Threat Name: Elevation Using Impersonation
    - Threat Category: Elevation Of Privilege
    - Justification: Not applicable - Keycloak's purpose is user authentication and authorization
    - Existing Mitigations: Only an administrator can grant privileges. Privilege extends from user to application.
    - Notable Gap: None

- *Threat ID*: 35.
    - Threat Name: Spoofing the Keycloak Process
    - Threat Category: Spoofing
    - Justification: Keycloak application connections are secured by protocols
    - Existing Mitigations: Keycloak connects to client applications using a choice of either OpenID Connect or SAML protocols.
    - Notable Gap: None

- *Threat ID*: 36.
    - Threat Name: Potential Lack of Input Validation for Keycloak
    - Threat Category: Tampering
    - Justification: Keycloak provides administrators with an option to add custom validators as needed
    - Existing Mitigations:  Keycloak includes eight validators by default: Length, integer, double, URI, pattern, email. local-date, person name-prohibited-characters, username prohibited characters, and options
    - Notable Gap: None

<!--Start of Trenton's Threats -->
- *Threat ID*: 37.
    - Threat Name: Potential Data Repudiation by Keycloak
    - Threat Category: 	Repudiation
    - Justification: Keycloak audits all actions
    - Existing Mitigations: Auditing is a critical part of Keycloak's architecture
    - Notable Gap: None

- *Threat ID*: 38.
    - Threat Name: Data Flow Sniffing
    - Threat Category: Information Disclosure
    - Justification: Keycloak can not guarantee encrypted Data from web applications
    - Existing Mitigations: None
    - Notable Gap: Keycloak can not guarantee encrypted Data from web applications

- *Threat ID*: 39.
    - Threat Name: Potential Process Crash or Stop for Keycloak
    - Threat Category: 	Denial Of Service
    - Justification:  Application availability is critical for system use.
    - Existing Mitigations:  Keycloak can be configured using clustering to improve performance and availability.
    - Notable Gap:  None

- *Threat ID*: 40.
    - Threat Name: Data Flow Webapp Output Is Potentially Interrupted
    - Threat Category: Denial Of Service
    - Justification: Keycloak can not guarantee uninterrupted data flow from web applications
    - Existing Mitigations: None
    - Notable Gap: Keycloak can not guarantee uninterrupted data flow from web applications

- *Threat ID*: 41.
    - Threat Name: Keycloak May be Subject to Elevation of Privilege Using Remote Code Execution
    - Threat Category: Elevation Of Privilege
   - Justification:  Remote Code Execution is a common attack vector so care must be taken when accepting input from remote systems.
    - Existing Mitigations:  Keycloak features attribute validation
    - Notable Gap:  None

- *Threat ID*: 42.
    - Threat Name: Elevation by Changing the Execution Flow in Keycloak
    - Threat Category: Elevation Of Privilege
    - Justification: All data is sanitized
    - Existing Mitigations: Keycloak sanitizes all data to ensure no execution flows can be altered
    - Notable Gap: None

-  *Threat ID*: 43.
    -  Threat Name: Cross Site Request Forgery
    -  Threat Category: Elevation Of Privilege
    - Justification: Data parsing is utilized to prevent CSRF or XSRF
    - Existing Mitigations: All input data is sanitized before being processed
    - Notable Gap: None

-  *Threat ID*: 44.
    - Threat Name: Spoofing of the User External Destination Entity
    - Threat Category: Spoofing
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

-  *Threat ID*: 45.
    -  Threat Name: External Entity User Potentially Denies Receiving Data
    -  Threat Category: Repudiation
    - Justification: Keycloak audits all actions
    - Existing Mitigations: Auditing is a critical part of Keycloak's architecture
    - Notable Gap: None

-  *Threat ID*: 46.
    -  Threat Name: Data Flow Webapp Output Data Is Potentially Interrupted
    -  Threat Category: Denial Of Service
    - Justification: Keycloak can not guarantee uninterrupted data flow from web applications
    - Existing Mitigations: None
    - Notable Gap: Keycloak can not guarantee uninterrupted data flow from web applications

### 4. Design Observations Summary

### 4.1 Notable Gaps:

There are multiple notable gaps within this DFD scenario. The first of the notable gaps includes uninterrupted data flows. Since Keycloak is a software to help with user authentication, it cannot guarantee any uninterrupted data flows between the webapplication and itself. This is partially because Keycloak cannot mitigate threats that exist for the external web app. This also goes hand in hand with another notable gap of encrypted data coming from the web application. Keycloak cannot control what type of data the external web application sends or accepts, thus mitigation of this gap is hard. Finally, the notable gap that keycloak could work on is a spoofing threat. Users could possible spoof keycloak by redirecting the URI. Keycloak does have configurations that can mitigate this problem, but it is reliant on the set-up of keycloak.

### 5. Reflection

Throughout the previous sections of this semester project we've mostly taken a "divide and conquer" approach to the tasks presented to us.  We wanted to take a similar approach to this section but this section required more direct communication and comparing of submissions and notes in order to address the threats generated by the Microsoft tool.  As a result, Chris built a first-draft version of the the DFD and presented it in the weekly meeting for edits/approval by the group.  We took that edited second draft into the professor meeting and then by that evening we had a complete diagram we could use to generate our threats.  We then divided that list of threats out to the group and found that constant discussion over discord about particular points of many of the threats greatly facilitated completing that portion of the assignment.  One of the difficulties we ran into early on was many similar responses to some of the threats and we assumed this would be incorrect.  After more deliberation we determined that many of these data flows could obviously be attacked in the same ways and therefore some similarity and overlap would be inevitable.


## Misc
[Back to Table of Contents](./README.md)

[Project Board for this section](https://github.com/orgs/22FAUNO-SA-Team-1/projects/5/views/1)
