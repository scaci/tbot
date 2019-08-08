# Telegram BOT for OpenWrt

This is a set of scripts to manage OpenWRT Routers using a Telegram BOT

### How its works ?

First of all,
Make a bot for you !
https://core.telegram.org/bots/api#authorizing-your-bot

With the bot created, you need to replace "[PUT YOUR BOT KEY HERE]" in /etc/config/tbot file with your bot key.

Second, you need to send a initial message to your bot in Telegram App.
After you send the message, in the OpenWRT run this:

``` curl -s -k -X GET https://api.telegram.org/bot<YOUR BOT ID>/getUpdates | grep -oE "\"id\":[[:digit:]]+" | head -n1 | awk -F : '{print $2}'```

Get the number and replace "[PUT ID OF THE CHAT THAT YOU START WITH BOT]" in /etc/config/tbot.

```
config tbot 'global'
        option api 'https://api.telegram.org/bot'
        option key '[PUT YOUR BOT KEY HERE]'
        option chat_id '[PUT ID OF THE CHAT THAT YOU START WITH BOT]'

```

### Directory structure

```

TO DO

```
#### tbot service

```

Syntax: /etc/init.d/tbot [command]

Available commands:
        start   Start the service
        stop    Stop the service
        restart Restart the service
        reload  Reload configuration files (or restart if service does not implement reload)
        enable  Enable service autostart
        disable Disable service autostart

```

#### plugins

All the commands that the telegram bot can execute.

There are some pre-built commands, which are:

 * Block: Creates a REJECT rule for a host (by macaddr) to make NAT for the internet.
 * Fwlist: Lists all firewall rules.
 * Gethosts: Lists the hosts that are in dhcp.leases, if the argument is a host, will list only the host found by regex.
 * Getip: WAN IP.
 * Start: Creates command help.
 * Unblock: Removes a firewall rule created by Telegram, without an argument, all rules created by Telegram are deleted.
 * Wifilist: List all devices connected to WiFi.

#### help for plugins

Help for available commands


#### telegram_bot

The telegram_bot script is a loop that receives the updates every second and checks to see if there is a command to execute. If there is a command, the script checks to see if there is a file with the same name as the command inside the plugins directory and runs it, if it exists. The output of the executed script is sent as a response message from the command.
Inside the plugins directory, there is the special command "start", which returns a message with the commands and a brief help on each command.
