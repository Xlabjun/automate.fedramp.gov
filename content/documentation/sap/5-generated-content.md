---
title: Generated Content
weight: 103
---
# Generating Content from OSCAL-based SAP

The following artifacts are historically generated by hand to summarize
content found in other FedRAMP-required content. When using OSCAL, these
artifacts can be generated from content found elsewhere. This includes
the:

-   **IP Addresses Slated for Testing**

-   **Databases Slated for Testing**

-   **Test Case Workbook**

If delivering FedRAMP content in OSCAL, assessors are no longer required
to manually generate and maintain these artifacts, provided the content
in their OSCAL-based FedRAMP SAP and the CSP\'s OSCAL-based FedRAMP SSP
remains accurate.

There are many ways a tool developer can generate these artifacts.
FedRAMP is developing Extensible Stylesheet Language Transformation
(XSLT) files to generate them. When ready, FedRAMP will make this freely
available to the public here:

[https://github.com/GSA/fedramp-automation/tree/master/dist/content/resources](https://github.com/GSA/fedramp-automation/tree/master/dist/content/resources)

**Tool developers are also encouraged to develop their own solutions to generating this content.**

### Generating the \"IP Addresses Slated for Testing\" List

The SAP must still identify the in-scope inventory items - either by
reference or using the \"all\" clause. Once identified, the list of IP
addresses slated for testing should be derived from the machine-readable
inventory found in the SSP.

As described in *Section 4.4.1, If No OSCAL-based SSP Exists or Has
Inaccurate Information (IP* Addresses), if the assessor finds SSP
information inventory to be missing or inaccurate, the SAP tool must
allow the assessor to insert inventory information into the
local-definitions section of the SAP.

### Generating the \"Databases Slated for Testing\" List

The SAP must still identify the in-scope inventory items - either by
reference or using the \"all\" clause. Once identified, the list of
Databases slated for testing should be derived from the machine-readable
inventory found in the SSP.

As described in *Section 4.4.1, If No OSCAL-based SSP Exists or Has
Inaccurate Information (IP* Addresses), if the assessor finds SSP
information inventory to be missing or inaccurate, the SAP tool must
allow the assessor to insert inventory information into the
local-definitions section of the SAP.