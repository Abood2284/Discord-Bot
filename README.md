# Discord-Bot

I created a Discord Bot for my SaaS application.

Purpose:
🏷️ Notify me when i new user joins my SaaS
💰 When a user pays for my subscription
👀 and more...

This way i could get instant notification to what's happening, and enjoy the dopamine hits ❤️

### Create a Application in Discord Developer Portal

head over to [Discord Developer Portal](https://discord.com/developers/docs/intro)

Then, Applications > New Application

Once you create it, srcoll to the bottom you will see a URL, open that

Then head over to `Bots` section from the sidebar

Scroll Down till you see Bot Permissions

Select `Bot` & `Send Messages` 

Then if you haven't copied the Token, you can always get a new one from `Bots` section

### Get the USER ID From Discord
First setup your account as Developer account

Gear icon > advanced > TUrn on Developer Mode

Then tap on your profile > copy User ID



### The Code Part
Create a new file wherever you want and paste the following code
`discordBot.ts`
```
import { REST } from "@discordjs/rest"
import {
  RESTPostAPIChannelMessageResult,
  RESTPostAPICurrentUserCreateDMChannelResult,
  Routes,
  APIEmbed,
} from "discord-api-types/v10"

export class DiscordClient {
  private rest: REST

  constructor(token: string | undefined) {
    this.rest = new REST({ version: "10" }).setToken(token ?? "")
  }

  async createDM(
    userId: string
  ): Promise<RESTPostAPICurrentUserCreateDMChannelResult> {
    return this.rest.post(Routes.userChannels(), {
      body: { recipient_id: userId },
    }) as Promise<RESTPostAPICurrentUserCreateDMChannelResult>
  }

  async sendEmbed(
    channelId: string,
    embed: APIEmbed
  ): Promise<RESTPostAPIChannelMessageResult> {
    return this.rest.post(Routes.channelMessages(channelId), {
      body: { embeds: [embed] },
    }) as Promise<RESTPostAPIChannelMessageResult>
  }
}
```


Then whenever you want to use this
i.e: a api route `api/discord/route.ts`

```
import { DiscordClient } from "@/app/lib/discordBot"


export const POST = async () => { 
     const discord = new DiscordClient(process.env.DISCORD_BOT_TOKEN)

    const dmChannel = await discord.createDM('process.env.USER_ID')

    await discord.sendEmbed(
          dmChannel.id,
            {
                title: "👤 New User",
                color: 0x00ff00,
                fields: [{
                    name: "email",
                    value: "example@gmail.com"
                }]
    })

    return new Response("Message sent")
}
```


Voila🎉







