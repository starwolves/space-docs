# Console Commands

It is possible to add new commands for clients to enter by just adjusting server-side values. Console commands are for a part server-side and modular.

Please search for existing commands ie "spawnEntity" and see and learn how we can listen to commands in a modular way. 

RCON status is handed out by default to all players in the current prototype, but in the future it is restricted to users who know a certain password and login with it through the console first. With brute force protection and all.

Trigger console commands by writing to the `InputConsoleCommand` event.

First register the new console command and name by adding a `initialize_console_commands` startup system that adds to the resource `ConsoleCommands.list`.

Then listen to incoming console command events from systems by matching `InputConsoleCommand.command_name` with your command name.

