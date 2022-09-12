<!-- This shouldn't be displayed -->
# CYBR 8420 Project Proposal
## Team 1

# Open-Source Project: Keycloak

## Perceived Operation Environment

The primary users of Keycloak will likely be profit generating organizations of any size. Larger organizations will appreciate the security tools that allow the control of access to varying degrees for its employees. Medium sized organizations may been drawn to the ability to add single-sign on. Smaller organizations will likely appreciate the capability to add identity brokering and social login so that they can manage some degree of security without needing a high level of expertise to manage it. The truth is that any sized organization who does not specialize in security, or even technology, could appreciate the tools Keycloak can provide them, especially if that organization has any kind of web presence.

### System Engineering Diagram

![Keycloak System Engineering Diagram]( /system_diagram.png "Keycloak System Diagram")

### Potential Threats

As with any software which exists solely for the purpose of security, threats perceived by end users will be exactly the types of threats Keycloak is attempting to solve. These threats include:

* Internal users gaining access to restricted data within the organization.
* External users gaining access to restricted data within the organization.
* Maintaining / ensuring that usernames and passwords are securely stored.
* Liability and financial impact in the event of a security breach.


### Current Security Features

Keycloak is in the business of providing its users with security features via web APIs to an external server. As such, the primary security features will come from the specific protocols used. These protocols include:

* OpenID
* OAuth 2.0
* SAML 

Aside from the protocols above, Keycloak also takes the communication of external vulnerability reporting very seriously. Below is an excerpt of their GitHub security policy. 

> Security Policy

>The Keycloak team takes security very seriously, and aim to resolve issues as quickly as possible. Building secure software is a continuous process, and can always be improved. As such we welcome reports on potential security vulnerabilities, as well as suggestions around hardening the software and our process.
>
>Reporting a suspected vulnerability
>
>It is important that suspected vulnerabilities are disclosed in a responsible way, and are not publicly disclosed until after they have been analyzed and a fix is available.
>
>To report a security vulnerability, send an email to keycloak-security@googlegroups.com.
>
>If you would like to work with us on a fix for the security vulnerability, please include your GitHub username in the above email, and we will provide you access to a temporary private fork where we can collaborate on a fix without it being disclosed publicly.
>
>Do not open a public issue, send a pull request, or disclose any information about the suspected vulnerability publicly. If you discover any publicly disclosed security vulnerabilities, please notify us immediately through keycloak-security@googlegroups.com.
>
>Supported Versions
>
>Depending on the severity of a vulnerability the issue may be fixed in the current major.minor release of Keycloak, or for lower severity vulnerabilities or hardening in the following major.minor release. Refer to https://www.keycloak.org/downloads to find the latest release.
>
>If you are unable to regularly upgrade Keycloak we encourage you to consider Red Hat Single Sign-On, which offers long term support of specific versions of Keycloak. 

## Team Motivation

Our team was motivated to find a project with an achievable goal of fully analyzing a project within the semester time frame.  When our primary list of projects fell short, a cursory search into trending topics in familiar languages led to Keycloak.  After some group discussion as well as approval from Dr. Gandhi, we found Keycloak to be a perfect fit for several reasons.  First, we wanted a project that would be interesting enough to keep everyone thoroughly engaged. Second, Keycloak was primarily written in a language familiar to us: Java.  Finally, we also wanted to be sure the project would have an active community with a variety of contributors as a healthier project would lend itself well to our needs.  It serves as software that can affect small web apps scaling up to massive applications looking for authentication.

## Open-Source Project Description

### What is it?
Keycloak is an open source identity and access management tool used for authentication and authorization. Among other things, Keycloak allows its users to implement single-sign on, identify brokering / social logins (Google, Facebook, GitHub, etc...). Additionally, Keycloak has a suite of admin tools like their admin console and account management console used for fine tuning allowing organizations to customize Keycloak to their specific needs. 

### Contributors & Activity

As of this writing, Keycloak has 631 individual contributors and 14,800 lifetime commits. The GitHub repository has been starred 13,500 times and it has been forked 4,700 times indicating the the repository itself is quite popular. Keycloak is also very active. The newest commit is only 20 hours old and there have been 23 commits over the past week alone. 

### Languages & Platform

The majority of Keycloke was written in Java, with trace amounts of other languages filling the gaps. The precise of breakdown of programming languages is as follows:

* Java 92.2%
* JavaScript 3.0%
* HTML 2.9%
* FreeMarker 0.6%
* TypeScript 0.5%
* Python 0.2%
* Other 0.6%

Keycloak has two main components:

* Keycloak Server, which is responsible for the API and GUI.
* Keycloak application adapter, which makes up all of the libraries used to call the server.

Based on such a design, the primary platform for Keycloak is web-based. An organization would use the Keycloak API directly inside of their code, but monitoring Keycloak is done from the GUI-based admin console on the Keycloak server.

### Documentation

An extensive source for Keycloak documentation can be found at the following URL.

https://www.keycloak.org/documentation

## Licensing Information

Keycloak is licensed under the Apache License, Version 2. https://www.apache.org/licenses/LICENSE-2.0

### Summary of License

This license gives You (entity using the code that is not a contributor) the right to make utilize the code base, and make changes to the code base. The You can utilize the code base in production products and sell products with the code base installed in them; however, the licensor of the code holds no responsibility in any warranty or actions that your changes cause. All of the original code base falls under the Apache License and by no means can that be taken away. You may apply your own licenses to your own work, or expansion of the code, but the Apache License trumps all other licenses in regards to the licensor's code.

### Summary of Sections

1) Definitions - Provided legal definitions for "You", "Source", "Work", "Contributor", "Contribution", "Object", "Source", "Derivative Works", "Legal Entity", "Licensor", and "License". The important definitions include "you", which is the entity that is using the permissions that the license gives, "Licensor", the copyright owner, and "Work", the work made available by the license. 

2) Grant of Copyright License - gives you the permission to utilize the licensor's work.

3) Grant of Patent License - gives you the permission to utilize teh licensor's work in your patented work; however, if you start litigation saying that the work of the licensor violates the patent, this permission is revoked.

4) Redistribution - You may distribute the work of the licensor given that you include the Apache License in all of licensor's work, make known which works are yours, must retain the source work of any item you distribute, and if there exists a "NOTICE" file in the work, you must distribute a copy of teh attribution notices that are contained in the NOTICE file.

5) Submission of Contributions - unless otherwise stated, any contribution of the work will be under the Apache License.

6) Trademarks - the use of trademarks or the licensor are not permitted to be used

7) Disclaimer of Warranty - the work of the licensor is an "AS IS" basis without warranties. 

8) Limitation of Liability - no contributor can be liable to you for damages.

9) Accepting Warranty or Additional Liability - you may offer warranty of the licensor's work; however, you are solely responsible for said warranty.

## Contributing

Under the Apache License, Keycloak welcomes outside contributions. However there are some ground rules that they lay out [here](https://github.com/keycloak/keycloak/blob/main/CONTRIBUTING.md). Their primary checklist is as follows
> 1. A discussion around the change (https://github.com/keycloak/keycloak/discussions/categories/ideas)
> 2. A GitHub Issue with a good description associated with the PR
> 3. One feature/change per PR
> 4. One commit per PR
> 5. PR rebased on main (`git rebase`, not `git pull`) 
> 6. [Good descriptive commit message, with link to issue](#commit-messages-and-issue-linking)
> 7. No changes to code not directly related to your PR
> 8. Includes functional/integration test
> 9. Includes documentation


## Security Incident History
Like their security policy, Keycloak is also open about potential vulnerabilities as noted [here](https://github.com/cpitman/keycloak-documentation/blob/master/topics/security-vulnerabilities.adoc). Additionally,there are [68 CVE's](https://www.cvedetails.com/product/46161/Redhat-Keycloak.html?vendor_id=25) for Keycloak spanning all versions.

## Repo Links

Official Keycloak repo - https://github.com/keycloak/keycloak

ClassDocs- https://github.com/22FAUNO-SA-Team-1/ClassProjectDocuments

## Team Reflection

Our group, (which I imagine is quite similar to most other groups) has a variety of skills and interests.  As Chris had a fair amount of experience leading academic project groups, he offered to fill the Team lead position and received group approval.  His goals as team lead is to keep open and constant communication channels open between all group members, and to delegate a smooth and manageable workload for all members while reflecting on any input from the group.  Along with team lead, the other role we wanted to fill right away was a combined role of writing, administration, and reviewing, as when combined with the team lead, would facilitate the smoothest workload for the entire group.  Chris has worked with Trenton in the past few years through undergrad at UNK so having administrative help from him fits nicely with the group and Chris and Trenton together will also serve as a small review team for major deliverables.

Everyone in the group has some form of technical expertise as would be common for a Cyber based course at the graduate level.  With that in mind, we've decided to have all group members contribute in part to the technical role.  While we'll likely assign tasks that fit each individuals skill sets, the thought process behind inflating the technical role beyond a single member is to receive the widest viewpoint possible on all given subjects as we tackle them.

Thus far, everyone in the group has contributed quite well.  Discussion is constant on the communication channels we setup for our group, and meetings so far have been relatively brief, to the point, and productive.  We believe we have taken great steps in the early weeks of team formation, organization, and preparation in order to set ourselves up with the best chance for success.

