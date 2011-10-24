<?xml version="1.0" encoding="US-ASCII"?>
<!-- This template is for creating an Internet Draft using xml2rfc,
     which is available here: http://xml.resource.org. -->
<!DOCTYPE rfc SYSTEM "rfc2629.dtd" [
<!ENTITY RFC0792 SYSTEM "http://xml.resource.org/public/rfc/bibxml/reference.RFC.0792.xml">
<!ENTITY RFC2045 SYSTEM "http://xml.resource.org/public/rfc/bibxml/reference.RFC.2045.xml">
<!ENTITY RFC2046 SYSTEM "http://xml.resource.org/public/rfc/bibxml/reference.RFC.2046.xml">
<!ENTITY RFC2616 SYSTEM "http://xml.resource.org/public/rfc/bibxml/reference.RFC.2616.xml">
<!ENTITY RFC3986 SYSTEM "http://xml.resource.org/public/rfc/bibxml/reference.RFC.3986.xml">
<!ENTITY RFC4288 SYSTEM "http://xml.resource.org/public/rfc/bibxml/reference.RFC.4288.xml">
<!ENTITY RFC4346 SYSTEM "http://xml.resource.org/public/rfc/bibxml/reference.RFC.4346.xml">
<!ENTITY RFC4347 SYSTEM "http://xml.resource.org/public/rfc/bibxml/reference.RFC.4347.xml">
<!ENTITY RFC4944 SYSTEM "http://xml.resource.org/public/rfc/bibxml/reference.RFC.4944.xml">
<!ENTITY RFC5234 SYSTEM "http://xml.resource.org/public/rfc/bibxml/reference.RFC.5234.xml">
<!ENTITY RFC5785 SYSTEM "http://xml.resource.org/public/rfc/bibxml/reference.RFC.5785.xml">
<!ENTITY RFC5988 SYSTEM "http://xml.resource.org/public/rfc/bibxml/reference.RFC.5988.xml">
<!ENTITY I-D.ietf-core-coap SYSTEM "http://xml.resource.org/public/rfc/bibxml3/reference.I-D.draft-ietf-core-coap-06.xml">
<!ENTITY I-D.ietf-core-link-format SYSTEM "http://xml.resource.org/public/rfc/bibxml3/reference.I-D.draft-ietf-core-link-format-06.xml">
<!ENTITY I-D.shelby-core-coap-req SYSTEM "http://xml.resource.org/public/rfc/bibxml3/reference.I-D.draft-shelby-core-coap-req-01.xml">
<!ENTITY I-D.vanderstok-core-bc SYSTEM "http://xml.resource.org/public/rfc/bibxml3/reference.I-D.draft-vanderstok-core-bc-03.xml">
<!ENTITY I-D.brandt-coap-subnet-discovery SYSTEM "http://xml.resource.org/public/rfc/bibxml3/reference.I-D.draft-brandt-coap-subnet-discovery-00.xml">

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
<rfc category="std" ipr="trust200902" docName="draft-shelby-core-resource-directory-01">

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
             Sensinode
          </organization>
          <address>
            <postal>
             <street>Kidekuja 2</street>
             <city>Vuokatti</city>
             <code>88600</code>
             <country>FINLAND</country>
            </postal>
            <phone>+358407796297</phone>
            <email>zach@sensinode.com</email>
          </address>
        </author>

        <author initials="S" surname="Krco" fullname="Srdjan Krco">
          <organization>
             Ericsson
          </organization>
          <address>
            <postal>
             <street></street>
             <city></city>
             <code></code>
             <country></country>
            </postal>
            <phone></phone>
            <email>srdjan.krco@ericsson.com</email>
    	   </address>
        </author>

  <date year="2011"/>

  <area>Internet</area>

  <workgroup>CoRE</workgroup>
  <keyword>CoRE, Web Linking, Resource Discovery, Resource Directory</keyword>

    <abstract>
    <t>
	In many M2M scenarios, direct discovery of resources is not practical due to sleeping nodes, disperse networks, or networks where multicast traffic is inefficient. These problems can be solved by employing an entity called a Resource Directory (RD), which hosts descriptions of resources held on other servers, allowing lookups to be performed for those resources. This document specifies the web interfaces that a Resource Directory supports in order for web servers to discover the RD and to registrer, maintain, lookup and remove resources descriptions. Furthermore, new link attributes useful in conjunction with an RD are defined. 
	</t> 
    
    </abstract>
    </front>

    <middle>


<!--
Comments from IETF Quebec:

issues:
- terminology is a mess
- how to differentiate between multiple RDs in discovery
- how to tell the RD about the host
- hannes > trying to give meaning to the nice Architecture picture in the presentation

-->


  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <section anchor='introduction' title="Introduction">

  <t>
  The Constrained RESTful Environments (CoRE) working group aims at realizing the REST architecture in a suitable form for the most constrained nodes (e.g. 8-bit microcontrollers with limited RAM and ROM) and networks (e.g. 6LoWPAN). CoRE is aimed at machine-to-machine (M2M) applications such as smart energy and building automation <xref target="I-D.shelby-core-coap-req"/>.
  </t>
  <t>
  The discovery of resources offered by a constrained server is very important in machine-to-machine applications where there are no humans in the loop and static interfaces result in fragility. The discovery of resources provided by an HTTP Web Server is typically called Web Linking <xref target="RFC5988"/>. The use of Web Linking for the description and discovery of resources hosted by constrained web servers is specified by the CoRE Link Format <xref target="I-D.ietf-core-link-format"/>. This specification however only describes how to discover resources from the web server that hosts them by requesting /.well-known/core. In many M2M scenarios, direct discovery of resources is not practical due to sleeping nodes, disperse networks, or networks where multicast traffic is inefficient. These problems can be solved by employing an entity called a Resource Directory (RD), which hosts descriptions of resources held on other servers, allowing lookups to be performed for those resources.
  </t>
  <t>
  This document specifies the web interfaces that a Resource Directory supports in order for web servers to discover the RD and to registrer, maintain, lookup and remove resources descriptions. Furthermore, new link attributes useful in conjunction with a Resource Directory are defined. Although the examples in this document show the use of these interfaces with CoAP <xref target="I-D.ietf-core-coap"/>, they may be applied in an equivalent manner to HTTP <xref target="RFC2616"/>. 
  </t>

  </section>
  

  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <section anchor='arch' title="Architecture and Use Cases">

	<t>
	The resource directory architecture is shown in <xref target="fig-arch"/>. A Resource Directory (RD) is used as a repository for Web Links <xref target="RFC5988"/> about resources hosted on other web servers, which are called end-points (EP). An end-point is a web server associated with a port, thus a physical node may host one or more end-points. The RD implements a set of REST interfaces for end-points to register and maintain sets of Web Links (called resource directory entries), for the RD to validate entries, and for clients to lookup resources from the RD. End-points themselves can also act as clients.
	</t>
	<t>
	End-points are assumed to proactively register and maintain resource directory entries on the RD, which are soft state and need to be periodially refreshed. An EP is provided with interfaces to register, update and remove a resource directory entry. Furthermore, a mechanism to discover a RD using the CoRE Link Format is defined. It is also possible for an RD to proactively discover Web Links from EPs and add them as resource directory entries, or to validate existing resource directory entries. A lookup interface for discovering any of the Web Links held in the RD is provided using the CoRE Link Format. 
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
	  Over the last few years, mobile operators around the world have focused on development of M2M solutions in order to expand the business to the new type of users, i.e. machines. The machines are connected directly to a mobile network using appropriate embedded air interface (GSM/GPRS, WCDMA, LTE) or via a gateway providing short and wide range wireless interfaces. From the system design point of view, the ambition is to design horizontal solutions that can enable utilization of machines in different applications depending on their current availability and capabilities as well as application requirements, thus avoiding silo like solutions. One of the crucial enablers of such design is the ability to discover resources (machines - End Points) capable of providing required information at a given time or acting on instructions from the end users. 	
	  </t>
	  
	  <t>
	  In a typical scenario, during a boot-up procedure (and periodically afterwards), the machines (EPs) register with a Resource Directory (for example EPs installed on vehicles enabling tracking of their position for the fleet management purposes and monitoring environment parameters) hosted by the mobile operator or somewhere else in the network, submiting a description of own capabilities. Due to the usual network configuration of mobile networks, the EPs attached to the mobile network do not have routable addresses. Therefore, a remote server is usually used to provide proxy access to the EPs. The address of each (proxy) EP on this server is included in the resource description stored in the RD. The users, for example mobile applications for environment monitoring, contact the RD, look-up the EPs capable of providing information about the environment using appropriate set of tags, obtain information on how to contact them (URLs of the proxy server) and then initate interaction to obtain information that is finally processed, displayed on the screen and usually stored in a database. Similarly, fleet management systems provide a set of credentials along with the appropriate tags to the RD to look-up for EPs deployed on the vehicles the application is responsible for. 
	  </t>
	  </section>

	  <section anchor='automation' title="Use Case: Home and Building Automation">
	  <t>
	  Home and commercial building automation systems can benefit from the use of M2M web services. The use of CoRE in home automation across multiple subnets is described in <xref target="I-D.brandt-coap-subnet-discovery"/> and in commercial building automation in <xref target="I-D.vanderstok-core-bc"/>. The discovery requirements of these applications are demanding. Home automation usually relies on run-time discovery to commision the system, whereas in building automation a combination of professional commissioning and run-time discovery. Both home and building automation involve peer-to-peer interactions between end-points, and involve battery-powered sleeping devices. 
	  </t>
	  <t>
	  The exporting of resource information to other discovery systems is also important in these automation applications. In home automation there is a need to interact with other consumer electronics, which may already support DNS-SD, and in building automation larger resource directories or DNS-SD covering multiple buildings.
	  </t>
	  
	  </section>

  </section>

  

  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <section anchor='rd' title="Resource Directory Interfaces">

	<t>
	This section defines the REST interfaces between an RD and end-points, and a lookup interface between an RD and clients. Although the examples throughout this section assume use of CoAP    
	<xref target="I-D.ietf-core-coap"/>, these REST interfaces can also be realized using HTTP <xref target="RFC2616"/>. An RD implementing this specification MUST support the discovery, registration, update, removal and lookup interfaces defined in this section and MAY support the validation interface. For the purpose of validation, an end-point implementing this specification SHOULD support Etag validation on /.well-known/core.
	</t>

	  <section anchor='discovery' title="Discovery">
	
		<t>
		Before an end-point can make use of an RD, it must first know its location and optionally the path of the RD root resource. There can be several mechanisms for discovering the RD including assuming a default location (e.g. on an Edge Router in a LoWPAN), by assigning an anycast address to the RD, using DHCP, or by discovering the RD using the CoRE Link Format. This section defines discovery of the RD using the well-known interface of the CoRE Link Format <xref target="I-D.ietf-core-link-format"/> as the required mechanism. It is however expected that RDs will also be discoverable via other methods depending on the deployment. 
		</t>
		
		<t>
		Discovery is performed by sending either a multicast or unicast GET request to /.well-known/core and including a Resource Type (rt) parameter <xref target="I-D.ietf-core-link-format"/> with the value "core-rd" in the query string. Upon success, the response will contain a payload with a link format entry for each RD discovered, with the URL indicating the root resource of the RD. When performing multicast discovery, the multicast IP address used will depend on the scope required and the multicast capabilities of the network (TBD if a specific multicast address should be defined for RDs).
		</t>
		
		<t>
		An RD implementing this specification MUST support query filtering for the rt parameter as defined in <xref target="I-D.ietf-core-link-format"/>.
		</t>

        <t>The discovery interface is specified as follows: 
        <list style="hanging">
          <t hangText="Interaction:">EP -> RD</t>
          <t hangText="Path:">/.well-known/core</t>
          <t hangText="Method:">GET</t>	
          <t hangText="Content-Type:">application/link-format (if any)</t>
          <t hangText="Parameters:"> 
          	<list style="hanging">
          		<t hangText="Resource Type (rt):">MUST contain the value "core-rd"</t>
          		<t hangText="Instance (ins):">Used to differentiate between multiple RDs.</t>
          	</list>
          </t>
          <t hangText="Success:"> 2.05 "Content" with an application/link-format payload containing a matching entry for the RD resource.</t>
          <t hangText="Failure:"> 2.05 "Content" (should be a "No Content" code?) with an empty payload is returned in case no matching entry is found for a unicast request.</t>
          <t hangText="Failure:"> No error response to a multicast request.</t>
          <t hangText="Failure:"> 4.00 "Bad Request" </t>
        </list>
       	</t>

		<t>
		The following example shows an end-point discovering an RD using this interface, thus learning that the base RD resource is at /rd. Note that it is up to the RD to choose its base RD resource. 
		</t>

		<figure>
          <artwork align="left"><![CDATA[


 End-point                                             RD
     |                                                 |
     | ----- GET /.well-known/core?rt=core-rd ------>  |
     |                                                 |
     |                                                 |
     | <---- 2.05 Content "</rd>; rt="core-rd" ------  |
     |                                                 |


            ]]></artwork>
        </figure>
		
		
		
		<figure>
          <artwork align="left"><![CDATA[
Req: GET coap://[ff02::1]/.well-known/core?rt=core-rd
		
Res: 2.05 Content
</rd>; rt="core-rd"; ins="Primary"
            ]]></artwork>
        </figure>
		
	
	  </section>
	  
	
	  <section anchor='registration' title="Registration">

		<t>
		After discovering the location of an RD, an end-point MAY register its resources to the RD's registration interface. This interface accepts a POST from an end-point containing the list of resources to be added to the directory as the message payload in the CoRE Link Format along with query string parameters indicating the name of the end-point, an optional node identifier and the lifetime of the registration. The end-point name is formed by concatenating the Host and Instance parameters included with the registration. All parameters of the registration are optional. In the absense of a Host parameter, the RD will generate a unique one on behalf of the end-point. The RD then creates a new resource or updates an existing resource in the RD and returns its location. An end-point MUST use that location when refreshing registrations using this interface. End-point resources in the RD are kept active for the period indicated by the lifetime parameter. The end-point is reponsible for refreshing the entry within this period using either the registration or update interface. 
		</t>

        <t>The registration interface is specified as follows: 
        <list style="hanging">
          <t hangText="Interaction:">EP -> RD</t>
          <t hangText="Path:">/.well-known/core or /{rd-base}</t>
          <t hangText="Method:">POST</t>	
          <t hangText="Content-Type:">application/link-format</t>
          <t hangText="Etag:">The Etag option MUST be included to allow an RD to perform validation in the future.</t>
          <t hangText="Parameters:"> 
          	<list>
          		<t hangText="Lifetime (lt):">Lifetime of the registration in seconds. Range of 60-4294967295. If no lifetime is included, a default value of 86400 (24 hours) SHOULD be assumed.</t>
 				<t hangText="Host (h):">The host identifier or name of the registering node. The maximum length of this parameter is 63 octets. This parameter is combined with the Instance parameter (if any) to form the end-point name. If not included, the RD MUST generate a unique Host name on behalf of the node. </t>
 				<t hangText="Instance (ins):">The instance of the end-point on this host, if there are multiple. The maximum length of this parameter is 63 octets.</t>
 				<t hangText="Type (rt):">The semantic type of end-point. The maximum length of this parameter is 63 octets.</t>
          	</list>
          </t>
          <t hangText="Success:"> 2.01 "Created". The Location header of the new resource entry for the end-point could be e.g. in the form /{rd-base}/{end-point name}</t>
          <t hangText="Failure:"> 4.00 "Bad Request". Malformed request. </t>
          <t hangText="Failure:"> 5.03 "Service Unavailable". Service could not perform the operation. </t>
        </list>
       	</t>

<!-- TODO: Explain hos host and ins are used, and that the RD generates them if ommitted. The combination of host + ins must be unique within the RD. -->

<!-- TODO: Explain how the RD generates the Location: and how the node will use it. -->

		<t>
		The following example shows an end-point with the name "node1" registering two resources to an RD using this interface. 
		</t>

		<figure>
          <artwork align="left"><![CDATA[


 End-point                                             RD
     |                                                 |
     | --- POST /rd "</sensors..." ---------------->   |
     |                                                 |
     |                                                 |
     | <-- 2.01 Created Location: /rd/node1 ---------  |
     |                                                 |
            ]]></artwork>
        </figure>

		<figure>
          <artwork align="left"><![CDATA[
Req: POST coap://rd.example.org/rd?h=node1&lt=1024
Etag: 0x3f
Payload:
</sensors/temp>;ct=41;rt="TemperatureC";if="sensor",
</sensors/light>;ct=41;rt="LightLux";if="sensor"
		
Res: 2.01 Created 
Location: /rd/node1
            ]]></artwork>
        </figure>
	
	  </section>


	  <section anchor='update' title="Update">

		<t>
		The update interface is used by an end-point to refresh or update its registration with an RD. To use the interface, the end-point sends a PUT request to the resource returned in the Location option in the response to the first registration. An update MAY contain a payload in CoRE Link Format if there have been changes since the last registration or update. 
		</t>


        <t>The update interface is specified as follows: 
        <list style="hanging">
          <t hangText="Interaction:">EP -> RD</t>
          <t hangText="Path:">Location returned by registration.</t>
          <t hangText="Method:">PUT</t>	
          <t hangText="Content-Type:">application/link-format (if any)</t>
          <t hangText="Etag:">The Etag option MUST be included to allow an RD to compare the existing entry and perform validation in the future.</t>
          <t hangText="Parameters:"> 
          	<list>
          		<t hangText="Lifetime (lt):">Lifetime of the registration in seconds. Range of 60-4294967295. If no lifetime is included, a default value of 86400 (24 hours) SHOULD be assumed.</t>
          	</list>
          </t>
          <t hangText="Success:"> 2.04 "Changed" in case the resource and/or lifetime was successfully updated</t>
          <t hangText="Failure:"> 4.00 "Bad Request". Malformed request. </t>
          <t hangText="Failure:"> 5.03 "Service Unavailable". Service could not perform the operation. </t>
        </list>
       	</t>

		<t>
		The following example shows an end-point updating a new set of resources to an RD using this interface. 
		</t>


		<figure>
          <artwork align="left"><![CDATA[


 End-point                                             RD
     |                                                 |
     | --- PUT /rd/node1 "</sensors..." ------------>  |
     |                                                 |
     |                                                 |
     | <-- 2.04 Changed  ----------------------------  |
     |                                                 |
            ]]></artwork>
        </figure>


		<figure>
          <artwork align="left"><![CDATA[
Req: PUT /rd/node1
Etag: 0x40
Payload:
</sensors/temp/1>;ct=41;ins="Indoor";rt="TemperatureC";if="sensor",
</sensors/temp/2>;ct=41;ins="Outdoor";rt="TemperatureC";if="sensor",
</sensors/light>;ct=41;rt="LightLux";if="sensor"
		
Res: 2.04 Changed 
            ]]></artwork>
        </figure>

	
	  </section>
	  
	  
	  <section anchor='validation' title="Validation">

	  <t>
	  In some cases, an RD may want to validate that it has the latest version of an end-point's resource. This can be performed with a GET on the well-known interface of the CoRE Link Format including the latest Etag stored for that end-point. For the purpose of validation, an end-point implementing this specification SHOULD support Etag validation on /.well-known/core.
	  </t>

        <t>The validation interface is specified as follows: 
        <list style="hanging">
          <t hangText="Interaction:">RD -> EP</t>
          <t hangText="Path:">/.well-known/core</t>
          <t hangText="Method:">GET</t>	
          <t hangText="Content-Type:">application/link-format (if any)</t>
          <t hangText="Etag:">The Etag option MUST be included</t>
          <t hangText="Parameters:"> None</t>
          <t hangText="Success:"> 2.03 "Valid" in case the Etag matches</t>
          <t hangText="Success:"> 2.05 "Content" in case the Etag does not match, the response MUST include the most recent resource representation and its corresponding Etag.</t>
          <t hangText="Failure:"> 4.00 "Bad Request". Malformed request. </t>
        </list>
       	</t>

		<t>The following examples shows a successful validation.</t>

		<figure>
          <artwork align="left"><![CDATA[


 End-point                                             RD
     |                                                 |
     | <--- GET /.well-known/core Etag: 0x40 --------  |
     |                                                 |
     |                                                 |
     | --- 2.03 Valid  ----------------------------->  |
     |                                                 |
            ]]></artwork>
        </figure>


		<figure>
          <artwork align="left"><![CDATA[
Req: GET coap://{end-point}/.well-known/core
Etag: 0x40
		
Res: 2.03 Valid 
            ]]></artwork>
        </figure>
        
	
	  </section>

	  <section anchor='removal' title="Removal">

	  <t>
	  Although RD entries have soft state and will eventually timeout after their lifetime, an end-point SHOULD explicitly remove its entry from the RD if it knows it will no longer be available (for example on shut-down). This is accomplished using a removal interface on the RD by performing a DELETE on the end-point resource. 
	  </t>

        <t>The removal interface is specified as follows: 
        <list style="hanging">
          <t hangText="Interaction:">EP -> RD</t>
          <t hangText="Path:">Location returned by registration.</t>
          <t hangText="Method:">DELETE</t>	
          <t hangText="Content-Type:">None</t>
          <t hangText="Parameters:"> None</t>
          <t hangText="Success:"> 2.02 "Deleted" upon successful deletion</t>
          <t hangText="Failure:"> 4.00 "Bad Request". Malformed request. </t>
          <t hangText="Failure:"> 5.03 "Service Unavailable". Service could not perform the operation. </t>
        </list>
       	</t>

		<t>The following examples shows successful removal of the end-point from the RD.</t>


		<figure>
          <artwork align="left"><![CDATA[


 End-point                                             RD
     |                                                 |
     | --- DELETE /rd/node1  ----------------------->  |
     |                                                 |
     |                                                 |
     | <-- 2.02 Deleted  ----------------------------  |
     |                                                 |
            ]]></artwork>
        </figure>


		<figure>
          <artwork align="left"><![CDATA[
Req: DELETE /rd/node1
		
Res: 2.02 Deleted 
            ]]></artwork>
        </figure>

	
	  </section>


	  <section anchor='lookup' title="Lookup">

	  <t>
	  In order for an RD to be used for discovering resources registered with it, a lookup interface is provided. This lookup interface is provided as a default, and it is assumed that RDs may also support lookups to return resource descriptions in alternative formats (e.g. Atom or HTML Link) or using more advanced interfaces (e.g. supporting context or semantic based lookup). 
	  </t>
	  <t>
	  The lookup interface is provided using the CoRE Link Format <xref target="I-D.ietf-core-link-format"/> resource discovery mechanism on the root RD resource (/rd in the examples). The scope of the discovery is controlled by the depth of the resource the query is made on. A lookup on the root RD resource /rd queries all resource descriptions on the RD, whereas a lookup on /rd/node1 queries all resource descriptions held in the "node1" entry. An RD MUST support the query filtering defined in <xref target="I-D.ietf-core-link-format"/> to allow for filtered lookups.  
	  </t>

        <t>The lookup interface is specified as follows: 
        <list style="hanging">
          <t hangText="Interaction:">Client -> RD</t>
          <t hangText="Path:">/{rd-base} or e.g. /{rd-base}/{end-point}</t>
          <t hangText="Method:">GET</t>	
          <t hangText="Content-Type:">application/link-format (if any)</t>
          <t hangText="Parameters:"> 
          	<list style="hanging">
          		<t hangText="Filtering:">CoRE Link Format attributes may be included to filter the lookup.</t>
          	</list>
          </t>
          <t hangText="Success:"> 2.05 "Content" with an application/link-format payload containing a matching entries for the lookup.</t>
          <t hangText="Failure:"> 2.05 "Content" (should be a "No Content" code?) with an empty payload is returned in case no matching entry is found for a unicast request.</t>
          <t hangText="Failure:"> No error response to a multicast request.</t>
          <t hangText="Failure:"> 4.00 "Bad Request". Malformed request. </t>
          <t hangText="Failure:"> 5.03 "Service Unavailable". Service could not perform the operation. </t>
        </list>
       	</t>

		<t>
		The following example shows a client performing a lookup on an RD using this interface. 
		</t>

		<figure>
          <artwork align="left"><![CDATA[


   Client                                                          RD
     |                                                             |
     | ----- GET /rd/node1?rt=Temperature ---------------------->  |
     |                                                             |
     |                                                             |
     | <-- 2.05 Content "<coap://node1/temp>;rt="Temperature" ---- |
     |                                                             |


            ]]></artwork>
        </figure>
		
		
		
		<figure>
          <artwork align="left"><![CDATA[
Req: GET /rd/node1?rt=Temperature

Res: 2.05 Content
<coap://node1/temp>;rt="Temperature"
            ]]></artwork>
        </figure>
			  
	  
	  </section>

  </section>


  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->

  <section anchor="attributes" title="New Link-Format Attributes">

	<t>
	When using the CoRE Link Format to describe resources being discovered by or posted to a resource directory service, additional information about those resources is useful. This specification defines the following new attributes for use in the CoRE Link Format <xref target="I-D.ietf-core-link-format"/>:
	</t>

      <figure>
         <artwork><![CDATA[
         
   link-extension    = ( "ins" "=" quoted-string ) ; Max 63 octets
   link-extension    = ( "exp" ) 

         ]]></artwork>
       </figure>


	<section title="Resource Instance 'ins' attribute">
	
	 	 <t>
	 	 The Resource Instance "ins" attribute is an identifier for this resource, which makes it possible to distinguish from other similar resources. This attribute is similar in use to the "Instance" portion of a DNS-SD record, and SHOULD be unique across resources with the same Resource Type attribute in the domain it is used. A Resource Instance might be a descriptive string like "Ceiling Light, Room 3", a short ID like "AF39" or a unique UUID or iNumber. This attribute is used by a Resource Directory to distinguish between multiple instances of the same resource type within a system.
	 	 </t>
	 	 
	 	 <t>
	 	 This attribute MUST be no more than 63 octets in length. The resource identifier attribute MUST NOT appear more than once in a link description. 
         </t>
	
	</section>
   
	<section title="Export 'exp' attribute">
	
	 	 <t>
	 	 The Export "exp" attribute is used as a flag to indicate that a link description MAY be exported by a resource directory to external directories.  
	 	 </t>
	 	 <t>
	 	 The CoRE Link Format is used for many purposes between CoAP end-points. Some are useful mainly locally, for example checking the observability of a resource before accessing it, determining the size of a resource, or traversing dynamic resource structures. However, other links are very useful to be exported to other directories, for example the entry point resource to a functional service. 
         </t>
	
	</section>   
   
  </section>


  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->

  <section title="Security Considerations">
         <t> 
         This document needs the same security considerations as described in Section 7 of <xref target="RFC5988"/> and Section 6 of <xref target="I-D.ietf-core-link-format"/>. The /.well-known/core resource may be protected e.g. using DTLS when hosted on a CoAP server as described in <xref target="I-D.ietf-core-coap"/>. 
     	 </t>

  </section>

  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->

  <section title="IANA Considerations">

	<t>
	"core-rd" resource type needs to be registered if an appropriate registry is created.
	</t>
	
	<t>
	"ins" and "exp" attributes need to be registered when a future Web Linking attribute is created.
	</t>

     
  </section>

<!------------------------------------------------------>
<!--  SECTION: ACKNOWLEDGMENTS          -->
<!------------------------------------------------------>

<section title="Acknowledgments">

<t>Szymon Sasin, Carsten Bormann, Kerry Lynn, Peter van der Stok, Anders Brandt and Linyi Tian have provided helpful comments, discussions and ideas to improve and shape this document. The authors would also like to thank their collagues from the EU FP7 SENSEI project, where many of the resource directory concepts were originally developed.</t>

</section>

  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->

  <section title="Changelog">

    <t>
    </t>

  </section>

    </middle>

    <back>
    <references title='Normative References'>
       &I-D.ietf-core-link-format;
       &RFC5988;
       
    </references>

    <references title='Informative References'>
		&I-D.ietf-core-coap;
		&I-D.shelby-core-coap-req;
		&I-D.vanderstok-core-bc;
		&I-D.brandt-coap-subnet-discovery;
		&RFC2616;
       
    </references>
    </back>

</rfc>
