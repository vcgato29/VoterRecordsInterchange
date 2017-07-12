<br><br>
# **Draft - NIST Public Working Group Voter Records Interchange (VRI) Common Data Format Specification**

<br>

This document is a draft specification on a common data format (CDF) for voter records interchange (VRI).  It has been developed by NIST and members of the Voting Interoperability Public Working Group.  It currently contains a brief introduction and overview of the supported use cases, as well as a description of the UML model/XML schema.  Future sections will cover the use cases in greater detail.  

The VRI specification currently supports voter registration records exchanges between online voter registration portals/centers and voter registration systems, using the NVRA and FPCA forms.  The specification potentially will support the following list of use cases:

* Digital VR applications transmitted within State OVR systems, or to state OVR systems by third party OVR systems, or by Motor Vehicle or other NVRA agencies, including digital versions of the NVRA form, the FPCA form, or state specific forms.

* Similar applications for voter registration update (change of name, change of address); absentee ballot request; change of voter status, or type of requested modification already supported in NVRA form, FPCA form, or state specific forms.

* Subsets of such digital applications used for 3rd-party registrars to transfer users and user data to state OVR systems, or for Motor Vehicle or other NVRA agencies to transfer customer data to state OVR systems.

* Subsets of such digital applications exchanged between state VR systems and DMV or similar systems, to perform driver's license data matching as part of OVR processing.

* Data exchanged by DMV or similar systems and VR systems, as part of NVRA compliance to digitally notify VR systems of DMV records of DMV customers that requested new voter registration; similar data push from DMV of existing DMV records recently updated with change-of-address, as part of semi-automated steps toward permanent voter registration; other forms of data exchange to VR systems that might facilitate elements of automatic and/or permanent voter registration.

* Data interchange between state VR systems for records matching (e.g., use of the ERIC system).

* Subsets of voter records externalized from voter records systems for purposes of data aggregation and reporting, including but not limited to EAVS reporting

The UML model and XML schema are expected to be finalized in time to be considered in the next VVSG under development by NIST and the Election Assistance Commission (EAC).

<br>

# Table of Contents

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Introduction](#introduction)
	- [Purpose](#purpose)
	- [Audience](#audience)
	- [Motivation and Methodology](#motivation-and-methodology)
- [Voter Records Interchange XML Schema](#voter-records-interchange-xml-schema)
	- [Schema Stylistic Conventions](#schema-stylistic-conventions)
	- [Imports](#imports)
	- [Roots](#roots)
	- [Enumerations](#enumerations)
		- [*The **AssertionValue** Enumeration*](#the-assertionvalue-enumeration)
		- [*The **BallotReceiptMethod** Enumeration*](#the-ballotreceiptmethod-enumeration)
		- [*The **ContactMethodType** Enumeration*](#the-contactmethodtype-enumeration)
		- [*The **IdentifierType** Enumeration*](#the-identifiertype-enumeration)
		- [*The **PhoneCapability** Enumeration*](#the-phonecapability-enumeration)
		- [*The **RegistrationError** Enumeration*](#the-registrationerror-enumeration)
		- [*The **RegistrationForm** Enumeration*](#the-registrationform-enumeration)
		- [*The **RegistrationHelperType** Enumeration*](#the-registrationhelpertype-enumeration)
		- [*The **RegistrationMethod** Enumeration*](#the-registrationmethod-enumeration)
		- [*The **RegistrationProxy** Enumeration*](#the-registrationproxy-enumeration)
		- [*The **RegistrationRequestType** Enumeration*](#the-registrationrequesttype-enumeration)
		- [*The **ReportingUnitType** Enumeration*](#the-reportingunittype-enumeration)
		- [*The **SignatureSource** Enumeration*](#the-signaturesource-enumeration)
		- [*The **SignatureType** Enumeration*](#the-signaturetype-enumeration)
		- [*The **SuccessAction** Enumeration*](#the-successaction-enumeration)
		- [*The **VoterClassificationType** Enumeration*](#the-voterclassificationtype-enumeration)
		- [*The **VoterIdType** Enumeration*](#the-voteridtype-enumeration)
	- [Interfaces](#interfaces)
	- [Classes (Elements)](#classes-elements)
		- [*The **AbsenteeBallotRequest** Element*](#the-absenteeballotrequest-element)
		- [*The **AdditionalInfo** Element*](#the-additionalinfo-element)
		- [*The **ContactMethod** Element/Extension Base*](#the-contactmethod-elementextension-base)
			- [*The **PhoneContactMethod** xsi:type*](#the-phonecontactmethod-xsitype)
		- [*The **ElectionAdministration** Element*](#the-electionadministration-element)
		- [*The **ExternalIdentifier** Element*](#the-externalidentifier-element)
		- [*The **File** Element/Extension Base*](#the-file-elementextension-base)
			- [*The **Image** xsi:type*](#the-image-xsitype)
		- [*The **LatLng** Element*](#the-latlng-element)
		- [*The **Location** Element*](#the-location-element)
		- [*The **Name> (PreviousName)** Element*](#the-name-previousname-element)
		- [*The **Party** Element*](#the-party-element)
		- [*The **RegistrationHelper** Element*](#the-registrationhelper-element)
		- [*The **RegistrationProxy** Element*](#the-registrationproxy-element)
		- [*The **Signature (PreviousSignature)** Element*](#the-signature-previoussignature-element)
		- [*The **VoterClassification** Element*](#the-voterclassification-element)
		- [*The **VoterId** Element*](#the-voterid-element)
		- [*The **VoterRecordsRequest** Element*](#the-voterrecordsrequest-element)
		- [*The **VoterRecordsResponse** Element/Extension Base*](#the-voterrecordsresponse-elementextension-base)
			- [*The **RegistrationAcknowledgement** xsi:type*](#the-registrationacknowledgement-xsitype)
			- [*The **RegistrationRejection** xsi:type*](#the-registrationrejection-xsitype)
			- [*The **RegistrationSuccess** xsi:type*](#the-registrationsuccess-xsitype)
		- [*The **VoterRegistration** Element*](#the-voterregistration-element)
- [Appendices](#appendices)
	- [Acronyms](#acronyms)
	- [Glossary](#glossary)
	- [References](#references)
	- [UML Class Diagrams](#uml-class-diagrams)
	- [File Download Locations](#file-download-locations)
	- [XML Schema](#xml-schema)

<!-- /TOC -->

<br>

# Introduction

This document is a specification for a common data format (CDF) for voter records data interchange related to registration. The specification includes a data model in UML (Unified Modeling Language) [1] that itemizes and defines the data involved in voter records data interchange related to registration. The XML format is derived from the UML model, as may be other non-XML formats such as JSON and CSV.

The primary features of this specification include:

* Major data elements and their attributes and associations are fully defined in a UML data model.

* The data model can be used to generate data formats for today’s voter records systems as well as for future systems to be developed.

* Election data and results can be reported at flexible levels from highly aggregated to very detailed.

Detailed instructions for implementation and use of the XML schema are also included.

## Purpose

The purpose of this specification is to provide a data interchange format for voter records, with particular but not exclusive focus on interchange related to voter registration, especially online voter registration. A goal for specification is to include not only a concrete representation in XML (for use by interoperating systems to exchange voter record data with high fidelity to the semantics of the UML model), but also examples in lighter-weight formats such as JSON and CSV.

Advantages of using this specification include:

* A ready data interchange format for online voter registration (OVR) systems, removing the need for individual OVR system development projects to define data models and formats.

* Use in OVR systems currently being built, with early use and feedback on the CDF to inform continuing development of the CDF for additional use cases.

* Enabling software reference implementations of externalization and internalization of data in a CDF, promoting software re-use, and reduced efforts in development of systems that exchange voter record data.

Early use in OVR systems is enabled by the surge in OVR activities following broad interest in OVR adoption following the PCEA Final Report recommendations. Early experience with draft CDFs for data exchange (between state registration systems and DMV systems) should better inform ongoing work on a complete set of CDFs for all the uses cases in the sub-group’s scope, described below.

## Audience

The intended audience of this specification includes election officials, VR system designers and developers, as well as others in the election community including the general public. Some background in election administration or technology is useful in understanding the material in this specification.

## Motivation and Methodology

This document was motivated primarily to reduce the inherent diversity for U.S. election officials in exchanging data related to voter registration. The current varying systems involved and data produced often do not interoperate, adding more complexity to the process. Additionally, there are sometimes significant variations among different jurisdictions within a state as well among the states themselves in the way they automate the voter registration and related parts of voter record management.

NIST and a community of U.S. election officials, analysts, and voting system technologists analyzed varying VR scenarios and their associated data interchanges, to analyze existing practices and to create a standard data interchange format for emerging OVR systems. From this preliminary analysis, 9 use cases were developed:

1.	Digital NVRA Submission: Digital VR applications forms transmitted within state OVR systems, or to state OVR systems by third party OVR systems, following the format of the NVRA voter registration application form.

2.	Digital FPCA Submission: Similar transmissions in OVR processes, following the format of the FPCA form.

3.	Digital State OVR Submission: Similar transmissions in OVR processes, following the format of a state-specific form.

4.	Digital VR Update Submission: Similar application forms including: voter registration update (change of name, change of address); change of voter status; absentee ballot request.

5.	OVR Transfer: Subsets of such digital applications used for 3rd-party OVR registrars to transfer users and user data to state OVR systems.

6.	DMV Match: Subsets of such digital applications exchanged between state VR systems and DMV or similar systems, to perform driver's license data matching as part of OVR processing.

7.	DMV Notification: Data exchanged by DMV or similar systems and VR systems, as part of NVRA compliance to digitally notify VR systems of DMV records of DMV customers that requested voter registration. May also include: similar data push from DMV of existing DMV records recently updated with change-of-address, as part of semi-automated steps toward permanent voter registration; or other forms of data exchange to VR systems that might facilitate elements of automatic and/or permanent voter registration.

8.	Cross-State Records Match: Data interchange between state VR systems for and systems for records matching, e.g. the ERIC system, or as part of inter-state cross-check activities.

9.	EAVS Submission: Subsets of voter records externalized from voter records systems for purposes of data aggregation and reporting, including but not limited to EAVS reporting.

The initial focus of work the first use case, because it is the common basis of current efforts to develop new OVR systems, as an increasing number of states pass legislation enabling or requiring it. This initial focus was intended to quickly establish a baseline abstract data model as the basis for extension in later work on other use cases.

A UML data model was subsequently generated to represent the data associated with the first use case, and to show how the data elements are related and organized. Finally, an XML schema was generated from the UML data model. The XML schema defines the rules of the XML format.

The advantages of using a UML data model as an intermediate step to generating an XML schema include that the model is independent of the concrete XML format (or other potential formats that could be derived); relationships between data elements are easier to correctly define and visualize when they are independent of any specific data format. If changes are needed to the XML format, one can make changes to the UML model and then generate a new version of the format using commercial products.

Note that this specification addresses U.S. governmental elections and is not intended for use “as is” in other types of elections or in other countries. However, the specification was written with the intention that it be adaptable to other election environments.

<br>

#	Voter Records Interchange XML Schema

This section contains documentation and discussion of the features included in the VRI XML schema.  

In the sections below, an XML element or enumeration name is denoted using
a fixed-font and angle brackets, e.g., `<ElectionReport>` or `<ReportingUnitType>`. Attributes, enumeration values, or other XML syntax are in a fixed-font, e.g., `label` or `geo-json`.  An element is sometimes referred to as a "sub-element" when it is included in another element, e.g., `<Election>` is a sub-element of
`<ElectionReport>`.  "Includes" is used to denote that an element contains another
element as a sub-element, e.g., `<ElectionReport>` includes `<Election>`.  

<br>

##	Schema Stylistic Conventions

The XML schema was written observing the following stylistic conventions:

*	Element, attribute, enumeration, and primitive names observe variations of
"CamelCase" conventions , that is:
	*	Element and enumeration names begin with a capital letter and the
names that consist of multiple words are concatenated and each word
begins with a capital letter, thus "CamelCase".  For example,
`<VoterRegistration>`.
	*	Attribute names begin with a non-capital (lower-case) letter and the
names that consist of multiple words are concatenated, with the first
word beginning in a non-capital letter and subsequent words
beginning in a capital letter, thus "camelCase".  For example,
`mimeType`.
*	Enumeration value names are in non-capital letters, and names that consist
of multiple words are separated by hyphens.  For example, `1-of-n`.
*	Element, attribute, and enumeration value ordering is generally alphabetical,
with the following exceptions:
	*	`<EndDate>` follows element names such as `<StartDate>`, or
`<OtherType>` follows `<Type>`.
	*	If there is an enumeration value of `other`, it comes last in the list of
values.

<br>

## Imports

The schema (and instance files) imports two external schemas:

1.	The W3C digital signature schema, used in the optional `<Signature>` sub-element of `<VoterRecordsRequest>` and `<VoterRecordsResponse>` to
include a digital signature on XML instance files.
2.	The Federal Geographic Data Committee (FGDC) address schema [10],
which contains 11 types of addresses that are used to specify postal and
registration addresses for voters, used in the `<VoterRegistration>` and
other elements.

Schema Definition:

     <!--  ========== Imports ==========  -->
     <xsd:import namespace=http://www.fgdc.gov/schemas/address/addr schemaLocation=
      "addr.xsd"/>
     <xsd:import namespace=http://www.w3.org/2000/09/xmldsig# schemaLocation=
      "http://www.w3.org/2000/09/xmldsig#"/>

<br>

## Roots

The schema contains two root elements:

1.	`<VoterRecordsRequest>`, used as a root for registration request transactions.
2.	`<VoterRecordsResponse>`, used as a root for registration response
transactions.

Schema Definition:

     <!--  ========== Roots ==========  -->
     <xsd:element name="VoterRecordsRequest" type="VoterRecordsRequest"/>
     <xsd:element name="VoterRecordsResponse" type="VoterRecordsResponse"/>

<br>

## Enumerations

The following sections deal with the enumerations (i.e., simple types) in the
schema, which are generated from the enumerations in the UML models.

<br>

### *The **AssertionValue** Enumeration*

Used in request transactions.  

Enumeration for assertions from a voter or a third party as a department of motor
vehicles (DMV) in response to questions on a registration form, used in the
`<Assertion>` sub-element of `<VoterClassification>`.

Value | Definition
--- | ---
`no` | For a voter's assertion of "no" or "false".
`yes` | For a voter's assertion of "yes" or "true".
`unknown` | For a voter's assertion of "unknown".

Schema Definition:

    <xsd:simpleType name="AssertionValue">
        <xsd:restriction base="xsd:string">
            <xsd:enumeration value="no"/>
            <xsd:enumeration value="yes"/>
            <xsd:enumeration value="unknown"/>
        </xsd:restriction>
    </xsd:simpleType>

<br>
 
### *The **BallotReceiptMethod** Enumeration*
Used in request transactions.  

Enumeration for methods for delivering a ballot to the voter, used in the
`<BallotReceiptPreference>` sub-element of `<VoterRegistration>`.

Value | Definition
--- | ---
`email` | For email only.
`email-or-online` | For electronic mail or downloadable (this is vague, thus the separate values for email and online).
`fax` | For use of a fax.
`mail` | For postal mail.
`online` | For downloadable only.

Schema Definition:

    <xsd:simpleType name="BallotReceiptMethod">
        <xsd:restriction base="xsd:string">
						<xsd:enumeration value="email"/>
            <xsd:enumeration value="email-or-online"/>
            <xsd:enumeration value="fax"/>
            <xsd:enumeration value="mail"/>
						<xsd:enumeration value="online"/>
        </xsd:restriction>
    </xsd:simpleType>

<br>
 
### *The **ContactMethodType** Enumeration*
Used in request AND response transactions.  

Enumeration for methods for contacting a voter, used in the `<Type>` sub-element
of `<ContactMethod>`.

Value | Definition
--- | ---
`email` | For electronic mail.
`phone` | For use of a phone.
`other` | Used when the type of contact method is not included in this enumeration.

Schema Definition:

    <xsd:simpleType name="ContactMethodType">
        <xsd:restriction base="xsd:string">
            <xsd:enumeration value="email"/>
            <xsd:enumeration value="phone"/>
            <xsd:enumeration value="other"/>
        </xsd:restriction>
    </xsd:simpleType>

<br>
 
### *The **IdentifierType** Enumeration*
Used in request transactions.  

Enumeration for election data-related codes in the `<ExternalIdentifiers>`
element.

Value | Definition
--- | ---
`fips` | For FIPS codes.
`local-level` | For a code that is specific to a county or other similar locality.
`national-level`  | For a code that is used at the national level other than `ocd-id`.
`ocd-id` | For Open Civic Data identifiers.
`state-level` | For a code that is specific to a state.
`other` | Used when the type of code is not included in this enumeration.

Schema Definition:

    <xsd:simpleType name="IdentifierType">
        <xsd:restriction base="xsd:string">
            <xsd:enumeration value="fips"/>
            <xsd:enumeration value="local-level"/>
            <xsd:enumeration value="national-level"/>
            <xsd:enumeration value="ocd-id"/>
            <xsd:enumeration value="state-level"/>
            <xsd:enumeration value="other"/>
        </xsd:restriction>
    </xsd:simpleType>

<br>
 
### *The **PhoneCapability** Enumeration*
Used in request AND response transactions.  

Enumeration for telephone capabilities, used in the `<Capability>` sub-element of
`<PhoneContactMethod>`.

Value | Definition
--- | ---
`fax` | For telephones that include facsimile capabilities.
`mms` | For telephones that contain Multimedia Messaging Service (MMS) capabilities.
`sms` | For telephones that contain Short Messaging Service (SMS) capabilities.
`voice` | For telephones that contain voice capabilities.

Schema Definition:

    <xsd:simpleType name="PhoneCapability">
        <xsd:restriction base="xsd:string">
            <xsd:enumeration value="fax"/>
            <xsd:enumeration value="mms"/>
            <xsd:enumeration value="sms"/>
            <xsd:enumeration value="voice"/>
        </xsd:restriction>
    </xsd:simpleType>

<br>
 
###	*The **RegistrationError** Enumeration*
Used in response transactions.

Enumeration for registration-related errors, used in the `<Error>` sub-element of
`<RegistrationRejection>`.

Value | Definition
--- | ---
`identity-lookup-failed` | A lookup on the voter's identity failed.
`incomplete` | The registration request is incomplete.
`incomplete-address` | An address is incomplete.
`incomplete-name` | The voter's name is incomplete.
`ineligible` | The voter is ineligible to be registered.
`invalid-form` | The registration form specified is invalid.
`no-birth-date` | The registration request does not contain a birthdate.
`no-signature` | The registration request does not contain a signature.
`other` | Used when the type of error is not included in this** Enumeration*.

Schema Definition:

    <xsd:simpleType name="RegistrationError">
        <xsd:restriction base="xsd:string">
            <xsd:enumeration value="identity-lookup-failed"/>
            <xsd:enumeration value="incomplete"/>
            <xsd:enumeration value="incomplete-address"/>
            <xsd:enumeration value="incomplete-name"/>
            <xsd:enumeration value="ineligible"/>
            <xsd:enumeration value="invalid-form"/>
            <xsd:enumeration value="no-birth-date"/>
            <xsd:enumeration value="no-signature"/>
            <xsd:enumeration value="other"/>
        </xsd:restriction>

<br>

### *The **RegistrationForm** Enumeration*
Used in request transactions.  

Enumeration for types of registration forms, used in the `<RegistrationForm>` sub-element of `<VoterRecordsRequest>`.

Value | Definition
--- | ---
`fpca` | For the Federal Post Card Application form.
`nvra` | For the National Voter Registration Act form.
`other` | Used when the type of form is not included in this** Enumeration*.

Schema Definition:

    <xsd:simpleType name="RegistrationForm">
        <xsd:restriction base="xsd:string">
            <xsd:enumeration value="fpca"/>
            <xsd:enumeration value="nvra"/>
            <xsd:enumeration value="other"/>
        </xsd:restriction>
    </xsd:simpleType>

<br>
		 
### *The **RegistrationHelperType** Enumeration*
Used in request transactions.  

Enumeration for types of registration helpers, used in the `<Type>` sub-element of
`<RegistrationHelper>`.

Value | Definition
--- | ---
`assistant` | For a registration assistant.
`witness` | For a witness, only.

Schema Definition:

    <xsd:simpleType name="RegistrationHelperType">
        <xsd:restriction base="xsd:string">
            <xsd:enumeration value="assistant"/>
            <xsd:enumeration value="witness"/>
        </xsd:restriction>
    </xsd:simpleType>

<br>

### *The **RegistrationMethod** Enumeration*
Used in request transactions.  

Enumeration for the method used by the voter to register, used in the
`<RegistrationMethod>` sub-element of `<VoterRegistration>`.

Value | Definition
--- | ---
`armed-forces-recruitment-office` | The voter assisted by an armed forces recruitment office.
`motor-vehicle-office` | The voter via an MVA/DMV.
`other-agency-designated-by-state` | The voter assisted by an unspecified state-designated agency.
`public-assistance-office` | The voter assisted by a public assistance office.
`registration-drive-from-advocacy-group-or-political-party` | The voter via a registration drive.
`state-funded-agency-serving-persons-with-disabilities` | The voter assisted by a state-designated agency serving persons with disabilities.
`voter-via-election-registrars-office` | The voter via an election or registrar's office.
`voter-via-email` | The voter via email.
`voter-via-fax` | The voter via fax.
`voter-via-internet` | The voter via the Internet, e.g., a website.
`voter-via-mail` | The voter via postal mail.
`unknown` |
`other` | Used when the type of method is not included in this enumeration.

Schema Definition:

    <xsd:simpleType name="RequestSourceType">
        <xsd:restriction base="xsd:string">
            <xsd:enumeration value="armed-forces-recruitment-office"/>
            <xsd:enumeration value="motor-vehicle-office"/>
            <xsd:enumeration value="other-agency-designated-by-state"/>
            <xsd:enumeration value="public-assistance-office"/>
            <xsd:enumeration value="registration-drive-from-advocacy-group-or-political-party"/>
            <xsd:enumeration value="state-funded-agency-serving-persons-with-disabilities"/>
            <xsd:enumeration value="voter-via-election-registrars-office"/>
            <xsd:enumeration value="voter-via-email"/>
            <xsd:enumeration value="voter-via-fax"/>
            <xsd:enumeration value="voter-via-internet"/>
            <xsd:enumeration value="voter-via-mail"/>
            <xsd:enumeration value="unknown"/>
            <xsd:enumeration value="other"/>
        </xsd:restriction>
    </xsd:simpleType>

<br>

### *The **RegistrationProxy** Enumeration*
Used in request transactions.  

Enumeration for the proxy used for the voter's registration request, used in the
`<Type>` sub-element of `<RegistrationProxy>`.

Value | Definition
--- | ---
`armed-forces-recruitment-office` | The voter assisted by an armed forces recruitment office.
`motor-vehicle-office` | The voter via an MVA/DMV.
`other-agency-designated-by-state` | The voter assisted by an unspecified state-designated agency.
`public-assistance-office` | The voter assisted by a public assistance office.
`registration-drive-from-advocacy-group-or-political-party` | The voter via a registration drive.
`state-funded-agency-serving-persons-with-disabilities` | The voter assisted by a state-designated agency serving persons with disabilities.
`other` | Used when the type of source is not included in this** Enumeration*.

Schema Definition:

    <xsd:simpleType name="RegistrationProxyType">
        <xsd:restriction base="xsd:string">
            <xsd:enumeration value="armed-forces-recruitment-office"/>
            <xsd:enumeration value="motor-vehicle-office"/>
            <xsd:enumeration value="other-agency-designated-by-state"/>
            <xsd:enumeration value="public-assistance-office"/>
            <xsd:enumeration value="registration-drive-from-advocacy-group-or-political-party"/>
            <xsd:enumeration value="state-funded-agency-serving-persons-with-disabilities"/>
            <xsd:enumeration value="other"/>
        </xsd:restriction>
    </xsd:simpleType>

<br>

### *The **RegistrationRequestType** Enumeration*
Used in request transactions.  

Enumeration for the type of voter records request, used in the `<Type>` sub-element
of `<VoterRecordsRequest>`.

Value | Definition
--- | ---
`registration` | For a voter registration request.
`other` | Used when the type of request is not included in this enumeration.

Schema Definition:

    <xsd:simpleType name="RegistrationRequestType">
        <xsd:restriction base="xsd:string">
            <xsd:enumeration value="registration"/>
            <xsd:enumeration value="other"/>
        </xsd:restriction>
    </xsd:simpleType>

<br>

### *The **ReportingUnitType** Enumeration*
Used in request AND response transactions.  

Enumeration for the type of geopolitical unit, used in the `<Type>` sub-element in
the `<ReportingUnit>` element .  

Value | Definition
--- | ---
`ballot-batch` | Used for reporting batches of ballots that may cross precinct boundaries.
`ballot-style-area` | Used for a ballot style areas generally composed of precincts
`borough` | Used in CT, NJ, PA, other states, and New York City for boroughs. For AK and LA, see `county`.
`city` | Used for a city that reports results and/or for the district that encompasses it.
`city-council` | Used for city council districts.
`combined-precinct` | Used for one or more precincts that have been combined for the purposes of reporting.  Used for "Ward" if "Ward" is used interchangeably with "CombinedPrecinct".
`congressional` | Used for U.S. Congressional districts.
`county` | Used for a county and/or for the district that encompasses it.  In AK, used for counties that are called boroughs.  In LA, used for parishes.
`county-council` | Used for county council districts.
`drop-box` | Used for a dropbox for absentee ballots.
`judicial` | Used for judicial districts.
`municipality` | Used as applicable for various units such as towns, townships, villages that report votes and/or for the district that encompasses it.
`polling-place` | Used for a polling place.
`precinct` | Used also for "Ward" or "District" when these terms are used interchangeably with "Precinct".
`school` | Used for a school district.
`special` | Used for a special district.
`split-precinct` | Used for splits of precincts.
`state` | Used for a state and/or for the district that encompasses it.
`state-house` | Used for a state house or assembly district.
`state-senate` | Used for a state senate district.
`town` | Used in some New England states as a type of municipality that reports votes and/or for the district that encompasses it.
`township` | Used in some mid-western states as a type of municipality that reports votes and/or for the district that encompasses it.
`utility` | Used for a utility district.
`village` | Used as a type of municipality that reports votes and/or for the district that encompasses it.
`vote-center` | Used for a vote center.
`ward` | Used for combinations or groupings of precincts or other units.
`water` | Used for a water district.
`other` | Used for other types of reporting units not included in this enumeration.

Schema Definition:

    <xsd:simpleType name="ReportingUnitType">
        <xsd:restriction base="xsd:string">
            <xsd:enumeration value="ballot-batch"/>
            <xsd:enumeration value="ballot-style-area"/>
            <xsd:enumeration value="borough"/>
            <xsd:enumeration value="city"/>
            <xsd:enumeration value="city-council"/>
            <xsd:enumeration value="combined-precinct"/>
            <xsd:enumeration value="congressional"/>
            <xsd:enumeration value="county"/>
            <xsd:enumeration value="county-council"/>
            <xsd:enumeration value="drop-box"/>
            <xsd:enumeration value="judicial"/>
            <xsd:enumeration value="municipality"/>
            <xsd:enumeration value="polling-place"/>
            <xsd:enumeration value="precinct"/>
            <xsd:enumeration value="school"/>
            <xsd:enumeration value="special"/>
            <xsd:enumeration value="split-precinct"/>
            <xsd:enumeration value="state"/>
            <xsd:enumeration value="state-house"/>
            <xsd:enumeration value="state-senate"/>
            <xsd:enumeration value="town"/>
            <xsd:enumeration value="township"/>
            <xsd:enumeration value="utility"/>
            <xsd:enumeration value="village"/>
            <xsd:enumeration value="vote-center"/>
            <xsd:enumeration value="ward"/>
            <xsd:enumeration value="water"/>
            <xsd:enumeration value="other"/>
        </xsd:restriction>
    </xsd:simpleType>

<br>

### *The **SignatureSource** Enumeration*
Used in request transactions.  

Enumeration for source of the voter's signature, used in the `<Source>` sub-
element of `<Signature>`.

Value | Definition
--- | ---
`dmv` | For the department of motor vehicles, motor vehicle authority.
`local` | For an unspecified local source.
`state` | For an unspecified state source.
`voter` | The voter has included a signature on the form.
`other` | Used when the source of the signature is not included in this enumeration.

Schema Definition:

    <xsd:simpleType name="Source">
        <xsd:restriction base="xsd:string">
            <xsd:enumeration value="dmv"/>
            <xsd:enumeration value="local"/>
            <xsd:enumeration value="state"/>
            <xsd:enumeration value="voter"/>
            <xsd:enumeration value="other"/>
        </xsd:restriction>
    </xsd:simpleType>

<br>

### *The **SignatureType** Enumeration*
Used in request transactions.  

Enumeration for the type of voter signature, used in the `<Type>` sub-element of
`<Signature>`.

Value | Definition
--- | ---
`digital` | For a digital signature.
`dynamic` | For use with biometrics or other artifacts captured as part of the act of the voter signing the registration form.
`electronic` | For a facsimile of the signature applied to a marking surface, e.g., paper.
`other` | Used when the type of signature is not included in this enumeration.

Schema Definition:

    <xsd:simpleType name="SignatureType">
        <xsd:restriction base="xsd:string">
            <xsd:enumeration value="digital"/>
            <xsd:enumeration value="dynamic"/>
            <xsd:enumeration value="electronic"/>
            <xsd:enumeration value="other"/>
        </xsd:restriction>
    </xsd:simpleType>

<br>

### *The **SuccessAction** Enumeration*
Used in response transactions.

Enumeration for a response to a voter records request, indicating that the response
to the request is successful and the action that occurred, used in the `<Action>` sub-element of `<RegistrationSuccess>`.  The success action may not necessarily match the requested action, as noted in section 1.6.20.3).

Value | Definition
--- | ---
`address-updated` | For indicating that an address was updated.
`name-updated` | For indicating that a name was updated.
`registration-cancelled` | For indicating that a registration was cancelled.
`registration-created` | For indicating that a registration was created.
`registration-updated` | For indicating that a registration was updated.
`status-updated` | For indicating that a registration status was updated.
`other` | Used for other types of success actions not included in this enumeration.

Schema Definition:

    <xsd:simpleType name="SuccessAction">
        <xsd:restriction base="xsd:string">
            <xsd:enumeration value="address-updated"/>
            <xsd:enumeration value="name-updated"/>
            <xsd:enumeration value="registration-cancelled"/>
            <xsd:enumeration value="registration-created"/>
            <xsd:enumeration value="registration-updated"/>
            <xsd:enumeration value="status-updated"/>
            <xsd:enumeration value="other"/>
        </xsd:restriction>
    </xsd:simpleType>

<br>

### *The **VoterClassificationType** Enumeration*
Used in request transactions.  

Enumeration for voter status classifications, used in the `<Type>` sub-element of
`<VoterClassification>`. Whether the voter status, e.g., `18-on-election-day`, is
true, false, or unknown depends on the value of the `<Assertion>` sub-element.

Value | Definition
--- | ---
`active-duty` | The voter is a member of the Uniformed Services or Merchant Marine on active duty (FPCA)
`active-duty-spouse-or-dependent` | OR the voter is an elgible spouse or dependent (FPCA).
`activated-national-guard` | The voter is an activated National Guard member on State orders (FPCA).
`citizen-abroad-intent-to-return` | The voter is a US citizen residing outside the US and has intention to return.
`citizen-abroad-return-uncertain` | The voter is a US citizen residing outside the US and their return is uncertain (FPCA).
`citizen-abroad-never-resided` | The voter is a US citizen and has never resided in the US (FPCA).
`eighteen-on-election-day` | The voter will be 18 on election day (NVRA).
`deceased` | The voter is deceased (NVRA).
`declared-incompetent` | The voter has been declared incompetent (NVRA).
`disabled` | The voter is disabled (NVRA).
`non-felon` | The voter is not a felon (NVRA).
`permanently-denied` | The voter has not been permanently denied (NVRA).
`protected` | The voter status is protected (NVRA).
`restored-felon` | The voter is a restored felon (NVRA).
`united-states-citizen` | The voter is a United States citizen (NVRA).
`other` | Used when the type of voter classification is not included in this enumeration.

Schema Definition:

    <xsd:simpleType name="VoterClassificationType">
        <xsd:restriction base="xsd:string">
            <xsd:enumeration value="eighteen-on-election-day"/>
            <xsd:enumeration value="deceased"/>
            <xsd:enumeration value="declared-incompetent"/>
            <xsd:enumeration value="disabled"/>
            <xsd:enumeration value="non-felon"/>
            <xsd:enumeration value="permanently-denied"/>
            <xsd:enumeration value="protected"/>
            <xsd:enumeration value="restored-felon"/>
            <xsd:enumeration value="united-states-citizen"/>
            <xsd:enumeration value="other"/>
        </xsd:restriction>
    </xsd:simpleType>

<br>

### *The **VoterIdType** Enumeration*
Used in request transactions.  

Enumeration for the type of voter ID, used in the `<Type>` sub-element of
`<VoterId>`.

Value | Definition
--- | ---
`drivers-license` | Used for a driver's license.
`local-voter-registration-id` | Used for a local voter registration ID.
`ssn` | Used for a complete Social Security number.
`ssn4` | Used for the last four digits of a Social Security number.
`state-id` | Used for a state ID that is not a state voter registration ID.
`state-voter-registration-id` | Used for a state's voter registration ID.
`unspecified-document-with-name-and-address` | Used for a document that contains the voter's name and address, such as a utility bill.
`unspecified-document-with-photo-identification` | Used for a document that contains a photograph of the voter.
`unknown` |
`other` | Used when the type of ID is not included in this enumeration.

Schema Definition:

    <xsd:simpleType name="VoterIdType">
        <xsd:restriction base="xsd:string">
            <xsd:enumeration value="drivers-license"/>
            <xsd:enumeration value="local-voter-registration-id"/>
            <xsd:enumeration value="unspecified-document-with-name-and-address"/>
            <xsd:enumeration value="unspecified-document-with-photo-identification"/>
            <xsd:enumeration value="ssn"/>
            <xsd:enumeration value="ssn4"/>
            <xsd:enumeration value="state-id"/>
            <xsd:enumeration value="state-voter-registration-id"/>
            <xsd:enumeration value="unknown"/>
            <xsd:enumeration value="other"/>
        </xsd:restriction>
    </xsd:simpleType>

<br>

## Interfaces
The UML model includes an interface to the FGDC address schema, which permits
any one of the 11 address subtypes to be used in any of the address elements that
are of type `<Address>`.

Schema Definition:

    <xsd:group name="Address">
        <xsd:choice>
            <xsd:element name="CommunityAddress_type"
             type="addr:CommunityAddress_type"/>
            <xsd:element name="FourNumberAddressRange_type"
             type="addr:FourNumberAddressRange_type"/>
            <xsd:element name="GeneralAddressClass_type"
             type="addr:GeneralAddressClass_type"/>
            <xsd:element name="IntersectionAddress_type"
             type="addr:IntersectionAddress_type"/>
            <xsd:element name="LandmarkAddress_type"
             type="addr:LandmarkAddress_type"/>
            <xsd:element name="NumberedThoroughfareAddress_type"
             type="addr:NumberedThoroughfareAddress_type"/>
            <xsd:element name="TwoNumberAddressRange_type"
             type="addr:TwoNumberAddressRange_type"/>
            <xsd:element name="USPSGeneralDeliveryOffice_type"
             type="addr:USPSGeneralDeliveryOffice_type"/>
            <xsd:element name="USPSPostalDeliveryBox_type"
             type="addr:USPSPostalDeliveryBox_type"/>
            <xsd:element name="USPSPostalDeliveryRoute_type"
             type="addr:USPSPostalDeliveryRoute_type"/>
            <xsd:element name="UnnumberedThoroughfareAddress_type"
             type="addr:UnnumberedThoroughfareAddress_type"/>
        </xsd:choice>
    </xsd:group>

<br>

## Classes (Elements)
The following sections deal with the elements (i.e., complex types) in the schema,
which are generated from the UML model classes.

<br>

### *The **AbsenteeBallotRequest** Element*
Used in request transactions.  

`<VoterRecordsRequest>` optionally includes this element for absentee ballot
requests made with the FPCA form registration requests or made independently.

The `<Semantics>` sub-element is used to hold the type of absentee ballot request,
using the `AbsenteeBallotRequestSemantics` enumeration.

Element | Multiplicity | Type | Element Description
--- | :---: | --- | ---
`<Semantics>` | 1 | `AbsenteeBallotRequestSemantics` | The type of request, e.g., `request-ab-for-next-election`.
`<SpecifiedElection>` | 0 or more | `Election` | For specifying the election pertaining to the ballot request.

Schema definition:

    <xsd:complexType name="AbsenteeBallotRequest">
        <xsd:sequence>
            <xsd:element name="Semantics" type="AbsenteeBallotRequestSemantics"/>
            <xsd:element name="SpecifiedElection" type="Election" minOccurs="0" maxOccurs="unbounded"/>
        </xsd:sequence>
    </xsd:complexType>

<br>

### *The **AdditionalInfo** Element*
Used in request transactions.  

`<VoterRegistration>` optionally includes this element for specifying information
not addressed in this schema by other elements and attributes, e.g., state-specific
information that does not "fit" in any other element. The information will thus be
highly specific to the generating application, and consuming applications must
"know" the meaning of the information to make use of it.  For this reason, use of
this element is discouraged as much as is possible.  

The `<StringValue>` and `<FileValue>` sub-elements are both optional, however at
least one of them must be included.

Element | Multiplicity | Type | Element Description
--- | :---: | --- | ---
`<Name>` | 1 | `xsd:string` | Name of the value.
`<FileValue>` | 0 or 1 | `xsd:string` | Used if the value is in a file; contains the filename.
`<StringValue>` | 0 or 1 | `xsd:string` | Used if the value is a string; contains the string.

Schema definition:

    <xsd:complexType name="AdditionalInfo">
        <xsd:sequence>
            <xsd:element name="FileValue" type="File" minOccurs="0"/>
            <xsd:element name="Name" type="xsd:string"/>
            <xsd:element name="StringValue" type="xsd:string" minOccurs="0"/>
        </xsd:sequence>
    </xsd:complexType>

<br>

### *The **ContactMethod** Element/Extension Base*
Used in request and response transactions.

`<ElectionAdministration>` optionally includes this element to specify how to
contact the election administration.

`<VoterRegistration>` optionally includes this element to specify the method for
contacting a voter regarding the voter's registration request.  If the voter can be
contacted in multiple ways, the application creating the XML instance file should
order the occurrences of `<ContactMethod>` by priority.

The `<PhoneContactMethod>` element uses `<ContactMethod>` as an extension base, thus `<ContactMethod>` can be used with `xsi:type="PhoneContactMethod"` when the contact method is for a telephone and it is necessary to describe the capabilities of the telephone.  For example, to indicate that a telephone number can receive voice and text messages, the following could be used:

    <ContactMethod xsi:type="PhoneContactMethod">
        <Capability>sms</Capability>
        <Capability>voice</Capability>
        <Type>phone</Type>
        <Value>304-123-4567</Value>
    </ContactMethod>

The `<Capability>` sub-element is provided by the `<PhoneContactMethod>` element.

Attribute | Required | Type | Attribute Description
--- | :---: | --- | ---
`xsi:type="PhoneContactMethod"` | no | `xsi:type` | Used to describe capabilities of the telephone when contact method is for a telephone. See section 1.6.13 for additional elements specific to this usage.

<br>

Element | Multiplicity | Type | Element Description
--- | :---: | --- | ---
`<Type>` | 1 | `ContactMethodType` | The contact method type, e.g., `email` or `phone`.
`<OtherType>` | 0 or 1 | `xsd:string` | Used when `<ContactMethodType>` value is `other`.
`<Value>` | 1 | `xsd:string` | Contains an email address or phone number, etc.

Schema definition:

    <xsd:complexType name="ContactMethod">
        <xsd:sequence>
            <xsd:element name="OtherType" type="xsd:string" minOccurs="0"/>
            <xsd:element name="Type" type="ContactMethodType"/>
            <xsd:element name="Value" type="xsd:string"/>
        </xsd:sequence>
    </xsd:complexType>

<br>

#### *The **PhoneContactMethod** xsi:type*
Used in request and response transactions.  

`<RegistrationAssistant>`, and `<RegistrationProxy>` use this element to specify a telephone number as well as the capabilities of the telephone, e.g., `sms`, `fax`, etc.

`<PhoneContactMethod>` is an `xsi:type of <ContactMethod>`, i.e., it uses `<ContactMethod>` as an extension base.  Thus, the elements that include `<ContactMethod>` could use `xsi:type="PhoneContactMethod"`  as applicable.  An example, using sub-elements defined in `<ContactMethod>`, is as follows:

    <ContactMethod xsi:type="PhoneContactMethod">
        <Capability>sms</Capability>
        <Capability>voice</Capability>
        <Type>phone</Type>
        <Value>304-123-4567</Value>
    </PhoneContactMethod>

Element | Multiplicity | Type | Element Description
--- | :---: | --- | ---
`<Capability>` | 0 or more | `PhoneCapability` | Specifies the phone's capabilities, e.g., `fax`, `sms`.

Schema definition:

    <xsd:complexType name="PhoneContactMethod">
        <xsd:complexContent>
            <xsd:extension base="ContactMethod">
                <xsd:sequence>
                    <xsd:element name="Capability" type="PhoneCapability" minOccurs="0"
                     maxOccurs="unbounded"/>
                </xsd:sequence>
            </xsd:extension>
        </xsd:complexContent>
    </xsd:complexType>

<br>

### *The **ElectionAdministration** Element*
Used in response transactions.

`<ElectionAdministration>` optionally includes the `<ContactInformation>` element to specify contact information for the election authority.

`<ReportingUnit>` optionally includes this element to specify various information about an election authority.

Element | Multiplicity | Type | Element Description
--- | :---: | --- | ---
`<ContactMethod>` | 0 or more | `ContactMethod` | For including various contact information.
`<Location>` | 0 or 1 | `Location` | Location of the election authority.
`<Name>` | 0 or 1 | `xsd:string` | Name of the election authority.
`<Uri>` | 0 or more | `xsd:anyURI` | A URL for the election authority.

Schema Definition:

    <xsd:complexType name="ElectionAdministration">
        <xsd:sequence>
            <xsd:element name="Location" type="Location" minOccurs="0"/>
            <xsd:element name="ContactMethod" type="ContactMethod" minOccurs="0" maxOccurs="unbounded"/>
            <xsd:element name="Name" type="xsd:string" minOccurs="0"/>
            <xsd:element name="Uri" type="xsd:anyURI" minOccurs="0" maxOccurs="unbounded"/>
        </xsd:sequence>

<br>

### *The **ExternalIdentifier** Element*
Used in request AND response transactions.  

`<Party>` and `<ReportingUnit>` optionally include this element for associating a jurisdiction's
codes, i.e., identifiers, with political parties or geopolitical units such as counties, towns,
precincts, etc. Multiple occurrences of `<ExternalIdentifier>` can be used to associate
multiple codes, e.g., if there is a desire to associate multiple codes with an object such as state-
specific codes as well as OCD-IDs (Open Civic Data Identifiers [11]), as follows:

    <ExternalIdentifiers>
        <ExternalIdentifier>
            <Type>state-level</Type>
            <Value>54</Value>
        </ExternalIdentifier>
        <ExternalIdentifier>
            <Type>ocd-id</Type>
            <Value>ocd-division/country:us/state:wv</Value>
        </ExternalIdentifier>
    </ExternalIdentifiers>

If the type of identifier is not listed in enumeration `<IdentifierType>`, use `other` and include the type (that is not listed in the enumeration) in `OtherType`, e.g.,

    <ExternalIdentifier>
        <Type>other</Type>
        <Value>101-A</Value>
        <OtherType>Ohio County Precinct Numbers</OtherType>
    </ExternalIdentifier>

Element | Multiplicity | Type | Element Description
--- | :---: | --- | ---
`<Type>` | 1 | `IdentifierType` | An identifier type, e.g., FIPS.
`<OtherType>` | 0 or 1 | `xsd:string` | Used when `<IdentifierType>` value is `other`.
`<Value>` | 1 | `xsd:string` | The identifier used by the jurisdiction.

Schema definition:

    <xsd:complexType name="ExternalIdentifier">
        <xsd:sequence>
            <xsd:element name="OtherType" type="xsd:string" minOccurs="0"/>
            <xsd:element name="Type" type="IdentifierType"/>
            <xsd:element name="Value" type="xsd:string"/>
        </xsd:sequence>
    </xsd:complexType>

<br>

### *The **File** Element/Extension Base*
Used in request transactions.  

`<VoterId>` optionally includes this element to specify a filename for voter identification
purposes such as for a utility bill. `<AdditionalInfo>` also optionally includes this element.  

`<File>` extends the `xsd:base64Binary` simple type to add the attributes for filename and (Multi-Purpose Internet Mail Extensions) MIME type, e.g., `application/pdf` for a file of type PDF.

The `<Image>` element uses this element as an extension base, thus `<File>` can be used with `xsi:type="Image"` when the type of file is for an image, e.g., `image/png`.

Attribute | Required | Type | Attribute Description
--- | :---: | --- | ---
`<fileName>` | no | `xsd:string` | The filename.
`<mimeType>` | no | `xsd:string` | The MIME type associated with the file.
`xsi:type="Image"` | no | `xsi:type` | Used if the type of file is for an image.

Schema definition:

    <xsd:complexType name="File">
        <xsd:simpleContent>
            <xsd:extension base="xsd:base64Binary">
                <xsd:attribute name="fileName" type="xsd:string"/>
                <xsd:attribute name="mimeType" type="xsd:string"/>
            </xsd:extension>
        </xsd:simpleContent>
    </xsd:complexType>

<br>

#### *The **Image** xsi:type*
Used in request transactions.  

`<Signature>` optionally includes this element to indicate that a file contains an image of a voter's signature.  `<Image>` uses `<File>` as an extension base, thus attributes and elements of `<File>` can be included in `<Image>`.

Schema definition:

    <xsd:complexType name="Image">
        <xsd:complexContent>
            <xsd:extension base="File"/>
        </xsd:complexContent>
    </xsd:complexType>

<br>

### *The **LatLng** Element*
Used in response transactions.  

`<Location>` optionally includes this element to specify the latitude and longitude of a voter's voting location.

Element | Multiplicity | Type | Element Description
--- | :---: | --- | ---
`<Latitude>` | 1 | `xsd:float` | Latitude of the contact location.
`<Longitude>` | 1 | `xsd:float` | Longitude of the contact location.
`<Source>` | 0 or 1 | `xsd:string` | System used to perform the lookup from location name to lat/lng, e.g., the name of a geocoding service.

Schema definition:

    <xsd:complexType name="LatLng">
        <xsd:sequence>
            <xsd:element name="Latitude" type="xsd:float"/>
            <xsd:element name="Longitude" type="xsd:float"/>
            <xsd:element name="Source" type="xsd:string" minOccurs="0"/>
        </xsd:sequence>
    </xsd:complexType>

<br>

### *The **Location** Element*
Used in response transactions.  

`<ReportingUnit>` optionally includes this element to specify the address and directions to a voter's voting location. The `<LatLng>` element can be included to specify the latitude and longitude of the voting location.

Element | Multiplicity | Type | Element Description
--- | :---: | --- | ---
`<Address>` | 0 or 1 | `Address` | Address of the voting location.
`<Directions>` | 0 or 1 | `xsd:string` | Directions to the voting location.
`<LatLng>` | 0 or 1 | `xsd:string` | Latitude/longitude of the voting location.

Schema definition:

    <xsd:complexType name="Location">
        <xsd:sequence>
            <xsd:element name="LatLng" type="LatLng" minOccurs="0"/>
            <xsd:element name="Address" minOccurs="0">
                <xsd:complexType>
                    <xsd:sequence>
                        <xsd:group ref="Address" minOccurs="0" maxOccurs="1"/>
                    </xsd:sequence>
                </xsd:complexType>
            </xsd:element>
            <xsd:element name="Directions" type="xsd:string" minOccurs="0"/>
        </xsd:sequence>
    </xsd:complexType>

<br>

### *The **Name> (PreviousName)** Element*
Used in request transactions.  

`<VoterRegistration>` includes this element for specifying the name of a voter and, optionally, for specifying a previous name of the voter, using `<PreviousName>` instead of `<Name>`.
`<MiddleName>` also includes this element for specifying the name of a registration helper.

Multiple occurrences of the `<MiddleName>` sub-element can be used as needed, e.g., for names
with additional middle names or nicknames such as "John Andrew Winston (Jack) Smith", as
follows:

    <Name>
        <FirstName>John</FirstName>
        <MiddleName>Andrew</MiddleName>
        <MiddleName>Winston</MiddleName>
        <MiddleName>(Jack)</MiddleName>
        <LastName>Smith</LastName>
    </Name>

Element | Multiplicity | Type | Element Description
--- | :---: | --- | ---
`<FirstName>` | 0 or 1 | `xsd:string` | Person's first (given) name.
`<FullName>` | 0 or 1 | `InternationalizedText` | Person's full name.
`<LastName>` | 0 or 1 | `xsd:string` | Person's last (family) name.
`<MiddleName>` | 0 or more | `xsd:string` | Person's middle name.
`<Prefix>` | 0 or 1 | `xsd:string` | A prefix associated with the person, e.g., Mr.
`<Suffix>` | 0 or 1 | `xsd:string` | A suffix associated with the person, e.g., Jr.

Schema definition:

    <xsd:complexType name="Name">
        <xsd:sequence>
            <xsd:element name="FirstName" type="xsd:string" minOccurs="0"/>
            <xsd:element name="FullName" type="xsd:string" minOccurs="0"/>
            <xsd:element name="LastName" type="xsd:string" minOccurs="0"/>
            <xsd:element name="MiddleName" type="xsd:string" minOccurs="0" maxOccurs="unbounded"/>
            <xsd:element name="Prefix" type="xsd:string" minOccurs="0"/>
            <xsd:element name="Suffix" type="xsd:string" minOccurs="0"/>
        </xsd:sequence>
    </xsd:complexType>

<br>

### *The **Party** Element*
Used in request transactions.  

`<VoterRegistration>` includes this element to specify a voter's political party.

Element | Multiplicity | Type | Element Description
--- | :---: | --- | ---
`<Abbreviation>` | 0 or 1 | `xsd:string` | Short name for the party, e.g., "DEM".
`<ExternalIdentifiers>` | 0 or 1 | `ExternalIdentifiers` | For associating an ID with the party.
`<Name>` | 1 | `xsd:string` | Official full name of the party, e.g., "Republican".

Schema Definition:

    <xsd:complexType name="Party">
        <xsd:sequence>
            <xsd:element name="Abbreviation" type="xsd:string" minOccurs="0"/>
            <xsd:element name="ExternalIdentifier" type="ExternalIdentifier"
             minOccurs="0" maxOccurs="unbounded"/>
            <xsd:element name="Name" type="xsd:string"/>
        </xsd:sequence>
    </xsd:complexType>

<br>

### *The **RegistrationHelper** Element*
Used in request transactions.  

`<VoterRegistration>` optionally includes this element to specify information about a registration assistant involved in a voter's registration request.

`<RegistrationAssistant>` includes the `<Name>` element to specify the registration assistant's name and optionally includes the `<Signature>` element if a registration assistant's signature is
required.

Element | Multiplicity | Type | Element Description
--- | :---: | --- | ---
`<Address>` | 0 or 1 | `Address` | Address of the registration assistant.
`<Name>` | 1 | `Name` | To specify the name of the assistant.
`<Phone>` | 0 or 1 | `PhoneContactMethod` | Registration assistant's phone number.
`<Signature>` | 0 or 1 | `Signature` | To specify the signature of the assistant.
`<Type>` | 1 | `RegistrationHelperType` | To specify the type of helper, e.g., `assistant`.

Schema definition:

    <xsd:complexType name="RegistrationHelper">
        <xsd:sequence>
            <xsd:element name="Name" type="Name" minOccurs="0"/>
            <xsd:element name="Signature" type="Signature" minOccurs="0"/>
            <xsd:element name="Address" minOccurs="0">
                <xsd:complexType>
                    <xsd:sequence>
                        <xsd:group ref="Address" minOccurs="0" maxOccurs="1"/>
                    </xsd:sequence>
                </xsd:complexType>
            </xsd:element>
            <xsd:element name="Phone" type="PhoneContactMethod" minOccurs="0"/>
            <xsd:element name="Type" type="RegistrationHelperType"/>
        </xsd:sequence>
    </xsd:complexType>

<br>

### *The **RegistrationProxy** Element*
Used in request transactions.  

`<VoterRegistration>` optionally includes this element to specify information about the proxy for a voter records request.

`<OriginTransactionId>` can be used to include an optional identifier of the originating external
transaction from the proxy, e.g., used for the transaction ID generated by a DMV application
enacting a voter registration request to a registration portal application (on behalf of a citizen
obtaining a driver's license). This sub-element is not to be confused with `<TransactionId>` in
`<VoterRecordsRequest>`, which is used to include a transaction ID of the voter records request,
e.g., the transaction ID of the registration portal's voter records request.

Element | Multiplicity | Type | Element Description
--- | :---: | --- | ---
`<Address>` | 0 or 1 | `Address` | An address associated with the proxy.
`<Name>` | 0 or 1 | `xsd:string` | A name associated with the proxy.
`<OriginTransactionId>` | 0 or 1 | `xsd:string` | An identifier associated with the transaction between the proxy and, e.g., the registration portal.
`<Phone>` | 0 or 1 | `PhoneContactMethod` | A phone number associated with the proxy.
`<TimeStamp>` | 0 or 1 | `xsd:date` | The date of the request from the proxy.
`<Type>` | 1 | `RegistrationProxyType` | The type of the requesting proxy, e.g., `motor-vehicle-office`, `voter-via-email`.
`<OtherType>` | 0 or 1 | `xsd:string` | Used when `<RegistrationProxyType>` value is other.

Schema definition:

    <xsd:complexType name="RegistrationProxy">
        <xsd:sequence>
            <xsd:element name="Address" minOccurs="0">
                <xsd:complexType>
                    <xsd:sequence>
                        <xsd:group ref="Address" minOccurs="0" maxOccurs="1"/>
                    </xsd:sequence>
                </xsd:complexType>
            </xsd:element>
            <xsd:element name="Name" type="xsd:string" minOccurs="0"/>
            <xsd:element name="OriginTransactionId" type="xsd:string" minOccurs="0"/>
            <xsd:element name="OtherType" type="xsd:string" minOccurs="0"/>
            <xsd:element name="Phone" type="PhoneContactMethod" minOccurs="0"/>
            <xsd:element name="TimeStamp" type="xsd:date" minOccurs="0"/>
            <xsd:element name="Type" type="RegistrationProxyType"/>
        </xsd:sequence>
    </xsd:complexType>

<br>

### *The **Signature (PreviousSignature)** Element*
Used in request transactions.  

`<VoterRegistration>` includes this element for specifying information about a voter's signature on a registration request. If there is a need to include previous signature that uses a different name, e.g., a maiden name, `<VoterRegistration>` uses `<PreviousSignature>`
instead of `<Signature>`.

`<Source>` is used to specify the source of the voter's signature, for example, on file at a department of motor vehicles. `<FileValue>` is used to include an image of the voter's signature.

Element | Multiplicity | Type | Element Description
--- | :---: | --- | ---
`<Date>` | 0 or 1 | `xsd:date` | The date of the signature, i.e., when created.
`<FileValue>` | 0 or 1 | `Image` | The signature image in base 64 binary.
`<Source>` | 0 or 1 | `SignatureSource` | A source for the signature, e.g., `dmv`.
`<OtherSource>` | 0 or 1 | `xsd:string` | Used when `<Source>` value is `other`.
`<Type>` | 0 or 1 | `SignatureType` | A signature type, e.g., `dynamic`.
`<OtherType>` | 0 or 1 | `xsd:string` | Used when `<SignatureType>` value is `other`.

Schema definition:

    <xsd:complexType name="Signature">
        <xsd:sequence>
            <xsd:element name="Date" type="xsd:date" minOccurs="0"/>
            <xsd:element name="FileValue" type="Image" minOccurs="0"/>
            <xsd:element name="OtherSource" type="xsd:string" minOccurs="0"/>
            <xsd:element name="OtherType" type="xsd:string" minOccurs="0"/>
            <xsd:element name="Source" type="SignatureSource" minOccurs="0"/>
            <xsd:element name="Type" type="SignatureType" minOccurs="0"/>
        </xsd:sequence>
    </xsd:complexType>

<br>

### *The **VoterClassification** Element*
Used in request transactions.  

`<VoterRegistration>` includes this element to describe a voter's classification per criteria on the voter's registration form, e.g., `united-states-citizen` or `18-on-election-day`.

`<VoterClassification>` includes assertions of the voter in response to the voter registration form criteria. For example, an assertion of `true` may be used with a criterion of united-states-citizen. Assertions can be negative, such as providing an assertion of `false` for a criterion of
felon, or an assertion of `unknown` to indicate that the voter does not know whether they meet or do not meet the specific criteria on the form.

Element | Multiplicity | Type | Element Description
--- | :---: | --- | ---
`<Assertion>` | 1 | `AssertionValue` | A positive or negative or unknown assertion.
`<Type>` | 1 | `VoterClassificationType` | A classification type, e.g., `disabled`.
`<OtherType>` | 0 or 1 | `xsd:string` | Used when `<VoterClassificationType>` value is `other`.

Schema definition:

    <xsd:complexType name="VoterClassification">
        <xsd:sequence>
            <xsd:element name="Assertion" type="AssertionValue"/>
            <xsd:element name="OtherType" type="xsd:string" minOccurs="0"/>
            <xsd:element name="Type" type="VoterClassificationType"/>
        </xsd:sequence>
    </xsd:complexType>

<br>

### *The **VoterId** Element*
Used in request transactions.  

Used to include information about a voter's identification that may be required in a registration request. `<VoterRegistration>` includes `<VoterId>`.

`<AttestNoSuchId>` is used to attest that the voter has no ID, thus if it is included, the value should be `false`. The `<StringValue>` and `<FileValue>` sub-elements are both optional, however at least one of them must be included.

Element | Multiplicity | Type | Element Description
--- | :---: | --- | ---
`<AttestNoSuchId>` | 0 or 1 | `xsd:boolean` | Used to attest that the voter has no ID. Assumed to be `false` if not present.
`<DateOfIssuance>` | 0 or 1 | `xsd:date` | Date the ID was issued.
`<FileValue>` | 0 or 1 | `File` | Used to include a file name for the ID.
`<StringValue>` | 0 or 1 | `xsd:string` | Used to include the ID as a string.
`<Type>` | 1 | `VoterIdType` | The type of voter ID
`<OtherType>` | 0 or 1 | `xsd:string` | Used when `<VoterIdType>` value is `other`.

Schema definition:

    <xsd:complexType name="VoterId">
        <xsd:sequence>
            <xsd:element name="AttestNoSuchId" type="xsd:boolean" minOccurs="0"/>
            <xsd:element name="DateOfIssuance" type="xsd:date" minOccurs="0"/>
            <xsd:element name="FileValue" type="File" minOccurs="0"/>
            <xsd:element name="StringValue" type="xsd:string" minOccurs="0"/>
            <xsd:element name="Type" type="VoterIdType"/>
            <xsd:element name="OtherType" type="xsd:string" minOccurs="0"/>
        </xsd:sequence>
    </xsd:complexType>

<br>

### *The **VoterRecordsRequest** Element*
The root element for request transactions.  

For defining items pertaining to the status and type of the voter records request and when it was
generated.  `<VoterRecordsRequest>` includes the `<VoterRegistration>` element to specify
various information about the voter in question. The optional `<Signature>` sub-element is
used for an XML digital signature [9] on XML instance files. `<Signature>` must be the last
sub-element of `<VoterRecordsRequest>`.

Element | Multiplicity | Type | Element Description
--- | :---: | --- | ---
`<GeneratedDate>` | 1 | `xsd:date` | The date that the voter records request was generated.
`<Issuer>` | 0 or 1 | `xsd:string` | The name of the issuer of the voter records request transaction, e.g., `State of West Virginia Voter Registration Portal`.
`<TransactionId>` | 0 or 1 | `xsd:string` | An identifier of the voter records request transaction.
`<Type>` | 1 or more | `RegistrationRequestType` | The type of request, e.g., `registration`.
`<OtherType>` | 0 or 1 | `xsd:string` | Used when `<RequestType>` value is `other`.
`<VendorApplicationId>` | 0 or 1 | `xsd:string` | An identifier of the vendor application generating the voter registration request, e.g., X-VRDB Version 3.1.a.
`<VoterRegistration>` | 1 | `VoterRegistration` | Specifies information about the voter who is the subject of the request.
`<Signature>` | 0 or 1 | `Signature` | Reference to the `<Signature>` element of the W3C digital signature schema imported into this schema.

Schema definition:

    <xsd:complexType name="VoterRecordsRequest">
        <xsd:sequence>
            <xsd:element name="VoterRegistration" type="VoterRegistration"/>
            <xsd:element name="GeneratedDate" type="xsd:date"/>
            <xsd:element name="Issuer" type="xsd:string" minOccurs="0"/>
            <xsd:element name="OtherType" type="xsd:string" minOccurs="0"/>
            <xsd:element ref="ds:Signature" minOccurs="0"/>
            <xsd:element name="TransactionId" type="xsd:string" minOccurs="0"/>
            <xsd:element name="Type" type="RegistrationRequestType" maxOccurs="unbounded"/>
            <xsd:element name="VendorApplicationId" type="xsd:string" minOccurs="0"/>
        </xsd:sequence>
    </xsd:complexType>
    <xsd:complexType name="VoterRecordsResponse" abstract="true">
        <xsd:sequence>
            <xsd:element name="ElectionAdministration" type="ElectionAdministration" minOccurs="0"/>
            <xsd:element ref="ds:Signature" minOccurs="0"/>
            <xsd:element name="TransactionId" type="xsd:string" minOccurs="0"/>
        </xsd:sequence>
    </xsd:complexType>

<br>

### *The **VoterRecordsResponse** Element/Extension Base*
The root element for response transactions.  

For defining items pertaining to the status of a response to a voter records request.  
`<VoterRecordsResponse>` is an abstract element with three `xsi:type`s that get used according to the type of response:

*	`<VoterRecordsResponse xsi:type="RegistrationAcknowledgement">`, used to indicate
an acknowledgement only 
*	`<VoterRecordsResponse xsi:type="RegistrationRejection">`, used to indicate a
failure and the type of failure
*	`<VoterRecordsResponse xsi:type="RegistrationSuccess">`, used to indication that a
successful registration action occurred and the type of registration action, which may
differ from the type of registration action requested 

`<VoterRecordsResponse>` optionally includes the `<TransactionId>` sub-element associated
with the voter records request.  The optional `<Signature>` sub-element is used for an XML
digital signature [9] on XML instance files. `<Signature>` must be the last sub-element of
`<VoterRecordsResponse>`.

Element | Multiplicity | Type | Element Description
--- | :---: | --- | ---
`<TransactionId>` | 0 or 1 | `xsd:string` | Transaction ID associated with the voter records request.
`<Signature>` | 0 or 1 | `Signature` | Reference to the `<Signature>` element of the W3C digital signature schema imported into this schema.

Schema definition:

    <xsd:complexType name="VoterRecordsResponse" abstract="true">
        <xsd:sequence>
            <xsd:element name="TransactionId" type="xsd:string" minOccurs="0"/>
            <xsd:element ref="ds:Signature" minOccurs="0"/>
        </xsd:sequence>
    </xsd:complexType>

<br>

#### *The **RegistrationAcknowledgement** xsi:type*
Used in response transactions.  

For indicating that the request was received but action on the request is pending.

Schema Definition:

    <xsd:complexType name="RegistrationAcknowledgement">
        <xsd:complexContent>
            <xsd:extension base="VoterRecordsResponse"/>
        </xsd:complexContent>
    </xsd:complexType>

<br>

#### *The **RegistrationRejection** xsi:type*
Used in response transactions.

For indicating that the request failed.  The `<Error>` sub-element is used to indicate the type of error that occurred. The `<AdditionalDetails>` sub-element can be used to provide more information as to the rejection.

Element | Multiplicity | Type | Element Description
--- | :---: | --- | ---
`<AdditionalDetails>` | 0 or more | `xsd:string` | Used to provide additional details as applicable.
`<Error>` | 0 or more | `RegistrationError` | Used to indicate the type of error.
`<OtherError>` | 0 or more | `xsd:string` | Used when `<RegistrationError>` value is `other`.

Schema definition:

    <xsd:complexType name="RegistrationRejection">
        <xsd:complexContent>
            <xsd:extension base="VoterRecordsResponse">
                <xsd:sequence>
                    <xsd:element name="AdditionalDetails" type="xsd:string" minOccurs="0"
                     maxOccurs="unbounded"/>
                    <xsd:element name="Error" type="RegistrationError" minOccurs="0"
                     maxOccurs="unbounded"/>
                    <xsd:element name="OtherError" type="xsd:string" minOccurs="0"
                     maxOccurs="unbounded"/>
                </xsd:sequence>
            </xsd:extension>
        </xsd:complexContent>
    </xsd:complexType>

<br>

#### *The **RegistrationSuccess** xsi:type*
Used in response transactions.

For indicating a successful response to a request.  The `<Action>` sub-element is used to
indicate the action that occurred, which may differ from what was requested.  For example, a
request for a new voter registration may succeed, but if the voter was already registered, the
response may indicate a registration update as opposed to a registration create.

The response also includes, optionally, information useful to the voter, including a description of
the voter's precinct and polling place, as well as the districts (i.e., contests) associated with the
precinct.

Element | Multiplicity | Type | Element Description
--- | :---: | --- | ---
`<Action>` | 0 or 1 | `SuccessAction` | Used to indicate the action that occurred.
`<OtherAction>` | 0 or 1 | `xsd:string` | Used when `<SuccessAction>` value is other.
`<Districts>` | 0 or more | `ReportingUnit` | The districts associated with the voter's precinct.
`<EffectiveDate>` | 0 or 1 | `xsd:date` | The effective date of the action.
`<PollingPlace>` | 0 or 1 | `ReportingUnit` | The voter's polling place.
`<Precinct>` | 0 or 1 | `ReportingUnit` | The voter's precinct.

Schema definition:

    <xsd:complexType name="RegistrationSuccess">
        <xsd:complexContent>
            <xsd:extension base="VoterRecordsResponse">
                <xsd:sequence>
                    <xsd:element name="Action" type="SuccessAction" minOccurs="0"
                     maxOccurs="unbounded"/>
                    <xsd:element name="OtherAction" type="xsd:string" minOccurs="0"
                     maxOccurs="1"/>
                    <xsd:element name="Districts" type="ReportingUnit" minOccurs="0"
                     maxOccurs="unbounded"/>
                    <xsd:element name="EffectiveDate" type="xsd:date" minOccurs="0"/>
                    <xsd:element name="PollingPlace" type="ReportingUnit" minOccurs="0"/>
                    <xsd:element name="Precinct" type="ReportingUnit" minOccurs="0"/>
                </xsd:sequence>
            </xsd:extension>
        </xsd:complexContent>
    </xsd:complexType>

<br>

### *The **VoterRegistration** Element*
Used in request transactions.  

`<VoterRecordsRequest>` includes this element to specify information about the voter.

All sub-elements are optional excepting `<Name>` and `<RegistrationAddress>` and
`<RegistrationMethod>`.  If the `<RegistrationAddressIsMailingAddress>` boolean is `true`,
`<MailingAddress>` need not be included.

Element | Multiplicity | Type | Element Description
--- | :---: | --- | ---
`<AdditionalInfo>` | 0 or more | `AdditionalInfo` | For including other information not specified by this schema.
`<BallotReceiptPreference>` | 0 or more | `BallotReceiptMethod` | The voter's preference on how to receive their ballot in order from their most preferred method to least, used if an absentee ballot request.
`<ContactMethod>` | 0 or more | `ContactMethod` | How to contact the voter, listed in order of preference.
`<DateOfBirth>` | 0 or 1 | `xsd:date` |
`<Ethnicity>` | 0 or 1 | `xsd:string` |
`<Gender>` | 0 or 1 | `xsd:string` | Older systems may not understand values other than 'Male' or 'Female' (the only choices available on FPCA).
`<LastDateOfUSResidency>` | 0 or 1 | `xsd:date` |
`<MailingAddress>` | 0 or 1 | `Address` | Where the voter receives postal mail.
`<Name>` | 1 | `Name` | Voter's name.
`OverseasEmployer` |  |  |
`<Party>` | 0 or 1 | `Party` | Voter's political party.
`<PreviousName>` | 0 or 1 | `Name` | A voter's previous name.
`<PreviousRegistrationAddress>` | 0 or 1 | `Address` | Where the voter was previously registered.
`<PreviousSignature>` | 0 or 1 | `Signature` | Information about a previous voter signature on the registration form.
`<RegistrationAddress>` | 1 | `Address` | Where the voter is registered or requests to be registered.
`<RegistrationAddressIsMailingAddress>` | 0 or 1 | `xsd:boolean` | If set to true, `<MailingAddress>` need not be included.
`<RegistrationForm>` | 0 or 1 | `RegistrationForm` | If the request is for a voter registration, the registration form used by the voter.
`<RegistrationHelper>` | 0 or 1 | `RegistrationHelper` | Included if the registration involves a registration assistant organization.
`<OtherRegistrationForm>` | 0 or 1 | `xsd:string` | Used when `<RegistrationForm>` value is other.
`<RegistrationMethod>` | 1 | `RegistrationMethod` | The method used by the voter to register.
`<OtherRegistrationForm>` | 0 or 1 | `xsd:string` | Used when `<RegistrationMethod>` value is other.
`<RegistrationProxy>` | 0 or 1 | `RegistrationMethod` | Included if the registration request is via a proxy, e.g., the DMV.
`<Signature>` | 0 or 1 | `Signature` | Information about the voter signature on the registration form.
`<VoterClassification>` | 0 or more | `VoterClassification` | How the voter is classified per assertions the voter has made on a registration form.
`<VoterId>` | 0 or more | `VoterId` | Information to provide voter identity.

Schema definition:

    <xsd:complexType name="VoterRegistration">
        <xsd:sequence>
            <xsd:element name="Party" type="Party" minOccurs="0"/>
            <xsd:element name="AdditionalInfo" type="AdditionalInfo" minOccurs="0" maxOccurs="unbounded"/>
            <xsd:element name="VoterId" type="VoterId" minOccurs="0" maxOccurs="unbounded"/>
            <xsd:element name="Name" type="Name"/>
            <xsd:element name="VoterClassification" type="VoterClassification" minOccurs="0" maxOccurs="unbounded"/>
            <xsd:element name="Signature" type="Signature" minOccurs="0"/>
            <xsd:element name="ContactMethod" type="ContactMethod" minOccurs="0" maxOccurs="unbounded">
                <xsd:annotation>
                    <xsd:documentation xml:lang="en">
                Contact methods, listed in order of contact preference.
             </xsd:documentation>
                </xsd:annotation>
            </xsd:element>
            <xsd:element name="RegistrationHelper" type="RegistrationHelper" minOccurs="0" maxOccurs="unbounded"/>
            <xsd:element name="RegistrationProxy" type="RegistrationProxy" minOccurs="0"/>
            <xsd:element name="BallotReceiptPreference" type="BallotReceiptMethod" minOccurs="0" maxOccurs="unbounded">
                <xsd:annotation>
                    <xsd:documentation xml:lang="en">
                     The voter's preference on how to receive their ballot in order from their most preferred method to least.
                     This property is only used if this request is an absentee ballot request.
             </xsd:documentation>
                </xsd:annotation>
            </xsd:element>
            <xsd:element name="DateOfBirth" type="xsd:date" minOccurs="0"/>
            <xsd:element name="Ethnicity" type="xsd:string" minOccurs="0"/>
            <xsd:element name="Gender" type="xsd:string" minOccurs="0">
                <xsd:annotation>
                    <xsd:documentation xml:lang="en">
                     Older systems may not understand values other than 'Male' or 'Female' (the only choices available on FPCA)
             </xsd:documentation>
                </xsd:annotation>
            </xsd:element>
            <xsd:element name="LastDateOfUSResidency" type="xsd:date" minOccurs="0"/>
            <xsd:element name="MailForwardingAddress" minOccurs="0">
                <xsd:complexType>
                    <xsd:sequence>
                        <xsd:group ref="Address" minOccurs="0" maxOccurs="1"/>
                    </xsd:sequence>
                </xsd:complexType>
            </xsd:element>
            <xsd:element name="MailingAddress" minOccurs="0">
                <xsd:complexType>
                    <xsd:sequence>
                        <xsd:group ref="Address" minOccurs="0" maxOccurs="1"/>
                    </xsd:sequence>
                </xsd:complexType>
            </xsd:element>
            <xsd:element name="OtherRegistrationForm" type="xsd:string" minOccurs="0"/>
            <xsd:element name="OtherRegistrationMethod" type="xsd:string" minOccurs="0"/>
            <xsd:element name="OverseasEmployer" type="xsd:string" minOccurs="0"/>
            <xsd:element name="PreviousName" type="Name" minOccurs="0"/>
            <xsd:element name="PreviousRegistrationAddress" minOccurs="0">
                <xsd:complexType>
                    <xsd:sequence>
                        <xsd:group ref="Address" minOccurs="0" maxOccurs="1"/>
                    </xsd:sequence>
                </xsd:complexType>
            </xsd:element>
            <xsd:element name="PreviousSignature" type="Signature" minOccurs="0"/>
            <xsd:element name="RegistrationAddress">
                <xsd:complexType>
                    <xsd:sequence>
                        <xsd:group ref="Address" minOccurs="1" maxOccurs="1"/>
                    </xsd:sequence>
                </xsd:complexType>
            </xsd:element>
            <xsd:element name="RegistrationAddressIsMailingAddress" type="xsd:boolean" minOccurs="0"/>
            <xsd:element name="RegistrationForm" type="RegistrationForm" minOccurs="0"/>
            <xsd:element name="RegistrationMethod" type="RegistrationMethod"/>
            <xsd:element name="SelectedLanguage" type="xsd:language" minOccurs="0">
                <xsd:annotation>
                    <xsd:documentation xml:lang="en">
                     The language specified by the voter, if any.
             </xsd:documentation>
                </xsd:annotation>
            </xsd:element>
        </xsd:sequence>
    </xsd:complexType>

<br>

# Appendices

## Acronyms

Selected acronyms used in this document are defined below.

Acronym | Meaning
--- | ---
**CDF** | Common Data Format
**EAC** | Election Assistance Commission
**EAVS** | EAC Election Administration and Voting Survey
**FIPS** | Federal Information Processing Standard
**FPCA** | Federal Post Card Application
**FWAB** | Federal Write-in Absentee Ballot
**JSON** | JavaScript Object Notation
**MMS** | Multimedia Messaging Service
**MIME** | Multipurpose Internet Mail Extensions
**NIST** | National Institute of Standards and Technology
**NVRA** | National Voter Registration Act
**OCD-ID** | Open Civic Data Identifiers
**SMS** | Short Message Service
**UML** | Unified Modeling Language
**UOCAVA** | Uniform and Overseas Citizens Assistance in Voting Act
**VVSG** | Voluntary Voting Systems Guidelines
**XML** | eXtensible Markup Language

<br>

## Glossary

Selected terms used throughout this document are defined below. In some of the definitions, there is ancillary information that is not part of the definition but helpful in understanding the definition; this ancillary information is preceded with "Note:".  Synonyms are preceded with "Syn:".

**Election official**:
Any county clerk and recorder, election judge, member of a
canvassing board, member of a board of county commissioners,
member or secretary of a board of directors authorized to conduct
public elections, representative of a governing body, or other person
contracting for or engaged in the performance of election duties as
required by the election code.

**Electoral district**:
As used in elections, administrative divisions in which voters are
entitled to vote in contests that are specific to that division, such as
those for state senators and delegates.

**Polling place**:
Location at which voters cast ballots in-person on vote-capture
devices (e.g., DRE) under the supervision of poll workers usually on
election day.  Syn: polling station or poll.  Note: A polling place is
typically in 1-to-1 correspondence with a precinct except for
combined precincts and vote centers.

**Precinct**:
An election administration division corresponding to a contiguous
geographic area that is the basis for determining the contests and
measures on which the voters legally residing in that area are eligible
to vote.

**Registration assistant**:
An organization whose purpose includes assisting voters in
registering to vote.

**Registration proxy**:
An organization that submits a voter registration request on behalf of
the voter, e.g., a DMV office that submits a voter registration request
for a voter.

**Registration witness**:
An individual who witnesses a voter's registration, i.e., the voter
signing his/her registration form.

**Reporting unit**:
An administrative division that reports votes or to which votes are
associated, e.g., state, county, city, precinct, etc.

**Schema**:
A file containing definitions of data elements and attributes with
rules for usage, e.g., for XML.

**UOCAVA voter**:
From the Uniform and Overseas Citizens Assistance in Voting Act
(UOCAVA); A U.S. citizen who is an active member of the
Uniformed Services and the Merchant Marine, or the commissioned
corps of the Public Health Service or the National Oceanic and
Atmospheric Administration, their eligible family members, and
U.S. citizens residing outside the United States.

<br>

## References

Election Assistance Commission, Election Administration and Voting Survey,
[http://www.eac.gov](http://www.eac.gov).

Federal Geographic Data Committee (FGDC), United States Thoroughfare,
Landmark, And Postal Address Data Standard,
[http://www.fgdc.gov/standards/projects/FGDC-standards-projects/address-data/index_html](http://www.fgdc.gov/standards/projects/FGDC-standards-projects/address-data/index_html).

Object Management Group (OMG), UML Specification version 1.1 (OMG
document ad/97-08-11) September 22, 2011, [http://omg.org/](http://omg.org/).

Open Civic Data, OCD Identifiers,
[http://opencivicdata.readthedocs.org/en/latest/ocdids.html](http://opencivicdata.readthedocs.org/en/latest/ocdids.html).

W3C, Extensible Markup Language (XML) 1.0 (Fifth Edition), W3C
Recommendation, November 26, 2008, [http://www.w3.org/TR/xml/](http://www.w3.org/TR/xml/).

W3C, XML Signature Syntax and Processing (Second Edition), W3C
Recommendation, June 10, 2008, [http://www.w3.org/TR/xmldsig-core/](http://www.w3.org/TR/xmldsig-core/).

<br>

## UML Class Diagrams

This appendix contains detailed images of the UML class diagrams that when viewed electronically can be expanded to show attributes and other details.  The images can also be
downloaded using the instructions in Appendix - File Download Locations.

[Voter Records Request UML Class Diagram](Figures/VoterRegistrationRequestV23.png "Voter Records Request UML class diagram")

[Voter Records Response UML Class Diagram](Figures/VoterRegistrationResponseV23.png "Voter Records Response UML class diagram")

[Interface to FGDC Address Types schema](Figures/addrxsd.png "Interface to FGDC address types schema")

<br>

## File Download Locations

The files associated with this specification are available for download from a NIST repository.  

These files are:

*	This specification,
*	XML schema,
*	Example XML files - TBD,
*	Validation tools - TBD, and
*	UML model.

Other files or updates to the files may be added.  The repository can be found via the following URL:

[http://vote.nist.gov](http://vote.nist.gov)

<br>

## XML Schema

        <?xml version="1.0" encoding="UTF-8"?>
        <xsd:schema elementFormDefault="qualified" targetNamespace="NIST_V1_voter_records_interchange.xsd" version="1.0"
         xmlns="NIST_V1_voter_records_interchange.xsd" xmlns:addr="http://www.fgdc.gov/schemas/address/addr"
				 xmlns:ds="http://www.w3.org/2000/09/xmldsig#" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
          <!-- ========== Imports ========== -->
          <xsd:import namespace="http://www.fgdc.gov/schemas/address/addr" schemaLocation="addr.xsd"/>
          <xsd:import namespace="http://www.w3.org/2000/09/xmldsig#" schemaLocation="http://www.w3.org/2000/09/xmldsig#"/>
          <!-- ========== Roots ========== -->
          <xsd:element name="VoterRecordsRequest" type="VoterRecordsRequest"/>
          <xsd:element name="VoterRecordsResponse" type="VoterRecordsResponse"/>
          <!-- ========== Primitives ========== -->
          <!-- ========== Enumerations ========== -->
          <xsd:simpleType name="AssertionValue">
        <xsd:restriction base="xsd:string">
          <xsd:enumeration value="no"/>
          <xsd:enumeration value="yes"/>
          <xsd:enumeration value="unknown"/>
        </xsd:restriction>
      </xsd:simpleType>
      <xsd:simpleType name="BallotReceiptMethod">
        <xsd:restriction base="xsd:string">
          <xsd:enumeration value="email"/>
          <xsd:enumeration value="email-or-online"/>
          <xsd:enumeration value="fax"/>
          <xsd:enumeration value="mail"/>
          <xsd:enumeration value="online"/>
        </xsd:restriction>
      </xsd:simpleType>
      <xsd:simpleType name="ContactMethodType">
        <xsd:restriction base="xsd:string">
          <xsd:enumeration value="email"/>
          <xsd:enumeration value="phone"/>
          <xsd:enumeration value="other"/>
        </xsd:restriction>
      </xsd:simpleType>
      <xsd:simpleType name="IdentifierType">
        <xsd:restriction base="xsd:string">
          <xsd:enumeration value="fips"/>
          <xsd:enumeration value="local-level"/>
          <xsd:enumeration value="national-level"/>
          <xsd:enumeration value="ocd-id"/>
          <xsd:enumeration value="state-level"/>
          <xsd:enumeration value="other"/>
        </xsd:restriction>
      </xsd:simpleType>
      <xsd:simpleType name="PhoneCapability">
        <xsd:restriction base="xsd:string">
          <xsd:enumeration value="fax"/>
          <xsd:enumeration value="mms"/>
          <xsd:enumeration value="sms"/>
          <xsd:enumeration value="voice"/>
        </xsd:restriction>
      </xsd:simpleType>
      <xsd:simpleType name="RegistrationError">
        <xsd:restriction base="xsd:string">
          <xsd:enumeration value="identity-lookup-failed"/>
          <xsd:enumeration value="incomplete"/>
          <xsd:enumeration value="incomplete-address"/>
          <xsd:enumeration value="incomplete-name"/>
          <xsd:enumeration value="ineligible"/>
          <xsd:enumeration value="invalid-form"/>
          <xsd:enumeration value="no-birth-date"/>
          <xsd:enumeration value="no-signature"/>
          <xsd:enumeration value="other"/>
        </xsd:restriction>
      </xsd:simpleType>
      <xsd:simpleType name="RegistrationForm">
        <xsd:restriction base="xsd:string">
          <xsd:enumeration value="fpca"/>
          <xsd:enumeration value="nvra"/>
          <xsd:enumeration value="other"/>
        </xsd:restriction>
      </xsd:simpleType>
      <xsd:simpleType name="RegistrationHelperType">
        <xsd:restriction base="xsd:string">
          <xsd:enumeration value="assistant"/>
          <xsd:enumeration value="witness"/>
        </xsd:restriction>
      </xsd:simpleType>
      <xsd:simpleType name="RegistrationMethod">
        <xsd:restriction base="xsd:string">
          <xsd:enumeration value="armed-forces-recruitment-office"/>
          <xsd:enumeration value="motor-vehicle-office"/>
          <xsd:enumeration value="other-agency-designated-by-state"/>
          <xsd:enumeration value="public-assistance-office"/>
          <xsd:enumeration value="registration-drive-from-advocacy-group-or-political-party"/>
          <xsd:enumeration value="state-funded-agency-serving-persons-with-disabilities"/>
          <xsd:enumeration value="voter-via-election-registrars-office"/>
          <xsd:enumeration value="voter-via-email"/>
          <xsd:enumeration value="voter-via-fax"/>
          <xsd:enumeration value="voter-via-internet"/>
          <xsd:enumeration value="voter-via-mail"/>
          <xsd:enumeration value="unknown"/>
          <xsd:enumeration value="other"/>
        </xsd:restriction>
      </xsd:simpleType>
      <xsd:simpleType name="RegistrationProxyType">
        <xsd:restriction base="xsd:string">
          <xsd:enumeration value="armed-forces-recruitment-office"/>
          <xsd:enumeration value="motor-vehicle-office"/>
          <xsd:enumeration value="other-agency-designated-by-state"/>
          <xsd:enumeration value="public-assistance-office"/>
          <xsd:enumeration value="registration-drive-from-advocacy-group-or-political-party"/>
          <xsd:enumeration value="state-funded-agency-serving-persons-with-disabilities"/>
          <xsd:enumeration value="other"/>
        </xsd:restriction>
      </xsd:simpleType>
      <xsd:simpleType name="RegistrationRequestType">
        <xsd:restriction base="xsd:string">
          <xsd:enumeration value="registration"/>
          <xsd:enumeration value="ballot-request"/>
          <xsd:enumeration value="other"/>
        </xsd:restriction>
      </xsd:simpleType>
      <xsd:simpleType name="ReportingUnitType">
        <xsd:restriction base="xsd:string">
          <xsd:enumeration value="ballot-batch"/>
          <xsd:enumeration value="ballot-style-area"/>
          <xsd:enumeration value="borough"/>
          <xsd:enumeration value="city"/>
          <xsd:enumeration value="city-council"/>
          <xsd:enumeration value="combined-precinct"/>
          <xsd:enumeration value="congressional"/>
          <xsd:enumeration value="county"/>
          <xsd:enumeration value="county-council"/>
          <xsd:enumeration value="drop-box"/>
          <xsd:enumeration value="judicial"/>
          <xsd:enumeration value="municipality"/>
          <xsd:enumeration value="polling-place"/>
          <xsd:enumeration value="precinct"/>
          <xsd:enumeration value="school"/>
          <xsd:enumeration value="special"/>
          <xsd:enumeration value="split-precinct"/>
          <xsd:enumeration value="state"/>
          <xsd:enumeration value="state-house"/>
          <xsd:enumeration value="state-senate"/>
          <xsd:enumeration value="town"/>
          <xsd:enumeration value="township"/>
          <xsd:enumeration value="utility"/>
          <xsd:enumeration value="village"/>
          <xsd:enumeration value="vote-center"/>
          <xsd:enumeration value="ward"/>
          <xsd:enumeration value="water"/>
          <xsd:enumeration value="other"/>
        </xsd:restriction>
      </xsd:simpleType>
      <xsd:simpleType name="SignatureSource">
        <xsd:restriction base="xsd:string">
          <xsd:enumeration value="dmv"/>
          <xsd:enumeration value="local"/>
          <xsd:enumeration value="state"/>
          <xsd:enumeration value="voter"/>
          <xsd:enumeration value="other"/>
        </xsd:restriction>
      </xsd:simpleType>
      <xsd:simpleType name="SignatureType">
        <xsd:restriction base="xsd:string">
          <xsd:enumeration value="digital"/>
          <xsd:enumeration value="dynamic"/>
          <xsd:enumeration value="electronic"/>
          <xsd:enumeration value="other"/>
        </xsd:restriction>
      </xsd:simpleType>
      <xsd:simpleType name="SuccessAction">
        <xsd:restriction base="xsd:string">
          <xsd:enumeration value="address-updated"/>
          <xsd:enumeration value="name-updated"/>
          <xsd:enumeration value="registration-cancelled"/>
          <xsd:enumeration value="registration-created"/>
          <xsd:enumeration value="registration-updated"/>
          <xsd:enumeration value="status-updated"/>
          <xsd:enumeration value="other"/>
        </xsd:restriction>
      </xsd:simpleType>
      <xsd:simpleType name="VoterClassificationType">
        <xsd:restriction base="xsd:string">
          <xsd:enumeration value="eighteen-on-election-day"/>
          <xsd:enumeration value="deceased"/>
          <xsd:enumeration value="declared-incompetent"/>
          <xsd:enumeration value="felon"/>
          <xsd:enumeration value="permanently-denied"/>
          <xsd:enumeration value="protected-voter"/>
          <xsd:enumeration value="restored-felon"/>
          <xsd:enumeration value="united-states-citizen"/>
          <xsd:enumeration value="other"/>
          <xsd:enumeration value="active-duty"/>
          <xsd:enumeration value="active-duty-spouse-or-dependent"/>
          <xsd:enumeration value="activated-national-guard"/>
          <xsd:enumeration value="citizen-abroad-intent-to-return"/>
          <xsd:enumeration value="citizen-abroad-return-uncertain"/>
          <xsd:enumeration value="citizen-abroad-never-resided"/>
        </xsd:restriction>
      </xsd:simpleType>
      <xsd:simpleType name="VoterIdType">
        <xsd:restriction base="xsd:string">
          <xsd:enumeration value="drivers-license"/>
          <xsd:enumeration value="local-voter-registration-id"/>
          <xsd:enumeration value="ssn"/>
          <xsd:enumeration value="ssn4"/>
          <xsd:enumeration value="state-id"/>
          <xsd:enumeration value="state-voter-registration-id"/>
          <xsd:enumeration value="unspecified-document-with-name-and-address"/>
          <xsd:enumeration value="unspecified-document-with-photo-identification"/>
          <xsd:enumeration value="unknown"/>
          <xsd:enumeration value="other"/>
        </xsd:restriction>
      </xsd:simpleType>
      <!-- ========== Interfaces Defined ========== -->
      <!-- === Interface Address === -->
      <xsd:group name="Address">
        <xsd:choice>
          <xsd:element name="CommunityAddress_type" type="addr:CommunityAddress_type"/>
          <xsd:element name="FourNumberAddressRange_type" type="addr:FourNumberAddressRange_type"/>
          <xsd:element name="GeneralAddressClass_type" type="addr:GeneralAddressClass_type"/>
          <xsd:element name="IntersectionAddress_type" type="addr:IntersectionAddress_type"/>
          <xsd:element name="LandmarkAddress_type" type="addr:LandmarkAddress_type"/>
          <xsd:element name="NumberedThoroughfareAddress_type" type="addr:NumberedThoroughfareAddress_type"/>
          <xsd:element name="TwoNumberAddressRange_type" type="addr:TwoNumberAddressRange_type"/>
          <xsd:element name="USPSGeneralDeliveryOffice_type" type="addr:USPSGeneralDeliveryOffice_type"/>
          <xsd:element name="USPSPostalDeliveryBox_type" type="addr:USPSPostalDeliveryBox_type"/>
          <xsd:element name="USPSPostalDeliveryRoute_type" type="addr:USPSPostalDeliveryRoute_type"/>
          <xsd:element name="UnnumberedThoroughfareAddress_type" type="addr:UnnumberedThoroughfareAddress_type"/>
        </xsd:choice>
      </xsd:group>
      <!-- ========== Interfaces Extended ========== -->
      <!-- ========== Classes ========== -->
      <xsd:complexType name="AdditionalInfo">
        <xsd:sequence>
          <xsd:element minOccurs="0" name="FileValue" type="File"/>
        </xsd:sequence>
        <xsd:attribute name="object_id" type="xsd:ID" use="required"/>
        <xsd:attribute name="Name" type="xsd:string" use="required"/>
        <xsd:attribute name="StringValue" type="xsd:string"/>
      </xsd:complexType>
      <xsd:complexType name="ContactMethod">
        <xsd:attribute name="object_id" type="xsd:ID" use="required"/>
        <xsd:attribute name="OtherType" type="xsd:string"/>
        <xsd:attribute name="Type" type="ContactMethodType" use="required"/>
        <xsd:attribute name="Value" type="xsd:string" use="required"/>
      </xsd:complexType>
      <xsd:complexType name="ElectionAdministration">
        <xsd:sequence>
          <xsd:element minOccurs="0" name="Location" type="Location"/>
          <xsd:element maxOccurs="unbounded" minOccurs="0" name="ContactMethod" type="ContactMethod"/>
          <xsd:element maxOccurs="unbounded" minOccurs="0" name="Uri" type="xsd:anyURI"/>
        </xsd:sequence>
        <xsd:attribute name="object_id" type="xsd:ID" use="required"/>
        <xsd:attribute name="Name" type="xsd:string"/>
      </xsd:complexType>
      <xsd:complexType name="ExternalIdentifier">
        <xsd:attribute name="object_id" type="xsd:ID" use="required"/>
        <xsd:attribute name="OtherType" type="xsd:string"/>
        <xsd:attribute name="Type" type="IdentifierType" use="required"/>
        <xsd:attribute name="Value" type="xsd:string" use="required"/>
      </xsd:complexType>
      <xsd:complexType name="File">
        <xsd:simpleContent>
          <xsd:extension base="xsd:base64Binary">
            <xsd:attribute name="fileName" type="xsd:string"/>
            <xsd:attribute name="mimeType" type="xsd:string"/>
          </xsd:extension>
        </xsd:simpleContent>
      </xsd:complexType>
      <xsd:complexType name="Image">
        <xsd:complexContent>
          <xsd:extension base="File">
          </xsd:extension>
        </xsd:complexContent>
      </xsd:complexType>
      <xsd:complexType name="LatLng">
        <xsd:attribute name="object_id" type="xsd:ID" use="required"/>
        <xsd:attribute name="Latitude" type="xsd:float" use="required"/>
        <xsd:attribute name="Longitude" type="xsd:float" use="required"/>
        <xsd:attribute name="Source" type="xsd:string"/>
      </xsd:complexType>
      <xsd:complexType name="Location">
        <xsd:sequence>
          <xsd:element minOccurs="0" name="LatLng" type="LatLng"/>
          <xsd:element minOccurs="0" name="Address">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:group maxOccurs="1" minOccurs="0" ref="Address"/>
              </xsd:sequence>
            </xsd:complexType>
          </xsd:element>
        </xsd:sequence>
        <xsd:attribute name="object_id" type="xsd:ID" use="required"/>
        <xsd:attribute name="Directions" type="xsd:string"/>
      </xsd:complexType>
      <xsd:complexType name="Name">
        <xsd:sequence>
          <xsd:element maxOccurs="unbounded" minOccurs="0" name="MiddleName" type="xsd:string"/>
        </xsd:sequence>
        <xsd:attribute name="object_id" type="xsd:ID" use="required"/>
        <xsd:attribute name="FirstName" type="xsd:string"/>
        <xsd:attribute name="FullName" type="xsd:string"/>
        <xsd:attribute name="LastName" type="xsd:string"/>
        <xsd:attribute name="Prefix" type="xsd:string"/>
        <xsd:attribute name="Suffix" type="xsd:string"/>
      </xsd:complexType>
      <xsd:complexType name="Party">
        <xsd:sequence>
          <xsd:element maxOccurs="unbounded" minOccurs="0" name="ExternalIdentifier" type="ExternalIdentifier"/>
        </xsd:sequence>
        <xsd:attribute name="object_id" type="xsd:ID" use="required"/>
        <xsd:attribute name="Abbreviation" type="xsd:string"/>
        <xsd:attribute name="Name" type="xsd:string" use="required"/>
      </xsd:complexType>
      <xsd:complexType name="PhoneContactMethod">
        <xsd:complexContent>
          <xsd:extension base="ContactMethod">
            <xsd:sequence>
              <xsd:element maxOccurs="unbounded" minOccurs="0" name="Capability" type="PhoneCapability"/>
            </xsd:sequence>
          </xsd:extension>
        </xsd:complexContent>
      </xsd:complexType>
      <xsd:complexType name="RegistrationAcknowledgement">
        <xsd:complexContent>
          <xsd:extension base="VoterRecordsResponse">
          </xsd:extension>
        </xsd:complexContent>
      </xsd:complexType>
      <xsd:complexType name="RegistrationHelper">
        <xsd:sequence>
          <xsd:element minOccurs="0" name="Name" type="Name"/>
          <xsd:element minOccurs="0" name="Signature" type="Signature"/>
          <xsd:element minOccurs="0" name="Address">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:group maxOccurs="1" minOccurs="0" ref="Address"/>
              </xsd:sequence>
            </xsd:complexType>
          </xsd:element>
          <xsd:element minOccurs="0" name="Phone" type="PhoneContactMethod"/>
        </xsd:sequence>
        <xsd:attribute name="object_id" type="xsd:ID" use="required"/>
        <xsd:attribute name="Type" type="RegistrationHelperType" use="required"/>
      </xsd:complexType>
      <xsd:complexType name="RegistrationProxy">
        <xsd:sequence>
          <xsd:element minOccurs="0" name="Address">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:group maxOccurs="1" minOccurs="0" ref="Address"/>
              </xsd:sequence>
            </xsd:complexType>
          </xsd:element>
          <xsd:element minOccurs="0" name="Phone" type="xsd:IDREF"/>
        </xsd:sequence>
        <xsd:attribute name="object_id" type="xsd:ID" use="required"/>
        <xsd:attribute name="Name" type="xsd:string"/>
        <xsd:attribute name="OriginTransactionId" type="xsd:string"/>
        <xsd:attribute name="OtherType" type="xsd:string"/>
        <xsd:attribute name="TimeStamp" type="xsd:date"/>
        <xsd:attribute name="Type" type="RegistrationProxyType" use="required"/>
      </xsd:complexType>
      <xsd:complexType name="RegistrationRejection">
        <xsd:complexContent>
          <xsd:extension base="VoterRecordsResponse">
            <xsd:sequence>
              <xsd:element maxOccurs="unbounded" minOccurs="0" name="AdditionalDetails" type="xsd:string"/>
              <xsd:element maxOccurs="unbounded" minOccurs="0" name="Error" type="RegistrationError"/>
              <xsd:element maxOccurs="unbounded" minOccurs="0" name="OtherError" type="xsd:string"/>
            </xsd:sequence>
          </xsd:extension>
        </xsd:complexContent>
      </xsd:complexType>
      <xsd:complexType name="RegistrationSuccess">
        <xsd:complexContent>
          <xsd:extension base="VoterRecordsResponse">
            <xsd:sequence>
              <xsd:element minOccurs="0" name="ElectionAdministration" type="ElectionAdministration"/>
              <xsd:element maxOccurs="unbounded" minOccurs="0" name="Action" type="SuccessAction"/>
              <xsd:element maxOccurs="unbounded" minOccurs="0" name="Districts" type="ReportingUnit"/>
              <xsd:element minOccurs="0" name="PollingPlace" type="xsd:IDREF"/>
              <xsd:element minOccurs="0" name="Precinct" type="xsd:IDREF"/>
            </xsd:sequence>
            <xsd:attribute name="EffectiveDate" type="xsd:date"/>
          </xsd:extension>
        </xsd:complexContent>
      </xsd:complexType>
      <xsd:complexType name="ReportingUnit">
        <xsd:sequence>
          <xsd:element minOccurs="0" name="Location" type="xsd:IDREF"/>
          <xsd:element maxOccurs="unbounded" minOccurs="0" name="ExternalIdentifier" type="xsd:IDREF"/>
        </xsd:sequence>
        <xsd:attribute name="object_id" type="xsd:ID" use="required"/>
        <xsd:attribute name="IsDistricted" type="xsd:boolean"/>
        <xsd:attribute name="Name" type="xsd:string"/>
        <xsd:attribute name="OtherType" type="xsd:string"/>
        <xsd:attribute name="Type" type="ReportingUnitType" use="required"/>
      </xsd:complexType>
      <xsd:complexType name="Signature">
        <xsd:sequence>
          <xsd:element minOccurs="0" name="FileValue" type="Image"/>
        </xsd:sequence>
        <xsd:attribute name="object_id" type="xsd:ID" use="required"/>
        <xsd:attribute name="Date" type="xsd:date"/>
        <xsd:attribute name="OtherSource" type="xsd:string"/>
        <xsd:attribute name="OtherType" type="xsd:string"/>
        <xsd:attribute name="Source" type="SignatureSource"/>
        <xsd:attribute name="Type" type="SignatureType"/>
      </xsd:complexType>
      <xsd:complexType name="VoterClassification">
        <xsd:attribute name="object_id" type="xsd:ID" use="required"/>
        <xsd:attribute name="Assertion" type="AssertionValue" use="required"/>
        <xsd:attribute name="OtherType" type="xsd:string"/>
        <xsd:attribute name="Type" type="VoterClassificationType" use="required"/>
      </xsd:complexType>
      <xsd:complexType name="VoterId">
        <xsd:sequence>
          <xsd:element minOccurs="0" name="FileValue" type="xsd:IDREF"/>
        </xsd:sequence>
        <xsd:attribute name="object_id" type="xsd:ID" use="required"/>
        <xsd:attribute name="AttestNoSuchId" type="xsd:boolean"/>
        <xsd:attribute name="DateOfIssuance" type="xsd:date"/>
        <xsd:attribute name="OtherType" type="xsd:string"/>
        <xsd:attribute name="StringValue" type="xsd:string"/>
        <xsd:attribute name="Type" type="VoterIdType" use="required"/>
      </xsd:complexType>
      <xsd:complexType name="VoterRecordsRequest">
        <xsd:sequence>
          <xsd:element name="VoterRegistration" type="VoterRegistration"/>
          <xsd:element minOccurs="0" ref="xsd:IDREF"/>
          <xsd:element maxOccurs="unbounded" name="Type" type="RegistrationRequestType"/>
        </xsd:sequence>
        <xsd:attribute name="object_id" type="xsd:ID" use="required"/>
        <xsd:attribute name="GeneratedDate" type="xsd:date" use="required"/>
        <xsd:attribute name="Issuer" type="xsd:string"/>
        <xsd:attribute name="OtherType" type="xsd:string"/>
        <xsd:attribute name="TransactionId" type="xsd:string"/>
        <xsd:attribute name="VendorApplicationId" type="xsd:string"/>
      </xsd:complexType>
      <xsd:complexType abstract="true" name="VoterRecordsResponse">
        <xsd:sequence>
          <xsd:element minOccurs="0" ref="xsd:IDREF"/>
        </xsd:sequence>
        <xsd:attribute name="object_id" type="xsd:ID" use="required"/>
        <xsd:attribute name="TransactionId" type="xsd:string"/>
      </xsd:complexType>
      <xsd:complexType name="VoterRegistration">
        <xsd:sequence>
          <xsd:element minOccurs="0" name="Party" type="Party"/>
          <xsd:element maxOccurs="unbounded" minOccurs="0" name="AdditionalInfo" type="AdditionalInfo"/>
          <xsd:element maxOccurs="unbounded" minOccurs="0" name="VoterId" type="VoterId"/>
          <xsd:element name="Name" type="xsd:IDREF"/>
          <xsd:element maxOccurs="unbounded" minOccurs="0" name="VoterClassification" type="VoterClassification"/>
          <xsd:element minOccurs="0" name="Signature" type="xsd:IDREF"/>
          <xsd:element maxOccurs="unbounded" minOccurs="0" name="ContactMethod" type="xsd:IDREF"/>
          <xsd:element maxOccurs="unbounded" minOccurs="0" name="RegistrationHelper" type="RegistrationHelper"/>
          <xsd:element minOccurs="0" name="RegistrationProxy" type="RegistrationProxy"/>
          <xsd:element maxOccurs="unbounded" minOccurs="0" name="BallotReceiptPreference" type="BallotReceiptMethod"/>
          <xsd:element minOccurs="0" name="MailForwardingAddress">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:group maxOccurs="1" minOccurs="0" ref="Address"/>
              </xsd:sequence>
            </xsd:complexType>
          </xsd:element>
          <xsd:element minOccurs="0" name="MailingAddress">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:group maxOccurs="1" minOccurs="0" ref="Address"/>
              </xsd:sequence>
            </xsd:complexType>
          </xsd:element>
          <xsd:element minOccurs="0" name="PreviousName" type="xsd:IDREF"/>
          <xsd:element minOccurs="0" name="PreviousRegistrationAddress">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:group maxOccurs="1" minOccurs="0" ref="Address"/>
              </xsd:sequence>
            </xsd:complexType>
          </xsd:element>
          <xsd:element minOccurs="0" name="PreviousSignature" type="xsd:IDREF"/>
          <xsd:element name="RegistrationAddress">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:group maxOccurs="1" minOccurs="1" ref="Address"/>
              </xsd:sequence>
            </xsd:complexType>
          </xsd:element>
        </xsd:sequence>
        <xsd:attribute name="object_id" type="xsd:ID" use="required"/>
        <xsd:attribute name="DateOfBirth" type="xsd:date"/>
        <xsd:attribute name="Ethnicity" type="xsd:string"/>
        <xsd:attribute name="Gender" type="xsd:string"/>
        <xsd:attribute name="LastDateOfUSResidency" type="xsd:date"/>
        <xsd:attribute name="OtherRegistrationForm" type="xsd:string"/>
        <xsd:attribute name="OtherRegistrationMethod" type="xsd:string"/>
        <xsd:attribute name="OverseasEmployer" type="xsd:string"/>
        <xsd:attribute name="RegistrationAddressIsMailingAddress" type="xsd:boolean"/>
        <xsd:attribute name="RegistrationForm" type="RegistrationForm"/>
        <xsd:attribute name="RegistrationMethod" type="RegistrationMethod" use="required"/>
        <xsd:attribute name="SelectedLanguage" type="xsd:language"/>
      </xsd:complexType>
    </xsd:schema>

<br>