# <a href='https://github.com/eduardom-chatbot/whatsapp-rpa/'><img src='./img/wrpa.png' height='60' alt='WhatsApp RPA' aria-label='https://github.com/eduardom-chatbot/whatsapp-rpa/' /></a>
 <a href='https://github.com/eduardom-chatbot/whatsapp-rpa/'>
Access WhatsApp RPA git and leave your star</a> <br>  <br>
<b>WhatsApp RPA</b> is a library that leverages WhatsApp functionality through RPA.

With <b>WhatsApp RPA</b>, you can create chatbots, customer service platforms, or any system that uses WhatsApp.

## Buy a license

The license costs $40 per month. To purchase, contact us via WhatsApp by clicking the image below!

<a target="_blank" href="https://web.whatsapp.com/send?phone=5521985413659&text=I%20want%20to%20buy%201%20license" target="_blank"><img title="whatsapp" height="100" width="375" src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f7/WhatsApp_logo.svg/2000px-WhatsApp_logo.svg.png"></a>

## Quickstart

To use WhatsApp RPA, run the command below in your project:

```bash
$ npm install whatsapp-rpa
```

or using yarn:

```bash
$ yarn add whatsapp-rpa
```

## Documentations

- <a href="#getting-started">Getting Started</a>
- <a href="#multiples-sessions">Multiples Sessions</a>
- <a href="#optional-parameters">Optional Parameters</a>
- <a href="#disconnect-functions">Disconnect Functions </a>
- <a href="#message-sending-functions">Message Sending Functions </a>
  - <a href="#send-message-text">Send Text </a>
  - <a href="#send-message-image">Send Image </a>
  - <a href="#send-message-video">Send Video </a>
  - <a href="#send-message-audio">Send Audio </a>
  - <a href="#send-message-audio-voice">Send Voice </a>
  - <a href="#send-message-document">Send Document </a>
- <a href="#get-all-contacts">Get All Contacts </a>
- <a href="#delete-message">Delete Message </a>
- <a href="#forwarding-message">Forwarding Message </a>
- <a href="#mute-chat">Mute Chat </a>
- <a href="#get-chats">Get Chats </a>
- <a href="#read-chat-messages">Read Chat Messages</a>
- <a href="#get-block-list">Get Block List </a>
- <a href="#archive-chat">Archive Chat </a>
- <a href="#delete-chat">Delete Chat </a>
- <a href="#pin-chat">Pin Chat </a>
- <a href="#block-contact">Block Contact </a>
- <a href="#get-profile-status">Get Profile Status </a>
- <a href="#get-picture">Get Picture </a>
- <a href="#set-picture">Set Picture </a>
- <a href="#get-number-profile">Get Number Profile </a>
- <a href="#groups-functions">Groups Functions </a>
  - <a href="#create-group">Create Group</a>
  - <a href="#add-participants-group">Add Participants Group</a>
  - <a href="#add=admins-group">Add Admins Group</a>
  - <a href="#change-name-of-group">Change Name of Group</a>
  - <a href="#change-name-of-group">Change Description of Group</a>
  - <a href="#leave-group">Leave Group</a>
  - <a href="#revoke-group-link">Revoke Group Link</a>
  - <a href="#set-group-settings">Set Group Settings</a>
  - <a href="#get-groups-list">Get Groups List</a>
- <a href="#get-host-device">Get Host Device</a>
- <a href="#chat-messages-functions">Chat Messages Functions</a>
- <a href="#update-presence">Update Presence</a>
- <a href="#send-messages-for-status">Send Messages for Status</a>
- <a href="#observation-events">Observation Events</a>

## Getting Started

```javascript
const whatsAppRPA = require("whatsapp-rpa");

async function start(){

  const client = await whatsAppRPA.create({
    qrMaxRetries: 5,
    license: "89YSLT7O-JBOLJQTF-R956WKVP-IPXUJNT0",
    session: "Marketing",
    statusFind: status=>{
      console.log(status);
    },
    qrcode: (sessionId, base64QR, asciiQR, urlCode)=>{
      console.log(asciiQR);
    },
    onMessage: async message => {
      if (message.type == "textMessage" && message.content.textMessage.text == "Hi") {
        await client.sendText(message.from, "Let's GO WhatsApp RPA");
      }
    }
  });

  return client;
}

(async function(){
  let client = await start();
  let response = await client.sendText('0000000000000', 'Thanks for using WhatsApp RPA!!!');
  console.log(response);
})();
```

## Multiples Sessions

After executing the create() function, **WhatsApp RPA** will create a WhatsApp Web instance. If you're not logged in, it will print a QR code on the terminal. Scan it with your phone, and you're done! **WhatsApp RPA** will save the session, eliminating the need for constant authentication. Multiple sessions can be created simultaneously by entering a different name for each session in the create() function:

```javascript
// Init sales WhatsApp bot
whatsAppRPA.create({
  session:'sales',
  license: "89YSLT7O-JBOLJQTF-R956WKVP-IPXUJNT0",
  qrcode: (sessionId, base64QR, asciiQR, urlCode)=>{
    console.log(asciiQR);
  }
}).then((salesClient) => {...});

// Init support WhatsApp bot
whatsAppRPA.create({
  session:'support',
  license: "89YSLT7O-JBOLJQTF-R956WKVP-IPXUJNT0",
  qrcode: (sessionId, base64QR, asciiQR, urlCode)=>{
    console.log(asciiQR);
  }
}).then((supportClient) => {...});
```

## Optional Parameters

Optional parameters are started along with the connection as events of **QRCODE and CONNECTION STATUS**, plus extra options

```javascript
const whatsAppRPA = require("whatsapp-rpa");

whatsAppRPA.create({
  session: "Marketing",
  license: "89YSLT7O-JBOLJQTF-R956WKVP-IPXUJNT0", // Valid license to use WhatsApp RPA
  qrMaxRetries: 5, // Number of connection attempts,
  qrcode: (sessionId, base64QR, asciiQR, urlCode) => {
    console.log("sessionId: " + sessionId)
    console.log("base64 image of qrcode: " + base64QR);
    console.log("Terminal image of qrcode in caracter ascii: " + asciiQR);
    console.log("Terminal string hash of qrcode: " + urlCode);
  },
  statusFind: (statusSession) => {
    console.log("Status Session: ", statusSession);
  },
  onMessage: (message) => { // Receive an event all the time you receive a message from some contact
    console.log(message);
  }
});
```
## Callback StatusFind

Track the connection status of your sessions using the statusFind function or by accessing the client.status attribute. The following codes are generated:

| Status                  | Condition                                                                                                                                                      |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `disconnected`    | The client has disconnected or has been disconnected                                                                                                                           |
| `waitingForQRCodeScan`        | Waiting for QR Code to scan                                                                                                                       |
| `qrTimeOut`        | Max qrcode retries reached                                                                                                                       |
| `checkingPhoneIsConnected`        | Checking phone is connected                                                                                                                       |
| `isConnected`        | The client has successfully connected list                                                                                                                       |
| `ready`        | The client is ready to be used                                                                                                                       |
| `clientDestroyded`        | Disconnected for WhatsApp socket server                                                                                                                       |
| `restarting`        | Restarting connection                                                                                                                       |

## Disconnect and restart Functions

> Disconnect Function

```javascript
  client.disconnect();
```

> Restart Function

```javascript
  client.restart();
```

## Message Sending Functions

We've developed the easiest way to send messages through **WhatsApp RPA**.

> Sending messages can be sent to the contact's number, example: **5561999999999**

### Send Message Text

```javascript
let response = await client.sendText("5561999999999", "Thanks for using WhatsApp RPA!😀");

```

##### Return with success
```javascript
{
  status: 200,
  type: 'text',
  isMedia: false,
  id: '4FB07A601E54',
  to: '5561999999999',
  from: '5521988888888',
  content: { textMessage: { text: 'Thanks for using WhatsApp RPA!😀' } },
  timestamp: 1757873296
}
```
##### Return with erro

```javascript
{
  isError: true,
  status: 400 | 401 | 500,
  type: 'text',
  message: 'message of erro'
}
```

### Send Message Image

> For image submission, you can use URL or the local file path or base64

```javascript
let response = await client.sendImage("5561999999999", "https://images.pexels.com/photos/15173523/pexels-photo-15173523.jpeg", { caption: "Text optional" });

```

##### Return with success
```javascript
{
  status: 200,
  type: 'image',
  isMedia: true,
  to: '5561999999999',
  from: '5522988888888',
  id: '7AC0FA7E1641',
  content: {
    imageMessage: {
      mediaKey: "q7xP14wfpAp0dEz-hhyaRJxK/Bu61TQpBaUtcnI67Co=",
      directPath: "/o7/c/y42/a4/q199/XnU2AQNLa9_mQf--F1J-S5xG4CXiBo1vQn-5UQa4vqKRWCICXlFl0J4zdnrYjHzqfqAz-ng-AHV76ljpD_Dwj6IgRZTRyHvpj5BWFsclfg?ccb=9-4&oh=90_gQo4Q5Aa27Zj06p7XT6_0ZrJQndr7hYhTmBgJxTknjvn2pw9ZG&yw=13G1BYYY&_td_zue=f9uw1q",
      url: "https://mmg.whatsapp.net/o7/c/y42/a4/q199/YWJBmP1Pp6_mQf--E2Z-xo3XSG9C5YNvQn-TWBJlCI1WKqFl0JAja6vkHWq7zdnyfqAz-ng-AH6ljpDV7_6IgRvpj5BWFsclfgDwjZTRyH?ddc=1-4&oh=01_Q5Aa2gHqYhZp09WWhIPard9nBgJxDdtjtm1X97n5UE1_3pc0XQ&oe=68EEEF0A&_pd_sbd=e6pr9q&ffs3=true",
      mediaKeyTimestamp: 1757898812,
      mimetype: "image/jpeg",
      encFilehash: "T1qaJ1fujEzaKCWZp9Die4AOATWBgOsaU6tQtwcKiGP=",
      filehash: "J/sODI1jwfut79ObwjqbUhFIjnN6sLYWokM+V6IqVbu="
    }
  },
  caption: "Text optional",
  timestamp: 1757875223
}

```
##### Return with erro

```javascript
{
  isError: true,
  status: 400 | 401 | 500,
  type: 'image',
  message: 'message of erro'
}
```

### Send Message Video
> For video submission, you can use URL or the local file path or base64

```javascript
let response = await client.sendVideo('5561999999999', "./video.mp4", { caption: "Text optional" });

```

##### Return with success
```javascript
{
  status: 200,
  type: "video",
  isMedia: true,
  to: "5561999999999",
  from: "5522988888888",
  id: "5AC0FA7E1742",
  content: {
    videoMessage: {
      mediaKey: "GyavqJuypZYsAZa/yTTileBa6P7u1vF17w//Fe0l10=",
      directPath: "/v/t57.1090-15/770801971_0118991146473844_1591834079100596746_n.enc?ccb=11-4&oh=02_P4cB7hfmDSpkhC57y4cdEuEDnn7D1F86zccBCHY6Fj8zBPW8Pw&PF=50G1072A&_nc_sid=5e94e0",
      url: "https://mmg.whatsapp.net/v/t57.1090-15/770801971_0118991146473844_1591834079100596746_n.enc?ccb=11-4&oh=02_P4cB7hfmDSpkhC57y4cdEuEDnn7D1F86zccBCHY6Fj8zBPW8Pw&PF=50G1072A&_nc_sid=5e94e0&mms3=true",
      mediaKeyTimestamp: 1758245795,
      mimetype: "video/mp4",
      encFilehash: "Q7cymaXBy/UAn+1pqh1fsqvGEEZXOmjLYZTNM0f/M/k=",
      filehash: "pcD85MlqaCHYj9Gbx7NcA+TWaaBx1ZJlJYyMk/geBbk="
    }
  },
  caption: "Text optional",
  timestamp: "1757878378"
}

```
##### Return with erro

```javascript
{
  isError: true,
  status: 400 | 401 | 500,
  type: 'video',
  message: 'message of erro'
}
```

### Send Message Audio
> For audio submission, you can use URL or the local file path or base64

```javascript
let response = await client.sendAudio("5561999999999", "./audio.mp3");

```

##### Return with success
```javascript
{
  status: 200,
  type: "audio",
  isMedia: true,
  to: "5561999999999",
  from: "5522988888888",
  id: "5AC1FA7E1752",
  content: {
    audioMessage: {
      mediaKey: "GMpwatj7CaI4fba1cFqf0HTbxHWujvTaxWy9BIF77vk=",
      directPath: "/v/t55.1004-28/91017391_216714067413181_9171357172587152137_n.enc?ccb=11-4&oh=07_P7BW1fFcqLotdEeQd1FcQpmtwAvb-87BzCE-0bvTWqUfsBTaFc&oe=51F31Ta3&_nc_sid=1f13e9",
      url: "https://mmg.whatsapp.net/v/t55.1004-28/91017391_216714067413181_9171357172587152137_n.enc?ccb=11-4&oh=07_P7BW1fFcqLotdEeQd1FcQpmtwAvb-87BzCE-0bvTWqUfsBTaFc&oe=51F31Ta3&_nc_sid=1f13e9&mms3=true",
      mediaKeyTimestamp: 1758241922,
      mimetype: "audio/mpeg",
      encFilehash: "c1zU1qa7MyJue6bz1TAB9ovNaaPaYa816qw+yA03M1M=",
      filehash: "qZFY1bM1Uac7Md2UG1711uwOdhh1F7WuiPEBBI1WMnY="
    }
  },
  timestamp: "1757878450"
}

```
##### Return with erro

```javascript
{
  isError: true,
  status: 400 | 401 | 500,
  type: 'audio',
  message: 'message of erro'
}
```
### Send Message Audio Voice
> For audio voice submission, you can use URL or the local file path or base64

```javascript
let response = await client.sendVoice("5561999999999", "./audio.mp3");

```

##### Return with success
```javascript
{
  status: 200,
  type: "voice",
  isMedia: true,
  to: "5561999999999",
  from: "5522988888888",
  id: "4EB172B279F6",
  content: {
    audioMessage: {
      mediaKey: "VPfWaPNa91Ma+1mc1T1rcaqbaaaxtlNnFFv1aDufpO1=",
      directPath: "/v/t28.1007-10/130751491_0714618212810416_3372171241161710107_n.enc?ccb=11-4&oh=06_P4Cw1fQbgYP1WZbdQFYHC_4tUacdNb1uTJY-1ybfuvqz179cew+Bf=19F71C1g&_nc_sid=1f18e1",
      url: "https://mmg.whatsapp.net/v/t28.1007-10/130751491_0714618212810416_3372171241161710107_n.enc?ccb=11-4&oh=06_P4Cw1fQbgYP1WZbdQFYHC_4tUacdNb1uTJY-1ybfuvqz179cew+Bf=19F71C1g&_nc_sid=1f18e1&mms3=true",
      mediaKeyTimestamp: 1758245472,
      mimetype: "audio/ogg; codecs=opus",
      encFilehash: "AvtuWb1fPcemaQfb0FzfyO1N09a1yCuzy/UctfacIFa=",
      filehash: "ae1y17cla0if/lBcp1Paifj1FaC1rMvCtb/ac1cKYza="
    }
  },
  timestamp: "1757878999"
}

```
##### Return with erro

```javascript
{
  isError: true,
  status: 400 | 401 | 500,
  type: 'voice',
  message: 'message of erro'
}
```

### Send Message Document
> For document submission, you can use URL or the local file path or base64

```javascript
let response = await client.sendDocument("5561999999999", "./pdf-test.pdf", "Filename Optional");

```

##### Return with success
```javascript
{
  status: 200,
  type: "document",
  isMedia: true,
  to: "5561999999999",
  from: "5522988888888",
  id: "1EB041C279F7",
  content: {
    documentMessage: {
      mediaKey: "AbwFaC7EczdYFAPGv7Bgxz3IMNaqtcI1fpqlc17aChJ=",
      directPath: "/v/t14.6001-67/121765183_7280162821611872_6711610756718128711_n.enc?ccb=11-4&oh=07_Q6A171Ga-z8ATceaQbRacu70juzqFACi17f1sGAcODcp0hUgtzwaf=71c51d1B&_nc_sid=1f19f1",
      url: "https://mmg.whatsapp.net/v/t14.6001-67/121765183_7280162821611872_6711610756718128711_n.enc?ccb=11-4&oh=07_Q6A171Ga-z8ATceaQbRacu70juzqFACi17f1sGAcODcp0hUgtzwaf=71c51d1B&_nc_sid=1f19f1&mms3=true",
      mediaKeyTimestamp: 1758243185,
      mimetype: "application/pdf",
      encFilehash: "a7MapaXdwfQzfVfcdaMja41cS7dFted7fS1OFdfdisk=",
      filehash: "dq1fAq+fbcFeaNpcqaCC78aCybo1w0UZVicfw1bcFXs="
    }
  },
  timestamp: "1757879619"
}
```
##### Return with erro

```javascript
{
  isError: true,
  status: 400 | 401 | 500,
  type: 'document',
  message: 'message of erro'
}
```

## Get All Contacts

> List All Contacts


```javascript
let response = await client.getAllContacts();

```

Return with success 
```javascript
{
  type: 'get-all-contacts',
  chats: [
    {
      id: {
        server: 'c.us',
        user: '5521999999999',
        _serialized: '5521999999999@c.us'
      },
      number: '5521999999999',
      isBusiness: false,
      isEnterprise: false,
      name: 'John Doe',
      pushname: 'John Doe',
      shortName: 'John',
      statusMute: false,
      type: 'in',
      isMe: true,
      isUser: true,
      isGroup: false,
      isWAContact: true,
      isMyContact: true,
      isBlocked: false
    }
  ]
}
```
Return with erro
```javascript
{
  status: 400 | 401 | 500,
  type: 'get-all-contacts',
  message: 'message of erro'
}
```
## Get Block List

> List All Contacts Blocking


```javascript
let response = await client.getBlockList();

```

Return with success 
```javascript
{
  type: 'get-block-list',
  list: [ '5561911111111', '5561922222222' ]
}
```
Return with erro
```javascript
{
  status: 400 | 401 | 500,
  type: 'get-block-list',
  message: 'message of erro'
}
```

## Mute Chat

> Silence or remove the silence of a particular chat for a specific period


**Mute**
```javascript
//number of chat, time in hours
let response = await client.muteChat("5561955555555", 8);

```
**Unmute**
```javascript
//number of chat
let response = await client.unmuteChat("5561955555555");

```

Return with success 
```javascript
{
  device: "5521977777777",
  status: 200,
  type: 'mute-chat',
}
```
Return with erro
```javascript
{
  device: "5521977777777",
  status: 400 | 401 | 500,
  type: 'mute-chat',
  message: 'message of erro'
}
```
## Archive Chat

> Archive or unarchive a specific chat


**Archive**
```javascript
//number of chat, true
let response = await client.archiveChat("5561955555555", true);

```
**Unarchive**
```javascript
//number of chat, false
let response = await client.archiveChat("5561955555555", false);

```

Return with success 
```javascript
{
  device: "5521977777777",
  status: 200,
  type: 'archive-chat',
}
```
Return with erro
```javascript
{
  device: "5521977777777",
  status: 400 | 401 | 500,
  type: 'archive-chat',
  message: 'message of erro'
}
```
## Delete Chat

> Delete a specific chat

```javascript
//number of chat or group
let response = await client.deleteChat("5561955555555");
```

Return with success 

```javascript
{
  device: "5521977777777",
  status: 200,
  type: 'delete-chat'
}
```
Return with erro
```javascript
{
  device: "5521977777777",
  status: 400 | 401 | 404 | 500,
  type: 'delete-chat',
  message: 'message of error'
}
```
## Pin Chat

> Pin or unpin a specific chat


**Pin**
```javascript
//number of chat, true
let response = await client.pinChat("5561955555555", true);

```
**Unpin**
```javascript
//number of chat, false
let response = await client.pinChat("5561955555555", false);

```

Return with success 

```javascript
{
  device: "5521977777777",
  status: 200,
  type: "pin-chat"
}
```
Return with erro
```javascript
{
  device: "5521977777777",
  status: 400 | 401 | 404 | 500,
  message: 'message of error',
  type: "pin-chat"
}
```
## Block Contact

> Blocking or unblocking a specific contact


**Block**
```javascript
//number of chat
let response = await client.blockContact("5561955555555");

```
**Unblock**
```javascript
//number of chat
let response = await client.unblockContact("5561955555555");

```

Return with success 

```javascript
{
  device: "5521977777777",
  status: 200,
  type: 'block-contact'
}
```
Return with erro
```javascript
{
  device: "5521977777777",
  status: 400 | 401 | 500,
  type: 'block-contact',
  message: "message of error"
}
```
## Get Profile Status

> Displays the text of the status of a specific contact


```javascript
//number of chat
let response = await client.getProfileStatus("5561955555555");

```

Return with success 

```javascript
{
  device: "5521977777777",
  status: "Hello World!",
  type: 'get-profile-status'
}
```
Return with erro
```javascript
{
  device: this.contact,
  status: 400 | 401 | 500,
  type: 'get-profile-status',
  message: "message of error"
}
```
## Get Picture

> Displays the image of a specific contact


```javascript
//number of chat
let response = await client.getPicture("5561955555555");

```

Return with success 

```javascript
{
  device: "5521977777777",
  status: 200,
  type: "get-picture",
  img: ""
}
```
Return with erro
```javascript
{
  device: "5521977777777",
  status: 400 | 401 | 500,
  type: 'get-picture',
  message: "message of error"
}
```
## Set Picture

> Set image for profile or group


```javascript
//number of chat, file local path
let response = await client.setPicture("5521977777777", "./image-profile.png");

```

Return with success 

```javascript
{
  device: "5521977777777",
  status: 200,
  type: 'set-picture'
}
```
Return with erro
```javascript
{
  device: "5521977777777",
  status: 400 | 401 | 500,
  type: 'set-picture',
  message: "message of error"
}
```
## Get Number Profile

> Checks if a number exists in the WhastApp

```javascript
//number of chat
let response = await client.getNumberProfile("5561955555555");

```

Return with success 

```javascript
{
  type: 'get-number-profile',
  phone: "5561955555555",
  exist: true,
  isBusiness: false,
  isEnterprise: false,
  pushname: "John Doe",
  shortName: "John"
}
```
Return with erro
```javascript
{
  type: 'get-number-profile',
  status: 400 | 401 | 404 | 500,
  message: "message of error"
}
```

## Groups Functions

We created the easiest way to create groups with **WhatsApp RPA**

## Create Group

> Create a group with participants


```javascript
//name of group, array with number of contacts
let response = await client.createGroup("Name Group", ["5561966666666", "5561977777777"]);

```

Return with success 

```javascript
{
  device: "5521977777777",
  type: 'create-group',
  status: 200,
  groupId: "029363422217613551"
}
```
Return with erro
```javascript
{
  device: "5521977777777",
  status: 400 | 401 | 404 | 500,
  type: 'create-group',
  message: "message of error"
}
```

## Add Participants Group

> Add participants in group


```javascript
//id of group, array with number of contacts
let response = await client.addParticipantsGroup("029363422217613551", ["556155555555", "5561966666666"]);

```

Return with success 

```javascript
{
  device: "5521977777777",
  status: 200,
  type: 'add-participants-group'
}
```
Return with erro
```javascript
{
  device: "5521977777777",
  status: 400 | 401 | 404 | 500,
  type: 'add-participants-group',
  message: "message of error"
}
```
## Remove Participants Group

> Remove participants in group


```javascript
//id of group, array with number of contacts
let response = await client.removeParticipantsGroup("029363422217613551", ["556155555555", "5561966666666"]);

```

Return with success 

```javascript
{
  device: "5521977777777",
  status: 200,
  type: 'remove-participants-group'
}
```
Return with erro
```javascript
{
  device: "5521977777777",
  status: 400 | 401 | 404 | 500,
  type: 'remove-participants-group',
  message: "message of error"
}
```

## Add Admins Group

> Add or Remove participants of group as admin


```javascript
//id of group, array with number of contacts
let response = await client.addGroupAdmins("029363422217613551", ["556111111111", "5561922222222"]);

```
```javascript
//id of group, array with number of contacts
let response = await client.removeGroupAdmins("029363422217613551", ["556111111111", "5561922222222"]);

```

Return with success 

```javascript
{
  device: "5521977777777",
  status: 200,
  type: 'add-group-admins'
}
```
Return with erro
```javascript
{
  device: "5521977777777",
  status: 500,
  message: "message of error",
  type: 'add-group-admins'
}
```

## Set Picture of Group

> Set picture of group


```javascript
//id of group, URL or the local file path or base64
let response = await client.setGroupPicture("029363422217613551", "./logo.png");

```

Return with success 

```javascript
{
  device: "5521977777777",
  status: 200,
  type: 'set-group-picture'
}
```
Return with erro
```javascript
{
  device: "5521977777777",
  status: 400 | 401 | 404 | 500,
  message: "message of error",
  type: 'set-group-picture'
}
```

## Change Name of Group

> Change name of group


```javascript
//id of group, name group
let response = await client.groupTitle("029363422217613551", "new name of group");

```

Return with success 

```javascript
{
  device: "5521977777777",
  status: 200,
  type: 'group-title'
}
```
Return with erro
```javascript
{
  device: "5521977777777",
  status: 400 | 401 | 404 | 500,
  message: "message of error",
  type: 'group-title'
}
```
## Change Description of Group

> Change description of group


```javascript
//id of group, description group
let response = await client.groupDescription("029363422217613551", "description of group");

```

Return with success 

```javascript
{
  device: "5521977777777",
  status: 200,
  type: 'group-description'
}
```
Return with erro
```javascript
{
  device: "5521977777777",
  status: 400 | 401 | 404 | 500,
  type: 'group-description',
  message: 'message of error'
}
```

## Leave Group

> Leaves a group specified


```javascript
//id of group
let response = await client.leaveGroup("029363422217613551");

```

Return with success 

```javascript
{
  device: "5521977777777",
  status: 200,
  type: 'leave-group'
}
```
Return with erro
```javascript
{
  device: "5521977777777",
  status: 400 | 401 | 404 | 500,
  type: 'leave-group',
  message: error.message
}
```

## Revoke Group Link

> Revoke link from a specified group


```javascript
//id of group
let response = await client.revokeGroupLink("029363422217613551");

```

Return with success 

```javascript
{
  device: "5521977777777",
  status: 200,
  type: "revoke-group-link",
  linkGroup: "w3M40fQyuoDB2zaBoPIx0q"
}
```
Return with erro
```javascript
{
  device: "5521977777777",
  status: 400 | 401 | 500,
  message: "message of error",
  type: 'revoke-group-link'
}
```

## Set Group Settings

> Get info from a specified group

**Set sending messages in group only for admins**
```javascript
//id of group, type, boolean
let response = await client.setGroupSettings("029363422217613551", "message", true);

```
**Set change settings in group only for admins**
```javascript
//id of group, type, boolean
let response = await client.setGroupSettings("029363422217613551", "settings", true);

```

**Set add members in group only for admins**
```javascript
//id of group, type, boolean
let response = await client.setGroupSettings("029363422217613551", "add-members", true);

```

Return with success 

```javascript
{
  device: "5521977777777",
  status: 200,
  type: 'set-group-settings'
}
```
Return with erro
```javascript
{
  device: "5521977777777",
  status: 400 | 500,
  message: "Falha ao definir configurações",
  type: 'set-group-settings'
}
```
## Get Groups List

> Get all groups list

```javascript
let response = await client.getGroups();

```

Return with success 

```javascript
{
  device: "5521977777777",
  status: 200,
  type: "get-groups",
  groups: [{ id: "029363422217613551", name: "Manutenção de Computadores" }]
}
```
Return with erro
```javascript
{
  device: "5521977777777",
  status: 500,
  message: "message of error",
  type: 'get-groups'
}
```

## Get Host Device

> Get info of device

```javascript
let response = await client.getHostDevice();

```

Return with success 

```javascript
{
  status: 200,
  type: 'get-host-device',
  device: "5521977777777",
  phone: "5521977777777"
}
```
Return with erro
```javascript
{
  session: 'Marketing',
  status: 404,
  type: 'get-host-device',
  message: 'message of erro'
}
```

## Update Presence

Update your presence for a certain contact

Types of state: a = available, c = composing, r = recording, p = paused

```javascript
//chat number, state: a, c, r, p, milliseconds
let response = await client.setPresence('5561955555555', 'c', 5000);

```
Return with success

```javascript
{ 
  status: 200, 
  type: 'set-presence' 
}
```

Return with erro
```javascript
{
  status: 400 | 500,
  type: 'set-presence',
  message: 'message of erro'
}
```

## Observation Events

> Follow each event at the time that happen


### **Received Message Event**
<br>

> Receive an event all the time you receive a message from some contact

```javascript
//event:any
const client = await whatsAppRPA.create({
  license: "89YSLT7O-JBOLJQTF-R956WKVP-IPXUJNT0",
  onMessage: (event) => {
    console.log(event);
  }
});
```
Return of event onMessage

```javascript
{
  id: '4EB17B5B1179A8CB417B',
  event: "on-any-message",
  subevent: "received",
  type: "textMessage",
  isMedia: false,
  pushName: "John Doe",
  from: "5521988888888",
  to: "5511977777777",
  content: {
    textMessage: {
      text: "Hi"
    }
  },
  isGroup: false,
  participant: "",
  timestamp: 1633414066
}
```

