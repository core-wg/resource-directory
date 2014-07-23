<?xml version="1.0" encoding="US-ASCII"?>
<!-- This template is for creating an Internet Draft using xml2rfc,
     which is available here: http://xml.resource.org. -->
<!DOCTYPE rfc SYSTEM "rfc2629.dtd" [
<!ENTITY RFC0792 SYSTEM "http://xml2rfc.ietf.org/public/rfc/bibxml/reference.RFC.0792.xml">
<!ENTITY RFC2045 SYSTEM "http://xml2rfc.ietf.org/public/rfc/bibxml/reference.RFC.2045.xml">
<!ENTITY RFC2046 SYSTEM "http://xml2rfc.ietf.org/public/rfc/bibxml/reference.RFC.2046.xml">
<!ENTITY RFC2119 SYSTEM "http://xml2rfc.ietf.org/public/rfc/bibxml/reference.RFC.2119.xml">
<!ENTITY RFC2616 SYSTEM "http://xml2rfc.ietf.org/public/rfc/bibxml/reference.RFC.2616.xml">
<!ENTITY RFC3986 SYSTEM "http://xml2rfc.ietf.org/public/rfc/bibxml/reference.RFC.3986.xml">
<!ENTITY RFC4288 SYSTEM "http://xml2rfc.ietf.org/public/rfc/bibxml/reference.RFC.4288.xml">
<!ENTITY RFC4346 SYSTEM "http://xml2rfc.ietf.org/public/rfc/bibxml/reference.RFC.4346.xml">
<!ENTITY RFC4347 SYSTEM "http://xml2rfc.ietf.org/public/rfc/bibxml/reference.RFC.4347.xml">
<!ENTITY RFC4944 SYSTEM "http://xml2rfc.ietf.org/public/rfc/bibxml/reference.RFC.4944.xml">
<!ENTITY RFC5234 SYSTEM "http://xml2rfc.ietf.org/public/rfc/bibxml/reference.RFC.5234.xml">
<!ENTITY RFC5226 SYSTEM "http://xml2rfc.ietf.org/public/rfc/bibxml/reference.RFC.5226.xml">
<!ENTITY RFC5785 SYSTEM "http://xml2rfc.ietf.org/public/rfc/bibxml/reference.RFC.5785.xml">
<!ENTITY RFC5988 SYSTEM "http://xml2rfc.ietf.org/public/rfc/bibxml/reference.RFC.5988.xml">
<!ENTITY RFC6570 SYSTEM "http://xml2rfc.ietf.org/public/rfc/bibxml/reference.RFC.6570.xml">
<!ENTITY RFC6775 SYSTEM "http://xml2rfc.ietf.org/public/rfc/bibxml/reference.RFC.6775.xml">
<!ENTITY RFC6690 SYSTEM "http://xml2rfc.ietf.org/public/rfc/bibxml/reference.RFC.6690.xml">
<!ENTITY I-D.ietf-core-coap SYSTEM "http://xml2rfc.ietf.org/public/rfc/bibxml3/reference.I-D.ietf-core-coap.xml">
<!ENTITY I-D.vanderstok-core-bc SYSTEM "http://xml2rfc.ietf.org/public/rfc/bibxml3/reference.I-D.vanderstok-core-bc.xml">
<!ENTITY I-D.brandt-coap-subnet-discovery SYSTEM "http://xml2rfc.ietf.org/public/rfc/bibxml3/reference.I-D.brandt-coap-subnet-discovery.xml">
<!ENTITY I-D.ietf-core-groupcomm SYSTEM "http://xml2rfc.ietf.org/public/rfc/bibxml3/reference.I-D.ietf-core-groupcomm.xml">
<!ENTITY I-D.ietf-core-links-json SYSTEM "http://xml2rfc.ietf.org/public/rfc/bibxml3/reference.I-D.ietf-core-links-json.xml">

]>
<?xml-stylesheet type='text/xsl' href='rfc2629.xslt' ?>
<!-- used by XSLT processors -->
<!-- For a complete list and description of processing instructions (PIs),
     please see http://xml.resource.org/authoring/README.html. -->
<!-- Below are generally applicable Processing Instructions (PIs) that most I-Ds might want to use.
     (Here they are set differently than their defaults in xml2rfc v1.32) -->
<?rfc strict="yes" ?>
<!-- give errors regarding ID-nits and DTD validation -->
<!-- control the table of contents (ToC) -->
<?rfc toc="yes"?>
<!-- generate a ToC -->
<?rfc tocdepth="3"?>
<!-- the number of levels of subsections in ToC. default: 3 -->
<!-- control references -->
<?rfc symrefs="yes"?>
<!-- use symbolic references tags, i.e, [RFC2119] instead of [1] -->
<?rfc sortrefs="yes" ?>
<!-- sort the reference entries alphabetically -->
<!-- control vertical white space
     (using these PIs as follows is recommended by the RFC Editor) -->
<?rfc compact="yes" ?>
<!-- do not start each main section on a new page -->
<?rfc subcompact="no" ?>
<!-- keep one blank line between list items -->
<!-- end of list of popular I-D processing instructions -->
<rfc category="std" ipr="trust200902" docName="draft-ietf-core-resource-directory-02-pre">

<?xml-stylesheet type='text/xsl' href='rfc2629.xslt' ?>

<?rfc toc="yes" ?>
<?rfc symrefs="yes" ?>
<?rfc sortrefs="yes"?>
<?rfc iprnotified="no" ?>
<?rfc strict="yes" ?>

    <front>
        <title>CoRE Resource Directory</title>

        <author initials="Z" surname="Shelby" fullname="Zach Shelby">
          <organization>
             ARM
          </organization>
          <address>
            <postal>
             <street>150 Rose Orchard</street>
             <city>San Jose</city>
             <code>95134</code>
             <country>FINLAND</country>
            </postal>
            <phone>+1-408-203-9434</phone>
            <email>zach.shelby@arm.com</email>
          </address>
        </author>

    <author initials="C." surname="Bormann" fullname="Carsten Bormann">
      <organization>Universitaet Bremen TZI</organization>
      <address>
        <postal>
          <street>Postfach 330440</street>
          <city>Bremen</city>
          <code>D-28359</code>
          <country>Germany</country>
        </postal>
        <phone>+49-421-218-63921</phone>
        <email>cabo@tzi.org</email>
      </address>
    </author>

  <date year="2014"/>

  <area>Internet</area>

  <workgroup>CoRE</workgroup>
  <keyword>CoRE, Web Linking, Resource Discovery, Resource Directory</keyword>

    <abstract>
    <t>
	In many M2M applications, direct discovery of resources is not practical due to sleeping nodes, disperse networks, or networks where multicast traffic is inefficient. These problems can be solved by employing an entity called a Resource Directory (RD), which hosts descriptions of resources held on other servers, allowing lookups to be performed for those resources. This document specifies the web interfaces that a Resource Directory supports in order for web servers to discover the RD and to register, maintain, lookup and remove resources descriptions. Furthermore, new link attributes useful in conjunction with an RD are defined. 
	</t> 
    
    </abstract>
    </front>

    <middle>


  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <section anchor='introduction' title="Introduction">

  <t>
  The Constrained RESTful Environments (CoRE) work aims at realizing the REST architecture in a suitable form for the most constrained nodes (e.g. 8-bit microcontrollers with limited RAM and ROM) and networks (e.g. 6LoWPAN). CoRE is aimed at machine-to-machine (M2M) applications such as smart energy and building automation.
  </t>
  <t>
  The discovery of resources offered by a constrained server is very important in machine-to-machine applications where there are no humans in the loop and static interfaces result in fragility. The discovery of resources provided by an HTTP Web Server is typically called Web Linking <xref target="RFC5988"/>. The use of Web Linking for the description and discovery of resources hosted by constrained web servers is specified by the CoRE Link Format <xref target="RFC6690"/>. This specification however only describes how to discover resources from the web server that hosts them by requesting /.well-known/core. In many M2M scenarios, direct discovery of resources is not practical due to sleeping nodes, disperse networks, or networks where multicast traffic is inefficient. These problems can be solved by employing an entity called a Resource Directory (RD), which hosts descriptions of resources held on other servers, allowing lookups to be performed for those resources.
  </t>
  <t>
  This document specifies the web interfaces that a Resource Directory supports in order for web servers to discover the RD and to register, maintain, lookup and remove resource descriptions. Furthermore, new link attributes useful in conjunction with a Resource Directory are defined. Although the examples in this document show the use of these interfaces with CoAP <xref target="I-D.ietf-core-coap"/>, they can be applied in an equivalent manner to HTTP <xref target="RFC2616"/>. 
  </t>

  </section>
  
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** --> 
  <section anchor="terminology" title="Terminology">
	<t>The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
	"SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this
	document are to be interpreted as described in <xref target="RFC2119"/>. The
	term "byte" is used in its now customary sense as a synonym for "octet".</t>

	<t>This specification requires readers to be familiar with all the terms
	and concepts that are discussed in <xref target="RFC5988"/> and <xref target="RFC6690"/>. Readers should also be familiar with the terms and concepts discussed in <xref target="I-D.ietf-core-coap"/>. The URI Template format is used to describe the REST interfaces defined in this specification <xref target="RFC6570"/>. This specification makes use of the following additional terminology:
	<list style="hanging">
	  <t hangText="Resource Directory"><vspace />
		An web entity that stores information about web resources and  implements the REST interfaces defined in this specification for registration and lookup of those resources.</t>
	  <t hangText="Domain"><vspace />
		In the context of a Resource Directory, a domain is a logical grouping of endpoints. This specification assumes that the list of Domains supported by an RD is pre-configured by that RD.</t>
	  <t hangText="Group"><vspace />
		In the context of a Resource Directory, a group is a logical grouping of endpoints for the purpose of group communications. All groups within a domain are unique. </t>
	  <t hangText="Endpoint"><vspace />
		An endpoint (EP) is a term used to describe a web server or client in  <xref target="I-D.ietf-core-coap"/>. In the context of this specification an endpoint is used to describe a web server that registers resources to the Resource Directory. An endpoint is identified by its endpoint name, which is included during registration, and is unique within the associated domain of the registration.</t>
	</list>
	</t>

  </section>
  

  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <section anchor='arch' title="Architecture and Use Cases">

	<t>
	The resource directory architecture is shown in <xref target="fig-arch"/>. A Resource Directory (RD) is used as a repository for Web Links <xref target="RFC5988"/> about resources hosted on other web servers, which are called endpoints (EP). An endpoint is a web server associated with an IP address and port, thus a physical node may host one or more endpoints. The RD implements a set of REST interfaces for endpoints to register and maintain sets of Web Links (called resource directory entries), for the RD to validate entries, and for clients to lookup resources from the RD. Endpoints themselves can also act as clients. An RD can be logically segmented by the use of Domains. The domain an endpoint is associated with can be defined by the RD or configured by an outside entity.
	</t>
	<t>
	Endpoints are assumed to proactively register and maintain resource directory entries on the RD, which are soft state and need to be periodically refreshed. An endpoint is provided with interfaces to register, update and remove a resource directory entry. Furthermore, a mechanism to discover a RD using the CoRE Link Format is defined. It is also possible for an RD to proactively discover Web Links from endpoints and add them as resource directory entries, or to validate existing resource directory entries. A lookup interface for discovering any of the Web Links held in the RD is provided using the CoRE Link Format.  
	</t>


		<figure anchor="fig-arch" title="The resource directory architecture.">
          <artwork align="left"><![CDATA[
          
             Registration         Lookup
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
  
            ]]></artwork>
        </figure>


	  <section anchor='cellular' title="Use Case: Cellular M2M">
  	  <t>
	  Over the last few years, mobile operators around the world have focused on development of M2M solutions in order to expand the business to the new type of users, i.e. machines. The machines are connected directly to a mobile network using appropriate embedded air interface (GSM/GPRS, WCDMA, LTE) or via a gateway providing short and wide range wireless interfaces. From the system design point of view, the ambition is to design horizontal solutions that can enable utilization of machines in different applications depending on their current availability and capabilities as well as application requirements, thus avoiding silo like solutions. One of the crucial enablers of such design is the ability to discover resources (machines - endpoints) capable of providing required information at a given time or acting on instructions from the end users. 	
	  </t>
	  
	  <t>
	  In a typical scenario, during a boot-up procedure (and periodically afterwards), the machines (endpoints) register with a Resource Directory (for example EPs installed on vehicles enabling tracking of their position for the fleet management purposes and monitoring environment parameters) hosted by the mobile operator or somewhere else in the network, periodically a description of its own capabilities. Due to the usual network configuration of mobile networks, the EPs attached to the mobile network do not have routable addresses. Therefore, a remote server is usually used to provide proxy access to the EPs. The address of each (proxy) endpoint on this server is included in the resource description stored in the RD. The users, for example mobile applications for environment monitoring, contact the RD, look-up the endpoints capable of providing information about the environment using appropriate set of link parameters, obtain information on how to contact them (URLs of the proxy server) and then initiate interaction to obtain information that is finally processed, displayed on the screen and usually stored in a database. Similarly, fleet management systems provide the appropriate link parameters to the RD to look-up for EPs deployed on the vehicles the application is responsible for. 
	  </t>
	  </section>

	  <section anchor='automation' title="Use Case: Home and Building Automation">
	  <t>
	  Home and commercial building automation systems can benefit from the use of M2M web services. The use of CoRE in home automation across multiple subnets is described in <xref target="I-D.brandt-coap-subnet-discovery"/> and in commercial building automation in <xref target="I-D.vanderstok-core-bc"/>. The discovery requirements of these applications are demanding. Home automation usually relies on run-time discovery to commission the system, whereas in building automation a combination of professional commissioning and run-time discovery is used. Both home and building automation involve peer-to-peer interactions between endpoints, and involve battery-powered sleeping devices. 
	  </t>
	  <t>
	  The exporting of resource information to other discovery systems is also important in these automation applications. In home automation there is a need to interact with other consumer electronics, which may already support DNS-SD, and in building automation larger resource directories or DNS-SD covering multiple buildings.
	  </t>
	  
	  </section>

	  <section anchor='usecase-catalogues' title="Use Case: Semantics Catalogues">
	  
	  <t> 
	  Resources may be shared through data brokers that have no knowledge 		beforehand of who is going to consume the data. Resource Directory can be used to hold links about resources and services hosted anywhere to make them discoverable by a general class of applications. 
</t>

<t>
For example, environmental and weather sensors that generate data for public consumption may provide the data to an intermediary server, or broker. Sensor data are published to the intermediary upon changes or at regular intervals. Descriptions of the sensors that resolve to links to sensor data may be published to a Resource Directory. Applications wishing to consume the data can use the Resource Directory lookup function set to discover and resolve links to the desired resources and endpoints. The Resource Directory service need not be coupled with the data intermediary service. Mapping of Resource Directories to data intermediaries may be many-to-many.
</t>

<t>
Metadata in link-format or link-format+json representations are supplied by Resource Directories, which may be internally stored as semantic triples, or relation/attribute pairs providing metadata about resource links. External catalogs that are represented in other formats may be converted to link-format or link-format+json for storage and access by Resource Directories. Since it is common practice for these to be URN encoded, simple and lossless structural transforms will generally be sufficient to store external metadata in Resource Directories.
</t>

<t>
The additional features of Resource Directory allow domains to be defined to enable access to a particular set of resources from particular applications. this provides isolation and protection of sensitive data when needed. Resource groups may defined to allow batched reads from multiple resources.
</t>

	  </section>

  </section>


  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <section anchor='simple' title="Simple Directory Discovery">
  
  <t> Not all endpoints hosting resources are expected to know how to implement
  the Resource Directory Function Set and thus explicitly register with a
  Resource Directory (or other such directory server). Instead, simple endpoints
  can implement the generic Simple Directory Discovery approach described in
  this section. An RD implementing this specification MUST implement Simple
  Directory Discovery. However, there may be security reasons why this form of 
  directory discovery would be disabled. </t>
  
  <t>
  This approach requires that the endpoint makes available the hosted resources
that it wants to be discovered, as links on its /.well-known/core interface as 
  specified in <xref target="RFC6690"/>.
  </t>
  <t>
  The endpoint then finds one or more IP addresses of the directory server it 
  wants to know about its resources as described in 
  <xref target="simple_finding"/>.    
  </t>
  <t>
  An endpoint that wants to make itself discoverable occasionally
  sends a POST request to the /.well-known/core URI of any candidate directory
  server that it finds. The body of the POST request is either</t>

<t><list style='symbols'>
  <t>empty, in which case the directory server is encouraged by this POST 
  request to perform GET requests at the requesting server's default discovery
  URI.</t>
</list></t>

<t>or</t>

<t><list style='symbols'>
  <t>a non-empty link-format document, which indicates the specific services that the
requesting server wants to make known to the directory server.</t>
</list></t>

<t>The directory server integrates the information it received this way into its
resource directory.  It MAY make the information available to further
directories, if it can ensure that a loop does not form.  The protocol used
between directories to ensure loop-free operation is outside the scope of
this document.</t>
  
	<t>
		The following example shows an endpoint using simple resource discovery,
		by simply sending a POST with its links in the body to a directory. 
		</t>

		<figure>
          <artwork align="left"><![CDATA[


     EP                                               RD
     |                                                 |
     | -- POST /.well-known/core "</sen/temp>..." ---> |
     |                                                 |
     |                                                 |
     | <---- 2.01 Created   -------------------------  |
     |                                                 |


            ]]></artwork>
        </figure>
  
  
<section anchor="simple_finding" title="Finding a Directory Server">

<t>Endpoints that want to contact a directory server can obtain candidate IP
addresses for such servers in a number of ways.</t>

<t>In a 6LoWPAN, good candidates can be taken from:</t>

<t><list style='symbols'>
  <t>specific static configuration (e.g., anycast addresses), if any,</t>
  <t>the ABRO option of 6LoWPAN-ND <xref target="RFC6775"/>,</t>
  <t>other ND options that happen to point to servers (such as RDNSS),</t>
  <t>DHCPv6 options that might be defined later.</t>
</list></t>

<t>In networks with more inexpensive use of multicast, the candidate IP 
address may be a well-known multicast address, i.e. directory servers are 
found by simply sending POST requests to that well-known multicast address
 (details TBD).</t>

<t>As some of these sources are just (more or less educated) guesses,
endpoints MUST make use of any error messages to very strictly
rate-limit requests to candidate IP addresses that don't work out.
E.g., an ICMP Destination Unreachable message (and, in particular, the
port unreachable code for this message) may indicate the lack of a CoAP
server on the candidate host, or a CoAP error response code such as
4.05 "Method Not Allowed" may indicate unwillingness of a CoAP server
to act as a directory server.</t>

</section>
  
  
  </section>


  
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <section anchor='rd' title="Resource Directory Function Set">

	<t>
	This section defines the REST interfaces between an RD and endpoint servers, which is called the Resource Directory Function Set. Although the examples throughout this section assume use of CoAP    
	<xref target="I-D.ietf-core-coap"/>, these REST interfaces can also be realized using HTTP <xref target="RFC2616"/>. An RD implementing this specification MUST support the discovery, registration, update, and removal interfaces defined in this section. 
	</t>
	
	<t>
	Resource directory entries are designed to be easily exported to other discovery mechanisms such as DNS-SD. For that reason, parameters that would meaningfully be mapped to DNS are limited to a maximum length of 63 bytes.  
	</t>

	  <section anchor='discovery' title="Discovery">
	
		<t>
		Before an endpoint can make use of an RD, it must first know the RD's IP address, port and the path of its RD Function Set. There can be several mechanisms for discovering the RD including assuming a default location (e.g. on an Edge Router in a LoWPAN), by assigning an anycast address to the RD, using DHCP, or by discovering the RD using the CoRE Link Format (also see <xref target="simple_finding"/>). This section defines discovery of the RD using the well-known interface of the CoRE Link Format <xref target="RFC6690"/> as the required mechanism. It is however expected that RDs will also be discoverable via other methods depending on the deployment. 
		</t>
		
		<t>
		Discovery is performed by sending either a multicast or unicast GET request to /.well-known/core and including a Resource Type (rt) parameter <xref target="RFC6690"/> with the value "core.rd" in the query string. Likewise, a Resource Type parameter value of "core.rd-lookup" is used to discover the RD Lookup Function Set. Upon success, the response will contain a payload with a link format entry for each RD discovered, with the URL indicating the root resource of the RD. When performing multicast discovery, the multicast IP address used will depend on the scope required and the multicast capabilities of the network.
		</t>
		
		<t>
		An RD implementation of this specification MUST support query filtering for the rt parameter as defined in <xref target="RFC6690"/>.
		</t>

        <t>The discovery request interface is specified as follows: 
        <list style="hanging">
          <t hangText="Interaction:">EP -> RD</t>
          <t hangText="Method:">GET</t>	
          <t hangText="URI Template:">/.well-known/core{?rt}</t>
          <t hangText="URI Template Variables:"> 
          	<list style="hanging">
          		<t hangText="rt := ">Resource Type (optional). MAY contain the value "core.rd", "core.rd-lookup", "core.rd-group" or "core.rd*"</t>
          	</list>
          </t>
          <t hangText="Content-Type:">application/link-format (if any)</t>
        </list>
       	</t>
          
		<t>The following response codes are defined for this interface: 
        <list style="hanging">         
          <t hangText="Success:"> 2.05 "Content" with an application/link-format payload containing a matching entry for the RD resource.</t>
          <t hangText="Failure:"> 4.04 "Not Found" is returned in case no matching entry is found for a unicast request.</t>
          <t hangText="Failure:"> 4.00 "Bad Request" is returned in case of a malformed request for a unicast request.</t>
        <t hangText="Failure:"> No error response to a multicast request.</t>
        </list>
       	</t>

		<t>
		The following example shows an endpoint discovering an RD using this interface, thus learning that the base RD resource is at /rd. Note that it is up to the RD to choose its base RD resource, although it is recommended to use the base paths specified here where possible. 
		</t>

		<figure>
          <artwork align="left"><![CDATA[


     EP                                               RD
     |                                                 |
     | ----- GET /.well-known/core?rt=core.rd* ------> |
     |                                                 |
     |                                                 |
     | <---- 2.05 Content "</rd>; rt="core.rd" ------  |
     |                                                 |


            ]]></artwork>
        </figure>
		
		
		
		<figure>
          <artwork align="left"><![CDATA[
Req: GET coap://[ff02::1]/.well-known/core?rt=core.rd*
		
Res: 2.05 Content
</rd>;rt="core.rd",
</rd-lookup>;rt="core.rd-lookup",
</rd-group>;rt="core.rd-group"
            ]]></artwork>
        </figure>
		
	
	  </section>
	  
	
	  <section anchor='registration' title="Registration">

		<t>
		After discovering the location of an RD Function Set, an endpoint MAY register its resources using the registration interface. This interface accepts a POST from an endpoint containing the list of resources to be added to the directory as the message payload in the CoRE Link Format <xref target="RFC6690"/> or JSON Link Format <xref target="I-D.ietf-core-links-json"/> along with query string parameters indicating the name of the endpoint, its domain and the lifetime of the registration. All parameters except the endpoint name are optional. It is expected that other specifications MAY define further parameters (it is to be determined if a registry of parameters is needed for this purpose). The RD then creates a new resource or updates an existing resource in the RD and returns its location. An endpoint MUST use that location when refreshing registrations using this interface. Endpoint resources in the RD are kept active for the period indicated by the lifetime parameter. The endpoint is responsible for refreshing the entry within this period using either the registration or update interface. The registration interface MUST be implemented to be idempotent, so that registering twice with the same endpoint parameter does not create multiple RD entries.  
		</t>

        <t>The registration request interface is specified as follows: 
        <list style="hanging">
          <t hangText="Interaction:">EP -> RD</t>
          <t hangText="Method:">POST</t>	
          <t hangText="URI Template:">/{+rd}{?ep,d,et,lt,con}</t>
          <t hangText="URI Template Variables:"> 
          	<list>
 				<t hangText="rd := ">RD Function Set path (mandatory). This is the path of the RD Function Set. An RD SHOULD use the value "rd" for this variable whenever possible.</t>   
 				<t hangText="ep := ">Endpoint (mandatory). The endpoint identifier or name of the registering node, unique within that domain. The maximum length of this parameter is 63 bytes. </t>          	
 				<t hangText="d := ">Domain (optional). The domain to which this endpoint belongs. The maximum length of this parameter is 63 bytes. Optional. When this parameter is elided, the RD MAY associate the endpoint with a configured default domain.</t>
 				<t hangText="et := ">Endpoint Type (optional). The semantic type of the endpoint. The maximum length of this parameter is 63 bytes. Optional.</t>
          		<t hangText="lt := ">Lifetime (optional). Lifetime of the registration in seconds. Range of 60-4294967295. If no lifetime is included, a default value of 86400 (24 hours) SHOULD be assumed.</t>
 				<t hangText="con := ">Context (optional). This parameter sets the scheme, address and port at which this server is available in the form scheme://host:port. Optional. In the absence of this parameter the scheme of the protocol, source IP address and source port of the register request are assumed. </t>
          	</list>
          </t>
          <t hangText="Content-Type:">application/link-format</t>
          <t hangText="Content-Type:">application/link-format+json</t>
        </list>
       	</t>

        <t>The following response codes are defined for this interface: 
        <list style="hanging">
          <t hangText="Success:"> 2.01 "Created". The Location header MUST be included with the new resource entry for the endpoint. This Location MUST be a stable identifier generated by the RD as it is used for all subsequent operations on this registration. The resource returned in the Location is only for the purpose of the Update (POST) and Removal (DELETE), and MUST NOT implement GET or PUT methods.</t>
          <t hangText="Failure:"> 4.00 "Bad Request". Malformed request. </t>
          <t hangText="Failure:"> 5.03 "Service Unavailable". Service could not perform the operation. </t>
        </list>
       	</t>

		<t>
		The following example shows an endpoint with the name "node1" registering two resources to an RD using this interface. The resulting location /rd/4521 is just an example of an RD generated location.
		</t>

		<figure>
          <artwork align="left"><![CDATA[


    EP                                                RD
     |                                                 |
     | --- POST /rd?ep=node1 "</sensors..." ------->   |
     |                                                 |
     |                                                 |
     | <-- 2.01 Created Location: /rd/4521 ----------  |
     |                                                 |
            ]]></artwork>
        </figure>

		<figure>
          <artwork align="left"><![CDATA[
Req: POST coap://rd.example.com/rd?ep=node1
Payload:
</sensors/temp>;ct=41;rt="temperature-c";if="sensor",
</sensors/light>;ct=41;rt="light-lux";if="sensor"
		
Res: 2.01 Created 
Location: /rd/4521
            ]]></artwork>
        </figure>
	
	  </section>

	  <section anchor='update' title="Update">

		<t>
		The update interface is used by an endpoint to refresh or update its registration with an RD. To use the interface, the endpoint sends a POST request to the resource returned in the Location option in the response to the first registration. An update MAY update the lifetime or context parameters if they have changed since the last registration or update. Parameters that have not changed SHOULD NOT be included in an update. Upon receiving an update request, the RD resets the timeout for that endpoint and updates the scheme, IP address and port of the endpoint (using the source address of the update, or the context parameter if present).   
		</t>
		<t>
		An update MAY optionally add or replace links for the endpoint by including those  links in the payload of the update as a CoRE Link Format document. Including links in an update message greatly increases the load on an RD and SHOULD be done infrequently. A link is replaced only if both the target URI and relation type match (TODO: Section on what is a unique link in an RD needed.)
		</t>

        <t>The update request interface is specified as follows: 
        <list style="hanging">
          <t hangText="Interaction:">EP -> RD</t>
          <t hangText="Method:">POST</t>	
          <t hangText="URI Template:">/{+location}{?lt,con}</t>
          <t hangText="URI Template Variables:"> 
          	<list>
 				<t hangText="location := ">This is the Location path returned by the RD as a result of a successful registration.</t>   
<!--
 				<t hangText="et := ">Endpoint Type (optional). The semantic type of the endpoint. The maximum length of this parameter is 63 btyes. Optional.</t>
-->
          		<t hangText="lt := ">Lifetime (optional). Lifetime of the registration in seconds. Range of 60-4294967295. If no lifetime is included, a default value of 86400 (24 hours) SHOULD be assumed.</t>
 				<t hangText="con := ">Context (optional). This parameter sets the scheme, address and port at which this server is available in the form scheme://host:port. Optional. In the absence of this parameter the scheme of the protocol, source IP address and source port used to register are assumed. </t>
          	</list>

          </t>
          <t hangText="Content-Type:">application/link-format (optional)</t>
          <t hangText="Content-Type:">application/link-format+json (optional)</t>
        </list>
       	</t>
 
 		<t>The following response codes are defined for this interface: 
        <list style="hanging">
          <t hangText="Success:"> 2.04 "Changed" in the update was successfully processed.</t>
          <t hangText="Failure:"> 4.00 "Bad Request". Malformed request. </t>
          <t hangText="Failure:"> 5.03 "Service Unavailable". Service could not perform the operation. </t>
        </list>
       	</t>

		<t>
		The following example shows an endpoint updating a new set of resources to an RD using this interface. 
		</t>


		<figure>
          <artwork align="left"><![CDATA[


     EP                                                RD
     |                                                 |
     | --- POST /rd/4521  -------------------------->   |
     |                                                 |
     |                                                 |
     | <-- 2.04 Changed  ----------------------------  |
     |                                                 |
            ]]></artwork>
        </figure>


		<figure>
          <artwork align="left"><![CDATA[
Req: POST /rd/4521
		
Res: 2.04 Changed 
            ]]></artwork>
        </figure>

	  </section>
	  
	  
	  <section anchor='removal' title="Removal">

	  <t>
	  Although RD entries have soft state and will eventually timeout after their lifetime, an endpoint SHOULD explicitly remove its entry from the RD if it knows it will no longer be available (for example on shut-down). This is accomplished using a removal interface on the RD by performing a DELETE on the endpoint resource. 
	  </t>

        <t>The removal request interface is specified as follows: 
        <list style="hanging">
          <t hangText="Interaction:">EP -> RD</t>
          <t hangText="Method:">DELETE</t>	
          <t hangText="URI Template:">/{+location}</t>
          <t hangText="URI Template Variables:"> 
          	<list>
 				<t hangText="location := ">This is the Location path returned by the RD as a result of a successful registration.</t>  
 			</list>
 		  </t>
        </list>
       	</t>

		<t>The following responses codes are defined for this interface: 
        <list style="hanging">
          <t hangText="Success:"> 2.02 "Deleted" upon successful deletion</t>
          <t hangText="Failure:"> 4.00 "Bad Request". Malformed request. </t>
          <t hangText="Failure:"> 5.03 "Service Unavailable". Service could not perform the operation. </t>
        </list>
       	</t>

		<t>The following examples shows successful removal of the endpoint from the RD.</t>


		<figure>
          <artwork align="left"><![CDATA[


    EP                                                RD
     |                                                 |
     | --- DELETE /rd/4521  ------------------------>  |
     |                                                 |
     |                                                 |
     | <-- 2.02 Deleted  ----------------------------  |
     |                                                 |
            ]]></artwork>
        </figure>


		<figure>
          <artwork align="left"><![CDATA[
Req: DELETE /rd/4521
		
Res: 2.02 Deleted 
            ]]></artwork>
        </figure>

	
	  </section>
	  
	  </section>



<!-- **************************************************************** -->
<!-- **************************************************************** -->
<!-- **************************************************************** -->
<!-- **************************************************************** -->
<section anchor='group' title="Group Function Set">

	<t>
		This section defines a function set for the creation of groups of endpoints for the purpose of managing and looking up endpoints for group operations. The group function set is similar to the resource directory function set, in that a group may be created or removed. However unlike an endpoint entry, a group entry consists of a list of endpoints and does not have a lifetime associated with it. In order to make use of multicast requests with CoAP, a group MAY have a multicast address associated with it.  
	</t>
	
	
	  <section anchor='group-register' title="Register a Group">

		<t>
			In order to create a group, a management entity used to configure groups, makes a request to the RD indicating the name of the group to create (or update), the optional domain the group belongs to, and the optional multicast address of the group. The registration message includes the list of endpoints that belong to that group. If an endpoint has already registered with the RD, the RD attempts to use the context of the endpoint from its RD endpoint entry. If the client registering the group knows the endpoint has already registered, then it MAY send a blank target URI for that endpoint link when registering the group. Configuration of the endpoints themselves is out of scope of this specification. Such an interface for managing the group membership of an endpoint has been defined in <xref target="I-D.ietf-core-groupcomm"/>. 
		</t>

        <t>The registration request interface is specified as follows: 
        <list style="hanging">
          <t hangText="Interaction:">Manager -> RD</t>
          <t hangText="Method:">POST</t>	
          <t hangText="URI Template:">/{+rd-group}{?gp,d,con}</t>
          <t hangText="URI Template Variables:"> 
          	<list>
 				<t hangText="rd-group := ">RD Group Function Set path (mandatory). This is the path of the RD Group Function Set. An RD SHOULD use the value "rd-group" for this variable whenever possible.</t>   
 				<t hangText="gp := ">Group Name (mandatory). The name of the group to be created or replaced, unique within that domain. The maximum length of this parameter is 63 bytes. </t>          	
 				<t hangText="d := ">Domain (optional). The domain to which this group belongs. The maximum length of this parameter is 63 bytes. Optional. When this parameter is elided, the RD MAY associate the endpoint with a configured default domain.</t>
 				<t hangText="con := ">Context (optional). This parameter is used to set the IP multicast address at which this server is available in the form scheme://multicast-address:port. Optional. In the absence of this parameter no multicast address is configured. </t>
          	</list>
          </t>
          <t hangText="Content-Type:">application/link-format</t>
          <t hangText="Content-Type:">application/link-format+json</t>
        </list>
       	</t>

        <t>The following response codes are defined for this interface: 
        <list style="hanging">
          <t hangText="Success:"> 2.01 "Created". The Location header MUST be included with the new group entry. This Location MUST be a stable identifier generated by the RD as it is used for delete operations on this registration.</t>
          <t hangText="Failure:"> 4.00 "Bad Request". Malformed request. </t>
          <t hangText="Failure:"> 5.03 "Service Unavailable". Service could not perform the operation. </t>
        </list>
       	</t>

		<t>
		The following example shows a group with the name "lights" registering two endpoints to an RD using this interface. The resulting location /rd-group/12 is just an example of an RD generated group location.
		</t>

		<figure>
          <artwork align="left"><![CDATA[


    EP                                                RD
     |                                                 |
     | - POST /rd-group?gp=lights "<>;ep=node1..." --> |
     |                                                 |
     |                                                 |
     | <---- 2.01 Created Location: /rd-group/12 ----  |
     |                                                 |
            ]]></artwork>
        </figure>

		<figure>
          <artwork align="left"><![CDATA[
Req: POST coap://rd.example.com/rd-group?gp=lights
Payload:
<>;ep="node1",
<>;ep="node2"
		
Res: 2.01 Created 
Location: /rd-group/12
            ]]></artwork>
        </figure>
	
	  </section>

	  <section anchor='group-removal' title="Group Removal">

	  <t>
	  A group can be removed simply by sending a removal message to the location returned when registering the group. Removing a group MUST NOT remove the endpoints of the group from the RD. 
	  </t>

        <t>The removal request interface is specified as follows: 
        <list style="hanging">
          <t hangText="Interaction:">Manager -> RD</t>
          <t hangText="Method:">DELETE</t>	
          <t hangText="URI Template:">/{+location}</t>
          <t hangText="URI Template Variables:"> 
          	<list>
 				<t hangText="location := ">This is the Location path returned by the RD as a result of a successful group registration.</t>  
 			</list>
 		  </t>
        </list>
       	</t>

		<t>The following responses codes are defined for this interface: 
        <list style="hanging">
          <t hangText="Success:"> 2.02 "Deleted" upon successful deletion</t>
          <t hangText="Failure:"> 4.00 "Bad Request". Malformed request. </t>
          <t hangText="Failure:"> 5.03 "Service Unavailable". Service could not perform the operation. </t>
        </list>
       	</t>

		<t>The following examples shows successful removal of the group from the RD.</t>


		<figure>
          <artwork align="left"><![CDATA[


    EP                                                RD
     |                                                 |
     | --- DELETE /rd-group/412  ------------------->  |
     |                                                 |
     |                                                 |
     | <-- 2.02 Deleted  ----------------------------  |
     |                                                 |
            ]]></artwork>
        </figure>


		<figure>
          <artwork align="left"><![CDATA[
Req: DELETE /rd-group/12
		
Res: 2.02 Deleted 
            ]]></artwork>
        </figure>

	
	  </section>	
	  
</section>


<!-- **************************************************************** -->
<!-- **************************************************************** -->
<!-- **************************************************************** -->
<!-- **************************************************************** -->
<section anchor='lookup' title="RD Lookup Function Set">

	  <t>
	  In order for an RD to be used for discovering resources registered with it, a lookup interface can be provided using this function set. This lookup interface is defined as a default, and it is assumed that RDs may also support lookups to return resource descriptions in alternative formats (e.g. Atom or HTML Link) or using more advanced interfaces (e.g. supporting context or semantic based lookup). 
	  </t>
	  <t>
	  This function set allows lookups for domains, groups, endpoints and resources using attributes defined in the RD Function Set and for use with the CoRE Link Format. The result of a lookup request is the list of links (if any) in CoRE Link Format corresponding to the type of lookup. The target of these links SHOULD be the actual location of the domain, endpoint or resource, but MAY be an intermediate proxy e.g. in the case of an HTTP lookup interface for CoAP endpoints. Multiple query parameters MAY be included in a lookup, all included parameters MUST match for a resource to be returned. The character '*' MAY be included at the end of a parameter value as a wildcard operator.
	  </t>

        <t>The lookup interface is specified as follows: 
        <list style="hanging">
          <t hangText="Interaction:">Client -> RD</t>
          <t hangText="Method:">GET</t>	
          <t hangText="URI Template:">/{+rd-lookup-base}/{lookup-type}{?d,ep,gp,et,rt,page,count,resource-param}</t>
          <t hangText="Parameters:">
          	<list style="hanging">
          	
          		<t hangText="rd-lookup-base := ">RD Lookup Function Set path (mandatory). This is the path of the RD Lookup Function Set. An RD SHOULD use the value "rd-lookup" for this variable whenever possible.</t>
				<t hangText="lookup-type := ">("d", "ep", "res", "gp") (mandatory) This variable is used to select the kind of lookup to perform (domain, endpoint or resource).</t>
 				<t hangText="ep := ">Endpoint (optional). Used for endpoint, group and resource lookups.</t>          	
 				<t hangText="d := ">Domain (optional). Used for domain, group, endpoint and resource lookups.</t>			
 				<t hangText="page := ">Page (optional). Parameter can not be used without the count parameter. Results are returned from result set in pages that contains 'count' results starting from index (page * count).</t>
  				<t hangText="count := ">Count (optional). Number of results is limited to this parameter value. If the parameter is not present, then an RD implementation specific default value SHOULD be used.</t>
 				<t hangText="rt := ">Resource type (optional). Used for group, endpoint and resource lookups.</t>	 
 				<t hangText="et := ">Endpoint type (optional). Used for group, endpoint and resource lookups.</t>				
 				<t hangText="resource-param := ">Link attribute parameters (optional). Any link attribute as defined in Section 4.1 of <xref target="RFC6690"/>, used for resource lookups. </t>  
          	</list>
          </t>
        </list>
       	</t>
       	
 		<t>The following responses codes are defined for this interface: 
        <list style="hanging">      	
          <t hangText="Success:"> 2.05 "Content" with an application/link-format  or application/link-format+json payload containing a matching entries for the lookup.</t>
          <t hangText="Failure:"> 4.04 "Not Found" in case no matching entry is found for a unicast request.</t>
          <t hangText="Failure:"> No error response to a multicast request.</t>
          <t hangText="Failure:"> 4.00 "Bad Request". Malformed request. </t>
          <t hangText="Failure:"> 5.03 "Service Unavailable". Service could not perform the operation. </t>
        </list>
       	</t>

<!-- Should this be split into 3 different URI Template definitions instead of one? -->

		<t>
		The following example shows a client performing a resource lookup:
		</t>

		<figure>
          <artwork align="left"><![CDATA[


   Client                                                          RD
     |                                                             |
     | ----- GET /rd-lookup/res?rt=temperature ----------------->  |
     |                                                             |
     |                                                             |
     | <-- 2.05 Content "<coap://node1/temp>;rt="temperature" ---- |
     |                                                             |


            ]]></artwork>
        </figure>
		
		<figure>
          <artwork align="left"><![CDATA[
Req: GET /rd-lookup/res?rt=temperature

Res: 2.05 Content
<coap://{ip:port}/temp>
            ]]></artwork>
        </figure>

		<t>
		The following example shows a client performing an endpoint lookup:
		</t>

		<figure>
          <artwork align="left"><![CDATA[


   Client                                                          RD
     |                                                             |
     | ----- GET /rd-lookup/ep?et=power-node -------------------->  |
     |                                                             |
     |                                                             |
     | <-- 2.05 Content "<coap://{ip:port}>;ep="node5" ----------- |
     |                                                             |


            ]]></artwork>
        </figure>
		
		<figure>
          <artwork align="left"><![CDATA[
Req: GET /rd-lookup/ep?et=power-node

Res: 2.05 Content
<coap://{ip:port}>;ep="node5",
<coap://{ip:port}>;ep="node7"
            ]]></artwork>
        </figure>			  

		<t>
		The following example shows a client performing a domain lookup:
		</t>

		<figure>
          <artwork align="left"><![CDATA[


   Client                                                          RD
     |                                                             |
     | ----- GET /rd-lookup/d ---------------------------------->  |
     |                                                             |
     |                                                             |
     | <-- 2.05 Content "</rd>;d=domain1,</rd>;d=domain2 --------- |
     |                                                             |


            ]]></artwork>
        </figure>
		
		<figure>
          <artwork align="left"><![CDATA[
Req: GET /rd-lookup/d

Res: 2.05 Content
</rd>;d="domain1",
</rd>;d="domain2"
            ]]></artwork>
        </figure>	

		<t>
		The following example shows a client performing a group lookup for all groups:
		</t>

		<figure>
          <artwork align="left"><![CDATA[


   Client                                                          RD
     |                                                             |
     | ----- GET /rd-lookup/gp --------------------------------->  |
     |                                                             |
     |                                                             |
     | <-- 2.05 Content </rd-group/12>;gp="lights1";d="domain1" -- |
     |                                                             |


            ]]></artwork>
        </figure>
		
		<figure>
          <artwork align="left"><![CDATA[
Req: GET /rd-lookup/gp

Res: 2.05 Content
</rd-group/12>;gp="lights1";d="domain1"
            ]]></artwork>
        </figure>	
	
		<t>
		The following example shows a client performing a lookup for all endpoints in a particular group:
		</t>

		<figure>
          <artwork align="left"><![CDATA[


   Client                                                          RD
     |                                                             |
     | ----- GET /rd-lookup/ep?gp=lights1----------------------->  |
     |                                                             |
     |                                                             |
     | <-- 2.05 Content "</rd>;d=domain1,</rd>;d=domain2 --------- |
     |                                                             |


            ]]></artwork>
        </figure>
		
		<figure>
          <artwork align="left"><![CDATA[
Req: GET /rd-lookup/ep?gp=lights1

Res: 2.05 Content
<coap://host:port>;ep="node1",
<coap://host:port>;ep="node2",
            ]]></artwork>
        </figure>		

		<t>
		The following example shows a client performing a lookup for all groups an endpoint belongs to:
		</t>

		<figure>
          <artwork align="left"><![CDATA[


   Client                                                          RD
     |                                                             |
     | ----- GET /rd-lookup/gp?ep=node1 ------------------------>  |
     |                                                             |
     |                                                             |
     | <-- 2.05 Content "</rd>;d=domain1,</rd>;d=domain2 --------- |
     |                                                             |


            ]]></artwork>
        </figure>
		
		<figure>
          <artwork align="left"><![CDATA[
Req: GET /rd-lookup/gp?ep=node1

Res: 2.05 Content
<coap://host:port>;gp="lights1";ep="node1",
            ]]></artwork>
        </figure>	

	  
  </section>


  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->

  <section anchor="attributes" title="New Link-Format Attributes">

	<t>
	When using the CoRE Link Format to describe resources being discovered by or posted to a resource directory service, additional information about those resources is useful. This specification defines the following new attributes for use in the CoRE Link Format <xref target="RFC6690"/>:
	</t>

      <figure>
         <artwork><![CDATA[
         
   link-extension    = ( "ins" "=" quoted-string ) ; Max 63 bytes         
   link-extension    = ( "exp" ) 

         ]]></artwork>
       </figure>

	<section title="Resource Instance 'ins' attribute">
	
	 	 <t>
	 	 The Resource Instance "ins" attribute is an identifier for this resource, which makes it possible to distinguish from other similar resources. This attribute is similar in use to the "Instance" portion of a DNS-SD record, and SHOULD be unique across resources with the same Resource Type attribute in the domain it is used. A Resource Instance might be a descriptive string like "Ceiling Light, Room 3", a short ID like "AF39" or a unique UUID or iNumber. This attribute is used by a Resource Directory to distinguish between multiple instances of the same resource type within a system.
	 	 </t>
 	 
	 	 <t>
	 	 This attribute MUST be no more than 63 bytes in length. The resource identifier attribute MUST NOT appear more than once in a link description. 
         </t>
	
	</section>
   
	<section title="Export 'exp' attribute">
	
	 	 <t>
	 	 The Export "exp" attribute is used as a flag to indicate that a link description MAY be exported by a resource directory to external directories.  
	 	 </t>
	 	 <t>
	 	 The CoRE Link Format is used for many purposes between CoAP endpoints. Some are useful mainly locally, for example checking the observability of a resource before accessing it, determining the size of a resource, or traversing dynamic resource structures. However, other links are very useful to be exported to other directories, for example the entry point resource to a functional service. 
         </t>
	
	</section>   
   
  </section>

  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->

  <section anchor="dns-sd" title="DNS-SD Mapping">

	<t>
		TODO
	</t>
   
  </section>

  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->

  <section title="Security Considerations">
         <t> 
         This document needs the same security considerations as described in Section 7 of <xref target="RFC5988"/> and Section 6 of <xref target="RFC6690"/>. The /.well-known/core resource may be protected e.g. using DTLS when hosted on a CoAP server as described in <xref target="I-D.ietf-core-coap"/>. 
     	 </t>
     	 <t>
     	 Access control SHOULD be performed separately for the RD Function Set and the RD Lookup Function Set, as different endpoints may be authorized to register with an RD from those authorized to lookup endpoints from the RD. Such access control SHOULD be performed in as fine-grained a level as possible. For example access control for lookups could be performed either at the domain, endpoint or resource level. 
     	 </t>

	<t>
Services that run over UDP unprotected are vulnerable to unknowingly become part of a DDoS attack as UDP does not require return routability check. Therefore, an attacker can easily spoof the source IP of the target entity and send requests to such a service which would then respond to the target entity. This can be used for large-scale DDoS attacks on the target. Especially, if the service returns a response that is order of magnitudes larger than the request, the situation becomes even worse as now the attack can be amplified. DNS servers have been widely used for DDoS amplification attacks. Recently, it has been observed that NTP Servers, that also run on unprotected UDP have been used for DDoS attacks (http://tools.cisco.com/security/center/content/CiscoSecurityNotice/CVE-2013-5211) [TODO: Ref] since there is no return routability check and can have a large amplification factor. The responses from the NTP server were found to be 19 times larger than the request. A Resource Directory (RD) which responds to wild-card lookups is potentially vulnerable if run with CoAP over UDP. Since there is no return routability check and the responses can be significantly larger than requests, RDs can unknowingly become part of a DDoS amplification attack. Therefore, it is recommended that implementations must ensure return routability. This can be done, for example by responding to wild card lookups only over DTLS or TLS or TCP.
	</t>

  </section>

  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->

  <section title="IANA Considerations">

  <section anchor="iana-rt" title="Resource Types">

	<t>
	"core.rd", "core.rd-group" and "core.rd-lookup" resource types need to be registered with the resource type registry defined by <xref target="RFC6690"/>.
	</t>
  
  </section>	
	
  <section anchor="iana-link-ext" title="Link Extension">	
	<t>
	The "exp" attribute needs to be registered when a future Web Linking link-extension registry is created (e.g. in RFC5988bis).
	</t>
  </section>
     
  <section anchor="iana-registry" title="RD Parameter Registry">   
     
     <t>
     This specification defines a new sub-registry for registration and lookup parameters called "RD Parameters" under "CoRE Parameters". Although this specification defines a basic set of parameters, it is expected that other standards that make use of this interface will define new ones.  
     </t>
     
<!-- Size, format and syntax of registry entries -->
	<t>
	Each entry in the registry must include the human readable name of the parameter, the query parameter, validity requirements if any and a description. The query parameter MUST be a valid URI query key <xref target="RFC3986"/>. 
	</t>

    <!-- Initial assignments and reservations -->
    <t>Initial entries in this sub-registry are as follows:</t>

    <texttable anchor="tab-registry" title="RD Parameters">
          <ttcol align="left">Name</ttcol>
          <ttcol align="left">Query</ttcol>
          <ttcol align="left">Validity</ttcol>
          <ttcol align="left">Description</ttcol>

          <c>Endpoint Name</c><c>ep</c><c> < 63 bytes</c><c>Name of the endpoint</c>
		  <c>Lifetime</c><c>lt</c><c>60-4294967295</c><c>Lifetime of the registration in seconds</c>
		  <c>Domain</c><c>d</c><c> < 63 bytes</c><c>Domain to which this endpoint belongs</c>
		  <c>Endpoint Type</c><c>et</c><c></c><c>Semantic name of the endpoint</c>
		  <c>Context</c><c>con</c><c>URI</c><c>The scheme, address and port at which this server is available</c>
		  <c>Endpoint Name</c><c>ep</c><c></c><c>Name of the endpoint, max 63 bytes</c>
		  <c>Group Name</c><c>gp</c><c></c><c>Name of a group in the RD</c>
		  <c>Page</c><c>page</c><c>Integer</c><c>Used for pagination</c>
		  <c>Count</c><c>count</c><c>Integer</c><c>Used for pagination</c>
    </texttable>
     
     <t>
     The IANA policy for future additions to the sub-registry is "Expert Review" as described in <xref target="RFC5226"/>
     </t>     
     
  </section>

</section>

  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->

<!-- TODO: Add an advanced use case example section -->

<section anchor="examples" title="Examples">

	<t>
	
	</t>

</section>


<section title="Acknowledgments">

<t>Srdjan Krco, Szymon Sasin, Kerry Lynn, Esko Dijk, Peter van der Stok, Anders Brandt, Matthieu Vial, Michael Koster, Mohit Sethi, Sampo Ukkola and Linyi Tian have provided helpful comments, discussions and ideas to improve and shape this document. The authors would also like to thank their collagues from the EU FP7 SENSEI project, where many of the resource directory concepts were originally developed.</t>

</section>


  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->

  <section title="Changelog">

    <t>Changes from -01 to -02:
      <list>
        <t>o Added a catalogue use case.</t>
        <t>o Changed the registration update to a POST with optional link format payload. Removed the endpoint type update from the update.</t>
        <t>o Additional examples section added for more complex use cases.</t>
        <t>	</t>
      </list>
    </t>

    <t>Changes from -00 to -01:
      <list>
        <t>o Removed the ETag validation feature.</t>
        <t>o Place holder for the DNS-SD mapping section.</t>
        <t>o Explicitly disabled GET or POST on returned Location.</t>
        <t>o New registry for RD parameters.</t>
        <t>o Added support for the JSON Link Format.</t>
        <t>o Added reference to the Groupcomm WG draft.</t>
      </list>
    </t>

    <t>Changes from -05 to WG Document -00:
      <list>
        <t>o Updated the version and date.</t>
      </list>
    </t>

    <t>Changes from -04 to -05:
      <list>
        <t>o Restricted Update to parameter updates.</t>
        <t>o Added pagination support for the Lookup interface.</t>
        <t>o Minor editing, bug fixes and reference updates.</t>
        <t>o Added group support.</t>
        <t>o Changed rt to et for the registration and update interface. </t>
      </list>
    </t>

    <t>Changes from -03 to -04:
      <list>
        <t>o Added the ins= parameter back for the DNS-SD mapping.</t>
        <t>o Integrated the Simple Directory Discovery from Carsten.</t>
        <t>o Editorial improvements.</t>
        <t>o Fixed the use of ETags.</t>
      </list>
    </t>

    <t>Changes from -02 to -03:
      <list>
        <t>o Changed the endpoint name back to a single registration parameter ep= and removed the h= and ins= parameters.</t>
        <t>o Updated REST interface descriptions to use RFC6570 URI Template format.</t>
        <t>o Introduced an improved RD Lookup design as its own function set.</t>
        <t>o Improved the security considerations section.</t>   
        <t>o Made the POST registration interface idempotent by requiring the ep= paramter to be present.</t>  
      </list>
    </t>

    <t>Changes from -01 to -02:
      <list>
        <t>o Added a terminology section.</t>
        <t>o Changed the inclusing of an ETag in registration or update to a MAY.</t>
		<t>o Added the concept of an RD Domain and a registration parameter for it. </t>
		<t>o Recommended the Location returned from a registration to be stable, allowing for endpoint and Domain information to be changed during updates. </t>
		<t>o Changed the lookup interface to accept endpoint and Domain as query string parameters to control the scope of a lookup. </t>
      </list>
    </t>

  </section>

    </middle>

    <back>
    <references title='Normative References'>
       &RFC6690;
       &RFC2119;
       &RFC3986;
       &RFC5226;
       &RFC5988;
       &RFC6570;
       &I-D.ietf-core-links-json;
           
    </references>

    <references title='Informative References'>
		&I-D.ietf-core-coap;
		&I-D.vanderstok-core-bc;
		&I-D.brandt-coap-subnet-discovery;
		&I-D.ietf-core-groupcomm;
		&RFC6775;
		&RFC2616;
       
    </references>
    </back>

</rfc>
