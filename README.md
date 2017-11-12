# cudawaf

#### Table of Contents

1. [Description](#description)
2. [Setup - The basics of getting started with cudawaf](#setup)
    * [What cudawaf affects](#what-cudawaf-affects)
    * [Setup requirements](#setup-requirements)
    * [Beginning with cudawaf](#beginning-with-cudawaf)
3. [Usage - Configuration options and additional functionality](#"usage examples")
4. [Reference - An under-the-hood peek at what the module is doing and how](#reference)
5. [Limitations - OS compatibility, etc.](#limitations)
6. [Development - Guide for contributing to the module](#development)

## Description

With hundreds of lines of code to check - and vulnerabilities often subtle and hard to find - a serious data breach is often the first sign that a web application has problems. Having secured thousands of production applications against more than 11 billion attacks since 2008, the Barracuda Web Application Firewall is the ideal solution for organizations looking to protect web applications from data breaches and defacement. With the Barracuda Web Application Firewall, administrators do not need to wait for clean code or even know how an application works to secure their applications. Organizations can ensure robust security with a Barracuda Web Application Firewall hardware or virtual appliance, deployed either on-premises or in the cloud.
This module can be used to configure the Barracuda Web Application Firewall using Puppet. The following features can be configured using this module:

1. Service
2. Server
3. Certificates
4. Cloud-control

The detailed documentation on each of these REST API end points can be found here : [Barracuda Web Application REST API v3](https://campus.barracuda.com/product/webapplicationfirewall/api)

## Setup
`puppet module install puppetlabs-cudawaf`

## To install in a specific environment
`puppet module install puppetlabs-cudawaf --environment=<env-name>`

## Setup Requirements

Installing the gem files:
`/opt/puppetlabs/puppet/bin/gem install typoheus`
`/opt/puppetlabs/puppet/bin/gem install rest-client`

## Usage Examples

### Before you begin
### To create a Service:
```
wafservices  { 'WAFSVC-1':
  ensure        => present,
  name          => 'WAFSERVICE',
  type          => 'http',
  mask          => '255.255.255.255',
 ip_address    => '3.3.3.3',
 port          => '80',
 group         => 'default',
 vsite         => 'default',
 status                => 'On',
 address_version       => 'ipv4',
 enable_access_logs => 'Yes',
 svcname => 'ProdService',
       }
}
```
### To create a Real server:
```
wafservers{ 'WAFSERVER-2':
 ensure => present,
 name => 'server2',
 identifier=> 'IP Address',
 address_version => 'IPv4',
 status => 'In Service',
 ip_address => '8.8.8.8',
 service_name => 'demo_service_13',
 hostname => 'TEST',
 port => '80',
 comments => 'Creating the server',
}
```
### To create a Certificate:
```
wafcertificates{ 'WAFUPLOADSIGNEDCER-1':
 ensure => present,
 cer_name => 'wafuploadsignedcert1',
 name => 'signedcert1',
 signed_certificate => '/home/wafcertificates/fullchain.pem',
 allow_private_key_export => 'yes',
 type => 'pem',
 key =>'/home/wafcertificates/privkey.pem',
 assign_associated_key => 'no',
 upload => 'signed',
}
```
### To Upload a Signed Certificate:
```
wafcertificates{ 'WAFUPLOADSIGNEDCER-1':
  ensure => present,
  name => 'signedcert1',
  signed_certificate => '/home/wafcertificates/root.pem',
  allow_private_key_export => 'yes',
  type => 'pem',
  key =>'/home/wafcertificates/privkey.pem',
  assign_associated_key => 'no',
  upload => 'signed'
}

```
### To Upload a Trusted Certificate:
```
wafcertificates{ 'WAFUPLOADTRUSTEDCER-1':
  ensure => present,
  name => 'trustedcert1',
  trusted_certificate => '/home/wafcertificates/cer.pem',
  upload => 'trusted'
}

```
### To Upload a Intermediary Signed Certificate:
```
wafcertificates{ 'WAFUPLOADINTERMEDIATESIGNEDCER-1':
  ensure => present,
  name => 'signedcertint1',
  signed_certificate => '/home/wafcertificates/root.pem',
  intermediary_certificate => '/home/wafcertificates/inter.pem',
  allow_private_key_export => 'no',
  type => 'pem',
  key =>'/home/wafcertificates/privkey.pem',
  assign_associated_key => 'no',
  upload => 'signed'
}

```
### To Upload a Trusted Server Certificate:
```
wafcertificates{ 'WAFUPLOADTRUSTEDSERVERCER-1':
  ensure => present,
  name => 'trustedservercert1',
  trusted_server_certificate => '/home/wafcertificates/cer.pem',
  upload => 'trusted_server'
}
```
### To connect the WAF to Barracuda Cloud Control
```
wafcloudcontrol{ 'WAFCouldControl-1':
  ensure => present,
  connect_mode => 'cloud',
  state => 'connected',
  username => 'testmail@barracuda.com',
  password => 'password'
}
```

## Development

Since your module is awesome, other users will want to play with it. Let them
know what the ground rules for contributing are.

## Release Notes/Contributors/Etc.

If you aren't using changelog, put your release notes here (though you should
consider using changelog). You can also add any additional sections you feel
are necessary or important to include here. Please use the `## ` header.
