---
title: Overview
weight: 101
---
# FedRAMP OSCAL SSP Overview

This section provides a summary of several important concepts and details that apply to OSCAL-based FedRAMP SSP files.

The [*FedRAMP OSCAL Documentation*](/documentation) provides important concepts necessary for working with any OSCAL-based FedRAMP file. Familiarization with those concepts is important to understanding this guide.

## XML and JSON Formats

The examples provided here are in XML; however, FedRAMP accepts XML or JSON formatted OSCAL-based SSP files. NIST offers a utility that provides lossless conversion of OSCAL-compliant files between XML and JSON in either direction.

You may submit your SSP to FedRAMP using either format. If necessary, FedRAMP tools will convert the files for processing.

## SSP File Concepts

Unlike the traditional MS Word-based SSP, SAP, and SAR, the OSCAL-based versions of these files are designed to make information available through linkages, rather than duplicating information. In OSCAL, these linkages are established through import commands.

{{< figure src="/img/ssp-figure-1.png" title="OSCAL import linkage hierarchy." alt="Figure illustrating how the OSCAL catalog, profile, SSP, SAP, and SAR have linkages to the preceding OSCAL model. The POA&M model is omitted for simplicity." >}}

For example, the NIST control definitions and FedRAMP baseline content that normally appear in the SSP are defined in the FedRAMP profile and simply referenced by the SSP.

{{< figure src="/img/ssp-figure-2.png" title="Detailed overview of OSCAL models." alt="Figure showing the main contents of the OSCAL catalog, profile, SSP, SAP, SAR and POA&M models." >}}

For this reason, an OSCAL-based SSP points to the appropriate
OSCAL-based FedRAMP baseline as determined by the system's FIPS-199 impact level. Instead of duplicating control details, the OSCAL-based SSP simply points to the baseline content for information such as control definition statements, FedRAMP-added guidance, parameters, and FedRAMP-required parameter constraints.

### Resolved Profile Catalogs

The resolved profile catalog for each FedRAMP baseline is a pre-processing of the profile and catalog to produce the resulting data. This reduces overhead for tools by eliminating the need to open and follow references from the profile to the catalog. It also includes only the catalog information relevant to the baseline, reducing the overhead of opening a larger catalog.

Where available, tool developers have the option of following the links from the profile to the catalog as described above or using the resolved profile catalog.

Developers should be aware that at this time, catalogs and profiles remain relatively static. As OSCAL gains wider adoption, there is a risk that profiles and catalogs will become more dynamic, and a resolved profile catalog becomes more likely to be out of date. Early adopters may wish to start with the resolved profile catalog now, and plan to add functionality for the separate profile and catalog handling later in their product roadmap.

{{< figure src="/img/ssp-figure-3.png" title="FedRAMP SSP imports FedRAMP resolved profile catalog." alt="Figure showing that SSP reduces tool processing by importing the  resolved profile catalog for each FedRAMP baseline." >}}

For more information about resolved profile catalogs, see the [*Profile Resolution*](/documentation/general-concepts/profile-resolution/) section.

## OSCAL-based FedRAMP SSP Template

FedRAMP offers an OSCAL-based SSP shell file in both XML and JSON formats. This shell contains many of the FedRAMP required standards to help get you started. This document is intended to work in concert with that shell file. The OSCAL-based FedRAMP SSP Template is available in XML and JSON formats here:

-   OSCAL-based FedRAMP SSP Template (JSON Format):\
    [https://raw.githubusercontent.com/GSA/fedramp-automation/master/dist/content/rev5/templates/ssp/json/FedRAMP-SSP-OSCAL-Template.json](https://raw.githubusercontent.com/GSA/fedramp-automation/master/dist/content/rev5/templates/ssp/json/FedRAMP-SSP-OSCAL-Template.json)

-   OSCAL-based FedRAMP SSP Template (XML Format):\
    <https://github.com/GSA/fedramp-automation/raw/master/dist/content/rev5/templates/ssp/xml/FedRAMP-SSP-OSCAL-Template.xml>

## OSCAL's Minimum File Requirements

Every OSCAL-based FedRAMP SSP file must have a minimum set of required
fields/assemblies and must follow the OSCAL SSP core syntax found here:

[https://pages.nist.gov/OSCAL/documentation/schema/implementation-layer/ssp](https://pages.nist.gov/OSCAL/concepts/layer/implementation/ssp/)

## Importing the FedRAMP Baseline

OSCAL is designed for traceability. Because of this, the SSP is designed
to be linked to the FedRAMP baseline. Rather than duplicating content
from the baseline, the SSP is intended to reference the baseline content
itself.

Use the import-profile field to specify an existing OSCAL-based SSP. The
href flag may include any valid uniform resource identifier (URI),
including a relative path, absolute path, or URI fragment.

#### SSP Import Representation
{{< highlight xml "linenos=table" >}}
   <import-profile href="path/to/profile.xml" />

- OR -
   
   <import-profile href="#[uuid-value]" />

{{</ highlight >}}

#### XPath Queries
{{< highlight xml "linenos=table" >}}
  (SSP) URI to Baseline:
    /*/import-profile/@href
{{</ highlight >}}

If the value is a URI fragment, such as
#96445439-6ce1-4e22-beae-aa72cfe173d0, the value to the right of the
hashtag (#) is the universally unique identifier (UUID) value of a
resource in the SSP file's back-matter. Refer to the [*Attachments and Embedded Content*](/documentation/general-concepts/4-expressing-common-fedramp-template-elements-in-oscal/#attachments-and-embedded-content) section for guidance on handling.

#### SSP Back Matter Representation
{{< highlight xml "linenos=table" >}}
    <back-matter>
      <resource uuid="96445439-6ce1-4e22-beae-aa72cfe173d0">
          <title>FedRAMP Moderate Baseline</title>
          <prop name="type" value="baseline" />
          <!-- Specify the XML or JSON file location. Only one required. -->
          <rlink media-type="application/xml" href="./profile.xml" />
          <rlink media-type="application/json" href="./profile.json" />
      </resource>
  </back-matter>
{{</ highlight >}}

#### XPath Queries
{{< highlight xml "linenos=table" >}}
  (SSP) Referenced OSCAL-based FedRAMP Baseline XML: 
    /*/back-matter/resource[@uuid='96445439-6ce1-4e22-beae-aa72cfe173d0'] /rlink[@media-type='application/xml']/@href
  OR JSON:
    /*/back-matter/resource[@uuid='96445439-6ce1-4e22-beae-aa72cfe173d0'] /rlink[@media-type='application/json']/@href
{{</ highlight >}}

Note: Cloud Service Providers must import [FedRAMP profiles or resolved profile catalogs](https://github.com/GSA/fedramp-automation/tree/master/dist/content/rev5/baselines).

## Resolution Resource Prop

FedRAMP will be implementing a separate set of automated SSP validation rules for the rev 5 OSCAL templates. To ensure FedRAMP initiates the appropriate validation rules when processing OSCAL SSPs, SSP authors should add a new prop called `resolution-resource` in the metadata section and include an associated back-matter resource as shown below:

#### SSP Resolution Resource
{{< highlight xml "linenos=table" >}}
  <system-security-plan>
    <metadata>
        <title>FedRAMP System Security Plan (SSP)</title>
        <!-- cut -->
        <version>fedramp2.0.0-oscal1.0.4</version>
        <oscal-version>1.0.4</oscal-version>
        <revisions>
          <revision>
              <!-- cut -->
          </revision>
        </revisions>
        <!-- New rev 5 prop -->
        <prop ns="http://fedramp.gov/ns/oscal" name="resolution-resource"
          value="ace2963d-ecb4-4be5-bdd0-1f6fd7610f41" />
    </metadata>
    <!-- cut -->
    <back-matter>
        <resource uuid="ace2963d-ecb4-4be5-bdd0-1f6fd7610f41">
          <title>Resolution Resource</title>
          <prop name="dataset" class="collection" value="Special Publication"/>
          <prop name="dataset" class="name" value="800-53"/>
          <prop name="dataset" class="version" value="5.0.2"/>
          <prop name="dataset" class="organization" value="gov.nist.csrc"/>
          <remarks>
              <p>This "resolution resource" is used by FedRAMP as a local, authoritative indicator of what version SSP (rev 4 or rev 5) this OSCAL document is for.</p>
          </remarks>
        </resource>
    </back-matter>
  </system-security-plan>
{{</ highlight >}}

#### XPath Queries
{{< highlight xml "linenos=table" >}}
  (SSP) UUID of “resolution-resource”:
    /*/metadata/prop[@name=”resolution-resource”]/@value
  (SSP)Target baseline version:
    /*/back-matter/resource[@uuid=”uuid-of-resolution-resource”]/prop[@name=”dataset” and @class=”version”]/@value
{{</ highlight >}}

If the `resolution-resource` prop is not specified in the metadata section of the SSP, FedRAMP will assume the SSP should be validated using the rev 5 validation rules. If the `resolution-resource` prop is present, FedRAMP will use the validation rules that correspond with the version specified in the back-matter resource.
