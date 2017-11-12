# cudawaf

## Table of Contents

1. [Description](#description)
2. [Types](#resource-types)
2. [Setup - The basics of getting started with cudawaf](#setup)
3. [Usage - Configuration options and additional functionality](#usage-examples)
4. [Reference - An under-the-hood peek at what the module is doing and how](Reference)


## Description

With the Barracuda Web Application Firewall, administrators do not need to wait for clean code or even know how an application works to secure their applications. Organizations can ensure robust security with a Barracuda Web Application Firewall hardware or virtual appliance, deployed either on-premises or in the cloud.
This module enables management of the Barracuda Web Application Firewall using Puppet.

## Resource Types

The following features can be configured using this module:

1. `Service` - A Virtual Service is a combination of a Virtual IP (VIP) address and a TCP port, which listens and directs the traffic to the intended Service.


2. `Server` - A server object can be used to configure the networking information of the back-end server to be hosted on the Barracuda Web Application Firewall. Multiple real servers can be added and configured to load balance the incoming traffic for a Service.

3. `Certificates` - A signed certificate is a digital identity document that enables both server and client to authenticate each other.  Certificates are used with HTTPS protocol to encrypt secure information transmitted over the internet.  A certificate can be generated or procured from a third party Certificate Authority (CA). Generated certificates can be self-signed or signed by a trusted third-party CA. A certificate contains information such as user name, expiration date, a unique serial number assigned to the certificate by a trusted CA, the public key, and the name of the CA that issued the certificate.

4. `Cloud-control` - A comprehensive cloud-based service that enables administrators to monitor and configure multiple Barracuda Networks products from a single console.

The detailed documentation on each of these REST API end points can be found here : [Barracuda Web Application REST API v3](https://campus.barracuda.com/product/webapplicationfirewall/api)

## Setup

### Pre-Requisites

The following components are necessary to use this module:

  * A server running as a Puppet master.
  * A Puppet agent running as a controller to the Barracuda WAF.
  * A Barracuda Web Application Firewall running the firmware version v9.1 or above


### To install the module
`puppet module install puppetlabs-cudawaf`

### To install in a specific environment
`puppet module install puppetlabs-cudawaf --environment=<env-name>`

### Installing the gem files on the Puppet Agent node:
```
/opt/puppetlabs/puppet/bin/gem install typoheus
/opt/puppetlabs/puppet/bin/gem install rest-client
```
### Create a credentials file on the Puppet Agent node:

Path ```/etc/puppetlabs/puppet/credentials.json```

Example "credentials.json"

```
{
  "username":"admin",
  "password":"admin"
}
```

## Usage Examples

### Creating Resources
The following example manifests can be used to create resources on the Barracuda Web Application Firewall:

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
  service_name => 'ProdService',
  port => '80',
  comments => 'Server for ProdService',
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
