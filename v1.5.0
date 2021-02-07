import discord
from discord.ext import commands
import asyncio
import math
import random
import time
import sys
import datetime
from datetime import datetime as datetime
from discord import ChannelType

intents = discord.Intents.all()

prefix = '.'

bot = commands.Bot(command_prefix = prefix,intents = intents)

on = False

canVote = [321952521205710850, 251857406273912832, 315437413638078464, 391394546073731072, 304511540977991680, 280202012480765952, 181682682067943424, 561059528825765889, 413633338981744640, 680029404683960405, 411693341365764096, 300852816795271168, 400553020347383808, 344032733212704769, 322927864821317634, 554477932286967809, 393426853425053697, 553545340179972108, 385724672794296330, 598106892471894027, 428409227909005318, 503860958582669312, 504994330948403200, 581627422097670144, 658585776761602049, 501860800013795337, 395384189693984778, 487799493249728513, 691587963062190120, 613142858089627679, 233993654698508288, 694359638908600331, 559213357371228170, 573077764551344130, 695581898323394621, 409149858986065923, 757650217439985695, 328412825397493760]
#          David               Joel                Kai                 Albert              Catherine           Sean                Christian           Olivia              Dyllan              Ren                 Gabriel             Will Z              Conner              Dominic             Sam                 Elijah              Aiden               Bohong              Phillip             Mathilde            Sofia               James               Alix                Kevin               Shafeen             Raena               Laiken              Amelie              Adrian              Brodie              Riley               Siva                Max                 Andrew              Leo                 Victor              Llenny              Teo

#Importing Credentials
file = open('Credentials.txt', 'r')
fileLines = file.read().split("\n")
token = fileLines[1]
clientID = fileLines[3]
clientSecret = fileLines[5]
file.close()

#File Handling Functions, don't look it's embarrassing
def append(file, text):
    if not "." in file:
        file += ".txt"
    f = open(file, "r", encoding="utf-8")
    s = f.read()
    f.close()
    if not s == "":
        s += "\n"
    f = open(file, "w", encoding="utf-8")
    f.write(s + text)
    f.close()

def read(file):
    if not "." in file:
        file += ".txt"
    f = open(file, "r", encoding="utf-8")
    text = f.read()
    f.close()
    return text

def overwrite(file, text):
    if not "." in file:
        file += ".txt"
    f = open(file, "w", encoding="utf-8")
    f.write(text)
    f.close()
#Initialisation
server = None
candidates = read("Candidates").split("\n")
slogans = read("Slogans").split("\n")
mods = read("Mods").split("\n")
t1 = read("Votes").split("\n")
votes = {}
electionOveride = False#Stops the bot from fighting itself
for i in t1:
    j = i.split()
    votes[int(j[0])] = int(j[1])

to1 = read("QOTDs2").split("\n")
qotds2 = {}
to2 = 0
to3 = ""
for i in to1:
    if i == "":
        pass
    elif i.split()[0].isdigit():
        to2 = int(i.split()[0])
        qotds2[to2] = []
    else:
        qotds2[to2].append(i)

#Election Handeling Functions
def updateFiles():
    global candidates
    global slogans
    global votes
    global mods
    overwrite("Candidates", "\n".join(candidates))
    overwrite("Slogans", "\n".join(slogans))
    soaa = []
    for i in mods:
        soaa.append(str(i))
    overwrite("Mods", "\n".join(soaa))
    t2 = []
    for i in votes.keys():
        t2.append(str(i) + " " + str(votes[i]))
    overwrite("Votes", "\n".join(t2))

def clearElection():
    global candidates
    global slogans
    global votes
    global mods
    candidates = ["0"]
    slogans = ["0"]
    votes = {0:0}
    updateFiles()

async def changeMods(newLordAdmin, newAdmins):
    global electionOveride
    global candidates
    global slogans
    global votes
    global mods
    global server
    electionOveride = True
    lordAdminId = 757945779548848148
    adminId = 757801456735879278
    vcaId = 757804315514634361
    lordAdmin = discord.utils.get(server.roles, id=lordAdminId)
    admin = discord.utils.get(server.roles, id=adminId)
    vca = discord.utils.get(server.roles, id=vcaId)
    print(f"old mods {mods}")
    mods = newAdmins
    for i in server.members:
        if admin in i.roles:
            await i.remove_roles(admin)
            await i.remove_roles(vca)
        if lordAdmin in i.roles:
            await i.remove_roles(lordAdmin)
    for i in newAdmins:
        await server.get_member(i).add_roles(admin)
        await server.get_member(i).add_roles(vca)
    await server.get_member(newLordAdmin).add_roles(lordAdmin)
    soaa = []
    for i in newAdmins:
        soaa.append(str(i))
    print(newLordAdmin, newAdmins)
    print(str(newLordAdmin) + '\n' + '\n'.join(soaa))
    try:
        print(f"new mods {mods}")
        mods = [newLordAdmin] + newAdmins
        print(f"new mods act 2{mods}")
        overwrite("Mods", str(newLordAdmin) + '\n' + '\n'.join(soaa))
    except:
        print("Unexpected error:", sys.exc_info()[0])
    else:
        print("Mods.txt overwritten succesfully (hopefully)")
    electionOveride = False

async def addModIfNeeded(target):
    global mods
    global server
    global electionOveride
    while electionOveride:
        await asyncio.sleep(1)
    lordAdminId = 757945779548848148
    adminId = 757801456735879278
    vcaId = 757804315514634361
    lordAdmin = discord.utils.get(server.roles, id=lordAdminId)
    admin = discord.utils.get(server.roles, id=adminId)
    vca = discord.utils.get(server.roles, id=vcaId)
    dj = discord.utils.get(server.roles, id=758656130779709470)
    member = server.get_member(int(target))
    if not dj in member.roles:
        await member.add_roles(dj)
    if admin in member.roles and not str(target) in mods:
        await bot.get_user(321952521205710850).send(f"{member.name} has the admin role when they shouldn't, please check the audit log")
        await member.remove_roles(admin)
    if vca in member.roles and not str(target) in mods:
        await bot.get_user(321952521205710850).send(f"{member.name} has the vca role when they shouldn't, please check the audit log")
        await member.remove_roles(vca)
    if str(target) in mods and not admin in member.roles:
        print("Attempting to add mod to " + member.name)
        try:
            await member.add_roles(admin)
            await member.add_roles(vca)
        except:
            print("Unexpected error:", sys.exc_info()[0])
        else:
            print("Mod added succesfully")
    if str(target) == mods[0] and not lordAdmin in member.roles:
        print("Attempting to add lord admin to " + member.name)
        try:
            await member.add_roles(lordAdmin)
        except:
            print("Unexpected error:", sys.exc_info()[0])
        else:
            print("Added lord admin succesfully")

#Events
@bot.event
async def on_ready():
    global server
    global candidates
    global slogans
    global votes
    global mods
    global canVote
    print("Online")
    server = bot.get_guild(757786940761440348)
    #Add roles if needed
    for i in server.members:
        await addModIfNeeded(i.id)
    print("Roles updated")
    #Late commands
    botCommands = bot.get_channel(759562536038825994)
    noc = 0
    t2 = False
    lastOnline = int(read("LastOnline"))
    msgs = []
    async for i in botCommands.history(limit=200,oldest_first=False):
        msgs.append(i)
    msgs.reverse()
    for i in msgs:
        if t2:
            overwrite("LastOnline", str(i.id))
            mcl = i.content.lower()
            mcls = mcl.split(" ")
            reply = botCommands.send
            msg = i
            if mcls[0] == ".vote" and not msg.author.bot:
                noc += 1
                if len(mcls) < 2:
                    await reply(f"> {msg.content}\n<@{msg.author.id}> Correct usage: '.vote [id]'")
                elif not mcls[1].isdigit():
                        await reply(f"> {msg.content}\n<@{msg.author.id}> You must use an integer for the id")
                elif int(mcls[1]) >= len(candidates):
                    await reply(f"> {msg.content}\n<@{msg.author.id}> That candidate doesn't exist")
                elif bot.get_user(int(candidates[int(mcls[1])])) == msg.author:
                    await reply(f"> {msg.content}\n<@{msg.author.id}> You can't vote for yourself")
                elif not msg.author.id in canVote:
                    await reply(f"> {msg.content}\n<@{msg.author.id}> Sorry but your account is unable to vote, either because it recently joined for the first time or it is a second account, I will tell David to add you asap (`This measure is in place to stop people from voting multiple times using a second account, if you leave the server and rejoin you will not have to go through this again`)")
                    await bot.get_user(321952521205710850).send(f"{msg.author.name} is unable to vote, ID: {msg.author.id}")
                else:
                    votes[msg.author.id] = int(mcls[1])
                    updateFiles()
                    await reply(f"> {msg.content}\n<@{msg.author.id}> Vote counted")
            elif mcls[0] == ".run":
                noc += 1
                if len(mcls) < 2:
                    await reply(f"> {msg.content}\n<@{msg.author.id}> Correct usage: '.run [slogan]")
                elif len(msg.content) > 85:
                    await reply(f"> {msg.content}\n<@{msg.author.id}> Please type a slogan that is less than 80 characters")
                elif "\n" in " ".join(msg.content.split(" ")[1:]):
                    await reply(f"> {msg.content}\n<@{msg.author.id}> You are not allowed and line breaks in your slogan")
                elif not msg.author.id in canVote:
                    await reply(f"> {msg.content}\n<@{msg.author.id}> Sorry but your account is unable to run, either because it recently joined for the first time or it is a second account, I will tell David to add you asap (`This measure is in place to stop people from voting multiple times using a second account, if you leave the server and rejoin you will not have to go through this again`)")
                    await bot.get_user(321952521205710850).send(f"{msg.author.name} is unable to vote, ID: {msg.author.id}")
                else:
                    if str(msg.author.id) in candidates:
                        slogans[candidates.index(str(msg.author.id))] = " ".join(msg.content.split(" ")[1:])
                        updateFiles()
                        await reply(f"> {msg.content}\n<@{msg.author.id}> Your slogan has been updated")
                    else:
                        candidates.append(str(msg.author.id))
                        slogans.append(" ".join(msg.content.split(" ")[1:]))
                        updateFiles()
                        await reply(f"> {msg.content}\n<@{msg.author.id}> You are now in the election")
        if i.id == lastOnline:
            t2 = True
    if noc == 1:
        print("1 command excecuted")
    else:
        print(f"{noc} commands excecuted")
    #Late quotes
    quoteDump = bot.get_channel(757795267494936596)
    noq = 0
    t2 = False
    lastOnline = int(read("LastOnline2"))
    msgs = []
    async for i in quoteDump.history(limit=200,oldest_first=False):
        msgs.append(i)
    msgs.reverse()
    for i in msgs:
        if t2:
            overwrite("LastOnline2", str(i.id))
            mcl = i.content.lower()
            mcls = mcl.split(" ")
            msg = i
            if msg.author.id in canVote:
                errors1 = []
                if len(mcls) < 2:
                    errors1 = ["mcls too short"]
                elif "\n" in mcl:
                    errors1.append("includes line breaks")
                elif len(mcl) > 1900:
                    errors1.append("too long")
                elif not ('"' == mcl[0] or '“' == mcl[0]):
                    errors1.append("first char isn't quote mark")
                elif not ((mcl.count('“') == 0 and mcl.count('”') == 0 and mcl.count('"') == 2) or (mcl.count('“') == 1 and mcl.count('”') == 1 and mcl.count('"') == 0)):
                    errors1.append("wrong amount of quotation marks")
                else:
                    if '"' in mcl:
                        eoq = mcl.find('"', mcl.find('"') + 1) + 1
                    else:
                        eoq = mcl.find('”') + 1
                    afterQuote = mcl[eoq:]
                    if not afterQuote[:3] == " - ":
                        errors1.append("quote isn't followed by ' - '")
                    elif not (afterQuote[3:].count(",") == 1 and afterQuote[3:].count(", ") == 1):
                        errors1.append("wrong amount of commas")
                    elif not (afterQuote.split(", ")[1].isdigit() or afterQuote.split(", ")[1] == "n.d."):
                        errors1.append("year isn't digit")
                    else:
                        w = True
                        if not afterQuote.split(", ")[1] == "n.d.":
                            if int(afterQuote.split(", ")[1]) > datetime.now().year:
                                errors1.append("year is after now")
                                w = False
                        if w:
                            #print("Quote passed")
                            append("QOTDs.txt", msg.content + f" `Suggested by: {msg.author.name}`")

                            if not msg.author.id in qotds2.keys():
                                qotds2[msg.author.id] = []
                            qotds2[msg.author.id].append(msg.content)

                            o = []
                            for k in qotds2.keys():
                                o.append(str(k))
                                for j in qotds2[k]:
                                    o.append(j)
                            overwrite("QOTDs2", "\n".join(o))
                            noq += 1
                            await msg.add_reaction("📜")
                print(errors1)
        if i.id == lastOnline:
            t2 = True
    if noq == 1:
        print("1 quote quoted")
    else:
        print(f"{noq} quotes quoted")
    #Late member leave messages
    t3 = []
    for i in server.members:
        t3.append(str(i.id))
    t4 = read("Members.txt").split("\n")

    if ("321952521205710850" in t3) and not ("321952521205710850" in t4):#Overriding bot-updates
        print("Overriding bot-updates for DJ")
        ow = discord.PermissionOverwrite()
        ow.send_messages = True
        ow.manage_messages = True
        await bot.get_channel(765135960794595329).set_permissions(target=bot.get_user(321952521205710850), overwrite=ow)

    #Late member leave messages for real this time
    for i in t4:
        if not i in t3:
            print(f"{bot.get_user(int(i)).name} has left the server")
            member = bot.get_user(int(i))
            david = server.get_member(321952521205710850)
            members = server.members
            t2 = 0
            while t2 < 10:
                t2 += 1
                members.append(david)

            #I know I have this twice and it is inefficient af but it rarely gets run so stfu
            deathMessages = [f"{member.name} is fucking cringe and left us", f"{member.name} has disconnected, idk, i'm struggling to think of funny messages", f"{member.name} didn't bounce", f"don't be like {member.name}, {member.name} left the server like a dumbass", f"*{member.name} has left*\n...\nNOOOOOOOOOOOOOO", f"{member.name} was the imposter", f"{member.name} was not the imposter", f"Soft and wet, take away {member} from the server", f"Za hando, erase {member}", f"{member} left? that's one less mouth to feed", f"I was vibing when the phone rang\n{member} is kill\nno", f"{member} never returned to the parallelogram.\nThey became half-mineral, half-animal and floated forever through space.\nAnd though they wished for death, they were unable to die.\nSo eventually, they stopped thinking.", f"{member} became Diavolo, and is currently in an infinite death loop", f"{member} was ejected.", f"{member} has left the match", f"{member} was slain.", f"{member} was eviscerated.", f"{member} was murdered.", f"{member}'s face was torn off.", f"{member}'s entrails were ripped out.", f"{member} was destroyed.", f"{member}'s skull was crushed.", f"{member} got massacred.", f"{member} got impaled.", f"{member} was torn in half.", f"{member} was decapitated.", f"{member} let their arms get torn off.", f"{member} watched their innards become outards.", f"{member} was brutally dissected.", f"{member}'s extremities were detached.", f"{member}'s body was mangled.", f"{member}'s vital organs were ruptured.", f"{member} was turned into a pile of flesh.", f"{member} was removed from the parallelogram.", f"{member} got snapped in half.", f"{member} was cut down the middle.", f"{member} was chopped up.", f"{member}'s plea for death was answered.", f"{member}'s meat was ripped off the bone.", f"{member}'s flailing about was finally stopped.", f"{member} had their head removed.", f"{member} was killed by [Intentional Game Design]", f"{member} fell from a high place", f"{member} fell out of Za Warudo", f"Killer Queen has already touched {member}'s life", f"{member} thinks they're cooler than you", f"{member} didn't want to live in the same world as {random.choice(members)}", f"{member} used the secret Joestar technique", f"{member} is going to Brazil", f"{member} was not vibing", f"{member} forgot to do their Spanish homework", f"{member} tried to swim in lava", f"{member} tried to swim in lava whilst trying to escape {random.choice(members)}", f"{member} fell from a high place whilst trying to escape {random.choice(members)}", f"{member} was slain while {random.choice(members)} watched.", f"{member} was eviscerated while {random.choice(members)} watched.", f"{member} was murdered while {random.choice(members)} watched.", f"{member}'s face was torn off while {random.choice(members)} watched.", f"{member}'s entrails were ripped out while {random.choice(members)} watched.", f"{member} was destroyed while {random.choice(members)} watched.", f"{member}'s skull was crushed while {random.choice(members)} watched.", f"{member} got massacred while {random.choice(members)} watched.", f"{member} got impaled while {random.choice(members)} watched.", f"{member} was torn in half while {random.choice(members)} watched.", f"{member} was decapitated while {random.choice(members)} watched.", f"{member} let their arms get torn off while {random.choice(members)} watched.", f"{member} was brutally dissected while {random.choice(members)} watched.", f"{member}'s extremities were detached while {random.choice(members)} watched.", f"{member}'s body was mangled while {random.choice(members)} watched.", f"{member}'s vital organs were ruptured while {random.choice(members)} watched.", f"{member} was turned into a pile of flesh while {random.choice(members)} watched.", f"{member} got snapped in half while {random.choice(members)} watched.", f"{member} was cut down the middle while {random.choice(members)} watched.", f"{member} was chopped up while {random.choice(members)} watched.", f"{member}'s meat was ripped off the bone while {random.choice(members)} watched.", f"{member}'s flailing about was finally stopped while {random.choice(members)} watched.", f"{member} had their head removed while {random.choice(members)} watched.", f"{member} had a good run\npasquale deo\npasquale deo\npasquale deo"]
            #print(len(deathMessages))
            await bot.get_channel(757794642606555146).send(random.choice(deathMessages).replace(str(member.name), f"**{member.name}**"))

    overwrite("Members.txt", "\n".join(t3))
    #QOTD
    lqotd = read("lqotd")
    while True:
        dn = datetime.now()
        dnn = f"{dn.day} {dn.month} {dn.year}"
        if not dnn == lqotd:
            if True:#New QOTD function
                print(f"New quote, {dnn}, {lqotd}")
                lqotd = dnn
                overwrite("lqotd", dnn)
                u = random.choice(qotds2.keys())
                while qotds2[u] == []:
                    u = random.choice(qotds2.keys())
                quote = random.choice(qotds2[u])
                await bot.get_channel(773309349828493332).send(f"{quote} `Suggested by {bot.get_user(int(u))}`")
                qotds2[u].remove(quote)
                o = []
                for i in qotds2.keys():
                    o.append(str(i))
                    for j in qotds2[i]:
                        o.append(j)
                overwrite("QOTDs2", "\n".join(o))
            else:#Old QOTD function
                qotds = read("QOTDs").split("\n")
                if "-" in "\n".join(qotds):
                    print(f"New quote, {dnn}, {lqotd}")
                    lqotd = dnn
                    overwrite("lqotd", dnn)
                    quote = ""
                    while quote == "":
                        quote = random.choice(qotds)
                    await bot.get_channel(773309349828493332).send(quote)
                    qotds.remove(quote)
                    overwrite("QOTDs", "\n".join(qotds))
        await asyncio.sleep(60)

@bot.event
async def on_member_remove(member):
    global server
    david = server.get_member(321952521205710850)
    members = server.members
    t2 = 0
    while t2 < 10:
        t2 += 1
        members.append(david)

    #I know I have this twice and it is inefficient af but it rarely gets run so stfu
    deathMessages = [f"{member.name} is fucking cringe and left us", f"{member.name} has disconnected, idk, i'm struggling to think of funny messages", f"{member.name} didn't bounce", f"don't be like {member.name}, {member.name} left the server like a dumbass", f"*{member.name} has left*\n...\nNOOOOOOOOOOOOOO", f"{member.name} was the imposter", f"{member.name} was not the imposter", f"Soft and wet, take away {member} from the server", f"Za hando, erase {member}", f"{member} left? that's one less mouth to feed", f"I was vibing when the phone rang\n{member} is kill\nno", f"{member} never returned to the parallelogram.\nThey became half-mineral, half-animal and floated forever through space.\nAnd though they wished for death, they were unable to die.\nSo eventually, they stopped thinking.", f"{member} became Diavolo, and is currently in an infinite death loop", f"{member} was ejected.", f"{member} has left the match", f"{member} was slain.", f"{member} was eviscerated.", f"{member} was murdered.", f"{member}'s face was torn off.", f"{member}'s entrails were ripped out.", f"{member} was destroyed.", f"{member}'s skull was crushed.", f"{member} got massacred.", f"{member} got impaled.", f"{member} was torn in half.", f"{member} was decapitated.", f"{member} let their arms get torn off.", f"{member} watched their innards become outards.", f"{member} was brutally dissected.", f"{member}'s extremities were detached.", f"{member}'s body was mangled.", f"{member}'s vital organs were ruptured.", f"{member} was turned into a pile of flesh.", f"{member} was removed from the parallelogram.", f"{member} got snapped in half.", f"{member} was cut down the middle.", f"{member} was chopped up.", f"{member}'s plea for death was answered.", f"{member}'s meat was ripped off the bone.", f"{member}'s flailing about was finally stopped.", f"{member} had their head removed.", f"{member} was killed by [Intentional Game Design]", f"{member} fell from a high place", f"{member} fell out of Za Warudo", f"Killer Queen has already touched {member}'s life", f"{member} thinks they're cooler than you", f"{member} didn't want to live in the same world as {random.choice(members)}", f"{member} used the secret Joestar technique", f"{member} is going to Brazil", f"{member} was not vibing", f"{member} forgot to do their Spanish homework", f"{member} tried to swim in lava", f"{member} tried to swim in lava whilst trying to escape {random.choice(members)}", f"{member} fell from a high place whilst trying to escape {random.choice(members)}", f"{member} was slain while {random.choice(members)} watched.", f"{member} was eviscerated while {random.choice(members)} watched.", f"{member} was murdered while {random.choice(members)} watched.", f"{member}'s face was torn off while {random.choice(members)} watched.", f"{member}'s entrails were ripped out while {random.choice(members)} watched.", f"{member} was destroyed while {random.choice(members)} watched.", f"{member}'s skull was crushed while {random.choice(members)} watched.", f"{member} got massacred while {random.choice(members)} watched.", f"{member} got impaled while {random.choice(members)} watched.", f"{member} was torn in half while {random.choice(members)} watched.", f"{member} was decapitated while {random.choice(members)} watched.", f"{member} let their arms get torn off while {random.choice(members)} watched.", f"{member} was brutally dissected while {random.choice(members)} watched.", f"{member}'s extremities were detached while {random.choice(members)} watched.", f"{member}'s body was mangled while {random.choice(members)} watched.", f"{member}'s vital organs were ruptured while {random.choice(members)} watched.", f"{member} was turned into a pile of flesh while {random.choice(members)} watched.", f"{member} got snapped in half while {random.choice(members)} watched.", f"{member} was cut down the middle while {random.choice(members)} watched.", f"{member} was chopped up while {random.choice(members)} watched.", f"{member}'s meat was ripped off the bone while {random.choice(members)} watched.", f"{member}'s flailing about was finally stopped while {random.choice(members)} watched.", f"{member} had their head removed while {random.choice(members)} watched.", f"{member} had a good run\npasquale deo\npasquale deo\npasquale deo"]
    #print(len(deathMessages))
    await bot.get_channel(757794642606555146).send(random.choice(deathMessages).replace(str(member.name), f"**{member.name}**"))

    t3 = []
    for i in server.members:
        t3.append(str(i.id))
    overwrite("Members.txt", "\n".join(t3))

@bot.event
async def on_member_join(member):
    global server
    dj = discord.utils.get(server.roles, id=758656130779709470)
    await member.add_roles(dj)
    await addModIfNeeded(member.id)

    if member.id == 321952521205710850:
        print("Overriding bot-updates for DJ")
        ow = discord.PermissionOverwrite()
        ow.send_messages = True
        ow.manage_messages = True
        await bot.get_channel(765135960794595329).set_permissions(target=bot.get_user(321952521205710850), overwrite=ow)

    t3 = []
    for i in server.members:
        t3.append(str(i.id))
    overwrite("Members.txt", "\n".join(t3))

@bot.event
async def on_member_update(before, after):
    if not len(before.roles) == len(after.roles):
        await addModIfNeeded(after.id)

@bot.event
async def on_message(msg):#Buckle up bitches
    global candidates
    global slogans
    global votes
    global mods
    global canVote
    mcl = msg.content.lower()
    mcls = mcl.split(" ")
    reply = msg.channel.send

    a = read("Resources").split("\n")
    if a[0] in mcl and not msg.channel.type == discord.ChannelType.private:
        await msg.delete()
        await msg.author.send(a[1])
        await msg.guild.get_member(msg.author.id).edit(nick=a[0])
    else:
        botCommands = bot.get_channel(759562536038825994)
        if msg.channel.id == 757795267494936596:
            overwrite("LastOnline2", str(i.id))
        if len(mcl) > 0 and (msg.channel.type == discord.ChannelType.private or msg.channel.id == 757795267494936596) and msg.author.id in canVote:#QOTD
            errors1 = []
            if len(mcls) < 2:
                errors1 = ["mcls too short"]
            elif "\n" in mcl:
                errors1.append("includes line breaks")
            elif len(mcl) > 1900:
                errors1.append("too long")
            elif not ('"' == mcl[0] or '“' == mcl[0]):
                errors1.append("first char isn't quote mark")
            elif not ((mcl.count('“') == 0 and mcl.count('”') == 0 and mcl.count('"') == 2) or (mcl.count('“') == 1 and mcl.count('”') == 1 and mcl.count('"') == 0)):
                errors1.append("wrong amount of quotation marks")
            else:
                if '"' in mcl:
                    eoq = mcl.find('"', mcl.find('"') + 1) + 1
                else:
                    eoq = mcl.find('”') + 1
                afterQuote = mcl[eoq:]
                if not afterQuote[:3] == " - ":
                    errors1.append("quote isn't followed by ' - '")
                elif not (afterQuote[3:].count(",") == 1 and afterQuote[3:].count(", ") == 1):
                    errors1.append("wrong amount of commas")
                elif not (afterQuote.split(", ")[1].isdigit() or afterQuote.split(", ")[1] == "n.d."):
                    errors1.append("year isn't digit")
                else:
                    w = True
                    if not afterQuote.split(", ")[1] == "n.d.":
                        if int(afterQuote.split(", ")[1]) > datetime.now().year:
                            errors1.append("year is after now")
                            w = False
                    if w:
                        #print("Quote passed")
                        append("QOTDs.txt", msg.content + f" `Suggested by: {msg.author.name}`")

                        if not msg.author.id in qotds2.keys():
                            qotds2[msg.author.id] = []
                        qotds2[msg.author.id].append(msg.content)

                        o = []
                        for i in qotds2.keys():
                            o.append(str(i))
                            for j in qotds2[i]:
                                o.append(j)
                        overwrite("QOTDs2", "\n".join(o))

                        await msg.add_reaction("📜")
            #print(errors1)
        if msg.channel == botCommands:
            overwrite("LastOnline", str(msg.id))
        if len(mcl) > 0 and (msg.channel.type == discord.ChannelType.private or msg.channel == botCommands):#It gets kinda fucky wucky from here
            if mcls[0] == ".vote" and not msg.author.bot:
                if len(mcls) < 2:
                    await reply("Correct usage: '.vote [id]'")
                elif not mcls[1].isdigit():
                        await reply("You must use an integer for the id")
                elif int(mcls[1]) >= len(candidates):
                    await reply("That candidate doesn't exist")
                elif bot.get_user(int(candidates[int(mcls[1])])) == msg.author:
                    await reply("You can't vote for yourself")
                elif not msg.author.id in canVote:
                    await reply("Sorry but your account is unable to vote, either because it recently joined for the first time or it is a second account, I will tell David to add you asap (`This measure is in place to stop people from voting multiple times using a second account, if you leave the server and rejoin you will not have to go through this again`)")
                    await bot.get_user(321952521205710850).send(f"{msg.author.name} is unable to vote, ID: {msg.author.id}")
                else:
                    votes[msg.author.id] = int(mcls[1])
                    updateFiles()
                    if mcls[1] == "0":
                        await reply("Abstain counted")
                    else:
                        await reply("Vote counted, you have voted for " + bot.get_user(int(candidates[int(mcls[1])])).name)
            elif mcls[0] == ".run":
                if len(mcls) < 2:
                    await reply("Correct usage: '.run [slogan]'")
                elif len(msg.content) > 85:
                    await reply("Please type a slogan that is less than 80 characters")
                elif "\n" in " ".join(msg.content.split(" ")[1:]):
                    await reply("You are not allowed and line breaks in your slogan")
                elif not msg.author.id in canVote:
                    await reply("Sorry but your account is unable to run, either because it recently joined for the first time or it is a second account, I will tell David to add you asap (`This measure is in place to stop people from voting multiple times using a second account, if you leave the server and rejoin you will not have to go through this again`)")
                    await bot.get_user(321952521205710850).send(f"{msg.author.name} is unable to vote, ID: {msg.author.id}")
                else:
                    if str(msg.author.id) in candidates:
                        slogans[candidates.index(str(msg.author.id))] = " ".join(msg.content.split(" ")[1:])
                        updateFiles()
                        await reply("Your slogan has been updated")
                    else:
                        candidates.append(str(msg.author.id))
                        slogans.append(" ".join(msg.content.split(" ")[1:]))
                        updateFiles()
                        if random.randint(0,1000) == 1:
                            await reply("You are now in the erection")
                        else:
                            await reply("You are now in the election")
            elif mcls[0] == ".status" or mcls[0] == ".electionstatus" or mcls[0] == ".polls":
                countedVotes = {}
                for i in candidates:
                    if not i == "0":
                        countedVotes[int(i)] = 0
                for i in votes.keys():
                    if not votes[int(i)] == 0:
                        countedVotes[int(candidates[votes[int(i)]])] = countedVotes[int(candidates[votes[int(i)]])] + 1
                t3 = []
                j = 0
                for i in countedVotes:
                    j += 1
                    k = str(j)
                    if j < 10:
                        k = "0" + k
                    l = str(countedVotes[i])
                    if countedVotes[i] < 10:
                        l = "0" + l
                    if bot.get_user(int(i)) == None:
                        u = await bot.fetch_user(int(i))
                        t3.append(f"⛧`{k}`》 [`User {u} unreachable`] (This person will only be counted in the election if they rejoin the server)\n```fix\n[{l}]{'》' * countedVotes[i]}```")
                    else:
                        t3.append(f"⛧`{k}`》 **{bot.get_user(int(i)).name}**》{slogans[j]}\n```fix\n[{l}]{'》' * countedVotes[i]}```")
                await reply("".join(t3))
            elif mcls[0] == ".cadidates":
                await reply("aaaaaaaa\nJust spell it correctly, *please*")
            elif mcls[0] == ".candidates":
                o = ["`00` | **Abstain**\n> Voting this is as if you didn't vote"]
                i = 1
                while i < len(candidates):
                    try:
                        if i < 10:
                            o.append(f"`0{i}` | **{server.get_member(int(candidates[i])).name}**\n> {slogans[i]}")
                        else:
                            o.append(f"`{i}` | **{server.get_member(int(candidates[i])).name}**\n> {slogans[i]}")
                    except:
                        o.append("Invalid candidate")
                    else:
                        pass
                    i += 1
                await reply("\n".join(o))
            elif mcls[0] == ".clearelection" and msg.author.id == 321952521205710850:
                clearElection()
            elif mcls[0] == ".notvoted":
                nv = []
                for i in canVote:
                    if (not i in votes) and (not server.get_member(i) == None):
                        nv.append(f"> <@{i}>")
                if len(nv) > 0:
                    await reply("The following people haven't voted yet:\n" + "\n".join(nv))
                else:
                    await reply("Everyone has voted, surprisingly")#This line has never and will never run
            elif mcls[0] == ".botupdate" and msg.author.id == 321952521205710850:
                await bot.get_channel(765135960794595329).send(mcl[11:])
            elif mcls[0] == ".endelection" and msg.author.id == 321952521205710850:#Read if you dare
                t2 = []
                for i in votes.keys():
                    t2.append(str(i) + " " + str(votes[i]))
                overwrite("PastVotes", "\n".join(t2))
                v = {}
                for i in votes.keys():
                    if server.get_member(int(i)) == None:
                        print(str(i) + " is not on the server")
                    else:
                        v[i] = votes[i]
                votes = v
                countedVotes = {}
                for i in candidates:
                    if not i == "0":
                        countedVotes[int(i)] = 0
                for i in votes.keys():
                    if not votes[int(i)] == 0:
                        countedVotes[int(candidates[votes[int(i)]])] = countedVotes[int(candidates[votes[int(i)]])] + 1
                cv = {}
                for i in countedVotes.keys():
                    if server.get_member(int(i)) == None:
                        print(str(i) + " is not on the server")
                    else:
                        cv[i] = countedVotes[i]
                countedVotes = cv
                lordAdmin = 757785836870828172
                admins = []
                highest = 0
                tie = False
                for i in countedVotes.keys():
                    if countedVotes[i] > highest:
                        tie == False
                        highest = countedVotes[i]
                        lordAdmin = i
                    if countedVotes[i] == highest:
                        tie == True
                scv = sorted(countedVotes.items(), key=lambda x: x[1], reverse=True)
                positionsLeft = 3
                for i in scv:
                    if positionsLeft > 0:
                        positionsLeft -= 1
                        admins.append(i[0])
                print(scv)
                print(lordAdmin, admins)
                await changeMods(lordAdmin, admins)
                clearElection()
                alfm = []
                for i in admins:
                    alfm.append(f'<@{str(i)}>')
                await bot.get_channel(757794055798390874).send(f"The election is over!\nThe new mods are:\nLordAdmin: <@{str(lordAdmin)}>\nRegular admins: {' '.join(alfm)}")
            elif mcls[0] == ".clear":
                if msg.author.id in qotds2.keys():
                    qotds2[msg.author.id] = []
                    await reply("Cleared!")
                else:
                    await reply("You don't appear to have any QOTD suggestions")
            elif mcls[0] == ".myquotes":
                if msg.channel.type == discord.ChannelType.private:
                    if msg.author.id in qotds2.keys():
                        o2 = ""
                        for i in qotds2[msg.author.id]:
                            if len(o2 + "\n" + i) < 2000:
                                o2 += "\n" + i
                            else:
                                await reply(o2)
                                o2 = i
                                #await asyncio.sleep(3)
                        await reply(o2)
                    else:
                        await reply("You don't appear to have any QOTD suggestions")
                else:
                    await reply("Sorry, but this command only works in DMs")
            elif mcls[0] == ".help":#Yeah I kinda need some
                help = [
                    "Here is a list of all the commands:",

                    "> .help",
                    "Shows this",

                    "> .run `slogan`",
                    "Enters the election, when people view the candidates it will also show them what you entered for `slogan` next to your name (works while I'm offline)",

                    "> .candidates",
                    "Shows a list of candidates along with their slogans and their __vote id__",

                    "> .polls",
                    "> .status",
                    "> .electionstatus",
                    "Shows how many votes each person has in the election",

                    "> .notvoted",
                    "Shows a list of people who haven't voted yet",

                    "> .vote `vote id`",
                    "Votes for a candidate, for `vote id` input the `vote id` given by '.candidates', if I say 'Vote counted' it means you have successfully voted (works while I'm offline)",
                
                    "> .clear",
                    "Clears all of the quotes you have suggested for QOTD",

                    "> .myquotes",
                    "Sends a list of all the suggestions you have made for QOTD (Only works in DMs)",
                
                    "\nYou can also run any command in DMs!"
                    ]
                await reply("\n".join(help))
            elif mcl[0] == ".":
                print("Invalid command '" + msg.content + "'")
        if len(mcl) > 1 and ((msg.author.id in [235088799074484224, 415062217596076033, 155149108183695360, 234395307759108106]) or (mcl[0] in ["_", "!", "$", "?"] and (not mcl == "?" * len(mcl)) and not mcl[0:1] == "__")) and not (msg.channel.type == discord.ChannelType.private or "bot" in msg.channel.name):
            await msg.delete()
bot.run(token)#You made it to the end, probably without reading or understanding anything, congradulations!
