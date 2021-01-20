RG Nets Training OOP
====================

Slides - Intro
-----
+ [001 RG Nets rXg intro](https://docs.google.com/presentation/d/1qYxrjJF7jJzmr2nIER8uguGfpe2RgP9Qk69d5fBPrkw/edit)
+ [011 rXg install bare metal](https://docs.google.com/presentation/d/1lxhKTTWGZp572GU9zVFxfgZe1FWOc1TUDNhChxP8q-U/edit#slide=id.g4837f8d2da_0_0)
+ [012 rXg install virtual](https://docs.google.com/presentation/d/1IX3gTkx6UxZz-udYpZAqKBRFH286uMxVa3OcwD4QWs4/edit)

Lab/Demo - Intro
--------
+ [Show Lucidchart of lab environment](https://lucid.app/lucidchart/14e0a4f9-aa56-49ed-a077-feff43c1608a/view?page=0_0#)
+ Assign Labs
+ Demonstrate virtual installation

Slides - Initial Configuration
---
+ [020 rXg Initital Configuration](https://docs.google.com/presentation/d/1fvzs3I_Z-XWQtbo-uGC4HCY5EXQRI4CLtcGlhZcii64/edit#slide=id.g49bcb7c211_0_0)

Lab/Demo - Initial Configuration
---
+ make a plan (Lucidchart)
+ demonstrate assigning WAN IP via CLI tools at first boot
+ access web interface for first time via pre-assigned WAN IP
+ talk about Admins
    + create first admin
+ talk about licensing
    + apply licensing
+ Set FQDN and Timezone
+ Talk about SSL Certificates
    + create/apply valid SSL certificate
+ add SSH public Key to admin record, and SSH to rXg

Slides - Network Configuration
-----
+ [030 rXg Network Configuration](https://docs.google.com/presentation/d/1lj5qB6TpNa4huzbzy4NEm44fcTxS5wuqNIYPPBJSKXM/edit#slide=id.g49bcb7c211_0_0)
+ [031 rXg vRG](https://docs.google.com/presentation/d/1AocpwMGkvplkk51R2zfUES6UA-nHiteNqKGSUWURt4E/edit#slide=id.g5f98161078_0_181)
+ [032 Location Integration](https://docs.google.com/presentation/d/1LnLof0Rq_ri2ddiwM4BKwgR33Gi-Cab_q5lhLBvLuX0/edit#slide=id.g7056244fe6_0_4)

Lab/Demo - Network Configuration
----
+ Discuss existing LAN configuration
+ Build first VLAN 
    - Name: Onboarding VLAN
    - Physical Interface: igb1
    - VLAN ID: 101
    - Autoincrement: Disable
    - Autoincrement Ratio: blank
+ Create first network address
    - Name: Onboarding Address
    - Ethernet: none
    - VLAN: Onboarding VLAN
    - Primary: unchecked
    - IP: 10.0.0.1/24
    - Autoincrement: 1
    - DHCP: Leave unchecked
+ Change subnet to /30
+ change "Autoincrement" to 64  
    + Why 64? 
+ edit VLAN autoincrement-ratio to 8
    + How many VLANs will we end up with? (8)
+ change VLAN autoincrement-ratio to 16
    + How many VLANs will it change to? (4)
+ change VLAN autoincrement-ratio to 1
    + How many VLANs will it change to?(1)
+ create DHCP pool for Onboarding
    + Name: Onboarding DHCP Pool
    + Start IP: 10.0.0.2
    + End IP: 10.0.0.254
+ Create second VLAN for "Residents"
    - Name: Residents VLAN
    - Physical Interface: igb1
    - Vlan ID: 1001
    - Auto-increment: enable
    - Auto-increment ratio: 1
+ create second Network address for "Residents"
    - Name: Residents Addresses
    - ethernet: none
    - VLAN: Residents VLAN
    - Primary: unchecked
    - IP: 172.20.1.0/24
    - Autoincrement: 100
    - DHCP: Checked
+ Create Third VLAN for "Guests"
    - Name: Guest VLANs
    - Physical Interface: igb1
    - VLAN ID: 2001
    - Autoincrement: Disable
    - Ratio: blank
+ Create third Network Addres for "Guests"
    - Name: Guest Addresses
    - Ethernet: none
    - VLAN: Guest VLANs
    - IP: 192.168.10.1/30
    - Autoincrement: 256
    - DHCP: Checked

Slides - Identity/Policy Configuration
-----
+ [050 rXg Identity - Policy - Enforcement](https://docs.google.com/presentation/d/1rmKTIkoK7d2hzRaxJrsELquwDQEjBppGFzCPMyu_oXY/edit#slide=id.g83f70dbc05_1_36)

Labs/Demo - Identity Configuration
----
+ Create IP group for Onboarding Networks
+ Create IP group for Residents Networks
+ Create IP group for Guest Networks
+ Create Account Group for "Basic Residents"
+ Create Account Group for "Advanced Residents"
+ Create Account Group for "Free Guests"
+ Create Account for "VIP Guests"
+ Create Shared Credential group for "quick Free button" 
+ Create Shared Credential for "Conference User"

Labs/Demo - Policy Configuration
----
### Policy skeleton
+ Create Policy for "Onboarding"
    - Name: Onboarding
    - IP Groups: Onboarding 
+ Create Policy for "Basic Residents"
    - Name: Basic Residents
    - IP Groups: Resident Networks
    - Account Groups: Basic Residents
+ Create Policy for "Advanced Residents"
    - Name: Advanced Residents
    - Account Groups: Advanced Residents
+ Create Policy for "Free Guests"
    - IP Groups: Guest Networks
    - Account Groups: Free Guests
+ Create Policy for "VIP Guests"
    - Account Groups: VIP Guests
+ Create Policy for "Free Button Guests"
    - Shared Credential Group: free
+ Create Policy for "Conference Users"
    - Shared Credentail Group: Conference User

### Basic Portal Setup 
+ Create Splash Portal for "Onboarding"
    - Name: Splash Portal
    - rXg Portal: Default
    - Policies: Default, Onboarding
+ Create Landing Portal for "Landing"
    - Name: Landing Portal
    - rXg Portal: Default
    - Portal Mode: MAC or Cookie
    - Background Mode: MAC
    - Session Limit: unlimited
    - Idle timeout: 20 Mins
    - Policies: Basic Residents, Advanced Residents, Free Guests, VIP Guests

### Payment Gateway Setup
+ Visit [developer.authorize.net](https://developer.authorize.net/)
    - Create Sandbox Account
    - Create Sandbox Account
    - Developer Type: Open-source
+ Create Payement Gateway
    - Name: Authorize.net
    - Direct Gateway: Authorize.net
    - Login: API ID
    - Password: Transaction Key
    - Test Mode: Enabled

### Usage Plan Setup
+ Create Time Plan "Unlimited"
    - Name: Unlimited
    - Price: 0
    - Time: Unlimited
+ Create Quota Plan "Unlimited"
    - Name: Unlimited
    - Price: 0
    - Download: Unlimited
    - Upload: Unlimited
+ Create Quota Plan "50Gb"
    - Name: 50Gb
    - Price: 0
    - Download: 50Gb
    - Upload: 10Gb
+ Create Misc Plan "$30"
    - Name: $30
    - Price: 30
+ Create Misc Plan "$60"
    - Name: $60
    - Price: $60
+ Create Misc Plan "$5"
    - Name: $5
    - Price: $5
+ Create Usage Plan for Basic Residents
    - Name: Resident - 10Mbps
    - Account Group: Basic Residents
    - Splash Portal: Splash Portal
    - Usage: Unlimited
    - Quota: Unlimited
    - Misc: $30
    - Relative Lifetime: 30 Days
    - Automatic Login: Enabled
    - Max Sessions: Unlimited
    - Max Devices: Unlimited
    - Merchant: Authorize.net
    - Interval: Monthly
    - Expiration: Never
+ Create Usage Plan for Advanced Residents
    - Name: Resident - 50Mbps
    - Account Group: Advanced Residents
    - Splash Portal: Splash Portal
    - Usage: Unlimited
    - Quota: Unlimited
    - Misc: $60
    - Relative Lifetime: 30 Days
    - Automatic Login: Enabled
    - Max Sessions: Unlimited
    - Max Devices: Unlimited
    - Merchant: Authorize.net
    - Interval: Monthly
    - Expiration: Never
+ Create Usage Plan for Free Guests
    - Name: Guest Free Access
    - Account Group: Free Guests
    - Splash Portal: Splash Portal
    - Usage: Unlimited
    - Quota: Unlimited
    - Misc: none
    - Relative Lifetime: 1 Days
    - Automatic Login: Enabled
    - Max Sessions: Unlimited
    - Max Devices: Unlimited
    - Expiration: Never
+ Create Usage Plan for VIP Guests
    - Name: Premium Guest 50Gb
    - Account Group: VIP Guests
    - Splash Portal: Splash Portal
    - Usage: Unlimited
    - Quota: Unlimited
    - Misc: $5
    - Relative Lifetime: 1 Days
    - Automatic Login: Enabled
    - Max Sessions: Unlimited
    - Max Devices: Unlimited
    - Merchant: Authorize.net
    - Interval: On-Demand
    - Expiration: Never

Labs/Demo - Enforcement Configuration
----
+ 