---
title: CoRE Resource Directory
docname: draft-ietf-core-resource-directory-latest
stand_alone: true
ipr: trust200902
cat: std
pi:
  strict: 'yes'
  toc: 'yes'
  tocdepth: '3'
  symrefs: 'yes'
  sortrefs: 'yes'
  compact: 'yes'
  comments: yes
  subcompact: 'no'
  iprnotified: 'no'
title: CoRE Resource Directory
area: Internet
wg: CoRE
kw: CoRE, Web Linking, Resource Discovery, Resource Directory
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
  org: SmartThings
  street: 665 Clyde Avenue
  city: Mountain View
  code: '94043'
  country: USA
  phone: "+1-707-502-5136"
  email: Michael.Koster@smartthings.com
- ins: C. Bormann
  name: Carsten Bormann
  org: Universitaet Bremen TZI
  street: Postfach 330440
  city: Bremen
  code: D-28359
  country: Germany
  phone: "+49-421-218-63921"
  email: cabo@tzi.org
- ins: P. van der Stok
  name: Peter van der Stok
  org: consultant
  abbrev: consultant
  phone: "+31-492474673 (Netherlands), +33-966015248 (France)"
  email: consultancy@vanderstok.org
  uri: www.vanderstok.org
- ins: C. Amsüss
  name: Christian Amsüss
  street: Hollandstr. 12/4
  code: '1020'
  country: Austria
  phone: "+43-664-9790639"
  email: christian@amsuess.com
  role: editor
normative:
  RFC6690:
  RFC2119:
  RFC3986:
  RFC8126:
#  RFC6335: portreg
  RFC6570:
  RFC6763: dnssd
#  RFC7396:
informative:
  RFC7252:
  RFC7390:
  RFC6775:
  RFC6874:
  RFC7230:
  RFC8132:
#  RFC3629: utf8
#  RFC5198: nvt-utf8
#  RFC1123: hostreq
#  RFC1034: dns1
  RFC7641:
  ER: DOI.10.1145/320434.320440
  RFC8288:
  I-D.silverajan-core-coap-protocol-negotiation:
  I-D.ietf-ace-oauth-authz:
  I-D.ietf-core-links-json:
  I-D.ietf-core-rd-dns-sd:

--- abstract

In many IoT applications, direct discovery of resources is not practical
due to sleeping nodes, disperse networks, or networks where multicast traffic
is inefficient. These problems can be solved by employing an entity called
a Resource Directory (RD), which contains information about resources held on
other servers, allowing lookups to be performed for those resources. The input to an RD is composed of links and the output is composed of links constructed from the information stored in the RD. This
document specifies the web interfaces that a Resource Directory supports for web servers to discover the RD and to register, maintain, lookup
and remove information on resources. Furthermore, new target attributes useful
in conjunction with an RD are defined.

--- middle

# Introduction {#introduction}

In the work on Constrained RESTful Environments (CoRE), a REST architecture
suitable for constrained nodes (e.g. with limited RAM and ROM {{?RFC7228}})
and networks (e.g. 6LoWPAN {{?RFC4944}})
has been established and is used in
Internet-of-Things (IoT) or
machine-to-machine (M2M) applications such as smart energy
and building automation.

The discovery of resources offered by a constrained server is very important
in machine-to-machine applications where there are no humans in the loop and
static interfaces result in fragility. The discovery of resources provided by
an HTTP Web Server is typically called Web Linking {{RFC8288}}. The use of
Web Linking for the description and discovery of resources hosted by
constrained web servers is specified by the CoRE Link Format
{{RFC6690}}. However, {{RFC6690}} only describes how to discover
resources from the web server that hosts them by querying
`/.well-known/core`. In many constrained scenarios, direct discovery of resources is
not practical due to sleeping nodes, disperse networks, or networks where
multicast traffic is inefficient. These problems can be solved by employing
an entity called a Resource Directory (RD), which contains information about resources held on
other servers, allowing lookups to be performed for those resources.

This document specifies the web interfaces that a Resource Directory supports for web servers to discover the RD and to register, maintain, lookup
and remove information on resources. Furthermore, new target attributes useful in
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
concepts that are discussed in {{RFC3986}}, {{RFC8288}} and {{RFC6690}}. Readers should
also be familiar with the terms and concepts discussed in {{RFC7252}}.  To
describe the REST interfaces defined in this specification, the URI Template
format is used {{RFC6570}}.

This specification makes use of the following additional terminology:

{: vspace='0'}
resolve against
:   The expression "a URI-reference is *resolved against* a base URI" is used
to describe the process of {{RFC3986}} Section 5.2. Noteworthy corner cases are
that if the URI-reference is a (full) URI and resolved  against any base URI, that gives the original full URI, and
that resolving an empty URI reference gives the base URI without any fragment identifier.

Resource Directory
:   A web entity that stores information about web resources and implements the
REST interfaces defined in this specification for registration and lookup
of those resources.

Sector
:   In the context of a Resource Directory, a sector is a
logical grouping of endpoints.

: The abbreviation "d=" is used for the sector in query parameters for
compatibility with deployed implementations.

Endpoint
:   Endpoint (EP) is a term used to describe a web server or client in {{RFC7252}}.
In the context of this specification an endpoint is used to describe a
web server that registers resources to the Resource Directory. An endpoint
is identified by its endpoint name, which is included during registration,
and has a unique name within the associated sector of the registration.

Registration Base URI
:   The Base URI of a Registration is a URI that typically gives scheme and
authority information about an Endpoint. The Registration Base URI is provided at registration time, and is used by the Resource Directory to
resolve relative references of the registration into URIs.

Target
:   The target of a link is the destination address (URI) of the link. It is sometimes identified with "href=", or displayed as `<target>`. Relative targets need resolving with respect to the Base URI (section 5.2 of {{RFC3986}}).

:   This use of the term Target is consistent with {{RFC8288}}'s use of the term.

Context
:   The context of a link is the source address (URI) of the link,
    and describes which resource is linked to the target.
    A link's context is made explicit in serialized links as the "anchor=" attribute.

:   This use of the term Context is consistent with {{RFC8288}}'s use of the term.

Directory Resource
:  A resource in the Resource Directory (RD) containing registration resources.

Registration Resource
:  A resource in the RD that contains information about an Endpoint and its links.

Commissioning Tool
: Commissioning Tool (CT) is a device that assists during the installation of the
network by assigning values to parameters, naming endpoints and groups, or adapting
the installation to the needs of the applications.

Registrant-ep
:    Registrant-ep is the endpoint that is registered into the RD. The registrant-ep can register itself, or a CT registers the registrant-ep.

RDAO
: Resource Directory Address Option.
  A new IPv6 Neighbor Discovery option defined for announcing a Resource Directory's address.



# Architecture and Use Cases {#arch}

## Principles

The Resource Directory is primarily a tool to make discovery operations more
efficient than querying /.well-known/core on all connected devices, or across
boundaries that would be limiting those operations.

It provides information about resources hosted by other devices that could otherwise only be obtained by
directly querying the /.well-known/core resource on these other devices, either by a unicast request or a multicast request.

Information SHOULD only be stored in the resource directory
if it can be obtained by querying the described device's
/.well-known/core resource directly.

Data in the resource directory can only be provided by the
device which hosts those data or a dedicated Commissioning Tool (CT).
These CTs are thought to act on behalf of endpoints too constrained, or generally
unable, to present that information themselves. No other client can modify data
in the resource directory. Changes to the information in the Resource Directory do not propagate automatically back to the web servers from where the information originated.

## Architecture

The resource directory architecture is illustrated in {{fig-arch}}. A
Resource Directory (RD) is used as a repository of registrations
describing resources hosted on other web servers, also called endpoints
(EP).
An endpoint is a web server associated with a scheme, IP address and port. A physical node may host one or more endpoints. The
RD implements a set of REST interfaces for endpoints to register and maintain
resource directory registrations, and for endpoints to
lookup resources from the RD. An RD can be logically segmented by the use of Sectors.

A mechanism to discover an RD using CoRE Link Format {{RFC6690}} is defined.

Registrations
in the RD are soft state and need to be periodically refreshed.

An endpoint uses specific interfaces to register, update and remove a registration. It is also possible for an RD to fetch Web Links
from endpoints and add their contents to resource directory registrations.

At the first registration of an endpoint, a "registration resource" is created,
the location of which is returned to the registering endpoint. The registering
endpoint uses this registration resource to manage the contents of registrations.

A lookup interface for discovering any of the Web Links stored in the RD is
provided using the CoRE Link Format.

~~~~
             Registration         Lookup
              Interface         Interface
  +----+          |                 |
  | EP |----      |                 |
  +----+    ----  |                 |
                --|-    +------+    |
  +----+          | ----|      |    |     +--------+
  | EP | ---------|-----|  RD  |----|-----| Client |
  +----+          | ----|      |    |     +--------+
                --|-    +------+    |
  +----+    ----  |                 |
  | CT |----      |                 |
  +----+

~~~~
{: #fig-arch title='The resource directory architecture.' align="left"}


A Registrant-EP MAY keep concurrent registrations to more than one RD at the same time
if explicitly configured to do so,
but that is not expected to be supported by typical EP implementations.
Any such registrations are independent of each other.
The usual expectation when multiple discovery mechanisms or addresses are configured
is that they constitute a fall-back path for a single registration.


## RD Content Model {#ER-model}

The Entity-Relationship (ER) models shown in {{fig-ER-WKC}} and {{fig-ER-RD}} model the contents of /.well-known/core and the resource directory respectively, with entity-relationship diagrams [ER][]. Entities (rectangles) are used for concepts that exist independently. Attributes (ovals) are used for concepts that exist only in connection with a related entity. Relations (diamonds) give a semantic meaning to the relation between entities. Numbers specify the cardinality of the relations.

Some of the attribute values are URIs. Those values are always full URIs and never relative references in the information model.
They can, however, be expressed as relative references in serializations, and often are.

These models provide an abstract view of the information expressed in link-format documents and a Resource Directory. They cover the concepts, but not necessarily all details of an RD's operation; they are meant to give an overview, and not be a template for implementations.

~~~~
                    +----------------------+
                    |   /.well-known/core  |
                    +----------------------+
                               |
                               | 1
                       ////////\\\\\\\
                      <    contains   >
                       \\\\\\\\///////
                               |
                               | 0+
                     +--------------------+
                     |      link          |
                     +--------------------+
                               |
                               |  1   oooooooo
                               +-----o target o
                               |      oooooooo
          oooooooooooo   0+    |
         o    target  o--------+
         o  attribute o        | 0+   oooooo
          oooooooooooo         +-----o rel  o
                               |      oooooo
                               |
                               | 1    ooooooooo
                               +-----o context o
                                      ooooooooo



~~~~
{: #fig-ER-WKC title='E-R Model of the content of /.well-known/core' align="left"}

The model shown in {{fig-ER-WKC}} models the contents of /.well-known/core which contains:

* a set of links belonging to the hosting web server

The web server is free to choose links it deems appropriate to be exposed in its `.well-known/core`.
Typically, the links describe resources that are served by the host, but the set can also contain links to resources on other servers (see examples in {{RFC6690}} page 14).
The set does not necessarily contain links to all resources served by the host.

A link has the following attributes (see {{RFC8288}}):

* Zero or more link relations: They describe relations between the link context and the link target.

  In link-format serialization, they are expressed as space-separated values in the "rel" attribute, and default to "hosts".

* A link context URI: It defines the source of the relation, e.g. *who* "hosts" something.

  In link-format serialization, it is expressed in the "anchor" attribute. It defaults to that document's URI.

* A link target URI: It defines the destination of the relation (e.g. *what* is hosted), and is the topic of all target attributes.

    In link-format serialization, it is expressed between angular brackets, and sometimes called the "href".

* Other target attributes (e.g. resource type (rt), interface (if), or content format (ct)).
  These provide additional information about the target URI.


~~~~
             +----------------------+
             |  resource-directory  |
             +----------------------+
                        | 1
                        |
                        |
                        |
                        |
                   //////\\\\
                  < contains >
                   \\\\\/////
                        |
                     0+ |
 ooooooo     1  +---------------+
o  base o-------|  registration |
 ooooooo        +---------------+
                    |       | 1
                    |       +--------------+
       oooooooo   1 |                      |
      o  href  o----+                 /////\\\\
       oooooooo     |                < contains >
                    |                 \\\\\/////
       oooooooo   1 |                      |
      o   ep   o----+                      | 0+
       oooooooo     |             +------------------+
                    |             |      link        |
       oooooooo 0-1 |             +------------------+
      o    d   o----+                      |
       oooooooo     |                      |  1   oooooooo
                    |                      +-----o target o
       oooooooo   1 |                      |      oooooooo
      o   lt   o----+     ooooooooooo   0+ |
       oooooooo     |    o  target   o-----+
                    |    o attribute o     | 0+   oooooo
    ooooooooooo 0+  |     ooooooooooo      +-----o rel  o
   o  endpoint o----+                      |      oooooo
   o attribute o                           |
    ooooooooooo                            | 1   ooooooooo
                                           +----o context o
                                                 ooooooooo
~~~~
{: #fig-ER-RD title='E-R Model of the content of the Resource Directory' align="left"}

The model shown in {{fig-ER-RD}} models the contents of the resource directory which contains in addition to /.well-known/core:

* 0 to n Registrations of endpoints,

A registration is associated with one endpoint. A registration defines a set of links as defined for /.well-known/core. A Registration has six types of attributes:

* an endpoint name ("ep", a Unicode string) unique within a sector
* a Registration Base URI ("base", a URI typically describing the scheme://authority part)
* a lifetime ("lt"),
* a registration resource location inside the RD ("href"),
* optionally a sector ("d", a Unicode string)
* optional additional endpoint attributes (from {{iana-registry}})

The cardinality of "base" is currently 1;
future documents are invited to extend the RD specification to support multiple values (e.g. {{I-D.silverajan-core-coap-protocol-negotiation}}).
Its value is used as a Base URI when resolving URIs in the links contained in the endpoint.

Links are modelled as they are in {{fig-ER-WKC}}.

## Link-local addresses and zone identifiers {#linklocal}

Registration Base URIs can contain link-local IP addresses.
To be usable across hosts, those can not be serialized to contain zone identifiers (see {{RFC6874}} Section 1).

Link-local addresses can only be used on a single link
(therefore RD servers can not announce them when queried on a different link),
and lookup clients using them need to keep track of which interface they got them from.

Therefore, it is advisable in many scenarios
to use addresses with larger scope if available.

## Use Case: Cellular M2M {#cellular}

Over the last few years, mobile operators around the world
have focused on development of M2M solutions in order to
expand the business to the new type of users: machines. The
machines are connected directly to a mobile network using an appropriate
embedded wireless interface (GSM/GPRS, WCDMA, LTE) or via a gateway providing
short and wide range wireless interfaces. From the system design point of
view, the ambition is to design horizontal solutions that can enable utilization
of machines in different applications depending on their current availability
and capabilities as well as application requirements, thus avoiding silo
like solutions. One of the crucial enablers of such design is the ability
to discover resources (machines --- endpoints) capable of providing required
information at a given time or acting on instructions from the end users.

Imagine a scenario where endpoints installed on vehicles enable
tracking of the position of these vehicles for fleet management purposes and allow
monitoring of environment parameters. During the boot-up process
endpoints register with a Resource Directory, which is hosted by the
mobile operator or somewhere in the cloud. Periodically, these endpoints
update their registration and may modify resources they offer.

When endpoints are not always connected, for example because they enter
a sleep mode, a remote server is usually used to provide proxy access to
the endpoints. Mobile apps or web applications for environment monitoring contact the RD, look up the endpoints capable of providing information about the environment using an appropriate set of link parameters, obtain information on how to contact them (URLs of the proxy server), and then initiate interaction to obtain information that is finally processed, displayed on the screen and usually stored in a database. Similarly, fleet management systems provide
the appropriate link parameters to the RD to look up for EPs deployed on
the vehicles the application is responsible for.


## Use Case: Home and Building Automation {#automation}

Home and commercial building automation systems can benefit from the use
of M2M web services.  The discovery requirements of these applications are
demanding. Home automation usually relies on run-time discovery to commission
the system, whereas in building automation a combination of professional
commissioning and run-time discovery is used. Both home and building automation
involve peer-to-peer interactions between endpoints, and involve battery-powered
sleeping devices.

Two phases can be discerned for a network servicing the system: (1) installation and (2) operation. During the operational phase, the network is connected to the Internet with a Border router (6LBR) and the nodes connected to the network can use the Internet services that are provided by the Internet Provider or the network administrator. During the installation phase, the network is completely stand-alone, no 6LBR is connected, and the network only supports the IP communication between the connected nodes. The installation phase is usually followed by the operational phase.


## Use Case: Link Catalogues {#usecase-catalogues}

Resources may be shared through data brokers that have no knowledge beforehand
of who is going to consume the data. Resource Directory can be used to hold
links about resources and services hosted anywhere to make them discoverable
by a general class of applications.

For example, environmental and weather sensors that generate data for public
consumption may provide data to an intermediary server, or broker. Sensor
data are published to the intermediary upon changes or at regular intervals.
Descriptions of the sensors that resolve to links to sensor data may be published
to a Resource Directory. Applications wishing to consume the data can use
RD Lookup to discover and resolve links
to the desired resources and endpoints. The Resource Directory service need
not be coupled with the data intermediary service. Mapping of Resource Directories
to data intermediaries may be many-to-many.

Metadata in web link formats like {{RFC6690}} which may be internally stored as  triples, or relation/attribute
pairs providing metadata about resource links, need to be supported by Resource Directories . External catalogues that are
represented in other formats may be converted to common web linking formats for
storage and access by Resource Directories. Since it is common practice for these
to be encoded in URNs {{?RFC8141}}, simple and lossless structural transforms should
generally be sufficient to store external metadata in Resource Directories.

The additional features of Resource Directory allow sectors to be defined
to enable access to a particular set of resources from particular applications.
This provides isolation and protection of sensitive data when needed. Application groups with multicast addresses may be defined to support efficient data transport.

# RD discovery and other interface-independent components

This and the following sections define the required set of REST interfaces between a Resource Directory
(RD), endpoints and lookup clients. Although the examples throughout these sections assume the use of
CoAP {{RFC7252}}, these REST interfaces can also be realized using HTTP {{RFC7230}}.
Only multicast discovery operations are not possible on HTTP, and Simple Registration can not be executed as base attribute (which is mandatory for HTTP) can not be used there.
In all definitions in these sections, both CoAP response codes (with dot notation) and HTTP response codes
(without dot notation) are shown. An RD implementing this specification MUST support
the discovery, registration, update, lookup, and removal interfaces.

All operations on the contents of the Resource Directory MUST be atomic and idempotent.

For several operations, interface templates are given in list form;
those describe the operation participants, request codes, URIs, content formats and outcomes.
Sections of those templates contain normative content about
Interaction, Method, URI Template and URI Template Variables
as well as the details of the Success condition.
The additional sections
on options like Content-Format and on Failure codes
give typical cases that an implementation of the RD should deal with.
Those serve to illustrate the typical responses
to readers who are not yet familiar with all the details of CoAP based interfaces;
they do not limit what a server may respond under atypical circumstances.

REST clients (registrant-EPs / CTs, lookup clients, RD servers during simple registrations)
MUST be prepared to receive any unsuccessful code and act upon it
according to its definition, options and/or payload to the best of their capabilities,
falling back to failing the operation if recovery is not possible.
In particular, they should retry the request upon 5.03 (Service Unavailable; 503 in HTTP)
according to the Max-Age (Retry-After in HTTP) option,
and fall back to link-format when receiving 4.15 (Unsupported Content-Format; 415 in HTTP).

A resource directory MAY make the information submitted to it available to further
directories, if it can ensure that a loop does not form.  The protocol used
between directories to ensure loop-free operation is outside the scope of
this document.

## Finding a Resource Directory {#finding_an_rd}

A (re-)starting device may want to find one or more resource directories
for discovery purposes. Dependent on the operational conditions, one or more of the techniques below apply.

The device may be pre-configured to exercise specific mechanisms for
finding the resource directory:

1. It may be configured with a specific IP address for the RD.  That IP
   address may also be an anycast address, allowing the network to
   forward RD requests to an RD that is topologically close; each
   target network environment in which some of these preconfigured
   nodes are to be brought up is then configured with a route for this
   anycast address that leads to an appropriate RD.  (Instead of using
   an anycast address, a multicast address can also be preconfigured.
   The RD servers then need to configure one of their
   interfaces with this multicast address.)
2. It may be configured with a DNS name for the RD and use DNS to return
   the IP address of the RD; it can find a DNS server to perform the lookup using the usual mechanisms for finding DNS servers.
3. It may be configured to use a service discovery mechanism such as
   DNS-SD, as outlined in {{rd-using-dnssd}}.

For cases where the device is not specifically configured with a way
to find a resource directory, the network may want to provide a
suitable default.

3. If the address configuration of the network is performed via SLAAC,
   this is provided by the RDAO option {{rdao}}.
4. If the address configuration of the network is performed via DHCP,
   this could be provided via a DHCP option (no such option is defined
   at the time of writing).

Finally, if neither the device nor the network offers any specific
configuration, the device may want to employ heuristics to find a
suitable resource directory.

The present specification does not fully define these heuristics, but
suggests a number of candidates:

5. In a 6LoWPAN, just assume the Border Router (6LBR) can act as a
   resource directory (using the ABRO option to find that {{RFC6775}}).
   Confirmation can be obtained by sending a Unicast to
   `coap://[6LBR]/.well-known/core?rt=core.rd*`.

6. In a network that supports multicast well, discovering the RD using
   a multicast query for /.well-known/core as specified in CoRE Link
   Format {{RFC6690}}: Sending a Multicast GET to
   `coap://[MCD1]/.well-known/core?rt=core.rd*`.  RDs within the
   multicast scope will answer the query.

 When answering a multicast request directed at a link-local address,
  the RD may want to respond from a routable address;
  this makes it easier for registrants to use one of their own routable addresses for registration.

As some of the RD addresses obtained by the methods listed here are
just (more or less educated) guesses, endpoints MUST make use of any
error messages to very strictly rate-limit requests to candidate IP
addresses that don't work out.  For example, an ICMP Destination
Unreachable message (and, in particular, the port unreachable code for
this message) may indicate the lack of a CoAP server on the candidate
host, or a CoAP error response code such as 4.05 "Method Not Allowed"
may indicate unwillingness of a CoAP server to act as a directory
server.

The following RD discovery mechanisms are recommended:

  * In managed networks with border routers that need stand-alone operation, the RDAO option is recommended (e.g. operational phase described in {{automation}}).
  * In managed networks without border router (no Internet services available), the use of a preconfigured anycast address is recommended (e.g. installation phase described in {{automation}}).
  * In networks managed using DNS-SD, the use of DNS-SD for discovery as described in {{rd-using-dnssd}} is recommended.

The use of multicast discovery in mesh networks is NOT recommended.


### Resource Directory Address Option (RDAO) {#rdao}

The Resource Directory Address Option (RDAO) using IPv6 Neighbor Discovery (ND) carries
information about the address of the Resource Directory (RD). This information is
needed when endpoints cannot discover the Resource Directory with a link-local
or realm-local scope multicast address, for instance because the
endpoint and the RD are separated by a Border Router
(6LBR). In many circumstances the availability of DHCP cannot be guaranteed either
during commissioning of the network. The presence and the use of the RD is
essential during commissioning.

It is possible to send multiple RDAO options in one message,
indicating as many resource directory addresses.

The RDAO format is:

~~~~
0                   1                   2                   3
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|     Type      |  Length = 3   |       Valid Lifetime          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                           Reserved                            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
+                                                               +
|                                                               |
+                          RD Address                           +
|                                                               |
+                                                               +
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

Fields:

Type:                   TBD38

Length:                 8-bit unsigned integer.  The length of
                        the option in units of 8 bytes.
                        Always 3.

Valid Lifetime:         16-bit unsigned integer.  The length of
                        time in units of 60 seconds (relative to
                        the time the packet is received) that
                        this Resource Directory address is valid.
                        A value of all zero bits (0x0) indicates
                        that this Resource Directory address
                        is not valid anymore.

Reserved:               This field is unused.  It MUST be
                        initialized to zero by the sender and
                        MUST be ignored by the receiver.

RD Address:             IPv6 address of the RD.
~~~~
{: #fig-rdao title='Resource Directory Address Option' align="left"}

### Using DNS-SD to discover a resource directory {#rd-using-dnssd}

A resource directory can advertise its presence in DNS-SD
using the service name `_core-rd._udp`
defined in this document.
(This method is currently only defined for CoAP over UDP.
This is both for simplicity and because this is expected to be the common use case;
analogous services for other transports can be registered once there is demand for them).

The use of that service names implies that CoAP-over-UDP is used.
The SRV record points the client to a host name and port to use as a starting point for the URI discovery steps of {{discovery}}.

This section is a simplified concrete application of the more generic mechanism
specified in {{I-D.ietf-core-rd-dns-sd}}.

## Payload Content Formats

Resource Directory implementations using this specification MUST support the
application/link-format content format (ct=40).

Resource Directories implementing this specification MAY support additional content formats.

Any additional content format supported by a Resource Directory implementing this
specification SHOULD be able to express all the information expressible in link-format.
It MAY be able to express information that is inexpressible in link-format,
but those expressions SHOULD be avoided where possible.


## URI Discovery {#discovery}

Before an endpoint can make use of an RD, it must first know the RD's address
and port, and the URI path information for its REST APIs. This section defines
discovery of the RD and its URIs using the well-known interface of the
CoRE Link Format {{RFC6690}} after having discovered a host as described in {{finding_an_rd}}.

Discovery of the RD registration URI path is performed by sending either a multicast or
unicast GET request to `/.well-known/core` and including a Resource Type (rt)
parameter {{RFC6690}} with the value "core.rd" in the query string. Likewise, a
Resource Type parameter value of "core.rd-lookup\*" is used to discover the
URIs for RD Lookup operations, core.rd\* is used to discover all URI paths for RD operations.
Upon success, the response will contain a payload with
a link format entry for each RD function discovered, indicating the URI
of the RD function returned and the corresponding Resource Type. When performing
multicast discovery, the multicast IP address used will depend on the scope required
and the multicast capabilities of the network (see {{mc-registration}}).

A Resource Directory MAY provide hints about the content-formats it supports in the links it exposes or registers, using the "ct" target attribute, as shown in the example below. Clients MAY use these hints to select alternate content-formats for interaction with the Resource Directory.

HTTP does not support multicast and consequently only unicast discovery can be supported
at the using the HTTP `/.well-known/core` resource.

An implementation of this  resource directory specification MUST support query filtering for
the rt parameter as defined in {{RFC6690}}.

While the link targets in this discovery step are often expressed in path-absolute form,
this is not a requirement.
Clients of the RD SHOULD therefore accept URIs of all schemes they support,
both as URIs and relative references,
and not limit the set of discovered URIs to those hosted at the address used for URI discovery.

The URI Discovery operation can yield multiple URIs of a given resource type.
The client of the RD can use any of the discovered addresses initially.

The discovery request interface is specified as follows
(this is exactly the Well-Known Interface of {{RFC6690}} Section 4,
with the additional requirement that the server MUST support query filtering):

Interaction:
: EP and Client -> RD

Method:
: GET

URI Template:
: /.well-known/core{?rt}

URI Template Variables:
: rt :=
  : Resource Type. SHOULD contain one of the values "core.rd", "core.rd-lookup\*",
  "core.rd-lookup-res", "core.rd-lookup-ep", or "core.rd\*"

Accept:
: absent, application/link-format or any other media type representing web links

The following response is expected on this interface:

Success:
: 2.05 "Content" or 200 "OK" with an
  application/link-format or other web link payload containing one or more matching entries for the RD resource.

The following example shows an endpoint discovering an RD using this interface,
thus learning that the directory resource location, in this example, is /rd, and that the
content-format delivered by the server hosting the resource is application/link-format
(ct=40).  Note that it is up to the RD to choose its RD locations.

~~~~
Req: GET coap://[MCD1]/.well-known/core?rt=core.rd*

Res: 2.05 Content
</rd>;rt="core.rd";ct=40,
</rd-lookup/ep>;rt="core.rd-lookup-ep";ct=40,
</rd-lookup/res>;rt="core.rd-lookup-res";ct=40,
~~~~
{: #example-discovery title="Example discovery exchange" }

The following example shows the way of indicating that a client may request
alternate content-formats. The Content-Format code attribute "ct" MAY include a
space-separated sequence of Content-Format codes as specified in
Section 7.2.1 of {{RFC7252}}, indicating that multiple content-formats are available.
The example below shows the required Content-Format 40 (application/link-format)
indicated as well as a CBOR and JSON representation from {{I-D.ietf-core-links-json}}
(which have no numeric values assigned yet, so they are shown as TBD64 and TBD504 as in that draft).
The RD resource locations /rd, and /rd-lookup are example values.
The server in this example also indicates that it is capable of providing observation on resource lookups.

~~~~
Req: GET coap://[MCD1]/.well-known/core?rt=core.rd*

Res: 2.05 Content
</rd>;rt="core.rd";ct="40 65225",
</rd-lookup/res>;rt="core.rd-lookup-res";ct="40 TBD64 TBD504";obs,
</rd-lookup/ep>;rt="core.rd-lookup-ep";ct="40 TBD64 TBD504",
~~~~
{: #example-discovery-ct title="Example discovery exchange indicating additional content-formats" }

From a management and maintenance perspective,
it is necessary to identify the components that constitute the RD server.
The identification refers to information about for example client-server incompatibilities,
supported features, required updates and other aspects.
The URI discovery address, a described in section 4 of {{RFC6690}} can be used to find the identification.

It
would typically be stored in an implementation information link
(as described in {{?I-D.bormann-t2trg-rel-impl}}):

~~~~
Req: GET /.well-known/core?rel=impl-info

Res: 2.05 Content
<http://software.example.com/shiny-resource-directory/1.0beta1>;
    rel="impl-info"
~~~~
{: #example-impl-discovery title="Example exchange of obtaining implementation information" }

Note that depending on the particular server's architecture,
such a link could be anchored at the RD server's root,
at the discovery site (as in this example) or
at individual RD components.
The latter is to be expected when different applications
are run on the same server.


# Registration {#registration}

After discovering the location of an RD, a registrant-ep or CT MAY
register the resources of the registrant-ep using the registration interface. This interface
accepts a POST from an endpoint containing the list of resources to be added
to the directory as the message payload in the CoRE Link Format {{RFC6690}} or other representations of web links, along with query
parameters indicating the name of the endpoint, and optionally the sector,
lifetime and base URI of the registration.
It is expected that other specifications will define further parameters (see
{{iana-registry}}). The RD then creates a new registration resource in the RD and returns its location. The receiving endpoint MUST use that
location when refreshing registrations using this interface. Registration
resources in the RD are kept active for the period indicated by the lifetime
parameter. The creating endpoint is responsible for refreshing the registration resource within this
period using either the registration or update interface. The registration
interface MUST be implemented to be idempotent, so that registering twice
with the same endpoint parameters ep and d (sector) does not create multiple registration resources.

The following rules apply for a registration request targeting a given (ep, d) value pair:

* When the (ep, d) value pair of the registration-request is different from any existing registration,
  a new registration is generated.
* When the (ep, d) value pair of the registration-request is equal to an existing registration,
  the content and parameters of the existing registration are replaced with the content of the registration request.

The posted link-format document can (and typically does) contain relative references
both in its link targets and in its anchors, or contain empty anchors.
The RD server needs to resolve these references in order to faithfully represent them in lookups.
They are resolved against the base URI of the registration,
which is provided either explicitly in the `base` parameter or constructed implicitly from the requester's URI as constructed from its network address and scheme.

For media types to which {{limitedlinkformat}} applies
(i.e. documents in application/link-format),
the RD only needs to accept representations in Limited Link Format as described there.
Its behavior with representations outside that subset is implementation defined.

The registration request interface is specified as follows:

Interaction:
: EP -> RD


Method:
: POST


URI Template:
: {+rd}{?ep,d,lt,base,extra-attrs\*}


URI Template Variables:
: rd :=
  : RD registration URI
    (mandatory). This is the location of
    the RD, as obtained from discovery.

  ep :=
  : Endpoint name (mostly mandatory). The endpoint name is an identifier
    that MUST be unique within a sector.

    As the endpoint name is a Unicode string,
    it is encoded in UTF-8 (and possibly pct-encoded) during variable expansion (see {{RFC6570}} Section 3.2.1).
    The endpoint name MUST NOT contain any character in the inclusive ranges 0-31 or 127-159.

    The maximum length of this parameter is 63 UTF-8 encoded bytes.

    If the RD is configured to recognize the endpoint (e.g. based on its security context), the RD assigns an endpoint name based on a set of configuration parameter values.

  d :=
  : Sector (optional). The sector to which this endpoint belongs.
    When this parameter is not present, the
    RD MAY associate the endpoint with a configured default sector or leave it empty.

    The sector is encoded like the ep parameter, and is limited to 63 UTF-8 encoded bytes as well.

    The endpoint name and sector name are not set when one or both are set in an accompanying authorization token.

  lt :=
  : Lifetime (optional). Lifetime of the registration in seconds. Range of 1-4294967295.
    If no lifetime is included in the initial registration, a default value of
    90000 (25 hours) SHOULD be assumed.

  base :=
  : Base URI (optional). This parameter sets the base URI of the registration, under which
    the relative links in the payload are to be interpreted. The specified URI typically does not have a path component of its own, and MUST be suitable as a base URI to resolve any relative references given in the registration. The parameter is therefore usually of the shape "scheme://authority" for
    HTTP and CoAP URIs.
    The URI SHOULD NOT have a query or fragment component
    as any non-empty relative part in a reference would remove those parts from the resulting URI.

  : In the absence of this parameter the scheme of the protocol, source address
    and source port of the registration request are assumed.
    The Base URI is consecutively constructed by concatenating the used protocol's scheme
    with the characters "://", the requester's source address as an address
    literal and ":" followed by its port (if it was not the protocol's default
    one) in analogy to {{RFC7252}} Section 6.5.

  : This parameter is
    mandatory when the directory is filled by a third party such as an
    commissioning tool.

  : If the registrant-ep uses an ephemeral port to register with, it MUST include the base
    parameter in the registration to provide a valid network path.

  : A registrant that can not be reached by potential lookup clients at the address it registers from
     (e.g. because it is behind some form of Network Address Translation (NAT))
     MUST provide a reachable base address with its registration.

  : If the Base URI contains a link-local IP literal, it MUST NOT contain a Zone Identifier,
    and MUST be local to the link on which the registration request is received.

  : Endpoints that register with a base that contains a path component
    can not meaningfully use {{RFC6690}} Link Format due to its prevalence of
    the Origin concept in relative reference resolution.
    Those applications should use different representations of links to which {{limitedlinkformat}} is not applicable
    (e.g. {{?I-D.hartke-t2trg-coral}}).
    <!-- or may use non-Limited-Link-Format documents on servers that share their necessarily-non6690 understanding of links – but we can't say that in an RFC, can we? -->

  extra-attrs :=
  : Additional registration attributes (optional). The endpoint can pass any
    parameter registered at {{iana-registry}} to the directory. If the RD is
    aware of the parameter's specified semantics, it processes it accordingly.
    Otherwise, it MUST store the unknown key and its value(s) as an endpoint
    attribute for further lookup.

Content-Format:
: application/link-format or any other indicated media type representing web links

The following response is expected on this interface:

Success:
: 2.01 "Created" or 201 "Created". The Location-Path option or Location header field
  MUST be included in the response. This location MUST be a stable identifier
  generated by the RD as it is used for all subsequent
  operations on this registration resource. The registration resource location thus returned is for the purpose of updating the lifetime
  of the registration and for maintaining the content of the
  registered links, including updating and deleting links.

: A registration with an already registered ep and d value pair
  responds with the same success code and location as the original registration;
  the set of links registered with the endpoint is replaced with the links
  from the payload.

: The location MUST NOT have a query or fragment component,
  as that could conflict with query parameters during the Registration Update operation.
  Therefore, the Location-Query option MUST NOT be present in a successful response.

If the registration fails, including request timeouts,
or if delays from Service Unavailable responses with Max-Age or Retry-After
accumulate to exceed the registrant's configured timeouts,
it SHOULD pick another registration URI from the "URI Discovery" step
and if there is only one or the list is exhausted,
pick other choices from the "Finding a Resource Directory" step.
Care has to be taken to consider the freshness of results obtained earlier,
e.g. of the result of a `/.well-known/core` response,
the lifetime of an RDAO option and
of DNS responses.
Any rate limits and persistent errors from the "Finding a Resource Directory" step
must be considered for the whole registration time,
not only for a single operation.


The following example shows a registrant-ep with the name "node1" registering
two resources to an RD using this interface. The location "/rd"
is an example RD location discovered in a request similar to {{example-discovery}}.

~~~~
Req: POST coap://rd.example.com/rd?ep=node1
Content-Format: 40
Payload:
</sensors/temp>;ct=41;rt="temperature-c";if="sensor",
<http://www.example.com/sensors/temp>;
  anchor="/sensors/temp";rel="describedby"

Res: 2.01 Created
Location-Path: /rd/4521
~~~~
{: #example-payload title="Example registration payload" }

A Resource Directory may optionally support HTTP. Here is an example of almost the same registration operation above, when done using HTTP.

~~~~
Req:
POST /rd?ep=node1&base=http://[2001:db8:1::1] HTTP/1.1
Host: example.com
Content-Type: application/link-format

</sensors/temp>;ct=41;rt="temperature-c";if="sensor",
<http://www.example.com/sensors/temp>;
  anchor="/sensors/temp";rel="describedby"

Res:
HTTP/1.1 201 Created
Location: /rd/4521
~~~~
{: #example-payload-http title="Example registration payload as expressed using HTTP" }

## Simple Registration {#simple}

Not all endpoints hosting resources are expected to know how to upload links to an RD as described in {{registration}}. Instead, simple endpoints can implement the Simple Registration approach described in this section. An RD implementing this specification MUST implement Simple Registration. However, there may
be security reasons why this form of directory discovery would be disabled.

This approach requires that the registrant-ep makes available the hosted resources
that it wants to be discovered, as links on its `/.well-known/core` interface as
specified in {{RFC6690}}.
The links in that document are subject to the same limitations as the payload of a registration
(with respect to {{limitedlinkformat}}).

* The registrant-ep finds one or more addresses of the directory server as described in {{finding_an_rd}}.

* The registrant-ep sends (and regularly refreshes with) a POST
request to the `/.well-known/core` URI of the directory server of choice. The body of the POST request is empty, and triggers the resource
directory server to perform GET requests at the requesting registrant-ep's /.well-known/core to obtain the link-format payload to register.

  The registrant-ep includes the same registration parameters in the POST request as it would per {{registration}}. The registration base URI of the registration is taken from the registrant-ep's network address (as is default with regular registrations).

  Example request from registrant-EP to RD (unanswered until the next step):

~~~~
Req: POST /.well-known/core?lt=6000&ep=node1
(No payload)
~~~~
{: #example-simple1 title="First half example exchange of a simple registration" }

* The Resource Directory queries the registrant-ep's discovery resource to determine the success of the operation.
It SHOULD keep a cache of the discovery resource and not query it again as long as it is fresh.

  Example request from the RD to the registrant-EP:

~~~~
Req: GET /.well-known/core
Accept: 40

Res: 2.05 Content
Content-Format: 40
Payload:
</sen/temp>
~~~~
{: #example-simple2 title="Example exchange of the RD querying the simple endpoint" }

  With this response, the RD would answer the previous step's request:

~~~~
Res: 2.04 Changed
~~~~
{: #example-simple3 title="Second half example exchange of a simple registration" }

The sequence of fetching the registration content before sending a successful response
was chosen to make responses reliable,
and the caching item was chosen to still allow very constrained registrants.
Registrants MUST be able to serve a GET request to `/.well-known/core` after having requested registration.
Constrained devices MAY regard the initial request as temporarily failed when they need RAM occupied by their own request to serve the RD's GET,
and retry later when the RD already has a cached representation of their discovery resources.
Then, the RD can reply immediately and the registrant can receive the response.

The simple registration request interface is specified as follows:

Interaction:
: EP -> RD


Method:
: POST


URI Template:
: /.well-known/core{?ep,d,lt,extra-attrs\*}


URI Template Variables are as they are for registration in {{registration}}.
The base attribute is not accepted to keep the registration interface simple;
that rules out registration over CoAP-over-TCP or HTTP that would need to specify one.

The following response is expected on this interface:

Success:
: 2.04 "Changed".


For the second interaction triggered by the above, the registrant-ep takes the role of server and the RD the role of client.
(Note that this is exactly the Well-Known Interface of {{RFC6690}} Section 4):
<!-- the above paragraph could just as well be any other text;
what matters is that the tables above and below are clearly separated. -->

Interaction:
: RD -> EP


Method:
: GET


URI Template:
: /.well-known/core


The following response is expected on this interface:

Success:
: 2.05 "Content".


The RD MUST delete registrations created by simple registration after the expiration of their lifetime. Additional operations on the registration resource cannot be executed because no registration location is returned.


## Third-party registration {#third-party-registration}

For some applications, even Simple Registration may be too taxing
for some very constrained devices, in particular if the security requirements
become too onerous.

In a controlled environment (e.g. building control), the Resource Directory
can be filled by a third party device, called a Commissioning Tool (CT). The commissioning
tool can fill the Resource Directory from a database or other means. For
that purpose scheme, IP address and port of the URI of the registered device is
 the value of the "base" parameter of the registration described in {{registration}}.

It should be noted that the value of the "base" parameter applies to all the links of the registration and has consequences for the anchor value of the individual links as exemplified in {{weblink}}. An eventual (currently non-existing) "base" attribute of the link is not affected by the value of "base" parameter in the registration.

## Operations on the Registration Resource

This section describes how the registering endpoint can maintain the registrations that it created. The registering endpoint can be the registrant-ep or the CT. An endpoint SHOULD NOT use this interface for registrations that it did not create. The registrations are resources of the RD.

After the initial registration, the registering endpoint retains the returned location of the Registration Resource for further operations, including refreshing the registration in order to extend the lifetime and "keep-alive" the registration. When the lifetime of the registration has expired, the RD SHOULD NOT respond to discovery queries concerning this endpoint. The RD SHOULD continue to provide access to the Registration Resource after a registration time-out occurs in order to enable the registering endpoint to eventually refresh the registration. The RD MAY eventually remove the registration resource for the purpose of garbage collection. If the Registration Resource is removed, the corresponding endpoint will need to be re-registered.

The Registration Resource may also be used cancel the registration using DELETE, and to perform further operations beyond the scope of this specification.

These operations are described below.

### Registration Update {#update}

The update interface is used by the registering endpoint to refresh or update its
registration with an RD. To use the interface, the registering endpoint sends a POST request to the registration resource returned by the initial registration operation.

An update MAY update the lifetime or the base URI registration parameters
"lt", "base" as in {{registration}}. Parameters that are not being changed SHOULD NOT
be included in an update. Adding parameters that have not changed increases
the size of the message but does not have any other implications.
Parameters MUST be included as query parameters in an update operation as
in {{registration}}.

A registration update resets the timeout of the registration to the (possibly
updated) lifetime of the registration, independent of whether a `lt` parameter
was given.

If the base URI of the registration is changed in an update,
relative references submitted in the original registration or later updates are resolved anew against the new base.

The registration update operation only describes the use of POST with an empty payload.
Future standards might describe the semantics of using content formats and payloads
with the POST method to update the links of a registration (see {{link-up}}).

The update registration request interface is specified as follows:

Interaction:
: EP -> RD

Method:
: POST

URI Template:
: {+location}{?lt,base,extra-attrs\*}


URI Template Variables:
: location :=
  : This is the Location returned by the RD as a result of a successful
    earlier registration.

  lt :=
  : Lifetime (optional). Lifetime of the registration in seconds. Range of 1-4294967295.
    If no lifetime is included, the previous last
  lifetime set on a previous update or the original registration
  (falling back to 90000) SHOULD be used.


  base :=
  : Base URI (optional). This parameter updates the Base URI established in the
    original registration to a new value.

    If the parameter is set in an update, it is stored by the RD as the new
    Base URI under which to interpret the relative links present in the payload of the original registration, following
    the same restrictions as in the registration.

    If the parameter is not set in the request but was set before, the previous
    Base URI value is kept unmodified.

    If the parameter is not set in the request and was not set before either, the
    source address and source port of the update request are stored as the
    Base URI.

  extra-attrs :=
  : Additional registration attributes (optional). As with the registration,
    the RD processes them if it knows their semantics. Otherwise, unknown
    attributes are stored as endpoint attributes, overriding any previously
    stored endpoint attributes of the same key.

  : Note that this default behavior does not allow removing an endpoint attribute in an update.
    For attributes whose functionality depends on the endpoints' ability to remove them in an update,
    it can make sense to define a value whose presence is equivalent to the absence of a value.
    As an alternative, an extension can define different updating rules for their attributes.
    That necessitates either discovery of whether the RD is aware of that extension,
    or tolerating the default behavior.

Content-Format:
: none (no payload)

The following responses are expected on this interface:

Success:
: 2.04 "Changed" or 204 "No Content" if the update was successfully processed.

Failure:
: 4.04 "Not Found" or 404 "Not Found". Registration does not exist (e.g. may have been removed).

If the registration fails in any way, including "Not Found" and request timeouts,
or if the time indicated in a Service Unavailable Max-Age/Retry-After exceeds the remaining lifetime,
the registering endpoint SHOULD attempt registration again.


The following example shows how the registering endpoint updates its registration resource at
an RD using this interface with the example location value: /rd/4521.

~~~~
Req: POST /rd/4521

Res: 2.04 Changed
~~~~
{: #example-update title="Example update of a registration" }

The following example shows the registering endpoint updating its registration resource at
an RD using this interface with the example location value: /rd/4521. The initial registration by the registering endpoint set the following values:

* endpoint name (ep)=endpoint1
* lifetime (lt)=500
* Base URI (base)=coap://local-proxy-old.example.com:5683
* payload of {{example-payload}}

The initial state of the Resource Directory is reflected in the following request:

~~~~
Req: GET /rd-lookup/res?ep=endpoint1

Res: 2.05 Content
Payload:
<coap://local-proxy-old.example.com:5683/sensors/temp>;ct=41;
    rt="temperature-c";if="sensor";
    anchor="coap://local-proxy-old.example.com:5683/",
<http://www.example.com/sensors/temp>;
    anchor="coap://local-proxy-old.example.com:5683/sensors/temp";rel="describedby"
~~~~
{: #example-update-base-lookup-pre title="Example lookup before a change to the base address" }

The following example shows the registering endpoint changing the Base URI to `coaps://new.example.com:5684`:

~~~~
Req: POST /rd/4521?base=coaps://new.example.com:5684

Res: 2.04 Changed
~~~~
{: #example-update-base title="Example registration update that changes the base address" }

The consecutive query returns:

~~~~
Req: GET /rd-lookup/res?ep=endpoint1

Res: 2.05 Content
Payload:
<coap://new.example.com:5684/sensors/temp>;ct=41;
    rt="temperature-c";if="sensor";
    anchor="coap://new.example.com:5684/",
<http://www.example.com/sensors/temp>;
    anchor="coap://new.example.com:5684/sensors/temp";rel="describedby"
~~~~
{: #example-update-base-lookup-post title="Example lookup after a change to the base address" }

### Registration Removal {#removal}

Although RD registrations have soft state and will eventually timeout after their
lifetime, the registering endpoint SHOULD explicitly remove an entry from the RD if it
knows it will no longer be available (for example on shut-down). This is
accomplished using a removal interface on the RD by performing a DELETE on
the endpoint resource.

The removal request interface is specified as follows:

Interaction:
: EP -> RD

Method:
: DELETE

URI Template:
: {+location}

URI Template Variables:
: location :=
  : This is the Location returned by the RD as a result of a successful
    earlier registration.

The following responses are expected on this interface:

Success:
: 2.02 "Deleted" or 204 "No Content" upon successful deletion

Failure:
: 4.04 "Not Found" or 404 "Not Found". Registration does not exist (e.g. may already have been removed).

The following examples shows successful removal of the endpoint from the RD with example location value /rd/4521.


~~~~
Req: DELETE /rd/4521

Res: 2.02 Deleted
~~~~
{: #example-removal title="Example of a registration removal" }


### Further operations {#link-up}

Additional operations on the registration can be specified in future documents, for example:

* Send iPATCH (or PATCH) updates ({{RFC8132}}) to add, remove or change the links of a registration.
* Use GET to read the currently stored set of links in a registration resource.

Those operations are out of scope of this document, and will require media types suitable for modifying sets of links.

# RD Lookup {#lookup}

To discover the resources registered with the RD,
a lookup interface must be provided. This lookup interface
is defined as a default, and it is assumed that RDs may also support lookups
to return resource descriptions in alternative formats (e.g. JSON or CBOR link format {{I-D.ietf-core-links-json}})
or using more advanced interfaces (e.g. supporting context or semantic
based lookup) on different resources that are discovered independently.

RD Lookup allows lookups for endpoints and resources
using attributes defined in this document and for use with the CoRE
Link Format. The result of a lookup request is the list of links (if any)
corresponding to the type of lookup.  Thus, an endpoint lookup MUST return a list of endpoints and a resource lookup MUST return a list of links to resources.

The lookup type is selected by a URI endpoint, which is indicated by a Resource Type as per {{lookup-types}} below:

| Lookup Type | Resource Type | Mandatory |
| Resource | core.rd-lookup-res | Mandatory |
| Endpoint | core.rd-lookup-ep | Mandatory |
{: #lookup-types title='Lookup Types'}

## Resource lookup

Resource lookup results in links that are semantically equivalent to the links submitted to the RD.
The links and link parameters returned by the lookup are equal to the submitted ones,
except that the target and anchor references are fully resolved.

Links that did not have an anchor attribute are therefore returned with the  base URI of the registration as the anchor.
Links of which href or anchor was submitted as a (full) URI are returned with these attributes unmodified.

Above rules allow the client to interpret the response as links without any further knowledge of the storage conventions of the RD.
The Resource Directory MAY replace the registration base URIs with a configured intermediate proxy, e.g. in the case of an HTTP lookup interface for CoAP endpoints.

If the base URI of a registration contains a link-local address,
the RD MUST NOT show its links unless the lookup was made from the
same link.
The RD MUST NOT include zone identifiers in the resolved URIs.


## Lookup filtering

Using the Accept Option, the requester can control whether the returned list is returned in CoRE Link Format (`application/link-format`, default) or in alternate content-formats (e.g. from {{I-D.ietf-core-links-json}}).

The page and count parameters are used to obtain lookup results in specified increments using pagination, where count specifies how many links to return and page specifies which subset of links organized in sequential pages, each containing 'count' links, starting with link zero and page zero. Thus, specifying count of 10 and page of 0 will return the first 10 links in the result set (links 0-9). Count = 10 and page = 1 will return the next 'page' containing links 10-19, and so on.

Multiple search criteria MAY be included in a lookup. All included criteria MUST match for a link to be returned. The Resource Directory MUST support matching with multiple search criteria.

A link matches a search criterion if it has an attribute of the same name and the same value, allowing for a trailing "\*" wildcard operator as in Section 4.1 of {{RFC6690}}.
Attributes that are defined as "link-type" match if the search value matches any of their values (see Section 4.1 of {{RFC6690}}; e.g. `?if=core.s` matches `;if="abc core.s";`).
A resource link also matches a search criterion if its endpoint would match the criterion, and vice versa, an endpoint link matches a search criterion if any of its resource links matches it.

Note that `href` is a valid search criterion and matches target references. Like all search criteria, on a resource lookup it can match the target reference of the resource link itself, but also the registration resource of the endpoint that registered it.
Queries for resource link targets MUST be in URI form (i.e. not relative references) and are matched against a resolved link target. Queries for endpoints SHOULD be expressed in path-absolute form if possible and MUST be expressed in URI form otherwise; the RD SHOULD recognize either.
The `anchor` attribute is usable for resource lookups, and, if queried, MUST be for in URI form as well.

Endpoints that are interested in a lookup result repeatedly or continuously can use
mechanisms like ETag caching, resource observation ({{RFC7641}}),
or any future mechanism that might allow more efficient observations of collections.
These are advertised, detected and used according to their own specifications
and can be used with the lookup interface as with any other resource.

When resource observation is used,
every time the set of matching links changes, or the content of a matching link changes, the RD sends a notification with the matching link set.
The notification contains the successful current response to the given request,
especially with respect to representing zero matching links
(see "Success" item below).

The lookup interface is specified as follows:

Interaction:
: Client -> RD

Method:
: GET

URI Template:
: {+type-lookup-location}{?page,count,search\*}


URI Template Variables:
: type-lookup-location :=
  : RD Lookup URI for a given lookup type (mandatory). The address is
    discovered as described in {{discovery}}.

  search :=
  : Search criteria for limiting the number of results (optional).

  page :=
  : Page (optional). Parameter cannot be used without the count
    parameter. Results are returned from result set in pages that contain
    'count' links starting from index (page \* count). Page numbering starts
    with zero.

  count :=
  : Count (optional). Number of results is limited to this parameter value. If
    the page parameter is also present, the response MUST only include 'count'
    links starting with the (page \* count) link in the result set from the query. If
    the count parameter is not present, then the response MUST return all matching
    links in the result set. Link numbering starts with zero.

Accept:
: absent, application/link-format or any other indicated media type representing web links


The following responses codes are defined for this interface:

Success:
: 2.05 "Content" or 200 "OK" with an `application/link-format` or other web link payload containing matching entries for the lookup.

  The payload can contain zero links (which is an empty payload in {{RFC6690}} link format, but could also be `[]` in JSON based formats),
  indicating that no entities matched the request.


##  Resource lookup examples

The examples in this section assume the existence of CoAP hosts with a default CoAP port 61616. HTTP hosts are possible and do not change the nature of the examples.

The following example shows a client performing a resource lookup with the example resource look-up locations discovered in {{example-discovery}}:

~~~~
Req: GET /rd-lookup/res?rt=temperature

Res: 2.05 Content
<coap://[2001:db8:3::123]:61616/temp>;rt="temperature";
           anchor="coap://[2001:db8:3::123]:61616"
~~~~
{: #example-lookup-res title="Example a resource lookup" }

A client that wants to be notified of new resources as they show up can use
observation:

~~~~
Req: GET /rd-lookup/res?rt=light
Observe: 0

Res: 2.05 Content
Observe: 23
Payload: empty

(at a later point in time)

Res: 2.05 Content
Observe: 24
Payload:
<coap://[2001:db8:3::124]/west>;rt="light";
    anchor="coap://[2001:db8:3::124]",
<coap://[2001:db8:3::124]/south>;rt="light";
    anchor="coap://[2001:db8:3::124]",
<coap://[2001:db8:3::124]/east>;rt="light";
    anchor="coap://[2001:db8:3::124]"
~~~~
{: #example-lookup-obs title="Example an observing resource lookup" }

The following example shows a client performing a paginated resource lookup

~~~~
Req: GET /rd-lookup/res?page=0&count=5

Res: 2.05 Content
<coap://[2001:db8:3::123]:61616/res/0>;rt=sensor;ct=60;
    anchor="coap://[2001:db8:3::123]:61616",
<coap://[2001:db8:3::123]:61616/res/1>;rt=sensor;ct=60;
    anchor="coap://[2001:db8:3::123]:61616",
<coap://[2001:db8:3::123]:61616/res/2>;rt=sensor;ct=60;
    anchor="coap://[2001:db8:3::123]:61616",
<coap://[2001:db8:3::123]:61616/res/3>;rt=sensor;ct=60;
    anchor="coap://[2001:db8:3::123]:61616",
<coap://[2001:db8:3::123]:61616/res/4>;rt=sensor;ct=60;
    anchor="coap://[2001:db8:3::123]:61616"

Req: GET /rd-lookup/res?page=1&count=5

Res: 2.05 Content
<coap://[2001:db8:3::123]:61616/res/5>;rt=sensor;ct=60;
    anchor="coap://[2001:db8:3::123]:61616",
<coap://[2001:db8:3::123]:61616/res/6>;rt=sensor;ct=60;
    anchor="coap://[2001:db8:3::123]:61616",
<coap://[2001:db8:3::123]:61616/res/7>;rt=sensor;ct=60;
    anchor="coap://[2001:db8:3::123]:61616",
<coap://[2001:db8:3::123]:61616/res/8>;rt=sensor;ct=60;
    anchor="coap://[2001:db8:3::123]:61616",
<coap://[2001:db8:3::123]:61616/res/9>;rt=sensor;ct=60;
    anchor="coap://[2001:db8:3::123]:61616"
~~~~
{: #example-lookup-page title="Examples of paginated resource lookup" }

The following example shows a client performing a lookup of all resources
of all endpoints of a given endpoint type. It assumes that two endpoints (with endpoint
names `sensor1` and `sensor2`) have previously registered with their respective
addresses `coap://sensor1.example.com` and `coap://sensor2.example.com`, and
posted the very payload of the 6th request of section 5 of {{RFC6690}}.

It demonstrates how absolute link targets stay unmodified, while relative ones
are resolved:

~~~~
Req: GET /rd-lookup/res?et=oic.d.sensor

<coap://sensor1.example.com/sensors>;ct=40;title="Sensor Index";
    anchor="coap://sensor1.example.com",
<coap://sensor1.example.com/sensors/temp>;rt="temperature-c";
    if="sensor"; anchor="coap://sensor1.example.com",
<coap://sensor1.example.com/sensors/light>;rt="light-lux";
    if="sensor"; anchor="coap://sensor1.example.com",
<http://www.example.com/sensors/t123>;rel="describedby";
    anchor="coap://sensor1.example.com/sensors/temp",
<coap://sensor1.example.com/t>;rel="alternate";
    anchor="coap://sensor1.example.com/sensors/temp",
<coap://sensor2.example.com/sensors>;ct=40;title="Sensor Index";
    anchor="coap://sensor2.example.com",
<coap://sensor2.example.com/sensors/temp>;rt="temperature-c";
    if="sensor"; anchor="coap://sensor2.example.com",
<coap://sensor2.example.com/sensors/light>;rt="light-lux";
    if="sensor"; anchor="coap://sensor2.example.com",
<http://www.example.com/sensors/t123>;rel="describedby";
    anchor="coap://sensor2.example.com/sensors/temp",
<coap://sensor2.example.com/t>;rel="alternate";
    anchor="coap://sensor2.example.com/sensors/temp"
~~~~
{: #example-lookup-multiple title="Example of resource lookup from multiple endpoints" }

## Endpoint lookup {#ep-lookup}

The endpoint lookup returns registration resources which can only be manipulated by the registering endpoint.

Endpoint registration resources are annotated with their endpoint names (ep), sectors (d, if present) and registration base URI (base; reports the registrant-ep's address if no explicit base was given) as well as a constant resource type (rt="core.rd-ep"); the lifetime (lt) is not reported.
Additional endpoint attributes are added as target attributes to their endpoint link unless their specification says otherwise.

Links to endpoints SHOULD be presented in path-absolute form or, if required, as (full) URIs. (This avoids the RFC6690 ambiguities.)

Base addresses that contain link-local addresses MUST NOT include zone identifiers,
and such registrations <!-- or "' base attributes" --> MUST NOT be
shown unless the lookup was made from the same link from which the
registration was made.

While Endpoint Lookup does expose the registration resources,
the RD does not need to make them accessible to clients.
Clients SHOULD NOT attempt to dereference or manipulate them.

A Resource Directory can report endpoints in lookup that are not hosted at the same address.
Lookup clients MUST be prepared to see arbitrary URIs as registration resources in the results
and treat them as opaque identifiers;
the precise semantics of such links are left to future specifications.

The following example shows a client performing an endpoint type (et) lookup with  the value oic.d.sensor (which is currently a registered rt value):

~~~~
Req: GET /rd-lookup/ep?et=oic.d.sensor

Res: 2.05 Content
</rd/1234>;base="coap://[2001:db8:3::127]:61616";ep="node5";
et="oic.d.sensor";ct="40";rt="core.rd-ep",
</rd/4521>;base="coap://[2001:db8:3::129]:61616";ep="node7";
et="oic.d.sensor";ct="40";d="floor-3";rt="core.rd-ep"
~~~~
{: #example-lookup-ep title="Examples of endpoint lookup" }


# Security policies {#policies}

The Resource Directory (RD) provides assistance to applications situated on a selection of nodes to discover endpoints on connected nodes. This section discusses different security aspects of accessing the RD.

The contents of the RD are inserted in two ways:

1.  The node hosting the discoverable endpoint fills the RD with the contents of /.well-known/core by:
     * Storing the contents directly into RD (see {{registration}})
     * Requesting the RD to load the contents from /.well-known/core (see {{simple}})

2.  A Commissioning Tool (CT) fills the RD with endpoint information for a set of discoverable nodes. (see {{registration}} with base=authority parameter value)

In both cases, the nodes filling the RD should be authenticated and authorized to change the contents of the RD. An Authorization Server (AS) is responsible to assign a token to the registering node to authorize the node to discover or register endpoints in a given RD {{I-D.ietf-ace-oauth-authz}}.

It can be imagined that an installation is divided in a set of security regions, each one with its own RD(s) to discover the endpoints that are part of a given security region. An endpoint that wants to discover an RD, responsible for a given region, needs to be authorized to learn the contents of a given RD. Within a region, for a given RD, a more fine-grained security division is possible based on the values of the endpoint registration parameters. Authorization to discover endpoints with a given set of filter values is recommended for those cases.

When a node registers its endpoints, criteria are needed to authorize the node to enter them. An important aspect is the uniqueness of the (endpoint name, and optional sector) pair within the RD. Consider the two cases separately: (1) CT registers endpoints, and (2) the registering node registers its own endpoint(s).

   * A CT needs authorization to register a set of endpoints. This authorization can be based on the region, i.e. a given CT is authorized to register any endpoint (endpoint name, sector) into a given RD, or to register an endpoint with (endpoint name, sector) value pairs assigned by the AS, or can be more fine-grained, including a subset of registration parameter values.
   * A given endpoint that registers itself, needs to proof its possession of its unique (endpoint name, sector) value pair. Alternatively, the AS can authorize the endpoint to register with an (endpoint name, sector) value pair assigned by the AS.

A separate document needs to specify these aspects to ensure interoperability between registering nodes and RD. The subsections below give some hints how to handle a subset of the different aspects.

## Secure RD discovery

The Resource Server (RS) discussed in {{I-D.ietf-ace-oauth-authz}} is equated to the RD. The client (C) needs to discover the RD as discussed in {{finding_an_rd}}. C can discover the related AS by sending a request to the RD. The RD denies the request by sending the address of the related AS, as discussed in section 5.1 of {{I-D.ietf-ace-oauth-authz}}.
The client MUST send an authorization request to the AS. When appropriate, the AS returns a token that specifies the authorization permission which needs to be specified in a separate document.

## Secure RD filtering

The authorized parameter values for the queries by a given endpoint must be registered by the AS. The AS communicates the parameter values in the token. A separate document needs to specify the parameter value combinations and their storage in the token. The RD decodes the token and checks the validity of the queries of the client.

## Secure endpoint Name assignment {#secure-ep}

This section only considers the assignment of a name to the endpoint based on an automatic mechanism without use of AS. More elaborate protocols are out of scope. The registering endpoint is authorized by the AS to discover the RD and add registrations. A token is provided by the AS and communicated from registering endpoint to RD.  It is assumed that DTLS is used to secure the channel between registering endpoint and RD, where the registering endpoint is the DTLS client. Assuming that the client is provided by a certificate at manufacturing time, the certificate is uniquely identified by the CN field and the serial number (see {{?RFC5280}} Section 4.1.2.2). The RD can assign a unique endpoint name by using the certificate identifier as endpoint name. Proof of possession of the endpoint name by the registering endpoint is checked by encrypting the certificate identifier with the private key of the registering endpoint, which the RD can decrypt with the public key stored in the certificate.
Even simpler, the authorized registering endpoint can generate a random number (or string) that identifies the endpoint. The RD can check for the improbable replication of the random value. The RD MUST check that registering endpoint uses only one random value for each authorized endpoint.


# Security Considerations

The security considerations as described in Section 5 of {{RFC8288}} and
Section 6 of {{RFC6690}} apply. The `/.well-known/core` resource may be
protected e.g. using DTLS when hosted on a CoAP server as described in
{{RFC7252}}. DTLS or TLS based security SHOULD be used on all resource
directory interfaces defined in this document<!-- TODO: Improve the exact DTLS
or TLS security requirements and references  -->.

## Endpoint Identification and Authentication {#endpoint_identification}

An Endpoint (name, sector) pair is unique within the et of endpoints registered by the RD. An Endpoint MUST NOT be identified by its protocol, port or IP
address as these may change over the lifetime of an Endpoint.

Every operation performed by an Endpoint on a resource directory
SHOULD be mutually authenticated using Pre-Shared Key, Raw Public Key or
Certificate based security.

Consider the following threat: two devices A and B are registered at a single server. Both devices have unique, per-device credentials for use with DTLS to make sure that only parties with authorization to access A or B can do so.

Now, imagine that a malicious device A wants to sabotage the device B. It uses its credentials during the DTLS exchange. Then, it specifies the
endpoint name of device B as the name of its own endpoint in device A. If the server does not check
whether the identifier provided in the DTLS handshake matches the
identifier used at the CoAP layer then it may be inclined to use the
endpoint name for looking up what information to provision to the malicious device.

{{secure-ep}} specifies an example that removes this threat for endpoints that have a certificate installed.


## Access Control

Access control SHOULD be performed separately for the RD registration and Lookup
API paths, as different endpoints may be authorized to register
with an RD from those authorized to lookup endpoints from the RD. Such access
control SHOULD be performed in as fine-grained a level as possible. For example
access control for lookups could be performed either at the sector, endpoint
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
attacks. There is also a danger that NTP Servers could become implicated in denial-of-service (DoS) attacks since they run on unprotected UDP, there
is no return routability check, and they can have a large amplification factor.
The responses from the NTP server were found to be
19 times larger than the request. A Resource Directory (RD) which responds
to wild-card lookups is potentially vulnerable if run with CoAP over UDP.
Since there is no return routability check and the responses can be significantly
larger than requests, RDs can unknowingly become part of a DDoS amplification
attack.




# IANA Considerations

## Resource Types {#iana-rt}

IANA is asked to enter the following values into the Resource Type (rt=) Link
Target Attribute Values sub-registry of the Constrained Restful Environments
(CoRE) Parameters registry defined in {{RFC6690}}:

| Value               | Description                           | Reference              |
| core.rd             | Directory resource of an RD           | RFCTHIS {{discovery}} |
| core.rd-lookup-res  | Resource lookup of an RD              | RFCTHIS {{discovery}} |
| core.rd-lookup-ep   | Endpoint lookup of an RD              | RFCTHIS {{discovery}} |
| core.rd-ep          | Endpoint resource of an RD            | RFCTHIS {{lookup}}    |


## IPv6 ND Resource Directory Address Option

This document registers one new ND option type under the sub-registry "IPv6 Neighbor Discovery Option Formats":

* Resource Directory Address Option (TBD38)

\[ The RFC editor is asked to replace TBD38
with the assigned number in the document;
the value 38 is suggested. \]

## RD Parameter Registry {#iana-registry}

This specification defines a new sub-registry for registration and lookup
parameters called "RD Parameters" under "CoRE Parameters". Although this
specification defines a basic set of parameters, it is expected that other
standards that make use of this interface will define new ones.

Each entry in the registry must include

* the human readable name of the parameter,
* the short name as used in query parameters or target attributes,
* indication of whether it can be passed as a query parameter at registration of endpoints, as a query parameter in lookups, or be expressed as a target attribute,
* validity requirements if any,
* a description,
* and a link to reference documentation.

The query parameter MUST be both a valid URI query key {{RFC3986}} and a token as used in {{RFC8288}}.

The description must give details on whether the parameter can be updated, and how it is to be processed in lookups.

The mechanisms around new RD parameters should be designed in such a way that they tolerate RD implementations that are unaware of the parameter and expose any parameter passed at registration or updates on in endpoint lookups. (For example, if a parameter used at registration were to be confidential, the registering endpoint should be instructed to only set that parameter if the RD advertises support for keeping it confidential at the discovery step.)

Initial entries in this sub-registry are as follows:

| Full name             | Short | Validity           | Use | Description                                                             |
| Endpoint Name         | ep    | Unicode*           | RLA | Name of the endpoint                                                    |
| Lifetime              | lt    | 1-4294967295       | R   | Lifetime of the registration in seconds                                 |
| Sector                | d     | Unicode*           | RLA | Sector to which this endpoint belongs                                   |
| Registration Base URI | base  | URI                | RLA | The scheme, address and port and path at which this server is available |
| Page                  | page  | Integer            |  L  | Used for pagination                                                     |
| Count                 | count | Integer            |  L  | Used for pagination                                                     |
| Endpoint Type         | et    | {{et-description}} | RLA | Semantic type of the endpoint (see {{et-registry}})                     |
{: #tab-registry title='RD Parameters' }

(Short: Short name used in query parameters or target attributes. Validity: Unicode* = 63 Bytes of UTF-8 encoded Unicode, with no control characters as per {{registration}}. Use: R = used at registration, L = used at lookup, A = expressed in target attribute

The descriptions for the options defined in this document are only summarized here.
To which registrations they apply and when they are to be shown is described in the respective sections of this document.
All their reference documentation entries point to this document.

The IANA policy for future additions to the sub-registry is "Expert Review"
as described in {{RFC8126}}. The evaluation should consider
formal criteria,
duplication of functionality (Is the new entry redundant with an existing one?),
topical suitability (E.g. is the described property actually a property of the endpoint and not a property of a particular resource, in which case it should go into the payload of the registration and need not be registered?),
and the potential for conflict with commonly used target attributes (For example, `if` could be used as a parameter for conditional registration if it were not to be used in lookup or attributes, but would make a bad parameter for lookup, because a resource lookup with an `if` query parameter could ambiguously filter by the registered endpoint property or the {{RFC6690}} target attribute).
It is expected that the registry will receive between 5 and 50 registrations in total over the next years.

### Full description of the "Endpoint Type" Registration Parameter {#et-description}

An endpoint registering at an RD can describe itself with endpoint types,
similar to how resources are described with Resource Types in {{RFC6690}}.
An endpoint type is expressed as a string, which can be either a URI or one of
the values defined in the Endpoint Type sub-registry.
Endpoint types can be passed in the `et` query parameter as part of extra-attrs
at the Registration step,
are shown on endpoint lookups using the `et` target attribute,
and can be filtered for using `et` as a search criterion in resource and
endpoint lookup.
Multiple endpoint types are given as separate query parameters or link
attributes.

Note that Endpoint Type differs from Resource Type in that it uses multiple
attributes rather than space separated values.
As a result, Resource Directory implementations automatically support correct
filtering in the lookup interfaces from the rules for unknown endpoint
attributes.

## "Endpoint Type" (et=) RD Parameter values {#et-registry}

This specification establishes a new sub-registry under "CoRE Parameters"
called '"Endpoint Type" (et=) RD Parameter values'.
The registry properties (required policy, requirements, template) are identical
to those of the Resource Type parameters in {{RFC6690}}, in short:

The review policy is IETF Review for values starting with "core", and
Specification Required for others.

The requirements to be enforced are:

* The values MUST be related to the purpose described in {{et-description}}.

* The registered values MUST conform to the ABNF reg-rel-type definition of
  {{RFC6690}} and MUST NOT be a URI.

* It is recommended to use the period "." character for segmentation.

The registry initially contains one value:

 * "core.rd-group": An application group as described in {{groups}}.


## Multicast Address Registration {#mc-registration}

   <!-- IANA has assigned -->
   IANA is asked to assign
   the following multicast addresses for use by CoAP nodes:

   IPv4  -- "all CoRE resource directories" address MCD2 (suggestion: 224.0.1.189), from the "IPv4
      Multicast Address Space Registry".  As the address is used for
      discovery that may span beyond a single network, it has come from
      the Internetwork Control Block (224.0.1.x, RFC 5771).

   IPv6  -- "all CoRE resource directories" address MCD1 (suggestions FF0X::FE), from the "IPv6 Multicast
      Address Space Registry", in the "Variable Scope Multicast
      Addresses" space (RFC 3307).  Note that there is a distinct
      multicast address for each scope that interested CoAP nodes should
      listen to; CoAP needs the Link-Local and Site-Local scopes only.

\[ The RFC editor is asked to replace MCD1 and MCD2
with the assigned addresses throughout the document. \]

## Well-Kown URIs

IANA is asked to extend
<!-- IANA has extended -->
the reference for the "core" URI suffix
in the "Well-Known URIs" registry
to reference this document next to {{RFC6690}},
as this defines the resource's behavior for POST requests.

## Service Names and Transport Protocol Port Number Registry

IANA is asked to enter a new item into the Service Names and Transport Protocol Port Number Registry:

* Service name: "core-rd"
* Protocol: "udp"
* Description: "Resource Directory accessed using CoAP"
* Reference: this document

# Examples {#examples}

Two examples are presented: a Lighting Installation example in {{lt-ex}} and a LWM2M example in {{lwm2m-ex}}.

## Lighting Installation {#lt-ex}

This example shows a simplified lighting installation which makes use of
the Resource Directory (RD) with a CoAP interface to facilitate the installation and start-up of
the application code in the lights and sensors. In particular, the example
leads to the definition of a group and the enabling of the corresponding
multicast address as described in {{groups}}. No conclusions must be drawn on the realization of actual
installation or naming procedures, because the example only "emphasizes" some of the issues
that may influence the use of the RD and does not pretend to be normative.

### Installation Characteristics {#lt-in-ch}

The example assumes that the installation is managed. That means that a Commissioning
Tool (CT) is used to authorize the addition of nodes, name them, and name
their services. The CT can be connected to the installation in many ways:
the CT can be part of the installation network, connected by WiFi to the
installation network, or connected via GPRS link, or other method.

It is assumed that there are two naming authorities for the installation:
(1) the network manager that is responsible for the correct operation of
the network and the connected interfaces, and (2) the lighting manager that
is responsible for the correct functioning of networked lights and sensors.
The result is the existence of two naming schemes coming from the two managing
entities.

The example installation consists of one presence sensor, and two luminaries,
luminary1 and luminary2, each with their own wireless interface. Each luminary
contains three lamps: left, right and middle. Each luminary is accessible
through one endpoint. For each lamp a resource exists to modify the settings
of a lamp in a luminary. The purpose of the installation is that the presence
sensor notifies the presence of persons to a group of lamps. The group of
lamps consists of: middle and left lamps of luminary1 and right lamp of luminary2.

Before commissioning by the lighting manager, the network is installed and
access to the interfaces is proven to work by the network manager.

At the moment of installation, the network under installation is not necessarily
connected to the DNS infra structure. Therefore, SLAAC IPv6 addresses are
assigned to CT, RD, luminaries and sensor shown in {{interface-S}} below:

| Name | IPv6 address |
| luminary1 | 2001:db8:4::1 |
| luminary2 | 2001:db8:4::2 |
| Presence sensor | 2001:db8:4::3 |
| Resource directory | 2001:db8:4::ff |
{: #interface-S title='interface SLAAC addresses'}

In {{rd-en}} the use of resource directory during installation is
presented.


### RD entries {#rd-en}

It is assumed that access to the DNS infrastructure is not always possible
during installation. Therefore, the SLAAC addresses are used in this section.

For discovery, the resource types (rt) of the devices are important. The
lamps in the luminaries have rt: light, and the presence sensor has rt: p-sensor.
The endpoints have names which are relevant to the light installation manager.
In this case luminary1, luminary2, and the presence sensor are located in
room 2-4-015, where luminary1 is located at the window and luminary2 and
the presence sensor are located at the door. The endpoint names reflect
this physical location. The middle, left and right lamps are accessed via
path /light/middle, /light/left, and /light/right respectively. The identifiers
relevant to the Resource Directory are shown in {{endpoint}} below:

| Name | endpoint | resource path | resource type |
| luminary1 | lm_R2-4-015_wndw  | /light/left  |  light  |
| luminary1 | lm_R2-4-015_wndw  | /light/middle  |  light  |
| luminary1 | lm_R2-4-015_wndw  | /light/right  |  light  |
| luminary2 | lm_R2-4-015_door  | /light/left  |  light  |
| luminary2 | lm_R2-4-015_door  | /light/middle  |  light  |
| luminary2 | lm_R2-4-015_door  | /light/right  |  light  |
| Presence sensor | ps_R2-4-015_door  | /ps  |  p-sensor  |
{: #endpoint title='Resource Directory identifiers'}

It is assumed that the CT knows the RD's address, and has performed URI
discovery on it that returned a response like the one in the {{discovery}} example.

The CT inserts the endpoints of the luminaries and the sensor in the RD
using the registration base URI parameter (base) to specify the interface address:


~~~~
Req: POST coap://[2001:db8:4::ff]/rd
  ?ep=lm_R2-4-015_wndw&base=coap://[2001:db8:4::1]&d=R2-4-015
Payload:
</light/left>;rt="light",
</light/middle>;rt="light",
</light/right>;rt="light"

Res: 2.01 Created
Location-Path: /rd/4521

Req: POST coap://[2001:db8:4::ff]/rd
  ?ep=lm_R2-4-015_door&base=coap://[2001:db8:4::2]&d=R2-4-015
Payload:
</light/left>;rt="light",
</light/middle>;rt="light",
</light/right>;rt="light"

Res: 2.01 Created
Location-Path: /rd/4522

Req: POST coap://[2001:db8:4::ff]/rd
  ?ep=ps_R2-4-015_door&base=coap://[2001:db8:4::3]d&d=R2-4-015
Payload:
</ps>;rt="p-sensor"

Res: 2.01 Created
Location-Path: /rd/4523
~~~~
{: #example-lighting-1 title="Example of registrations a CT enters into an RD" }

The sector name d=R2-4-015 has been added for an efficient lookup because
filtering on "ep" name is more awkward. The same sector name is communicated to
the two luminaries and the presence sensor by the CT.

The group is specified in the RD. The base parameter is set to the site-local
multicast address allocated to the group.
In the POST in the example below, the resources supported by all group members are published.

~~~~
Req: POST coap://[2001:db8:4::ff]/rd
?ep=grp_R2-4-015&et=core.rd-group&base=coap://[ff05::1]
Payload:
</light/left>;rt="light",
</light/middle>;rt="light",
</light/right>;rt="light"

Res: 2.01 Created
Location-Path: /rd/501
~~~~
{: #example-lighting-2 title="Example of a multicast group a CT enters into an RD" }

After the filling of the RD by the CT, the application in the luminaries
can learn to which groups they belong, and enable their interface for the
multicast address.

The luminary, knowing its sector and being configured to join any group
containing lights, searches for candidate groups and joins them:

~~~~
Req: GET coap://[2001:db8:4::ff]/rd-lookup/ep
  ?d=R2-4-015&et=core.rd-group&rt=light

Res: 2.05 Content
</rd/501>;ep="grp_R2-4-015";et="core.rd-group";
          base="coap://[ff05::1]";rt="core.rd-ep"
~~~~
{: #example-lighting-3 title="Example of a lookup exchange to find suitable multicast addresses" }

From the returned base parameter value, the luminary learns the multicast address
of the multicast group.

Alternatively, the CT can communicate the multicast address directly to the
luminaries by using the "coap-group" resource specified in {{RFC7390}}.

~~~~
Req: POST coap://[2001:db8:4::1]/coap-group
Content-Format: application/coap-group+json
Payload:
{ "a": "[ff05::1]", "n": "grp_R2-4-015"}

Res: 2.01 Created
Location-Path: /coap-group/1
~~~~
{: #example-lighting-4 title="Example use of direct multicast address configuration" }

Dependent on the situation, only the address, "a", or the name, "n", is specified
in the coap-group resource.

The presence sensor can learn the presence of groups that support resources with rt=light in its own sector by sending the same request, as used by the luminary. The presence sensor learns the multicast address to use for sending messages to the luminaries.

## OMA Lightweight M2M (LWM2M) Example {#lwm2m-ex}

This example shows how the OMA LWM2M specification makes use of Resource Directory (RD).

OMA LWM2M is a profile for device services based on CoAP(OMA Name Authority). LWM2M defines a simple object model and a number of abstract interfaces and operations for device management and device service enablement.

An LWM2M server is an instance of an LWM2M middleware service layer, containing a Resource Directory along with other LWM2M interfaces defined by the LWM2M specification.

CoRE Resource Directory (RD) is used to provide the LWM2M Registration interface.

LWM2M does not provide for registration sectors and does not currently
use the rd-lookup interface.

The LWM2M specification describes a set of interfaces and a resource model used between a LWM2M device and an LWM2M server. Other interfaces, proxies, and applications are currently out of scope for LWM2M.

The location of the LWM2M Server and RD URI path is provided by the LWM2M Bootstrap process, so no dynamic discovery of the RD is used. LWM2M Servers and endpoints are not required to implement the /.well-known/core resource.

### The LWM2M Object Model {#lwm2m-obj}

The OMA LWM2M object model is based on a simple 2 level class hierarchy consisting of Objects and Resources.

An LWM2M Resource is a REST endpoint, allowed to be a single value or an array of values of the same data type.

An LWM2M Object is a resource template and container type that encapsulates a set of related resources. An LWM2M Object represents a specific type of information source; for example, there is a LWM2M Device Management object that represents a network connection, containing resources that represent individual properties like radio signal strength.

Since there may potentially be more than one of a given type object, for example more than one network connection, LWM2M defines instances of objects that contain the resources that represent a specific physical thing.

The URI template for LWM2M consists of a base URI followed by Object, Instance, and Resource IDs:

{/base-uri}{/object-id}{/object-instance}{/resource-id}{/resource-instance}

The five variables given here are strings.  base-uri can also have the
special value "undefined" (sometimes called "null" in RFC 6570).
Each of the variables object-instance, resource-id, and
resource-instance can be the special value "undefined" only if the
values behind it in this sequence also are "undefined".  As a special
case, object-instance can be "empty" (which is different from
"undefined") if resource-id is not "undefined".

<!--
[^_TEMPLATE_TODO]

[^_TEMPLATE_TODO]: This text needs some help from an RFC 6570 expert.
 -->

base-uri := Base URI for LWM2M resources or "undefined" for default (empty) base URI

object-id := OMNA (OMA Name Authority) registered object ID (0-65535)

object-instance := Object instance identifier (0-65535) or
"undefined"/"empty" (see above)) to refer to all instances of an object ID

resource-id := OMNA (OMA Name Authority) registered resource ID (0-65535) or "undefined" to refer to all resources within an instance

resource-instance := Resource instance identifier or "undefined" to refer to single instance of a resource

LWM2M IDs are 16 bit unsigned integers represented in decimal (no
leading zeroes except for the value 0) by URI format strings. For
example, a LWM2M URI might be:

~~~~
/1/0/1
~~~~

The base uri is empty, the Object ID is 1, the instance ID is 0, the
resource ID is 1, and the resource instance is "undefined". This
example URI points to internal resource 1, which represents the
registration lifetime configured, in instance 0 of a type 1 object
(LWM2M Server Object).

### LWM2M Register Endpoint {#lwm2m-reg}

LWM2M defines a registration interface based on the REST API, described in {{registration}}. The
RD registration URI path of the LWM2M Resource Directory is specified to be "/rd".

LWM2M endpoints register object IDs, for example </1>, to indicate that a particular object type is supported, and register object instances, for example </1/0>, to indicate that a particular instance of that object type exists.

Resources within the LWM2M object instance are not registered with the RD, but may be discovered by reading the resource links from the object instance using GET with a CoAP Content-Format of application/link-format. Resources may also be read as a structured object by performing a GET to the object instance with a Content-Format of senml+json.

When an LWM2M object or instance is registered, this indicates to the LWM2M server that the object and its resources are available for management and service enablement (REST API) operations.

LWM2M endpoints may use the following RD registration parameters as defined in {{tab-registry}} :


~~~~
ep - Endpoint Name
lt - registration lifetime
~~~~

Endpoint Name, Lifetime, and LWM2M Version are mandatory parameters for the register operation, all other registration parameters are optional.

Additional optional LWM2M registration parameters are defined:


|       Name       | Query |           Validity           | Description                                                    |
| Binding Mode     | b     | {"U",UQ","S","SQ","US","UQS"}| Available Protocols                                            |
|
| LWM2M Version    | ver   | 1.0                          | Spec Version
|
| SMS Number       | sms   |                              | MSISDN
{: #tab-lwm2m-registry title='LWM2M Additional Registration Parameters'}


The following RD registration parameters are not currently specified for use in LWM2M:


~~~~
et - Endpoint Type
base - Registration Base URI
~~~~

The endpoint registration must include a payload containing links to all supported objects and existing object instances, optionally including the appropriate link-format relations.

Here is an example LWM2M registration payload:


~~~~ linkformat
</1>,</1/0>,</3/0>,</5>
~~~~

This link format payload indicates that object ID 1 (LWM2M Server Object) is supported, with a single instance 0 existing, object ID 3 (LWM2M Device object) is supported, with a single instance 0 existing, and object 5 (LWM2M Firmware Object) is supported, with no existing instances.

### LWM2M Update Endpoint Registration {#lwm2m-regupdate}

The LwM2M update is really very similar to the registration update as described in {{update}}, with
the only difference that there are more parameters defined and
available. All the parameters listed in that section are also available
with the initial registration but are all optional:


~~~~
lt - Registration Lifetime
b - Protocol Binding
sms - MSISDN
link payload - new or modified links
~~~~

A Registration update is also specified to be used to update the LWM2M server whenever the endpoint's UDP port or IP address are changed.

### LWM2M De-Register Endpoint {#lwm2m-dereg}

LWM2M allows for de-registration using the delete method on the returned location from the initial registration operation. LWM2M de-registration proceeds as described in {{removal}}.


# Acknowledgments

Oscar Novo, Srdjan Krco, Szymon Sasin, Kerry Lynn, Esko Dijk, Anders
Brandt, Matthieu Vial, Jim Schaad, Mohit Sethi, Hauke Petersen, Hannes Tschofenig, Sampo Ukkola, Linyi
Tian, Jan Newmarch, Matthias Kovatsch, Jaime Jimenez and Ted Lemon have provided helpful comments, discussions and ideas to improve and
shape this document. Zach would also like to thank his colleagues from the
EU FP7 SENSEI project, where many of the resource directory concepts were
originally developed.


# Changelog

changes from -23 to -24

* Discovery using DNS-SD added again
* Minimum lifetime (lt) reduced from 60 to 1
* References added
* IANA considerations
  * added about .well-known/core resource
  * added DNS-SD service names
  * made RDAO option number a suggestion
  * added "reference" field to endpoint type registry
* Lookup: mention that anchor is a legitimate lookup attribute
* Terminology and example fixes
* Layout fixes, esp. the use of non-ASCII characters in figures

changes from -22 to -23

* Explain that updates can not remove attributes
* Typo fixes

changes from -21 to -22

* Request a dedicated IPv4 address from IANA (rather than sharing with All CoAP nodes)
* Fix erroneous examples
* Editorial changes
  * Add figure numbers to examples
  * Update RD parameters table to reflect changes of earlier versions in the text
  * Typos and minor wording

changes from -20 to -21

(Processing comments during WGLC)

* Defer outdated description of using DNS-SD to find an RD to the defining document
* Describe operational conditions in automation example
* Recommend particular discovery mechanisms for some managed network scenarios

changes from -19 to -20

(Processing comments from the WG chair review)

* Define the permissible characters in endpoint and sector names
* Express requirements on NAT situations in more abstract terms
* Shifted heading levels to have the interfaces on the same level
* Group instructions for error handling into general section
* Simple Registration: process reflowed into items list
* Updated introduction to reflect state of CoRE in general, reference RFC7228
  (defining "constrained") and use "IoT" term in addition to "M2M"
* Update acknowledgements
* Assorted editorial changes
  - Unify examples style
  - Terminology: RDAO defined and not only expanded
  - Add CT to {{fig-arch}}
  - Consistency in the use of the term "Content Format"

changes from -18 to -19

* link-local addresses: allow but prescribe split-horizon fashion when used,
  disallow zone identifiers
* Remove informative references to documents not mentioned any more

changes from -17 to -18

* Rather than re-specifying link format (Modernized Link Format), describe a
  Limited Link Format that's the uncontested subset of Link Format
* Acknowledging the -17 version as part of the draft
* Move "Read endpoint links" operation to future specification like PATCH
* Demote links-json to an informative reference, and removed them from exchange
  examples
* Add note on unusability of link-local IP addresses, and describe mitigation.
* Reshuffling of sections: Move additional operations and endpoint lookup back
  from appendix, and groups into one
* Lookup interface tightened to not imply applicability for non link-format
  lookups (as those can have vastly different views on link cardinality)
* Simple registration: Change sequence of GET and POST-response, ensuring
  unsuccessful registrations are reported as such, and suggest how devices that
  would have required the inverse behavior can still cope with it.
* Abstract and introduction reworded to avoid the impression that resources are
  stored in full in the RD
* Simplify the rules governing when a registration resource can or must be
  changed.
* Drop a figure that has become useless due to the changes of and -13 and -17
* Wording consistency fixes: Use "Registrations" and "target attributes"
* Fix incorrect use of content negotiation in discovery interface description
  (Content-Format -> Accept)
* State that the base attribute value is part of endpoint lookup even when
  implicit in the registration
* Update references from RFC5988 to its update RFC8288
* Remove appendix on protocol-negotiation (which had a note to be removed
  before publication)

changes from -16 to -17

(Note that -17 is published as a direct follow-up to -16, containing a single change to be discussed at IETF103)

* Removed groups that are enumerations of registrations and have dedicated mechanism
* Add groups that are enumerations of shared resources and are a special case of endpoint registrations

changes from -15 to -16

* Recommend a common set of resources for members of a group
* Clarified use of multicast group in lighting example
* Add note on concurrent registrations from one EP being possible but not expected
* Refresh web examples appendix to reflect current use of Modernized Link Format
* Add examples of URIs where Modernized Link Format matters
* Editorial changes

changes from -14 to -15

* Rewrite of section "Security policies"
* Clarify that the "base" parameter text applies both to relative references
  both in anchor and href
* Renamed "Registree-EP" to  Registrant-EP"
* Talk of "relative references" and "URIs" rather than "relative" and
  "absolute" URIs. (The concept of "absolute URIs" of {{RFC3986}} is not needed in RD).
* Fixed examples
* Editorial changes

changes from -13 to -14

* Rename "registration context" to "registration base URI" (and "con" to
  "base") and "domain" to "sector" (where the abbreviation "d" stays for
  compatibility reasons)
* Introduced resource types core.rd-ep and core.rd-gp
* Registration management moved to appendix A, including endpoint and group lookup
* Minor editorial changes
  * PATCH/iPATCH is clearly deferred to another document
  * Recommend against query / fragment identifier in con=
  * Interface description lists are described as illustrative
  * Rewording of Simple Registration
* Simple registration carries no error information and succeeds immediately (previously, sequence was unspecified)
* Lookup: href are matched against resolved values (previously, this was unspecified)
* Lookup: lt are not exposed any more
* con/base: Paths are allowed
* Registration resource locations can not have query or fragment parts
* Default life time extended to 25 hours
* clarified registration update rules
* lt-value semantics for lookup clarified.
* added template for simple registration

changes from -12 to -13

* Added "all resource directory" nodes MC address
* Clarified observation behavior
* version identification
* example rt= and et= values
* domain from figure 2
* more explanatory text
* endpoints of a groups hosted by different RD
* resolve RFC6690-vs-8288 resolution ambiguities:
    - require registered links not to be relative when using anchor
    - return absolute URIs in resource lookup

changes from -11 to -12

* added Content Model section, including ER diagram
* removed domain lookup interface; domains are now plain attributes of groups and endpoints
* updated chapter "Finding a Resource Directory"; now distinguishes configuration-provided, network-provided and heuristic sources
* improved text on: atomicity, idempotency, lookup with multiple parameters, endpoint removal, simple registration
* updated LWM2M description
* clarified where relative references are resolved, and how context and anchor interact
* new appendix on the interaction with RFCs 6690, 5988 and 3986
* lookup interface: group and endpoint lookup return group and registration resources as link targets
* lookup interface: search parameters work the same across all entities
* removed all methods that modify links in an existing registration (POST with payload, PATCH and iPATCH)
* removed plurality definition (was only needed for link modification)
* enhanced IANA registry text
* state that lookup resources can be observable
* More examples and improved text

changes from -09 to -10

* removed "ins" and "exp" link-format extensions.
* removed all text concerning DNS-SD.
* removed inconsistency in RDAO text.
* suggestions taken over from various sources
* replaced "Function Set" with "REST API", "base URI", "base path"
* moved simple registration to registration section

changes from -08 to -09

* clarified the "example use" of the base RD resource values /rd, /rd-lookup, and /rd-group.

* changed "ins" ABNF notation.

* various editorial improvements, including in examples

* clarifications for RDAO

changes from -07 to -08

* removed link target value returned from domain and group lookup types

* Maximum length of domain parameter 63 bytes for consistency with group

* removed option for simple POST of link data, don't require a .well-known/core resource to accept POST data and handle it in a special way; we already have /rd for that

* add IPv6 ND Option for discovery of an RD

* clarify group configuration section 6.1 that endpoints must be registered before including them in a group

* removed all superfluous client-server diagrams

* simplified lighting example

* introduced Commissioning Tool



* RD-Look-up text is extended.


changes from -06 to -07

* added text in the discovery section to allow content format hints to be exposed in the discovery link attributes

* editorial updates to section 9

* update author information

* minor text corrections


Changes from -05 to -06

* added note that the PATCH section is contingent on the progress of
  the PATCH method


changes from -04 to -05

* added Update Endpoint Links using PATCH

* http access made explicit in interface specification

* Added http examples


Changes from -03 to -04:

* Added http response codes

* Clarified endpoint name usage

* Add application/link-format+cbor content-format

Changes from -02 to -03:

* Added an example for lighting and DNS integration

* Added an example for RD use in OMA LWM2M

* Added Read Links operation for link inspection by endpoints

* Expanded DNS-SD section

* Added draft authors Peter van der Stok and Michael Koster

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

* Fixed tickets 383 and 372

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


# Groups Registration and Lookup {#groups}

The RD-Groups usage pattern allows announcing application groups inside a Resource Directory.

Groups are represented by endpoint registrations.
Their base address is a multicast address,
and they SHOULD be entered with the endpoint type `core.rd-group`.
The endpoint name can also be referred to as a group name in this context.

The registration is inserted into the RD by a Commissioning Tool,
which might also be known as a group manager here.
It performs third party registration and registration updates.

The links it registers SHOULD be available on all members that join the group.
Depending on the application, members that lack some resource
MAY be permissible if requests to them fail gracefully.


The following example shows a CT registering a group with the name “lights” which provides two resources.
The directory resource path /rd
is an example RD location discovered in a request similar to {{example-discovery}}.

~~~~
Req: POST coap://rd.example.com/rd?ep=lights&et=core.rd-group
                                  &base=coap://[ff35:30:2001:db8::1]
Content-Format: 40
Payload:
</light>;rt="light";if="core.a",
</color-temperature>;if="core.p";u="K"

Res: 2.01 Created
Location-Path: /rd/12
~~~~
{: #example-group-registration title="Example registration of a group"}

In this example, the group manager can easily permit devices that have no
writable color-temperature to join, as they would still respond to brightness
changing commands. Had the group instead contained a single resource that sets
brightness and color temperature atomically, endpoints would need to support
both properties.

The resources of a group can be looked up like any other resource,
and the group registrations (along with any additional registration parameters)
can be looked up using the endpoint lookup interface.


The following example shows a client performing and endpoint lookup for all groups.

~~~~
Req: GET /rd-lookup/ep?et=core.rd-group

Res: 2.05 Content
Payload:
</rd/501>;ep="GRP_R2-4-015";et="core.rd-group";
                                   base="coap://[ff05::1]",
</rd/12>;ep=lights&et=core.rd-group;
         base="coap://[ff35:30:2001:db8::1]";rt="core.rd-ep"
~~~~
{: #example-group-lookup title="Example lookup of groups"}

The following example shows a client performing a lookup of all resources of all endpoints (groups) with et=core.rd-group.

~~~~
Req: GET /rd-lookup/res?et=core.rd-group

<coap://[ff35:30:2001:db8::1]/light>;rt="light";if="core.a";
     et="core.rd-group";anchor="coap://[ff35:30:2001:db8::1]",
<coap://[ff35:30:2001:db8::1]/color-temperature>;if="core.p";u="K";
     et="core.rd-group";
     anchor="coap://[ff35:30:2001:db8::1]"
~~~~
{: #example-group-lookup-res title="Example lookup of resources inside groups"}

# Web links and the Resource Directory {#weblink}

Understanding the semantics of a link-format document and its URI references is
a journey through different documents ({{RFC3986}} defining URIs, {{RFC6690}}
defining link-format documents based on {{RFC8288}} which defines Link header fields,
and {{RFC7252}} providing the transport). This appendix summarizes
the mechanisms and semantics at play from an entry in `.well-known/core` to a
resource lookup.

This text is primarily aimed at people entering the field of Constrained
Restful Environments from applications that previously did not use web
mechanisms.

The explanation of the steps makes some shortcuts in the more confusing details of {{RFC6690}},
which are justified as all examples being in Limited Link Format.

## A simple example

Let's start this example with a very simple host, `2001:db8:f0::1`. A client
that follows classical CoAP Discovery ({{RFC7252}} Section 7), sends the
following multicast request to learn about neighbours supporting resources with
resource-type "temperature".

The client sends a link-local multicast:

    GET coap://[ff02::fd]:5683/.well-known/core?rt=temperature

    RES 2.05 Content
    </temp>;rt=temperature;ct=0
{: #example-weblink-wkc title="Example of direct resource discovery"}

where the response is sent by the server, `[2001:db8:f0::1]:5683`.

While the client -- on the practical or implementation side -- can just go
ahead and create a new request to `[2001:db8:f0::1]:5683` with Uri-Path:
`temp`, the full resolution steps for insertion into and retrieval from the RD without any shortcuts are:

### Resolving the URIs {#resolveURI}

The client parses the single returned record. The link's target (sometimes
called "href") is "`/temp`", which is a relative URI that needs resolving.
The base
URI <coap://[ff02::fd]:5683/.well-known/core> is used to resolve the
reference /temp against.

The Base URI of the requested resource can be composed from the options of the CoAP GET request by following the steps of
{{RFC7252}} section 6.5 (with an addition at the end of 8.2) into
"`coap://[2001:db8:f0::1]/.well-known/core`".

Because "`/temp`" starts with a single slash,
the record's target is resolved by replacing the path "`/.well-known/core`"
from the Base URI (section 5.2 {{RFC3986}}) with the relative target URI "`/temp`" into
"`coap://[2001:db8:f0::1]/temp`".

### Interpreting attributes and relations

Some more information but the record's target can be obtained from the payload:
the resource type of the target is "temperature", and its content format is
text/plain (ct=0).

A relation in a web link is a three-part statement that specifies a named relation between the so-called "context resource"
and the target resource, like "*This page* has *its table
of contents* at */toc.html*". In link format documents,
there is an implicit "host relation" specified with default parameter: rel="hosts".

In our example, the context resource of the link is the URI specified in the GET request "coap:://[2001:db8:f0::1]/.well-known/core". A full English expression of the "host relation" is:

'`coap://[2001:db8:f0::1]/.well-known/core` is hosting the resource
`coap://[2001:db8:f0::1]/temp`, which is of the resource type "temperature" and
can be accessed using the text/plain content format.'

## A slightly more complex example

Omitting the `rt=temperature` filter, the discovery query would
have given some more records in the payload:

    GET coap://[ff02::fd]:5683/.well-known/core

    RES 2.05 Content
    </temp>;rt=temperature;ct=0,
    </light>;rt=light-lux;ct=0,
    </t>;anchor="/sensors/temp";rel=alternate,
    <http://www.example.com/sensors/t123>;anchor="/temp";
        rel="describedby"
{: #example-weblink-wkc-extended title="Extended example of direct resource discovery"}

Parsing the third record, the client encounters the "anchor" parameter. It is
a URI relative to the Base URI of the request and is thus resolved to
"`coap://[2001:db8:f0::1]/sensors/temp`".
That is the context resource of the link, so the "rel" statement is not about
the target and the Base URI any more, but about the target and the resolved
URI. Thus, the third record could be read as
"`coap://[2001:db8:f0::1]/sensors/temp` has an alternate representation at
`coap://[2001:db8:f0::1]/t`".

Following the same resolution steps, the fourth record can be read as "`coap://[2001:db8:f0::1]/sensors/temp` is
described by `http://www.example.com/sensors/t123`".

## Enter the Resource Directory

The resource directory tries to carry the semantics obtainable by classical
CoAP discovery over to the resource lookup interface as faithfully as possible.

For the following queries, we will assume that the simple host has used Simple
Registration to register at the resource directory that was announced to it,
sending this request from its UDP port `[2001:db8:f0::1]:6553`:

    POST coap://[2001:db8:f01::ff]/.well-known/core?ep=simple-host1
{: #example-weblink-simple title="Example request starting a simple registration"}

The resource directory would have accepted the registration, and queried the
simple host's `.well-known/core` by itself. As a result, the host is registered
as an endpoint in the RD with the name "simple-host1". The registration is
active for 90000 seconds, and the endpoint registration Base URI is
"`coap://[2001:db8:f0::1]`" following the resolution steps described in {{resolveURI}}. It should be remarked that the Base URI constructed that way always yields a URI of the form: scheme://authority without path suffix.

If the client now queries the RD as it would previously have issued a multicast
request, it would go through the RD discovery steps by fetching
`coap://[2001:db8:f0::ff]/.well-known/core?rt=core.rd-lookup-res`, obtain
`coap://[2001:db8:f0::ff]/rd-lookup/res` as the resource lookup endpoint, and
issue a request to `coap://[2001:db8:f0::ff]/rd-lookup/res?rt=temperature` to
receive the following data:

    <coap://[2001:db8:f0::1]/temp>;rt=temperature;ct=0;
        anchor="coap://[2001:db8:f0::1]"
{: #example-weblink-lookup-result title="Example payload of a response to a resource lookup"}

This is not *literally* the same response that it would have received from a
multicast request, but it contains the equivalent statement:

'`coap://[2001:db8:f0::1]` is hosting the resource
`coap://[2001:db8:f0::1]/temp`, which is of the resource type "temperature" and
can be accessed using the text/plain content format.'

(The difference is whether `/` or `/.well-known/core` hosts the resources,
which does not matter in this application; if it did, the endpoint would have been more explicit. Actually, /.well-known/core does NOT host the resource but stores a URI reference to the resource.)

To complete the examples, the client could also query all resources hosted at
the endpoint with the known endpoint name "simple-host1". A request to
`coap://[2001:db8:f0::ff]/rd-lookup/res?ep=simple-host1` would return

    <coap://[2001:db8:f0::1]/temp>;rt=temperature;ct=0;
        anchor="coap://[2001:db8:f0::1]",
    <coap://[2001:db8:f0::1]/light>;rt=light-lux;ct=0;
        anchor="coap://[2001:db8:f0::1]",
    <coap://[2001:db8:f0::1]/t>;
        anchor="coap://[2001:db8:f0::1]/sensors/temp";rel=alternate,
    <http://www.example.com/sensors/t123>;
        anchor="coap://[2001:db8:f0::1]/sensors/temp";rel="describedby"
{: #example-weblink-lookup-result-extended title="Extended example payload of a response to a resource lookup"}

All the target and anchor references are already in absolute form there, which
don't need to be resolved any further.

Had the simple host done an equivalent full registration with a base= parameter (e.g.
`?ep=simple-host1&base=coap+tcp://simple-host1.example.com`), that context would
have been used to resolve the relative anchor values instead, giving

    <coap+tcp://simple-host1.example.com/temp>;rt=temperature;ct=0;
        anchor="coap+tcp://simple-host1.example.com"
{: #example-weblink-lookup-result-base title="Example payload of a response to a resource lookup with a dedicated base URI"}

and analogous records.

## A note on differences between link-format and Link header fields {#resolution-rules}

While link-format and Link header fields look very similar and are based on the same
model of typed links, there are some differences between {{RFC6690}} and
{{RFC8288}}, which are dealt with differently:

* "Resolving the target against the anchor":
  {{RFC6690}} Section 2.1 states that the anchor of a link is used as the Base URI
  against which the term inside the angle brackets (the target) is resolved,
  falling back to the resource's URI with paths stripped off (its "Origin").
  In contrast to that,
  {{RFC8288}} Section B.2 describes that the anchor is immaterial to the
  resolution of the target reference.

  RFC6690, in the same section, also states that absent anchors set the context of
  the link to the target's URI with its path stripped off, while according to
  {{RFC8288}} Section 3.2, the context is the resource's base URI.

  The rules introduced in {{limitedlinkformat}} ensure
  that an RD does not need to deal with those differences
  when processing input data.
  Lookup results are required to be absolute references for the same reason.

* There is no percent encoding in link-format documents.

  A link-format document is a UTF-8 encoded string of Unicode characters and
  does not have percent encoding, while Link header fields are practically ASCII
  strings that use percent encoding for non-ASCII characters, stating the
  encoding explicitly when required.

  For example, while a Link header field in a page about a Swedish city might read

  ~~~~
  Link: </temperature/Malm%C3%B6>;rel="live-environment-data"
  ~~~~

  a link-format document from the same source might describe the link as

  ~~~~
  </temperature/Malmö>;rel="live-environment-data"
  ~~~~

  Parsers and producers of link-format and header fields need to be aware of this
  difference.

# Limited Link Format {#limitedlinkformat}

The CoRE Link Format as described in {{RFC6690}}
has been interpreted differently by implementers,
and a strict implementation
rules out some use cases of a Resource Directory
(e.g. base values with path components).

This appendix describes
a subset of link format documents called Limited Link Format.
The rules herein are not very limiting in practice --
all examples in RFC6690, and all deployments the authors are aware of already stick to them --
but ease the implementation of resource directory servers.

It is applicable to representations in the application/link-format media type,
and any other media types that inherit {{RFC6690}} Section 2.1.

A link format representation is in Limited Link format if,
for each link in it,
the following applies:

* All URI references either follow the URI or the path-absolute ABNF rule of
  RFC3986 (i.e. target and anchor each either start with a scheme or with a
  single slash),

* if the anchor reference starts with a scheme, the target reference starts
  with a scheme as well (i.e. relative references in target cannot be used when
  the anchor is a full URI), and

* the application does not care whether links without an explicitly given
  anchor have the origin's "/" or "/.well-known/core" resource as their link
  context.


<!--  LocalWords:  lookups multicast lookup RESTful CoRE LoWPAN CoAP
 -->
<!--  LocalWords:  microcontrollers URI DNS EP IP EPs discoverable
 -->
<!--  LocalWords:  Metadata metadata lossless anycast ABRO RDNSS ICMP
 -->
<!--  LocalWords:  DHCPv RD's DHCP RDs unicast JSON CBOR wildcard TLS
 -->
<!--  LocalWords:  subdomain substring prepending subtype DTLS UDP
 -->
<!--  LocalWords:  routability NTP TCP WiFi GPRS FQDN SLAAC IPv SDOs
 -->
<!--  LocalWords:  OMA LWM IETF OMNA API SMS MSISDN
 -->
