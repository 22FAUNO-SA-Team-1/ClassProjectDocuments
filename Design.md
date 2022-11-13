



### 1. Data Flow Diagram and Threat Modeling

1. [Keycloak DFD](https://github.com/22FAUNO-SA-Team-1/ClassProjectDocuments/blob/70ef72df39501e67f1419819c7d63b5194a6ec71/Diagrams/Part%204/DFD%20Model%20Screenshot.PNG)
2. [Threat Modeling Report](https://htmlpreview.github.io/?https://github.com/22FAUNO-SA-Team-1/ClassProjectDocuments/blob/39a5ba295d804d09f1305d362b79e30f07344579/Diagrams/Part%204/Keycloak%20Threat%20Report.htm)

### 2. Introduction

The scenario we have based our data flow diagram on, and by extension generated and addressed the various threats existing in said scenario, revolves around what we believe is the primary purpose and most common use of Keycloak, namely authenticating a valid user and providing secure access to an application.  In this scenario an external interactor representing a user wishing to access protected assets provided by a web application must be properly authenticated by Keycloak before gaining said access.  This user will be prompted by Keycloak to enter their credentials as a data flow source into the Keycloak process.  Keycloak will then, based on the Keycloak server settings, determine how that user should be authenticated, and will send an authentication request to the internal truststore data source, or in the case where the application relies on an external LDAP server for authentication, that authntication request is sent out to an external interactor representing the LDAP server as that server is outside the controle of the Keycloak codebase


### 3. Individual Threat Review
<!--Start of Chris' Threats -->
- *Threat ID*: 1.
    - Threat Name:  Potential Excessive Resource Consumption for Keycloak or Truststore
    - Threat Category:  Denial of Service
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 2.
    - Threat Name:  Spoofing of Destination Data Store Truststore
    - Threat Category:  Spoofing
    - Justification:
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
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 5.
    - Threat Name:  Data Flow LDAP Authentication Request Is Potentially Interrupted
    - Threat Category:  Denial of Service
     - Justification:  Keycloak cannot authenticate users if LDAP service is interrupted.
	- Existing Mitigations:  None
    - Notable Gap:  Difficult to mitigate.  Could be mitigated with a distributed network connection between Keycloak and LDAP server.

- *Threat ID*: 6.
    - Threat Name:  External Entity LDAP Server Potentially Denies Receiving Data
    - Threat Category:  Repudiation
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 7.
    - Threat Name:  Spoofing of the LDAP Server External Destination Entity
    - Threat Category:  Spoofing
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

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
    - Justification:  
    - Existing Mitigations:  
    - Notable Gap:

- *Threat ID*: 15.
    - Threat Name:  Potential Lack of Input Validation for Keycloak
    - Threat Category:  Tampering
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 16.
    - Threat Name:  Spoofing the Keycloak Process
    - Threat Category:  Spoofing
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 17.
    - Threat Name:  Spoofing the LDAP Server External Entity
    - Threat Category:  Spoofing
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 18.
    - Threat Name:  Elevation Using Impersonation
    - Threat Category:  Elevation Of Privilege
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

<!--Start of Ethen's Threats -->
- *Threat ID*: 19.
    - Threat Name: Spoofing the User External Entity
    - Threat Category: Spoofing
    - Justification: Keycloak uses standard authroization
    - Existing Mitigations: Keycloak utilizes standard authorization protocols to prevent spoofing
    - Notable Gap: None

- *Threat ID*: 20.
    - Threat Name: Elevation Using Impersonation
    - Threat Category: Elevation Of Privilege
    - Justification: 
    - Existing Mitigations:
    - Notable Gap:

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
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

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
    - Notable Gap:

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
    - Existing Mitigations: Keycloak includes eight validators by default: Length, integer, double, uri, pattern, email. local-date, person name-prohibited-characters, username prohibited characters, and options
    - Notable Gap: None

- *Threat ID*: 29.
    - Threat Name: Spoofing the Keycloak Process
    - Threat Category: Spoofing
    - Justification: Keycloak allows Open Redirects
    - Existing Mitigations: Written configuration recommendation
    - Notable Gap: If an administrator does not make a redirect uri sufficiently specific, or if a user is already authenticated and redirect uris have not been configured, spoofing Keycloak is possible. 

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
    - Existing Mitigations: The SSL trust manager ensures the web application's identity is valid and ensures the DNS domain name gainst the server's certificate
    - Notable Gap: None

- *Threat ID*: 33.
    - Threat Name: Spoofing the Web Application External Entity
    - Threat Category: Spoofing
    - Justification: Not applicable - Web Applications do not have permissions exploit in Keycloak
    - Existing Mitigations: Users have permissions to access web applications.
    - Notable Gap: None 
  
- *Threat ID*: 34.
    - Threat Name: Elevation Using Impersonation
    - Threat Category: Elevation Of Privilege
    - Justification: Not applicable - Keycloak's sole purpose is user authentication and authorization
    - Existing Mitigations: Only an administrator can grant privileges. Privilege extends from user to application.
    - Notable Gap: Not Applicable

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
    - Existing Mitigations:  Keycloak includes eight validators by default: Length, integer, double, uri, pattern, email. local-date, person name-prohibited-characters, username prohibited characters, and options
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

### 5. Reflection


## Misc
[Back to Table of Contents](./README.md)

[Project Board for this section](https://github.com/orgs/22FAUNO-SA-Team-1/projects/5/views/1)
