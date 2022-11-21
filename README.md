regional-asa
============

This script will create extremely accurate network objects based off region.

https://www.ip2location.com/free/visitor-blocker

The script is intended to be run on a Unix host. Copy country.list and regional-asa.sh to a folder on your host. Make the .sh file executable (chmod +x regional-asa.sh). Then run it. Based on your inputs, it will generate a file with a listing of network objects and an object-group that you could then use in an ASA ACL.

To be honest, most people don't go to the trouble since the listings end up being huge and may even exceed the capability of an ASA if you were to, for example, try to exclude all of a region like Asia.

Example of running the script:

root@eve-ng:~/asa# ls -al
total 3256
drwxr-xr-x 2 root root    4096 Feb  8 18:03 .
drwx------ 5 root root    4096 Feb  8 17:58 ..
-rw-r--r-- 1 root root    4248 Feb  8 18:03 country.list
-rwxrwxrwx 1 root root    8768 Feb  8 17:59 regional-asa.sh
root@eve-ng:~/asa# ./regional-asa.sh
Please choose the authority you would like to acquire addresses from.
1. ARIN
2. LACNIC
3. APNIC
4. AfriNIC
5. RIPE
 
[1-5]? 3
--2021-02-08 18:03:41--  ftp://ftp.apnic.net/pub/stats/apnic/delegated-apnic-latest
           => 'APNIC.orig'
Resolving ftp.apnic.net (ftp.apnic.net)... 202.12.29.205, 2001:dc0:2001:11::205
Connecting to ftp.apnic.net (ftp.apnic.net)|202.12.29.205|:21... connected.
Logging in as anonymous ... Logged in!
==> SYST ... done.    ==> PWD ... done.
==> TYPE I ... done.  ==> CWD (1) /pub/stats/apnic ... done.
==> SIZE delegated-apnic-latest ... 3303673
==> PASV ... done.    ==> RETR delegated-apnic-latest ... done.
Length: 3303673 (3.2M) (unauthoritative)

delegated-apnic-latest            100%[============================================================>]   3.15M  1.00MB/s    in 3.2s    

2021-02-08 18:03:45 (1.00 MB/s) - 'APNIC.orig' saved [3303673]


Would you like to specify a country?
[y/n]? y

Please enter the name or part of the country's english name. Mal
0  :  FK - Falkland Islands (Malvinas)
1  :  GT - Guatemala
2  :  ML - Mali
3  :  MT - Malta
4  :  MV - Maldives
5  :  MW - Malawi
6  :  MY - Malaysia
7  :  SO - Somalia
8  :  None of the above
Please select the nubmer associated with the country you desire.
[0-8]? 6
You have selected:  MY - Malaysia
MY
Creation of APNIC.cidr has started.
Creation of APNIC.cidr has finshed.
Creation of APNIC.conf has started.
 54 / 154 
Creation of APNIC.cidr has finished.
root@eve-ng:~/asa# ls -al
total 3276
drwxr-xr-x 2 root root    4096 Feb  8 18:03 .
drwx------ 5 root root    4096 Feb  8 17:58 ..
-rw-r--r-- 1 root root    2464 Feb  8 18:03 APNIC.cidr
-rw-r--r-- 1 root root   13675 Feb  8 18:03 APNIC.conf
-rw-r--r-- 1 root root 3303673 Feb  8 18:03 APNIC.orig
-rw-r--r-- 1 root root    4248 Feb  8 18:03 country.list
-rwxrwxrwx 1 root root    8768 Feb  8 17:59 regional-asa.sh
root@eve-ng:~/asa# 
root@eve-ng:~/asa# more APNIC.conf
object network APNIC1
subnet 43.228.244.0 255.255.252.0
object network APNIC2
subnet 43.228.248.0 255.255.252.0
object network APNIC3
subnet 43.251.18.0 255.255.254.0
...
(omitted objects 4-153)
...
object network APNIC154
subnet 218.100.75.0 255.255.255.0
object-group network APNIC
network-object object APNIC1
network-object object APNIC2
network-object object APNIC3
network-object object APNIC4
...
(omitted the remaining objects in the object-group)
...
root@eve-ng:~/asa#
