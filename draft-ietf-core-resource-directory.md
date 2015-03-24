---
stand_alone: true
ipr: trust200902
docname: draft-ietf-core-resource-directory-03pre
cat: std
pi:
  strict: 'yes'
  toc: 'yes'
  tocdepth: '3'
  symrefs: 'yes'
  sortrefs: 'yes'
  compact: 'yes'
  subcompact: 'no'
  iprnotified: 'no'
title: CoRE Resource Directory
area: Internet
wg: CoRE
kw: CoRE, Web Linking, Resource Discovery, Resource Directory
date: '2015-03-24'
author:
- ins: Z. Shelby
  name: Zach Shelby
  org: ARM
  street: 150 Rose Orchard
  city: San Jose
  code: '95134'
  country: USA
  phone: "+1-408-203-9434"
  email: zach.shelby@arm.com
- ins: M. Koster
  name: Michael Koster
  org: ARM
- ins: C. Bormann
  name: Carsten Bormann
  org: Universitaet Bremen TZI
  street: Postfach 330440
  city: Bremen
  code: D-28359
  country: Germany
  phone: "+49-421-218-63921"
  email: cabo@tzi.org
normative:
  RFC6690:
  RFC2119:
  RFC3986:
  RFC5226:
  RFC5988:
  RFC6335:
  RFC6570:
  RFC6763:
  I-D.ietf-core-links-json:
informative:
  RFC7252:
  I-D.ietf-core-groupcomm:
  RFC6775:
  RFC7230:
  RFC3629:
  RFC5198:
  RFC1123:
  RFC1034:

--- abstract

In many M2M applications, direct discovery of resources is not practical
due to sleeping nodes, disperse networks, or networks where multicast traffic
is inefficient. These problems can be solved by employing an entity called
a Resource Directory (RD), which hosts descriptions of resources held on
other servers, allowing lookups to be performed for those resources. This
document specifies the web interfaces that a Resource Directory supports
in order for web servers to discover the RD and to register, maintain, lookup
and remove resources descriptions. Furthermore, new link attributes useful
in conjunction with an RD are defined.

--- middle

# Introduction {#introduction}

The work on Constrained RESTful Environments (CoRE) aims at realizing the
REST architecture in a suitable form for the most constrained nodes (e.g.,
8-bit microcontrollers with limited RAM and ROM) and networks (e.g. 6LoWPAN).
CoRE is aimed at machine-to-machine (M2M) applications such as smart energy
and building automation.

The discovery of resources offered by a constrained server is very important
in machine-to-machine applications where there are no humans in the loop and
static interfaces result in fragility. The discovery of resources provided by
an HTTP Web Server is typically called Web Linking {{RFC5988}}. The use of
Web Linking for the description and discovery of resources hosted by
constrained web servers is specified by the CoRE Link Format
{{RFC6690}}. This specification however only describes how to discover
resources from the web server that hosts them by requesting
`/.well-known/core`. In many M2M scenarios, direct discovery of resources is
not practical due to sleeping nodes, disperse networks, or networks where
multicast traffic is inefficient. These problems can be solved by employing
an entity called a Resource Directory (RD), which hosts descriptions of
resources held on other servers, allowing lookups to be performed for those
resources.

This document specifies the web interfaces that a Resource Directory supports
in order for web servers to discover the RD and to register, maintain, lookup
and remove resource descriptions. Furthermore, new link attributes useful in
conjunction with a Resource Directory are defined. Although the examples in
this document show the use of these interfaces with CoAP {{RFC7252}}, they
can be applied in an equivalent manner to HTTP {{RFC7230}}.


# Terminology {#terminology}

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
"SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL"
in this
document are to be interpreted as described in {{RFC2119}}. The
term "byte" is used in its now customary sense as a synonym for "octet".

This specification requires readers to be familiar with all the terms and
concepts that are discussed in {{RFC5988}} and {{RFC6690}}. Readers should
also be familiar with the terms and concepts discussed in {{RFC7252}}.  To
describe the REST interfaces defined in this specification, the URI Template
format is used {{RFC6570}}.

This specification makes use of the following additional terminology:

Resource Directory
:   A web entity that stores information about web resources and implements the
REST interfaces defined in this specification for registration and lookup
of those resources.
{: vspace='0'}

Domain
:   In the context of a Resource Directory, a domain is a
logical grouping of endpoints. This specification assumes
that the list of Domains supported by an RD is
pre-configured by that RD. When a domain is exported to DNS,
the domain value equates to the DNS domain name.
{: vspace='0'}

Group
:   In the context of a Resource Directory, a group is a logical grouping of
endpoints for the purpose of group communications. All groups within a domain
are unique.
{: vspace='0'}

Endpoint
:   Endpoint (EP) is a term used to describe a web server or client in {{RFC7252}}. In the context of this specification an endpoint is used to describe a
web server that registers resources to the Resource Directory. An endpoint
is identified by its endpoint name, which is included during registration,
and is unique within the associated domain of the registration.
{: vspace='0'}


# Architecture and Use Cases {#arch}

The resource directory architecture is illustrated in {{fig-arch}}. A
Resource Directory (RD) is used as a repository for Web Links {{RFC5988}}
about resources hosted on other web servers, which are called endpoints
(EP).
An endpoint is a web server associated with a scheme, IP address and port
(called Context), thus a physical node may host one or more endpoints. The
RD implements a set of REST interfaces for endpoints to register and maintain
sets of Web Links (called resource directory entries), and for clients to
lookup resources from the RD or maintain groups. Endpoints themselves can
also act as clients. An RD can be logically segmented by the use of Domains.
The domain an endpoint is associated with can be defined by the RD or configured
by an outside entity. This information hierarchy is shown in {{fig-hierarchy}}.

Endpoints are assumed to proactively register and maintain resource directory
entries on the RD, which are soft state and need to be periodically refreshed.
An endpoint is provided with interfaces to register, update and remove a
resource directory entry. Furthermore, a mechanism to discover an RD using
the CoRE Link Format is defined. It is also possible for an RD to proactively
discover Web Links from endpoints and add them as resource directory entries.
A lookup interface for discovering any of the Web Links held in the RD is
provided using the CoRE Link Format.

~~~~
             Registration     Lookup, Group
  +----+          |                 |
  | EP |----      |                 |
  +----+    ----  |                 |
                --|-    +------+    |
  +----+          | ----|      |    |     +--------+
  | EP | ---------|-----|  RD  |----|-----| Client |
  +----+          | ----|      |    |     +--------+
                --|-    +------+    |
  +----+    ----  |                 |
  | EP |----      |                 |
  +----+

~~~~
{: #fig-arch title='The resource directory architecture.' align="left"}

~~~~
               +------------+
               |   Domain   | <-- Name
               +------------+
                    |     |
                    |   +------------+
                    |   |   Group    | <-- Name, IP
                    |   +------------+
                    |     |
               +------------+
               |  Endpoint  |  <-- Name, Scheme, IP, Port
               +------------+
                     |
                     |
               +------------+
               |  Resource  |  <-- Target, Parameters
               +------------+

~~~~
{: #fig-hierarchy title='The resource directory information hierarchy.' align="left"}

## Use Case: Cellular M2M {#cellular}

Over the last few years, mobile operators around the world
have focused on development of M2M solutions in order to
expand the business to the new type of users: machines. The
machines are connected directly to a mobile network using an appropriate
embedded air interface (GSM/GPRS, WCDMA, LTE) or via a gateway providing
short and wide range wireless interfaces. From the system design point of
view, the ambition is to design horizontal solutions that can enable utilization
of machines in different applications depending on their current availability
and capabilities as well as application requirements, thus avoiding silo
like solutions. One of the crucial enablers of such design is the ability
to discover resources (machines --- endpoints) capable of providing required
information at a given time or acting on instructions from the end users.

In a typical scenario, during a boot-up procedure (and periodically afterwards),
the machines (endpoints) register with a Resource Directory (for example
EPs installed on vehicles enabling tracking of their position for fleet management
purposes and monitoring environment parameters) hosted by the mobile operator
or somewhere else in the network, periodically a description of its own capabilities.
Due to the usual network configuration of mobile networks, the EPs attached
to the mobile network do not have routable addresses. Therefore, a remote
server is usually used to provide proxy access to the EPs. The address of
each (proxy) endpoint on this server is included in the resource description
stored in the RD. The users, for example mobile applications for environment
monitoring, contact the RD, look-up the endpoints capable of providing information
about the environment using appropriate set of link parameters, obtain information
on how to contact them (URLs of the proxy server) and then initiate interaction
to obtain information that is finally processed, displayed on the screen
and usually stored in a database. Similarly, fleet management systems provide
the appropriate link parameters to the RD to look-up for EPs deployed on
the vehicles the application is responsible for.


## Use Case: Home and Building Automation {#automation}

Home and commercial building automation systems can benefit from the use
of M2M web services.  The discovery requirements of these applications are
demanding. Home automation usually relies on run-time discovery to commission
the system, whereas in building automation a combination of professional
commissioning and run-time discovery is used. Both home and building automation
involve peer-to-peer interactions between endpoints, and involve battery-powered
sleeping devices.

The exporting of resource information to other discovery systems is also
important in these automation applications. In home automation there is a
need to interact with other consumer electronics, which may already support
DNS-SD, and in building automation larger resource directories or DNS-SD
covering multiple buildings.


## Use Case: Link Catalogues {#usecase-catalogues}

Resources may be shared through data brokers that have no knowledge beforehand
of who is going to consume the data. Resource Directory can be used to hold
links about resources and services hosted anywhere to make them discoverable
by a general class of applications.

For example, environmental and weather sensors that generate data for public
consumption may provide the data to an intermediary server, or broker. Sensor
data are published to the intermediary upon changes or at regular intervals.
Descriptions of the sensors that resolve to links to sensor data may be published
to a Resource Directory. Applications wishing to consume the data can use
the Resource Directory lookup function set to discover and resolve links
to the desired resources and endpoints. The Resource Directory service need
not be coupled with the data intermediary service. Mapping of Resource Directories
to data intermediaries may be many-to-many.

Metadata in link-format or link-format+json representations are supplied
by Resource Directories, which may be internally stored as  triples, or relation/attribute
pairs providing metadata about resource links. External catalogs that are
represented in other formats may be converted to link-format or link-format+json
for storage and access by Resource Directories. Since it is common practice
for these to be URN encoded, simple and lossless structural transforms will
generally be sufficient to store external metadata in Resource Directories.

The additional features of Resource Directory allow domains to be defined
to enable access to a particular set of resources from particular applications.
This provides isolation and protection of sensitive data when needed. Resource
groups may defined to allow batched reads from multiple resources.


# Simple Directory Discovery {#simple}

Not all endpoints hosting resources are expected to know how to implement the
Resource Directory Function Set (see {{rd}}) and thus explicitly register
with a Resource Directory (or other such directory server). Instead, simple
endpoints can implement the generic Simple Directory Discovery approach
described in this section. An RD implementing this specification MUST
implement Simple Directory Discovery. However, there may be security reasons
why this form of directory discovery would be disabled.

This approach requires that the endpoint makes available the hosted resources
that it wants to be discovered, as links on its `/.well-known/core` interface as
specified in {{RFC6690}}.

The endpoint then finds one or more IP addresses of the directory server it
wants to know about its resources as described in {{simple_finding}}.

An endpoint that wants to make itself discoverable occasionally
sends a POST request to the `/.well-known/core` URI of any candidate directory
server that it finds. The body of the POST request is either

* empty, in which case the directory server is encouraged by this POST
  request to perform GET requests at the requesting server's default discovery
  URI.

or

* a non-empty link-format document, which indicates the specific services
  that the requesting server wants to make known to the directory server.

The directory server integrates the information it received this way into its
resource directory.  It MAY make the information available to further
directories, if it can ensure that a loop does not form.  The protocol used
between directories to ensure loop-free operation is outside the scope of
this document.

The following example shows an endpoint using simple resource discovery,
by simply sending a POST with its links in the body to a directory.

~~~~
     EP                                               RD
     |                                                 |
     | -- POST /.well-known/core "</sen/temp>..." ---> |
     |                                                 |
     |                                                 |
     | <---- 2.01 Created   -------------------------  |
     |                                                 |
~~~~
{: align="left"}

## Finding a Directory Server {#simple_finding}

Endpoints that want to contact a directory server can obtain candidate IP
addresses for such servers in a number of ways.

In a 6LoWPAN, good candidates can be taken from:

* specific static configuration (e.g., anycast addresses), if any,

* the ABRO option of 6LoWPAN-ND {{RFC6775}},

* other ND options that happen to point to servers (such as RDNSS),

* DHCPv6 options that might be defined later.

In networks with more inexpensive use of multicast, the candidate IP
address may be a well-known multicast address, i.e. directory servers are
found by simply sending POST requests to that well-known multicast address
(details TBD).

As some of these sources are just (more or less educated) guesses, endpoints
MUST make use of any error messages to very strictly rate-limit requests to
candidate IP addresses that don't work out.  For example, an ICMP Destination
Unreachable message (and, in particular, the port unreachable code for this
message) may indicate the lack of a CoAP server on the candidate host, or a
CoAP error response code such as 4.05 "Method Not Allowed" may indicate
unwillingness of a CoAP server to act as a directory server.


## Third-party registration {#third-party-registration}

For some applications, even Simple Directory Discovery may be too taxing
for certain very constrained devices, in particular if the security requirements
become too onerous.

In a controlled environment (e.g. building control), the Resource Directory
can be filled by a third device, called an installation tool. The installation
tool can fill the Resource Directory from a database or other means. For
that purpose the scheme, IP address and port of the registered device is
indicated in the Context parameter of the registration as well.


# Resource Directory Function Set {#rd}

This section defines the REST interfaces between an RD and endpoints, which
is called the Resource Directory Function Set. Although the examples
throughout this section assume the use of CoAP {{RFC7252}}, these REST
interfaces can also be realized using HTTP {{RFC7230}}. An RD implementing
this specification MUST support the discovery, registration, update, lookup,
and removal interfaces defined in this section.

Resource directory entries are designed to be easily exported to other
discovery mechanisms such as DNS-SD. For that reason, parameters that would
meaningfully be mapped to DNS SHOULD be limited to a maximum length of 63
bytes.<!-- TODO: Is there maybe also a need to further restrict the set of
characters available?  -->

## Discovery {#discovery}

Before an endpoint can make use of an RD, it must first know the RD's IP
address, port and the path of its RD Function Set. There can be several
mechanisms for discovering the RD including assuming a default location
(e.g. on an Edge Router in a LoWPAN), by assigning an anycast address to the
RD, using DHCP, or by discovering the RD using the CoRE Link Format (see also
{{simple_finding}}). This section defines discovery of the RD using the
well-known interface of the CoRE Link Format {{RFC6690}} as the required
mechanism. It is however expected that RDs will also be discoverable via
other methods depending on the deployment.

Discovery is performed by sending either a multicast or unicast GET request
to `/.well-known/core` and including a Resource Type (rt) parameter
{{RFC6690}} with the value "core.rd" in the query string. Likewise, a
Resource Type parameter value of "core.rd-lookup" is used to discover the RD
Lookup Function Set.  Upon success, the response will contain a payload with
a link format entry for each RD discovered, with the URL indicating the root
resource of the RD. When performing multicast discovery, the multicast IP
address used will depend on the scope required and the multicast capabilities
of the network.

An RD implementation of this specification MUST support query filtering for
the rt parameter as defined in {{RFC6690}}.

The discovery request interface is specified as follows:

Interaction:
: EP -> RD

Method:
: GET

URI Template:
: /.well-known/core{?rt}

URI Template Variables:
: rt :=
  : Resource Type (optional). MAY contain the value "core.rd", "core.rd-lookup",
  "core.rd-group" or "core.rd\*"

Content-Type:
: application/link-format (if any)

The following response codes are defined for this interface:

Success:
: 2.05 "Content" with an
  application/link-format payload containing one or more matching entries for
  the RD resource.

Failure:
: 4.04 "Not Found" is returned in case no matching entry is found for a unicast
  request.

Failure:
: 4.00 "Bad Request" is returned in case of a malformed request for a unicast
  request.

Failure:
: No error response to a multicast request.

The following example shows an endpoint discovering an RD using this interface,
thus learning that the base RD resource is, in this example, at /rd.  Note
that it is up to the RD to choose its base RD resource, although diagnostics
and debugging is facilitated by using the base paths specified here where
possible.

~~~~
     EP                                               RD
     |                                                 |
     | ----- GET /.well-known/core?rt=core.rd* ------> |
     |                                                 |
     |                                                 |
     | <---- 2.05 Content "</rd>; rt="core.rd" ------  |
     |                                                 |


~~~~
{: align="left"}

~~~~
Req: GET coap://[ff02::1]/.well-known/core?rt=core.rd*

Res: 2.05 Content
</rd>;rt="core.rd",
</rd-lookup>;rt="core.rd-lookup",
</rd-group>;rt="core.rd-group"
~~~~
{: align="left"}


## Registration {#registration}

After discovering the location of an RD Function Set, an endpoint MAY
register its resources using the registration interface. This interface
accepts a POST from an endpoint containing the list of resources to be added
to the directory as the message payload in the CoRE Link Format {{RFC6690}}
or JSON Link Format {{I-D.ietf-core-links-json}} along with query string
parameters indicating the name of the endpoint, its domain and the lifetime
of the registration. All parameters except the endpoint name are optional. It
is expected that other specifications will define further parameters (see
{{iana-registry}}). The RD then creates a new resource or updates an existing
resource in the RD and returns its location. An endpoint MUST use that
location when refreshing registrations using this interface. Endpoint
resources in the RD are kept active for the period indicated by the lifetime
parameter. The endpoint is responsible for refreshing the entry within this
period using either the registration or update interface. The registration
interface MUST be implemented to be idempotent, so that registering twice
with the same endpoint parameter does not create multiple RD entries.

The registration request interface is specified as follows:

Interaction:
: EP -> RD


Method:
: POST


URI Template:
: /{+rd}{?ep,d,et,lt,con}


URI Template Variables:
: rd :=
  : RD Function Set
    path (mandatory). This is the path of
    the RD Function Set, as obtained from discovery. An RD SHOULD use the value
    "rd" for this variable whenever possible.

  ep :=
  : Endpoint (mandatory). The endpoint identifier or name of the registering
    node, unique within that domain. The maximum length of this parameter is
    63 bytes.

  d :=
  : Domain (optional). The domain to which this endpoint belongs. This parameter
    SHOULD be less than 63 bytes. Optional. When this parameter is elided, the
    RD MAY associate the endpoint with a configured default domain. The domain
    value is needed to export the endpoint to DNS-SD (see {{dns-sd}}).

  et :=
  : Endpoint Type (optional). The semantic type of the endpoint. This parameter
    SHOULD be less than 63 bytes. Optional.

  lt :=
  : Lifetime (optional). Lifetime of the registration in seconds. Range of 60-4294967295.
    If no lifetime is included, a default value of 86400 (24 hours) SHOULD be
    assumed.

  con :=
  : Context (optional). This parameter sets the scheme, address and port at
    which this server is available in the form scheme://host:port. Optional. In
    the absence of this parameter the scheme of the protocol, source IP address
    and source port of the register request are assumed. This parameter is
    mandatory when the directory is filled by a third party such as an
    installation tool.

Content-Type:
: application/link-format

Content-Type:
: application/link-format+json

The following response codes are defined for this interface:

Success:
: 2.01 "Created". The Location header
  MUST be included with the new resource entry for the
  endpoint. This Location MUST be a stable identifier
  generated by the RD as it is used for all subsequent
  operations on this registration. The resource returned in
  the Location is only for the purpose of the Update (POST)
  and Removal (DELETE), and MUST NOT implement GET or PUT
  methods.


Failure:
: 4.00 "Bad Request". Malformed request.


Failure:
: 5.03 "Service Unavailable". Service could not perform the operation.

The following example shows an endpoint with the name "node1" registering
two resources to an RD using this interface. The resulting location /rd/4521
is just an example of an RD generated location.

~~~~
    EP                                                RD
     |                                                 |
     | --- POST /rd?ep=node1 "</sensors..." ------->   |
     |                                                 |
     |                                                 |
     | <-- 2.01 Created Location: /rd/4521 ----------  |
     |                                                 |
~~~~
{: align="left"}

~~~~
Req: POST coap://rd.example.com/rd?ep=node1
Payload:
</sensors/temp>;ct=41;rt="temperature-c";if="sensor",
</sensors/light>;ct=41;rt="light-lux";if="sensor"

Res: 2.01 Created
Location: /rd/4521
~~~~
{: align="left"}


## Update {#update}

The update interface is used by an endpoint to refresh or update its
registration with an RD. To use the interface, the endpoint sends a POST
request to the resource returned in the Location option in the response to
the first registration.  An update MAY update the lifetime or context
parameters if they have changed since the last registration or
update. Parameters that have not changed SHOULD NOT be included in an
update. Upon receiving an update request, the RD resets the timeout for that
endpoint and updates the scheme, IP address and port of the endpoint (using
the source address of the update, or the context parameter if present).

An update MAY optionally add or replace links for the endpoint by including
those links in the payload of the update as a CoRE Link Format
document. Including links in an update message greatly increases the load on
an RD and SHOULD be done infrequently. A link is replaced only if both the
target URI and relation type match (see {{endpoint_identification}}).<!--
TODO: explain how a patch format, probably not RFC7386, could be used to
delete, replace, and add entries.)  -->

The update request interface is specified as follows:

Interaction:
: EP -> RD

Method:
: POST

URI Template:
: /{+location}{?lt,con}


URI Template Variables:
: location :=
  : This is the Location path returned by the RD as a result of a successful
    earlier registration.

  lt :=
  : Lifetime (optional). Lifetime of the registration in seconds. Range of 60-4294967295.
    If no lifetime is included, a default value of 86400 (24 hours) SHOULD be
    assumed.


  con :=
  : Context (optional). This parameter sets the scheme, address and port at
    which this server is available in the form
    scheme://host:port. Optional. In the absence of this parameter the scheme
    of the protocol, source IP address and source port used to register are
    assumed. This parameter is compulsory when the directory is filled by a
    third party such as an installation tool.

Content-Type:
: application/link-format (optional)

Content-Type:
: application/link-format+json (optional)


The following response codes are defined for this interface:

Success:
: 2.04 "Changed" in the update was successfully processed.

Failure:
: 4.00 "Bad Request". Malformed request.

Failure:
: 4.04 "Not Found". Registration does not exist (e.g. may have expired).

Failure:
: 5.03 "Service Unavailable". Service could not perform the operation.


The following example shows an endpoint updating a new set of resources to
an RD using this interface.


~~~~
     EP                                                RD
     |                                                 |
     | --- POST /rd/4521  -------------------------->   |
     |                                                 |
     |                                                 |
     | <-- 2.04 Changed  ----------------------------  |
     |                                                 |
~~~~
{: align="left"}


~~~~
Req: POST /rd/4521

Res: 2.04 Changed
~~~~
{: align="left"}


## Removal {#removal}

Although RD entries have soft state and will eventually timeout after their
lifetime, an endpoint SHOULD explicitly remove its entry from the RD if it
knows it will no longer be available (for example on shut-down). This is
accomplished using a removal interface on the RD by performing a DELETE on
the endpoint resource.

The removal request interface is specified as follows:

Interaction:
: EP -> RD

Method:
: DELETE

URI Template:
: /{+location}

URI Template Variables:
: location :=
  : This is the Location path returned by the RD as a result of a successful
    earlier registration.

The following responses codes are defined for this interface:

Success:
: 2.02 "Deleted" upon successful deletion

Failure:
: 4.00 "Bad Request". Malformed request.

Failure:
: 4.04 "Not Found". Registration does not exist (e.g. may have expired).

Failure:
: 5.03 "Service Unavailable". Service could not perform the operation.

The following examples shows successful removal of the endpoint from the RD.


~~~~
    EP                                                RD
     |                                                 |
     | --- DELETE /rd/4521  ------------------------>  |
     |                                                 |
     |                                                 |
     | <-- 2.02 Deleted  ----------------------------  |
     |                                                 |
~~~~
{: align="left"}


~~~~
Req: DELETE /rd/4521

Res: 2.02 Deleted
~~~~
{: align="left"}



# Group Function Set {#group}

This section defines a function set for the creation of groups of endpoints
for the purpose of managing and looking up endpoints for group operations.
The group function set is similar to the resource directory function set,
in that a group may be created or removed. However unlike an endpoint entry,
a group entry consists of a list of endpoints and does not have a lifetime
associated with it. In order to make use of multicast requests with CoAP,
a group MAY have a multicast address associated with it.

## Register a Group {#group-register}

In order to create a group, a management entity used to configure groups,
makes a request to the RD indicating the name of the group to create (or
update), optionally the domain the group belongs to, and optionally the multicast
address of the group. The registration message includes the list of endpoints
that belong to that group. If an endpoint has already registered with the
RD, the RD attempts to use the context of the endpoint from its RD endpoint
entry. If the client registering the group knows the endpoint has already
registered, then it MAY send a blank target URI for that endpoint link when
registering the group. Configuration of the endpoints themselves is out of
scope of this specification. Such an interface for managing the group membership
of an endpoint has been defined in {{I-D.ietf-core-groupcomm}}.

The registration request interface is specified as follows:

Interaction:
: Manager -> RD

Method:
: POST

URI Template:
: /{+rd-group}{?gp,d,con}

URI Template Variables:
: rd-group :=
  : RD Group Function Set path (mandatory). This is the path of the RD Group
    Function Set. An RD SHOULD use the value "rd-group" for this variable whenever
    possible.

  gp :=
  : Group Name (mandatory). The name of the group to be created or replaced,
    unique within that domain. The maximum length of this parameter is 63 bytes.

  d :=
  : Domain (optional). The domain to which this group belongs. The maximum
    length of this parameter is 63 bytes. Optional. When this parameter is
    elided, the RD MAY associate the endpoint with a configured default
    domain. The domain value is needed to export the endpoint to DNS-SD (see
    {{dns-sd}})

  con :=
  : Context (optional). This parameter is used to set the IP multicast
    address at which this server is available in the form
    scheme://multicast-address:port.  Optional. In the absence of this parameter
    no multicast address is configured.  This parameter is compulsory when the
    directory is filled by an installation tool.

Content-Type:
: application/link-format

Content-Type:
: application/link-format+json


The following response codes are defined for this interface:

Success:
: 2.01 "Created". The Location header MUST be included with the new group
  entry.  This Location MUST be a stable identifier generated by the RD as it
  is used for delete operations on this registration.

Failure:
: 4.00 "Bad Request". Malformed request.

Failure:
: 5.03 "Service Unavailable". Service could not perform the operation.


The following example shows a group with the name "lights" registering two
endpoints to an RD using this interface. The resulting location /rd-group/12
is just an example of an RD generated group location.


~~~~
    EP                                                RD
     |                                                 |
     | - POST /rd-group?gp=lights "<>;ep=node1..." --> |
     |                                                 |
     |                                                 |
     | <---- 2.01 Created Location: /rd-group/12 ----  |
     |                                                 |
~~~~
{: align="left"}


~~~~
Req: POST coap://rd.example.com/rd-group?gp=lights
Payload:
<>;ep="node1",
<>;ep="node2"

Res: 2.01 Created
Location: /rd-group/12
~~~~
{: align="left"}


## Group Removal {#group-removal}

A group can be removed simply by sending a removal message to the location
returned when registering the group. Removing a group MUST NOT remove the
endpoints of the group from the RD.

The removal request interface is specified as follows:

Interaction:
: Manager -> RD

Method:
: DELETE

URI Template:
: /{+location}


URI Template Variables:
: location :=
  : This is the Location path returned by the RD as a result of a successful
    group registration.

The following responses codes are defined for this interface:

Success:
: 2.02 "Deleted" upon successful deletion

Failure:
: 4.00 "Bad Request". Malformed request.

Failure:
: 4.04 "Not Found". Group does not exist.

Failure:
: 5.03 "Service Unavailable". Service could not perform the operation.


The following examples shows successful removal of the group from the RD.


~~~~
    EP                                                RD
     |                                                 |
     | --- DELETE /rd-group/412  ------------------->  |
     |                                                 |
     |                                                 |
     | <-- 2.02 Deleted  ----------------------------  |
     |                                                 |
~~~~
{: align="left"}


~~~~
Req: DELETE /rd-group/12

Res: 2.02 Deleted
~~~~
{: align="left"}



# RD Lookup Function Set {#lookup}

In order for an RD to be used for discovering resources registered with it,
a lookup interface can be provided using this function set. This lookup interface
is defined as a default, and it is assumed that RDs may also support lookups
to return resource descriptions in alternative formats (e.g. Atom or HTML
Link) or using more advanced interfaces (e.g. supporting context or semantic
based lookup).

This function set allows lookups for domains, groups, endpoints and resources
using attributes defined in the RD Function Set and for use with the CoRE
Link Format. The result of a lookup request is the list of links (if any)
corresponding to the type of lookup.  Using the Accept Option, the requester
can control whether this list is returned in CoRE Link Format
(`application/link-format`, default) or its JSON form
(`application/link-format+json`).  The target of these links SHOULD be the
actual location of the domain, endpoint or resource, but MAY be an
intermediate proxy e.g. in the case of an HTTP lookup interface for CoAP
endpoints. Multiple query parameters MAY be included in a lookup, all
included parameters MUST match for a resource to be returned.  The character
'\*' MAY be included at the end of a parameter value as a wildcard operator.

The lookup interface is specified as follows:

Interaction:
: Client -> RD

Method:
: GET

URI Template:
: /{+rd-lookup-base}/{lookup-type}{?d,ep,gp,et,rt,page,count,resource-param}


URI Template Variables:
: rd-lookup-base :=
  : RD Lookup Function Set path (mandatory). This is the path of the RD Lookup
    Function Set. An RD SHOULD use the value "rd-lookup" for this variable whenever
    possible.


  lookup-type :=
  : ("d", "ep", "res", "gp") (mandatory) This variable is used to select the
    kind of lookup to perform (domain, endpoint, resource, or group).

  ep :=
  : Endpoint (optional). Used for endpoint, group and resource lookups.

  d :=
  : Domain (optional). Used for domain, group, endpoint and resource lookups.

  page :=
  : Page (optional). Parameter can not be used without the count
    parameter. Results are returned from result set in pages that contains
    'count' results starting from index (page \* count).

  count :=
  : Count (optional). Number of results is limited to this parameter value. If
    the parameter is not present, then an RD implementation specific default
    value SHOULD be used.

  rt :=
  : Resource type (optional). Used for group, endpoint and resource lookups.

  et :=
  : Endpoint type (optional). Used for group, endpoint and resource lookups.


  resource-param :=
  : Link attribute parameters (optional). Any link attribute as defined in Section
    4.1 of {{RFC6690}}, used for resource lookups.

The following responses codes are defined for this interface:

Success:
: 2.05 "Content" with an `application/link-format` or
  `application/link-format+json` payload containing a matching entries for
  the lookup.

Failure:
: 4.04 "Not Found" in case no matching entry is found for a unicast request.

Failure:
: No error response to a multicast request.

Failure:
: 4.00 "Bad Request". Malformed request.

Failure:
: 5.03 "Service Unavailable". Service could not perform the operation.

The following example shows a client performing a resource lookup:


~~~~
   Client                                                          RD
     |                                                             |
     | ----- GET /rd-lookup/res?rt=temperature ----------------->  |
     |                                                             |
     |                                                             |
     | <-- 2.05 Content <coap://{host:port}/temp>;rt="temperature" |
     |                                                             |
~~~~
{: align="left"}


~~~~
Req: GET /rd-lookup/res?rt=temperature

Res: 2.05 Content
<coap://{host:port}/temp>;rt="temperature"
~~~~
{: align="left"}

The following example shows a client performing an endpoint lookup:


~~~~
   Client                                                          RD
     |                                                             |
     | ----- GET /rd-lookup/ep?et=power-node -------------------->  |
     |                                                             |
     |                                                             |
     | <-- 2.05 Content <coap://{ip:port}>;ep="node5" ------------ |
     |                                                             |
~~~~
{: align="left"}


~~~~
Req: GET /rd-lookup/ep?et=power-node

Res: 2.05 Content
<coap://{ip:port}>;ep="node5",
<coap://{ip:port}>;ep="node7"
~~~~
{: align="left"}

The following example shows a client performing a domain lookup:


~~~~
   Client                                                          RD
     |                                                             |
     | ----- GET /rd-lookup/d ---------------------------------->  |
     |                                                             |
     |                                                             |
     | <-- 2.05 Content </rd>;d=domain1,</rd>;d=domain2 ---------- |
     |                                                             |
~~~~
{: align="left"}


~~~~
Req: GET /rd-lookup/d

Res: 2.05 Content
</rd>;d="domain1",
</rd>;d="domain2"
~~~~
{: align="left"}

The following example shows a client performing a group lookup for all groups:


~~~~
   Client                                                          RD
     |                                                             |
     | ----- GET /rd-lookup/gp --------------------------------->  |
     |                                                             |
     |                                                             |
     | <-- 2.05 Content </rd-group/12>;gp="lights1"; ------------- |
     |                               d="example.com" ------------- |
     |                                                             |
~~~~
{: align="left"}


~~~~
Req: GET /rd-lookup/gp

Res: 2.05 Content
</rd-group/12>;gp="lights1";d="example.com"
~~~~
{: align="left"}

The following example shows a client performing a lookup for all endpoints
in a particular group:


~~~~
   Client                                                          RD
     |                                                             |
     | ----- GET /rd-lookup/ep?gp=lights1----------------------->  |
     |                                                             |
     |                                                             |
     | <-- 2.05 Content <coap://{host:port}>;ep="node1" ---------- |
     |                                                             |
~~~~
{: align="left"}


~~~~
Req: GET /rd-lookup/ep?gp=lights1

Res: 2.05 Content
<coap://{host:port}>;ep="node1",
<coap://{host:port}>;ep="node2",
~~~~
{: align="left"}

The following example shows a client performing a lookup for all groups an
endpoint belongs to:


~~~~
   Client                                                          RD
     |                                                             |
     | ----- GET /rd-lookup/gp?ep=node1 ------------------------>  |
     |                                                             |
     |                                                             |
     | <-- 2.05 Content <coap://{ip:port}>;gp="lights1";ep="node1" |
     |                                                             |
~~~~
{: align="left"}


~~~~
Req: GET /rd-lookup/gp?ep=node1

Res: 2.05 Content
<coap://{ip:port}>;gp="lights1";ep="node1",
~~~~
{: align="left"}


# New Link-Format Attributes {#attributes}

When using the CoRE Link Format to describe resources being discovered by
or posted to a resource directory service, additional information about those
resources is useful. This specification defines the following new attributes
for use in the CoRE Link Format {{RFC6690}}:

~~~~
   link-extension    = ( "ins" "=" quoted-string ) ; Max 63 bytes
   link-extension    = ( "exp" )
~~~~

## Resource Instance attribute 'ins'

The Resource Instance "ins" attribute is an
identifier for this resource, which makes it possible
to distinguish it from other similar resources. This attribute is similar
in use to the &lt;Instance> portion of a DNS-SD record (see {{cheshire}}, and SHOULD be unique across resources with the same Resource Type attribute
in the domain it is used. A Resource Instance might be a descriptive string
like "Ceiling Light, Room 3", a short ID like "AF39" or a unique UUID or
iNumber. This attribute is used by a Resource Directory to distinguish between
multiple instances of the same resource type within the directory.

This attribute MUST be no more than 63 bytes in length. The resource identifier
attribute MUST NOT appear more than once in a link description.


## Export attribute 'exp'

The Export "exp" attribute is used as a flag to indicate that a link description
MAY be exported by a resource directory to external directories.

The CoRE Link Format is used for many purposes between CoAP endpoints. Some
are useful mainly locally, for example checking the observability of a resource
before accessing it, determining the size of a resource, or traversing dynamic
resource structures. However, other links are very useful to be exported
to other directories, for example the entry point resource to a functional
service.



# DNS-SD Mapping {#dns-sd}

CoRE Resource
Discovery is intended to support fine-grained discovery of hosted
resources, their attributes, and possibly other resource relations {{RFC6690}}. In contrast, service discovery generally refers to a coarse-grained
resolution of an endpoint's IP address, port number, and protocol.

Resource and service discovery are complementary in the case of large
networks, where the latter can facilitate scaling.  This document
defines a mapping between CoRE Link Format attributes and DNS-Based
Service Discovery {{RFC6763}} fields that permits
discovery of CoAP services by either means.

## DNS-based Service discovery {#cheshire}

DNS-Based Service Discovery (DNS-SD) defines a conventional method of
configuring DNS PTR, SRV, and TXT resource records to facilitate
discovery of services (such as CoAP servers in a subdomain) using the
existing DNS infrastructure.  This section gives a brief overview of
DNS-SD; see {{RFC6763}} for a detailed
specification.

DNS-SD service names are limited to 255 octets and are of the form:

Service Name = &lt;Instance>.&lt;ServiceType>.&lt;Domain>.

The service name is the label of SRV/TXT resource records. The SRV RR specifies
the host and the port of the endpoint. The TXT RR provides additional information.

The &lt;Domain> part of the service name is identical to the global (DNS
subdomain) part of the authority in URIs that identify servers or groups
of servers.

The &lt;ServiceType> part is composed of at least two labels.  The first
label of the pair is the application protocol name {{RFC6335}} preceded by an
underscore character.  The second label indicates the transport and is always
"_udp" for UDP-based CoAP services.  In cases where narrowing the scope of
the search may be useful, these labels may be optionally preceded by a
subtype name followed by the "_sub" label.  An example of this more specific
&lt;ServiceType> is "lamp._sub._dali._udp".

The default &lt;Instance> part of the service name may be set at the factory
or during the commissioning process.  It SHOULD uniquely identify an instance
of &lt;ServiceType> within a &lt;Domain>.  Taken together, these three
elements comprise a unique name for an SRV/ TXT record pair within the DNS
subdomain.

The granularity of a service name MAY be that of a host or group, or it could
represent a particular resource within a CoAP server.  The SRV record
contains the host name (AAAA record name) and port of the service while
protocol is part of the service name.  In the case where a service name
identifies a particular resource, the path part of the URI must be carried in
a corresponding TXT record.

A DNS TXT record is in practice limited to a few hundred octets in length,
which is indicated in the resource record header in the DNS response message.
The data consists of one or more strings comprising a key=value pair.  By
convention, the first pair is txtver=&lt;number> (to support different
versions of a service description).


## mapping ins to &lt;Instance> {#ins}

The Resource Instance "ins" attribute maps to the &lt;Instance> part of a
DNS-SD service name.  It is stored directly in the DNS as a single DNS label
of canonical precomposed UTF-8 {{RFC3629}} "Net-Unicode" (Unicode
Normalization Form C) {{RFC5198}} text.  However, to the extent that the
"ins" attribute may be chosen to match the DNS host name of a service, it
SHOULD use the syntax defined in Section 3.5 of {{RFC1034}} and Section 2.1
of {{RFC1123}}.

The &lt;Instance> part of the name of a service being offered on the network
SHOULD be configurable by the user setting up the service, so that he or she
may give it an informative name.  However, the device or service SHOULD NOT
require the user to configure a name before it can be used.  A sensible
choice of default name can allow the device or service to be accessed in many
cases without any manual configuration at all.  The default name should be
short and descriptive, and MAY include a collision-resistant substring such
as the lower bits of the device's MAC address, serial number, fingerprint, or
other identifier in an attempt to make the name relatively unique.

DNS labels are currently limited to 63 octets in length and the
entire service name may not exceed 255 octets.


## Mapping rt to &lt;ServiceType> {#exp}

The resource type "rt" attribute is mapped into the &lt;ServiceType> part of
a DNS-SD service name and SHOULD conform to the reg-rel-type production of
the Link Format defined in Section 2 of {{RFC6690}}.  The "rt" attribute MUST
be composed of at least a single Net-Unicode text string, without underscore
'_' or period '.' and limited to 15 octets in length, which represents the
application protocol name.  This string is mapped to the DNS-SD
&lt;ServiceType> by prepending an underscore and appending a period followed
by the "_udp" label.  For example, rt="dali" is mapped into "_dali._udp".

The application protocol name may be optionally followed by a period
and a service subtype name consisting of a Net-Unicode text string,
without underscore or period and limited to 63 octets.  This string
is mapped to the DNS-SD &lt;ServiceType> by appending a period followed
by the "_sub" label and then appending a period followed by the
service type label pair derived as in the previous paragraph.  For
example, rt="dali.light" is mapped into "light._sub._dali._udp".

The resulting string is used to form labels for DNS-SD records which
are stored directly in the DNS.


## Domain mapping {#domain}

DNS domains are defined from the "d" attribute.The domain attribute is
suffixed to the host name and should be consistent with the domain name
attributed to the hosting network segment.<!-- Was "zone", but I'm sure this
is orthogonal to DNS delegation.  -->


## TXT Record key=value strings {#TXT}

A number of {{RFC6763}} key/value pairs are derived from link-format
information, to be exported in the DNS-SD as key=value strings in a
TXT record ({{RFC6763}}, Section 6.3).

The resource &lt;URI> is exported as key/value pair "path=&lt;URI>".

The Interface Description "if" attribute is exported as key/value
pair "if=&lt;Interface Description>".

The DNS TXT record can be further populated by importing any other
resource description attributes as they share the same key=value
format specified in Section 6 of {{RFC6763}}.


## Importing resource links into DNS-SD {#import}

Assuming the ability to query a Resource Directory or multicast a GET
(?exp) over the local link, CoAP resource discovery may be used to
populate the DNS-SD database in an automated fashion.  CoAP resource
descriptions (links) can be exported to DNS-SD for exposure to
service discovery by using the Resource Instance attribute as the
basis for a unique service name, composed with the Resource Type as
the &lt;ServiceType>, and registered in the correct &lt;Domain>.  The agent
responsible for exporting records to the DNS
zone file SHOULD be authenticated to the DNS server.
The following example shows an agent discovering a resource to be
exported:

~~~~
    Agent                                                          RD
      |                                                             |
      | --- GET /rd-lookup/res?exp ------------------------------>  |
      |                                                             |
      |                                                             |
      | <-- 2.05 Content "<coap://node1/light/1>;exp; ------------  |
      |                   rt="dali.light";ins="FrontSpot"           |
      |                   d="example.com"                           |
      |                                                             |
~~~~
{: align="left"}


~~~~
   Req: GET /rd-lookup/res?exp

   Res: 2.05 Content
   <coap://[FDFD::1234]:61616/light/1>;
     exp;ct=41;rt="dali.light";ins="FrontSpot";
               d="example.com"
~~~~
{: align="left"}

The agent subsequently registers the following DNS-SD RRs:

~~~~
node1.example.com.                IN AAAA
                          FDFD::1234
_dali._udp.example.com            IN PTR
                          FrontSpot._dali._udp.example.com
light._sub._dali._udp.example.com IN PTR
                          FrontSpot._dali._udp.example.com
FrontSpot._dali._udp.example.com  IN SRV  0 0 5678
                          node1.example.com.
FrontSpot._dali._udp.example.com  IN TXT
                          txtver=1;path=/light/1
~~~~
{: align="left"}

In the above figure the Service Name is chosen as
FrontSpot._dali._udp.example.com without the light._sub service prefix. An
alternative Service Name would be:
FrontSpot.light._sub._dali._udp.example.com.



# Security Considerations

The security considerations as described in Section 7 of {{RFC5988}} and
Section 6 of {{RFC6690}} apply. The `/.well-known/core` resource may be
protected e.g. using DTLS when hosted on a CoAP server as described in
{{RFC7252}}. DTLS or TLS based security SHOULD be used on all resource
directory interfaces defined in this document (TODO: Improve the exact DTLS
or TLS security requirements and references).

## Endpoint Identification and Authentication {#endpoint_identification}

An Endpoint is determined to be unique by an RD by the Endpoint identifier
parameter included during Registration, and any associated TLS or DTLS security
bindings. An Endpoint MUST NOT be identified by its protocol, port or IP
address as these may change over the lifetime of an Endpoint.

Every operation performed by an Endpoint or Client on a resource directory
SHOULD be mutually authenticated using Pre-Shared Key, Raw Public Key or
Certificate based security. Endpoints using a Certificate MUST include the
Endpoint identifier as the Subject of the Certificate, and this identifier
MUST be checked by a resource directory to match the Endpoint identifier
included in the Registration message.


## Access Control

Access control SHOULD be performed separately for the RD Function Set and
the RD Lookup Function Set, as different endpoints may be authorized to register
with an RD from those authorized to lookup endpoints from the RD. Such access
control SHOULD be performed in as fine-grained a level as possible. For example
access control for lookups could be performed either at the domain, endpoint
or resource level.


## Denial of Service Attacks

Services that run over UDP unprotected are vulnerable to unknowingly
become part of a DDoS attack as UDP does not require return
routability check. Therefore, an attacker can easily spoof the source
IP of the target entity and send requests to such a service which
would then respond to the target entity. This can be used for
large-scale DDoS attacks on the target. Especially, if the service
returns a response that is order of magnitudes larger than the
request, the situation becomes even worse as now the attack can be
amplified. DNS servers have been widely used for DDoS amplification
attacks. Recently, it has been observed that NTP Servers, that also
run on unprotected UDP have been used for DDoS attacks
(http://tools.cisco.com/security/center/content/CiscoSecurityNotice/CVE-2013-5211)
[TODO: Ref, and cut down the verbiage, as this is already discussed in
RFC 7252] since there is no return routability check and can have a large
amplification factor. The responses from the NTP server were found to be
19 times larger than the request. A Resource Directory (RD) which responds
to wild-card lookups is potentially vulnerable if run with CoAP over UDP.
Since there is no return routability check and the responses can be significantly
larger than requests, RDs can unknowingly become part of a DDoS amplification
attack. Therefore, it is RECOMMENDED that implementations ensure return routability.
This can be done, for example by responding to wild card lookups only over
DTLS or TLS or TCP.



# IANA Considerations

## Resource Types {#iana-rt}

"core.rd", "core.rd-group" and "core.rd-lookup" resource types need to be
registered with the resource type registry defined by {{RFC6690}}.


## Link Extension {#iana-link-ext}

The "exp" attribute needs to be registered when a future Web Linking link-extension
registry is created (e.g. in RFC5988bis).


## RD Parameter Registry {#iana-registry}

This specification defines a new sub-registry for registration and lookup
parameters called "RD Parameters" under "CoRE Parameters". Although this
specification defines a basic set of parameters, it is expected that other
standards that make use of this interface will define new ones.

Each entry in the registry must include the human readable name of the parameter,
the query parameter, validity requirements if any and a description. The
query parameter MUST be a valid URI query key {{RFC3986}}.

Initial entries in this sub-registry are as follows:

| Name          | Query | Validity      | Description                                                    |
| Endpoint Name | ep    |               | Name of the endpoint                                           |
| Lifetime      | lt    | 60-4294967295 | Lifetime of the registration in seconds                        |
| Domain        | d     |               | Domain to which this endpoint belongs                          |
| Endpoint Type | et    |               | Semantic name of the endpoint                                  |
| Context       | con   | URI           | The scheme, address and port at which this server is available |
| Endpoint Name | ep    |               | Name of the endpoint, max 63 bytes                             |
| Group Name    | gp    |               | Name of a group in the RD                                      |
| Page          | page  | Integer       | Used for pagination                                            |
| Count         | count | Integer       | Used for pagination                                            |
{: #tab-registry title='RD Parameters'}

The IANA policy for future additions to the sub-registry is "Expert Review"
as described in {{RFC5226}}.



# Examples {#examples}




# Acknowledgments

Srdjan Krco, Szymon Sasin, Kerry Lynn, Esko Dijk, Peter van der Stok, Anders
Brandt, Matthieu Vial, Michael Koster, Mohit Sethi, Sampo Ukkola and Linyi
Tian have provided helpful comments, discussions and ideas to improve and
shape this document. Zach would also like to thank his collagues from the
EU FP7 SENSEI project, where many of the resource directory concepts were
originally developed.


# Changelog

Changes from -01 to -02:

*  Added a catalogue use case.

*  Changed the registration update to a POST with optional link format payload. Removed the endpoint type update from the update.

*  Additional examples section added for more complex use cases.

*  New DNS-SD mapping section.

*  Added text on endpoint identification and authentication.

*  Error code 4.04 added to Registration Update and Delete requests.

*  Made 63 bytes a SHOULD rather than a MUST for endpoint name and resource type parameters.


Changes from -00 to -01:

*  Removed the ETag validation feature.

*  Place holder for the DNS-SD mapping section.

*  Explicitly disabled GET or POST on returned Location.

*  New registry for RD parameters.

*  Added support for the JSON Link Format.

*  Added reference to the Groupcomm WG draft.


Changes from -05 to WG Document -00:

*  Updated the version and date.


Changes from -04 to -05:

*  Restricted Update to parameter updates.

*  Added pagination support for the Lookup interface.

*  Minor editing, bug fixes and reference updates.

*  Added group support.

*  Changed rt to et for the registration and update interface.


Changes from -03 to -04:

*  Added the ins= parameter back for the DNS-SD mapping.

*  Integrated the Simple Directory Discovery from Carsten.

*  Editorial improvements.

*  Fixed the use of ETags.


Changes from -02 to -03:

*  Changed the endpoint name back to a single registration parameter ep= and removed the h= and ins= parameters.

*  Updated REST interface descriptions to use RFC6570 URI Template format.

*  Introduced an improved RD Lookup design as its own function set.

*  Improved the security considerations section.

*  Made the POST registration interface idempotent by requiring the ep= parameter to be present.


Changes from -01 to -02:

*  Added a terminology section.

*  Changed the inclusion of an ETag in registration or update to a MAY.

*  Added the concept of an RD Domain and a registration parameter for it.

*  Recommended the Location returned from a registration to be stable, allowing for endpoint and Domain information to be changed during updates.

*  Changed the lookup interface to accept endpoint and Domain as query string parameters to control the scope of a lookup.

--- back
