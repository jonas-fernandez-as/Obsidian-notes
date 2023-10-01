Recall that zone transfers are **used to copy a domain's database from the primary server to the secondary server**. If an attacker is able to perform a zone transfer with the primary or secondary name servers for a domain, the attacker will be able to view all DNS records for that domain.



The idea is obtain a list of subdomains of a domain, if the DNS server is vulnerable.

We can use dig for example, this is the command for the doman zone transfer
```
dig @10.10.10.123 friendzone.red axfr
```

we can see the domain servers and the mail servers

```
dig @10.10.10.123 friendzone.red ds
```
and the mail servers
```
dig @10.10.10.123 friendzone.red mx
```

We modify the /etc/hosts file and add the new domains to the ip of friendzone.red.. the idea is not use a bruteforce attack ... or fuzz all the other domines.

