# Create Syncwerk User Preconfiguration Files
This script is used to create the Syncwerk client pre-configuration files for Windows, OSX or Linux systems. The script will create a compressed zip file with pre-configuration files for all users on a single or clustered Syncwerk server installation.


# Install Prerequisites
```
apt-get install curl zip
```


# Download as root
```
wget https://raw.githubusercontent.com/syncwerk/helper-scripts/master/create-syncwerk-user-preconfiguration-files -O /usr/local/sbin/create-syncwerk-user-preconfiguration-files
chmod 700 /usr/local/sbin/create-syncwerk-user-preconfiguration-files
```

Set the correct `SERVER_ADDRESS` and `ADMIN_TOKEN` in `/usr/local/sbin/create-syncwerk-user-preconfiguration-files`.

You can retrieve your admin token with 
```
curl -d "username=username@example.com&password=123456" https://yncwerk.com/api2/auth-token/
``` 


# Update API rate limiting
Add the following lines to seahub_settings.py and restart your Syncwerk server.
```
# rest_framwork
REST_FRAMEWORK = {
    'DEFAULT_THROTTLE_RATES': {
        'ping': '10000/minute',
        'anon': '10000/minute',
        'user': '10000/minute',
    },
}
```


# Usage
After setting the server address and admin token for your installation simply run 
```
create-syncwerk-user-preconfiguration-files
```
as root. Then download and extract the zip file. On Windows you may want to use [WinSCP](https://winscp.net/) to retrieve the zip file.


# Client Setup

**Note:** If no user token is provided, despite the username/email being pre-configured the user will have to enter username and password manually to connect with his Syncwerk server. If you want to setup the client configuration 100% automatic. You will have to provide the username and users token within the pre-configuration methods/files.

We are planning to provide a method to obtain and setup the user tokens automatically. However, this is currently not a top priority.

## Windows
For Windows you may import the syncwerk-client.reg file within the users registry settings. This can be done manually by each user or for instance via group policy objects (GPO). To setup GPO to set these registry entries on Windows, please refer to [Microsoft's documentation](https://technet.microsoft.com/en-us/library/cc753092.aspx).

As an alternative for syncwerk-client.reg you may simply distribute the seafile.ini to your users home directory `%USERPROFILE%`. The client will search for this file and use the pre-configuration settings accordingly. 

## OSX and Linux
Distribute `.seafilerc` to the users home directory prior to starting your Syncwerk client.


# Weblinks
- [Syncwerk FAQs](http://manual.seafile.com/faq.html)
- [Syncwerk Desktop customization](http://manual.seafile.com/config/desktop_customization.html)
- [MS Support - How to use Group Policy to remotely install software in Windows Server 2008 and in Windows Server 2003](https://support.microsoft.com/en-us/kb/816102)
- [MS TechNet - Configure a Registry Item](https://technet.microsoft.com/en-us/library/cc753092.aspx)
