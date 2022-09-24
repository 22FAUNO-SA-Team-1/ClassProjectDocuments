# Security Requirements Report
Team OneWon
## Essential Interactions, Diagrams, and Alignment Analysis

### Use Case 1: Implementing a Truststore

![Keycloak System Engineering Diagram]( ./Diagrams/Truststore.png "Keycloak Trust Diagram")

#### Description

The truststore is a foundational part of the Keycloak mechanism for remote connections.  Any Keycloak implementation that will connect to external systems will need the certificate of any external server to establish a secure SSL connection.  Remote server or Certificate Authority certificates must be stored in order to validate identity.  This validation is needed to prevent man-in-the-middle attacks.


#### Use Case

In this case, a non-privileged user will connect to an external server using Keycloak.  The user expects to connect to the external server without issue and without risk of credential theft.  The user believes proper care has been taken to configure the system and underlying security in order to connect to the expected endpoint.  Server configuration should be for maximum security to prevent any security breach from external entities.



#### Misuse Case

In reality, a hacker may attempt to infiltrate the Keycloak server and the hacker's goals may not be immediately apparent.  For example, curiosity may be a motivating factor which leads to an opportunistic security breach.  Should the Keycloak server be attacked, layers of security should be in place to maintain the integrity of the truststore.  Using a strong truststore password will eliminate the possibly of the bad actor altering the truststore file.  Similarly, a security certificate must be swiftly revoked from the truststore should a remote server's certificate become compromised.  Regular logging and auditing will eliminate these potential security risks.  The hacker may attempt to alter or delete logs to cover any trace of their presence in the system.  An alerting feature may be configured to notify administrators of mass file changes within the system.



#### Security Requirement

The Keycloak truststore is created and maintained using the pre-built Java "keytool" utility, therefore the truststore utilizes the underlying security of keytool.  The truststore password will be embedded into web applications that make use of the truststore for connection purposes.  Only administrators should have access to the embedded password, and robust system security is necessary to prevent external hackers from accessing the password.  Regular system logging and auditing should be conducted to monitor for any truststore changes.  Additionally, a file hash fingerprint should be recorded before and after any truststore changes to audit for any unrecorded changes to the file.


### Use Case 2: Creating a Realm

![Keycloak Realm Diagram]( ./Diagrams/realmv4.png "Keycloak Realm Diagram")

#### Description

Realms are the base level structure inside of Keycloak. Realms manage users in a given domain, what applications are assigned to those users, and what permissions the users have inside of those applications. The Master Realm is provided by Keycloak upon initial setup. The administrator of the master realm is the only individual with authority to create and manage other realms in the environment. Realms separate the authority of various administrators. For example, the Master Admin may choose to create an employee realm, a customer realm, and a contractor realm. The authority of each sub-realm administrator only extends to users/application inside of their realm.

#### Use Case

In this use case, the master administrator has created a sub-realm. While doing so, the master admin will manage the sub-realm admin account (assigning the role to a separate user if applicable). The master admin will also go on to add standard users (handled in a separate case) and assign them applications and permissions. Optionally, the new realm can be configured according to specific security requirements. Creating cryptographic realm keys is a base level security measure required by all realms. Optionally, the admin can choose to require SSL/HTTPS connections and establish a brute force detection policy that defines what a brute force attack looks like, and how they should be handled. 

#### Misuse Case

In this misuse case the bad actor is a malicious standard user who wishes to assign new applications to their role and/or escalate their current application permissions. Because it is the path of least resistance, the malicious user will first attempt to change permission settings from their own account. Because the standard user does not have administrative privileges, their attempt to edit their own permissions will fail and they will have to resort to more complex methods of achieving their goal. 
The next logical step for the attacker will be to obtain administrator privileges within their realm. The simplest way to achieve this goal would be to phish the sub-realm administrator’s login information. This can be mitigated by configuring the Keycloak sub-realm to use multi-factor authentication. Another more complex method would be to initiate a man in the middle attack to obtain the administrative user’s password. Since cryptographic realm keys are required with Keycloak, this risk is mitigated by default. This risk is further mitigated if the administrative user has added an SSL/HTTPS requirement in the realm’s security settings.
Finally, a user could choose to write a script to brute force the administrator’s password. If the administrator established a brute force detection policy, this risk will be mitigated. However, because brute force detection is optional, not establishing a brute force detection policy would offer the attacker their highest probability of success. 

#### Security Requirement
It is absolutely essential that Keycloak administrators take security precautions that are not enabled by default. First and foremost, brute force detection should be activated and the maximum number of unsuccessful login attempts should be set a reasonable number, three to five should be sufficient (Max is thirty). Keycloak also offers a feature to notify administrators when the maximum number of login attempts has been reached and the account is locked, this too should be enabled. Additionally, Multi-factor authentication should be turned on to ensure that even if a rouge user is able to capture the administrator's login credentials, they would not be able to access the account successfully. Finally, requiring SSL/HTTPS will compliment the default security provided by cryptographic realm keys which is one of the only security measures Keycloak makes mandatory.


### Use Case 3:  User Management

![]( ./Diagrams/UsersMUCv2.png)

#### Description
Proper user and admin account management is critical for any application and Keycloak offers a variety of ways to add, secure, and maintain a collection of authorized users.  Users can be added directly through the web interface dashboard by an admin, as well as via via shell commands or even queried via LDAP or another user database service.  Users can also be assigned a Realm at creation, or Realm affiliation can be changed later at the admin's discretion.  This scenario will explore ensuring your user accounts are created securely and not maliciously manipulated.

#### Use Case
The user in question for this scenario is a a system administrator in charge of adding and maintaining the user accounts for a system that has employed Keycloak for authorization.  The administrator will have the ability to add a user manually via the browser dashboard, and will have a variety of actions available in order to ensure that only authorized users are added and accounts aren't modified unless intended by the company.  Along with the ability to add a user directly, the administrator will have the option of using an LDAP service to retrieve users from an existing user database.  Finally, the administrator will maintain the integrity of the existing user base and ensure that accounts aren't gaining escalated privileges without authorization.

#### Misuse Case
On the Misuse side of this scenario, the attacker will be a rogue inside employee with user privileges.  The first line of attack will be to create a fake malicious account with access to internal assets.  This attack will be mitigated by maintaining logs showing when a user account is added or modified, when it is added or modified, and any particulars changed through the modification.  In response to logs, an attack may attempt to modify the logs in order to cover their tracks.  This in turn, can be prevented by ensuring that your logs allow appending data only so that log entries can be added but no entry can be modified.

Another route of attack may be to either elevate the privileges of the attackers account, or an accomplice's account.  This can be mitigated by ensuring that every user has the absolute minimum privilege status required to function in their company role.  In response an attacker may instead seek to escalate horizontally by attempting to gain control of a newly added user account that either has a very easy to crack password or worse yet still has a default issued password where a user hasn't logged in and changed the password to an individualized one yet.  The attack can be rebuffed here by ensuring that the default password is changed semi-regularly or rotated from an internal list of default passwords.  Administrators can also keep an eye on user behavior to catch any suspicious behavior.

Finally, in the case where user accounts are queried from an LDAP server, an attacker may wish to attempt a malicious LDAP injection attack in order to gain unauthorized access or other valuable information from the database.  This attack can be mitigated be including input validation in the user database software which may still have some vulnerabilities to exploit.  In order to ensure the LDAP server can't be attacked and a malicious user can't retrieve account information from the server, any and all user input must be both validated and sanitized.

#### Security Requirements
* Utilize logging to monitor the addition, deletion, and modification of user accounts and ensure the log files can only be appended to in order to disallow malicious modifications.
* Minimize privileges for all users with regard to ensuring they can complete their role in the company and ensure that default passwords aren't the same and left in place for long periods of time.
* Ensure that user input in regards to user database queries is always BOTH validated and sanitized.


### Use Case 4: Authorizing a User

![Keycloak Authorization Diagram]( ./Diagrams/Ethen_Kuether_Authorization_UseCase.jpg "Keycloak Authroization Diagram")

#### Description
User authentication is important for any application, especially keycloak. Keycloak is a tool to help manage user authorizations and helps implement features such as single sign on (SSO), Dual Factor Authentication, and the use of Active Directory, plus many more tools. With an application that holds up the front line of many other applications, it is important that the user authentication is secure. Malicious actors should not be able to sign on as other employees as that would compromise not just one application, but many.

#### Use Case
The user in this use case is not necessarily a person, but the system requesting authorized access. This could mean whenever a user signs in, or goes to a page. If the user has already signed in, the SSO feature of keycloak should be able to authorize the user without the user having to re-enter their credentials. The authorization use case may also include dual factor authentication if the user is signing on for the first time. Finally, the use case involves making sure the necessary information such as passwords are hashed to provide the best security.

#### Misuse Case

#### Security Requirements

### Use Case 5: Hosting a Keycloak server

![Hosting a Keycloak server Diagram](./Diagrams/TCusecase.png "Hosting a Keycloak server Diagram")
#### Description

Keeping High Uptime on a SSO service like Keycloak is important since if it lags the entire user base will lag thus decreasing organizational efficiency.

#### Use Case
In this scenario, a Systems Engineer on a tight timetable needs to create many servers today. Luckily hosting a (barebones) Keycloak Server seems relatively simple, they create a cluster with multiple server nodes, configure logs (because everyone loves logs), then start the service in each node and pass off to the Security Team.


##### Misuse Case
On the opposite side, lies a Turncoat (TC) whose goal is to lower uptime. TC first tries to Denial of Service (DoS) a node. Unfourtunately, it looks like the Service is still operational due to node redundancy. Looks like TC will have to Distrubute a DoS per node. This will multiply overhead and increase suspicion on TC.


#### Security Requirement
* Utilize logging to detect attempted DoS and DDoS attempts
* Node fall over if DoS is successful
* Increase nodes to increase threshold for a successful DDoS attempt
* Utilize external resorces (e.g. Firewall, disabling accounts) to stop the Turncoat's goal of Hindering SSO


### Reflection on Security Requirements

## OSS Project Documentation Review

Security Documentation

Being that Keycloak is in the business of security, their security related documentation was fairly extensive. Keycloak has included many guides on their website describing various methods for setting the appropriate security measures based on the users need. 

[Server Administration Guide](https://www.keycloak.org/docs/latest/server_admin/index.html)

[Section 15](https://www.keycloak.org/docs/latest/server_admin/index.html#mitigating-security-threats) is dedicated entirely to mitigating security threats. There are a wide variety of security options available to users of Keycloak many of which are not turned on by default. Some of these features include: Brute force detection, Clickjack prevention, SSL/HTTPS requirements, and default PBKDF2 password hashing.

Keycloak also has documentation specifically focusing on securing the clients and services being integrated with Keycloak.

[Securing Applications and Services Guide](https://www.keycloak.org/docs/latest/securing_apps/index.html)

//TODO

Security Related Issues and Security Advisories

There are currently no open security related issues on Keycloak's Github. This is likely due Keycloak's [security policy](https://github.com/keycloak/keycloak/security/policy) which expressly prohibits opening issues for security related concerns. Instead, Keycloak prefers private email communications and no public announcement about the vulnerability until it can be patched. 

Keycloak also maintains a [security advisory](https://github.com/keycloak/keycloak/security/advisories) board on their Github page, which describe notable (patched) security vulnerabilities, along with their severity level. 

## Overall Planning and Reflection
We chose, like I imagine many have done and will continue, to have each group memeber tackle one of the 5 use / misuse cases and then divide the rest of the writeup as evenly possible among group members.  Ethen took on the use and misuse case reflection while John and Zach dove into the Keycloak documentation review.  Chris covered the project planning and reflection for this stage while adjusting based on group feedback and Trenton managed administrative duties, managing github and signing off on final reviews before submission.

The first roadblack our group encountered was the 5 key interactions.  The initial plan was to have each member find an interaction themselves and report back to the group on a "first come first served" basis to prevent any overlap.  We all had touble initially finding interactions that would fit the requirements of the assignment.  The consensus was that it was difficult to find 5 distinct cases with an OSSP that served one main purpose of serving as an authentication tool for applications.  Time, research, and plenty of group discussion, led to a few initial cases that allowed the rest of the group to determine use cases to really dig into the diagram work and develop the "cat and mouse" style of attack and defend planning.  The rest of the security requirements phase went quite smoothly.
