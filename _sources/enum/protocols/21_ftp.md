# FTP

FTP, or File Transfer Protocol is one of the simplest services we'll run into during our testing. 

All FTP does is set up a protocol whereby a folder can be shared over a network, it's far simpler than something like SMB to configure and there are lots of versions that have public exploits we can use.


> It is a plain-text protocol that uses as new line character 0x0d 0x0a so it's important to connect using telnet instead of nc.

## Fingerprinting

As noted above, we'll connect using `telnet` due to having to use a proper new line character.

```bash
telnet 10.10.10.10 25
```

Sometimes we run into SFTP (secure):

```bash
openssl s_client -connect domain.com:21 -starttls ftp
```

> This can be running on `Port 22` so pay attention to the `nmap` results!

## Attack Vectors

FTP has a bunch of uses, the main thing is to figure out how it's being used. A good example might be a webserver that's using the FTP server to allow files to be uploaded, we could potentially use this functionality to upload a webshell to get us command execution. Or upload a file with an XSS payload to steal some credentials.

- Directory Traversal/Sensitive Data Exposure (find interesting things)
- FTP Bounce (Port scanning)
- Malicious file upload 
- Service vulnerabilities

## Access

FTP can be accessed in one of two ways:

- Anonymously
- Authenticated

### Anonymous Access

Check that the service is running properly using `nmap` or `nc`. Once you've established that it is running simply connect to the target using:

```bash
ftp 10.10.10.10
```

When prompted for your login credentials simply type `anonymous`. You will sometimes need to enter a password as well (this can generally be anything you like you won't get asked for it again, but I tend to just use `anonymous`)

If the target is configured to allow anonymous access you'll be able to log in and get access to whatever files are hosted on there.

### Authenticated Access

In the case that the target does not allow for anonymous access, or in some cases where it's allowed but the anonymous user doesn't have read/write permissions we can log in using credentials captured from other services on the target.

Same flow as the anonymous login, but this time use the credentials you found to log in with, and hope that you get access to what you need to progress.

## Download all files

```bash
wget -m ftp://anonymous:anonymous@10.10.10.98 #Donwload all
wget -m --no-passive ftp://anonymous:anonymous@10.10.10.98 #Download all
```

## FTP Bounce

FTP Bounce is a way of using an FTP server that is configured to allow the `PORT` command to perform a port scan of internal services.

[What is an FTP Bounce attack](https://www.thesecuritybuddy.com/vulnerabilities/what-is-ftp-bounce-attack/)

Could be useful if we find ourselves with no other vectors for performing an enumeration of a targets intenal connections.

## General Commands

```bash
USER username
PASS password
HELP # Get list of available commands
PORT 127,0,0,1,0,80 # Connect to a target on a given port (FTP Bounce)
GET file # Download a single file from the host
PUT file # Upload a single file to the host
LIST # List files in current directory
```
