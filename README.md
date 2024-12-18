# Intune_handson


requirements:

1. developer license - developers.microsoft.com
2. domain controller
3. create 3 win11 machine


![alt text](image.png)
![alt text](image-1.png)
![alt text](image-2.png)


![alt text](image-3.png)

 - select configurable sandbox - to try on your own

![alt text](image-4.png)


thulasi@th40032944.onmicrosoft.com
@GtGt


![alt text](image-5.png)


Download win11 ISO

![alt text](image-6.png)


setup domain controller with windows server 2019

in C: drive > 
 
  - DC-ADID for domain controller
  - CL1-ADID for client machine 1
  - CL2-ADID for client machine 2

for vhd storage
![alt text](image-7.png)

# vm creation

GEN: 2
memory 2048
connection: internalNATswitch
size : 100gb
bootable image: iso: server iso

![alt text](image-9.png)
![alt text](image-8.png)
![alt text](image-10.png)
![alt text](image-11.png)
![alt text](image-12.png)
![alt text](image-13.png)
![alt text](image-14.png)


# START THE VM
![alt text](image-15.png)
![alt text](image-16.png)
![alt text](image-17.png)
![alt text](image-18.png)

Administrator

Admin@12345

CHANGE THE COMPUTER NAME AND SETUP NETWORKING IP
BEFORE SETTING ADDS

## Networking:

192.168.0.9
255.255.255.0
192.168.0.1

![alt text](image-19.png)

setting ADDS role

![alt text](image-20.png)
![alt text](image-21.png)
![alt text](image-22.png)
![alt text](image-23.png)

![alt text](image-24.png)
![alt text](image-25.png)
![alt text](image-26.png)
![alt text](image-27.png)

![alt text](image-28.png)


# setting up win11 client vm

RAM: 4gb - 4096
size: 100gb
ISO: win11 iso
(note: uncheck secure boot, enable tpm, 2 virtual processor)

![alt text](image-29.png)



useful sites:



1. admin.microsoft.com
2. entra.microsoft.com
3. intune.microsoft

  


benefits of hybrid:

- user account will be available in 2 directories
- password synchronisation

AD sync:
![alt text](image-30.png)
![alt text](image-32.png)

powershell to upddate tls 1.2

![alt text](image-31.png)

install vc++ redistributables, ...
![alt text](image-33.png)
![alt text](image-34.png)


create 2 users in onprem under one OU (sales ) and try to sync it
![alt text](image-35.png)

connect to entra directory using global admin

also allow local admin to connect the forest
![alt text](image-36.png)
![alt text](image-37.png)

![alt text](image-38.png)

select only OU and devices to sync
 ![alt text](image-39.png)


![alt text](image-40.png)
![alt text](image-41.png)

verify in AAD

![alt text](image-42.png)



# user accounts

 - cloud user
 - sync user ( from onprem ad)
 - guest (invited)


for license assignment - add / remove / reprocess 
 - only with m365 admin center



# groups 

- 1. M365 
    - for application collaboration ( sharepoint, teams, shared mailbox)
    - only users will be member of this group
    - will have email id ( email enabled group)

    ![alt text](image-43.png)
- 2. security
    - best for permission and grouping
    - can include user, device, apps..

    ![alt text](image-44.png)

    ![alt text](image-45.png)

# device join methods

    - use client 1

    - create one user (test1) in admin portal and assign one license ( that should include P1 )
    ![alt text](image-49.png)

settings > access > connect > join the device using entra id
![alt text](image-46.png)![alt text](image-47.png)![alt text](image-48.png)
- use the account to authenticate and join

![alt text](image-50.png)

![alt text](image-51.png)

verify the status in portal

![alt text](image-52.png)

verify the automatic enrollment ( intune, should be pre enabled)
![alt text](image-54.png)

![alt text](image-55.png)

setup MFA

![alt text](image-56.png)
    
check the sync status locally
![alt text](image-58.png)

![alt text](image-59.png)


to force the sync manually and locally

`dsregcmd /status`

![alt text](image-57.png)


Check in entra portal

![alt text](image-60.png)

![alt text](image-61.png)
![alt text](image-62.png)



`we can also enroll the same device using company portal app`

### use CL2 to onboard to intune using company portal ( but this will be just device - registered)

use company portal from windows store 
![alt text](image-96.png)
![alt text](image-97.png)

use  `testuser2@th40032944.onmicrosoft.com`( with license )

![alt text](image-98.png)
![alt text](image-99.png)

setup MFA,
setup Hello

![alt text](image-100.png)

verify the registration locally and also in portal
![alt text](image-101.png)
![alt text](image-102.png)

![alt text](image-103.png)
![alt text](image-104.png)

## MDM lifecycle ( in managing via intue)

- enrollment
- inventory
- 



# device operations  from intune

- Retire
    - this will only company data managed by intune, not touching user personal data
    - before retire, make sure the installed apps via intune should be deleted
- delete

- wipe
    - best for corporate device, to remove everything, like sanitize
- sync
    - force the sync from portal ( gpupdate /force locally)
- restart
    - initiate the restart
- collect diagnostics



 # Device management

 intune portal > devices > windows > enrollment > automatic enrollment
 
( only when having premium licence directory  / Entra ID)


`any device which is onboarded to cloud, will be enrolled in intune ? ` - No, unless it is configured by the organization

![alt text](image-53.png)




# create cl3 for further works same as other clients

![alt text](image-63.png)
![alt text](image-64.png)
![alt text](image-65.png)


# Dynamic group membership handson
create a group to collect users from department Sales
![alt text](image-66.png)

![alt text](image-67.png)

use 2 users to have department like sales

create the group query to filter and group

![alt text](image-68.png)

verify the group
![alt text](image-69.png)

![alt text](image-70.png)

see who grouped this based on query
![alt text](image-71.png)


### Likewise, create a dynamic group to collect win10 device
'
![alt text](image-72.png)


![alt text](image-73.png)

verify the members

![alt text](image-74.png)



# user with device management 


by default user needs a license to enroll

 - default - 5 device, a user can enroll
![alt text](image-77.png)
![alt text](image-76.png)

 - max can be 15 ( based on org config)

 ![alt text](image-75.png)


 - based on device platform also we can restrict


# onprem mode of policy vs Cloud configuration

earlier we mostly rely on GPO to enforce machines ( capable with domain join )

but in cloud,

device may or may not have domain join features

may be just registered

we can use
 - configuration profile
        - select the platform
            - win10, win11, android, ios, mac
        - select the template
           
        - assign to group ( cannot be to one user ,same as SCCM)
        - monitor the status

## configuration profile

![alt text](image-78.png)

![alt text](image-79.png)

![alt text](image-80.png)

![alt text](image-81.png)

try disabling bluetooth connectivity 

![alt text](image-82.png)

![alt text](image-83.png)


if needed you can filter the device as well from the group using rules

![alt text](image-84.png)

![alt text](image-85.png)


initiate the sync to see if the policies are updated in device


check the config status

![alt text](image-88.png)
![alt text](image-89.png)
![alt text](image-90.png)
![alt text](image-91.png)

`policy interval time: within 3mins / 15mins / 2 hrs / max 8 hrs`


## custom configuration profile

 - CSP
    - configuration service provider ( some settings that gives us the path of the regkey settings)

    - OMA - URI
        -   open mobile alliance
        -   uniform resource identifier
        -   the csp path to findout

# Application
        
# Need of intune for applications

 - prepare the application
 - assign the application
 - update the application
 - uninstall the application
 - monitor the application

can work in different kinds of platforms
 - windows
 - android
 - mac
 - ios

![alt text](image-92.png)


types of application ( say in windows)

- windows store apps
- exe, msi ( line of business )
- m365 application

![alt text](image-93.png)


# Applications in Intune

![alt text](image-86.png)
![alt text](image-87.png)



Application cycle ( add, deploy, configure, protect, retire)


in sccm ( create, distribute, deploy, assign, retire)

 - prepare / add
    - based on device type, script, create
        - Device platform
        - Available mode of installation
        - Required ( silent and forced)
        - based on sysytem installation
        - based on user installation ( for particular user, whereever he logs in)
        - catogery ( optional to group )


Catalogues to publish the application
 - portal.manage.microsoft.com
 - company portal app

Mode of enforcing app installation

 - Available ( user will go and get it from catalogues)
 - required ( silent installation, force )

sample:
![alt text](image-159.png)

![alt text](image-94.png)
![alt text](image-95.png)

## creation of store application

![alt text](image-105.png)

assignment
![alt text](image-106.png)

![alt text](image-107.png)

sync the device to get this app

for sample in client1 ( as it dont have company portal, we can use portal.manage.microsoft.com page as catalogue. this will launch from browser to windowsStore)
![alt text](image-108.png)
![alt text](image-109.png)
![alt text](image-110.png)
![alt text](image-111.png)
![alt text](image-112.png)
![alt text](image-113.png)
![alt text](image-114.png)
![alt text](image-115.png)

for client2 ( company portal)

## creation of MSI application ( LOB )

![alt text](image-116.png)

no need of distribute the content ( unlike SCCM )

## tricky with .exe

https://learn.microsoft.com/en-us/mem/intune/apps/apps-win32-app-management


![alt text](image-117.png)
![alt text](image-118.png)

## requirements
 - win10 1607 or later
 - device must be joined to azure AD and auto enrolled
 - max 8gb app


by default, intune doesnot support .exe,
 - to overcome we need to convert
 - into intunewin format


https://github.com/microsoft/Microsoft-Win32-Content-Prep-Tool/blob/master/IntuneWinAppUtil.exe
![alt text](image-119.png)


lets deploy notepade++ exe
admin.microsoft.com/#/homepage

![alt text](image-120.png)
![alt text](image-121.png)

![alt text](image-122.png)

![alt text](image-123.png)

 - app information
 - create program
 - requirements
 - detection rules
 - dependencies
 - supersedence
 - assignments
 - review and create



## creation of M365 application

![alt text](image-124.png)

![alt text](image-126.png)
![alt text](image-127.png)

![alt text](image-128.png)

![alt text](image-136.png)
![alt text](image-137.png)


## creation of android apps
![alt text](image-129.png)

point to note for android

![alt text](image-130.png)

for android, we need a google account

we need to connect gms ( google mobile services)

![alt text](image-131.png)

![alt text](image-132.png)
![alt text](image-133.png)
![alt text](image-134.png)


you will see some default apps added ( related to google play )
![alt text](image-135.png)

![alt text](image-138.png)

if i select Store
 - i need to give `application url` ( android store app, just sharing the url link)
 - i need to give the application ( managed google play app, to push the actual application like forcing, authenticator, )


## like wise for

 - ios store app
 - kiosk app



# COMPLIANCE POLICIES

 - complaint / non compliant to our org standard
 - email notificiation to retire immediately
 - windows / android / mac/ ios


Steps 
 - create policy based on environment
 - settings
 - assign
 - monitor


 ![alt text](image-139.png)

 ![alt text](image-140.png)

 ![alt text](image-141.png)

enabling `antivirus and firewall`

![alt text](image-142.png)

action items based on noncompliance
![alt text](image-143.png)

Assign to the group

![alt text](image-144.png)

review and create

![alt text](image-145.png)


![alt text](image-153.png)




# CONDITIONAL ACCESS policy
 
- it is actually from ENTRA ID / Azure AD



like 
 - allowing MFA if device accessing company resource
 - allow based on location
 - restrict access

by default MFA is disabled

![alt text](image-146.png)


# try creating new conditional access policy

![alt text](image-147.png)


select specific /all user to enforce
![alt text](image-148.png)

to target resource
 - all cloud apps / particual apps

based on network as well

![alt text](image-149.png)


based on some condition as well ( say on windows platform)

![alt text](image-150.png)

![alt text](image-151.png)

![alt text](image-152.png)

![alt text](image-154.png)


# AUTOPILOT program




why ?

 - to install os
 - onboard
 - to install apps / updates /
 - post configurations
 - report and compliance

why from windows 10 ?

- speciliazed 
- default with cloud connection and integration
- preinstalled agent to handle

requirements

- device internet connectivity ( stable )
- work or school account 
- license ( with org )
- entra ID p1 & P2 license ( with org )
- intune licese (with user)
- Get the serial no. ( either manually or powershell)




## Autopilot can be applicable to both New device & existing device
- the hardware id using powershell to enroll 
- Configuration profile ( device enrollment intune will require this)
- Deploy the post configuration tasks


## possible actions

- vendor
    - can directly upload hardwareId if access is provided / authorized role

- from org admin
    - create deployment profile ( autopilot)
    - create a dynamic device group 
    - assign the profile
    - profiles, compliance, apps....


## ways to get serial no. ( only powershell, no GUI)

https://learn.microsoft.com/en-us/autopilot/add-devices#powershell


allow execution
        ` Set-ExecutionPolicy -Scope Process -ExecutionPolicy RemoteSigned `

![alt text](image-155.png)

install the script  `Install-Script -Name Get-WindowsAutopilotInfo`
![alt text](image-157.png)

get the info to CSV

`Get-WindowsAutopilotInfo -OutputFile AutopilotHWID.csv`

![alt text](image-158.png)


if the intune portal is not having the device ID uploaded, the autopilot will not start on this machine

to upload: ( login as admin in the machine, where csv found)

![alt text](image-161.png)
![alt text](image-162.png)

once after uploading,
![alt text](image-163.png) 


Before the Autopilot starts, we will get the start of OOBE page

![alt text](image-160.png)



## dynamic group for autopilot
![alt text](image-165.png)
query ` (device.devicePhysicalIDs -any (_ -startsWith "[ZTDid]")) `
![alt text](image-164.png)


verification
![alt text](image-166.png)


## Deployment profile

![alt text](image-167.png)

![alt text](image-168.png)


    - user driven moden
        - user will login to this machine with work account and initiate

![alt text](image-169.png)


![alt text](image-170.png)
    - only cloud / hybird ( onprem + cloud)

![alt text](image-173.png)

Profile has been created

---------------------------------------


## company branding ( optional from ENTRA ID, else microsoft default )
     to put org name and logo

![alt text](image-174.png)

![alt text](image-175.png)


### if new device, 
when powering on with OS, it will be default in OOBE mode

### existing
- as my cl2 is also in intune, we need to sysprep to get the OOBE ( out of box experience)

![alt text](image-176.png)

![alt text](image-177.png)
 - generalize => to wipe previous configurations

or

reset the PC
![alt text](image-178.png)
![alt text](image-179.png)
![alt text](image-180.png)

![alt text](image-181.png)
![alt text](image-185.png)


to test autopilot with
 - apps ( vlc)
 ![alt text](image-182.png)
-  device restriction with configuration profile
![alt text](image-183.png)


## allow preprovisioning deployment in Deployment profile
        - will not require User authentication, with help of tpm 2.0
![alt text](image-184.png)

mostly vendor will give to org ID mode

OOBE by pressing winKey 5 times,
 - it will directly enroll,
 - it will apply configurations,
 - it will compliance policy

 then reseal the device and will be given to end user

  - only things which has user context ( like userApp installation)
  - that will be done when user uses work account to login

like Zero touch ( SCCM, to save time )



### for kiosk machines,

new deployment profile
![alt text](image-186.png)



# -----------------------------------------------------

# handson 

create 5 users with license

users
Username: testuser1@th40032944.onmicrosoft.com
@Gt
@GtGt



device local admins ( clients)
Client1
Client1@12345

# -----------------------------------------------------
# Revision

# part : 1

task 
1. developers license
    - use work group account
    - authenticate
    - accept the consents
2. Domain controller setup
    - use Hyper v to create VM box
    - locate the Server ISO
    - install and start
        - set hostname
        - set the IP ( 192.168.0.9 , 255.255.255.0, 192.168.0.1)
        - update the firewall and rules
        - restart the vm
    - DC-setup
        - install the role ADDS, DNS
        - promote the server as domain controller
        - new forest
             - wintraining.com
        - update preferred DNS in IPV4 ( because the same server hosts DNS )

3. setup 2 win11 machine ( 1 with and 1 without domain joined )

    -   name: CL1-TH40032944 ( no domain join )
        - RAM: 4gb
        - gen 2
        - size 100gb
        - iso: win11
            - secure boot - no
            - enable tpm - yes
            - virtual processor - 2
        - install and start
        - set hostname
        - set the IP ( 192.168.0.10 , 255.255.255.0, 192.168.0.1)
        - update the firewall and rules
        - restart the vm
        - preferred DNS : 8.8.8.8 (google)
    

    -   name: CL2-TH40032944 ( no domain join )
        - RAM: 4gb
        - gen 2
        - size 100gb
        - iso: win11
            - secure boot - no
            - enable tpm - yes
            - virtual processor - 2
        - install and start
        - set hostname
        - set the IP ( 192.168.0.11 , 255.255.255.0, 192.168.0.1)
        - update the firewall and rules
        - restart the vm
        - preferred DNS : 8.8.8.8 (google)
     

# part : 2

cloud : 
 - IAAS
 - PAAS
 - SAAS


 portals
  - admin.microsoft.com
  - intune.microsoft.com
  - entra.microsoft.com

    - Active Directory - identity and Access, Ai & AP ( ADDS onprem )
    - Azure Active Directory - identity and Access, Ai & AP ( PAAS based cloud service )
        - addons like Default SSO for native products on  internet
 

Users after cloud:

- cloud users
- sync users ( created at onprem AD, later syncd to cloud )
- guest users


thulasi : tenant
 - thulasi.com
 - thulasi.net
 - thulasi.org


# Sync from onprem to AAD
 - login to onprem DC
 - install entra connect syc / AzureAD hybrid sync
 - authenticate to AAD tenant using global admin account
 - add the forest using the onprem Admin account
 - select the objects to sync ( devices, users, OU...)
 - before that create a OU and add 2 new users inonprem to see in AAD
 - initiate the manual sync
 - verify in entra portal
 - confirm if they are synced from onprem

# Groups in Portal
    - M365 group ( for communication and not for permission)
            - can only have users

        - group name: 
        - group desc:
        - group email: auto created
        - assign roles : to any users
        - membership type: Assigned user / Dynamic based on Query
        - owner 

    - security ( for permission and not for communication)
            - can have users + devices + apps

        - name:
        - desc:
        - members: assigned / dynamic users/devices
        - owner

# Device Enrollment
 - BYOD ( device belongs to user, managed by org)
 - CYOD ( device belongs to org, managed by org)


# 3 ways to manage ( intune join types )

  - Azure AD Registered
    - requires personal account
    - supported: android, ios, win11
    - BYOD

  - Azure AD joined
    - requires work or school (Entra ID)
    - Supported: win10 / win11
    - CYOD - corporate owned
    
  - Hybrid join
    - requires Domain account
    - Supported: win10 / win11


# Task - AD Join
  - 1. use CL1 device ( having internet connectivity)
  - 2. in entra portal create a user with license
  - 3. use the identity to join the CL1 machine 
  - 4. having the automatic enrollment in Intune, the device will be also enrolled in Intune
  - 5. reboot the client and setup the initial onboarding configs.

# TASK - AUTO ENROLL USING COMPANY PORTAL ( AD REGISTER )

    - 1. USE CL2 DEVICE ( HAVING internet connectivity)
    - search or install company portal using store
    - create one user and assign the license in entra portal
    - use the identity to login to intune in company portal
    - then accept mfa and other checks to register
    - verify in portal


# part - 3

Device enrollment 
    - default 5
    - max 15
    - also can be based on platform/type ( windows,mac ios, android)



Device Configuration Profile ( CI / CB in SCCM)
 - device restriction based on platform
    - disable bluetooth & cellular
    - assignments ( to group based on criteria)
    - rules ( enforce based on some applicability )



# part -4 



# part -5

## windows autopilot program




# questions

 - 1. how many accounts you can login to win7
        - win7 - local account, domain account
        - win8 - local account, domain account, microsoft account
        - win 10 - local account, domain account, AzureAD account  ( first to have direct cloud integration)
 - 2. 