---
v: 3
docname: draft-ietf-lamps-automation-keyusages-latest
cat: std
consensus: 'true'
submissiontype: IETF
lang: en
pi:
  toc: 'true'
  tocdepth: '3'
  symrefs: 'true'
  sortrefs: 'false'
title: X.509 Extended Key Usage (EKU) for configuration, updates and safety-communication
abbrev: EKU for config, update, and safety
area: sec
wg: LAMPS Working Group
keyword:
- Industrial Automation
- ERJU
- extended key usage
- extension
- PKI
date: 2025
author:
- name: Hendrik Brockhaus
  org: Siemens
  abbrev: Siemens
  street: Werner-von-Siemens-Strasse 1
  code: '80333'
  city: Munich
  country: Germany
  email: hendrik.brockhaus@siemens.com
  uri: https://www.siemens.com
- name: David Goltzsche
  org: Siemens Mobility
  street: Ackerstrasse 22
  code: '38126'
  city: Braunschweig
  country: Germany
  email: david.goltzsche@siemens.com
  uri: https://www.mobility.siemens.com
contributor:
- name: Szofia Fazekas-Zisch
  org: Siemens AG
  abbrev: Siemens
  street: Breslauer Str. 5
  code: '90766'
  city: Fuerth
  country: Germany
  email: szofia.fazekas-zisch@siemens.com
  uri: https://www.siemens.com
- name: Baptiste Fouques
  org: Alstom
  email: baptiste.fouques@alstomgroup.com
- name: Daniel Gutierrez Orta
  org: CAF Signalling
  email: daniel.gutierrez@cafsignalling.com
- name: Martin Weller
  org: Hitachi Rail
  email: martin.weller@urbanandmainlines.com
- name: Nicolas Poyet
  org: SNCF
  email: nicolas.poyet@sncf.fr

normative:
  RFC2119:
  RFC5280:
  RFC8174:
  X.680:
    target: https://www.itu.int/rec/T-REC.X.680
    title: >
      Information Technology - Abstract Syntax Notation One (ASN.1):
      Specification of basic notation
    author:
      org: ITU-T
    date: '2021-02'
    seriesinfo:
      ITU-T Recommendation X.680
  X.690:
    target: https://www.itu.int/rec/T-REC.X.690
    title: >
      Information Technology - ASN.1 encoding rules:
      Specification of Basic Encoding Rules (BER),
      Canonical Encoding Rules (CER)
      and Distinguished Encoding Rules (DER)
    author:
      org: ITU-T
    date: '2021-02'
    seriesinfo:
      ITU-T Recommendation X.690

informative:
  RFC5246:
  RFC7299:
  RFC8446:
  RFC9162:
  RFC9336:
  RFC9509:
  Directive-2016/797:
    target: https://eur-lex.europa.eu/eli/dir/2016/797/2020-05-28
    title: 'Directive 2016/797 - Interoperability of the rail system within the EU'
    author:
      org: European Parliament, Council of the European Union
    date: '2020-05-28'
  ERJU:
    target: https://rail-research.europa.eu/wp-content/uploads/2025/03/ERJU-SP-Cybersecurity-Specifications-V1.0.zip
    title: 'Shared Cybersecurity Services Specification - SP-SEC-ServSpec - V1.0'
    author:
      org: Europe's Rail Joint Undertaking
    date: '2025-02'
  EU-CRA :
    target: https://digital-strategy.ec.europa.eu/en/library/cyber-resilience-act
    title: 'Proposal for a REGULATION OF THE EUROPEAN PARLIAMENT AND OF THE COUCIL on horizontal cybersecurity requirements for products with digital elements and amending Regulation (EU) 2019/1020'
    author:
      org: European Commission
    date: '2022-09-15'
  EU-STRATEGY:
    target: https://digital-strategy.ec.europa.eu/en/library/eus-cybersecurity-strategy-digital-decade-0
    title: "The EU's Cybersecurity Strategy for the Digital Decade"
    author:
      org: European Commission
    date: '2020-12-16'
  NIST_Glossary:
    target: https://csrc.nist.gov/glossary/term/safety
    title: 'Directive (EU) 2022/2555 of the European Parliament and of the Council'
    author:
      org: NIST CSRC
  ISO.IEC.IEEE_12207:
    target: https://www.iso.org/standard/63712.html
    title: 'Systems and software engineering – Software life cycle processes'
    author:
      org: ISO/IEC/IEEE
    date: '2024-12-14'
  NIS2:
    target: https://digital-strategy.ec.europa.eu/en/policies/nis2-directive
    title: 'Directive (EU) 2022/2555 of the European Parliament and of the Council'
    author:
      org: European Commission
    date: '2024-12-14'
  IEC.62443-4-2:
    target: https://webstore.iec.ch/publication/34421
    title: 'Security for industrial automation and control systems - Part 4-2: Technical security requirements for IACS components'
    author:
      org: IEC
    date: '2019-02-27'
    seriesinfo:
      IEC 62443-4-2:2019
  IEC.62443-3-3:
    target: https://webstore.iec.ch/publication/7033
    title: 'Industrial communication networks - Network and system security - Part 3-3: System security requirements and security levels'
    author:
      org: IEC
    date: '2013-08-07'
    seriesinfo:
      IEC 62443-3-3:2013
  CE-marking:
    target: https://single-market-economy.ec.europa.eu/single-market/ce-marking_en
    title: 'CE marking'
    author:
      org: European Commission
  SMI-PKIX-PURPOSE:
    target: https://www.iana.org/assignments/smi-numbers/smi-numbers.xhtml#smi-numbers-1.3.6.1.5.5.7.3
    title: 'SMI Security for PKIX Extended Key Purpose'
    author:
      org: IANA
    date: false
  SMI-PKIX-MOD:
    target: https://www.iana.org/assignments/smi-numbers/smi-numbers.xhtml#smi-numbers-1.3.6.1.5.5.7.0
    title: 'SMI Security for PKIX Module Identifier'
    author:
      org: IANA
    date: false

--- abstract


RFC 5280 defines the Extended Key Usage (EKU) extension and several extended key purposes (KeyPurposeIds) for use with that extension in X.509 certificates.  This document defines KeyPurposeIds for general-purpose and trust anchor configuration files, for software and firmware update packages, and for safety-critical communication to be included in the EKU extension of X.509 v3 public key certificates.

--- middle

# Introduction {#Intro}

Key purposes (KeyPurposeIds) added to the certificate's extended key usage extension as defined in {{RFC5280}} are meant to express intent as to the purpose of the named usage, for humans and for complying libraries. A full list of KeyPurposeIds is maintained in the IANA registry "SMI Security for PKIX Extended Key Purpose" {{SMI-PKIX-PURPOSE}}. The use of the anyExtendedKeyUsage KeyPurposeId, as defined in {{Section 4.2.1.12 of RFC5280}}, is generally considered a poor practice.

This document defines KeyPurposeIds for certificates that are used for the following purposes, among others:

* Validating signatures of general-purpose software configuration files.

* Validating signatures of trust anchor configuration files.

* Validating signatures of software and firmware update packages.

* Authenticating communication endpoints authorized for safety-critical communication.


If the purpose of an issued certificate is not restricted, i.e., the type of operations for which a public key contained in the certificate can be used in unintended ways, the risk of cross-application attacks is increased. Failure to ensure adequate segregation of duties means that an application or system that generates the public/private keys and applies for a certificate to the operator Certification Authority (CA) could obtain a certificate that can be misused for tasks that this application or system is not entitled to perform. For example, management of trust anchors is a particularly critical task. A device could potentially accept a trust anchor configuration file signed by a service that uses a certificate with no Extended Key Usage (EKU) or with the KeyPurposeId id-kp-codeSigning ({{Section 4.2.1.12 of RFC5280}}) or id-kp-documentSigning {{RFC9336}}. A device should only accept trust anchor configuration files if the file is verified with a certificate that has been explicitly issued for this purpose.

The KeyPurposeId id-kp-serverAuth ({{Section 4.2.1.12 of RFC5280}}) can be used to identify that the certificate is for a TLS WWW server, and the KeyPurposeId id-kp-clientAuth ({{Section 4.2.1.12 of RFC5280}}) can be used to identify that the certificate is for a TLS WWW client. However, there are currently no KeyPurposeIds for usage with X.509 certificates for safety-critical communication.

This document addresses the above problems by defining keyPurposeIds for the EKU extension of X.509 public key certificates. These certificates are either used for signing files (general-purpose configuration and trust anchor configuration files, software and firmware update packages) or are used for safety-critical communication.

Vendor-defined KeyPurposeIds used within a PKI governed by vendors typically do not pose interoperability concerns, as non-critical extensions can be safely ignored if unrecognized. However, using KeyPurposeIds outside of their intended vendor-controlled environment or in ExtendedKeyUsage extensions that have been marked critical can lead to interoperability issues. Therefore, it is advisable not to rely on vendor-defined KeyPurposeIds. Instead, this specification defines standard KeyPurposeIds to ensure interoperability across various vendors and industries.

The definitions of theses KeyPurposeIds are intentionally broad to allow their use in different deployments even though they were initially motivated by industrial automation and rail automation, see {{UseCases}}. The details for each deployment needs to be described in the relevant technical standards and certificate policies.


# Conventions and Definitions {#conventions}

{::boilerplate bcp14-tagged}

This document uses terms defined in [RFC5280]. X.509 certificate extensions are defined using ASN.1 [X.680] and [X.690].

The term 'safety-critical communication' refers to communication that could, under certain conditions, lead to a state in which human life, health, property, or the environment is endangered. For the definition of 'safety' see [NIST_Glossary] and [ISO.IEC.IEEE_12207].


# Extended Key Purpose for configuration files, update packages and safety-communication {#EKU}

This specification defines the KeyPurposeIds id-kp-configSigning, id-kp-trustAnchorConfigSigning, id-kp-updatePackageSigning, and id-kp-safetyCommunication. These KeyPurposeIds are used, respectively, for: signing general-purpose configuration files or trust anchor configuration files, signing software or firmware update packages, or authenticating communication peers for safety-critical communication. As described in {{Section 4.2.1.12 of RFC5280}}, "\[i\]f the \[extended key usage\] extension is present, then the certificate MUST only be used for one of the purposes indicated" and "\[i\]f multiple \[key\] purposes are indicated the application need not recognize all purposes indicated, as long as the intended purpose is present".

None of the KeyPurposeIds specified in this document are intrinsically mutually exclusive.  Instead, the acceptable combinations of those KeyPurposeIds with others specified in this document and with other KeyPurposeIds specified elsewhere are left to the technical standards of the respective application and the certificate policy of the respective PKI.  For example, a technical standard may specify: 'Different keys and certificates must be used for safety communication and for trust anchor updates, and a relying party must ignore the KeyPurposeId id-kp-trustAnchorConfigSigning if id-kp-safetyCommunication is one of the specified key purposes in a certificate.' The certificate policy for example may specify: 'The id-kp-safetyCommunication KeyPuposeId should not be included in an issued certificate together with the KeyPurposeId id-kp-trustAnchorConfigSigning.' Technical standards and certificate policies of different applications may specify other rules.  Further considerations on prohibiting combinations of KeyPurposeIds is described in {{security}}.

Systems or applications that verify the signature of a general-purpose configuration file or trust anchor configuration file, the signature of a software or firmware update package, or the authentication of a communication peer for safety-critical communication SHOULD require that corresponding KeyPurposeIds be specified by the EKU extension. If the certificate requester knows the certificate users are mandated to use these KeyPurposeIds, it MUST enforce their inclusion. Additionally, such a certificate requester MUST ensure that the KeyUsage extension be set to digitalSignature for signature verification, to keyEncipherment for public key encryption, and keyAgreement for key agreement.


# Including the Extended Key Purpose in Certificates {#include-EKU}

{{RFC5280}} specifies the EKU X.509 certificate extension for use on end entity certificates. The extension indicates one or more purposes for which the certified public key is valid. The EKU extension can be used in conjunction with the Key Usage (KU) extension, which indicates the set of basic cryptographic operations for which the certified key may be used. The EKU extension syntax is repeated here for convenience:

~~~
   ExtKeyUsageSyntax  ::=  SEQUENCE SIZE (1..MAX) OF KeyPurposeId

   KeyPurposeId  ::=  OBJECT IDENTIFIER
~~~

As described in {{RFC5280}}, the EKU extension may, at the option of the certificate issuer, be either critical or non-critical. The inclusion of KeyPurposeIds id-kp-configSigning, id-kp-trustAnchorConfigSigning, id-kp-updatePackageSigning, and id-kp-safetyCommunication in a certificate indicates that the public key encoded in the certificate has been certified for the following usages:

* id-kp-configSigning

> A public key contained in a certificate containing the KeyPurposeId id-kp-configSigning may be used for verifying signatures of general-purpose configuration files of various formats (e.g., XML, YAML, or JSON). Configuration files are used to configure hardware or software.

* id-kp-trustAnchorConfigSigning

> A public key contained in a certificate containing the KeyPurposeId id-kp-trustAnchorConfigSigning may be used for verifying signatures of trust anchor configuration files of various formats (e.g., XML, YAML, or JSON).
> Trust anchor configuration files are used to add or remove trust anchors to the trust store of a device.

* id-kp-updatePackageSigning

> A public key contained in a certificate containing the KeyPurposeId id-kp-updatePackageSigning may be used for verifying signatures of software or firmware update packages. Update packages are used to install software (including bootloader, firmware, safety-related applications, and others) on systems.

* id-kp-safetyCommunication

> A public key contained in a certificate containing the KeyPurposeId id-kp-safetyCommunication may be used to authenticate a communication peer for safety-critical communication based on TLS or other protocols.

~~~
   id-kp  OBJECT IDENTIFIER  ::=
       { iso(1) identified-organization(3) dod(6) internet(1)
         security(5) mechanisms(5) pkix(7) 3 }

   id-kp-configSigning             OBJECT IDENTIFIER ::= { id-kp 41 }
   id-kp-trustAnchorConfigSigning  OBJECT IDENTIFIER ::= { id-kp 42 }
   id-kp-updatePackageSigning      OBJECT IDENTIFIER ::= { id-kp 43 }
   id-kp-safetyCommunication       OBJECT IDENTIFIER ::= { id-kp 44 }
~~~


# Implications for a Certification Authority {#ca-implication}

The procedures and practices employed by a certification authority must ensure that the correct values for the EKU extension as well as the KU extension are inserted in each certificate that is issued. The inclusion of the id-kp-configSigning, id-kp-trustAnchorConfigSigning, id-kp-updatePackageSigning, and id-kp-safetyCommunication KeyPurposeIds does not preclude the inclusion of other KeyPurposeIds.


# Security Considerations {#security}

The Security Considerations of {{RFC5280}} are applicable to this document. These extended key usage key purposes do not introduce new security risks but instead reduce existing security risks by providing the means to identify if a certificate is generated to verify the signature of a general-purpose or trust anchor configuration file, the signature of a software or firmware update package, or the authentication of a communication peer for safety-critical communication.

To reduce the risk of specific cross-protocol attacks, the relying party may additionally prohibit use of specific combinations of KeyPurposeIds.  The procedure for allowing or disallowing combinations of KeyPurposeIds using excluded KeyPurposeId and permitted KeyPurposeId, as carried out by a relying party, is defined in {{Section 4 of RFC9336}}.  The technical standards and certificate policies of the application should explicitly enumerate requirements for excluded or permitted KeyPurposeIds or their combinations. It is out of scope of this document to enumerate those, but an example of excluded KeyPurposeIds can be the presence of the anyExtendedKeyUsage KeyPurposeId. Examples of allowed KeyPurposeIds combinations can be the presence of id-kp-safetyCommunication together with id-kp-clientAuth or id-kp-serverAuth.

# Privacy Considerations {#privacy}

In some protocols, e.g., [TLS 1.2](#RFC5246), certificates are exchanged in the clear. In other protocols, e.g., [TLS 1.3](#RFC8446), the certificates are encrypted. The inclusion of the EKU extension can help an observer determine the purpose of the certificate. In addition, if the certificate is issued by a public certification authority, the inclusion of an EKU extension can help an attacker to monitor the Certificate Transparency logs {{RFC9162}} to identify the purpose of the certificate which may reveal private information of the certificate subject.


# IANA Considerations {#iana}

IANA is requested to register the following ASN.1 {{X.680}} module OID in the "SMI Security for PKIX Module Identifier" registry {{SMI-PKIX-MOD}}. This OID is defined in {{asn1}}.

| Decimal | Description                  | References |
|:--------|:-----------------------------|:-----------|
| TBD1    | id-mod-config-update-sc-eku  | This-RFC   |


IANA is also requested to register the following OIDs in the "SMI Security for PKIX Extended Key Purpose" registry {{SMI-PKIX-PURPOSE}}.  These OIDs are defined in {{include-EKU}}.

| Decimal | Description                    | References |
|:--------|:-------------------------------|:-----------|
| 41      | id-kp-configSigning            | This-RFC   |
| 42      | id-kp-trustAnchorConfigSigning | This-RFC   |
| 43      | id-kp-updatePackageSigning     | This-RFC   |
| 44      | id-kp-safetyCommunication      | This-RFC   |


# Acknowledgments {#acknow}

We would like to thank the authors of {{RFC9336}} and {{RFC9509}} for  their excellent template.

We also thank all reviewers of this document for their valuable feedback.


--- back

# ASN.1 Module {#asn1}

The following module adheres to ASN.1 specifications {{X.680}} and
{{X.690}}.

~~~ asn1
<CODE BEGINS>

Automation-EKU
  { iso(1) identified-organization(3) dod(6) internet(1)
    security(5) mechanisms(5) pkix(7) id-mod(0)
    id-mod-config-update-sc-eku (TBD1) }

DEFINITIONS IMPLICIT TAGS ::=
BEGIN

-- OID Arc

id-kp OBJECT IDENTIFIER ::=
  { iso(1) identified-organization(3) dod(6) internet(1)
    security(5) mechanisms(5) pkix(7) kp(3) }

-- Extended Key Usage Values

id-kp-configSigning            OBJECT IDENTIFIER ::= { id-kp 41 }
id-kp-trustAnchorConfigSigning OBJECT IDENTIFIER ::= { id-kp 42 }
id-kp-updatePackageSigning     OBJECT IDENTIFIER ::= { id-kp 43 }
id-kp-safetyCommunication      OBJECT IDENTIFIER ::= { id-kp 44 }

END


<CODE ENDS>
~~~

# Use Cases {#UseCases}

Automation hardware and software products strive to become more safe and secure by fulfilling mandatory, generic system requirements related to cyber security driven by federal offices like the [European Union Cyber Resilience Act](#EU-CRA) governed by the European Commission and the High Representative of the Union for Foreign Affairs and Security Policy.
Automation products connected to the Internet would bear the so-called [CE marking](#CE-marking) to indicate they comply.
Such regulation was announced in the [2020 EU Cybersecurity Strategy](#EU-STRATEGY), and complements other legislation in this area, specifically the NIS2 Framework, [Directive on measures for a high common level of cybersecurity across the Union](#NIS2).

2020 EU Cybersecurity Strategy suggests to implement and extend international standards such as the [Security for industrial automation and control systems - Part 4-2: Technical security requirements for IACS components](#IEC.62443-4-2) (IACS refers to industrial automation and control system) and the [Industrial communication networks - Network and system security - Part 3-3: System security requirements and security levels](#IEC.62443-3-3). Automation hardware and software products of diverse vendors that are connected on automation networks and the Internet can be used to build common automation solutions. Standardized attributes would allow transparency of security properties and interoperability for vendors in context of software and firmware updates, general-purpose configuration, trust anchor configuration, and safety communication.

A concrete example for Automation is a Rail Automation system. The [Europe's Rail Joint Undertaking System Pillar](#ERJU) delivers a unified operational concept and a functional, safe, and secure system architecture with system requirements for Rail Automation. The deliverables include due consideration of cyber security aspects based on the IEC 62443 series of standards, focused on the European railway network to which [Directive 2016/797 - Interoperability of the rail system within the EU](#Directive-2016/797) applies.

# History of Changes {#history}

[RFC Editor: Please remove this appendix in the release version of the document.]

Changes from 06 -> 07:

* Moved Section 1.1 to the Appendix

* Addressed DISCUSS items from Mohamed Boucadair and Paul Wouters

* Addressed AD review comments from Paul Wouters and Orie Steele

* Fixed some minor issues

* Updated reference of EU Rail specification to V1.0

Changes from 05 -> 06:

* Addressed AD review comments from Mike Bishop, Gorry Fairhurst, Andy Newton, Mohamed Boucadair, Erik Kline, and Eric Vyncke

Changes from 04 -> 05:

* Addressed SECDIR review comments from Carl Wallace

Changes from 03 -> 04:

* Addressed Deb's AD review comments (see "AD Comments on draft-ietf-lamps-automation-keyusages")

* Added early allocated OIDs

Changes from 02 -> 03:

* Rename id-kp-trustanchorSigning to id-kp-trustAnchorConfigSigning

* Rename id-kp-updateSigning to id-kp-updatePackageSigning

* Fixed some nits

Changes from 01 -> 02:

* Updates Sections 3 and 6 addressing last call comments (see "WG Last Call for draft-ietf-lamps-automation-keyusages-01")

Changes from 01 -> 02:

* Implemented the changes requested during WGLC


Changes from 00 -> 01:

* Fixed some minor nids and wording issues


draft-ietf-lamps-automation-keyusages version 00:

* Updated document and filename after WG adoption


Changes from 00 -> 01:

* Updated last paragraph of Section 1 addressing WG adoption comments by Rich and Russ

* Updated name and OID of ASN.1 module


draft-brockhaus-lamps-automation-keyusages version 00:

* Broadened the scope to general automation use case and use ERJU as an example.

* Fixed some nits reported.


draft-brockhaus-lamps-eu-rail-keyusages version 00:

* Initial version of the document following best practices from RFC 9336 and RFC 9509
