# Schemas

YAML schema, examples, and validators for OpenControl format. You can find the formal definitions and learn about how to do validation in the [`kwalify/`](kwalify/) folder.

## Components

Components represent individual parts of an application that deal with specific security requirements. For example, in the [AWS compliance documentation](https://github.com/opencontrol/aws-compliance) the [EC2](https://github.com/opencontrol/aws-compliance/blob/master/IAM/component.yaml) component deals with access control and identity management security requirements. In the [Cloud Foundry compliance documentation](https://github.com/opencontrol/cf-compliance), the [UAA](https://github.com/opencontrol/cf-compliance/blob/master/UAA/component.yaml) the [Cloud Controller](https://github.com/opencontrol/cf-compliance/tree/master/CloudController) components deal with those requirements. In a straightforward Django-based application, for example, Django would be the component that deals with access control and identity management. As a developer building an SSP you most likely only deal with the component documentation.

### Structure

```yaml
name: Name of the component
key: Key of the component (defaults to the filename if not present)
documentation_complete: Manual check if the documentation is complete (for gap analysis)
schema_version: 3.0.0
references:
  - name: Name of the reference ie. EC2 website
    path: Relative path of local file or URL ie. diagrams/diagram-1.png
    type: Type of reference ie. Image, URL
  - name: Name of the reference ie. EC2 website
    path: Relative path of local file or URL ie. diagrams/diagram-1.png
    type: Type of reference ie. Image, URL
verifications:
  - key: Key of verification
    name: Name of verification
    path: Relative path of local file or URL ie. diagrams/diagram-1.png
    type: Type of reference ie. Image, URL
  - key: Key of verification
    name: Name of verification
    path: Relative path of local file or URL ie. diagrams/diagram-1.png
    type: Type of reference ie. Image, URL
satisfies:
  - standard_key: Standard Key (NIST-800-53)
    control_key: Control Key (CM-2)
    narrative:
      - key: The optional key that represents a particular section of the control. If the key is not specified, assume the string in the following text represents the entire control
        text: The narrative text for the particular section / entire control if there is no key specified
    implementation_status: Manual status of implementation (for gap analysis)
    control_origin: The text representing the control origination
    parameters:
     - key: "The key for a particular parameter of the specific control"
       text: "The parameter text for a particular parameter of a specific control"
    covered_by:
      - verification_key: The specific verification ID that the reference links, no component or system is needed for internal references
      - system_key: System name of the verification (can link to other systems / components)
        component_key: System name of the verification (can link to other systems / components)
        verification_key: The specific verification ID that the reference links to
```

## Standards

A standard is a list composed of individual security requirements called controls. The U.S. Government's main security standard is [NIST 800-53](https://web.nvd.nist.gov/view/800-53/home).

### Examples

```yaml
# nist-800-53.yaml
standards:
  C-2:
    name: User Access
    description: There is an affordance for managing access by...

# PCI.yaml
standards:
  Regulation-6:
    name: User Access PCI
    description: There is an affordance for managing access by...
```

## Certifications

Since standards can have thousands of security requirements (aka controls), agencies like the [GSA](http://www.gsa.gov/) or organizations such as [FedRAMP](https://www.fedramp.gov) have curated a list of controls they require in order grant an IT system Authority to Operate (ATO). The GSA, for example, developed a certification called [the Lightweight ATO (LATO)](https://gsablogs.gsa.gov/innovation/2014/12/10/it-security-security-in-an-agile-development-cloud-world-by-kurt-garbars/), which uses only 24 controls.

### Example

```yaml
# Fisma.yaml
standards:
  NIST-800-53:
    C-2:
    C-3:
  PCI:
    6:
```

## opencontrol.yaml

The `opencontrol.yaml` file defines an application's documentation configuration settings.

### Structure

```yaml
schema_version: "1.0.0" # 1.0.0 is the current opencontrol.yaml schema version
name: Project_Name # Name of the project
metadata:
  description: "A description of the system"
  maintainers:
    - maintainer_email@email.com
components: # A list of paths to components written in the opencontrol format for more information view: https://github.com/opencontrol/schemas
  - ./component-1
certifications: # An optional list of certifications for more information visit: https://github.com/opencontrol/schemas
  - ./cert-1.yaml
standards: # An optional list of standards for more information visit: https://github.com/opencontrol/schemas
  - ./standard-1.yaml
dependencies:
  certifications: # An optional list of certifications stored remotely
    - url: https://github.com/18F/GSA-Certifications
      revision: master
  systems:  # An optional list of repos that contain an opencontrol.yaml stored remotely
    - url: https://github.com/18F/cg-compliance
      revision: master
  standards:   # An optional list of remote repos containing standards info that contain an opencontrol.yaml
    - url: https://github.com/opencontrol/NIST-800-53-Standards
      revision: master
```
