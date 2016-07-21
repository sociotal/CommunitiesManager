#SocIoTal Communities Manager Introduction

A SocIoTal Community provides a closed environment where only registered users and entities can share information: registered resources/entities will publish data that only registered users will be able to read. The original idea refers to a SocIoTal Community, created and managed by a SocIoTal user, as a group of users and resources with a common objective or inquisitiveness. A role set definition will decide who can do what within the community.

This introduction describes the SocIoTal Communities Manager tool and its functioning, including the creation, operation and exploitation of communities within the SocIoTal framework. Updated and detailed examples of the methods here pointed are available in [*SocIoTal WiKi*](https://github.com/sociotal/SOCIOTAL/wiki/SocIoTal-Communities-Manager).

##SocIoTal Community definition
-----------------------------

The concept of the SocIoTal Community is based on the types of relationships among smart objects and users. In this case, the usual driver of communities is the common interest relationship, putting together users and information related to a similar target or application. This way, SocIoTal defines a community as a secure cooperation between different producer users, making entities (devices, services, events, resources etc.) available to selected consumer users (community members), to achieving a common objective.

Although an ad-hoc community creation mechanism is also possible in the SocIoTal Integrated Platform ([*platform.sociotal.eu*](platform.sociotal.eu)), the project provides a platform-based communities structure, established between users and entities connected to the same infrastructure network. This structure, built in SocIoTal Integrated Platform, provides centralized services to SocIoTal users, as creation and definition of communities, registering and management of users, roles management and so on.

##SocIoTal Communities Manager 
-----------------------------

Within the SocIoTal framework, the Communities Manager is the element that provides the Communities creation and management mechanisms and directly links with the Identity Management and Context Management services. SocIoTal’s Communities Manager offers a centralized point to create SocIoTal Communities for registered users to, in turn, register entities and share them. SocIoTal Communities, therefore, provides a way for interested user to share interesting data and/or to access interesting resources whilst SocIoTal Communities Manager implements the mechanisms to create and manage these communities.

From a high level point of view, this SocIoTal component will manage:

-   **Users**: represent user entities within the SocIoTal Communities framework, keeping the credentials (names, passwords, roles and tokens) needed to be authenticated within the Communities Manager and the SocIoTal framework plus other extra information relevant for other SocIoTal applications (address, nicknames, organization, etc.). The SocIoTal user will be created and managed using the SocIoTal IdM, so registered users through other SocIoTal tools (such the User Environment) can be also used within the Communities tools and vice versa.

-   **Communities**: seen as groups of users and resources identified by a name and an id plus a description. The Communities Manager will provide the mechanisms to identify a user as member of a given community whilst a predefined set of roles will identify the different rights on it. Communities can be created by SocIoTal registered users and they, as community creators, will be able to manage them (add new users, modify a community or delete it).

-   **Domains**: as a group of isolated communities and users. Every user and community will be linked to only one domain, with the possibility to be duplicated under other different one. In current Communities Manager instance, a default domain (called SocIoTal) will be set for pilots and initial developments and new domains, for extra purposes, may be created by the SocIoTal Platform administrator.

-   **Tokens**: provided by the Communities Manager and requested by a user, the Community-Tokens, represented by their corresponding UUID (Universal Unique Identifier) will identify the requestor user (previously registered within the Communities Manager), the community and the domain it belongs to, providing also extra related information such as the role the requestor has and its validity (as well as its expiring date).

The Communities Manager provides mechanisms to register users, manage communities and request and validate Community-Tokens. These methods collection is published and updated in the [*SocIoTal Wiki*](https://github.com/sociotal/SOCIOTAL/wiki/SocIoTal-Communities-Manager).

The current implementation of SocIoTal Communities Manager in Figure below relies on FIWARE Identity Management Generic Enabler available implementation, the KeyRock. An instance of the Keyrock (Version 4.4.1) is also provided by the SocIoTal IdM so, user management will be implemented using SocIoTal IdM whilst the rest of communities’ management functionalities will directly use this SocIoTal Keyrock instance. This way, integration issues are addressed, guaranteeing the same set of users and identities (the same user’s directory) for the whole set of SocIoTal components and compatible identification/authentication mechanisms.

| <img src="https://github.com/IEMaestro/CommunitiesManager/blob/master/documentation/ComM_Integration.png" width="850" height="600" />                                                                     |
|-------------------------------------------------------------------------------------------------------------------------------|
| <span id="_Ref444606420" class="anchor"><span id="_Toc445736928" class="anchor"></span></span>1. Communities Manager integration |

From the point of view of the user (either developer or final user), SocIoTal Communities Manager provides a HTTP/HTTPS RESTful API divided into three main set of methods:

-   **Users API**: groups the functionalities related to the users’ creation and management. This set of functionalities are directly linked to SocIoTal IdM, using the provided JAVA API and its SCIM compliant interface. This way, all SocIoTal components link to the same shared user’s directory.

-   **Communities API**: contains the methods to create (and manage) communities and assign users and roles. It is implemented through the Keystone V3 API provided by the Keyrock instance of SocIoTal, shared also by the SocIoTal IdM. This way, SocIoTal IdM will have access also to the domains/communities schema created by the Communities Manager.

-   **Community-Tokens API**: includes the request and retrieve community-tokens operations and the community-tokens validation. It is implemented over the same token schema SocIoTal IdM uses to validate users by user/password mechanism, so community-token can be also used to authenticate users

For the SocIoTal Integrated platform administrator, with access to the platform instance, the Keystone python-openstackclient (V3) including http RESTful V3 API and Keystone command line, will be available, following Keyrock APIs documentation. These API collections will allow the administrator to configure extra parameters, as create and manage new domains, add/remove new roles and manage communities and users.

According to what has been mentioned, SocIoTal Communities Manager requires an operative instance of the SocIoTal IdM component, including its KeyRock instance with http OpenStack Keystone V3 interface support:

-   The SocIoTal IdM component provides the Users’ Directory to store all the identities created within the Communities manager, using the SCIM standard

-   The Keyrock, through its supported keystone core, provides:

    -   Domains & Communities directory that supports the mechanisms to create, store and manage all the communities’ structure.

    -   Tokens Management tools to create, store, query, retrieve and validate the community tokens.

The IP addresses and ports of these required instances can be given through the corresponding configuration file of the Communities Manager. Its northbound offers to users a RESTful API to manage user identities and communities, as well as to query, retrieve and validate Community Tokens, further used to integrate with other SocIoTal platform components. Current working SocIoTal Communities Manager version supports FIWARE KeyRock IdM versions 4.3, 4.4 and 5.

| <img src="https://github.com/IEMaestro/CommunitiesManager/blob/master/documentation/ComM_Implementation.png" width="800" height="500" /> |
|-------------------------------------------------------------|
|2. Communities Manager Updated implementation                  |

SocIoTal Communities Manager building blocks, methods and functionalities have been developed using Eclipse IDE for Java Developers (Indigo version). It utilised Java SE Development Kit 7 from OpenJDK (java-1.7.0-openjdk) and Open Java runtime 7 (openjdk-7-jre) to develop and run the resulting Java code, including the RESTEasy JAX-RS implementation libraries provided by JBoss to build RESTful Web Services and RESTful Java applications. For the prototype version, SocIoTal Communities Manager methods and APIs are distributed as a web application archive (WAR) to be deployed within a Web Application Server. For the instances involved in the different tests performed so far, JBoss AS 7.1 has been selected. Figure 9 details the updated implementation.

### Users directory/SocIoTal IdM integration

As described above, communities’ management within SocIoTal project requires user management. A SocIoTal physical user is mapped to one (or several) virtual users, known as “users” within the communities’ management or “identities” in other SocIoTal components. Every user will be identified by a “name” and an “id” and will belong to a domain. In addition to these, the defined user may include extra attributes, such as nickname, e-mail, postal address, organization and so on. “SocIoTal Wiki-Communities Manager-Register a User template” shows the current set of supported attributes. Further versions of the SocIoTal Communities Manager will include more attributes and the support of user-defined ones. The created virtual user (or identity) will be authenticated by its pair “name/password” against the Communities Manager. To implement this, the instance of the Communities Manager requires an internal users’ directory, provided by the SocIoTal IdM.

Integration with SocIoTal IdM is embedded within the Communities Manager through the JAVA libraries that implements the SocIoTal IdM KeyRock Client SCIM interface, detailed in the SocIoTal WiKi. SocIoTal Communities Manager uses this Java API to create, store, read and modify virtual users or identities. This way, any identity created by the Communities Manager will be compatible and available to any other SocIoTal component integrated with the SocIoTal IdM and viceversa.

### Community Token generation

The Community-Token will be the key to identify the user, the role and the community membership, and therefore, the key to enable community management within SocIoTal integrated platform. SocIoTal Community-Tokens are based on Keyrock/Keystone UUID authentication tokens that provide authentication mechanisms as well as validation and information related to projects, domains and roles the authenticated identity has or belongs to. Figure 10 shows how tokens are created by Keystone/Keyrock and later used by the client to “sign” every subsequent API request.

| <img src="https://github.com/IEMaestro/CommunitiesManager/blob/master/documentation/Token_Gen.png" width="850" height="900" /> |
|-----------------------------------------------------------|
| 3. Community-Token request/validation flow                   |

Based on supplied username/password plus domain and community (if specified), SocIoTal Communities Manager would:

-   Require Keyrock/Keystone instance to generate an UUID token, valid for the domain and community specified.

-   Store the UUID token in its backend.

-   Retrieve a copy of both, the UUID token and the complete token with all info (but the password) related to user, domain, role and corresponding projects back to the Communities Manager.

-   Communities Manager processes the retrieved token, complements this with community info it belongs to and send a copy of both the token UUID (plain text) and the complete related info (JSON format). This is what we call Community-Token

-   The client would cache the token:

    -   The JSON Community-Token format serves the user to know what does this token allow it to do (based on role and community info) and since when (validity period).

    -   The Community-Token UUID should be passed along with each API call by the client, integrated with the Communities Manager.

 ```json
{
	"values": {
		"token": {
			"methods": ["password"],
			"issued_at": "2016-02-04T16:00:11.676055Z",
			"community": {
				"id": "206fa9c0e1f344758d240099f2cb78d7",
				"name": "6ee840f1-601c-4bda-bc28-cb3ef9b9812a",
				"domain": {
					"id": "c43a3df1e0f74480b38158b09ebf0b56",
					"name": "SocIoTal"
				}
			},
			"expires_at": "2016-02-04T17:00:11.676023Z",
			"audit_ids": ["OcEtiNIYTliS1zJ60NrsPQ"],
			"roles": [{
				"id": "ff0422fda2cf4c0c89e5b9260ec79a45",
				"name": "owner"
			}],
			"user": {
				"id": "6ee840f1-601c-4bda-bc28-cb3ef9b9812a",
				"name": "user1",
				"domain": {
					"id": "c43a3df1e0f74480b38158b09ebf0b56",
					"name": "SocIoTal"
				}
			}
		}
	},
	"communityToken": "e054d935c5584fdda866ab420d48447b"
  
 ```

```JSON format Community-Token example```

Once a Community-Token is obtained and it’s corresponding UUID is added to a request, the corresponding API endpoint (e.g. the SocIoTal Context Manager), in order to validate this token:

-   The API endpoint would send this UUID back to SocIoTal Communities Manager for validation (using its “Validate” method).

-   Communities Manager would take the UUID and match it against its auth backend (Keyrock/Keystone) (check UUID string, expiration date).

-   Keyrock/Keystone would return “success” or “failure” message to the Community Manager. If success, Communities Manager will retrieve also the set of info associated to the requested community-token, for the API endpoint to check roles, users and corresponding communities and domains.

The token validation mechanism is only available (requires a special authentication-token) for SocIoTal APIs endpoints (Context Manager, User Environment, IdM, Trust Mangera, etc.). In further versions, a special community-token validation method will be implemented for external developers.

### SocIoTal Context Manager integration

SocIoTal Context Manager, as detailed in SocIoTal’s architecture and Context Manager implementation, includes the SocIoTal’s Entities directory. This means that every resource, event, application structure or other element (except users) required by SocIoTal components or deployments will be stored here. So information related to entities belonging to communities will relay in the SocIoTal Context Manager, assisted by the Communities Manager.

SocIoTal Context Manager supports two different operation modes related to Communities:

-   With no “Community-Token” presence in the request. In this case, all the requests will be addressed to a default community within a default domain. This is, all resources are assumed to belong to the same community. There will be no users/roles (form the point of view of communities) filtering and only SocIoTal security framework policies and restrictions (through the required Capability-Token) will apply. This could be useful when an instance of SocIoTal is going to be used for a single and well defined purpose, such as the platform for an application.

-   When “Community-Token” is attached to the request. This token will be validated by the Communities Manager and info related to it will be retrieved to the Context Broker (as mentioned in the section above for API endpoints). The Context Broker here will apply the corresponding communities’ restrictions/actions required, depending on the requested method and the info obtained from the validated Community-Token.

Within the case of Community-Token presence, and assuming it has been successfully validated by the Communities Manager (otherwise, the request will be rejected), the Context Manager will act as follows:

In case of registering a new entity (either through the NGSI9, the NGSI10 or the EXTENDED API) the Community-ID where this new entity will be included will be extracted from the Community-Token. This way, only users belonging to a given community will be able to register new entities within this community. The Community ID will be automatically added, as a prefix, to the proposed entity ID. This prefix will be managed (and filtered if required) by the Context Manager. The requestor user will have no control over it, avoiding so further attempts to access resources from other communities. An “owner” attribute will be added to the entity structure, including, by default, the user id extracted from the Community-Token. Methods to point to other “owners” will be implemented.

In case of updating an existing entity (either through the NGSI9, the NGSI10 or the EXTENDED API), the Community-Token will provide the Community-ID prefix of the entity ID provided in the request (to properly identify the entity and the community it belongs to) and the user requestor ID. By default, (other operatives could also be performed), this user ID must match the owner id stored in the entity in order to allow the update. This way, only the owner of the entity, belonging to the Community the entity belongs to, will be able to write or change the entity. If no-owner is specified, the default operative will understand that all members of the community (with a specific role, provided by the Community-Token) may update this resource.

When entities searching, discovering or subscription request that provides access to entities stored information, is addressed, the Community-ID provided by the Community-Token will automatically complement the request (the Context Manager will add it to the searching, discovering or subscription process). This way, only users able to provide a Community-Token for the selected community, will be able to perform these actions, and retrieved information will be restricted only to those entities belonging to the community. This operative will also apply to the case a user tries to directly read an entity.

Information sharing inside a SocIoTal Community
-----------------------------------------------

This section intends to describe, step by step, the processes to share and to access information within a community. All the methods required (and mentioned here) are described (and updated) in [*SocIoTal Wiki*](https://github.com/sociotal/SOCIOTAL/wiki/SocIoTal-Communities-Manager).

### User registration

The first step to start working with SocIoTal Integrated Platform is to create a SocIoTal User or Identity. A Physical user may have several different identities, linked to different SocIoTal tools, communities or applications.

SocIoTal Users or Identities rely on the SocIoTal IdM. Traditional and basic, but necessary, identity management operations in scenarios where claim-based accesses are not needed, are addressed within SocIoTal IdM by integrating with FIWARE Keyrock. As the result of this integration, SocIoTal IdM KeyRock Client is provided. This Client is composed of a Java library that provides a basic API for identity management relying on the FIWARE KeyRock server. To carry out such communication, the SCIM 2.0 and Identity API v3 interfaces provided by this IdM are used. SocIoTal Communities Manager, in turn, provides a set of RESTful methods to register users and create SocIoTal Identities, based on this provided SocIoTal IdM KeyRock Client. This way, any identity created by the SocIoTal IdM KeyRock Client, from any other SocIoTal component, will be compatible and accessible from the Communities Manager and vice versa.

Using the Communities Manager RESTfull API, the way to register a user is through calling the method RegisterUser (using SCIM API) or addUser (directly with Keystone V3 API). This method allows a future SocIoTal User to define, create and register a new SocIoTal Identity within a SocIoTal Domain. A physical user may create as many SocIoTal Identities as needed for their purposes. An Identity will be always linked to a domain, allowing the reuse of the selected identity name (username) in different domains. The associated request payload (below) includes several attributes to define user identity. It must include the parameters “username” and “password”, but it can also include any of the shown parameters in the figure. The “active” element sets the user as “enabled” (true) or “disabled” (false). By default, if it is not pointed, the user will be registered as “disabled” (false). A domain is also mandatory to define a SocIoTal Identity. If no domain element is provided, Communities Manager will assume “SocIoTal” as the default domain.

| {                                                
                                                   
 "username":"User number 1",                       
                                                   
 "nickName":"user1",                               
                                                   
 "password":"passUser1",                           
                                                   
 "email": "user1@test.com",                        
                                                   
 "description":"User1 description here",           
                                                   
 "domain":"SocIoTal",                              
                                                   
 "address":{                                       
                                                   
 "addressType":"Work",                             
                                                   
 "addressStreetAddress":"User1Adress”,             
                                                   
 "addressLocality":"User1Locality",                
                                                   
 "addressPostalCode":"39005",                      
                                                   
 "addressCountry":"Spain"                          
                                                   
 },                                                
                                                   
 "org":"User1Organization",                        
                                                   
 "department":"User1Department",                   
                                                   
 "active":true                                     
                                                   
 }                                                 |
|--------------------------------------------------|
| Communities Manager registerUser payload example |

Further versions of the Communities Manager will define extra parameters and also allow user-defined ones. As a result, the registerUser method will return the identifiers (Ids) for the domain, user and community. These values have to be kept by the user in order to be used in future actions. The community Id corresponds to the id (not the name) of the private community of the user (automatically generated by the registerUser method), which will have the same name as the value of the user Id. This user private community is intended for new user’s purposes. Every new entity this user created in SocIoTal will be automatically registered within this community, if no other community is pointed. Initially, only the new created user will have access to his private community, until the user decides to include other identities or keep it for his/her own purposes.

### Communities creation

As mentioned before, a community (within SocIoTal) represents a set of users that shares access to a set of registered entities. A community will be represented by an ID (a UUID according RFC 4122 ), generated and managed by the Communities Manager, belonging to a specified domain (different domains will isolate different application areas for the SocIoTal Integrated Platform). This UUID will be attached to those entities registered within this community and the Communities Manager will grant access to these entities using the Community-Token, to those users belonging to the community.

Once a user has been registered within SocIoTal IdM (through Communities Manager Interface or directly using the SocIoTal IdM API, e.g. through the SocIoTal UserEnv.) this user have just created an identity belonging to a specified domain. Using this identity, the user will be able to create communities within this domain.

To create a community, the steps are:

1.  Ask Communities Manager for a token, identifying the user as member of the domain where the community wants to be created

2.  Once the user has been properly identified and its domain membership has been checked, the platform will retrieve the corresponding Community-Token UUID. This UUID must be added to the Communities Creation request header (as Community-Token header) before been sent to the platform. The payload of this request will include a description and a communityName. The creator ID (userId) and the domain will be extracted from the provided token. Further info could be provided to describe/complement the community in future versions.

3.  Now, the new community has been created. The initial ownership of this community will be assigned to the user whose identity Id (userId) was included in the Community-Token. Information about the Community, the Domain it belongs to and its owner is retrieved in the response from the Communities Creation request.

This community owner will now be able to request Community-Tokens linked to this community. These tokens will allow the user to add new users to the community, assign roles, register entities and later access these entities within the SocIoTal CM.

#### Adding members

Users membership within SocIoTal Communities is driven by Keyrock roles. A user is considered as part of a community when it has (at least) one role assigned. In order to evaluate the functionalities of communities’ management within SocIoTal allowing a quick deployment, only two roles are managed in first Communities Manager version: Owner and Member, but other roles can easily be defined and managed by the platform administrator, through the Keystone V3 API provided by the SocIoTal Keyrock instance.

-   **Member** role: this role allows the user to add resources/entities to the community and access (read) registered ones.

-   **Owner** role: in addition to Member privileges, it allows to add (and remove) new users, by their corresponding user Ids and assign/modify the roles they will have. An Owner role can make into an owner an existing member role and vice versa (a community may have as many owners as owners decide). Also, this role allows modifying the attributes (so far, the name and the description) of the communities owned, and even deletes them.

Every pair userId+role will be always linked to a community, whilst a userId could have different roles in the same community. This is, User A with role Owner in community X, role Member in community Y and no membership in community Z, can modify whatever he/she consider in community X, read entities (or register new ones) in community Y and no access at all to community Z resources nor users.

The method to add an existing user to a community is AssignRole. The corresponding payload will point the userId to add and the roleId this user will have within the community. To call this method, a Community-Token scoped to the desired community with owner role is required to be included on its header. In order to be added as a community member, a new user should first contact any community owner (contact ways can be specified in the “description” of the community or just adding a new attribute such “owner e-mail”).

#### Entities registration

Entities directory in SocIoTal is managed by the Context Manager, so entities registration must be done through its API. Integration between Context Manager and Communities Manager is done through the Community-Token: SocIoTal CM can read the Community-Token, in the header of every request, and evaluate this against the SocIoTal Communities Manager. This way, SocIoTal CM knows the user the request comes from, the community it belongs and the role played. Automatically, it adds the community identifier to the entity ID element of the request (either an update, a query, a discover, a subscribe or a delete action) causing an automatic filtering that restricts the scope of the action to the entities belonging to the community extracted from the token. Further interactions can also be linked to the role or the userId. In the case of new context/entity registration and update context (APPEND) action, the userId is extracted from the given Community-Token and automatically an “owner” attribute, containing this id is added to the new element registered. This way, only the user (identity) that initially registered the context entity will be able to update it, while keeps its community membership.

### Community-Token request

After user registration, request a Community-Token should be the next step. Community-Tokens can be used for two main purposes:

-   User identification/authentication: when requesting a Community-Token, a pair user/password must be provided. This pair will be used against the KeyRock to validate the user. Obtaining a valid token means the pair user/password is correct, so the user has been authenticated. All Community-Tokens complies with this functionality.

-   Community/Domain membership checking: to perform this functionality, the token must include information related to the community and the domain it belongs to. In order to achieve this, when requesting the token, it must be scoped: this is, in addition to the pair username/password, the domain and/or the community should be pointed. This way, if the user is properly authenticated and is member of the pointed domain and/or community, the token will also include information about the specified domain, community and role the user plays on it.

    -   If only “domain” is specified, the obtained token will refer only to this domain (no reference to communities or roles will be added). This token is useful to create/list communities within the given domain.

    -   A Community-Token with info related to communities and roles is required to perform any action related to users/entities management, and also to modify/delete a community.

The method to request a Community-Token is token/request. Its payload permits to include, besides the username and password (mandatory), the domainName and the communityName, allowing so to scope the requested token. As a result, the complete Community-Token plus its UUID (to be included in further requests) are retrieved.

### Information access request (to SocIoTal Context Manager)

Although other SocIoTal components or external applications can make use of the Community-Token to manage the access to services provided, here is considered the integration with the SocIoTal Context Manager, in order to control the access to entities stored in the SocIoTal Entities Directory.This part focuses on accessing a registered entity belonging to a community.

#### Reading information from a registered device

Considering, as the starting point, a user (US1) with an identity already registered and belonging to an already created community (COM1) grouping a set of already registered (in the Context Manager and as part of COM1) entities, from where we can select one (COM1:E1). How to achieve these steps is detailed in previous sections.

To interact with SocIoTal Context Manager (supposing the case communities are used) a Community-Token is required. In order US1 wants to read (execute a “queryContext” method) the process is:

1.  US1 must first obtain a Community-Token from the Communities Manager scoped to COM1. To do this, a HTTP POST to “/SocIoTal\_COMM\_V1/TOKEN/request” method must be called, defining the scope:

| {                        
                           
 "name":"US1",             
                           
 "password":"passUS1",     
                           
 "communityName": "COM1",  
                           
 "domainName":"SocIoTal"   
                           
 }                         |
|--------------------------|

1.  If US1 is properly authenticated, Communites Manager will retrieve to the requestor a Community-Token, valid for the pointed community (COM1) in the selected domain (SocIoTal) that identifies US1 as the requestor user. What is interesting here is the UUID of this Community-Token (e.g. fff4ac1b24024ff181a391c835738a6f).

2.  Now US1 has obtained a valid Community-Token, the “queryContext” method call should be performed as pointed in [*SocIoTal WiKi*](https://github.com/sociotal/SOCIOTAL/wiki/SocIoTal-Communities-Manager), including the selected entityID. Plus, the Community-Token obtained UUID must be added to the Community-Token header of the request before calling.

3.  When SocIoTal CM receives the request, it validates the Capability-Token UUID captured, identifying the requestor user (US1), the community the token refers to (COM1) and the role attached (in order to read entities data, so far, all managed roles will be allowed. Further versions may use this attribute to filter actions).

4.  SocIoTal CM will modify the selected entity ID to add, as a prefix to this ID, the community Id where the selected entity (COM1:E1) was registered and executes the corresponding query to the SocIoTal’s Entities Resource. This way, if COM1:E1 was registered using a COM1 Community-Token, only members able to get a COM1 scoped token will read this entity.

5.  Info retrieved by SocIoTal CM from the Entities Directory is filtered and delivered to requestor US1.

A similar process is followed when discovering or subscribing methods are called from a requestor.

#### Modifying/Updating data of a registered device

As mentioned before, all the mechanisms involving entities are somehow managed by the SocIoTal CM. The Community-Token is the integration key to add communities support to CM. In current implementations of SocIoTal CM and Communities Manager, to read entities registered within a given community, only extracted communityId parameter is used (userId and/or role can be used e.g. in internal log files to register accesses). To modify (including delete action) existing entities, apart from the communityId, the attribute “owner”, defined when registering a new entity is required.

The attribute “owner” stores the Id (userId) of the identity that initially registered the entity. In order to perform an “updateContext” method, the Community-Token userId included must match the userId set as the “owner” of the entity. This owner attribute can also be modified by the original entity creator, setting as the new owner any user member of the community. If the “owner” is set to “none”, every community member will be able to update the entity.
