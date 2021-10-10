import asyncio
import datetime
import random

import discord
from discord import Intents
from discord import user
from discord.ext import commands
from discord.ext import tasks
from discord_components import *
from discord_components import DiscordComponents

client = discord.Client()
intents = discord.Intents.default()
intents.members = True
client = discord.Client(intents=intents)

intents = Intents.default()
intents.members = True

client = commands.Bot(command_prefix="+", intents=intents)


### Events
@client.event
async def on_ready():
    print('Bot Is Ready!')
    print('BOLBOL')
    print('Vertox#9999')
    client.my_current_task = live_status.start()
    DiscordComponents(client)
    servers = len(client.guilds)
    members = 0
    for guild in client.guilds:
        members += guild.member_count - 1


## Live Status
@tasks.loop()
async def live_status(seconds=75):
    Dis = client.get_guild(config.guildID)  # Int

    activity = discord.Activity(type=discord.ActivityType.watching, name=f'+info to get info')
    await client.change_presence(activity=activity)
    await asyncio.sleep(15)

    activity = discord.Activity(type=discord.ActivityType.watching, name=f'👥 {Dis.member_count} Members')
    await client.change_presence(activity=activity)
    await asyncio.sleep(10)


## DiscordID Lookup
@client.command(aliases=['discord', 'did', 'whois'])
@commands.has_permissions(manage_messages=True)
async def look(ctx, disid: int = None):
    await ctx.message.delete()

    if not disid:
        await ctx.send('<@{}>, Please Specify A DiscordID!'.format(ctx.message.author.id))
        return
    try:
        obj = await client.fetch_user(disid)
        if not obj:
            await ctx.send('User `{}` Not Found!'.format(disid))
        else:
            dembed = discord.Embed(title='User Details', descrption='API Returted Values :',
                                   color=discord.Color((0x0007b)))
            dembed.add_field(name='Discord Username :', value=obj)
            dembed.add_field(name='DiscordID :', value=obj.id)
            dembed.set_image(url=obj.avatar_url)
            await ctx.send(embed=dembed)
    except Exception as err:
        print(err)


## Config
class config:
    guildID = PUT HERE YOUR GUILD ID 
    Token = "PUT HERE YOUR BOT TOKEN"


#######################################################
##welcome
#######################################################
@client.event
async def on_member_join(member):
    print(f"Member joined! {member.name}")


@client.event
async def on_member_remove(member):
    print(f"Member leaved! {member.name}")


####links blocker
@client.event
async def on_message(message):
    if 'https://' in message.content:
        await message.delete()
        await message.channel.send(f"{message.author.mention} Don't send links!")
    else:
        if 'discord.gg' in message.content:
            await message.delete()
            await message.channel.send(f"{message.author.mention} Don't send links!")
            channel = client.get_channel(##LOGS CHANNEL ID)
            embed = discord.Embed(title=f"Message deleted",
                                  description=f"{message.author.mention} has sent a link : {message.content} ",
                                  color=discord.Colour.random())
            now = datetime.datetime.now().strftime("%H:%M")
            embed.set_footer(text=f"Today at | {now}")
            await channel.send(embed=embed)
    if '@everyone' in message.content:
        await message.author.send(f"{message.author.mention} Do not try to mention our members ok?")
        await message.author.kick()
        await message.delete()
        channel = client.get_channel(##LOGS CHANNEL ID)
        embed = discord.Embed(title=f"Member kicked",
                              description=f"{message.author.mention} has been kicked for: `mention @everyone`",
                              color=discord.Colour.random())
        now = datetime.datetime.now().strftime("%H:%M")
        embed.set_footer(text=f"Today at | {now}")
        await channel.send(embed=embed)
    if '@here' in message.content:
        await message.author.send(f"{message.author.mention} Do not try to mention our members ok?")
        await message.author.kick()
        await message.delete()
        channel = client.get_channel(##LOGS CHANNEL ID)
        embed = discord.Embed(title=f"Member kicked",
                              description=f"{message.author.mention} has been kicked for: `mention @here`",
                              color=discord.Colour.random())
        now = datetime.datetime.now().strftime("%H:%M")
        embed.set_footer(text=f"Today at | {now}")
        await channel.send(embed=embed)
    if 'buy' in message.content:
        await message.author.send(f"{message.author.mention} If you want to buy it open a ticket or type +prices")
    else:
        await client.process_commands(message)


#######################################################
# kick command
#######################################################
@client.command()
async def kick(ctx, member: discord.Member, *, reason=None):
    guild = ctx.guild
    if (not ctx.author.guild_permissions.administrator):
        await ctx.send(f"{ctx.author.mention} You do not have permission to use this command!")
        return
    await member.kick(reason=reason)
    await ctx.send(f"{member.name} has been kicked from the server!")
    channel5 = client.get_channel(##LOGS CHANNEL ID)
    embed2 = discord.Embed(title=f"Member kicked",
                           description=f"{ctx.author.mention} kicked {member.mention} from the server",
                           color=discord.Colour.random())
    embed2.add_field(name="Reason", value=reason, inline=False)
    now = datetime.datetime.now().strftime("%H:%M")
    embed2.set_footer(icon_url=ctx.author.avatar_url,
                      text=f"Kicked by {ctx.author.name}#" + ctx.author.discriminator + f'' + f" Today at | {now}")
    await channel5.send(embed=embed2)


#######################################################
# clear command
#######################################################
@client.command()
@commands.has_permissions(manage_messages=True)
async def clear(ctx, *, amount=10):
    guild = ctx.guild
    if (not ctx.author.guild_permissions.administrator):
        await ctx.send(f"{ctx.author.mention} You don't have permission to use this command!")
    await ctx.channel.purge(limit=amount)
    await ctx.send(f'`{amount} messages has been deleted`')
    channel = client.get_channel(##LOGS CHANNEL ID)
    embed2 = discord.Embed(title=f"Messages cleard", description=f"{ctx.author.mention} Cleard messages",
                           color=discord.Colour.random())
    embed2.add_field(name="Cleard messages", value=amount, inline=False)
    embed2.add_field(name="Channel", value=channel.mention, inline=False)
    await channel.send(embed=embed2)


#######################################################
##mute
#######################################################
@client.command()
@commands.has_permissions(manage_roles=True)
async def mute(ctx, member: discord.Member, time, *, reason=None):
    guild = ctx.guild
    if (not ctx.author.guild_permissions.administrator):
        await ctx.send(f"{ctx.author.mention} You don't have permission to use this command!")

    for role in guild.roles:
        if role.name == f"Your mute role":
            await member.add_roles(role)
    for role in guild.roles:
        if role.name == f"Members role":
            await member.remove_roles(role)
            await ctx.send(f"{member.mention} has been muted for {time} for: {reason}")
            await ctx.message.delete()
            channel = client.get_channel(##LOGS CHANNEL ID)
            embed2 = discord.Embed(title=f"Member Muted", description=f"The muted user: {member.mention}",
                                   color=discord.Colour.random())
            embed2.add_field(name="Time", value=time, inline=False)
            embed2.add_field(name="Reason", value=reason, inline=False)
            now = datetime.datetime.now().strftime("%H:%M")
            embed2.set_footer(icon_url=ctx.author.avatar_url,
                              text=f"Muted by {ctx.author.name}#" + ctx.author.discriminator + f'' + f" Today at | {now}")
            await channel.send(embed=embed2)
            d = time[-1]

            num = int(time[:-1])

            if d == "s":
                await asyncio.sleep(num)

            if d == "m":
                await asyncio.sleep(num * 60)

            if d == "h":
                await asyncio.sleep(num * 60 * 60)

            if d == "d":
                await asyncio.sleep(num * 60 * 60 * 24)
            await member.remove_roles(role)
            channel2 = client.get_channel(##LOGS CHANNEL ID)
            embed = discord.Embed(title="Member Unmuted", description=f"Unmuted {member.mention}",
                                  color=discord.Colour.random())
            await channel2.send(embed=embed)


#######################################################
###rmute command
@client.command()
@commands.has_permissions(manage_roles=True)
async def rmute(ctx, member: discord.Member, time, *, reason=None):
    guild = ctx.guild
    if (not ctx.author.guild_permissions.administrator):
        await ctx.send(f"{ctx.author.mention} You don't have permission to use this command!")

    for role in guild.roles:
        if role.name == f"Your mute role":
            await member.add_roles(role)
    for role in guild.roles:
        if role.name == f"Members role":
            await member.remove_roles(role)
            await ctx.send(f"{member.mention} has been muted for {time} for: {reason}")
            await ctx.message.delete()
            channel = client.get_channel(##LOGS CHANNEL ID)
            embed2 = discord.Embed(title=f"Member Muted", description=f"The muted user: {member.mention}",
                                   color=discord.Colour.random())
            embed2.add_field(name="Time", value=time, inline=False)
            embed2.add_field(name="Reason", value=reason, inline=False)
            now = datetime.datetime.now().strftime("%H:%M")
            embed2.set_footer(icon_url=ctx.author.avatar_url,
                              text=f"Muted by {ctx.author.name}#" + ctx.author.discriminator + f'' + f" Today at | {now}")
            await channel.send(embed=embed2)
            d = time[-1]

            num = int(time[:-1])

            if d == "s":
                await asyncio.sleep(num)

            if d == "m":
                await asyncio.sleep(num * 60)

            if d == "h":
                await asyncio.sleep(num * 60 * 60)

            if d == "d":
                await asyncio.sleep(num * 60 * 60 * 24)
            await member.add_roles(role)
        for role in guild.roles:
            if role.name == f"Your mute role":
                await member.remove_roles(role)
                channel2 = client.get_channel(##LOGS CHANNEL ID)
                embed = discord.Embed(title="Member Unmuted", description=f"Unmuted {member.mention}",
                                      color=discord.Colour.random())
                await channel2.send(embed=embed)



##unmute
@client.command()
@commands.has_permissions(manage_roles=True)
async def unmute(ctx, member: discord.Member, *, reason=None):
    if ctx.author.guild_permissions.mute_members:
        guild = ctx.guild

        for role in guild.roles:
            if role.name == f"Your mute role":
                await member.remove_roles(role)
        for role in guild.roles:
            if role.name == f"Members role":
                await member.add_roles(role)
                await ctx.send(f"{member.mention} was unmuted")
                await ctx.message.delete()
                channel = client.get_channel(##LOGS CHANNEL ID)
                embed2 = discord.Embed(title=f"Member Unmuted", description=f"{member.mention} was unmuted",
                                       color=discord.Colour.random())
                embed2.add_field(name="Reason", value=reason, inline=False)
                now = datetime.datetime.now().strftime("%H:%M")
                embed2.set_footer(icon_url=ctx.author.avatar_url,
                                  text=f"Unmuted by {ctx.author.name}#" + ctx.author.discriminator + f'' + f" Today at | {now}")
                await channel.send(embed=embed2)




#######################################################
##ban
#######################################################
@client.command()
@commands.has_permissions(ban_members=True)
async def ban(ctx, member: discord.Member, time, *, reason=None):
    if ctx.author.guild_permissions.ban_members:
        guild = ctx.guild
        await guild.ban(member)
        await ctx.send(f"{member.mention} has been banned from the server!")
        await ctx.message.delete()
        channel = client.get_channel(##LOGS CHANNEL ID)
        embed2 = discord.Embed(title=f"Member Banned", description=f"The banned user: {member.name}",
                               color=discord.Colour.random())
        embed2.add_field(name="Time", value=time, inline=False)
        embed2.add_field(name="Reason", value=reason, inline=False)
        now = datetime.datetime.now().strftime("%H:%M")
        embed2.set_footer(icon_url=ctx.author.avatar_url,
                          text=f"Banned by {ctx.author.name}#" + ctx.author.discriminator + f'' + f" Today at | {now}")
        await channel.send(embed=embed2)
        d = time[-1]

        num = int(time[:-1])

        if d == "s":
            await asyncio.sleep(num)

        if d == "m":
            await asyncio.sleep(num * 60)

        if d == "h":
            await asyncio.sleep(num * 60 * 60)

        if d == "d":
            await asyncio.sleep(num * 60 * 60 * 24)
        await member.unban()
        channel2 = client.get_channel(##LOGS CHANNEL ID)
        embed = discord.Embed(title="Member Unbanned", description=f"{member.mention} was unbanned",
                              color=discord.Colour.random())
        embed.set_footer(text=f" Today at | {now}")
        await channel2.send(embed=embed)


####unban
@client.command()
async def unban(ctx, member: discord.Member):
    await member.unban()
    channel2 = client.get_channel(##LOGS CHANNEL ID)
    embed2 = discord.Embed(title="Member Unbanned", description=f"{member.mention} was unbanned",
                           color=discord.Colour.random())
    now = datetime.datetime.now().strftime("%H:%M")
    embed2.set_footer(text=f" Today at | {now}")
    await channel2.send(embed=embed2)




#######################################################
##jail
#######################################################
@client.command()
@commands.has_permissions(administrator=True)
async def jail(ctx, member: discord.Member, time, *, reason=None):
    guild = ctx.guild
    for role in guild.roles:
        if role.name == f"Jailed":
            await member.add_roles(role)
        if role.name == f".":
            await member.remove_roles(role)
            await ctx.message.delete()
            c = await guild.create_text_channel(name=f"{member.name} Cell ⛓️")
            embed3 = discord.Embed(title=f"Message!",
                                   description=f"{member.mention} Hey GAY, uou have been arrested for `{reason}`\n You have to wait until you will be free\n Or you can try to escape.",
                                   color=discord.Color((0)))
            embed3.set_footer(text=f"You have {time} until you will be free")
            embed3.set_thumbnail(
                url="https://cdn.discordapp.com/attachments/578554610483200010/880929485002768384/138897-xzzlszrwux-1585139103.png")
            await c.send(embed=embed3)
            channel = client.get_channel(##LOGS CHANNEL ID)
            embed2 = discord.Embed(title=f"Member Arrested",
                                   description=f"The user {member.mention} has been jailed for",
                                   color=discord.Colour.random())
            embed2.add_field(name="Time", value=time, inline=False)
            embed2.add_field(name="Reason", value=reason, inline=False)
            now = datetime.datetime.now().strftime("%H:%M")
            embed2.set_footer(icon_url=ctx.author.avatar_url,
                              text=f"Jailed by {ctx.author.name}#" + ctx.author.discriminator + f'' + f" Today at | {now}")
            await channel.send(embed=embed2)
            d = time[-1]

            num = int(time[:-1])

            if d == "s":
                await asyncio.sleep(num)

            if d == "m":
                await asyncio.sleep(num * 60)

            if d == "h":
                await asyncio.sleep(num * 60 * 60)

            if d == "d":
                await asyncio.sleep(num * 60 * 60 * 24)
            channel2 = client.get_channel(##LOGS CHANNEL ID)
            embed = discord.Embed(title="Member Unjailed", description=f"Unjailed person {member.mention}",
                                  color=discord.Colour.random())
            now = datetime.datetime.now().strftime("%H:%M")
            embed.set_footer(text=f"{member.mention} was unjaild Today at | {now}")
            await channel2.send(embed=embed)
    guild = ctx.guild
    for role in guild.roles:
        if role.name == f".":
            await member.add_roles(role)
        if role.name == f"Jailed":
            await member.remove_roles(role)
            await c.delete()


##########################
##release
##########################
@client.command()
@commands.has_permissions(manage_roles=True)
async def release(ctx, member: discord.Member, *, reason=None):
    if ctx.author.guild_permissions.mute_members:
        guild = ctx.guild

        for role in guild.roles:
            if role.name == f"Jailed":
                await member.remove_roles(role)
                await ctx.send(f"{member.mention} was unjailed")
                await ctx.message.delete()
                channel = client.get_channel(##LOGS CHANNEL ID)
                embed2 = discord.Embed(title=f"Member unjailed", description=f"{member.mention} was unjailed",
                                       color=discord.Colour.random())
                embed2.add_field(name="Reason", value=reason, inline=False)
                now = datetime.datetime.now().strftime("%H:%M")
                embed2.set_footer(icon_url=ctx.author.avatar_url,
                                  text=f"Unjailed by {ctx.author.name}#" + ctx.author.discriminator + f'' + f" Today at | {now}")
                await channel.send(embed=embed2)

        for role in guild.roles:
            if role.name == f".":
                await member.add_roles(role)


###delete message log
@client.event
async def on_message_delete(message):
    embed = discord.Embed(title=f"Message deleted", description=f"{message.author.mention} Type `{message.content}`",
                          color=discord.Colour.random())
    now = datetime.datetime.now().strftime("%H:%M")
    embed.set_footer(icon_url=message.author.avatar_url,
                     text=f"Sended by {message.author.name}#" + message.author.discriminator + f'' + f" Today at | {now}")
    channel = client.get_channel(##LOGS CHANNEL ID)
    await channel.send(embed=embed)


##sug
@client.command()
async def sug(ctx, *, reason=None):
    embed = discord.Embed(title=f"**__New suggestion__**", description=f"**{reason}**", color=discord.Color((0x0007b)))
    embed.set_footer(icon_url=ctx.author.avatar_url,
                     text=f"New suggest from | {ctx.author.name}#" + ctx.author.discriminator + f'')
    channel = client.get_channel(##SUGGESTION CHANNEL ID)
    msg = await channel.send(embed=embed)
    await ctx.message.delete()

    await msg.add_reaction("✅")
    await msg.add_reaction("❌")

    channel2 = client.get_channel(##LOGS CHANNEL ID)
    embed2 = discord.Embed(title=f"New Suggestion", description=f"{ctx.author.mention} suggest new suggestion",
                           color=discord.Color((0x0007b)))
    embed2.add_field(name="Suggestion", value=reason, inline=False)
    embed2.add_field(name="Channel", value=channel.mention, inline=False)
    await channel2.send(embed=embed2)


##verify
@client.command()
async def verify(ctx):
    guild = ctx.guild
    verifyRole = discord.utils.get(ctx.guild.roles, name="Citizen")
    await ctx.author.add_roles(verifyRole)
    await ctx.message.delete()


##verify2
@client.command()
async def VERIFY(ctx):
    guild = ctx.guild
    verifyRole = discord.utils.get(ctx.guild.roles, name="Citizen")
    await ctx.author.add_roles(verifyRole)
    await ctx.message.delete()


##verify3
@client.command()
async def Verify(ctx):
    guild = ctx.guild
    verifyRole = discord.utils.get(ctx.guild.roles, name="Citizen")
    await ctx.author.add_roles(verifyRole)
    await ctx.message.delete()


###verifymessage
@client.command()
@commands.has_permissions(administrator=True)
async def ve(ctx):
    embed = discord.Embed(title=f"Verify Message",
                          description=f" Before verifying yourself go read <#881237857203810415>\nType `!verify` to get `Citizen` role",
                          color=discord.Color((0x3498db)))
    await ctx.send(embed=embed)


###tickets, not works:(
@client.command()
@commands.has_permissions(administrator=True)
async def ticket(ctx, reason=None):
    guild = ctx.guild
    await ctx.message.delete()
    t = await guild.create_text_channel(catagory=f"TICKETS LIST", name=f"{ctx.author.name} ticket {reason}")
    embed = discord.Embed(title=f"Kraken ticket system",
                          description=f"Hey {ctx.author.mention}, This is your ticket\n Please type here what you need and our staff will help you soon as they can.\n `Note` that if you will not answer in 10m the ticket will closed\n **Click `TWICE` the lock button to close the ticket**",
                          color=discord.Color((0x3498db)))
    embed.set_thumbnail(
        url="https://cdn.discordapp.com/avatars/881503148865359903/8c054405114afe3f5872c4c4f79b5d9e.webp?size=1024")
    now = datetime.datetime.now().strftime("%H:%M")
    embed.set_footer(text=f"Today at | {now}")
    msg = await t.send(embed=embed)
    await msg.add_reaction("🔒")
    user == client.user
    await client.wait_for('reaction_add', check=lambda reaction, member: reaction.emoji == '🔒')
    embed2 = discord.Embed(title=f"Kraken ticket system", description=f"Ticket closed", color=discord.Color((0x0007b)))
    embed2.set_thumbnail(
        url="https://cdn.discordapp.com/avatars/881503148865359903/8c054405114afe3f5872c4c4f79b5d9e.webp?size=1024")
    now = datetime.datetime.now().strftime("%H:%M")
    embed2.set_footer(text=f"Today at | {now}")
    await msg.add_reaction("🔒")
    await client.wait_for('reaction_add', check=lambda reaction, member: reaction.emoji == '🔒')
    await msg.delete()
    await t.send(embed=embed2)


# membercount command
@client.command(aliases=["mc"])
async def members(ctx):
    a = ctx.guild.member_count
    b = discord.Embed(title=f"Members in {ctx.guild.name}", description=a, color=discord.Colour.random())
    now = datetime.datetime.now().strftime("%H:%M")
    b.set_footer(text=f"Today at | {now}")
    await ctx.send(embed=b)




##download


###nuke channel command
@client.command()
@commands.has_permissions(administrator=True)
async def nukech(ctx, *, amount=200000000000):
    guild = ctx.guild
    await ctx.channel.purge(limit=amount)
    embed = discord.Embed(title=f"Nuked Successfully!", description=f" Cleard {amount}",
                          color=discord.Color((0xe74c3c)))
    embed.set_image(url="https://cdn.discordapp.com/attachments/888340677132439552/888885244235759616/giphy.gif")
    now = datetime.datetime.now().strftime("%H:%M")
    embed.set_footer(text=f"Today at | {now}")
    await ctx.send(embed=embed)
    channel = client.get_channel(##LOGS CHANNEL ID)
    embed2 = discord.Embed(title=f"Channel nuked", description=f"{ctx.author.mention} Cleard messages",
                           color=discord.Colour.random())
    embed2.add_field(name="Cleard messages", value=amount, inline=False)
    embed2.add_field(name="Channel", value=channel.mention, inline=False)
    await channel.send(embed=embed2)


###say command
@client.command()
@commands.has_permissions(administrator=True)
async def say(ctx, *, msg):
    await ctx.message.delete()
    await ctx.send(f"{msg}")
    channel = client.get_channel(##LOGS CHANNEL ID)
    embed = discord.Embed(title=f"`Say` command was used",
                          description=f"{ctx.author.mention} Requsted me to say **{msg}**",
                          color=discord.Colour.random())
    now = datetime.datetime.now().strftime("%H:%M")
    embed.set_footer(text=f"Today at | {now}")
    await channel.send(embed=embed)




###warn command
@client.command()
@commands.has_permissions(administrator=True)
async def warn(ctx, member: discord.Member, reason=None):
    guild = ctx.guild
    await ctx.message.delete()
    await ctx.send(f"{member.mention} has been warned for {reason}")
    channel = client.get_channel(##LOGS CHANNEL ID)
    embed = discord.Embed(title=f"Member warned",
                          description=f"{member.mention} was warned by {ctx.author.mention} in `{guild.name}`",
                          color=discord.Colour.random())
    now = datetime.datetime.now().strftime("%H:%M")
    embed.set_footer(text=f"Today at | {now}")
    await channel.send(embed=embed)


@client.command()
@commands.has_permissions(administrator=True)
async def g(ctx, time=None, *, prize=None):
    if time == None:
        return await ctx.author.send(f"שים טיים יא זונה")
    elif prize == None:
        return await ctx.author.send(f"What prize?")

    embed = discord.Embed(title=f"🎉 Giveaway!", description=f"{ctx.author.mention} is giving away!",
                          color=discord.Color((0x0007b)))
    time_convert = {"s": 1, "m": 60, "h": 3600, "d": 86400}
    gawtime = int(time[:-1]) * time_convert[time[-1]]
    now = datetime.datetime.now().strftime("%H:%M")
    embed.set_footer(text=f"Giveaway ends in {time} from now | {now} ")
    embed.add_field(name=f"Prize", value=f"**{prize}**", inline=True)
    embed.add_field(name=f"Time", value=f"**{time}**", inline=False)
    gaw_msg = await ctx.send(embed=embed)

    await gaw_msg.add_reaction("🎉")
    await asyncio.sleep(gawtime)

    new_gaw_msg = await ctx.channel.fetch_message(gaw_msg.id)

    users = await new_gaw_msg.reactions[0].users().flatten()
    users.pop(users.index(client.user))

    winner = random.choice(users)

    await ctx.send(f"{winner.mention} won **{prize}**")


client.run('PUT HERE YOUR TOKEN')
