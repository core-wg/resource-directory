Lines 1440ff are unclassified

Not all points get invidiual issues, most are to be eventually responded to and
/ or resolved in PRs.

Points are classified by priority for WG discussion as WGF-9 (WG input is
crucial) to WGF-0 (any editor's fix can do this without further discussion).
Points with open issues, PRs or even resolved in merged PRs without further
comment are not assigned a WGF level here, as that's reflected in the issue
tracker.

In general, @@@ markers indicate that something is not filed into the right
worlfklows yet, including text parts in responses that can't refer to a PR yet
because it's not yet written (but the response text's language indicate it is).

Russ for Gen-ART
================

Original: https://datatracker.ietf.org/doc/review-ietf-core-resource-directory-25-genart-telechat-housley-2020-07-27/

Tracking issue: https://github.com/core-wg/resource-directory/issues/247


> Section 7.1 says: "... can be transported in the subject."  I think
> you should say "subject field" or "subject name".  Do you mean to
> exclude the subject alternative name?

Issue: https://github.com/core-wg/resource-directory/issues/255

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

response:

With growing numbers of participants, the chances some collision happening
stays at a constant level even with the 2n length due to the birthday paradox,
which is why the collision on the initial attempt is highlighted.

A bit of clarifying information was added in
https://github.com/core-wg/resource-directory/pull/248, without attempting to
verbosely lay out the whole background.

> Nits: [...]

resonse:

All addressed in https://github.com/core-wg/resource-directory/pull/246

> IDnits reports:
> 
>  == There are 3 instances of lines with non-ascii characters in the
>     document.

response:

Two of them are in an author's name, the third is in an example and relevant
there (as it talks about variations of a representation containing non-ascii
charactes).

>  == There are 1 instance of lines with multicast IPv4 addresses in the
>     document.  If these are generic example addresses, they should be
>     changed to use the 233.252.0.x range defined in RFC 5771

response:

That instance is a suggestion to IANA, it will be replaced with the actually
assigned address.

>  == There are 3 instances of lines with non-RFC3849-compliant IPv6
>     addresses in the document.  If these are example addresses, they
>     should be changed.

response:

see GENERIC-FFxxDB

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

response:

We've taken the comment as an opportunity to cut back on the history lesson
(change in https://github.com/core-wg/resource-directory/pull/249).

> It is my impression, that Security Considerations were mostly written having
> in mind that (D)TLS is always used, however it is only "SHOULD" in this draft
> (or even "MAY" if we look at RFC6690 which Security Considerations this draft
> refers to). I think that adding a few words describing which consequences for
> security not using (D)TLS would have and in which cases it is allowed will
> make the Security Considerations more consistent. 

response:

Which level of protection is adaequate depends on the security policies. The
text you refer to was missed in updates, and now reflects that some security
mechanism ((D)TLS or OSCORE) SHOULD be used where the policies in place
indicate sensitive data. (See
https://github.com/core-wg/resource-directory/pull/250 for the full change).

Roman Danyliw
=============

Original: https://datatracker.ietf.org/doc/draft-ietf-core-resource-directory/ballot/#roman-danyliw

Tracking issue: https://github.com/core-wg/resource-directory/issues/257

As DISCUSS:

> There appear to be a few areas of straightforward, under-specified elements
> of the authorization model.  
> 
> -- How does the RD know that a node claiming to be a CT is in fact a CT and
> is permitted to register on behalf of end-points?  It seems like there is a
> missing, simple statement to make that this is configured out of band with
> the RD?  Or is that carrier somehow in a authentication credentials?	

response:

The RD does not distinguish between endpoints and CTs; both are just CoAP
clients that have to present suitable credentials.

The first mention of CTs in the security policies has some text to that.

> -- Is there are reason why there is not normative guidance requiring the RD
> to check whether authentication clients are authorized to register particular
> resources?  Section 7.1 covers the issue, but all of Section 7.* is
> explicitly noted as informative.  Section 8.1. says “Endpoint authentication
> needs to be checked independently of whether there are configured
> requirements on the credentials for a given endpoint name (Section 7.1) or
> whether arbitrary names are accepted (Section 7.1.1)” but this text seems to
> frame it as authentication issue.  Section 8.2 seems to stress only the
> distinction between the registration and lookup API.

response:

The "authentication" here is a plain error -- it is the authorization that
needs to be checked. (Fixed in
https://github.com/core-wg/resource-directory/pull/271).

The First-Come-First-Remembered policy that is now provided should help the
reader to understand how a policy can come to an authorization decision even
with arbitrary endpoint names (see
https://github.com/core-wg/resource-directory/pull/258).

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

@@@

Issue: https://github.com/core-wg/resource-directory/issues/255

And as COMMENT:

> ** Section 3.5.  Per “When endpoints are not connected … a remote server is
> usually used to provide proxy access to the endpoints”, this architecture
> wasn’t entirely clear to me.  How can a proxy provide access to an endpoint
> that isn’t connected?  Or is proxy meant as a substitute here or an
> intermediary?

response:

There have been different approaches to the sleepy nodes problem. None has
resulted in a WG document even, but the awake helper being a proxy is a common
theme. Note that a proxy does not need to contact the origin server to serve
all requests as it may have a fresh representation. The different approaches to
sleepy result in "beefed up" proxies that have better chances of being able to
serve some kind of cached response.

<!--
CB: patent sticken territory; proxies are safest
→ CB: provide PR
-->

> ** Section 3.6.  The home and building automation use case doesn’t make any
> reference to the RD architecture (like the other two use cases).

response:

They do now (as per https://github.com/core-wg/resource-directory/pull/264).

<!--
can sure sprinkle some keywords but what does it contribute?

maybe examples, tradfri?
-->

> ** Section 4.0.  Per “… falling back to failing the operations if recovery is
> not possible”, can “failing the operation” be clarified?

response:

Not without growing the text a lot. A failed operation's meaning depends on the
operation: In steps from finding an RD to registration, it means stopping
whatever was just tried and continuing with other options, and eventually
running out of them notifying the user. In regstration updates, it means that a
new registration is started. In lookups, the application may fall back to
methods outside the specification (eg. doing multicast discovery when no RD is
around or usable) or just report to the user.

For where there is something to say, sections contain paragraphs starting "If
the ... fails".

> ** Per Section 4.0.  Per “An RD MAY make the information submitted to it
> available to further directories”, are there circumstances where end points
> would not want that?

response:

If there is a security policy in place for link confidentiality, yes. The
presence of such a policy doesn't rule out replication at all, though -- if the
target directory is authorized to receive the links (as it upholds the same
link confidentiality policies), forwarding can still be justified, and is
expected to happen like that in managed installations.

The paragraph has been amended to refer to the security policies applicable to
lookups. (Concrete change in
https://github.com/core-wg/resource-directory/pull/272).

> ** Section 4.1.  Per “2. In a network that supports multicast well, …”, what
> does it mean to “support multicast _well_”?

response:

"Not well" is meant to roughly summarize "not efficiently" (when the multicast
would be flooded over many individual links), "not conveniently" (when it works
but the tooling around it makes it hard to use for implementations), or even
"not at all" (when other CoAP transports than CoAP-over-UDP are involved).

> ** Section 5.  Per the ep definition of the URI Template Variables, what does
> it mean for the an endpoint to be “(mostly mandatory)?

"Mostly mandatory" here means that it is mandatory unless the RD is configured
to recognize the endpoint name from the credentials. Enhancements have been
made to the wording of the exception at
https://github.com/core-wg/resource-directory/pull/273, which should make the
phrase "mostly mandatory" fully understandable by the end of the item.

> ** Section 7.1.  Per “When certificates are used as authorization
> credentials, the sector(s) and endpoint name(s) can be transported in the
> subject”, recommend being more precise on what exact X.509 field(s) you mean
> when saying “subject”.

@@@

Issue: https://github.com/core-wg/resource-directory/issues/255

> ** Section 7.1.1. Per “Registrants that are prepared to pick a different
> identifier when their initial attempt at registration is unauthorized should
> pick an identifier at least twice as long as the expected number of
> registrants”, how would a registrant know the population size?

response:

Applications can describe typical sizes of their deployments; for example, in a
single-tenant home automation system, 256 is a sane upper bound for the size
estimate, so 4 hex digits would suffice.

Where there's no such estimates available, the UUID way is universally
applicable.

> ** Section 7.2.  Per “To avoid the limitations, RD applications should
> consider prescribe that lookup clients only use the discovered information as
> hints, and describe which pieces of information need to be verified with the
> server”, I wasn’t sure which verification this would be.

response:

This triggered a deeper discussion at <https://mailarchive.ietf.org/arch/msg/core/JyW0XAkXre1wvKoNxMwegOUCywc/>.

@@@

<!--
WGF-9

group: Are we aligned on "don't just blindly POST empty stuff with your permission unless you're sure the server is what you think it is?" otherwise respond "depends too much on the non-RD application's policies that we could make any more precise statement"
--.

> ** Section 7.3.  This section cautions about the differences between the
> registrant publishes itself vs. what is in the RD.  It might be worth
> reiterating that the RD may also publish what it knows to others per Section
> 4.0’s “An RD MAY make the information submitted to it available to further
> directories”

response:

A reference has been added in the other direction, as that is where the care
must be taken -- the "MAY make [...] available" now cautions about any link
confidentiality policies (change in
https://github.com/core-wg/resource-directory/pull/272).

> ** Editorial Nits -- Global.  s/can not/cannot/g

response:

Addressed in https://github.com/core-wg/resource-directory/pull/253

> -- Section 4.  Editorial.  Per “Only multicast discovery operations are not
> possible on HTTP, and Simple Registration can not be executed as base
> attribute … can not be used there”, this sentence didn’t parse.

response:

Understandable; it was changed in https://github.com/core-wg/resource-directory/pull/274.

Benjamin Kaduk
==============

Original: https://datatracker.ietf.org/doc/draft-ietf-core-resource-directory/ballot/#benjamin-kaduk

Tracking issue: https://github.com/core-wg/resource-directory/issues/257

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

response:

There is no external policy we could reference, so a new section was created.
The First-Come-First-Remembered policy implements one of the candidates that
were considered for this role, and was picked because unlike its "endpoint name
comes from the certificate" it is a mode which an implementation can use
without any further configuration whatsoever.

The related changes can be viewed in https://github.com/core-wg/resource-directory/issues/258.

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

respond:

The responsibilities are the other way around. The RD does not need to know the
clients' expectations, the clients may only expect things they know to be true
of the RD.

See GENERIC-WHOPICKSMODEL.

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

WGF-8 @@@ just as I don't even know what to respond

> If I understand correctly, we have some codepoint squatting going on in
> the examples (e.g., for resource types).

response:

The rt=temperature-c, rt=light-lux and if=sensor are used where endpoints
mimick the examples of RFC6690; it is a point here to have things look just
like in direct discovery.

The if=core.a and if=core.p use values from the expired and partially abandoned
core-interfaces -- given its future is unclear, they've been replaced by
examples with tag URIs, as has the et=oic.d.sensor (a value that's registered,
but not to for et but for rt) and rt="light"; rt=sensor was dropped as it was
not essential to the example.

The individual changes are listed in https://github.com/core-wg/resource-directory/pull/266.

<!--
CB: ensure unit names SenML compatible
-->

> We should talk about the security properties of the various RD discovery
> mechanisms that are defined.

response:

A section was added in the security considerations on this topic (see
https://github.com/core-wg/resource-directory/pull/275 for text
changes). It does not go into the properties of each mechanism, as most
discovery steps are generally unprotected; instead, it emphasizes the
importance of checking the RD's authorization for any security properties the
client would expect.

More profoundly, though, it also played into the discussion at
<https://mailarchive.ietf.org/arch/msg/core/JyW0XAkXre1wvKoNxMwegOUCywc/>,
which has not come to a conclusion yet. @@@

<!-- "RD is at coap://box123.example.com/lcd-display" – POSTing confidential
stuff there b/c well yeah coap://box123.example.com/rd *would* be authorized
-->

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

response:

Putting it in there as a trusted entity would give the reader a wrong
impression of the general case. Any trust placed in the RD must be earned by a
security policy backed by the RD's credentials.

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

response:

The intended structure that's linearized into the sadly untreeish structure of
written language was

* discovery
* of registrations
  - creation
  - maintenance
  - removal
* lookup

I think this is what the current text expresses, whereas the proposed one
groups discovery with "of registrations", while it's more a top-level thing.

>    Commissioning Tool
>       Commissioning Tool (CT) is a device that assists during the
>       installation of the network by assigning values to parameters,
>       naming endpoints and groups, or adapting the installation to the
>       needs of the applications.
> 
> Is "the installation of the network" a one-time event?   (Might a CT be
> involved when adding a new device to a network at a later time?)

@@@ tracked at https://github.com/core-wg/resource-directory/issues/290

> Section 3.1
> 
>    Information SHOULD only be stored in the RD if it can be obtained by
>    querying the described device's /.well-known/core resource directly.
> 
> When might that not be the case?

response:

The prime example here is with devices that don't even have a copy of what they
might want to (but can't for resource constraints) express there; those use a
CT to do their work.

The second example I can come up with is when devices have complex
confidentiality requirements on the links, but rely on the RD and thus publish
data to an authorized RD of which they don't even know who precisely might be
authorized to read them.

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

WGF-6
@@@ generalize to network address, as we really serve both points
@@@ the bad part here is that we're using RFC7252 endpoint terminology without
really meaning it

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

response:

The act of the endpoint triggering the RD to fetch links from it is the
creation. And the "locking out" is the correct reading -- a client that uses
simple client has no way of managing the contents. If it were capable enough to
do that, it'd go the regular registration route.

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

response:

Right; fixed in https://github.com/core-wg/resource-directory/pull/276.

> Section 4.1
> 
>    1.  In a 6LoWPAN, just assume the Border Router (6LBR) can act as an
>        RD (using the ABRO option to find that [RFC6775]).  Confirmation
>        can be obtained by sending a Unicast to "coap://[6LBR]/.well-
>        known/core?rt=core.rd*".
> 
> nit(?): I was unaware that "Unicast" was a proper noun.

response:

Addressed in https://github.com/core-wg/resource-directory/pull/277.

> Section 4.3
> 
>    "core.rd" in the query string.  Likewise, a Resource Type parameter
>    value of "core.rd-lookup*" is used to discover the URIs for RD Lookup
>    operations, core.rd* is used to discover all URI paths for RD
>    operations.  [...]
> 
> Is the distinction between URIs (for RD Lookup) and URI paths (for RD)
> important here?

response:

No, it isn't. Fixed in https://github.com/core-wg/resource-directory/pull/277.

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

response:

If it was a requirement on the server, the clients could rely on it and thus
implicitly limit the set by failing to parse the full URIs.

(It could say "explicitly or implicitly limit", but only the "implicitly limit" case justifies the "therefore".)

>    It would typically be stored in an implementation information link
>    (as described in [I-D.bormann-t2trg-rel-impl]):
> 
>    Req: GET /.well-known/core?rel=impl-info
> 
> This seems to be depicting a link-relation type that is not registered
> at https://www.iana.org/assignments/link-relations/link-relations.xhtml
> , i.e., codepoint squatting.  Please put in a stronger disclaimer that
> this is an example link relation type, not just an example exchange.

response:

A note has been added that the type is just proposed in a WIP document (in
https://github.com/core-wg/resource-directory/pull/278).

> Section 5
> 
> These first few paragraphs give the impression that this is
> first-come-first-served with minimal authentication or authorization
> checking.  Mentioning that there are authorization checks, with a
> forward-reference, might be helpful.

response:

It's more a last-come-longest-remembered, but even the most minimal security
policies would ensure that the registration resources belong to the "same"
device (for whatever the policy defines as same).

Clarified in https://github.com/core-wg/resource-directory/pull/292 @@@merge.

>    further parameters (see Section 9.3).  The RD then creates a new
>    registration resource in the RD and returns its location.  The
> 
> Is this returned "registration resource" expected to function as a
> "capability URL" (https://www.w3.org/TR/capability-urls/) that would
> need to contain an appropriate amount of entropy to be reasonably
> unguessable by parties other than the registrant-ep/CT responsible for
> it?

response:

No, it is not a capability URL -- it will be discoverable through the endpoint
lookup interface.

Note that around ACE, bearer tokens (which capability URLs are) are generally
discouraged in favor of proof-of-possession tokens.

>    The registration request interface is specified as follows:
> 
>    Interaction:  EP -> RD
> 
> I thought that the CT could be a requestor as well as the EP.

response:

WGF-6 because it's "we're using RFC7252 endpoint terminology"
@@@ do we have a term for who does the registration?

>          well.  The endpoint name and sector name are not set when one
>          or both are set in an accompanying authorization token.
> 
> What should the RD do if they are set but also present in the
> accompanying authorization token?

response:

The wording has been updated in
https://github.com/core-wg/resource-directory/pull/273; it now (by
construction, but also explicitly) explains conflict handling.

>    Req: POST coap://rd.example.com/rd?ep=node1
>    Content-Format: 40
>    Payload:
>    </sensors/temp>;ct=41;rt="temperature-c";if="sensor",
> 
> (side note) XML for the sensors, not SenML?  With Carsten as an author,
> even? ;)

response:

This is clearly a mistake, and got removed in an emergency update in
https://github.com/core-wg/resource-directory/pull/279.

More seriously, though, these examples are from RFC6690 (which does not have
ct= entries for reasons of chronology), and keeping them aligned is a good
thing.

>    An RD may optionally support HTTP.  Here is an example of almost the
>    same registration operation above, when done using HTTP.
> 
>    Req:
>    POST /rd?ep=node1&base=http://[2001:db8:1::1] HTTP/1.1
>    Host: example.com
> 
> Wouldn't "Host: rd.example.com" be closer to "almost the same
> registration"?

response:

Fixed in https://github.com/core-wg/resource-directory/pull/277.

(I had brief qualms about introducing a protocol-negotiation situation here,
but performing "almost the same registration" over two protocols already
necessarily does that).

> Section 5.1
> 
> I'm a little uneasy about specifying new behavior for POST to the
> existin /.well-known/core that was defined by RFC 6690 for other uses.
> What factors go into using the same well-known URI vs. defining a new
> one for this usage?

response:

It-always-having-been-that-way, primarily. As no large deployments are known,
this is fixed in https://github.com/core-wg/resource-directory/pull/259
by switching to a standalone /.well-known/rd.

<!-- From discussion:

Any deployment of simple? Otherwise, /.well-known/rd would be fine with me

LWM2M doesn't use that

let's take /.well-known/rd (with note; "implementations may")
-->

>    The sequence of fetching the registration content before sending a
>    successful response was chosen to make responses reliable, and the
>    caching item was chosen to still allow very constrained registrants.
> 
> I'm not sure what "the caching item" is supposed to be (if it's not a
> typo/misordering of words).

response:

Now phrased as "the point about caching" (in
https://github.com/core-wg/resource-directory/pull/277) which should
be easier to read. A few lines up we recommend that the RD caches the .wk/c,
and this provides the rationale.

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

response:

No, it's a choice there as well. One server may keep them around forever, and
when the simple client comes back it'll show with the same registration
resource in the resource lookup. Another server may GC it and assign a
different registration resource when it returns.

> Section 5.3.1
> 
>    An update MAY update the lifetime or the base URI registration
>    parameters "lt", "base" as in Section 5.  Parameters that are not
> 
> What about the "extra-attrs"; are they inherently forbidden from
> updates?

response:

The introduction paragraph was overly specific and fixed in https://github.com/core-wg/resource-directory/pull/294 @@@merge.

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

response:

The restrictions apply to the new value, and were moved up there in https://github.com/core-wg/resource-directory/pull/294 @@@merge.

>    The following example shows how the registering endpoint updates its
>    registration resource at an RD using this interface with the example
>    location value: /rd/4521.
> 
> The path component "4521" contains a worryingly small amount of
> unpredictableness; I would prefer examples that used longer random
> locations, as for capability URLs.  (Throughout the document, of
> course.)  See also draft-gont-numeric-ids-sec-considerations, that I'm
> AD sponsoring, though I do not see any clear issues on first glance.

response:

See comment on the original capability URL question -- they are not.

> (Also, it might be worth another sentence that this update is serving
> just to reset the lifetime, making no other changes, since this might be
> expected to be a common usage.)

response:

Stating purpose rather than mechanism now since @@small-editorials.

> Section 6
> 
> With "Resource Lookup" and "Endpoint Lookup" as (apparent) top-level
> siblings, would it make sense to put 6.2, or at least 6.3, as
> subsections under 6.1?

WGF-6

@@@ more of a "lookup filtering as a named paragraph group"? b/c the uri template follows after it

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

response:

Some words added for clarity in https://github.com/core-wg/resource-directory/pull/294 @@@merge.

>    If the base URI of a registration contains a link-local address, the
>    RD MUST NOT show its links unless the lookup was made from the same
>    link.  The RD MUST NOT include zone identifiers in the resolved URIs.
> 
> Same link as what?

response:

The link the endpoint sits on; clarified in https://github.com/core-wg/resource-directory/pull/294 @@@merge.

> Section 6.2
> 
>    The page and count parameters are used to obtain lookup results in
>    specified increments using pagination, where count specifies how many
> 
> (We haven't introduced the page and count parameters yet.)

response:

Wording has been enhanced in https://github.com/core-wg/resource-directory/pull/294 @@@merge.

<!--

WGF-3
just introduce them (but i never liked it up there anyway so can i move it down to after filtering? it's not like that's the most important, and it comes last sequentially in impls as well)

-->

>    operator as in Section 4.1 of [RFC6690].  Attributes that are defined
>    as "link-type" match if the search value matches any of their values
> 
> Where is it specified how an attribute might be "defined as
> 'link-type'"?  This is the only instance of the string "link-type" in
> this document, and it does not appear in RFC 6690 at all...

response:

That should have said "relation-types"; it does now, and also refers to the 6690 ABNF (since https://github.com/core-wg/resource-directory/pull/294 @@@merge).

>    references) and are matched against a resolved link target.  Queries
>    for endpoints SHOULD be expressed in path-absolute form if possible
>    and MUST be expressed in URI form otherwise; the RD SHOULD recognize
>    either.  The "anchor" attribute is usable for resource lookups, and,
>    if queried, MUST be for in URI form as well.
> 
> I don't see how it can be only a SHOULD to recognize either given these
> generation criteria.

response:

If the URI is on a different scheme/host, I assert things are clear. (Just to
ensure I didn't get your point wrong.)

Otherwise, in practice there can happen mistakes where server and client
disagree about the default values of the Uri-Scheme, Uri-Host and Uri-Port
options -- as anyone who's ever tried to set up an HTTP reverse proxy for a
WebDAV server can attest to. We're trying to avoid creating these situations,
but when they do happen. We don't automatically declare the offending party
broken by putting a MUST here, but encourage the peer to assist it. The client
can help by providing the relative reference (for then, disagreement passses
unnoticed), and the server by recognizing the full URI (for the client may have
obtained it and not know that it'd match what the server thinks is its Uri-Host
name).

(The "and MUST be expressed in URI form otherwise" sounds like a factual
necessity, but it is here to rule out the corner case of a client handing out
//hostname/path style references).

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

response:

Yes. Fixed in https://github.com/core-wg/resource-directory/pull/294 @@@merge.

> Section 6.4
> 
>    The endpoint lookup returns registration resources which can only be
>    manipulated by the registering endpoint.
> 
> This seems to leave it unclear whether the endpoint lookup is expected
> to return resources that the requestor will not have permission to
> manipulate (in addition to those it does have permission for).

response:

Clarified in https://github.com/core-wg/resource-directory/pull/294 @@@merge.

>    While Endpoint Lookup does expose the registration resources, the RD
>    does not need to make them accessible to clients.  Clients SHOULD NOT
>    attempt to dereference or manipulate them.
> 
> But why expose them at all if they're not going to be accessible?

response:

They serve as identifiers (think URI rather than URL), and may additionally be
used in implementation defined operations on the resource that could be allowed
for administrators. Last but not least, link-format (unlike the upcoming CoRAL)
does not have means of talking about something without naming it.

(I do see the point, and if we started RD anew with the benefit of having
CoRAL, chances are this would look a bit different, and the names would not be
exposed to just any lookup client).

The WG discussion of this did, however, lead to a point added to the security
considerations about the RD's choice of what to put in there (change in https://github.com/core-wg/resource-directory/pull/267 @@@merge).

<!--
CB: but information disclosure problem -> bycatch section
-->

>    An RD can report endpoints in lookup that are not hosted at the same
>    address.  [...]
> 
> The "same address" as what?

response:

Sharpened in https://github.com/core-wg/resource-directory/pull/294 @@@merge.

> Section 7.1
> 
>    Whenever an RD needs to provide trustworthy results to clients doing
>    endpoint lookup, or resource lookup with filtering on the endpoint
> 
> How will the RD know whether the client is expecting trustworthy
> results?  (When would a client *not* expect trustworthy results?)

response:

It won't per-client, it is configured for one. See GENERIC-WHOPICKSMODEL.

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

WGF-6
response:

The RD might do such checks, but then again the EP might just be using
different network interfaces simultaneously. At that point where the EP uses a
different (and usually dormant) network interface for registration, the line
between EP and the CT gets blurry; we tolerate that blurriness because the
distinction is not so much a technical one (the REST server does not care
whether the request originates at its network peer, is proxied through there or
sent from there on behalf of someone completely different) as long as the
credentials are good.

Frankly, I'm personally not too happy with distinguishing CTs in the first
place; it is more reflective of what I understand to be an industry practice
than a distinction in this CoAP application.

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

Issue: https://github.com/core-wg/resource-directory/issues/255

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

respond:

There is no requirement here as collisions only result in retries. (For those
cases where the client implementer thinks they can get away with not
implementing retry, lack of entropy can be a DOS vector).

The respective sections in the text were @@@ amended.

<!--
change to "(not necessarily secure) random", and add that if you rely on colls
-->

> Section 7.2
> 
>    When lookup clients expect that certain types of links can only
>    originate from certain endpoints, then the RD needs to apply
>    filtering to the links an endpoint may register.
> 
> As before, how will the RD know what behavior clients are relying on?

response:

It will not. It may, however, advertise it explicitly. If, for example, an
application like LwM2M always ensures trusted endpoint names, the RD may
advertise as rt="core.rd-lokup-ep example.lwm2m", and then clients that trust
that metadatum (which they'll want to verify from some claim) know they can
trust the RD to have checked ep names.

See also GENERAL-WHOPICKSMODEL

>    An RD may also require that only links are registered on whose anchor
>    (or even target) the RD recognizes as authoritative of.  One way to
> 
> I don't think I can parse this sentence (especially "the RD recognizes
> as authoritative of").

response:

Rephrased to "require that links are only registered if the registrant is
authorized to publish information about the anchor [...] of the link." in
https://github.com/core-wg/resource-directory/pull/294 @@@merge.

> Section 8
> 
> In contexts where we discuss DTLS and TLS as being generally comparable,
> we typically will state that DTLS replay protection is required in order
> to provide equivalent levels of protection.

response:

Thanks, it is now a requirement (full change in
https://github.com/core-wg/resource-directory/pull/260 @@@merge).

We've also taken this up as an action item for further work on CoAP, as there
is little awareness from CoAP users that this is optional in DTLS in the first
place, let alone in CoAP.

<!--
WGF-8 (yeah but CoAP in general)
WHAT, that's optional and CoAPS does not pull it in? I'd be surprised if
any implementer of CoAP who isn't working on DTLS but just using it as
described in 7252 is aware they must only implement long-term idempotent
resource handlers, and that that's a security requirement.

is there any part in RD that's not long-term replay safe? changes to lt,
and DELETE but who'll DELETE to later reuptake?

enforce on RD (and make a note in const-corr)
-->

> We might also want to reiterate or refer back to the previous discussion
> of the potential for attributes or resource/endpoint names, link
> relations, etc. that may need to be confidential, the relevant access
> control/filtering, and the avenues by which disclosure of resource names
> can occur even when access to those resources will not be permitted.  (I
> think some of this overlaps with 8288 and 6690, but don't mind repeating
> it.)

WGF-3
@@@ just-do-it backreference to 7.3 and if 6690/8288 has something ref there from 7.2

> Section 8.1
> 
> It's probably worth reiterating that all name comparisons must be done
> at sector scope (since failing to do so can lead to attacks).

WGF-2
@@@ just-do-it "given endpoint name" -> "given endpoint name (and sector)"

>    Endpoint authentication needs to be checked independently of whether
>    there are configured requirements on the credentials for a given
>    endpoint name (Section 7.1) or whether arbitrary names are accepted
>    (Section 7.1.1).
> 
> I think this is more properly authorization than authentication.

response:

Yes; fixed in https://github.com/core-wg/resource-directory/pull/271.

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

response:

The section has been shortened in https://github.com/core-wg/resource-directory/pull/249.

> Section 9.3
> 
> Should we also include "rt" in the initial entries?  I see it is used as
> a query parameter for resource lookup in the examples in Section 6.3.

response:

It's used as is any other link attribute. There's no registry for them, and
while there's been talk over ond over that it would be nice, I don't think
there will be any until linkformat-CoRAL conversion is defined (and even then
it may not be comprehensive). Selectively picking some distinguished common
link attributes into this registry won't make things less messy.

The prime line of defense against this getting messy is the expert guidance
that for some types of parameters their short names should be checked against
"commonly used target attributes".


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

response:

The text suggests that target attributes for registered resources need not be
registered. These unregistered wild-west attribute names can be used both with
resource lookups (matching only resources which), and in endpoint lookups
(matching endpoints that contain any resource which).

If `if` were to be put in for use in an RD parameter used with lookup, that
would not per se create ambiguous queries (the rules would still say "matches
either"), but the results would be prone to causing confusion.

> Section 10.1.2
> 
> Should we really be using unregistered resource types (i.e., codepoint
> squatting) in the examples?

response:

Addressed together with the earlier code squatting comments in https://github.com/core-wg/resource-directory/pull/266.

>    After the filling of the RD by the CT, the application in the
>    luminaries can learn to which groups they belong, and enable their
>    interface for the multicast address.
> 
> Just to check: the luminaries are learning their own group membership by
> querying the resource directory?

response:

Not directly. They (in this very particular example that seems to be based on
industry process but which I'd not necessarily recommend for imitation) use a
heuristic to find any multicast URI they might possibly provide, and join that
group.

(also GENERIC-ODDEXAMPLES)

@@@ "alternatively," remove as well

> Section 10.2.2
> 
> Please expand MSISDN.

response:

Taking a step back from this and other comments led to a drastical shortening
of the example.

See also GENERIC-ODDEXAMPLES

> Section 13.2
> 
> I think RFC 7252 should probably be normative.
> 
> Likewise for RFC 8288 ("the query parameter MUST be [...] a token as
> used in [RFC8288]").

WGF-6
@@@ agree based on guidance I read for ERT that it's normative even when required for an optional part; might even hit 7230

Erik Kline
==========

Original: https://datatracker.ietf.org/doc/draft-ietf-core-resource-directory/ballot/#erik-kline

Tracking issue: https://github.com/core-wg/resource-directory/issues/257

As DISCUSS:

> [ section 4.1.1 ]
> 
> * Did this get presented to 6man at any point, either via mail to the list or
>   chair or in a presentation slot at an IETF meeting or a 6man interim?
> 
>   I feel confident that there would be no objection to the option as described
>   here, but the working group should have its chance to make an evaluation
>   irrespective of my opinion.

see: GENERIC-6MAN

>   If this is to be used when link-local methods don't work, another option
>   would have been to add an RD PVD API key and recommend including a PVD
>   option.

WGF-6
@@@ gotta look that up

> [ section 4.1.1 & 9.2 ]
> 
> * Please clarify which ND messages can carry an RDAO.  I suspect they should
>   only appear in RAs, but it would be good to state the expectation explicitly.

respond:

You are right, and the text now says so.

The concrete change is in https://github.com/core-wg/resource-directory/pull/262.

<!--
just-do-it

"put it where DNS are put"
-->

> [ Appendix A. ]
> 
> * Can you explain the ff35:30:2001:db8:1 construction?  RFC 3306 section 4
>   defines some fine-grained structure, and I'm wondering how a group ID of 1
>   is selected/computed/well known.  If there is already a COAP document
>   describing this vis. RFC 3307 section 4.*, perhaps it's worth dropping a
>   reference in here.

response:

See GENERIC-FFxxDB

As COMMENT:

> [ section 1 ]
> 
> * I'm unclear on what "disperse networks" might mean.

response:

Well how do I phrase this ... so were we. As the term does not provide
justification for using an RD, it was removed from abstract and introduction in
https://github.com/core-wg/resource-directory/pull/269.

> [ section 10.1.1 ]
> 
> * What is meant by "therefore SLAAC addresses are assigned..." followed by this
>   table of not-very-random-looking IPv6 addresses?
> 
>   Is the assumption that there might not be some off-network DNS server but
>   there is some RA with a /64 A=1 PIO?

response:

There are two scenarios that satisfy the tacit assumptions -- a router can be
in place without an uplink or any DNS server and still supply ULA A=1 PIOs, or
there is a routable prefix around, but not yet coordinated with the lighting
installation.

The 2001:db8:: addresses are indeed not what one would get out of SLAAC, but
full random addresses would make the examples hard to read. Where the addresses
are introduced, they are now called stand-in addresses for the examples (see
https://github.com/core-wg/resource-directory/pull/268 for full
change).

<!--
see GENERIC-ODDEXAMPLES
-->

Éric Vyncke
===========

Original: https://datatracker.ietf.org/doc/draft-ietf-core-resource-directory/ballot/#eric-vyncke

Tracking issue: https://github.com/core-wg/resource-directory/issues/257

As DISCUSS:

> Thank you for the work put into this document. I am little puzzled by the
> document shepherd's write-up dated more than one year ago (the responsible AD
> has even changed and the change is not reflected in the write-up)... while
> well-written this write-up seems to indicate neither a large consensus nor a
> deep interest by the CORE WG community. But, I am trusting the past and
> current responsible ADs on this aspect.

response:

The shepherd write-up has been updated.

> Did the authors check with 6MAN WG about the new RDAO option for IPv6 NDP ? I
> was unable to find any 6MAN email related to this new NDP option and, after
> checking with the 6MAN WG chairs, they also do not remember any discussion.

see: GENERIC-6MAN

> == DISCUSS ==
> 
> -- Section 4.1 -- It will be trivial to fix, in IPv6 address configuration
> (SLAAC vs. DHCP) is orthogonal to DHCP 'other-information'. E.g., even if
> address is configured via SLAAC, DHCPv6 other-information can be used to
> configure the Recursive DNS Server (or possibly the RD).

response:

Thanks, fixed in https://github.com/core-wg/resource-directory/pull/263.

Conversely, the RDAO's applicability is now phrased more generally as well.

<!--
CB: can provide DHCP even when using SLAAC. then that option could be used as well.

"When DHCP is in use,"
-->

> -- Section 4.1.1 -- Another trivial DISCUSS to fix: in which message is this
> RDAO sent ? I guess unicast Router Advertisement but this MUST be specified.

response:

Indeed; fixed in https://github.com/core-wg/resource-directory/pull/262.

As COMMENT:

> == COMMENTS ==
> 
> In general, I wonder how much interactions and exchanges of ideas have
> happened in the long history of this document with the DNSSD (DNS Service
> Discovery) Working Group that has very similar constraints (sleeping nodes)
> and same objectives.

WGF-5
response:

Discussion was primarily on the level of granularity of service description
(what is announced in DNS-SD often corresponds to multiple resources in an RD),
and on protocol negotiation (input which is more suitable for the
protocol-negotiation work than it is going into RD).

> -- Section 2 -- To be honest: I am not too much an APP person; therefore, I
> was surprised to see "source address (URI)" used to identify the "anchor="...
> I do not mind too much the use of "destination address (URI)" as it is really
> a destination but the anchor does not appear to me as a "source address". Is
> it common terminology ? If so, then ignore my COMMENT, else I suggest to
> change to "destination URI" and simply "anchor" ?

WGF-4
response:

"Context" is the term that RFC8288 uses, and other than when defining the
Context we don't use source address for that (as that term is used more
commonly with CoAP/UDP messages).

The "source" part in the definition has no clear lineage in RFC8288 (which does
not introduce the term formally), it stems from a link going "from" somewhere
(the context, or source) "to" somewhere (the target).

@@@ better definition?

> -- Section 3.3 -- Should the lifetime be specified in seconds at first use in
> the text?

WGF-3
response:

The lifetime is thought of as a quantity of time, which is only expressed in
multiples of a unit when serialized (and there, it is consistently used with
seconds).

> -- Section 3.6 -- Is the use of "M2M" still current? I would suggest to use
> the word "IoT" esp when 6LBR (assuming it is 6LO Border Router) is cited
> later.

WGF-6
@@@ basically yes

> Please expand and add reference for 6LBR.

WGF-1
@@@

> Using 'modern' technologies (cfr LP-WAN WG) could also add justification to
> section 3.5.

WGF-6 see IoT above
@@@

> -- Section 4.1 -- About "coap://[MCD1]/.well-known/core?rt=core.rd*", what is
> the value of MCD1 ? The IANA section discuss about it but it may help the
> reader to give a hint before (or simply use TBDx that is common in I-D).

WGF-5
response:

TBDx would have been easier in hindight, but there's hope that until RFC editor
replaces that, more people will be reading the diffs than new people that might
stumble here will read the document, so it's kept that way.

> Any reason to use "address" rather than "group" in "When answering a
> multicast request directed at a link-local address" ?

WGF-2
@@@ just change to group

> Later "to use one of their own routable addresses for registration." but
> there can be multiple configured prefixes... Which one should the RD select ?
> Should this be specified ?

WGF-6
@@@ could say "use its preferred routable", but does that really say something?

> As a co-author of RFC 8801, I would have appreciated to read PvD option
> mentionned to discover the RD. Any reason why PvD Option cannot be used ?

WGF-5
response:

Probably because 8801 was just published, and the RDAO predates even -pvd-00 by
a year.

Sassiness aside, as I understand the PvD option from a skim and the vague
recollections of having had a look at this at some earlier time, this would be
orthogonal to the RDAO: a router with multiple PvDs could forward the RDAO from
either uplink inside PvD options, and pick a primary one to forward to
PvD-unaware hosts.

I don't see any conflict, but don't see much potential either (are there many
constrained devices that benefit from becoming PvD-aware?) -- so I'd keep it as
with any other RA option: They can be combined, but there's no particular
reason to point that out on either side.

It might, however, warrant a note where we say that it "MAY keep concurrent
registrations if explicitly configured to do so", as receiving PvD RDAOs can be
interpreted as that very configuration. @@@ discuss

(This is slightly more interesting in cases when the RD picks the addresses; a
note on that will appear in the next version of
draft-amsuess-core-resource-directory-extensions).

> -- Section 4.1.1 -- I suggest to swap the reserved and lifetime fields in
> order to be able to use a lifetime in units of seconds (to be consistent with
> other NDP options).

WGF-6
@@@ dunno

> -- Section 5 -- May be I missed it, but, can an end-point register multiple
> base URI ? E.g., multiple IPv6 addresses.

WGF-3
response:

No, that is deferred to protocol-negotiation (where whatever comes out of it is
expected to do multiple addresses on the same protocol just as well as
different protocols).

> -- Section 9.2 -- For information, value 38 is already assigned to RFC 8781.

WGF-1
@@@ remove recommendation


> == NITS ==
> 
> -- Section 2 -- The extra new lines when defining "Sector" are slighly
> confusing. Same applies to "Target" and "Context". This is cosmetic only.

Issue: https://github.com/core-wg/resource-directory/issues/252

Martin Duke
===========

Original: https://datatracker.ietf.org/doc/draft-ietf-core-resource-directory/ballot/#martin-duke

Tracking issue: https://github.com/core-wg/resource-directory/issues/257

As COMMENT:

> One nit: the sentence that contains “cannot be executed as a base attribute”
> appears to have been mangled.

WGF-1
@@@

Murray Kucherawy
================

Original: https://datatracker.ietf.org/doc/draft-ietf-core-resource-directory/ballot/#murray-kucherawy

Tracking issue: https://github.com/core-wg/resource-directory/issues/257

As COMMENT:

> In Section 9.2, you might want to mention that you're talking about a
> sub-registry under "Internet Control Message Protocol version 6 (ICMPv6)
> Parameters".

@@@

> In Section 9.3, you enumerate six fields in each registration, but the
> initial table of entries has only five columns.  It's obvious (I think) that
> the sixth column would be "this document" for all entries, but I suggest that
> you should either include the column or some prose making this explicit
> (since everything else is).

@@@

Warren Kumari
=============

Original: https://datatracker.ietf.org/doc/draft-ietf-core-resource-directory/ballot/#warren-kumari

Tracking issue: https://github.com/core-wg/resource-directory/issues/257

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

@@@ yes

> 2: "From the system design point of view, the ambition is to design
> horizontal solutions that  can enable utilization of machines in different
> applications depending on their current availability and capabilities as well
> as application requirements, thus avoiding silo like solutions" - this is
> very buzzwordy, and I have no idea what it is actually trying to say...

TODAY

drop it (and align next sentence)

@@@

> 3: "  A (re-)starting device may want to find one or more RDs for discovery
> purposes."
>
> Either I don't understand what this sentence is  trying to say, or "for
> discovery purposes" should be dropped.... 

@@@

> 4: "As some of the RD addresses obtained by the methods listed here are just
> (more or less educated) guesses, endpoints MUST make use of any error
> messages to very strictly rate-limit requests to candidate IP addresses that
> don't work out. " What happens if device A discovers RD X, and device B
> discovers RD Y? Surely there has to be some sort of deterministic method so
> that one doesn't end up in a "split brain" type outcome? 

response:

There are various approaches that can apply depending on the actual application:

* In managed networks, care can be taken to not make multiple RDs discoverable.
* In larger setups, multiple RDs may be available but set up for federation as
  it is being explored in draft-amsuess-core-rd-replication
* RDs that are deployed without overarching coordination can opt for the
  Opportunistic Resource Directory approach that is being explored in
  draft-amsuess-core-resource-directory-extensions, where one RD yields to the
  other. The above replication steps make that transition smooth.

But long story short, this draft does not attempt to solve them.

> Nits:
>
> 1: " The input to an RD is composed of links and the output is composed of
> links constructed from the information stored in the RD." While  true, this
> sentence doesn't actually communicate anything useful to the reader -- I'd
> suggest removing it from the Abstract (note that this is just a nit).

@@@

> 2: "The RD is primarily a tool to make discovery operations more efficient
> than querying /.well-known/core on all connected devices, or across
> boundaries that would be limiting those operations."
>
> s/would be limiting those/that would limit those/

@@@

Robert Wilton
=============

Original: https://datatracker.ietf.org/doc/draft-ietf-core-resource-directory/ballot/#robert-wilton

Tracking issue: https://github.com/core-wg/resource-directory/issues/257

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

response:

The wide variety of possible deployments and associated security policies make
it hard to say anything generic on operations. We do, however, plan to add a
section that describes one assumed-to-be-widespread application variety, and
for those cases something will be said on Operations, Administration and
Management (that'll be short though as it's a simple setup).

@@@

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

response:

Generally registrations are short-lived soft state, so if a device is upgraded
it does not need to remember its registration resource and can just register
anew.

In scenarios with controlled names, the new certificate will match the new name
(no matter whether it's the same or a new one); where names are picked at
random and persisted, the device will pick a new name just as well @@@ but the
detailed new section may say more here.

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

PR: https://github.com/core-wg/resource-directory/pull/253

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

@@@

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

response:

This is correct. The link's context was originally </sensors/temp> relative to
the endpoint's base, and while this is being changed from
<coap://local-proxy-old.e.c> to <coaps://new.e.c>, the link always points from
the resource on the EP to the description hosted on HTTP.

Answers to repeated points
==========================

GENERIC-FFxxDB
--------------

response:

There's a coverage gap between RFC3849 (defining example unicast addresses) and
RFC3306 (defining unicast-prefix-based multicast addresses). For lack of an
agreed-on example multicast address, the used ones were created by applying the
unicast-prefix derivation process to the example address. The results
(ff35:30:2001:db8::1) should be intuitively recognizable both as a multicast
address and as an example address.

The generated address did, however, fail to follow the guidance of RFC 3307 and
set the first bit of the Group ID. Consequently, the addresses were changed
from ff35:30:2001:db8::x to ff35:30:2001:db8::8000:x, which should finally be a
legal choice of a group address for anyone operating the 2001:db8:: site. (The
changes can be viewed at https://github.com/core-wg/resource-directory/pull/270).

GENERIC-ODDEXAMPLES
-------------------

WGF-7

@@@ many examples contain bewildering and possibly irrelevant stuff like LWM2M,
"the SLAAC addresses" or the luminaries that just so join groups based on URIs
they happen to share (and that's not even explicit). who will fix that? my fix
would be to rip out anything someone complains about the examples.

object ID, etc -- why in here? generous rip-out?

CB: delete table.

Object Model... binding mode ...

JJ: "example of how"?

informative reference?

replace 10.2, and put example link -- safer.

GENERIC-6MAN
------------

WGF-7

@@@

GENERIC-WHOPICKSMODEL
---------------------

WGF-8 (see referrers)

The client can only expect any level of trustworthiness if there is a
claim to that in the RD's credentials. Typically, though, that claim will not
be encoded there, but implied. For example, when configuring an application
that relies authenticated endpoint names, then telling the application to use
the RD authenticated via PKI as coaps://rd.example.com should only be done if
the configurator is sure that rd.example.com will do the required checks on
endpoint names.

Some clarification has been added in https://github.com/core-wg/resource-directory/pull/265.

<!--

CB: trusted 3rd party needs to set that; we don't provide way to do that (ACE could)

text to security policies: ~"are a (possibly configurable) property of the server; clients may assume none unless RD is authenticated and authorized to serve as an RD with these properties"

-->
