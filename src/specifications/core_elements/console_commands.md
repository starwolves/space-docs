# Console Commands

It is possible to add new commands for clients to enter by just adjusting server-side values. Console commands are for a part server-side and modular.

To add new console commands add an if == "" check and must also register it the bottom data value of `src/space/core/console_commands/systems.rs`.

RCON status is handed out by default to all players in the current prototype, but in the future it is restricted to users who know a certain password and login with it through the console first. With brute force protection and all.


