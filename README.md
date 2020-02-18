## LDAP
![image](https://user-images.githubusercontent.com/12431831/74765150-0bd9df80-52a9-11ea-9b5b-4ade1f4d2e12.png)

![image](https://user-images.githubusercontent.com/12431831/74765213-2613bd80-52a9-11ea-8e7b-3fd69560b417.png)


![image](https://user-images.githubusercontent.com/12431831/74765326-578c8900-52a9-11ea-98d0-ce7380e36912.png)


#### Netgroups
```
# /etc/hosts.deny
sshd: ALL
 . . . 
# /etc/hosts.allow
sshd: @sysadmin

# Getting Netgroups
getent netgroup sysadmin
sysadmin             (sa.willeke.com, , ) (xenhost.willeke.com, , )
```


![image](https://user-images.githubusercontent.com/12431831/74765489-b3571200-52a9-11ea-9547-e2cffbf0e228.png)

```
# Before adding any netgroup entries to the directory, you must create the container ou. By convention, I will use the ou=netgroup organizational unit for storing netgroups in this example:

dn: ou=netgroup,dc=plainjoe,dc=org
objectclass: organizationalUnit
ou: netgroup

# The sysadmin netgroup will be represented by this LDIF entry:

$ ./migrate_netgroup.pl /etc/netgroup 
dn: cn=sysadmin,ou=netgroup,dc=plainjoe,dc=org
objectClass: nisNetgroup
objectClass: top
cn: sysadmin
nisNetgroupTriple: (garion.plainjoe.org,-,-)
nisNetgroupTriple: (silk.plainjoe.org,-,-)

# The all_sysadmin netgroup contains the sysadmin and the secure_clients netgroups, so it will use the memberNisNetgroup attribute:

dn: cn=all_sysadmin,ou=netgroup,dc=plainjoe,dc=org
objectClass: nisNetgroup
objectClass: top
cn: all_sysadmin
memberNisNetgroup: sysadmin
memberNisNetgroup: secure_clients

# After adding these entries to your directory, you must configure the nss_base_netgroup parameter in /etc/ldap.conf to use the correct search suffix:

## /etc/ldap.conf
## <remaining parameters imitted>
## Configure the search parameters for netgroups.
nss_base_netgroup    ou=netgroup,dc=plainjoe,dc=org?one

# Finally, you must inform the the operating system to pass off netgroup queries to the LDAP directory by updating the netgroup entry in /etc/nsswitch.conf:

## /etc/nsswitch.conf
##  . . . 
netgroup:   ldap

# The getent tool can be used to query NSS for specific netgroups by giving the group name as a command-line parameter:

$ getent netgroup sysadmin 
sysadmin   (garion.plainjoe.org,-,-)(silk.plainjoe.org,-,-)
```

#### Search, Filter
"ou=patrons, dc=inflinx, dc=com"
![image](https://user-images.githubusercontent.com/12431831/74767408-d20ad800-52ac-11ea-8580-257ec990f14a.png)

Filter =  (attributetype operator value)

ex. (city=Moradabad)

Complex Filter :

Filter =  (operator filter1 filter2)

(&(city=Moradabad)(mail=*))

![image](https://user-images.githubusercontent.com/12431831/74767662-4b0a2f80-52ad-11ea-81a3-a5fc6dff8a7a.png)


![image](https://user-images.githubusercontent.com/12431831/74767778-80168200-52ad-11ea-9290-06b15e341350.png)



