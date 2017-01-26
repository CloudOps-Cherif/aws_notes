## Route 53

#### DNS 101
DNS converts human friendly domain names into an IP address
1. IP4 - 32 bit field, has 4 billion different addresses
2. IP6 - 128 bit - shit ton of addresses (support for this in AWS is not great)

Top level domain name: .gov, .edu, etc

Second level domain name: .co.uk (the .co part is the second level domain)

Top level domains are controlled by Internet Assigned Numbers Authority (IANA)

##### Domain registrar
* Authority that can assign domain names directly under one or more top level domain
* Domains are registered with InterNIC, a service of ICANN, which enforces uniqueness
* Each domain gets registered in a central database known as the WhoIS database

TTL (Time To Live) - length of time a DNS record is cached on the resolving server or the user's PC. The lower the TTL, the faster DNS changes will propagate. (You may want to drop TTL very low if doing a migration from old IP to new IP)

Canonical Name (CName)- used to resolve one domain to another

Alias Record - used to map resource record sets in your hosted zone to Elastic Load Balancers (ELB), CloudFront distribution, or S3 buckets that are configured as web sites. Can be used to map one DNS name (www.whatever.com) to another target DNS name (asdfkjasdf.s3.amazonaws.com)

Difference between CName can't be used for naked domain names (without www)

exam tips:
* Elastic Load Balancers do not have a predefined IPv4 address, you resolve them using a DNS name
* Understand difference between an Alias record and a CName
* Given the choice, always choose an Alias Record over a CName.
