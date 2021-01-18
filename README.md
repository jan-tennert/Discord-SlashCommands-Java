# Java-SlashCommands

# ! You need to enable Raw Events on you JDABuilder or the listeners won't work !

# Installation

#### Maven

```xml
<repositories>
     <repository>
	   <id>jitpack.io</id>
	   <url>https://jitpack.io</url>
     </repository>
</repositories
```
```xml
<dependency>
      <groupId>com.github.jan-tennert</groupId>
      <artifactId>Java-SlashCommands</artifactId>
      <version>1.0</version>
</dependency>
```

# Requirements

- latest Discord.JDA version

# Demo (Guild Commands)

```java
JDA jda = JDABuilder.createDefault("//Token")
                .setRawEventsEnabled(true) //Without this, slash commands won't work!
                .build();
SlashCommandBuilder builder = new SlashCommandBuilder(jda, 123456789, "token");
SlashCommandGuild guild = builder.getGuildCommandsFor(123456789);//Get guild commands with ID
guild.registerGuildCommand(new SlashCommand("test", "This is a test command!")); //Register a slash command

//To delete a command you have to iterate through guild.commands and find your command. Then just run guild.deleteGuildCommand(command.id). 
```       




![The Command](https://cdn.discordapp.com/attachments/775406836877885504/800702840720982037/unknown.png)

#### Now we can create a listener for the slash commands:

```java
public static class Listener extends SlashCommandListener {
        @Override
        public void run(@NotNull Member sender, @NotNull TextChannel channel, @NotNull SlashCommand command, @NotNull ArrayList<SlashCommandArgument> args) {
            if(command.getName().equals("test")) { //check if the slash command is our "test"
                channel.sendMessage("You entered the slash command: test").queue(); //Then just send a message
        }
    }
}
```



#### Now we need to add the listener and build the builder:

```java
builder.addListener(new Listener()); //Add the listener
builder.build(); //Build the builder so the listeners are ready
```


You can also add arguments to your command:

```java
 guild.registerGuildCommand(new SlashCommand("test", "This is a test command!",
                new SlashCommandOption("number", "Enter a number", true, SlashCommandOptionType.INTEGER))); //Register a slash command.

//Then in your listener:
channel.sendMessage("You entered the number: " + args.get(0).getValue()).queue();
```

![Command with arguments](https://cdn.discordapp.com/attachments/775406836877885504/800706541971046400/unknown.png)
