just web applications, displayed in WebView - so just some static files (css, js, html).

- recommended for bigger applications: react, typescript, scss

Create a Mini App when you want to make user life easier when displaying several buttons is not even close to the functionality, you want to provide.

# Ways to start a mini app
#### Keyboard Button Mini Apps

> **TL;DR:** Mini Apps launched from a **web_app** type [keyboard button](https://core.telegram.org/bots/api#keyboardbutton) can send data back to the bot in a _service message_ using [Telegram.WebApp.sendData](https://core.telegram.org/bots/webapps#initializing-mini-apps). This makes it possible for the bot to produce a response without communicating with any external servers.

Users can interact with bots using [custom keyboards](https://core.telegram.org/bots#keyboards), [buttons under bot messages](https://core.telegram.org/bots#inline-keyboards-and-on-the-fly-updating), as well as by sending freeform **text messages** or any of the **attachment types** supported by Telegram: photos and videos, files, locations, contacts and polls. For even more flexibility, bots can utilize the full power of **HTML5** to create user-friendly input interfaces.

You can send a **web_app** type [KeyboardButton](https://core.telegram.org/bots/api#keyboardbutton) that opens a Mini App from the specified URL.

To transmit data from the user back to the bot, the Mini App can call the [Telegram.WebApp.sendData](https://core.telegram.org/bots/webapps#initializing-mini-apps) method. Data will be transmitted to the bot as a String in a service message. The bot can continue communicating with the user after receiving it.

**Good for:**

- **Сustom data input interfaces** (a personalized calendar for selecting dates; selecting data from a list with advanced search options; a randomizer that lets the user “spin a wheel” and chooses one of the available options, etc.)
- **Reusable components** that do not depend on a particular bot.
#### Inline Button Mini Apps
> **TL;DR:** For more interactive Mini Apps like [@DurgerKingBot](https://t.me/durgerkingbot), use a **web_app** type [Inline KeyboardButton](https://core.telegram.org/bots/api#inlinekeyboardbutton), which gets basic user information and can be used to send a message on behalf of the user to the chat with the bot.

If receiving text data alone is insufficient or you need a more advanced and personalized interface, you can open a Mini App using a **web_app** type [Inline KeyboardButton](https://core.telegram.org/bots/api#inlinekeyboardbutton).

From the button, a Mini App will open with the URL specified in the button. In addition to the user's [theme settings](https://core.telegram.org/bots/webapps#color-schemes), it will receive basic user information (`ID`, `name`, `username`, `language_code`) and a unique identifier for the session, **query_id**, which allows messages on behalf of the user to be sent back to the bot.

The bot can call the Bot API method [answerWebAppQuery](https://core.telegram.org/bots/api#answerwebappquery) to send an inline message from the user back to the bot and close the Mini App. After receiving the message, the bot can continue communicating with the user.

**Good for:**

- Fully-fledged web services and integrations of any kind.
- The use cases are effectively **unlimited**.
#### Launching Mini Apps from the Menu Button

> **TL;DR:** Mini Apps can be launched from a customized menu button. This simply offers a quicker way to access the app and is otherwise **identical** to [launching a mini app from an inline button](https://core.telegram.org/bots/webapps#inline-button-mini-apps).

By default, chats with bots always show a convenient **menu button** that provides quick access to all listed [commands](https://core.telegram.org/bots#commands). With [Bot API 6.0](https://core.telegram.org/bots/api-changelog#april-16-2022), this button can be used to **launch a Mini App** instead.
To configure the menu button, you must specify the text it should show and the Mini App URL. There are two ways to set these parameters:

- To customize the button for **all users**, use [@BotFather](https://t.me/botfather) (the `/setmenubutton` command or _Bot Settings > Menu Button_).
- To customize the button for both **all users** and **specific users**, use the [setChatMenuButton](https://core.telegram.org/bots/api#setchatmenubutton) method in the Bot API. For example, change the button text according to the user's language, or show links to different Mini Apps based on a user's settings in your bot.

Apart from this, Mini Apps opened via the menu button work in the exact same way as when [using inline buttons](https://core.telegram.org/bots/webapps#inline-button-mini-apps).

> [@DurgerKingBot](https://t.me/durgerkingbot) allows launching its Mini App both from an inline button and from the menu button.
#### Inline Mode Mini Apps

> **TL;DR:** Mini Apps launched via **web_app** type [InlineQueryResultsButton](https://core.telegram.org/bots/api#inlinequeryresultsbutton) can be used anywhere in inline mode. Users can create content in a web interface and then seamlessly send it to the current chat via inline mode.

You can use the _button_ parameter in the [answerInlineQuery](https://core.telegram.org/bots/api#answerinlinequery) method to display a special 'Switch to Mini App' button either above or in place of the inline results. This button will **open a Mini App** from the specified URL. Once done, you can call the [Telegram.WebApp.switchInlineQuery](https://core.telegram.org/bots/webapps#initializing-mini-apps) method to send the user back to inline mode.

Inline Mini Apps have **no access** to the chat – they can't read messages or send new ones on behalf of the user. To send messages, the user must be redirected to **inline mode** and actively pick a result.

**Good for:**

- Fully-fledged web services and integrations in inline mode.
#### Direct Link Mini Apps

> **TL;DR:** Mini App Bots can be launched from a direct link in any chat. They support a _startapp_ parameter and are aware of the current chat context.

You can use direct links to **open a Mini App** directly in the current chat. If a non-empty _startapp_ parameter is included in the link, it will be passed to the Mini App in the _start_param_ field and in the GET parameter _tgWebAppStartParam_.

In this mode, Mini Apps can use the _chat_type_ and _chat_instance_ parameters to keep track of the current chat context. This introduces support for **concurrent** and **shared** usage by multiple chat members – to create live whiteboards, group orders, multiplayer games and similar apps.

Mini Apps opened from a direct link have **no access** to the chat – they can't read messages or send new ones on behalf of the user. To send messages, the user must be redirected to **inline mode** and actively pick a result.

**Examples**

`https://t.me/botusername/appname`  
`https://t.me/botusername/appname?startapp=command`

**Good for:**

- Fully-fledged web services and integrations that any user can open in one tap.
- Cooperative, multiplayer or teamwork-oriented services within a chat context.
- The use cases are effectively **unlimited**.

#### Launching Mini Apps from the Attachment Menu

> **TL;DR:** Mini App Bots can request to be added directly to a user's attachment menu, allowing them to be quickly launched from any chat.

Mini App Bots can request to be added directly to a user's attachment menu, allowing them to be quickly launched from **any type of chat**. You can configure in which types of chats your mini app can be started from the attachment menu (private, groups, supergroups or channels).

Attachment menu integration is currently only available for major advertisers on the [Telegram Ad Platform](https://promote.telegram.org/basics). However, **all bots** can use it in the [test server environment](https://core.telegram.org/bots/webapps#using-bots-in-the-test-environment).

To enable this feature for your bot, open [@BotFather](https://t.me/botfather) [from an account on the test server](https://core.telegram.org/bots/webapps#using-bots-in-the-test-environment) and send the `/setattach` command – or go to _Bot Settings > Configure Attachment Menu_. Then specify the URL that will be opened to launch the bot's Mini App via its icon in the attachment menu.

You can add a 'Settings' item to the context menu of your Mini App using [@BotFather](https://t.me/botfather). When users select this option from the menu, your bot will receive a `settingsButtonClicked` event.

In addition to the user's [theme settings](https://core.telegram.org/bots/webapps#color-schemes), the bot will receive basic user information (`ID`, `name`, `username`, `language_code`, `photo`), as well as public info about the chat partner (`ID`, `name`, `username`, `photo`) or the chat info (`ID`, `type`, `title`, `username`, `photo`) and a unique identifier for the web view session **query_id**, which allows messages of any type to be sent to the chat on behalf of the user that opened the bot.

The bot can call the Bot API method [answerWebAppQuery](https://core.telegram.org/bots/api#answerwebappquery), which sends an inline message from the user via the bot to the chat where it was launched and closes the Mini App.

> You can read more about adding bots to the attachment menu [here](https://core.telegram.org/bots/webapps#adding-bots-to-the-attachment-menu).

---
# Receive data from Mini App to Bot
a bot can receive data from a mini app only if triggered from a keyboard (not an InlineKeyboard).

### pattern for sending data to bot from inlinekeyboard
- mini app should call an API sending data (with chatID). for a webhook bot this api can be the webhook itself with a dedicated endpoint. this api should be able to send the desired message to the chat providing the right chatID

# Test environment

# Methods and events

The next thing important to know is there are 2 terms that are usually used in the context of apps communication:

- `events` are received by Mini App from Telegram app;
- `methods` are called by Mini App and executed by Telegram app
https://docs.telegram-mini-apps.com/apps-communication/methods

Internally, each of them is the event, but we will use these terms as a convention to speak the same language.



# Best Practices
- #### Meta tags
```html
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
<meta name="format-detection" content="telephone=no"/>
<meta http-equiv="X-UA-Compatible" content="IE=edge"/>
<meta name="MobileOptimized" content="176"/>
<meta name="HandheldFriendly" content="True"/>
<meta name="robots" content="noindex,nofollow"/>
```
- #### Links

```html
<a id="regular_link" href="">Regular link #2</a> (opens inside webview)
<a href="https://telegram.org/" target="_blank">target="_blank" link</a> (opens outside webview)
<a href="javascript:window.open('https://telegram.org/');">window.open() link</a> (opens outside webview)
<a href="https://t.me/like">LikeBot t.me link</a> (opens inside Telegram app)
<a href="javascript:Telegram.WebApp.openTelegramLink('https://t.me/vote');">web_app_open_tg_link()</a> (opens inside Telegram app)
<a href="javascript:Telegram.WebApp.openLink('https://google.com/');">web_app_open_link()</a> (opens outside webview)
<a href="tg://resolve?domain=vote">VoteBot tg:// link</a> (does not open)
<a href="javascript:Telegram.WebApp.openLink('https://telegra.ph/api',{try_instant_view:true});">web_app_open_link({try_instant_view:true})</a> (opens IV inside Telegram app)
```
- #### Permissions
```html
<a href="javascript:;" onclick="return DemoApp.requestLocation(this);">Request Location</a>
<a href="javascript:;" onclick="return DemoApp.requestVideo(this);">Request Video</a>
<a href="javascript:;" onclick="return DemoApp.requestAudio(this);">Request Audio</a>
<a href="javascript:;" onclick="return DemoApp.requestAudioVideo(this);">Request Audio+Video</a>
<a href="javascript:;" onclick="return DemoApp.testClipboard(this);" id="clipboard_test">Read from clipboard</a>
```
- #### Alerts
```html
<a href="javascript:;" onclick="alert('Hello!');">alert</a>
<a href="javascript:;" onclick="confirm('Are you sure?');">confirm</a>
<a href="javascript:;" onclick="prompt('How old are you?');">prompt</a>
<a href="javascript:;" onclick="DemoApp.showAlert('Hello!');">showAlert</a>
<a href="javascript:;" onclick="DemoApp.showConfirm('Are you sure?');">showConfirm</a>
<a href="javascript:;" onclick="DemoApp.requestWriteAccess();">requestWriteAccess</a>
<a href="javascript:;" onclick="DemoApp.requestContact();">requestContact</a>
<a href="javascript:;" onclick="DemoApp.showPopup();">showPopup</a>
<a href="javascript:;" onclick="DemoApp.showScanQrPopup();">showScanQrPopup</a>
<a href="javascript:;" onclick="DemoApp.showScanQrPopup(true);">showScanQrPopup (links only)</a>
```
- #### Haptics
```html
Impact: <a href="javascript:Telegram.WebApp.HapticFeedback.impactOccurred('heavy');">heavy</a>
<a href="javascript:Telegram.WebApp.HapticFeedback.impactOccurred('light');">light</a>
<a href="javascript:Telegram.WebApp.HapticFeedback.impactOccurred('medium');">medium</a>
<a href="javascript:Telegram.WebApp.HapticFeedback.impactOccurred('rigid');">rigid</a>
<a href="javascript:Telegram.WebApp.HapticFeedback.impactOccurred('soft');">soft</a>

Notification: <a href="javascript:Telegram.WebApp.HapticFeedback.notificationOccurred('error');">error</a>
<a href="javascript:Telegram.WebApp.HapticFeedback.notificationOccurred('success');">success</a>
<a href="javascript:Telegram.WebApp.HapticFeedback.notificationOccurred('warning');">warning</a>

Selection: <a href="javascript:Telegram.WebApp.HapticFeedback.selectionChanged();">changed</a>
```

# Developing a Mini App

https://core.telegram.org/bots/webapps
https://docs.telegram-mini-apps.com/

VanillaJS Boilerplate https://github.com/telegram-mini-apps-dev/vanilla-js-boilerplate/blob/master/index.html
https://github.com/revenkroz/telegram-web-app-bot-example/blob/master/index.html

## Node backend

## React
https://github.com/vkruglikov/react-telegram-web-app?tab=readme-ov-file

# Resources
https://docs.telegram-mini-apps.com/guides/creating-new-app
https://docs.telegram-mini-apps.com/functionality/viewport
https://core.telegram.org/api/bots/webapps
https://core.telegram.org/bots/webapps#initializing-mini-apps
https://core.telegram.org/bots/webapps#validating-data-received-via-the-mini-app