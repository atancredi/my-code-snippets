Instructions
https://bakanim.xyz/posts/deploy-telegram-bot-to-fly-io/
- `flyctl apps create`
- `flyctl volumes create data --size=1`
- `flyctl secrets set BOT_TOKEN=`
- `flyctl secrets set BOT_USERNAME=`
- `flyctl secrets set BOT_WEBHOOK=YOUR-APP-NAME.fly.dev`
- `flyctl secrets set BOT_SECRET=`
- `flyctl deploy`

- deployment
	- how to host a telegram bot on fly.io (best practices etcetc)