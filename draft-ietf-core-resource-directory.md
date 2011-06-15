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
<rfc category="std" ipr="trust200902" docName="draft-shelby-core-resource-directory-00pre">

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


  <date year="2011"/>

  <area>Internet</area>

  <workgroup>CoRE</workgroup>
  <keyword>CoRE, Web Linking, Resource Discovery, Resource Directory</keyword>

    <abstract>
    <t>
	In many M2M scenarios, direct discovery of resources is not practical due to sleeping nodes, disperse networks, or networks where multicast traffic is inefficient. These problems can be solved by employing an entity called a Resource Directory (RD), which hosts descriptions of resources held on other servers, allowing lookups to be performed for those resources. This document specifies the web interface that a Resource Directory supports in order for web servers to discover the RD and to registrer, maintain and remove resources descriptions. Furthermore, new link attributes useful in conjunction with a Resource Directory are defined. 
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
  The Constrained RESTful Environments (CoRE) working group aims at realizing the REST architecture in a suitable form for the most constrained nodes (e.g. 8-bit microcontrollers with limited RAM and ROM) and networks (e.g. 6LoWPAN). CoRE is aimed at machine-to-machine (M2M) applications such as smart energy and building automation <xref target="I-D.shelby-core-coap-req"/>.
  </t>
  <t>
  The discovery of resources offered by a constrained server is very important in machine-to-machine applications where there are no humans in the loop and static interfaces result in fragility. The discovery of resources provided by an HTTP Web Server is typically called Web Linking. The use of Web Linking for the description and discovery of resources hosted by constrained web servers is specified by the CoRE Link Format <xref target="I-D.ietf-core-link-format"/>. This specification however only describes how to discover resources from the web server that hosts them by requesting /.well-known/core. In many M2M scenarios, direct discovery of resources is not practical due to sleeping nodes, disperse networks, or networks where multicast traffic is inefficient. These problems can be solved by employing an entity called a Resource Directory (RD), which hosts descriptions of resources held on other servers, allowing lookups to be performed for those resources.
  </t>
  <t>
  This document specifies the web interface that a Resource Directory supports in order for web servers to discover the RD and to registrer, maintain and remove resources descriptions. Furthermore, new link attributes useful in conjunction with a Resource Directory are defined. Although the examples in this document show the use of these interfaces with CoAP <xref target="I-D.ietf-core-coap"/>, they may be applied in an equivalent manner to HTTP <xref target="RFC2616"/>.
  </t>

  </section>
  

  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <section anchor='arch' title="Architecture and Use Cases">

	<t>
	Introduction
	</t>


		<figure>
          <artwork align="left"><![CDATA[

                        +------+ 
  +----+                |      |
  | EP | ---------------|  RD  |
  +----+                |      |
                        +------+

            ]]></artwork>
        </figure>

	<t>
	Use cases: Home automation, building automation, Cellular M2M
	</t>


  </section>

  

  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <!-- **************************************************************** -->
  <section anchor='rd' title="Resource Directory Interface">

	<t>
	This section defines the REST interfaces between a Resource Directory and end-points. Although the examples throughout this section assume use of CoAP    
	<xref target="I-D.ietf-core-coap"/>, these REST interfaces can also be realized using HTTP <xref target="RFC2616"/>. An RD implementing this specification MUST support the discovery, registration, update and removal inerfaces defined in this section. 
	</t>

	  <section anchor='discovery' title="Discovery">
	
		<t>
		Before an end-point can make use of an RD, it must first know its location and optionally the path of the RD root resource. There can be several mechanisms for discovering the RD including assuming a default location (e.g. on an Edge Router in a LoWPAN), by assigning an anycast address to the RD, using DHCP, or by discovering the RD using the CoRE Link Format. This section describes defines discovery of the RD using the well-known interface of the CoRE Link Format <xref target="I-D.ietf-core-link-format"/> as the required mechanism. It is however expected that RDs will also be discoverable other methods depending on the deployment. 
		</t>
		
		<t>
		Discovery is performed by sending either a multicast or unicast GET request to /.well-known/core and including a resource type parameter <xref target="I-D.ietf-core-link-format"/> with the value "core-rd" in the query string. Upon success, the response will contain a payload with a link format entry for each RD discovered, with the URL indicating the root resource of the RD. When performing multicast discovery, the multicast IP address used will depend on the scope required and the multicast capabilities of the network.
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
          	</list>
          </t>
          <t hangText="Success:"> 2.05 "Content" with an application/link-format payload containing a matching entry for the RD resource.</t>
          <t hangText="Failure:"> 2.05 "Content" (should be a "No Content" code?) with an empty payload is returned in case no matching entry is found for a unicast request.</t>
          <t hangText="Failure:"> No error response to a multicast request.</t>
          <t hangText="Failure:"> 4.00 "Bad Request" </t>
        </list>
       	</t>

		<t>
		The following example shows an end-point discovering an RD using this interface. 
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
</rd>; rt="core-rd" 
            ]]></artwork>
        </figure>
		
	
	  </section>
	  
	
	  <section anchor='registration' title="Registration">

		<t>
		After discovering the location of an RD, an end-point MAY register its resources to the RD's registration interface. This interface accepts a POST from an end-point containing the list of resources to be added to the directory as the message payload in the CoRE Link Format along with query string parameters indicating the name of the end-point, an optional node identifier and the lifetime of the registration. The RD then creates a new resource or updates an existing resource in the RD and returns its location. An end-point MUST use the same end-point name when refreshing registrations using this interface. End-point resource in the RD are kept active for the period indicated by the lifetime parameter. The end-point is reponsible for refreshing the entry within this period using either the registration or update interface. 
		
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
          		<t hangText="End-point Name (n):">Name of the end-point, MUST be unique within this domain. If no end-point name is included, the RD SHOULD generate a unique name for the end-point. The maximum length of this parameter is 63 octets.</t>
          		<t hangText="Node Identifier (id):">An optional unique identifier for the node hosting the end-point (e.g. a serial number, EUI-64, UUID or iNumber).</t>
          		<t hangText="Lifetime (lt):">Lifetime of the registration in seconds. Range of 60-4294967295. If no lifetime is included, a default value of 86400 (24 hours) SHOULD be assumed.</t>
          	</list>
          </t>
          <t hangText="Success:"> 2.01 "Created". The Location header of the new resource entry for the end-point SHOULD be in the form /{rd-base}/{end-point name}</t>
          <t hangText="Failure:"> 4.00 "Bad Request" </t>
        </list>
       	</t>

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
Req: POST coap://rd.example.org/rd?n=node1&lt=1024
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
          <t hangText="Path:">/{rd-base}/{end-point name}</t>
          <t hangText="Method:">PUT</t>	
          <t hangText="Content-Type:">application/link-format (if any)</t>
          <t hangText="Etag:">The Etag option MUST be included to allow an RD to compare the existing entry and perform validation in the future.</t>
          <t hangText="Parameters:"> 
          	<list>
          		<t hangText="Lifetime (lt):">Lifetime of the registration in seconds. Range of 60-4294967295. If no lifetime is included, a default value of 86400 (24 hours) SHOULD be assumed.</t>
          	</list>
          </t>
          <t hangText="Success:"> 2.04 "Changed" in case the resource and/or lifetime was successfully updated</t>
          <t hangText="Failure:"> 4.00 "Bad Request" </t>
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
Req: PUT coap://{RD}/rd/{end-point name}
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
          <t hangText="Failure:"> 4.00 "Bad Request" </t>
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
          <t hangText="Path:">/{rd-base}/{end-point name}</t>
          <t hangText="Method:">DELETE</t>	
          <t hangText="Content-Type:">None</t>
          <t hangText="Parameters:"> None</t>
          <t hangText="Success:"> 2.02 "Deleted" upon successful deletion</t>
          <t hangText="Failure:"> 4.00 "Bad Request" </t>
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
Req: DELETE coap://{RD}/rd/node1
		
Res: 2.02 Deleted 
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
	 	 The Resource Instance "ins" attribute is an identifier for this resource, which makes it possible to distinguish from other similar resources. This attribute is similar in use to the "Instance" portion of a DNS-SD record [REF], and SHOULD be unique across resources with the same Resource Type attribute in the domain it is used. A Resource Instance might be a descriptive string like "Ceiling Light, Room 3", a short ID like "AF39" or a unique UUID [REF] or iNumber [REF]. This attribute is used by a Resource Directory to distinguish between multiple instances of the same resource type within a system.
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

<t>Szymon Sasin, Carsten Bormann, Kerry Lynn, Peter van der Stok, Anders Brandt and Linyi Tian.</t>

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
		&RFC2616;
       
    </references>
    </back>

</rfc>
