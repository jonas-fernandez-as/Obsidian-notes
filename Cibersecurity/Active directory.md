
## Phisical AD components

### Domain controller (the top dog)
A domain controller is a server with the AD(active directory) DS(domain services) server role installed that has specifically been promoted to a domain controller
- Host a copy of the AD DS directory store
- Provide authentication and authorization services
- Replicate updates to other domain controllers in the domain and forest
- Allow administrative access to manage user accounts and network resources

## AD DS Data Store
The AD DS ata store contains the database files and processes that store and manage directory information for users, services, and applications
- Consists of the Ntds.dit file
- Is stored by default in the %SystemRoot%\NTDS folder on all domain controllers
- Is accessible only through the domain controller processes and protocols

## Logical AD Components


### AD DS Schema
The AD DS Schema:
- Defines every type of objet that can be stored in the directory
-  Enforces rules regarding object creation and configuration

![[Screenshot_2023-10-22_19_42_26.png]]

### Domains
Domains are used to group and manage objetcs in an organization

- An administrative boundary for appliying policies to groups of objects
- A replication boundary for replicating data between domain controllers
- An authentication and authorization boundary that provides a way to limit the scope of access the resources 

### Trees
A domain tree is a hierarchy of domains in AD DS 
contoso.com (root)
emea.contoso.com (child)
na.contoso.com (child)

All domains in the tree:

- Share a contiguous namespace with the parent domain 
- Can have aditional child domains
- By default create a two way transitive trust with other domains


### Forests

A forest is a collection of one or more domain trees 

- Share a common schema
- Share a common configuration partition 
- Share a common global catalog to enable searching
- Enable trusts between al domains in the forest 
- Share the enterprise Admins and Schema Admins groups 

### Organizational Units (Ous)
Ous are Active directory containers that can contain users, groups, computers and others OUs
- Represent your organization herarchically and logically
- Manage a collection of objects in a consistent way
- Delegate permissions to administer groups of objects
- Apply policies

### Trusts
Trusts provide a mechanism for users to gain access to resources in another domain
Types :
- Directional 
- Transitive
All domains in a forest trust all other domains in the forest
Trust can extend outside the forest

### Objects 

- User
- InetOrgPerson
- Contacts
- Groups
- Computers
- Printers
- Shared folders









NOTES: PII personal identifable information