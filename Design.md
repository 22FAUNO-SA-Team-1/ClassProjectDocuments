



### 1. Data Flow Diagram and Threat Modeling

1. [Keycloak DFD](https://github.com/22FAUNO-SA-Team-1/ClassProjectDocuments/blob/70ef72df39501e67f1419819c7d63b5194a6ec71/Diagrams/Part%204/DFD%20Model%20Screenshot.PNG)
2. [Threat Modeling Report](https://htmlpreview.github.io/?https://github.com/22FAUNO-SA-Team-1/ClassProjectDocuments/blob/39a5ba295d804d09f1305d362b79e30f07344579/Diagrams/Part%204/Keycloak%20Threat%20Report.htm)

### 2. Introduction


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
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

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
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 9.
    - Threat Name:  Elevation by Changing the Execution Flow in Keycloak
    - Threat Category:  Elevation of Privilege
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

<!--Start of John's Threats -->
- *Threat ID*: 10.
    - Threat Name:  Keycloak May be Subject to Elevation of Privilege Using Remote Code Execution
    - Threat Category:  Elevation Of Privilege
    - Justification:  
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 11.
    - Threat Name:  Data Flow LDAP Authentication Response Is Potentially Interrupted
    - Threat Category:  Denial Of Service
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 12.
    - Threat Name:  Potential Process Crash or Stop for Keycloak
    - Threat Category:  Denial Of Service
    - Justification:  Availability is critical for any application
    - Existing Mitigations:  Keycloak can be configured using clustering to improve performance and availability.
    - Notable Gap:  None

- *Threat ID*: 13.
    - Threat Name:  Data Flow Sniffing
    - Threat Category:  Information Disclosure
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

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
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 25.
    - Threat Name: Potential Process Crash or Stop for Keycloak
    - Threat Category: Denial Of Service
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 26.
    - Threat Name: Data Flow Sniffing
    - Threat Category: Information Disclosure
    - Justification: Data flow is encrypted
    - Existing Mitigations: Keycloak encrypts the data flow with standard encryption policies
    - Notable Gap:

- *Threat ID*: 27.
    - Threat Name: Potential Data Repudiation by Keycloak
    - Threat Category: Repudiation
    - Justification: Keycloak audits all actions
    - Existing Mitigations: Auditing is a critical part of Keycloak's architecture
    - Notable Gap: None

<!--Start of Zach's Threats -->
- *Threat ID*: 28.
    - Threat Name: Potential Lack of Input Validation for Keycloak
    - Threat Category: Tampering
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 29.
    - Threat Name: Spoofing the Keycloak Process
    - Threat Category: Spoofing
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 30.
    - Threat Name: Data Flow User Input Is Potentially Interrupted
    - Threat Category: Denial Of Service
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 31.
    - Threat Name: External Entity Web Application Potentially Denies Receiving Data
    - Threat Category: Repudiation
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 32.
    - Threat Name: Spoofing of the Web Application External Destination Entity
    - Threat Category: Spoofing
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 33.
    - Threat Name: Spoofing the Web Application External Entity
    - Threat Category: Spoofing
    - Justification:
    - Existing Mitigations:
    - Notable Gap:
- *Threat ID*: 34.
    - Threat Name: Elevation Using Impersonation
    - Threat Category: Elevation Of Privilege
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 35.
    - Threat Name: Spoofing the Keycloak Process
    - Threat Category: Spoofing
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 36.
    - Threat Name: Potential Lack of Input Validation for Keycloak
    - Threat Category: Tampering
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

<!--Start of Trenton's Threats -->
- *Threat ID*: 37.
    - Threat Name: Potential Data Repudiation by Keycloak
    - Threat Category: 	Repudiation
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 38.
    - Threat Name: Data Flow Sniffing
    - Threat Category: Information Disclosure
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 39.
    - Threat Name: Potential Process Crash or Stop for Keycloak
    - Threat Category: 	Denial Of Service
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 40.
    - Threat Name: Data Flow Webapp Output Is Potentially Interrupted
    - Threat Category: Denial Of Service
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 41.
    - Threat Name: Keycloak May be Subject to Elevation of Privilege Using Remote Code Execution
    - Threat Category: Elevation Of Privilege
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

- *Threat ID*: 42.
    - Threat Name: Elevation by Changing the Execution Flow in Keycloak
    - Threat Category: Elevation Of Privilege
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

-  *Threat ID*: 43.
    -  Threat Name: Cross Site Request Forgery
    -  Threat Category: Elevation Of Privilege
    -  Justification:
    -  Existing Mitigations:
    -  Notable Gap:

-  *Threat ID*: 44.
    - Threat Name: Spoofing of the User External Destination Entity
    - Threat Category: Spoofing
    - Justification:
    - Existing Mitigations:
    - Notable Gap:

-  *Threat ID*: 45.
    -  Threat Name: External Entity User Potentially Denies Receiving Data
    -  Threat Category: Repudiation
    -  Justification:
    -  Existing Mitigations:
    -  Notable Gap:

-  *Threat ID*: 46.
    -  Threat Name: Data Flow Webapp Output Data Is Potentially Interrupted
    -  Threat Category: Denial Of Service
    -  Justification:
    -  Existing Mitigations:
    -  Notable Gap:

### 4. Design Observations Summary

### 4.1 Notable Gaps:

### 5. Reflection


## Misc
[Back to Table of Contents](./README.md)

[Project Board for this section](https://github.com/orgs/22FAUNO-SA-Team-1/projects/5/views/1)
