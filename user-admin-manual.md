---
layout: doc-with-toc
title: User & Admin manual
permalink: /user-admin-manual.html
description: A manual for both users and administrators, using and setting up your environment.
---
# User Manual

Last updated: {{ site.time | date: '%B %d, %Y' }}

## Online management and logging in

You can find {{site.compass.reseller.prodname}} at [www.{{site.compass.reseller.domain}}](https://www.{{site.compass.reseller.domain}}/){:target="_blank"}.

You need to enter your username and password and press 'Log on' to enter the site.

## My settings

Here you find information about your settings and the availability of the phone.

### Personal information

Here you can adjust your password and email address.

### Phone

Here you can find which phone you are currently logged on to and see which extensions and external numbers are associated with your account. If you have write permissions to the company, you are also able to change the phone you are logged on to.

The voicemail section will tell you if you have any messages waiting in your voicemail boxes.
To listen to voicemail messages, see [Listening to voicemail](#listening-to-voicemail).

### Status

You can set your status to the following values:
 - Available
 - No queue calls - blocks queue calls but allows direct calls.
 - No calls - blocks all calls, both direct as well as from a queue.

A queue can be configured to ignore user status. This allows for calls from high priority queues to still get through even if your status is 'No queue calls' or 'No calls'. For more details on this setting, see the [Ignore user status](#ignore-user-status) section.

When you're an agent for a queue that has wrap-up time configured, your status is automatically set to 'No queue calls' after completing a queue call, and reverted back to 'Available' when wrap-up time ends.

### User forwards

Here you can manage forwarding of incoming calls, e.g. forward immediately (Always), when you are busy (Busy), could not answer the phone (No Answer) or are not reachable (Unavailable). By setting the ring time you decide how long the phone rings before following the No Answer forward. Checking the CLI transparent box will display the original caller ID at the destination, instead of the caller ID of the forwarding party.

### Call flow switches

This list contains up to 10 call flow switches present in your company. If there are more than 10 switches, you can click through to the overview page to see them all. Next to the switch is a pencil icon, which allows you to change the setting of the call flow switch.

### Queues

Here you can find the queues to which you have been added, e.g. the type of phone calls you can expect (for example sales or support).

## Listening to voicemail

You are able to reach the voicemail box by phone in the following ways:
* Call 1233 with a logged on user and enter the PIN code when prompted
* Call the phone number for which the voicemail box is set, press \* during the welcome message and enter the PIN code when prompted

The structure of the voicemail menu is as follows:
* Main menu - "You have X new and Y old messages."
  If there are any messages, they are played. If not, the settings menu is played.
* During playback of a message you have the following choices:
  * 2 Pause
  * 4 Rewind message 5 seconds
  * 5 Replay message
  * 6 Fast forward message 5 seconds
  * \* Message menu
* After playing a message the message menu plays. Here you have the following choices:
  * 4 Previous message
  * 5 Repeat message
  * 6 Next message
  * 8 Delete message
  * 9 To settings menu
  * \* Help
  * \# Main menu
* These are the choices in the settings menu:
  * 1 Play welcome message
  * 2 Record welcome message
  * 5 Change PIN code
  * \* Main menu

## Service codes

You can change your telephony settings by dialling service codes. Service codes always start with an asterisk (\*), followed by the desired service code and followed by the settings, depending on the requested service code. You can also configure these service codes as speed dials with the function keys of your phone, if programming function keys is supported for your type of phone. Below you can find the currently implemented service codes and their explanation.

### \*1: Log on and log off from your phone

By dialling \*1, you can log on to your phone. If you were not logged on, you will hear a voice, asking you to enter your extension, followed by the pound key (\#). If you have configured a PIN code for your extension, you will be requested to enter the code as well. If the PIN code is correct, you will hear a voice indicating you have been logged on.

If a PIN code is not configured for your extension, you will not be asked for it either. When the entered extension is known within your company, a voice will directly tell you you have been logged on.

Whenever you enter an invalid extension, you will be informed of this and the phone will be hung up immediately. If you enter a correct extension, but an invalid PIN code, you may retry your PIN code up to two times. Therefore, if you enter an invalid PIN code three times in a row, the phone will be hung up.

If you were already logged on to the phone and dial \*1, you will be logged off of your phone. This will also be indicated by the voice, saying “goodbye”.

### \*48: Log in and log out from a queue

You can dial a service code, starting with \*48, to log in or log out from a queue within your company. The service code has to be followed by another asterisk and the queue short code. This queue short code can be configured by your administrator.

By default, when you log in to a queue, your identity will be configured in a way that your user forwards will **not** be followed in case you receive a call from the queue. Next to that, your identity will have priority 1 in the queue. This implies that your phone will ring immediately, according to the queue strategy, when an outside caller enters the specified queue.

It is possible to change these default settings by appending more parameters when dialling the service code. All parameters are separated by asterisks (\*). As mentioned, the first setting is mandatory and is the queue short code. The second setting is optional and is used to indicate whether your user forwards should be followed. A one (1) tells to follow them, a zero (0) lets the platform know you do not want your forwards to be followed.

With the last setting, you can control with which priority you will enter the queue. This priority can be either 1, 2 or 3. The priority indicates the amount of use of the queue that should be reached, before incoming calls will be attempted to be delivered on your phone.

The optional settings can only be configured when you are currently logged out from the queue. If you pass extra settings whilst you are already logged in to the queue, you will be logged out from the queue. The extra settings will be ignored.

Below are a few examples to clarify the use of \*48:

\*48\*8
If you are logged on to the queue with short code 8, you will be logged off. If you are not currently a member of the queue, you will be added to it with priority 1 and have your user forwards disabled if you receive calls from this queue.

\*48\*2\*1
Log in to the queue with short code 2 with priority 1. Your user forwards **will** be followed when you receive calls from that queue.

\*48\*4\*0\*2
Log in to the queue with short code 4 with priority 2. Your user forwards will **not** be followed.

### \*49: Global queue pause

Using \*49 the calling user can manually toggle their user status between 'Available' and 'No queue calls'. User status 'No queue calls' means the user can only receive direct calls until their user status is set to 'Available' again. For more details on the effects of user status, see the [User status](#status) section.

### \*55: Set call flow switch

In order to set or check the setting of a call flow switch from a phone, you can dial a service code starting with `*55*`. The service code is followed by a switch shortcode and optionally a new switch setting number. The shortcode of the switch and the possible settings can be configured by your administrator.

To check the current switch setting, dial `*55*(switch shortcode)`. A prompt will indicate the number of the current switch setting. This number ranges from 1 to (maximum) 100 and corresponds with the number of the switch branch.

To change a switch setting, dial `*55*(switch shortcode)*(new switch setting number)`. A prompt will confirm that the setting of the call flow switch has changed. For backwards compatibility switch setting 0 will be translated to setting 10.

Examples:

* Dial `*55*123` to hear a prompt that identifies the current switch setting of the call flow switch with shortcode 123.
* Dial `*55*123*55` to change the setting of the call flow switch 123 to the setting with number 55.
* Dial `*55*123*0` or `*55*123*10` to set switch 123 to setting 10.

### \*8: Pickup of a call coming in on another phone

When the phone of a colleague within your company is ringing, you are able to take over the call and pick up on your own phone. There are two ways of doing this:

\*8(extension of ringing phone) This is to take over the call from the ringing phone. There is an instant connection with the caller.

\*8 This is to take over a random call within your company. This option is useful when you are not sure which phone exactly is ringing. When multiple phones are ringing, a random call is picked up.

It is possible the administrator of your organisation has limited the use of call pickup. If this is the case, you are only able to pick up certain calls: calls that are coming in with users that are in the same group as you.

## XMPP chat

For connecting to XMPP chat you can use any XMPP client.

### Clients

Desktop clients:
* Gajim for Windows and Linux - [gajim.org](https://gajim.org/){:target="_blank"}
* Swift for OSX/macOS - [swift.im](https://www.swift.im/){:target="_blank"}

Mobile clients:
* Conversations for Android - paid from [Google Play](https://play.google.com/store/apps/details?id=eu.siacs.conversations){:target="_blank"} or free (donation) from [f-droid.org](https://f-droid.org/){:target="_blank"}

For more clients we suggest having a look at [xmpp.org/software/clients.html](https://xmpp.org/software/clients.html){:target="_blank"}

### Configuration
Please use these settings to set up your XMPP client:

If only a username and a password is requested and there is no separate XMPP server input field, use a so called *user address*, Jabber ID or JID:
* Username/JID: your-username@uc.{{site.compass.reseller.domain}}
* Password: your-password

If username and hostname are separate input fields:
* Username: your-username
* Hostname / XMPP server: uc.{{site.compass.reseller.domain}}
* Password: your-password

If your organisation makes use of a company domain (your username looks like 'user@example.com' instead of 'user@uc.{{site.compass.reseller.domain}}'), you might need to set a Connect Server in your client's settings:
* Connect Server: uc.{{site.compass.reseller.domain}}

Note: after 20 incorrect login attempts, the source IP address will be blocked from logging in to XMPP for 10 minutes.

Warning: in organisations with many users (hundreds or more) the contact list might not contain all contacts because of a platform limitation.

# Administrator Manual

## Online Management and Login

You can find {{site.compass.reseller.prodname}} at [{{site.compass.reseller.domain}}](https://www.{{site.compass.reseller.domain}}/){:target="_blank"}.

You need to enter your username and password and press Login to enter the site.

## User Levels

There are several levels which vary according to the user's rights and the availability of the buttons. These levels are described below.

1. **End-user**: This shows the options My settings and Log out. You can adjust personal settings, see which phone is available and manage forwarding and queue settings.
2. **Organisation manager**: You can manage everything within your own organisation.
3. **Reseller**: You have an extra button named Organisations. Here you can select the organisation you want to manage, and use the menu Manage to adjust as needed.
4. **Root Reseller**: You are able to manage all resellers.

## Quick Start Guide

This quick start guide will show how to set up a new company. For a more extensive guide, including images, see the manual.

### Add an Organisation

1. Click 'Organisations'.
2. Select your reseller account.
3. Click 'Add company'.
4. Enter the name of the organisation.
5. Enter the short name that has been given to you. It is very important that the correct name is entered here, it cannot be changed after you click 'Save'! The SIP domain should complete automatically. If it doesn't please paste the short name in front of the URL. Choose the language. Click 'Save'.
6. The new company had been added.

### Organising a Company

To be able to function a company needs at least the following elements:

1. A User
2. A Phone
3. An External number
4. An Extension
5. A Dial plan

### Add an External Number

Adding an external number is the first step. You will need an external number to enter at the next step, add a new user.

1. Go to 'Manage', 'Dial plan', 'External numbers'.
2. Click 'Add external number'.
3. Enter the external number, description and Name shown at outbound call. Click 'Save'.
4. The new external number had been added.

### Add a User

1. Go to 'Manage, 'Users', 'Users'.
2. Click 'Add user'.
3. Enter the data, if you also enter an Extension here, it will be automatically added. Click 'Save'.
4. The new user has been added.

### Add a Telephone

1. Go to 'Manage', 'Phones'.
2. Choose 'Add phone'.
3. Choose the Phone Model, and enter the required data. Click 'Save'.
4. The new telephone has been added.

### Add an Extension

If a user hasn't been given an extension when he or she was added, you can enter one here.

1. Go to 'Manage', 'Dial plan', 'Extensions'.
2. Click 'Add extension'.
3. Enter the extension and description. Click 'Save'.
4. The new extension has been added.

### Enter a Dial plan

Please enter a very simple dial plan at first, for example, call > user. Try to call to +3185-7739999 and also execute an incoming test call. If this doesn't work, find out where the error lies. If this does work, you can enter a more extensive dial plan. Dial plans can be entered for both extensions and external numbers. This example features an external number.

1. Go to 'Manage', 'Dial plan', 'External numbers'.
2. Choose the number you want to add a dial plan to.
3. Drag-and-drop desired elements from the left-hand menu to the canvas. For example, IVR menus, users or voice prompts.
4. Save the dial plan by clicking on the Save button, in the top right corner of the dial plan canvas.

## Organisations and Resellers

Only resellers can see and use the 'Organisations' button. It shows the list of organisations which are placed under your Reseller account.

Click on an organisation. The Manage organisation page will now show. You can manage this organisation using the Management menu. The name of the currently managed organisation will be mentioned on the left of the top menu bar.

### Add an Organisation

By selecting a reseller you have write permissions to in the Organisations overview, you can add an organisation. Click Add company to go to the Add organisation page.
* **Organisation name**: Enter the full name of the company
* **Short name**: Enter an abbreviated or descriptive name for the company, this will for example be used as the identification for this company on phones when logged on. This name can only contain lower case letters, digits and can be between 2 and 12 characters long. Be aware that the short name can not be edited after saving.
* **Select language**: A default language can be set for the company
* **Domain name**: It is possible to use your company's domain name to log in. This means usernames would look like "user@yourdomain.com". Ownership of the domain will have to be confirmed by adding a DNS entry. This field can also be left empty.

When finished click Save.

You can add an organisation by clicking Add company. Enter the full and abbreviated name of the company. The latter should be between 2 and 12 characters long and can only contain the lowercase letters A through Z and digits. Leave the SIP domain as is. Also, select the desired default language of the new company. Finally, click Save.

Note: if you were managing another company, you should first reselect your reseller from the Organisations menu. Otherwise, you will remain at the organisation level and the option Add company will not be shown.

In order to configure a domain (example: example-domain.ext) for a {{site.compass.reseller.prodname}} organisation, the following has to be set:

| Host | Type | Priority | TTL\* | Value (weight, port, server) |
| --- | --- | --- | --- | --- |
| _xmpp-server._tcp.example-domain.ext | SRV | 100 | 7200 | 33 5269 xmpp1.{{site.compass.reseller.domain}} |
| _xmpp-server._tcp.example-domain.ext | SRV | 100 | 7200 | 33 5269 xmpp2.{{site.compass.reseller.domain}} |
| _xmpp-server._tcp.example-domain.ext | SRV | 100 | 7200 | 33 5269 xmpp3.{{site.compass.reseller.domain}} |
| _xmpp-client._tcp.example-domain.ext | SRV | 100 | 7200 | 33 5222 xmpp1.{{site.compass.reseller.domain}} |
| _xmpp-client._tcp.example-domain.ext | SRV | 100 | 7200 | 33 5222 xmpp2.{{site.compass.reseller.domain}} |
| _xmpp-client._tcp.example-domain.ext | SRV | 100 | 7200 | 33 5222 xmpp3.{{site.compass.reseller.domain}} |

\* You may choose your own TTL, 7200 is a safe default.

Summarizing, the end user must set:
* 6 SRV records in total on their domain with equal priority (100) and weight (33) to hosts xmpp1, xmpp2 and xmpp3 on {{site.compass.reseller.domain}}.
* The first 3 records in the table above are for server connections with destination XMPP server with port 5269. This is required for XMPP Federation, so (sending) XMPP servers know which (receiving) XMPP servers to deliver messages to for your domain.
* The last 3 are for client connections with destination XMPP client with port 5222. This is required so XMPP clients know which XMPP servers they should connect to.

Note:

* If these DNS entries don't exist, certain features will not function well or not at all.
* {{site.compass.reseller.prodname}} checks the DNS settings when adding the domain to the company, and will not allow adding a domain if DNS is not correctly configured. Please consult with your DNS provider for more information.

After creating the required DNS records, you can double check the results with the "dig" command line tool (on Linux/Unix/macOS) like this:

```
$ dig SRV +noall +answer _xmpp-server._tcp.example-domain.ext
_xmpp-server._tcp.example-domain.ext.  7200 IN SRV 100 33 5269 xmpp3.{{site.compass.reseller.domain}}.
_xmpp-server._tcp.example-domain.ext.  7200 IN SRV 100 33 5269 xmpp1.{{site.compass.reseller.domain}}.
_xmpp-server._tcp.example-domain.ext.  7200 IN SRV 100 33 5269 xmpp2.{{site.compass.reseller.domain}}.
```
```
$ dig SRV +noall +answer _xmpp-client._tcp.example-domain.ext
_xmpp-client._tcp.example-domain.ext.  7200 IN SRV 100 33 5222 xmpp3.{{site.compass.reseller.domain}}.
_xmpp-client._tcp.example-domain.ext.  7200 IN SRV 100 33 5222 xmpp1.{{site.compass.reseller.domain}}.
_xmpp-client._tcp.example-domain.ext.  7200 IN SRV 100 33 5222 xmpp2.{{site.compass.reseller.domain}}.
```
Note:

* The order of records in the output of the dig command is random.
* 7200 is the default TTL and could be different in your results.

#### Edit an Organisation

A company can be edited by clicking Edit organisation on the Manage page of the company you wish to edit. The Edit organisation page loads, where you can modify the following settings:
* **Organisation name**: Edit the name of the organisation
* **Music on hold**: This is the default music played when a caller is put on hold and no other element within {{site.compass.reseller.prodname}} has music specified
* **Default CLI**: Sets the default outgoing phone number for external calls. If a user is not logged in on a phone, or only internal calls is set for a user who calls for example an emergency service number from a registered phone, this will be the phone number visible for the receiving party
* **Default Codec Profile**: When set to 'Default', high quality audio codecs are selected. When 'Low bandwidth' is set, lower quality audio codecs will be provisioned. These lower quality codecs will use up less bandwidth, but audio will be of a lesser quality as well. 'Default' is the advised setting
* **Select language**: The language set in this field will be the default language for the company. If individual users wish to use a different language, they can change this in their user settings.
* **Call-Pickup within group**: When enabled, picking up a call on another user's phone is only allowed when both users are in the same group. By default this option is disabled, which allows users to pick up any incoming call within the organisation
* **Change the domain name for this company (link)**: Redirects to the Edit company domain page, where a company domain can be set or edited. For more information on this subject, see the [Add an Organisation](#add-an-organisation-1)
On the Edit organisation page, click 'Save' to confirm any changes made or Cancel if you wish to discard all changes.

You can remove a company by clicking 'Delete' behind the company name. You will be asked to confirm the deletion.

### Resellers

An overview of all resellers can be found by going to Organisations in the main menu. To sort the list by reseller, click Type to separate resellers from companies.

Adding a reseller can be done by clicking Add reseller, but keep in mind you have to be on reseller level in order to be able to see this option.

On the Add reseller page, the following options can be set:

* **Name reseller**: Fill out the name of the reseller
* **Email address**: The email address filled out in this field will be used as the sender address in password emails to users. Notifications about added or deleted users will be sent to this address as well.
* **Base domain**: Only applies when creating white label resellers. Leave empty otherwise.
* **Web interface URL**: This URL will be used as the URL to the {{site.compass.reseller.prodname}} web interface in password emails to users.
* **Select language**: The language selected will be the default language used for this reseller
* **No-reply email address**: This will be the email address used as the sender address in voicemail and call recording emails
* **Allowed trunks**: Selection of trunks that can be assigned to new or existing organisations
* **Default trunk**: Trunk assigned to new organisations on creation
When finished, click Add.

**A note regarding email delivery:** Make sure that the email domains used for the fields 'Email address' and 'No-reply email address' allow {{site.compass.reseller.prodname}} to send email on behalf of those domains. This will help prevent spam filters from blocking emails. Include our SPF entry in the DNS SPF record of the domains, like so: `v=spf1 include:spf.{{site.compass.reseller.domain}} ~all`

If you wish to review the settings of a reseller, select the reseller in the Organisations overview and click Manage in the main menu. On the Manage page you can click Edit organisation to edit the reseller.

To permanently delete a reseller, click Delete organisation. You will be requested to confirm the deletion. In order to be able to remove a reseller, all companies within this reseller should first be deleted or moved to another reseller.

## Manage

Organisation managers can manage and configure various options and set up dial plans. The dial plan will be discussed elsewhere in this manual.

If you click Dial plan you will find several options to adjust the phone system of your department according to your needs. A list appears allowing you to create new records for the elements under Dial plan. These links will take you to the Add resource screens which belong to that specific element. If you want to select the element itself, you don't click on Dial plan but just hover your mouse over it followed by clicking on the element of your choice.

### Extensions

Extensions are the 2, 3, 4 or 5 digit numbers at which you can be reached through the internal network. To avoid confusion for example with emergency telephone numbers like 112, the first number of extensions can not be 0 or 1. You can create extensions in the range from 20 to 99, 200 to 998 (with the exception of 911), 2000 to 9999 and 20000 to 99999. You create an extension for one of your colleagues. Your colleague can use this extension to log in on any phone by pressing \*1 followed by the extension. Next the PIN code is prompted to be entered, but only when a PIN code is set.

By clicking on the description in the extension overview, you open the dial plan, where the desired path for an incoming call can be decided. A green check mark behind a name/extension means a dial plan has been configured for this extension.

By clicking on one of the categories the overview is sorted.

You can add a new extension by clicking 'Add new extension', underneath the overview. The 'Add extension' page loads. Enter the internal number in the 'Extension' field and in 'Description' a name corresponding to the extension, which will be visible in the overview list. This can be an extension number or a more detailed description like someone's name. Click 'Save'.

Creating a new extension can also be done while adding a new user (see 'Users').
Editing an extension can be done by clicking the Edit button (pencil icon) to the right of the description of the extensions overview list. The 'Edit extension' page will load where changes can be made.

Deleting an extension is done by clicking the delete button (bin icon). You will be requested to confirm the deletion.

### External Numbers

External numbers are phone numbers which are used to reach your company. These numbers are already provided by your telecommunication partner. You can add and remove external numbers yourself.  **Warning:**  if you remove an external number, you can no longer be reached on this number! If you have new external numbers you can add those yourself, as explained below. By clicking on the description of the number the dial plan for the external number will be opened. The dial plan is explained elsewhere in this manual.

If you click 'Add external number' above the overview the 'Add external number' page will load. Here you can enter the number in the 'External number' field. In the 'Description' field a name can be filled out which will be visible in the overview list. This could be just the number, or, if desired, a more comprehensive description.

The name (16 characters max) the receiving party of the call will see in its display is set in the 'Name shown at outbound call' field. When finished click 'Save'.

You can edit an external number by clicking the Edit button (pencil icon) to the right of the description in the overview. The 'Edit external number' page will load where the desired changes can be made. To delete an external number click the delete button (bin icon). You will be requested to confirm the deletion. **Warning:**  If you remove an external number you can no longer be reached on this number!

### Voice Prompts

You can add your voice prompts by uploading audio files. {{site.compass.reseller.prodname}} recognizes all sorts of different audio files, like .mp3 and .wav, which it converts to a usable format.

Click 'Add prompt' to upload a new audio file. The page 'Add Voice prompt' loads. By clicking 'Select' you are able to browse your files and select the audio file you want to upload. The name entered in the 'Name' field will be visible in the Voice prompt overview list. The 'Description' is where you enter the content of the voice prompt (for example: 'Good afternoon, welcome to Example Company). When finished click 'Save'.

To change the name and description of the voice prompt, click the Edit button (pencil icon). The 'Edit prompt' page will load.
Remove a voice prompt by clicking the delete button (bin icon). You will be requested to confirm the deletion.

### Music on Hold

You can add new categories by clicking 'Add music on hold'. After entering a name, click 'Next'. The 'Edit category music on hold' loads, where audio files can be added. Next to the 'New sound prompt' field, click 'Select' to browse through your files and select an audio file. It is possible to add more than one file to a category. When more than one file is added to a category, files will be played in alphabetical order. When finished click 'Save'.
Categories can be edited and audio files can be added and removed by clicking the Edit button (pencil icon). The 'Edit category music on hold' page will load.
To delete a category and its files click the delete button (bin icon). You will be requested to confirm the deletion.

### Queues

New queues can be created in two ways: by clicking the `Create queue` button on the queue overview page, or via the dial plan editor, selecting 'queues' from the left sidebar, and then clicking the `Create queue` button.


Some settings are not accessible during the queue creation process. You can change these other settings after creating the queue by navigating to either the queue overview page and then selecting the queue or the dial plan editor, selecting the queue, and then clicking the `Advanced options` button.

* **Name**: allows you to enter the name of the queue.

* **Shortcode**: code for the queue which can be used to log in and out of the queue on a phone by dialling *48*, followed by the shortcode. This code has to be a value greater than 0 and can contain a maximum of 5 numbers. This setting can only be changed after the creation process.

* **Ringing strategy**: determines how the queue calls agents logged in on the queue. The following options are available:
  * **Ring all phones simultaneously (default)**; all phones in the queue will ring at the same time.
  * **Ring all phones alternatively**; one phone at a time will receive a call.
  * **Ring phone least recently called from this queue**; phone least recently called will receive the call first.
  * **Ring phone with fewest calls from this queue**; phone to have received the least calls will ring first.
  * **Ring phone randomly**; phones will ring randomly.
  
* **Possible to connect to queue**: This setting can only be changed after the creation process.
  * **If at least one agent is logged in (default)**: new calls are placed in the queue if at least one queue agent is logged in on a phone, no matter what their user status is. If no queue agents are logged in on their phone, new calls are not placed in the queue and follow any remaining steps in the dial plan.
  * **If at least one agent is available**: new calls are placed in the queue if at least one queue agent is logged in on a phone and has a user status of 'Available'. If the user status of all queue agents is 'No queue calls' or 'No calls', new calls are not placed in the queue and follow any remaining steps in the dial plan. This behaviour can be overwritten with the 'Ignore user status' queue option.
  * **Always**: new calls are always placed in the queue, regardless of whether queue agents are logged in on their phone, or their user status.

* **Stay connected to queue**: This setting can only be changed after the creation process.
  * **While at least one agent is logged in (default)**: already waiting calls remain in the queue if at least one queue agent is logged in on a phone, no matter what their user status is. When the last queue agent logs out of their phone, waiting calls are removed from the queue and follow any remaining steps in the dial plan.
  * **If at least one agent is available**: already waiting calls remain in the queue if at least one queue agent is logged in on a phone and has a user status of 'Available'. If the user status of all queue agents is 'No queue calls' or 'No calls', waiting calls are removed from the queue and follow any remaining steps in the dial plan. This behaviour can be overwritten with the 'Ignore user status' queue option.
  * **Always**: already waiting calls remain in the queue, regardless of whether queue agents log out of their phone, or change their user status.

* **Maximum waiting time**: Time in seconds a call can stay in the queue (max. 65535 seconds). When the maximum waiting time is reached, the caller will exit the queue automatically. The default action is that a caller will continue to the next step in the dial plan. If there is no next step in the dial plan the call will end. In the dial plan editor you can customize the steps taken when the maximum waiting time is reached. These dial plan steps are not global but stored and customizable per individual dial plan.

* **Ring time**: The amount of time a call is offered to available agents in the queue. This setting can only be changed after the creation process.

* **Weight**: the weight of this queue compared to other queues. If an agent is logged in to multiple queues, a call to a queue with a higher weight will be offered first. The default is 1 (Normal) and the maximum is 5 (Extreme). This setting can only be changed after the creation process.

* **Call waiting if agent is already in a call:** this setting is only applicable if an agent has enabled call waiting on their phone. When disabled, this setting overrules the phone’s call waiting setting and a second queue call will not be offered if an agent is already in a call. Enabled by default. This setting can only be changed after the creation process.

* **Ignore agent's status**: When enabled, allows calls from this queue to be offered to agents with a user status of 'No queue calls' (for example, during wrap-up time) or 'No calls'. For more details, see the [Ignore user status](#ignore-user-status) section. This setting also has effect on the queue settings 'Possible to connect to a queue' and 'Stay connected to queue'. See the documentation for these queue options for more details. Disabled by default. This setting can only be changed after the creation process.

* **Enable wrap-up time**: The number of seconds after completing a queue call, in which the agent will not receive new queue calls. For more details, see the [Wrap-up time](#wrap-up-time) section. Disabled by default. This setting can only be changed after the creation process.

* **Gear up agents based on priority**: When this is set more agents will be gradually engaged depending on priority and availability. At login on a queue priority 1, 2 or 3 can be set for an agent. At first agents with priority 1 will be offered calls. When option 60/180 is set, this will be the case for the first 60 seconds. In the subsequent 180 seconds agents with priority 2 will be engaged, if agents with priority 1 are unavailable. After 240 seconds all agents including priority 3 will be offered calls, until the maximum waiting time has elapsed. Disabled by default. This setting can only be changed after the creation process.

* **Limit number of callers in queue**: If the filled in number of waiting callers in a queue is reached, the first new caller will not be placed in the queue. The default action is that a caller will immediately continue to the next step in the dial plan. If there is no next step in the dial plan the call will end. In the dial plan editor you can customize the steps taken when the maximum number of people in the queue is reached. These dial plan steps are not global but stored and customizable per individual dial plan. Disabled by default. This setting can only be changed after the creation process.

* **Callers can exit**: Allows users to exit a queue by pressing a key on their phone dial pad. When enabled, the queue settings page will show a phone dial pad to configure which keys the user can press to exit the queue. Available exit keys are the numbers 0 to 9, * and #. The default action is that a caller will immediately continue to the next step in the dial plan. If there is no next step in the dial plan the call will end. In the dial plan editor you can customize the steps taken for each individual queue exit, such as informing the caller about the exit they have taken. The selected exit keys will effect all dial plans containing the queue. Customized steps for an exit key are not global but stored and customizable per individual dial plan. Disabled by default. This setting can only be changed after the creation process.

* **Periodic Announcements**: choose which prompt is repeated during the waiting time in a queue. Disabled by default. This setting can only be changed after the creation process.

* **Time between announcements**: Sets the amount of time between periodic announcements. This setting will yield the Elapsed time in queue setting when not equal. This setting can only be changed after the creation process.

* **Time between announcements of callers waiting and hold time**: Sets an announcement stating the number of waiting callers in the queue. Any value will set the time in seconds between the callers waiting announcement and a hold time announcement when set.

* **Report estimated hold time**: Set whether and when estimated hold time is reported to the waiting caller. The following options are available:
  * **Each interval**: coincides with the Interval between announcements of callers waiting and hold time setting.
  * **Once**: coincides only the first time with the Interval between announcements of callers waiting and hold time setting.
  * **No**: Turns off the estimated hold time announcement.

* **Round estimated hold time (seconds)**: The amount in seconds the estimated waiting time is rounded off. For example, if the reported waiting time should be rounded off to 15 seconds, enter 15. This setting is only visible if `Report estimated hold time` is set to either `Each interval` or `Once`.

* **Music on hold**: Choose from files added in the Music on hold menu which music is played for a waiting caller. This setting can only be changed after the creation process.

Agents can be added to or removed from a queue both during and after the queue creation process. Once a queue is created, agents within it can be managed in two ways: either from the queue overview page by selecting the queue and navigating to the 'Agents' tab, or from the dial plan by selecting a queue and clicking the 'Manage agents' button. It is also possible to set an agent's priority and determine whether user forwards are followed. By default, the priority is set to 1, and the 'follow user forwards' option is disabled.


#### Wrap-up time

Note: For wrap-up to work, the agent handling the call needs to have the **wrap-up** feature enabled. For more information about user features, see [Additional features](#additional-features).

{{site.compass.reseller.prodname}} supports *wrap-up time*. When an agent completes a queue call, some time is allocated to handle (wrap-up)
the completed call and prepare for the next call. No new queue calls are offered to the agent during this time. The wrap-up time for a queue can be configured in the queue settings.

Wrap-up time is started when the agent 'completes' the queue call. The call is considered completed if:
* one of the parties hangs up.
* the call is transferred away from the agent.

Wrap-up time can be stopped automatically (time-based) or manually:
* If the wrap-up duration is set to a positive number of seconds, the platform will automatically stop wrap-up time after that duration.
* If the duration is set to -1, wrap-up time is automatically started after a queue call, but the user has to stop it manually. This can 
  be done by [calling \*49](#49-global-queue-pause), by clicking the `End wrap-up` button in Bridge, or by using the API.

Wrap-up interacts with *user status* (see [My Settings](#status)):
* If the user's status is 'Available', wrap-up will automatically change the status to 'No queue calls'.
  The agent will still be able to receive direct calls from colleagues.
  Queue calls will not be offered, unless a queue has **Ignore user status** set to enabled. For more details, see the [Ignore user status](#ignore-user-status) section.
* After wrap-up time, the user's status is reverted to 'Available'.

Wrap-up time is not started if:
* an agent has user forwards configured (see [User forwards](#user-forwards)).
* an agent already has a custom status when the call is completed.

#### Ignore user status

A queue can be configured with the option **ignore user status**: a queue of this type will always offer calls to agents,
*independent* of the agent's user status (and therefore, even during wrap-up time).
This can be used for queues with very important calls.

### Conference Box

You can set up a conference call using a Conference box. Provide each participant of the conference call with the number at which the conference box can be reached and if necessary the PIN code. The first person to call opens the conference call, and the other participants join in.

You can add a new Conference box by clicking 'Add new conference box'. Enter the name of the box and if necessary the PIN code. When finished click 'Save'.

To edit a conference box, in the Conference box overview click the Edit button (pencil icon) to the right of the name of the box you wish to edit. On the Edit Conference box page the Name and PIN code can be edited. To discard changes click Cancel, to save changes click Save.

Permanently deleting a conference box can be done by clicking the remove button (bin icon) in the conference box overview. You will be requested to confirm the deletion.

### Voicemail

To add a new voicemail box, click 'Add new voicemail'. On the Add Voicemail page a description and PIN code need to be filled out. The description can be a descriptive title, for example the name of the person or department the voicemail box will be assigned to. Entering an email address is optional, this can be set to receive the voicemail audio file in your email box.

The voicemail box settings can be edited by clicking the Edit button (pencil icon) to the right of the voicemail box' name you wish to edit, on the overview page. The description, PIN code and email address can be changed. To discard changes click Cancel, to save changes click Save.

To permanently delete a voicemail box, in the Voicemail box overview click the remove button (bin icon) behind the box to be removed. You will be requested to confirm the deletion.

### Dial plan forwards

When you add a dial plan forward, this forward will be available in the dial plan, under the Dial plan forwards element. For further details see the [Dial plan](#dial-plan) section.

Add a new dial plan forward by clicking 'Add new dial plan forward'.

* **Forward towards number**: Enter the phone number to which the you want to direct the forward. This can be an internal or external extension or for example a mobile phone number.
* **Description**: Enter a descriptive name.
* **Ring time (seconds)**: lets you choose how long the call will be offered to the destination of the forward, before reverting back to the dial plan. Setting ring time at 0 will not revert to the dial plan.
* **Extension**: When filled out, a new extension will be created. Its function is to forward to the destination number, therefore it could be used as a quick dial. For example, if the destination for the forward is someone's mobile number, only the extension has to be dialled in order to reach the person on mobile. The extension can also be used to add a more extensive dial plan.
* **CLI transparent**: When enabled the original caller ID will be sent to the destination, instead of the caller ID of the forwarding party.
* **Confirm received call**: When enabled the destination will be prompted to confirm answering the forwarded call by pressing a key on the phone. When denied, the call will return to the dial plan. If no other option is set, it will be terminated.

When finished setting these options, click Save.

To edit a dial plan forward, click the Edit button (pencil icon) in the overview. To discard changes click Cancel, to save changes click Save.

If you wish to permanently delete a forward, click the remove button (bin icon) in the overview. You will be requested to confirm the deletion.

<a name="overview-callflowswitch"></a>
### Call flow switch

When call flow switches have been added to a dial plan in your company, this overview gives you a list of all the switches, their shortcode and current switch setting. For further details on how to add a call flow switch, see the [Dial plan elements - Call flow switches](#dialplanelements-callflowswitch) section.

To change the setting of a call flow switch via the web interface, click the Edit button (pencil icon) to the right of the switch's name. On the Edit call flow switch page is an overview of all the available settings, with the current switch setting selected. Select the new switch setting and click Save. The new setting is visible in the call flow switch overview.
Deleting a call flow switch can be done by going to the dial plan in which the switch is located. Click the X icon in the call flow switch block to remove it from the dial plan and click Save.

## Users and Groups

In this section you will find groups, departments and users that have been created in the company. You will be able to create, modify and delete these items as well as set up user rights. Users in departments and groups can be allowed to pick up incoming phone calls on each other's phones by dialling \*8.

In the main menu, navigate to the Manage tab. Click Users in the submenu to go directly to the Add users and groups page to create groups, departments or users. Or to get an overview and more options for each individual item, select the Users submenu and then click on the item you want to work on.

### Groups

The function of Groups is to group together a selection of users. Users can belong to multiple groups. For example, cross-department project teams.

Groups can be used to limit the reach of a \*8 call pickup by first setting up groups of users that should be able to pick up each others calls, and then enabling Call-Pickup within group for a company.

To add a new group, click Add group. Fill out a descriptive name for the group and click Save.

Go to the Group information page by clicking on the group in the Groups overview. Here users can be added to the selected group by clicking the add button (plus icon) next to their username in the Non-members of the group list. Removing members is done by clicking the remove button (minus icon) next to the username in the Members of the group list. Use the Filter field to do a quick search for a user and when finished click Save to keep the changes made.

If you wish to change the name of the group, click Edit group on the Group information page. This can also be done by clicking the Edit button (pencil icon) to the right of the group name in the Groups overview.

Removing a group can only be done from the Groups overview, by clicking on the remove button (bin icon) behind the group name. You will be requested to confirm deleting the group.

### Departments

In the departments overview you can see all departments within the company. The function of departments is similar to that of groups, only this is meant to group together all the users who are for instance in the physical Support department within your organisation.

To add a new department, click Add department in the Departments overview. On the Add department page fill out a descriptive name for the department and when finished click Save.

Go to the Department information page by clicking on the department in the Departments overview. Here users can be added to the selected department by clicking the add button (plus icon) next to their username in the Non-members of the department list. Removing members is done by clicking the remove button (minus icon) next to the username in the Members of department list. Use the Filter field to do a quick search for a user and when finished click Save to keep the changes made.

If you wish to change the name of the department, click Edit department on the Department information page. This can also be done by clicking the Edit button (pencil icon) to the right of the department name in the Departments overview.

Removing a department can only be done from the Departments overview, by clicking on the remove button (bin icon) behind the department name. You will be requested to confirm deleting the department.

### User

The Users overview will give you an alphabetical list of all the users in the selected organisation.

Go to Add user to add a new user to the organisation. Be aware that adding users could affect monthly charges. On the Add user page the following can be filled out:

* **Full name**: Fill out the full name of the user
* **Select language**: Select the desired language of the user. This will overrule the company default language setting for text on the web interface and prompts on the phone
* **Username**: Will be generated while typing the Full name, but can also be customized. Can not contain capitals. This is the name that will be used to log in to {{site.compass.reseller.prodname}} and is visible on a phone's display on which the user has logged in
* **Password**: Type a custom password with which the user can log in to {{site.compass.reseller.prodname}} web interface. Has to be left empty when password is auto generated
* **Email**: Fill out the email address of the user. This is the address the auto generated password will be sent to, this field is mandatory when Send login credentials is checked
* **Send login credentials**: Check this option when you want the password to be auto generated. An email address should be filled out.
* **Extension**: If the user needs to have its own internal extension assigned, this is where it can be created. It can not be an existing extension, the new extension will be created upon adding the user
* **Description**: When assigning an extension to the user, the description is a short descriptive name given to the extension, which will be visible in the Extension overview. When no extension is assigned, this field can be left empty
* **PIN code for login**: Optional 1-4 number long code used for logging a user in on a phone securely
* **Outbound number**: When set to Internal calls only (default), the user is not able to call to an external number, only to extensions within the company. When set to an external number, the user is able to make calls outside of the company. The number selected will be set as the caller ID. This option is only available if an external number has been added to the company.

When finished filling out the desired options for the user, click Save.

To see a user's basic information and to set more options, click a user's name in the overview to go to the User information page. This page contains the user's name, email address, username, language and its identities.

The user can be edited by clicking Edit user on the User information page. On the Edit user page the Full name, email address, PIN code for login on a phone, language, and additional features can be changed. The features incur extra monthly charges, and can be changed after clicking the 'Edit features' button. If the desired changes have been made click Save, or Cancel to discard.

The user's identities can be edited by clicking the Edit button (pencil icon) to the right of the identity's name on the Users overview page. To get more information on the options available for an identity, see the [Identities](#identities) section.

Change password will allow the user's password to be changed without the need to know the current password. This can, for example, be used by administrators to generate a new password when the current one has been lost. If the user has set an email address, a new auto generated password can be sent to this address by checking the Generate new and email option. If you prefer to set a custom password, fill out and repeat the new password in the designated fields.

A user can have certain rights within an organisation. These determine what a user can view and edit and on which level. Go to Set up user-rights to view and change these rights for the selected user. More information about setting up user rights can be found in the [User rights](#user-rights) section.

One user can have multiple identities which can be added by clicking on Add identity. This will open the Create new identity page, which will be further explained in the [Identities](#identities) section.

To log the selected user on and off on a phone using the web interface, go to Log in and log off phone. On the Log on to phone page, an overview of all the phones in the company is visible. The selected user can be logged in on a phone by clicking the phone name. The user's name will appear behind the phone name when the user is logged in. Logging out is done by clicking the phone name again. The user's name disappears when logged out.

Settings can be used to view and change the user's personal and phone information, set user forwards and view voicemail and queues information. For more information, see the [Settings](#settings) section.

If you wish to permanently delete a user click the remove button (bin icon) behind the user's name in the Users overview. You will be requested to confirm deletion.

### Additional features

Users can be edited to enable additional features. These features incur monthly charges. Contact your reseller for more information.

The following features are currently available:

* **Call control API**: Enable the REST API calls that allow external applications to perform call control. Refer to the REST schema to see the exact calls that become available when enabling this feature.
* **Listen in API**: Enable the REST API calls that allow external applications to perform listen in. This enables the `/user/<id>/listenIn` REST API call. Refer to the REST schema for more details.
* **Wrap-up**: Enable wrap-up time after a queue call for this user. The wrap-up time needs to be configured on the queue and will only be applied to users with this feature enabled.
* **Queue pickup API**: Enable the REST API call that allows a user to pick up a queue call, even if the user is not an agent in the queue. This enables the `/user/<id>/pickupQueueCall` REST API call. Will also enable the `/extension/<id>/pickupQueueCall` API to direct to a specific extension instead of a user.

### User rights

User rights can be set by going to the user's User information page and clicking Set up user-rights.
Rights can only be set by users with write rights within the company or reseller.

The following levels of user rights are available:

* **Company rights**: User can manage its own company. For example create and manage dial plans, users, groups and departments.
* **Reseller rights**: In addition to the above, the user can also create and manage companies.
* **Super reseller rights**: In addition to the above, the user can also create and manage resellers.

If for example you want to grant a user company rights, select the Set up Company rights check box and click Next. On the next page, Company rights for user, select the company on which you want to grant the user rights. On the Set up company rights for user page, click the drop down list behind Apply rights and to select either Unauthorized for mortal user rights, View for read rights and Change for full read and write rights for the user in the selected company. Click Save to set the selected rights, or Cancel to discard.

### Identities

The first identity of a user is its primary identity and is created when the user is created. When a user with multiple identities logs in on a phone, all of its identities are visible on the display and, according to the type of phone used, each identity has its own line. Be aware that the maximum number of identities created in {{site.compass.reseller.prodname}} might not coincide with the maximum number of identity slots available on the phone the user is logged on to.

Additional identities for a user can be created by clicking Add identity on the selected user's User information page, which will open the Create identity page.
The following options can be set when creating a new identity:

* **Name identity**: Fill out the name of the identity
* **Description**: Fill out a descriptive name for the identity. This will be visible in the overview.
* **Extension**: If the identity needs to have its own internal extension assigned, this is where it can be created. It can not be an existing extension, the new extension will be created upon adding the identity
* **Outbound number**: When set to Internal calls only (default), the user is not able to call to an external number, only to extensions within the company. When set to an external number, the user is able to make calls outside of the company. The number selected will be set as the caller ID. This option is only available if an external number has been added to the company.
* **Voicemailbox**: Set the voicemailbox to which incoming messages are delivered
* **Hide caller ID**: When checked the caller ID of the identity will not be visible to the receiving party
* **Confirm received call**: When enabled the destination will be prompted to confirm answering the forwarded call by pressing a key on the phone. When denied, the call will return to the dial plan. If no other option is set, it will be terminated.
* **Record calls**: When enabled all incoming and outgoing calls of this identity will be recorded. Enabling this option will unveil three more fields. An Email recordings to field where an email address can be entered where the recordings will be sent. A disclaimer field which needs to be read and agreed upon by checking the I Agree check box.

When finished setting up the new identity, click Save.

The newly created identity is now visible in the overview on the User information page. To edit an identity, click the Edit button (pencil icon) to the right of the identity's name. On the Edit current identity page all above options can be edited.

If you wish to permanently delete an identity, click the remove button (bin icon) behind the identity name in the identities overview on the User information page. You will be requested to confirm deletion.

### Settings

The user's settings page provides an overview of the user's personal-, phone-, voicemail and queue information. To get to this page, select the user in the Users overview and click Settings.

The user's email address, password, phone and identity forwards can be modified.
In the User forwards section identity forwards can be set. The following options are available:

* **Always**: Incoming calls are always immediately forwarded to the given number.
* **Busy**: Incoming calls will only be forwarded when the user is already in a call.
* **No answer**: Incoming calls will be forwarded when the calls are not answered. A call will be considered not answered after the set ring time has passed.
* **Unavailable**: Incoming calls will be forwarded when the phone the user is registered to is unavailable. This is the case when either a user is not logged in on the phone, or the user has enabled the Do Not Disturb setting on the phone.

Incoming calls will be forwarded to the given number in the field below the forward option. This can be an internal extension, an external number for example a mobile phone number, or voicemail (1233). Additional options for user forwards are:

* **Ring time**: The set time (seconds) determines how long the call will be offered until a call is considered not answered.
* **CLI transparent**: When this option is enabled, the original caller ID will be sent to the destination, instead of the caller ID of the forwarding party.

## Phones

From the Manage menu you can choose the Phones menu option to get an overview of phones in the company.

### Adding a certified phone

{{site.compass.reseller.prodname}} is equipped with auto configuration for certified phones. Auto configuration takes care of configuring certified phones so that no manual set up is required.

Selected models of these brands have been certified to work with {{site.compass.reseller.prodname}}:

* Snom
* Yealink

See [Adding an unsupported phone](#adding-an-unsupported-phone) for phones that are not certified. 

To add a certified phone for auto configuration, go to Manage > Phones and click the Add phone button. You're able to set several basic properties:

* **Phone name**: give the phone a recognisable name
* **Phone model**: select the specific model/type of the certified phone you wish to add
* **MAC address**: enter the MAC address, usually found on a sticker on the bottom of the phone
* **Expansion pads** (only when applicable): enter the amount of connected expansion pads
* **VLAN ID** (only when applicable): enter the ID of the VLAN the phone should use to connect. Consult your network administrator for details. Valid VLAN ID values are between 1 and 4094. If unsure, leave this empty. Usually a VLAN ID is not needed.
* **Encryption** (only when applicable): Enable or disable encrypted SIPS and SRTP
  * **Optional** means a certified phone will always be auto configured to use unencrypted SIP over UDP and unencrypted RTP.
  * **Mandatory** means a certified phone will always be auto configured with SIPS and SRTP to force both the signaling and the audio to be encrypted.

Click Save to add the phone to the company. You will see the added phone in the overview of configured phones. You can edit the phone by clicking on the Edit button (pencil icon) to the right of the phone details in the overview.

Reset the phone settings to defaults as documented by the phone vendor to activate auto configuration. Auto configuration by {{site.compass.reseller.prodname}} takes place on the first boot after the reset to defaults. **Note**: the time window to reset and reboot the phone expires every week on Wednesday at 03:00 CET/CEST in the night. To open a new time window, go to the phone status page and click the Activate button for Provisioning by manufacturer.

### Adding an unsupported phone

Phones that have not been certified to work with {{site.compass.reseller.prodname}} can be added as one of these types:

* Generic SIP Phone, for physical phones with a MAC address
* Generic SIP DECT phone for wireless phones, and
* Generic SIP Softphone for software clients and mobile clients

Go to Manage > Phones and click the Add phone button. You're able to set several basic properties:

* **Phone name**: give the phone a recognisable name
* **Phone model**: select the specific model/type of the unsupported phone you wish to add
* **MAC address** (only when applicable): enter the MAC address, usually found on a sticker on the bottom of the phone
* **Encryption** (only when applicable): Select optional or mandatory SIPS and SRTP
  * **Optional** means a phone may register using unencrypted SIP over UDP and then use unencrypted RTP. It _may_ also connect using SIPS (SIP over TLS) and is then _required to use SRTP_ to force both the signaling and the audio to be encrypted.
  * **Mandatory** means a phone is _required_ to use both SIPS and SRTP to force both the signaling and the audio to be encrypted.

Click Save to add the phone to the company. You will see the added phone in the overview of configured phones. You can edit the phone by clicking on the Edit button (pencil icon) to the right of the phone details in the overview.

The phone can now be configured manually to register with {{site.compass.reseller.prodname}}.

### Phone status

To view the properties and details of an individual phone, click on the phone name in the Phones overview. You will see the phone's details, a Reboot button to remotely restart the phone, and a View provisioning button. The details in this overview are usually not required for normal operation.

### Phone settings

To change the settings of a particular phone, click on the phone's name in the overview to see all of the phone details. Above the table with details, there are several options: Settings and Edit function keys.

#### General phone settings

On the phone's status page, click Settings to see several configuration options for that specific phone.

You are able to enable call waiting and call waiting indication. If you enable call waiting, you will be able to answer a second incoming call while already on the phone in another call. You can choose if you wish to answer the second call or not. If call waiting is not enabled, a second call will not be routed to your phone so you will not be disturbed.

##### Custom settings

Custom settings enable an administrator to configure custom phone settings by entering provisioning data which is merged into {{site.compass.reseller.prodname}} default provisioning. Custom settings can be configured for an individual phone, or for a phone type, which applies to all phones of the specified type within the company.

Be aware: custom settings will override {{site.compass.reseller.prodname}} settings and incorrect input could potentially break the phone's functionality. This is advanced functionality and should only be set by an experienced administrator.

Examples for custom settings are changing a label or display text, remapping a key, or setting a speed dial to be a designated log in button.

##### Adding and removing custom settings

Click Add custom setting to reveal two fields: one for the setting name and another one for the value of that setting. Enter a setting name and value in the fields, click Save when done to push the altered configuration to the selected phone. Setting names and possible values can be found in the provisioning manual of the corresponding phone type or vendor.

Click Add custom setting again to add more fields for custom settings. A custom setting can be removed by pressing the *x* in front of the Setting name field.

##### Applying Snom permission flags

It is possible to apply permissions on Snom phone settings in provisioning data. To set a permission flag for a setting, add the corresponding character at the end of the setting key:

* `!` for user changeable
* `&` for read only
* `$` for read-write

More information on the effects of permission flags can be found on [the Permission Flags page on the Snom wiki](https://service.snom.com/display/wiki/Permission+Flags).

For example, if you have a setting with key `should_be_readonly` and want to make that setting write-protected, add a `&` at the end so the key becomes `should_be_readonly&`. To verify if the correct permission is applied in provisioning, you can check the provisioning output as described in the next section.

##### Checking provisioning

To view a phone's provisioning configuration, click View provisioning on the phone's Status page. If any customised settings have been applied, these will be visible at the bottom of the document.

##### Unsetting values

In some cases it may be necessary to unset a value that is set by (default or custom) provisioning. The method to do this differs per phone vendor. These are the methods for the vendors of supported phones:

* Snom
  * Set Setting name to the name of the setting you want to unset.
  * Set the value to " " (a space, without the quotes).
* Yealink
  * Set Setting name to the name of the setting you want to unset.
  * Set the value to `%NULL%`.

#### Edit function keys

Click Edit function keys to configure the function keys on supported phones.

The amount of function keys depends on the type of the selected phone. For a Snom phone with two rows of function keys, the keys are numbered from the top left (1) to bottom right.

You can configure a function for every key. There are different types of functions to choose from:

* **Standard**: the key will have the function assigned by the manufacturer.
* **Button**: you can choose one of the following functions: On hold, Call transfer, Do not disturb, Redial.
* **Line**: open a new line to start a call.
* **No function**: the key will have no function at all.
* **Speeddial**: call a number entered in the input field in this form.
* **Status**: the key will serve multiple functions: The light for this key will serve as a status light for the internal number configured in the input field. If the light is off the internal number is not busy or ringing. If the light is on and doesn't blink, the internal number is busy. If the light blinks, the internal number is ringing. If the button is pressed while the light is blinking, it functions as call pickup and the call that is ringing on the internal number is transferred to the phone and answered immediately. Additionally, pressing the key if the light is not on will call the internal number entered in the input field in this form.

Configuring function keys can be done for one phone individually, or for all phones of a particular type at the same time. To configure all phones of a particular type, choose Edit function keys directly from the Phones overview page. Choose the phone type you wish to configure. Set up configuration as you would do for an individual phone from there.

### Editing phones

To edit a phone, click the Edit button (pencil icon) in the overview, or click the Edit button on the Phone status page.

You will see the following:
* **Phone name**: adjust the name here if needed
* **Phone model**: make corrections here if the wrong model was entered earlier
* **MAC address**: make corrections here if the wrong MAC was entered earlier
* **Expansion pads** (only when applicable): adjust the amount of connected expansion pads
* **Codec profile** (only when applicable): a codec profile is already defined for the company. If you require a different codec profile for this specific phone, you can configure this here. For example, if a user is working from home on a low bandwidth internet connection. Our advice is to leave this set to default.
* **Firmware version** (only when applicable): configure which version of the firmware you would like this phone to have. {{site.compass.reseller.prodname}} will attempt to upgrade or downgrade the firmware on this phone to the selected version. Note that on some phone series, downgrading is blocked by the vendor for safety or compatibility reasons.
* **VLAN ID** (only when applicable): enter the ID of the VLAN the phone should use to connect. Consult your network administrator for details. Valid VLAN ID values are between 1 and 4094. If unsure, leave this empty. Usually a VLAN ID is not needed. To deconfigure a VLAN setting, enter the number 0. The next time the phone collects its configuration, it won't be in a VLAN anymore.
* **Encryption** (only when applicable):
  * **On certified phones**:
    * **Optional** means a certified phone will always be auto configured to use unencrypted SIP over UDP and unencrypted RTP.
    * **Mandatory** means a certified phone will always be auto configured with SIPS and SRTP to force both the signaling and the audio to be encrypted.
  * **On unsupported phones**:
    * **Optional** means a phone may register using unencrypted SIP over UDP and then use unencrypted RTP. It _may_ also connect using SIPS (SIP over TLS) and is then _required to use SRTP_ to force both the signaling and the audio to be encrypted.
    * **Mandatory** means a phone is _required_ to use both SIPS and SRTP to force both the signaling and the audio to be encrypted.

## Dial plan

As an administrator, you can edit everything related to dial plans. The procedure is to set up all the individual elements listed under the Dial plan menu first. When all users, prompts, queues and other elements have been set up, they can be added to the dial plan's tree-like structure. This determines when, and in which order, a particular action (play a prompt, connect to a user, etc) is taken. Dial plans can be set up for both external numbers as well as internal extensions. Go to either overview page and select the number or extension you wish to set up or edit a dial plan for.

### Editor

In the dial plan editor, you can create and alter dial plans by dragging and dropping dial plan elements. The available elements are on the left-hand side of the editor. At the top of the sidebar, there is a unique search field for all elements.

At the top left corner of the dial plan canvas is a phone number or extension to which the dial plan belongs. This is also the entry point of the call. Drag the elements from the left-hand sidebar to the empty slots below the number to create a dial plan.

An already placed element can be removed by hovering over the element in the dial plan and clicking the X in the top right corner of the element block. Clicking on the element block will open the element details, if there are any.

**Changes to the dial plan are not saved automatically.** Make sure you click "Save" in the top right corner before exiting the dial plan.

### Dial plan elements

The list of elements with which a dial plan can be made consists of the created resources, such as queues, prompts, users, and special elements such as IVR menus and call flow switches. In the next chapters we'll describe how these elements can be used.

<a name="dialplanelements-callflowswitch"></a>
#### Call flow switch

If a call should be routed differently depending on a condition set by a user, you can use a call flow switch. This for example allows an employee to redirect callers on the technical support number to a prompt indicating a service disruption, so all of the support engineers can work on resolving the issue.

Drag a call flow switch element to a free slot to configure a new switch. Follow the instructions to add all the relevant information.

You will be asked for a shortcode. A shortcode is a number used to identify the switch. The shortcode can also be used to change or check the active setting of a switch using a service code from a phone in your organisation. See the [Set call flow switch](#55-set-call-flow-switch) section for more details.

When the switch is created, simply drag dial plan elements to the switch branches to configure what happens if a call is routed through that branch. The active branch is indicated with the label `Active`. You can change the active branch by clicking on the switch element and clicking `Activate` for the desired setting in the right sidebar.

* All users in your organisation can check or change the setting of a switch by dialling a service code from their phone. This is explained in the [Set call flow switch](#55-set-call-flow-switch) section.
* Users with {{site.compass.reseller.prodname}} credentials can change switch settings using the switch overview on the My Settings page, or using the API.
* Administrators can additionally change switch settings on the call flow switches page in {{site.compass.reseller.prodname}} or in the dial plan editor as explained above. See the [Manage - Call flow switch](#overview-callflowswitch) section for more details.

#### IVR menu

If a call should be routed differently based on a choice made by the caller, you can use an Interactive Voice Response (IVR) Menu. This allows a caller to press, for example, `1` for support, and `2` for sales.

To add an IVR menu, drag the element to a free slot. Follow the instructions to add all the relevant information.

Number of repeats refers to the number of time callers are going to hear the menu repeated. The Timeout determines how much time a caller has to respond with a choice.

Now simply drag dial plan elements to the branches to configure what happens if a caller presses that key.

#### Number-based routing

If a call should be routed differently depending on the caller's number, you can use number-based routing. This for example allows for different dial plan branches for different countries of origin, or routing a customer directly to a specific support agent. Anonymous calls can be routed separately.

To add number-based routing, drag the element to a free slot. Follow the instructions to add all the relevant information. Determine how many different branches the routing should have, and give them a name such as 'Region A', 'Region B', 'Region C', and 'Customer X'. When ready, save the settings. The branches should now appear in the dial plan.

Click on the branch element to set which incoming numbers should be recognised. The Number list can take prefixes or complete numbers.

* Input one prefix or number per line in the Number list.
* Always use international format, but *without* the plus sign (+), minus sign (-) or spaces.
* To match a specific number, enter the complete number including international prefix ("31201234567").
* To match a region, enter the international prefix and the area code (example: "3120" to match all calls from Amsterdam).
* To match all mobile calls, enter the international prefix and the mobile number prefix ("316").
* To match all calls from a country, enter the international prefix number ("31").

These things are important to consider:
* Anonymous calls by default do not match any branch. If you wish to route anonymous calls, always have one branch with the option Caller ID blocked enabled. All anonymous calls will be routed through that branch.
* If more than one branch matches the incoming number, the first match will determine the branch the call is routed through. For example: if the first branch matches on "31612" and the second matches on "316", then incoming calls from number 31612345678 will be routed through the "31612" branch.
* If the incoming number doesn't match any manually configured branch, the call will be routed through the Default branch. This branch is always added automatically.

Do this for every branch, and make sure that every number (including anonymous) you wish to route has a match in one of the branches.

#### Time-based routing

If a call should be routed differently depending on the time of day, day of the week, or (part of) the month, you can use time-based routing. This allows you, for example, to play different prompts during the day, such as 'Good morning' and 'Good afternoon'.

To add time-based routing element, drag the element to a free slot. Follow the instructions to add all the relevant information. When ready, save the settings. The branches should now appear in the dial plan.

Click on the branch element to set at which times that branch should be followed. Do this for every branch, and make sure that there are no gaps between the time set for each branch. So 'Morning' is set from 08:00 to 12:00, then 'Afternoon' is set from 12:00 to 18:00, etc.

#### Dial plan forwards

If at some point in the dial plan you would like to forward the call to a different number (internal or external), you can use a dial plan forward. If a forward is used in a dial plan for an external number, and you are not forwarding to an internal extension, the external number of the dial plan will be used as the outgoing number for number recognition.

#### Conferences

If you would like to have a conference box in your dial plan, drag it to an open slot in the dial plan.

A conference box element will answer incoming calls automatically and ask for a PIN code if required. A conference box is a dial plan end point, meaning elements placed after a conference box will not be executed.

#### Call end

If this element is encountered in the dial plan before a call is answered, the call will end with a "Temporarily not available" status. If the call was answered earlier in the dial plan, and the call continued after that, it will end with a "Normal call clearing" status.

#### Busy signal

A call encountering this element will end with a "Busy" status.

#### Label (formerly Prefix)

If a call encounters this element, a configurable text will show in the display of the phone receiving the call together with the calling number. This can help to identify through which number a caller is calling. To set a label, drag the element in the dial plan and click on it to configure the text.

#### DTMF input

This element reads DTMF (digit) input from the caller. Use a fixed-length input if the amount of digits is fixed, ie. a 4-digit PIN code.

If variable-length is selected, the caller ends the input using the pound sign (#). Pressing the star (*) resets the input. DTMF input from the caller can be read by using the [{{site.compass.reseller.prodname}} XMPP API](developers-manual.html#{{ site.compass.reseller.prodname | append: " XMPP API" | slugify }}){:target="_blank"}.

*You are advised to use the selected voice prompt to instruct the caller on the usage of this element; for example, mention that input can be ended with the pound sign for variable-length inputs.*

#### Users

You can set up a dial plan to call a particular person by dragging one of the user elements to a free slot in the dial plan. The dial plan will call the first identity configured for this user. In addition, identity forwards set for that identity will be followed. If the call is not answered directly or after following forwards, the dial plan will continue to the next configured action.

#### Prompts

If you would like to play a message to the caller, you can add a prompt to a free slot in the dial plan. An incoming call will be answered automatically first, before playing the audio. When the prompt has finished playing the dial plan will continue to the next configured action.

#### Queues

If the handling of incoming calls should be distributed over a group of users (agents) while the caller waits in line, you can use a queue. This for example allows a sales team to route all received calls to the sales number and have users wait if all sales representatives are busy handling calls.

To create a new queue: open a dial plan in Studio and click on `Queues` in the `Dynamic elements` section on the bottom left. Then click `Create queue` and follow the steps.

Configuration options include the possibility to set a maximum waiting time for callers, enable informing the caller of the average waiting time, and determine a phone ring strategy.

It is possible to customize the dial plan steps for situations where:
* the caller escapes from the queue using a key on the dial pad
* the maximum number of people in the queue is reached
* the maximum waiting time is reached

**Note**: while queue settings, including options such as the selected exit keys, are global and apply to every dial plan you add the queue to. The customizable steps for queue exits are _not_ global and can be changed for every dial plan individually. If you want to use the same queue with the same customized exit steps for multiple dial plans, it's best to put the queue in a nested dial plan.

If the call has not ended when all custom steps for that exit have been followed, the dial plan will continue with the steps in the dial plan that are after the queue. If a caller is not able to enter a queue due to other settings, then the call will also be routed to the steps after the queue.

If a call is answered by an agent, the queue will be the end point of the call.

#### Voicemails

If you would like to offer the caller the option to leave a message, you can use a voicemail box.

A voicemail box element will answer incoming calls automatically.
A voicemail box is **_not_** a dial plan end point. If a caller presses # (hash key) to finish leaving a voicemail message, the call will continue through the dial plan.

### Examples

#### Front desk

If you would like to have all incoming calls handled by the front desk, create a front desk queue and add it to the first slot of the dial plan of the main external number. Click Save to save the dial plan. Now all incoming calls on the main external number will be offered to the front desk, where the call can be transferred to the relevant employee.

## Reports

{{site.compass.reseller.prodname}} offers Call Detail Records and Events reports both as download in the web interface as well as through an HTTP API.

Administrators can access 'CDRs and Events' in the {{site.compass.reseller.prodname}} web interface on the Manage page of the company.

Developers can request the same information using our CDR and Events API documented below.

### Types of reports

There are 5 types of reports available for download:

* Company CDR: contains a record for every call that enters or leaves a company. A user calling the external number of the company that user is part of, means two records: one call leaving the company, and one entering it.
* Queue CDR: contains a record for every call that enters a queue, also if the queue call isn't answered.
* User CDR: contains a record for every call that is received by a user. A call between two users in the same company means two records.
* Call Events: contains step by step events for all calls in the company, to track exactly how a call was handled by the platform.
* User Events: contains user actions, such as logging in and out on a phone or queue, and pausing and resuming receiving queue calls.

#### Company CDR

The columns in the Company CDR are:
* `call_id`: a unique identification of a call. If a call is split (for example when a call is distributed amongst agent(s) in a queue) then this will generate one or more new calls with new call IDs.
* `start_time`: time at which the call started. Formatted as "YYYY-MM-DD mm:hh:ss".
* `answer_time`: time at which the call was answered. Formatted as "YYYY-MM-DD mm:hh:ss". Incoming calls can also be answered by prompts or IVR menus. Empty if the call is not answered.
* `end_time`: time at which the call ended. Formatted as "YYYY-MM-DD mm:hh:ss".
* `timezone`: time zone offset relative to UTC as defined in ISO 8601. Example: "+02:00"
* `from_type`: the source type. Can be either `external` or `identity`.
* `from_id`: the identification of the identity. Only used if from_type is "identity", otherwise empty.
* `from_number`: the number of the source of the call. Empty if the call is anonymous or internal (within company).
* `from_name`: the name of the source of the call, if provided by the source. In some cases, the source number is also used as the name.
* `to_number`: the number of the destination of the call. If the record describes an incoming call, this is the number on which the call was received.
* `end_reason`: indicates the reason the call was ended. Can be either `busy`, `caller` (caller ended the call) or `callee` (receiver ended the call).

By processing the Company CDR in an application such as Excel, you can answer questions such as:
* How many calls do we receive?
* How often do we call a particular customer?
* What is the average call duration?
* Did a particular customer call more than 7 times yesterday?

#### Queue CDR

The columns in the Queue CDR are:

* `call_id`: a unique identification of a call. If a call is split (for example when a call is distributed amongst agent(s) in a queue) then this will generate one or more new calls with new call IDs.
* `start_time`: time at which the call started. Formatted as "YYYY-MM-DD mm:hh:ss". If a call enters the same queue several times, the start time will differ.
* `timezone`: time zone offset relative to UTC as defined in ISO 8601. Example: "+02:00"
* `wait_duration`: waiting time until an agent answered the call, in seconds.
* `agent_duration`: duration of the call after it was answered by an agent, in seconds.
* `queue_id`: the identification of the queue.
* `queue_name`: the name of the queue.
* `agent_id`: the identification of the agent that answered the call. Empty if a call was not answered.
* `agent_name`: the name of the agent that answered the call. Empty if a call was not answered.
* `from_number`: the number of the source of the call. Empty if the call is anonymous or internal (within company).

By processing the Queue CDR in an application such as Excel, you can answer questions such as:
* What was the average waiting time in a queue?
* Which agent answered the most calls?
* What was the average duration of calls in a queue?

#### User CDR

The columns in the User CDR are:

* `call_id`: a unique identification of a call. If a call is split (for example when a call is distributed amongst agent(s) in a queue) then this will generate one or more new calls with new call IDs.
* `parent_id`: the original call_id, if a call is split as described above. Empty if the call is not split.
* `start_time`: time at which the user was first involved in the call. Formatted as "YYYY-MM-DD mm:hh:ss". On incoming calls, a caller could have gone through IVR menus and prompts before a user gets involved.
* `answer_time`: time at which the call was answered. Formatted as "YYYY-MM-DD mm:hh:ss". Empty if the call is not answered.
* `end_time`: time at which the call ended. Formatted as "YYYY-MM-DD mm:hh:ss". If the call was not answered, or a call was forwarded, the call could continue elsewhere.
* `timezone`: time zone offset relative to UTC as defined in ISO 8601. Example: "+02:00"
* `identity_id`: a numeric, unique identification of the identity of a user.
* `identity_name`: the name of the identity of a user.
* `direction`: the direction of a call. Can be either `outgoing` or `incoming`.
* `number`: if available, contains the number the callee got as caller ID. For outgoing calls, this is the number configured for the identity used for the call. In some cases, the number may be unknown.

By processing the User CDR in an application such as Excel, you can answer questions such as:
* What was the average duration of calls answered by a user?
* Which user missed the least calls?

#### Call Events

For optimal freedom in reporting we offer Call Event Logging. This allows calls to be tracked step by step. 

The Call Event Logging system is meant for automatic processing in Business Intelligence tools. The column names and values are in English unless it concerns user input.

The columns in the response are:
* `id`: a numeric, unique identification of an event. Numbers ascend in the order in which events were written.
* `company_id`: a numeric, unique identification of a company.
* `call_id`: a unique identification of a call. If a call is split (for example when a call is distributed amongst agent(s) in a queue) then this will generate one or more new calls with new call IDs.
* `parent_id`: the original call ID. This is only provided if the call has been split.
* `step`: a counter that indicates in which order events occurred. Starts at 0.
* `timestamp`: time at which an event occurred. Formatted in a human-readable format based on ISO8601 and adjusted to work with MS Excel.
* `caller_type`: indicates the type of caller. Can be either `identity`, `external`, `pbx`, `queue` or `unknown`.
* `caller_id`, `caller_number` and `caller_desc`: more info about the caller, depending on caller type.
* `caller_state`: indicates the state of the caller. Can be either `connecting`, `ringing`, `answered`, `inactive` or `disconnected`.
* `callee_type`, `callee_id`, `callee_number`, `callee_desc` and `callee_state`: same as above, but for the receiving side. This will change while the call proceeds through the dial plan. For example, a call is answered by a queue first, then by an agent.
* `state`: indicates the state of a call. Can be either `conn` (connecting), `ring` (ringing), `answered` or `down` (call ended). It's possible for a call to change state from answered to ringing, for example after an unattended transfer.
* `end_reason`: indicates the reason the call was ended. Can be either `busy`, `caller` (caller ended the call), `callee` (receiver ended the call) or `replace` (call ended with a (semi) attended transfer).
* `queue_exit`: indicates the reason a caller exited a queue. Can be:
  * `max_len` when the maximum amount of callers in the queue is reached
  * `max_waittime` if maximum wait time in the queue is reached by the caller
  * `join_empty` if no agents are available to pick up the call due to being paused or logged out when a new caller tries to enter the queue
  * `leave_when_empty` if a caller is already queued but the queue is configured to purge callers if all agents log out
  * `key_X` when the caller has pressed a key on the dial pad. `X` refers to a key on the dial pad: 0-9, * or #

Note:
* Events are not sorted in the response.
* Event Logging is not real time. It can take a while for information to become available.
* The Event Log may contain information for events that are not completed yet.

The principles of Event Logging are as follows:
A call has a source (a caller), and a destination (a callee). Both are endpoints. These endpoints change over time: for example, a call first reaches a dial plan, then a queue, and then a user (identity).

For every destination, a call can have several states: connecting, ringing, and answered. If the call changes destination, the state of the call can change back from answered to ringing.

Subcalls are usually the result of the call reaching a queue. If a call reaches a queue, the status changes to answered. This call has a call_id but no parent_id (call_id of the parent). For every agent available in the queue, a new subcall is set up from the queue to the agent. Agents can be called one by one, or multiple at a time, depending on queue settings and the availability of agents. Calls set up from the queue to agents have a new call_id, but also lists the original call_id as parent_id. If an agent answers the queue call, all subcalls are ended (status: down) and the original call gets the agent that answered as the new destination.

#### User Events

User Events allow you to track user actions, such as logging in and out on a phone or queue, and pausing and resuming receiving queue calls.

The events are meant for automatic processing in Business Intelligence tools. The column names and values are in English unless it concerns user input.

The columns in the response are:
* `id`: a numeric, unique identification of an event. Numbers ascend in the order in which events were written.
* `timestamp`: time at which an event occurred. Formatted in a human-readable format based on ISO8601 and adjusted to work with MS Excel.
* `company_id`: a numeric, unique identification of a company.
* `company_name`: name of the company.
* `event_type`: the type of event. Values differ depending on the value of `target_type`.
  * If `target_type` is `phone`, this can be either `login` or `logoff`.
  * If `target_type` is `queue`, it can be either `login` (agent logged in on queue), `remove` (agent logged out of queue) or `queuePickup` (agent performed queue pickup).
  * If `target_type` is empty, can be `status` or `listenIn`
* `event_data`: Filled when `event_type` is `queuePickup` or `listenIn`. Contains the call ID of the picked up call or call that's listened in on respectively.
* `user_id`: a numeric, unique identification of a user.
* `user_name`: the login name of a user.
* `user_fullname`: the full name of a user.
* `identity_id`: only filled when `event_type` is `login` or `remove`. A numeric, unique identification of the identity of a user (the agent triggering the event).
* `identity_name`: the name of the identity referenced by `identity_id`.
* `target_type`: the type of element the event applies to. Can be either `queue` or `phone`, or empty.
* `target_id`: the unique identification of the element which the event applies to. For example: if `target_type` is a queue, `target_id` contains the ID of the queue.
* `target_name`: the name of the element which the event applies to. For example: if `target_type` is a queue, this will contain the full name of the queue.
* `receive_calls`: for events of type `status`, the user's setting whether to receive calls (`all`, `none` or `only_direct`).
* `display_status`: for events of type `status`, the display status string of the user.
* `call_id`: for events of type `status` that pertain to the start of wrap-up time, the identifier of the call that caused the wrap-up time. 
* `end_time`: for events of type `status` that pertain to the start of wrap-up time, the timestamp at which wrap-up time ends. If the wrap-up duration is indefinite, this field is left empty.

### Downloading reports from the web interface

The page 'CDRs and Events' allows you to select the desired report and the desired timeframe.

Select a start and an end date, choose the type of report and choose the desired format. Default is CSV. If you select 'CSV for Excel' an extra header is added to the file that makes parsing in Microsoft Excel easier.

A maximum of one month worth of data can be downloaded at a time. If you need reports on a longer period, split it up in chunks of one month each.

### CDR API

CDRs are available for download through an HTTP API at this endpoint:

`https://files.{{site.compass.reseller.domain}}/cdr/$COMPANY_ID/$DATE/$CDR_TYPE.csv`

The variables in this URL are:
* `$COMPANY_ID`: the Company ID, as listed in the {{site.compass.reseller.prodname}} web interface on the Manage page for the company you want to request events for.
* `$DATE`: date (YYYY-MM-DD) - request events from this day.
* `$CDR_TYPE`: one of the three CDR types: `company`, `user` or `queue`.

Reports are limited to one day per request.

### Events API

Call Events can be downloaded from the following URL:

`https://files.{{site.compass.reseller.domain}}/events/$COMPANY_ID/calls?startTime=$TIME1&endTime=$TIME2`

User Events can be downloaded using this similar URL:

`https://files.{{site.compass.reseller.domain}}/events/$COMPANY_ID/users?startTime=$TIME1&endTime=$TIME2`

The variables in these URLs are:
* `$COMPANY_ID`: the Company ID, as listed in the {{site.compass.reseller.prodname}} web interface on the Manage page for the company you want to request events for.
* `$TIME1`: start time (UNIX timestamp, GMT time zone) - request events from this time.
* `$TIME2`: end time (UNIX timestamp, GMT time zone) - request events up to this time.

## Call recordings

### Introduction

Call recording can be enabled for an identity (user), and for an external number. Please note:
* A call recording is saved to file and sent to the configured email address after the call ends. The recording file could become large, particularly on long calls.
* Call recording files are **_not_** saved on the telephony platform. If the email with the attached call recording file cannot be delivered successfully (for example, because the receiving mailbox is full) the recording file cannot be recovered.
* The users in your company are responsible for announcing that calls are recorded, and for maintaining a reasonable retention period for call recordings.

### Automatic processing

To allow call recordings to be automatically processed, the following data is stored in the header of the email message containing a call recording:

* `X-Compass-CompanyId`: the unique identification of the company in which the call recording took place.
* `X-Compass-Company`: the shortname of the company in which the call recording took place.
* `X-Compass-IdentityId`*: the unique identification of the identity (user) for which the call recording was done. Empty for external number call recordings.
* `X-Compass-Identity`*: the name of the identity (user) for which the call recording was done. Empty for external number call recordings.
* `X-Compass-ExternalNumberId`*: the unique identification of the external number for which the call recording was done. Empty for identity call recordings.
* `X-Compass-ExternalNumber`*: the external number for which the call recording was done. Empty for identity call recordings.
* `X-Compass-Direction`: the direction of a call. Can be either `inbound` or `outbound`.
* `X-Compass-MD5`: The MD5 checksum for the attached call recording file.

The headers marked with a \* are optional and could be empty depending on the type of recording. For example: if an identity call recording is done, the X-Compass-ExternalNumberId and X-Compass-ExternalNumber headers will be empty.

Note: next to the data in the headers, information about the call recording is also present in the body of the email, such as the numbers of the caller and callee. The information is simplified: if a call is routed through a queue or forward before call recording is triggered, the values may be different than expected.
