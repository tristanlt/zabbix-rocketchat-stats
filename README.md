# Monitoring RocketChat with Zabbix

RocketChat is a team chat software (like Slack). Zabbix is a monitoring tools.

This repository contains some file to monitor RocketChat with Zabbix.

The template was tested on Zabbix 3.4.

This script needs *Personnal Access Token* feature on RocketChat. 

# Monitoring spectre

This template will track :
  * RocketChat Active Users (all users)
  * RocketChat Online Users (connected users, online or aways)
  * Rocket Total Channel Messages (Number of messages since instance creation)
  * Rocket Total Private Group Messages (Number of messages since instance creation)
  * Rocket Total Channels (Number of channels)
  * Rocket Total Private Groups (Number of channels)
  * Rocket Total Livechats (Number of chats)
  
The monitoring templates issues alerts:
  * Average: if no monitoring data is received withing 10 minutes
  * Information: on version change
  
The template contains graphs for the measures and a host screen which displays the graphs.

The external script caches the following data in the file **/tmp/rocketchat\_stats\_<numeric id of the user>.pickle**:
 * monitoring data is cached for 90 seconds for overhead reduction (configurable)
 * login token is cached until fetching of the stats fails to reduce login overhead 

# Installation

  * Install python3 to your zabbix server/proxy
  * Copy **rocketchat-stats** from  **server/externalscripts/** to your Zabbix server to **ExternalScripts** location (ie. /var/lib/zabbix/externals-scripts/)
  * Import RocketChat-Stats.xml from **templates/** into your Zabbix server
  * Add a RocketChat user with a role which contains permissions **view-statistics**
  * Create a rocketchat api keypair
    * Login user
    * Profile -> My Account -> Personal Access Tokens
    * Create a Personal Token 
  * Test access on the commandline
```
sudo -u zabbix /var/lib/zabbix/externals-scripts/rocketchat-stats all https://rocket_chat.fqdn/api ujjdjdhhhh7822232 gf-PAT-l_W4jhddggdggsshdhdhdhkkkjfbbdbddt332
```
  * Assign and configure the template to the host which is running zabbix
    * Open the macro view
    * Display the inherited values by "Inherited and host macros"
    * Overwrite the ```{$ROCKETCHAT_``` macros by your values

# Debug mode

* increase verbosity
* print all received data from RocketChat API to terminal
* always get data from RocketChat API instead of using cache file

Usage :

```
export ROCKETCHAT_STATS_DEBUG="True"
export RC_API='https://rocket_chat.fqdn/api'
export RC_ID='ujjdjdhhhh7822232'
export RC_TOKEN='gf-PAT-l_W4jhddggdggsshdhdhdhkkkjfbbdbddt332'
python rocketchat-stats onlineUsers ${RC_API} ${RC_ID} ${RC_TOKEN}
```

# TODO

These checks are on our roadmap.

 * Per channels/groups statistics
 * Dead channels/groups detection (less than X message by since y days)
 * Hot channels/groups detection (more than X message by since y days)
 * Orphaned channels/groups detection (0 members)
 * Dead users detection (no login since X days)
  
Feel free create issues for new features.
  
# License
GNU General Public License v3
