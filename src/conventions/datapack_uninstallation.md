# Datapack Uninstallation (`*`)

## About

This convention ensures that you have a way of uninstalling *most* of the contents of your datapack. The word "most" is used here because Minecraft does not provide the necessary tools for us to actually uninstall or remove the entirety of the datapack.

## Implementation

There are no rules enforcing how you should implement the uninstallation method, as long as you do implement it. However, the following section outlines one of the many ways to implement it.

### Uninstallation Function

You can create an "uninstallation function", which when ran, will remove most of the content added from your datapack. You can tell the user about this function inside your project page or anywhere you like.

--------------------

### **Note from Convention Mod (Optional)**
> `scoreboard`, `storage`, or `team`, should be removed. 
> in a situation that you cannot remove them, you should includes the reason in the [request review](https://github.com/mc-datapacks/review-tracker/issues) or mcfunction comment.


###  **What you should be aware of about disabling the datapack**

**1. Use /datapack disable \<name>** - `tick.json`
> Pros
> - Only use two `datapack disable` commands. This will also disable the schedule command.
> - Automatic reload without `/reload`. **Note:** `/reload` command doesn't work with some 
server launchers.
>
> Cons
> - the datapack name is not reliable, the user can rename the datapack file to whatever they want.

**2. Use /schedule clear \<function>** - `load.json only`
### **How to use**
Typically you will use `load.json` to call mcfunctions like load, init, etc. to install scoreboard objectives, etc. as you need. which these files will only run once

But if you have to use the same functionality as `tick.json`, you simply use `load.json` to call your main or tick function and use the `schedule function convention:datapack/main 1t` on the last line.

**Fun Fact :** If you are a beginner, you may not know that you can make mcfunction run slower tick cycles as you like.
For example, run every 8 tick `schedule function <function> 8t` or every 1 second `schedule function <function> 20t or 1s` `t - tick`, `s - second`, `d - gameday`. Why am I talking about this? Because I've seen scoreboards used to do this before.

> Pros
> - Don't worry about the name 
> - No need to disable datapack.
>
> Cons
> - If not disabled, You must complete all schedule clear commands.
> - There must be no reload after all schedule clear until the datapack file is removed.

**3. Installation type**

`.zip`
> When uninstalling, if the user deletes the datapack file, they must exit game or stop server. when uninstalling 
>
> Pros
> - Just drag it into the datapack folder. 

`extract to folder`
> No need to exit the game or stop the server. when uninstalling
>
> Cons
> - You need to be sure that the user will extract the folder correctly. Including when you compress it into a .zip file, how do you make it?
> - `<name>.zip/<name>/data`
If it is like this, the user must click `Extract here`.
> - `<name>.zip/data`
If it is like this, the user must click `Extract to "<name>\"`

**4. Various reload methods**


 > 4.1 `/datapack disable <name>`
 >
 > 4.2 `/reload` (There is an issue with some server launchers.)
 >
 > 4.3 `Exit and Re-join game or Stop & Re-launch Server`

**5. Conclusion from convention mod's opinion.**


**Note:** The `tick.json` file typically runs before `load.json`, sometimes resulting in an incomplete installation. You can avoid that by making the init file to run before the tick function.
```json
{
    "values": [
        "convention:datapack/setup",
        "convention:datapack/main"
    ]
}

If it was me, what method would I use? 
> - Use `/schedule clear <function>` 
> - Use `.zip` (Just drag & drop, good experience for the users)
> 
> Because you don't have to worry about the name when disabling the datapack. And installing the `.zip` file will force the user to *exit the game or stop the server (reload method 3.3)* to delete the datapack file. Because the datapack is not disabled.


> - If schedule function command is more than 2
> - Use `Use /datapack disable <static_name>` (convention.zip every update)
> - Use `.zip` (Just drag & drop, good experience for the users)
> - If you only want to use `datapack disable .zip`, you must tell the user not to extract the files.
> 
> Because we want the `setup` or `init` file to run first.
Which we'll only use `datapack disable "file/convention.zip"` to have everything disabled in one command, with prohibits users from extracting files.
And installing `.zip` will be able to force the user to *exit the game or stop the server (reload method 3.3)* to delete the datapack file. 