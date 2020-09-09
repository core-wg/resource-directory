Russ for Gen-ART
================

Original: https://datatracker.ietf.org/doc/review-ietf-core-resource-directory-25-genart-telechat-housley-2020-07-27/

Tracking issue: https://github.com/core-wg/resource-directory/issues/247


> Section 7.1 says: "... can be transported in the subject."  I think
> you should say "subject field" or "subject name".  Do you mean to
> exclude the subject alternative name?



> Section 7.1.1 says:
> 
>    Registrants that are prepared to pick a different identifier when
>    their initial attempt at registration is unauthorized should pick an
>    identifier at least twice as long as the expected number of
>    registrants; registrants without such a recovery options should pick
>    significantly longer endpoint names (e.g. using UUID URNs [RFC4122]).
> 
> I think that the reason for the  recommendation on length is to reduce
> the likelihood of name collision.  However, it is not clear to me why
> this is linked in any way to authorization failures on the first
> attempt to register.

PR: https://github.com/core-wg/resource-directory/pull/248

> Nits: [...]

addressed: https://github.com/core-wg/resource-directory/pull/246

> IDnits reports:
> 
>  == There are 3 instances of lines with non-ascii characters in the
>     document.

respond:

Two of them are in an author's name, the third is in an example and relevant
there (as it talks about variations of a representation containing non-ascii
charactes).

>  == There are 1 instance of lines with multicast IPv4 addresses in the
>     document.  If these are generic example addresses, they should be
>     changed to use the 233.252.0.x range defined in RFC 5771

respond:

That instance is a suggestion to IANA, it will be replaced with the actually
assigned address.

>  == There are 3 instances of lines with non-RFC3849-compliant IPv6
>     addresses in the document.  If these are example addresses, they
>     should be changed.

respond:

Yes, that's because there's a coverage gap between RFC3849 (defining example
unicast addresses) and RFC3306 (defining unicast-prefix-based multicast
addresses). For lack of an agreed-on example multicast address, the used ones
were created by applying the unicast-prefix derivation process to the example
address. The results (ff35:30:2001:db8::1) should be intuitively recognizable
both as a multicast address and as an example address.

Valery for SecDir
=================

Original: https://datatracker.ietf.org/doc/review-ietf-core-resource-directory-25-secdir-telechat-smyslov-2020-08-09/

Tracking issue: https://github.com/core-wg/resource-directory/issues/247

> The -24 version of this draft was reviewed by Adam Montville. I looked over
> his review and I think that the issue he raised about possible  mitigation of
> DDoS amplification attacks has been addressed in this version. I personally
> think that sentences describing how DNS and NTP are vulnerable to
> amplification attacks are redundant in this document, but that's a matter of
> taste and doesn't hurt.

PR: https://github.com/core-wg/resource-directory/pull/249

> It is my impression, that Security Considerations were mostly written having
> in mind that (D)TLS is always used, however it is only "SHOULD" in this draft
> (or even "MAY" if we look at RFC6690 which Security Considerations this draft
> refers to). I think that adding a few words describing which consequences for
> security not using (D)TLS would have and in which cases it is allowed will
> make the Security Considerations more consistent. 

PR: https://github.com/core-wg/resource-directory/pull/250

Roman Danyliw
=============

Original: https://datatracker.ietf.org/doc/draft-ietf-core-resource-directory/ballot/#roman-danyliw

Tracking issue: @@@

As DISCUSS:

> There appear to be a few areas of straightforward, under-specified elements
> of the authorization model.  
> 
> -- How does the RD know that a node claiming to be a CT is in fact a CT and
> is permitted to register on behalf of end-points?  It seems like there is a
> missing, simple statement to make that this is configured out of band with
> the RD?  Or is that carrier somehow in a authentication credentials?	
> 
> -- Is there are reason why there is not normative guidance requiring the RD
> to check whether authentication clients are authorized to register particular
> resources?  Section 7.1 covers the issue, but all of Section 7.* is
> explicitly noted as informative.  Section 8.1. says “Endpoint authentication
> needs to be checked independently of whether there are configured
> requirements on the credentials for a given endpoint name (Section 7.1) or
> whether arbitrary names are accepted (Section 7.1.1)” but this text seems to
> frame it as authentication issue.  Section 8.2 seems to stress only the
> distinction between the registration and lookup API.
> 
> -- Section 8.1.  Per “If the server does not check whether the identifier
> provided in the DTLS handshake matches the identifier used at the CoAP layer
> then it may be inclined to use the endpoint name for looking up what
> information to provision to the malicious device.”, this is good advice.  If
> DTLS PSK and RPK are used, what identifiers does the RD have to check to
> ensure the DTLS and CoAP layers match?  Per 9.1.3.1. (for PSK) and 9.1.3.2.1
> (for RPK) of RFC7252 there is the notion of identifiers for DTLS but those
> don’t manifest in CoAP?  Additionally, when DTLS with a certificate is used,
> is it intended to compare the subjectAltName with the authority in the
> Registration Base URI (i.e., which exact certificate fields should it compare
> with the CoAP)?

And as COMMENT:

> ** Section 3.5.  Per “When endpoints are not connected … a remote server is
> usually used to provide proxy access to the endpoints”, this architecture
> wasn’t entirely clear to me.  How can a proxy provide access to an endpoint
> that isn’t connected?  Or is proxy meant as a substitute here or an
> intermediary?
> 
> ** Section 3.6.  The home and building automation use case doesn’t make any
> reference to the RD architecture (like the other two use cases).
> 
> ** Section 4.0.  Per “… falling back to failing the operations if recovery is
> not possible”, can “failing the operation” be clarified?
> 
> ** Per Section 4.0.  Per “An RD MAY make the information submitted to it
> available to further directories”, are there circumstances where end points
> would not want that?
> 
> ** Section 4.1.  Per “2. In a network that supports multicast well, …”, what
> does it mean to “support multicast _well_”?
> 
> ** Section 5.  Per the ep definition of the URI Template Variables, what does
> it mean for the an endpoint to be “(mostly mandatory)?
> 
> ** Section 7.1.  Per “When certificates are used as authorization
> credentials, the sector(s) and endpoint name(s) can be transported in the
> subject”, recommend being more precise on what exact X.509 field(s) you mean
> when saying “subject”.
> 
> ** Section 7.1.1. Per “Registrants that are prepared to pick a different
> identifier when their initial attempt at registration is unauthorized should
> pick an identifier at least twice as long as the expected number of
> registrants”, how would a registrant know the population size?
> 
> ** Section 7.2.  Per “To avoid the limitations, RD applications should
> consider prescribe that lookup clients only use the discovered information as
> hints, and describe which pieces of information need to be verified with the
> server”, I wasn’t sure which verification this would be.
> 
> ** Section 7.3.  This section cautions about the differences between the
> registrant publishes itself vs. what is in the RD.  It might be worth
> reiterating that the RD may also publish what it knows to others per Section
> 4.0’s “An RD MAY make the information submitted to it available to further
> directories”
> 
> ** Editorial Nits -- Global.  s/can not/cannot/g
> 
> -- Section 4.  Editorial.  Per “Only multicast discovery operations are not
> possible on HTTP, and Simple Registration can not be executed as base
> attribute … can not be used there”, this sentence didn’t parse.


Benjamin Kaduk
==============

Original: https://datatracker.ietf.org/doc/draft-ietf-core-resource-directory/ballot/#benjamin-kaduk

Tracking issue: @@@

As DISCUSS:

> I agree with Roman that the authorization model seems under-developed.
> While I recognize that there is need for flexibility across various
> deployments, I think that we should be providing a default model (and
> procedures for it) that will apply in many cases, and let
> deployments specify alternate models if needed.  This stuff is hard
> enough to get right that we should have a secure option that people can
> use if they don't need to have customized details.  (To be clear, I
> agree with the change of focus from -24 to -25 on the properties that a
> security policy needs to provide and/or consider, as that is
> fundamentally the important thing.  I just want a fallback/default
> option that "does something reasonable in most cases" in addition.
> Doing that by reference to some other existing thing would be fine, if
> such a thing exists.)
> 
> In particular, the current text seems to rely on the authorization
> model including:
> 
> (1) the RD knowing how clients will be using it (and thus what
> properties the RD needs to enforce), which in the general case cannot be
> known (though for static networks it could be), yet I don't see any
> discussion that indicates this as a prerequisite; and
> 
> (2) the client either knowing out-of-band that an entity is authorized
> to act as a RD or just blindly trusting any of the unauthenticated (*)
> advertisement mechanisms.  (* Yes, there may be some protection in the
> network on subscribing to the relevant multicast address, DNS-SD, etc.,
> but the client cannot a priori know that such protections are in place.)
> 
> Relatedly, the naming model and naming authority should have some
> clearer discussion.  We do mention in Section 7 the possibility for a
> weak naming model where the RD is responsible for enforcing uniqueness
> of names but otherwise link attributes are the primary authorization
> criteria (vs. a traditional scheme with a naming authority and naming
> hierarchy), but with naming as a fundamental prerequisite of any
> authentication/authorization scheme, I think clearer discussion of how a
> naming model is to be selected (and, perhaps more importantly, that it
> must be fixed as part of a given deployment) for a given network is
> needed.
> 
> If I understand correctly, we have some codepoint squatting going on in
> the examples (e.g., for resource types).
> 
> We should talk about the security properties of the various RD discovery
> mechanisms that are defined.

As COMMENT:

> My apologies for where these comments diverge off into rambling
> incoherency, or where I'm misunderstanding something that's clearly laid
> out; this document had the misfortune of being the last one I got to
> this week.
> 
> Section 1
> 
>    [RFC6690] only describes how to discover resources from the web
>    server that hosts them by querying "/.well-known/core".  In many
>    constrained scenarios, direct discovery of resources is not practical
>    due to sleeping nodes, disperse networks, or networks where multicast
>    traffic is inefficient.  These problems can be solved by employing an
>    entity called a Resource Directory (RD), which contains information
>    about resources held on other servers, allowing lookups to be
>    performed for those resources.
> 
> nit(?): I'd consider specifying that the RD is "a trusted entity".
> (Even when the resources themselves are authenticated, a hostile RD can
> still deny existence of a given resource, so by choosing to use an RD
> there is some level of trust involved.)
> 
> Section 2
> 
>    Resource Directory (RD)
>       A web entity that stores information about web resources and
>       implements the REST interfaces defined in this specification for
>       discovery, for the creation, the maintenance and the removal of
>       registrations, and for lookup of the registered resources.
> 
> nit: the list structure is not parallel here.  Maybe "for discovery,
> creation, maintenance, and removal of registrations, and for lookup of
> the registered resources"?
> 
>    Commissioning Tool
>       Commissioning Tool (CT) is a device that assists during the
>       installation of the network by assigning values to parameters,
>       naming endpoints and groups, or adapting the installation to the
>       needs of the applications.
> 
> Is "the installation of the network" a one-time event?   (Might a CT be
> involved when adding a new device to a network at a later time?)
> 
> Section 3.1
> 
>    Information SHOULD only be stored in the RD if it can be obtained by
>    querying the described device's /.well-known/core resource directly.
> 
> When might that not be the case?
> 
> Section 3.2
> 
>    The RD architecture is illustrated in Figure 1.  An RD is used as a
>    repository of registrations describing resources hosted on other web
>    servers, also called endpoints (EP).  An endpoint is a web server
>    associated with a scheme, IP address and port.  A physical node may
> 
> (side note) hmm, I feel like in the HTTP world an endpoint is more
> likely to be associated with a DNS name than an IP address, in common
> usage.  Also, we later go on to assert that the endpoint's name has
> primacy and that the IP address/port can be ephemeral.
> 
>    An endpoint uses specific interfaces to register, update and remove a
>    registration.  It is also possible for an RD to fetch Web Links from
>    endpoints and add their contents to its registrations.
> 
>    At the first registration of an endpoint, a "registration resource"
>    is created, the location of which is returned to the registering
>    endpoint.  The registering endpoint uses this registration resource
>    to manage the contents of registrations.
> 
> Does the "RD fetches links unilaterally" case count as a "first
> registration of an endpoint"?  I'm having a hard time seeing how these
> two statements are consistent with each other, and a naive reading
> admits the possibility that a given endpoint could be "locked out" of
> the ability to manage the contents of its registrations.
> 
> Section 4
> 
>    REST clients (registrant-EPs and CTs during registration and
>    maintenance, lookup clients, RD servers during simple registrations)
>    MUST be prepared to receive any unsuccessful code and act upon it
>    according to its definition, options and/or payload to the best of
>    their capabilities, falling back to failing the operation if recovery
>    is not possible.  In particular, they should retry the request upon
> 
> "MUST be prepared [...] to the best of their abilities" seems
> non-actionable.  The stuff after "In particular", on the other hand, is
> actual concrete guidance that could be mandated using normative
> language.
> 
> Section 4.1
> 
>    1.  In a 6LoWPAN, just assume the Border Router (6LBR) can act as an
>        RD (using the ABRO option to find that [RFC6775]).  Confirmation
>        can be obtained by sending a Unicast to "coap://[6LBR]/.well-
>        known/core?rt=core.rd*".
> 
> nit(?): I was unaware that "Unicast" was a proper noun.
> 
> Section 4.3
> 
>    "core.rd" in the query string.  Likewise, a Resource Type parameter
>    value of "core.rd-lookup*" is used to discover the URIs for RD Lookup
>    operations, core.rd* is used to discover all URI paths for RD
>    operations.  [...]
> 
> Is the distinction between URIs (for RD Lookup) and URI paths (for RD)
> important here?
> 
>    While the link targets in this discovery step are often expressed in
>    path-absolute form, this is not a requirement.  Clients of the RD
>    SHOULD therefore accept URIs of all schemes they support, both as
>    URIs and relative references, and not limit the set of discovered
>    URIs to those hosted at the address used for URI discovery.
> 
> I'm not sure I see how the "not limit [...] to those hosted at the
> address used for URI discovery" follows from the non-requirement for
> expression of the link-targets from discovery in path-absolute form.
> (Given the ability to send the discovery query to a multicast address,
> the guidance seems okay; it's just the "therefore" that is puzzling me.)
> 
>    It would typically be stored in an implementation information link
>    (as described in [I-D.bormann-t2trg-rel-impl]):
> 
>    Req: GET /.well-known/core?rel=impl-info
> 
> This seems to be depicting a link-relation type that is not registered
> at https://www.iana.org/assignments/link-relations/link-relations.xhtml
> , i.e., codepoint squatting.  Please put in a stronger disclaimer that
> this is an example link relation type, not just an example exchange.
> 
> Section 5
> 
> These first few paragraphs give the impression that this is
> first-come-first-served with minimal authentication or authorization
> checking.  Mentioning that there are authorization checks, with a
> forward-reference, might be helpful.
> 
>    further parameters (see Section 9.3).  The RD then creates a new
>    registration resource in the RD and returns its location.  The
> 
> Is this returned "registration resource" expected to function as a
> "capability URL" (https://www.w3.org/TR/capability-urls/) that would
> need to contain an appropriate amount of entropy to be reasonably
> unguessable by parties other than the registrant-ep/CT responsible for
> it?
> 
>    The registration request interface is specified as follows:
> 
>    Interaction:  EP -> RD
> 
> I thought that the CT could be a requestor as well as the EP.
> 
>          well.  The endpoint name and sector name are not set when one
>          or both are set in an accompanying authorization token.
> 
> What should the RD do if they are set but also present in the
> accompanying authorization token?
> 
>    Req: POST coap://rd.example.com/rd?ep=node1
>    Content-Format: 40
>    Payload:
>    </sensors/temp>;ct=41;rt="temperature-c";if="sensor",
> 
> (side note) XML for the sensors, not SenML?  With Carsten as an author,
> even? ;)
> 
>    An RD may optionally support HTTP.  Here is an example of almost the
>    same registration operation above, when done using HTTP.
> 
>    Req:
>    POST /rd?ep=node1&base=http://[2001:db8:1::1] HTTP/1.1
>    Host: example.com
> 
> Wouldn't "Host: rd.example.com" be closer to "almost the same
> registration"?
> 
> Section 5.1
> 
> I'm a little uneasy about specifying new behavior for POST to the
> existin /.well-known/core that was defined by RFC 6690 for other uses.
> What factors go into using the same well-known URI vs. defining a new
> one for this usage?
> 
>    The sequence of fetching the registration content before sending a
>    successful response was chosen to make responses reliable, and the
>    caching item was chosen to still allow very constrained registrants.
> 
> I'm not sure what "the caching item" is supposed to be (if it's not a
> typo/misordering of words).
> 
> Section 5.3
> 
>    queries concerning this endpoint.  The RD SHOULD continue to provide
>    access to the Registration Resource after a registration time-out
>    occurs in order to enable the registering endpoint to eventually
>    refresh the registration.  The RD MAY eventually remove the
>    registration resource for the purpose of garbage collection.  If the
>    Registration Resource is removed, the corresponding endpoint will
>    need to be re-registered.
> 
> (This MAY is actually a MUST for the simple registration case, per §5.1,
> right?)
> 
> Section 5.3.1
> 
>    An update MAY update the lifetime or the base URI registration
>    parameters "lt", "base" as in Section 5.  Parameters that are not
> 
> What about the "extra-attrs"; are they inherently forbidden from
> updates?
> 
>                             base :=  Base URI (optional).  This
>          parameter updates the Base URI established in the original
>          registration to a new value.  If the parameter is set in an
>          update, it is stored by the RD as the new Base URI under which
>          to interpret the relative links present in the payload of the
>          original registration, following the same restrictions as in
>          the registration.  If the parameter is not set in the request
> 
> nit: is it the interpretation of relative links that is following the
> same restrictions as in the registration, or the new value of the
> parameter being supplied in the update?
> 
>    The following example shows how the registering endpoint updates its
>    registration resource at an RD using this interface with the example
>    location value: /rd/4521.
> 
> The path component "4521" contains a worryingly small amount of
> unpredictableness; I would prefer examples that used longer random
> locations, as for capability URLs.  (Throughout the document, of
> course.)  See also draft-gont-numeric-ids-sec-considerations, that I'm
> AD sponsoring, though I do not see any clear issues on first glance.
> 
> (Also, it might be worth another sentence that this update is serving
> just to reset the lifetime, making no other changes, since this might be
> expected to be a common usage.)
> 
> Section 6
> 
> With "Resource Lookup" and "Endpoint Lookup" as (apparent) top-level
> siblings, would it make sense to put 6.2, or at least 6.3, as
> subsections under 6.1?
> 
> Section 6.1
> 
>    Resource lookup results in links that are semantically equivalent to
>    the links submitted to the RD.  The links and link parameters
>    returned by the lookup are equal to the submitted ones, except that
>    the target and anchor references are fully resolved.
> 
> Are the "submitted ones" the submissions at registration time, or during
> the lookup query itself?  (I assume registration-time, but being
> explicit costs little.)
> 
>    If the base URI of a registration contains a link-local address, the
>    RD MUST NOT show its links unless the lookup was made from the same
>    link.  The RD MUST NOT include zone identifiers in the resolved URIs.
> 
> Same link as what?
> 
> Section 6.2
> 
>    The page and count parameters are used to obtain lookup results in
>    specified increments using pagination, where count specifies how many
> 
> (We haven't introduced the page and count parameters yet.)
> 
>    operator as in Section 4.1 of [RFC6690].  Attributes that are defined
>    as "link-type" match if the search value matches any of their values
> 
> Where is it specified how an attribute might be "defined as
> 'link-type'"?  This is the only instance of the string "link-type" in
> this document, and it does not appear in RFC 6690 at all...
> 
>    references) and are matched against a resolved link target.  Queries
>    for endpoints SHOULD be expressed in path-absolute form if possible
>    and MUST be expressed in URI form otherwise; the RD SHOULD recognize
>    either.  The "anchor" attribute is usable for resource lookups, and,
>    if queried, MUST be for in URI form as well.
> 
> I don't see how it can be only a SHOULD to recognize either given these
> generation criteria.
> 
> Section 6.3
> 
>    The following example shows a client performing a lookup of all
>    resources of all endpoints of a given endpoint type.  It assumes that
>    two endpoints (with endpoint names "sensor1" and "sensor2") have
>    previously registered with their respective addresses
>    "coap://sensor1.example.com" and "coap://sensor2.example.com", and
>    posted the very payload of the 6th request of section 5 of [RFC6690].
> 
> Er, the 6th request is a GET; do we mean to say the response to the 6th
> request?
> 
> Section 6.4
> 
>    The endpoint lookup returns registration resources which can only be
>    manipulated by the registering endpoint.
> 
> This seems to leave it unclear whether the endpoint lookup is expected
> to return resources that the requestor will not have permission to
> manipulate (in addition to those it does have permission for).
> 
>    While Endpoint Lookup does expose the registration resources, the RD
>    does not need to make them accessible to clients.  Clients SHOULD NOT
>    attempt to dereference or manipulate them.
> 
> But why expose them at all if they're not going to be accessible?
> 
>    An RD can report endpoints in lookup that are not hosted at the same
>    address.  [...]
> 
> The "same address" as what?
> 
> Section 7.1
> 
>    Whenever an RD needs to provide trustworthy results to clients doing
>    endpoint lookup, or resource lookup with filtering on the endpoint
> 
> How will the RD know whether the client is expecting trustworthy
> results?  (When would a client *not* expect trustworthy results?)
> 
>    name, the RD must ensure that the registrant is authorized to use the
>    given endpoint name.  This applies both to registration and later to
>    operations on the registration resource.  It is immaterial there
>    whether the client is the registrant-ep itself or a CT is doing the
>    registration: The RD can not tell the difference, and CTs may use
> 
> I suppose there might be plausible authorization models where a
> return-routability check to a given address constitutes authorization to
> use that address as an endpoint name, in which case the RD can tell the
> difference between a registrant-ep and a CT attempting to act on its
> behalf.
> 
>    When certificates are used as authorization credentials, the
>    sector(s) and endpoint name(s) can be transported in the subject.  In
>    an ACE context, those are typically transported in a scope claim.
> 
> As Russ noted in the Gen-ART review, "transported in the subject" is
> sufficiently vague to not really be actionable.  It might be better to
> say that the holder of the private key corresponding to the public key
> certified in the certificate is generally considered authorized to act
> on behalf of any identities (including endpoint names) contained in the
> certificate's subject name.
> 
> Section 7.1.1
> 
>    Conversely, in applications where the RD does not check the endpoint
>    name, the authorized registering endpoint can generate a random
>    number (or string) that identifies the endpoint.  The RD should then
> 
> How much entropy/randomness in the random name?  Does a CSPRNG need to
> be used?  (I do see the follow-up about doubling the length in case of
> failure or starting with a UUID if that's not possible, but some
> guidance on where to start still seems appropriate.)
> 
> Section 7.2
> 
>    When lookup clients expect that certain types of links can only
>    originate from certain endpoints, then the RD needs to apply
>    filtering to the links an endpoint may register.
> 
> As before, how will the RD know what behavior clients are relying on?
> 
>    An RD may also require that only links are registered on whose anchor
>    (or even target) the RD recognizes as authoritative of.  One way to
> 
> I don't think I can parse this sentence (especially "the RD recognizes
> as authoritative of").
> 
> Section 8
> 
> In contexts where we discuss DTLS and TLS as being generally comparable,
> we typically will state that DTLS replay protection is required in order
> to provide equivalent levels of protection.
> 
> We might also want to reiterate or refer back to the previous discussion
> of the potential for attributes or resource/endpoint names, link
> relations, etc. that may need to be confidential, the relevant access
> control/filtering, and the avenues by which disclosure of resource names
> can occur even when access to those resources will not be permitted.  (I
> think some of this overlaps with 8288 and 6690, but don't mind repeating
> it.)
> 
> Section 8.1
> 
> It's probably worth reiterating that all name comparisons must be done
> at sector scope (since failing to do so can lead to attacks).
> 
>    Endpoint authentication needs to be checked independently of whether
>    there are configured requirements on the credentials for a given
>    endpoint name (Section 7.1) or whether arbitrary names are accepted
>    (Section 7.1.1).
> 
> I think this is more properly authorization than authentication.
> 
> Section 8.3
> 
>    attacks.  There is also a danger that NTP Servers could become
>    implicated in denial-of-service (DoS) attacks since they run on
>    unprotected UDP, there is no return routability check, and they can
>    have a large amplification factor.  The responses from the NTP server
>    were found to be 19 times larger than the request.  An RD which
> 
> (It's not clear to me why the specific discussion of NTP numbers is
> relevant here, since RD is not NTP.)
> 
> Section 9.3
> 
> Should we also include "rt" in the initial entries?  I see it is used as
> a query parameter for resource lookup in the examples in Section 6.3.
> 
>    *  indication of whether it can be passed as a query parameter at
>       registration of endpoints, as a query parameter in lookups, or be
>       expressed as a target attribute,
> 
> (Since this text does not clarify about lookup of endpoints vs.
> resources...
> 
>    Review" as described in [RFC8126].  The evaluation should consider
>    formal criteria, duplication of functionality (Is the new entry
>    redundant with an existing one?), topical suitability (E.g. is the
>    described property actually a property of the endpoint and not a
>    property of a particular resource, in which case it should go into
>    the payload of the registration and need not be registered?), and the
> 
> ... and this text suggests that query parameters for *resource* lookups
> need not be registered.)
> 
>    potential for conflict with commonly used target attributes (For
>    example, "if" could be used as a parameter for conditional
>    registration if it were not to be used in lookup or attributes, but
>    would make a bad parameter for lookup, because a resource lookup with
>    an "if" query parameter could ambiguously filter by the registered
>    endpoint property or the [RFC6690] target attribute).
> 
> Then why do we use it as an example of lookup filtering in Section 6.2?
> 
> Section 10.1.2
> 
> Should we really be using unregistered resource types (i.e., codepoint
> squatting) in the examples?
> 
>    After the filling of the RD by the CT, the application in the
>    luminaries can learn to which groups they belong, and enable their
>    interface for the multicast address.
> 
> Just to check: the luminaries are learning their own group membership by
> querying the resource directory?
> 
> Section 10.2.2
> 
> Please expand MSISDN.
> 
> Section 13.2
> 
> I think RFC 7252 should probably be normative.
> 
> Likewise for RFC 8288 ("the query parameter MUST be [...] a token as
> used in [RFC8288]").

Erik Kline
==========

Original: https://datatracker.ietf.org/doc/draft-ietf-core-resource-directory/ballot/#erik-kline

Tracking issue: @@@

As DISCUSS:

> [ section 4.1.1 ]
> 
> * Did this get presented to 6man at any point, either via mail to the list or
>   chair or in a presentation slot at an IETF meeting or a 6man interim?
> 
>   I feel confident that there would be no objection to the option as described
>   here, but the working group should have its chance to make an evaluation
>   irrespective of my opinion.
> 
>   ---
> 
>   If this is to be used when link-local methods don't work, another option
>   would have been to add an RD PVD API key and recommend including a PVD
>   option.
> 
> [ section 4.1.1 & 9.2 ]
> 
> * Please clarify which ND messages can carry an RDAO.  I suspect they should
>   only appear in RAs, but it would be good to state the expectation explicitly.
> 
> [ Appendix A. ]
> 
> * Can you explain the ff35:30:2001:db8:1 construction?  RFC 3306 section 4
>   defines some fine-grained structure, and I'm wondering how a group ID of 1
>   is selected/computed/well known.  If there is already a COAP document
>   describing this vis. RFC 3307 section 4.*, perhaps it's worth dropping a
>   reference in here.

As COMMENT:

> [ section 1 ]
> 
> * I'm unclear on what "disperse networks" might mean.
> 
> [ section 10.1.1 ]
> 
> * What is meant by "therefore SLAAC addresses are assigned..." followed by this
>   table of not-very-random-looking IPv6 addresses?
> 
>   Is the assumption that there might not be some off-network DNS server but
>   there is some RA with a /64 A=1 PIO?

Éric Vyncke
===========

Original: https://datatracker.ietf.org/doc/draft-ietf-core-resource-directory/ballot/#eric-vyncke

Tracking issue: @@@

As DISCUSS:

> Thank you for the work put into this document. I am little puzzled by the
> document shepherd's write-up dated more than one year ago (the responsible AD
> has even changed and the change is not reflected in the write-up)... while
> well-written this write-up seems to indicate neither a large consensus nor a
> deep interest by the CORE WG community. But, I am trusting the past and
> current responsible ADs on this aspect.
> 
> Did the authors check with 6MAN WG about the new RDAO option for IPv6 NDP ? I
> was unable to find any 6MAN email related to this new NDP option and, after
> checking with the 6MAN WG chairs, they also do not remember any discussion.
> 
> BTW, I appreciated the use of ASCII art to represent an entity-relationship
> diagram !
> 
> Please find below a couple of non-blocking COMMENTs (and I would appreciate a
> reply to each of my COMMENTs) and 2 blocking DISCUSS points (but only trivial
> actions/fixes are required).
> 
> I hope that this helps to improve the document,
> 
> Regards,
> 
> -éric
> 
> == DISCUSS ==
> 
> -- Section 4.1 -- It will be trivial to fix, in IPv6 address configuration
> (SLAAC vs. DHCP) is orthogonal to DHCP 'other-information'. E.g., even if
> address is configured via SLAAC, DHCPv6 other-information can be used to
> configure the Recursive DNS Server (or possibly the RD).
> 
> -- Section 4.1.1 -- Another trivial DISCUSS to fix: in which message is this
> RDAO sent ? I guess unicast Router Advertisement but this MUST be specified.

As COMMENT:

> == COMMENTS ==
> 
> In general, I wonder how much interactions and exchanges of ideas have
> happened in the long history of this document with the DNSSD (DNS Service
> Discovery) Working Group that has very similar constraints (sleeping nodes)
> and same objectives.
> 
> -- Section 2 -- To be honest: I am not too much an APP person; therefore, I
> was surprised to see "source address (URI)" used to identify the "anchor="...
> I do not mind too much the use of "destination address (URI)" as it is really
> a destination but the anchor does not appear to me as a "source address". Is
> it common terminology ? If so, then ignore my COMMENT, else I suggest to
> change to "destination URI" and simply "anchor" ?
> 
> -- Section 3.3 -- Should the lifetime be specified in seconds at first use in
> the text?
> 
> -- Section 3.6 -- Is the use of "M2M" still current? I would suggest to use
> the word "IoT" esp when 6LBR (assuming it is 6LO Border Router) is cited
> later.
> 
> Please expand and add reference for 6LBR.
> 
> Using 'modern' technologies (cfr LP-WAN WG) could also add justification to
> section 3.5.
> 
> -- Section 4.1 -- About "coap://[MCD1]/.well-known/core?rt=core.rd*", what is
> the value of MCD1 ? The IANA section discuss about it but it may help the
> reader to give a hint before (or simply use TBDx that is common in I-D).
> 
> Any reason to use "address" rather than "group" in "When answering a
> multicast request directed at a link-local address" ?
> 
> Later "to use one of their own routable addresses for registration." but
> there can be multiple configured prefixes... Which one should the RD select ?
> Should this be specified ?
> 
> As a co-author of RFC 8801, I would have appreciated to read PvD option
> mentionned to discover the RD. Any reason why PvD Option cannot be used ?
> 
> -- Section 4.1.1 -- I suggest to swap the reserved and lifetime fields in
> order to be able to use a lifetime in units of seconds (to be consistent with
> other NDP options).
> 
> -- Section 5 -- May be I missed it, but, can an end-point register multiple
> base URI ? E.g., multiple IPv6 addresses.
> 
> -- Section 9.2 -- For information, value 38 is already assigned to RFC 8781.
> 
> 
> 
> == NITS ==
> 
> -- Section 2 -- The extra new lines when defining "Sector" are slighly
> confusing. Same applies to "Target" and "Context". This is cosmetic only.

Martin Duke
===========

Original: https://datatracker.ietf.org/doc/draft-ietf-core-resource-directory/ballot/#martin-duke

Tracking issue: @@@

As COMMENT:

> One nit: the sentence that contains “cannot be executed as a base attribute”
> appears to have been mangled.

Murray Kucherawy
================

Original: https://datatracker.ietf.org/doc/draft-ietf-core-resource-directory/ballot/#murray-kucherawy

Tracking issue: @@@

As COMMENT:

> In Section 9.2, you might want to mention that you're talking about a
> sub-registry under "Internet Control Message Protocol version 6 (ICMPv6)
> Parameters".
> 
> In Section 9.3, you enumerate six fields in each registration, but the
> initial table of entries has only five columns.  It's obvious (I think) that
> the sixth column would be "this document" for all entries, but I suggest that
> you should either include the column or some prose making this explicit
> (since everything else is).

Warren Kumari
=============

Original: https://datatracker.ietf.org/doc/draft-ietf-core-resource-directory/ballot/#warren-kumari

Tracking issue: @@@

As COMMENT:

> Comments:
>
> 1: "These CTs are thought to act on behalf of endpoints too constrained, or
> generally unable, to present that information themselves. "
>
> This reads very oddly - "thought to act on" sounds like we've seen some of
> these in the wild, and only have a vague idea about how they work. Does
> "These CTs act on behalf of endpoints too constrained, or generally unable,
> to present that information themselves. " work?
> 
> 2: "From the system design point of view, the ambition is to design
> horizontal solutions that  can enable utilization of machines in different
> applications depending on their current availability and capabilities as well
> as application requirements, thus avoiding silo like solutions" - this is
> very buzzwordy, and I have no idea what it is actually trying to say...
> 
> 3: "  A (re-)starting device may want to find one or more RDs for discovery
> purposes."
>
> Either I don't understand what this sentence is  trying to say, or "for
> discovery purposes" should be dropped.... 
> 
> 4: "As some of the RD addresses obtained by the methods listed here are just
> (more or less educated) guesses, endpoints MUST make use of any error
> messages to very strictly rate-limit requests to candidate IP addresses that
> don't work out. " What happens if device A discovers RD X, and device B
> discovers RD Y? Surely there has to be some sort of deterministic method so
> that one doesn't end up in a "split brain" type outcome? 
> 
> 
> Nits:

> 1: " The input to an RD is composed of links and the output is composed of
> links constructed from the information stored in the RD." While  true, this
> sentence doesn't actually communicate anything useful to the reader -- I'd
> suggest removing it from the Abstract (note that this is just a nit).
> 
> 2: "The RD is primarily a tool to make discovery operations more efficient
> than querying /.well-known/core on all connected devices, or across
> boundaries that would be limiting those operations."
>
> s/would be limiting those/that would limit those/

Robert Wilton
=============

Original: https://datatracker.ietf.org/doc/draft-ietf-core-resource-directory/ballot/#robert-wilton

Tracking issue: @@@

As COMMENT:

> I'm glad that the shepherd's writeup indicates that there are implementations
> since that indicates that there does appear to be value in standardizing this
> work, despite its long journey.
> 
> My main comment (which I was considering raising as a discuss) is:
> 
> Since this document is defining a new service, I think that this document
> would benefit on having a short later section on "Operations, Administration
> and Management".  This document does seem to cover some
> management/administration considerations in various sections in a somewhat ad
> hoc way, but having a section highlighting those would potentially make it
> easier deploy.  Defining a common management model (or API) would potentially
> also make administration of the service easier, I don't know if that had been
> considered, and if useful could be done as a separate document.
> 
> A few other comments:
> 
>     5.3.  Operations on the Registration Resource
> 
>        An endpoint should not use this interface for registrations that it
>        did not create.  This is usually enforced by security policies, which
>        in general require equivalent credentials for creation of and
>        operations on a registration.
> 
> What happens if an endpoint is managing the registration and is upgraded to
> new hardware with a different certificate?  Would the updated endpoint expect
> to be able to update the registration?  Or would it have to wait for the
> existing registration to timeout (which could be a long time)?
> 
>     5.3.  Operations on the Registration Resource
> 
>        The Registration Resource may also be used cancel the registration
>        using DELETE, and to perform further operations beyond the scope of
>        this specification.
> 
>        These operations are described below.
>    
> Nit: Perhaps reword the second sentence.  Otherwise it seems to conflict with
> the last sentence of the prior paragraph.
> 
>     5.3.1.  Registration Update
> 
>        The update interface is used by the registering endpoint to refresh
>        or update its registration with an RD.  To use the interface, the
>        registering endpoint sends a POST request to the registration
>        resource returned by the initial registration operation.
> 
>        An update MAY update the lifetime or the base URI registration
>        parameters "lt", "base" as in Section 5.  Parameters that are not
>        being changed SHOULD NOT be included in an update.  Adding parameters
>        that have not changed increases the size of the message but does not
>        have any other implications.  Parameters MUST be included as query
>        parameters in an update operation as in Section 5.
> 
> The "SHOULD NOT" feels a bit strong to me, and I would prefer to see this as
> "MAY NOT".  In many cases, if the configuration is not too big then providing
> the full configuration makes it easy to guarantee that the receiver has
> exactly the correct configuration.  I appreciate that there are many cases
> where from an endpoint perspective it may want to keep the update small, but
> if I was doing this from a CT, I think that I would rather just resend the
> entire configuration, if it is not large.
> 
>     5.3.1.  Registration Update
> 
>        Req: GET /rd-lookup/res?ep=endpoint1
> 
>        Res: 2.05 Content
>        Payload:
>        <coap://local-proxy-old.example.com:5683/sensors/temp>;ct=41;
>            rt="temperature-c";if="sensor";
>            anchor="coap://local-proxy-old.example.com:5683/",
>        <http://www.example.com/sensors/temp>;
>            anchor="coap://local-proxy-old.example.com:5683/sensors/temp";
>            rel="describedby"
> 
>            Figure 14: Example lookup before a change to the base address
> 
> Just to check, is it correct that the anchor in the http link is also to
> coap://?  If this is wrong then there is a second example in the same section
> that also needs to be fixed.
