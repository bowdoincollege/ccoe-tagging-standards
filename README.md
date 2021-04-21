# Bowdoin Tagging Standards

<!--TOC-->

- [Bowdoin Tagging Standards](#bowdoin-tagging-standards)
  - [Overview and Purpose](#overview-and-purpose)
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
  - [Construction of the Name tag value](#construction-of-the-name-tag-value)
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
owner, environment, or other criteria.  Each tag has two parts:

- tag key (for example: costcenter, Name, environment).  Tag keys may be
case sensitive depending on the cloud provider
- A tag value (for example: 1234556, prod).  Like tag keys, tag values
may be case sensitive depending on the cloud provider

This document provides an overview and definition of tags required by
Bowdoin College and the standards for those keys and value that are
expected.  This is not meant to prevent other tags from being created
but intended to create a baseline of common tags.  Should a new tag
become required, it should be added to this document.  Conventions Used
in this Document

## General Naming standard

### Keys

In order to maintain consistency between platforms and different people
creating tags, the following naming standard is recommended for all
tags, defined or custom.

- Valid characters are a-z, 0-9, and - (dash)
- Only lowercase letters will be used
- Spaces will not be used; a dash must not be used to separate words
   only sections of a tag value
- Namespaces will not be used to prefix Bowdoin tags;
  tags without a prefix are assumed to be in Bowdoin namespace
- Prefixes may be added to third party tags (i.e., AWS)
- A key should not be longer than 63 characters

### Values

- Unless specified, valid characters are a-z, 0-9, and - (dash)
- Hungarian notation may be used with long names to help improve clarity
- Unrelated values will be placed in separate keys
- Compound tag values should not be used

## Defined Tags

### Name

Mandatory: Yes

This tag is a default tag for AWS resources, and has special behavior in
the AWS console.  Because of this, the key of this tag is an exception
to the naming rule in that it contains a capital letter.

The [format](#construction-of-the-name-tag-value) of this tag's value
depends on its resource type.

### costcenter

Mandatory: Yes

Reflects the Bowdoin Project Code from the financial system.  This is
used to identify and report portions of an overall invoice or billing
cycle that belong to a group on campus based on their financial project
number.  Shared resources (i.e., a common vnet/VPC), may be marked with
a project number from IT.  Important:  This is not used for chargeback,
but it is used for showback.  Valid Characters: Six digits, 0-9 matching
an existing project code in the Bowdoin financial system.

### environment

Mandatory: Yes

A deployment environment is a group of computer, network and
infrastructure configuration in which an application is deployed and
executed.  The values of this tag should reflect the current deployment
environments as defined (v2).  Note: The current environment list should
be evaluated and updated for current needs.  This is an effort that
needs to happen separately.

Valid values:

- prod
- dev
- stage
- test

### compliance

Required:  If it falls under one of the defined security
compliance groups

For regulatory compliance, and will be (GDPR, PCI,
HIPAA, etc.) as defined by policy.

### classification

Required: If it has a label other than sensitive.

For data classification.  This is "sensitive" by default, and not needed
to be specified unless it is different.  The value should be one of
the options in our data classification standard (Public, Sensitive,
Restricted, etc.)

### description

Mandatory: No

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

Mandatory: optional tag that is recommended.

This tag is a URL used to point to documentation.  At the time of this
discuss Confluence is our source of documentation, but it could a URL to
another source.

### resourcegroup

Mandatory: No

Used to identify service, aka resource group which may end up being the
name.

Example: All the components of Veeam would have a common value in this
tag.

## Construction of the Name tag value

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

### Application Naming Construct

`appname_construct` =
[`appname`](#application-name)-[[`environment`](#environment)-][[`number`](#number)-][[`region`](#region)]

#### Application Name

`appName`

all lowercase one word no dashes
App environment
prod
dev
stage
test
experimental

#### Number

`number`

start at 1
region
use if multi region only
example
myduodevices-prod-2

## Naming by resource type

### AWS resources

#### VPC

{{account_naming_construct}} - {{vpcPurpose}}
vpcPurpose
all lowercase one word no dashes
This name needs to be determined in consultation with Networking.
example
networking-prod-sharedservices

#### Subnet

{{account_name_construct}} - {{subnetPurpose}}[ - {{az}}]
subnetPurpose
all lowercase one word no dashes
az
This should not be used in Azure but should be used in AWS.
a
b
...
example
networking-prod-wvdstaff-a
academic-prod-workspaces-c

#### Virtual Network Gateway

{{region}} - {{vngType}}[-{{purpose}}]
This is an Azure-specific resource.
VngType
ergw
vpngw

Purpose
Optional, to differentiate multiple vngs in a single account

#### VPC/VNet Peering Link

{{source account name}}-{{sourceVPC}}-{{target account name}}-{{targetVPC}}
Note that there will be two resources, one on each end of the link.
example
entsystems-dev
networking-hub-something-wvd (networking side of link)
something-wvd-networking-hub (something side of link)

#### Route Tables

{{vpc_naming_construct}} - {{routeType}}[ - {{az}}]
routeType
public
private
az

For AWS only.

If the route table is distinct for each AZ (e.g. you are routing to
different NATs), you must add the following Zone - Zone should be #l
(e.g a, b, c, d, e)

example
networking-prod-sharedservices-vpc-public
networking-prod-sharedservices-vpc-public-a

#### Security Groups

{{appname_construct}} - {{resource_name}} - {{purpose}}
purpose
ssh
http
http
...
resource_name
elb
efs
nfs
instance
db
examples
dns
sql
banner-instance-443
entsys-prod-nonhybrid-vpc-private

#### Network ACL

{{account_naming_construct}} - {{purpose+direction}}
purpose+direction
ssh-in
http-in
http-out
example
networking-prod-ping-in

#### Instances

{{appname_construct}} - [{{service type}}] - [{{incremental identifier}}]
Incremental Identifier
Use a zero padded number
May contain sequential letters
start at 01
examples
security-prod-sumologcollector-1
security-prod-sumologcollector-01
security-prod-sumologcollector-a
security-prod-sumologcollector-db-a
security-prod-sumologcollector-web-a

#### Load Balancer

{{appname_construct}}– [{{service type}}] – [{{incremental identifier}}]
Launch Configuration
{{appname_construct}}– [{{service type}}] – [{{incremental identifier}}]
AutoScaling Group
{{appname_construct}}– [service type] – [incremental identifier]

#### AMI (AWS Machine Image)

{{image name}}[-{{service type}}][-{{unique identifier}}]
Image name is made up of OS name, OS version, and function
OS Name
rhel
centos
ubuntu
windows
OS Version
2012r2ent
10-{{build number}} i.e. 10-2004
7
Function
staffimage
base
Future: Create image specific tags for OS, version, and architecture
examples
rhel8base-web-012

#### SSH Pem Keys

{{account_naming_construct}} - {{appname_construct}}

#### Database Service

Applies to PaaS and SaaS, not self installed on IaaS
Does not apply to databases inside the service
{{appname_construct}} - {{db_vendor}}
db_vendor
oracle
mysql
postgres
maria
aurora
sql
mssql
access
sqlmi
example
sqlmanagedinstance-dev-1-mssql
common-dev-sqlmi

#### Lambda

{{appname_construct}}-[{{function name}}]
Function name

Used when the function is part of a larger app.  It should describe what
the function does.

example
sumologicworkday-test
enrollmentform-prod-updatelivedname

#### S3

Must be globally unique (due to DNS)
3-63 characters
{{account_naming_construct}} - [unique identifier]
unique identifier
could be {{appName}}
purpose, string, hash
This is up to the eye of the beholder.

#### Efs/ebs

Primary volumes will be named the same as EC2 instance.  Secondary
volumes use this convention.

{{primary volume name}}-disk{{incrementing number}}

#### IAM Users

{{account_naming_construct}}
{{teamName}} -[{{servicename}}-] {{environment}}
AWS uses Okta for person based access and a IAM user would not be created.
These would be created for service accounts (Terraform, etc.)

example
entsys-prod-cdk
network-prod-sharedservices
security-prod-azurecloudappsecurity

#### IAM Roles

{{account_naming_construct}} - {{appName}}
{{teamName}} -[{{servicename}}-] {{environment}}- {{appName}}
example
security-prod-resourcegroupstaggingapi

#### IAM Group **suggested format**/inconclusive

{{account_naming_construct}} - {{appName}}
{{teamName}} -[{{servicename}}-] {{environment}}- {{appName}}
example
security-prod-resourcegroupstaggingapi

### Azure resources

#### Blob Storage

Must be globally unique (due to DNS)
1-1024 characters
{{account_naming_construct}} - [unique identifier]
unique identifier
could be {{appName}}
purpose, string, hash
This is up to the eye of the beholder.
Storage Account name
3-24 characters, no dashes
Data: {{purpose}}{{incrementing number}}
Disks: {{instance name without dashes}}{{incrementing number}}

Attached Disk:

Primary volumes will be named the same as EC2 instance.  Secondary
volumes use this convention.

{{primary volume name}}-disk{{incrementing number}}
Example: vm-0_OsDisk_1_a762254a841f48b1a5c21395590feb1e
unique identifier
could be {{appName}}
purpose, string, hash
This is up to the eye of the beholder.
example
security-prod-crowdstrikesensor
securityprodcrowdstrikesensor
sumobrlogsrdfzegjek5hgy
bowdwvdprofile

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
