**Help** 
If you need help, please contact me on discord Nemijah#6392

**Setup** 
First, you need to require it obv.

```javascript
const xp = require('hive-xp');
```
Then you need to set the url to your mongoose database: 

```javascript
xp.setURL('mongodb url')
```

**Example** 
*I'm assuming you have setted up the module as introduced in 'Setup' area.*

**Level up code example** 

```javascript
if(message.author.bot) return;
const randomXp = Math.floor(Math.random() * 20);
const hasLeveledUp = await Levels.appendXp(message.author.id, message.guild.id, randomXp);
if (hasLeveledUp) {
    const user = await Levels.fetch(message.author.id, message.guild.id);
    message.channel.send(`You leveled up to ${user.level}! Keep it going!`);
}
```

**Rank card command example** 

*For the rank card I'll be using canvacord* 

```javascript
const member = message.mentions.members.last() || message.guild.members.cache.get(args[0]) || message.member;
const user = await Levels.fetch(member.id, message.guild.id);
const neededXp = Levels.xpFor(parseInt(user.level) + 1);

if(!user) return message.reply("You don't have any xp try sending some messages");

const rank = new canvacord.Rank()
    .setAvatar(member.user.displayAvatarURL({ dynamic: false, format: 'png' }))
    .setCurrentXP(user.xp)
    .setRequiredXP(neededXp)
    .setStatus(member.user.presence.status)
    .setLevel(user.level)
    .setRank(2, "idj", false)
    .setProgressBar("#FFA500", "COLOR")
    .setUsername(member.user.username)
    .setDiscriminator(member.user.discriminator);
    
    
    rank.build()
        .then(data => {
            const attachment = new MessageAttachment(data, "RankCard.png");
            message.channel.send(attachment);
        });
```


**Admin functions**

**createUser**
Creates an entry in database for that user if it doesnt exist.

```javascript
Levels.createUser(<UserID - String>, <GuildID - String>);
```

**deleteUser**
If the entry exists, it deletes it from database.

```javascript
Levels.deleteUser(<UserID - String>, <GuildID - String>);
```

**appendXp**
It adds a specified amount of xp to the current amount of xp for that user, in that guild. It re-calculates the level. It creates a new user with that amount of xp, if there is no entry for that user.

```javascript
Levels.appendXp(<UserID - String>, <GuildID - String>, <Amount - Integer>);
```

**appendLevel**
It adds a specified amount of levels to current amount, re-calculates and sets the xp reqired to reach the new amount of levels.

```javascript
Levels.appendLevel(<UserID - String>, <GuildID - String>, <Amount - Integer>);
```


**setXp**

It sets the xp to a specified amount and re-calculates the level.

```javascript
Levels.setXp(<UserID - String>, <GuildID - String>, <Amount - Integer>);
```


**setLevel**

Calculates the xp required to reach a specified level and updates it.

```javascript
Levels.setLevel(<UserID - String>, <GuildID - String>, <Amount - Integer>);
```


**subtractXp**
It removes a specified amount of xp to the current amount of xp for that user, in that guild. It re-calculates the level.

```javascript
Levels.appendXp(<UserID - String>, <GuildID - String>, <Amount - Integer>);
```

**subtractLevel**

It removes a specified amount of levels to current amount, re-calculates and sets the xp reqired to reach the new amount of levels.

```js
Levels.appendLevel(<UserID - String>, <GuildID - String>, <Amount - Number>);
```