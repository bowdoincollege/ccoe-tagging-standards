# Bowdoin Tagging Standards

<!--TOC-->

- [Bowdoin Tagging Standards](#bowdoin-tagging-standards)
  - [Overview and Purpose](#overview-and-purpose)
  - [Conventions Used in this Document](#conventions-used-in-this-document)
  - [General Naming standard](#general-naming-standard)
    - [Keys](#keys)
    - [Values](#values)
  - [Defined Tags](#defined-tags)
    - [Name](#name)
    - [costcenter](#costcenter)
    - [environment](#environment)
    - [compliance](#compliance)
    - [classification](#classification)
    - [description](#description)
    - [documentation](#documentation)
    - [resourcegroup](#resourcegroup)
  - [Custom Tags](#custom-tags)
  - [Common Naming Components](#common-naming-components)
    - [Account Naming Construct](#account-naming-construct)
    - [Application Naming Construct](#application-naming-construct)
  - [Naming by resource type](#naming-by-resource-type)
    - [AWS resources](#aws-resources)
    - [Azure resources](#azure-resources)
    - [Definitions](#definitions)

<!--TOC-->

## Overview and Purpose

Cloud resources can have metadata assigned to them in the form of tags.
Each tab is a label consisting of a user-defined key and value.  Tags
are used to help manage, identify, organize, search for and filter
resources.  Tags can be created to categorize resources by purpose,
owner, environment, or other criteria.

Each tag has two parts:

1. A tag key (such as `costcenter` or `environment`).
1. A tag value (such as `1234556` or `prod`).

This document provides an overview and definition of tags required by
Bowdoin College and the standards for those keys and value that are
expected.  This is not meant to prevent other tags from being created
but intended to create a baseline of common tags.  Should a new tag
become required, it should be added to this document.

## Conventions Used in this Document

## General Naming standard

### Keys

In order to maintain consistency between platforms and different people
creating tags, the following naming standard is recommended for all
tags, defined or custom.

- Valid characters are `a-z`, `0-9`, and `-` (dash)
- Only lowercase letters will be used
- Spaces will not be used; a dash must not be used to separate words
  only sections of a tag value
- Namespaces will not be used to prefix Bowdoin tags;
  tags without a prefix are assumed to be in Bowdoin namespace
- Prefixes may be added to third party tags (i.e., AWS)
- A key should not be longer than 63 characters

### Values

- Unless specified, valid characters are `a-z`, `0-9`, and `-` (dash)
- Hungarian notation may be used with long names to help improve clarity
- Unrelated values will be placed in separate keys
- Compound tag values should not be used

## Defined Tags

### Name

Status: Required

This tag is a default tag for AWS resources, and has special behavior in
the AWS console.  Because of this, the key of this tag is an exception
to the naming rule in that it contains a capital letter.

The [value](#naming-by-resource-type) of the Name tag depends on its
resource type.

### costcenter

Status: Required

Reflects the Bowdoin Project Code from the financial system.  This is
used to identify and report portions of an overall invoice or billing
cycle that belong to a group on campus based on their financial project
number.  Shared resources (i.e., a common vnet/VPC), may be marked with
a project number from IT.  Important:  This is not used for chargeback,
but it is used for showback.  Valid Characters: Six digits, 0-9 matching
an existing project code in the Bowdoin financial system.

### environment

Status: Required

A deployment environment is a group of computer, network and
infrastructure configuration in which an application is deployed and
executed.  The values of this tag should reflect the current deployment
environments [policy][environments-policy].

Valid values:

- prod
- dev
- stage
- test
- experimental

[environments-policy]: https://confluence.bowdoin.edu/display/ITKB/Deployment+Environments

### compliance

Status: Required

For regulatory compliance, and will be (GDPR, PCI, HIPAA, etc.) as
defined by policy.

### classification

Status: Required

For data classification.  The value should be one of the options in our
data classification standard (Public, Sensitive, Restricted, etc.)

### description

Status: Recommended

A free form place to store a note about a resource that does not fit
into a defined tag or other process.  This tag is intended to be used by
humans and should be human readable and values should not be coded into
this space

This tag is not mandatory as it is expected that other tags should
provide descriptive information about what a resource is or what it
does.  This tag is provided if more information is needed and does not
fit elsewhere.

Valid Characters: Any that are valid to the system it is being used on.

### documentation

Status: Optional

This tag is a URL used to point to documentation.  At this time,
Confluence is our source of documentation, but it could a URL to another
source.

### resourcegroup

Status: Optional

Used to identify service, aka resource group which may end up being the
name.

Example: All the components of Veeam would have a common value in this
tag.

## Custom Tags

## Common Naming Components

### Account Naming Construct

`account_naming_construct` =
[`teamname`](#team-name)-[[`servicename`](#service-name)-][`environment`](#environment)

#### Team Name

`teamname`

- networking
- security
- sysinf
- entsys
- academic
- clientservices

#### Service Name

`servicename`

- www
- Foo
- var

#### Examples

- networking-prod
- entsys-dev
- academic-workspaces-prod

### Application Naming Construct

`appname_construct` =
[`appname`](#application-name)-[[`environment`](#environment)-][[`number`](#number-increment)-][[`region`](#region)]

#### Application Name

`appname`

all lowercase one word no dashes

#### Number Increment

`number`

start at 1

#### Region

use if multi region only

Examples:

- myduodevices
- sqlmanagedinstance-prod-1-eastus

#### Incremental Identifier

`increment`

- Use a zero padded number
- May contain sequential letters
- start at 01

## Naming by resource type

### AWS resources

#### VPC

[`account_naming_construct`](#account-naming-construct)-[`vpcpurpose`](#vpc-purpose)

##### VPC Purpose

`vpcpurpose`

all lowercase one word no dashes
This name needs to be determined in consultation with Networking.

Examples:

- networking-prod-sharedservices
- entsys-prod-nonhybrid
- academic-prod-dcsresearch

#### Subnet

[`account_naming_construct`](#account-naming-construct)-[`subnetpurpose`](#subnet-purpose)[-[`az`](#availability-zone)]

##### Subnet Purpose

`subnetpurpose`

all lowercase one word no dashes

##### Availability Zone

`az`

- a
- b

Examples:

- networking-prod-wvdstaff-a
- academic-workspaces-prod-workspaces-a
- networking-prod-sharedservices-public-a
- academic-prod-dcsresearch-public-a

#### VPC Peering Link

[`source_vpc_name`](#vpc)-[`target_vpc_name`](#vpc)

Note that there will be two resources, one on each end of the link.

Examples:

- networking-hub-something-foo (networking side of link)
- something-foo-networking-hub (something side of link)

#### Route Tables

[`vpc`](#vpc)-[`routetype`](#route-type)[-[`az`](#availability-zone)]

##### Route Type

`routetype`

- public
- private
- isolated

If the route table is distinct for each AZ (e.g. you are routing to
different NATs), you must add the following Zone - Zone should be #l
(e.g a, b, c, d, e)

Examples:

- networking-prod-sharedservices-vpc-public
- networking-prod-sharedservices-vpc-public-a

#### Security Groups

[`appname`](#application-name)-[`resource_type`](#resource-type)-[`sgpurpose`](#security-group-purpose)

##### Security Group Purpose

- ssh
- http
- http

##### Resource Type

- elb
- efs
- nfs
- instance
- db

Examples:

- dns
- sql
- banner-instance-443
- entsys-prod-nonhybrid-vpc-private

#### Network ACL

[`account_naming_construct`](#account-naming-construct)-[`naclpurpose`](#network-acl-purpose)[-[`nacldirection`](#network-acl-direction)]

##### Network ACL Purpose

- ssh
- http

##### Network ACL Direction

- in
- out

Examples:

- networking-prod-ping-in
- networking-test-http-in

#### EC2 Instance

[`appname`](#application-name)-[`service_type`](#service-type)-[`increment`](#incremental-identifier)

Examples:

- security-prod-sumologcollector-1
- entsys-prod-myduodevices-1
- networking-prod-s3proxy-1

#### Load Balancer

[`appname_construct`](#application-naming-construct)[-[`service_type`](#service-type)][-[`increment`](#incremental-identifier)]

#### Launch Configuration

[`appname_construct`](#application-naming-construct)[-[`service_type`](#service-type)][-[`increment`](#incremental-identifier)]

#### AutoScaling Group

[`appname_construct`](#application-naming-construct)[-[`service_type`](#service-type)][-[`increment`](#incremental-identifier)]

#### AMI (AWS Machine Image)

[`os_name`](#ami-os-name)-[`os_ver`](#ami-os-version)-[`function`](#ami-function)[-[`service_type`](#service-type)][-`unique_identifier`]

Future: Create image specific tags for OS, version, and architecture

Examples:

- rhel8base-web-012

##### AMI OS Name

`os_name`

- rhel
- centos
- ubuntu
- windows

##### AMI OS Version

`os_ver`

- 2012r2ent
- 10-{{build number}} i.e. 10-2004
- 7

##### AMI Function

- staffimage
- base

#### SSH Pem Keys

[`account_naming_construct`](#account-naming-construct)-[`appname_construct`](#application-naming-construct)

#### Database Service

[`appname_construct`](#application-naming-construct)-[`db_vendor`](#database-vendor)

Applies to PaaS and SaaS, not self installed on IaaS
Does not apply to databases inside the service

Examples:

- sqlmanagedinstance-dev-1-mssql
- common-dev-sqlmi

##### Database Vendor

- oracle
- mysql
- postgres
- maria
- aurora
- sql
- mssql
- access
- sqlmi

#### Lambda

[`appname_construct`](#application-naming-construct)-[`function_name`](#lambda-function-name)

Examples:

- enrollmentform-prod-updatelivedname

##### Lambda Function Name

Used when the function is part of a larger app.  It should describe what
the function does.

#### S3

[`account_naming_construct`](#account-naming-construct)-[`unique_identifier`](#unique-identifier)

Must be globally unique (due to DNS)
3-63 characters

##### Unique Identifier

could be [`appname_construct`](#application-naming-construct),
purpose, string, hash
This is up to the eye of the beholder.

#### EFS/EBS

`primary volume name`-disk[`increment`](#incremental-identifier)

Primary volumes will be named the same as EC2 instance.  Secondary
volumes use this convention.

#### IAM Users

[`account_naming_construct`](#account-naming-construct)-[`team_name`](#team-name)[-[`service_name`](#service-name)]-[`environment`](#environment)

AWS uses Okta for person based access and a IAM user would not be created.
These would be created for service accounts (Terraform, etc.)

Examples:

- entsys-prod-cdk
- network-prod-sharedservices
- security-prod-azurecloudappsecurity

#### IAM Roles

[`account_naming_construct`](#account-naming-construct)-[`environment`](#environment)[-[`app_name`](#application-name)]

Examples:

- security-prod-resourcegroupstaggingapi
- systems-prod-awsbackupservice

#### IAM Group **suggested format**/inconclusive

[`account_naming_construct`](#account-naming-construct)-[-[`service_name`](#service-name)]-[`environment`](#environment)[-[`app_name`](#application-name)]

Examples:

- security-prod-resourcegroupstaggingapi

### Azure resources

#### Virtual Network Gateway

[`region`](#region)-[`vng_type`](#vng-type)[-[`vng_purpose`](#vng-purpose)]

##### VNG Type

- ergw
- vpngw

##### VNG Purpose

Optional, to differentiate multiple vngs in a single account

#### Blob Storage

[`account_naming_construct`](#account-naming-construct)-[`unique_identifier`](#unique-identifier)

Must be globally unique (due to DNS)
1-1024 characters

#### Service Principals

Use existing service account naming convention

### Definitions

Imperatives in this document shall be defined as: (based on RFC2119)

1. MUST

    This word, or the terms "REQUIRED" or "SHALL", mean that the definition
    is an absolute requirement of the specification.

1. MUST NOT

    This phrase, or the phrase "SHALL NOT", mean that the definition is an
    absolute prohibition of the specification.

1. SHOULD

    This word, or the adjective "RECOMMENDED", mean that there may exist
    valid reasons in particular circumstances to ignore a particular item,
    but the full implications must be understood and carefully weighed
    before choosing a different course.

1. SHOULD NOT

    This phrase, or the phrase "NOT RECOMMENDED" mean that there may exist
    valid reasons in particular circumstances when the particular behavior
    is acceptable or even useful, but the full implications should be
    understood and the case carefully weighed before implementing any
    behavior described with this label.

1. MAY

    This word, or the adjective "OPTIONAL", mean that an item is truly
    optional.  One vendor may choose to include the item because a
    particular marketplace requires it or because the vendor feels that
    it enhances the product while another vendor may omit the same item.
    An implementation which does not include a particular option MUST be
    prepared to interoperate with another implementation which does include
    the option, though perhaps with reduced functionality. In the same
    vein an implementation which does include a particular option MUST
    be prepared to interoperate with another implementation which does
    not include the option (except, of course, for the feature the option
    provides.)
