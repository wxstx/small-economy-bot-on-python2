# small-discord-bot-on-python
import discord
from discord.ext import commands
from discord.utils import get
from config import *
import json
import sys
import discord, random
import random

TOKEN = "your token here"
PREFIX = "!"
WALLET_DEFAULT = {"balance": 0}
client = commands.Bot(command_prefix=PREFIX, intents=discord.Intents.all())

bot = commands.Bot(command_prefix='!', intents=discord.Intents.all())


@client.command()
async def ping(ctx):
  await ctx.send("pong!")
  await ctx.send(ctx.author.name)


async def get_user_wallet(user_id):
  user_id = str(user_id)

  with open("wallets.json", "r") as file:
    users_wallets = json.load(file)

  if user_id not in users_wallets.keys():
    users_wallets[user_id] = WALLET_DEFAULT

  with open("wallets.json", "w") as file:
    json.dump(users_wallets, file)

  return users_wallets[user_id]


async def set_user_wallet(user_id, parameter, new_value):
  user_id = str(user_id)

  with open("wallets.json", "r") as file:
    users_wallets = json.load(file)

  if user_id not in users_wallets.keys():
    users_wallets[user_id] = WALLET_DEFAULT

  users_wallets[user_id][parameter] = new_value

  with open("wallets.json", "w") as file:
    json.dump(users_wallets, file)


@client.command()
async def balance(ctx):
  user_wallet = await get_user_wallet(ctx.author.id)
  await ctx.send(f"Ваш баланс: {user_wallet['balance']}")


@client.command()
async def change_balance(ctx, user_mention=None, amount=None):
  if not ctx.author.guild_permissions.administrator:
    await ctx.send("**Недостаточно прав!**")
    return

  if user_mention is None or amount is None:
    await ctx.send(f"**Вы не указали тот или иной параметр!**")

  user = get(ctx.guild.members, id=int(user_mention[2:-1]))
  user_wallet = await get_user_wallet(user.id)
  user_wallet["balance"] += int(amount)
  await set_user_wallet(user.id, "balance", user_wallet['balance'])

  await ctx.send(f"Баланс {user.name} был успешно изменён!\n" \
                 f"**Баланс {user.name}**: {user_wallet['balance']}")


@client.event
async def on_ready():
  print("Бот запустился!")


@client.command()
async def kill(ctx):
  if not ctx.author.guild_permissions.administrator:
    await ctx.send("**Недостаточно прав!**")
    return
  sys.exit()


@client.command()
async def randomh(ctx):
  randomk = ['Орёл', 'Решка']
  await ctx.send(f'Выпало: {random.choice(randomk)}')


@client.command()
async def hlp(ctx):
  await ctx.send(
      'Привет! Это бот для фарма виртуальной валюты! Ты можешь выиграть виртуальную валюту и обменять её на деньги или же предметы в игре и тд, с ценами можно ознакомиться тут - https://discord.com/channels/1169560374979342406/1169711498919170158 ! Виртуальную валюту можно выиграть пока что только в розыгрышах. У бота есть такие команды как: !hlp - помощь, !balance - проверка своего баланса, !randomh - игра в орёл или решка! За покупкой обращаться в лс к owner!'
  )


client.run(token=TOKEN)

