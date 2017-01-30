1/27/2017
===physical===
ESXi root password:	lab2017
Interface 1 (vmnic0)
IP: 			192.168.29.50/24
hostname:		linear
vswitch:		vmk0

Interface 2 (vmnic3)
IP:			192.168.29.40/24
hostname:		labrador

1/29/2017
===virtual===

p212 verified I see 2 physical nics correctly--vmnic0, vmnic3

see screenshot 1

p212 created management vswitch

p213 created IPS1 and IPS2 vswitch

p214 confirmed vswtich0 -> vmnic0

p214 created vswitch bridge and set uplink to vmnic3

p215 confirmed--see screenshot 2

p216 created bridged port group

p217 verified port groups list--see screenshot 3

p222 Final flight check
* ESXi installed
* did not need the fling
* vSphere installed on Win10
* 5 vswitches
* 5 port groups (1 was default)
* recognizing 128 GB RAM
* recognized two datastores correctly (screenshot 4)
* all above reflected on summary view (screenshot 5)

===guests===

p224 **staged all builds by creating the ISO folder and moving everything to linear **
 issue: ran into differences with the screenshot when browsing the datastore. Got a modal dialog that was different than screenshot
on p225. It did not refresh after I typed my new folder name and clicked the create directory button. In vSphere clinet I could 
verify that the directory got created. Datastore browser in the web UI did not ever show me the contents, but it worked on the vSphere 
client. The error logged was when I didn't provide a name for rthe directory (screenshot 6). The web UI allowed me to create the 
directory, just didn't show it to me (screenshot 7). Therefore I coldn't upload teh ISOs using the web UI.
 resolution: restarting the web UI fixed this, no big deal after all.

Uploaded other ISOs through vSphere without incident

p253 extracted metasploitable2

 issue: stalled on converting because I didn't grab the vmware converter while in my VMWare download portal
 resolution: will grab that tool Monday

p227 adding pfSense

p230
* set CPU, RAM, disk space successfully
* set thick provisioned lazy zero successfully
issue: could not select SATA controller 0
resolution: selected SATA controller 1 (this may be hardware dependent--I don't have a SATA 0)
see screenshot 8
p231 
* removed SCSI controller successfully
proofread: minor grammar error "we won't be needing it"
issue: didn't spot the "add network adapter" button immediately
resolution: scroll up in the wizard to see the button
* successfully provisioned adapters
* added CD/DVD successfully (same quirk as above)

p232

confirmed all settings--see screenshot 9
issue: some of the settings didn't show up on my onfirmation screen (provisioning, controlled)
resolution: none-I think this is just display/interace. Using the back button the correct values were there, although
it replaced the SCSI controller I removed when I went back

p233
* successfully created the pfSense vm

p234 
* confirmed disk settings were CORRECT
issue: network adapter settings needed to be corrected, see screenshot 10. Slightly different issue than in p234
resolution: fixed settings in edit before poweriung up the guest. My guess is using the back button in the wizard did this.

***POWERED ON!!!***

p238
nitpick: I had to click in the popup console window to get my keyboard to work

p239 
* closed and powered off
* successfully removed ISO mount

p241
issue: my pfSense won't boot, WAAH!
---taking a break---
plan to repeat the install since I have nothing invested in it yet

**repeated install successfully

error: p242 refers to virtualbox

issue: my ESXi does not show the MAC addresses of the network adapters when they are auto assigned.
resolution: Networking > Virtual switches > [click a name] vswitch topology reveals the mac address of the ports 
connected to the vswitch

Management: 00:0c:29:de:1f:7a
bridge: 00:0c:29:de:1f:84 (down, so determined by elimination)
IPS1: 00:0c:29:de:1f:70

See screenshot 11

WAN -> em2
LAN -> em1
OPT1 -> em0

Planned IP for WAN: 129.168.29.70

p243
Successfully configured WAN, static IP

clarification: "the wizard will ask you..." you need to run the wizard for each interface.

successfully configured all pfSense interfaces IP, see screenshot 12

successfully set up the easypass rule

nitpick: you don't say what to do to close the shell (ctrl-D or exit)

issue: failed to connect to pfSense web GUI at https://192.168.29.70/
from pfSense console, can ping the DGW (192.168.29.1) and workstation IP (192.168.29.109)
from workstation, cannot ping pfSense WAN (192.168.29.70) (maybe not supposed to work because RFC 1918 rules)
Checked physical network router for ARP entry for .70 (bzzzt my router sux)
Added DHCP reservation for .70 in the router, changed pfSense to DHCP on em2
see screenshot 13--this clearly worked, but the web GUI still won't load
tried reentering the easyrule

retried the web GUI
/var/log/filter/log shows the connection from my workstation getting dropped 
see screenshot 13

decided to RTF pfSense M https://doc.pfsense.org/index.php/Adding_Rules_With_easyrule

From here: https://doc.pfsense.org/index.php/Filter_Log_Format_for_pfSense_2.2
I can parse the log entry--this says rule 59 is causing my webGUI traffic to get dropped.

found this way to munge the rules by hand:
https://doc.pfsense.org/index.php/How_can_I_edit_the_PF_ruleset

no joy

tried option 11, restart web configurator (no joy)

+++decided to reset to factory default+++
I think the error on my part was saying yes to fall back to HTTP on web configurator. redoing the setup of pfSense

same result

---taking a break---

next attempt: try the webGUI right after issuing pfsense -d
see what pfsense -d writes to the log


resolution:
