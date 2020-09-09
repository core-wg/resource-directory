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
