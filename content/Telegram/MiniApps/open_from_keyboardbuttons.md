# on keyboardButton give both url and WebAppInfo or just WebAppInfo?
### both URL and WebAppInfo
- on iOs device a popup asking to "leave telegram" always appears, in other devices only on the first time.
- top right side of btn: "link icon"
- URL must be "direct url" given by BotFather when creating miniApp
### only WebAppInfo
- popup shows only on first time on all devices (iOs included)
- top right side of btn: "miniapp icon"
- WebAppInfo.url is the url given to BotFather when creating the miniApp

**NB**: start_param in initData is only available when miniapp is opened from "direct url" with `?startapp=`, otherwise if the URL is the straight App location any query parameter can be included - but they will not be available in start_param.
## SOLUTION:
use "direct url" with  `?startapp=` for sharing an url that opens the miniapp via telegram no matter where the link was opened from (eg. other apps like whatsapp - it always opens telegram and then the miniapp inside it, even if the user has no active chats with the bot);s
- the issue arises from the fact that "Web App URL" (included in WebAppInfo object for creating the InlineButton) IS NOT the "Direct URL" given by BotFather at time of creating the app, but the actual website that hosts it, and any queryParameter should be treated as normal (not inside initData.start_param)