# 💬🔕🔔 Topic Notifications

A plugin for Nagram XF. In a forum group, reading or replying in one topic clears that topic's notifications and nothing else. Every other topic keeps its own.

## Why I made it

I keep a forum group with a topic per project. In stock Nagram XF (and stock Telegram), opening one topic or sending a message in it wipes the notifications of every topic in that group. I lost track of messages that way. This plugin fixes it.

## What it changes

Forum groups only:

* Opening or reading a topic clears only that topic's notifications.
* Sending a message in a topic clears only that topic. Since 1.0.2 this also holds when the app reloads its notification list right after you send, which is what wiped the other topics in 1.0.1.
* Reading a topic on another device clears only that topic here too.
* Muting or clearing the whole group still clears everything, same as stock.

Private chats, normal groups and channels behave exactly as before. The shade refresh is silent, so no sound or vibration replays.

## Install

1. Use the plugin build of Nagram XF and turn on the plugin engine (Settings, N Settings, Plugins).
2. Download `topic_notifications.plugin` from the latest release.
3. Send it to your Saved Messages, tap it and choose install. You can also use the plus button on the Plugins screen.
4. Make sure Topic Notifications is switched on.

## How it works

Telegram reports reads for a forum group as one number for the whole group, and the notification controller drops every waiting message up to that number, whatever topic it came from. The plugin hooks `NotificationsController.processReadMessages`, `processDialogsUpdateRead` and `processLoadedUnreadMessages`, plus `MessagesController.markDialogAsRead`, keeps the highest read message per topic, and removes only the notifications whose own topic was read.

The app also rebuilds its notification list from the database now and then, and often right after you send. That rebuild skips the whole group while any topic of it is open, and the database already counts every topic as read. The plugin keeps a copy of the group's waiting notifications from just before the rebuild and puts back the ones whose topic you have not read, unless the group got muted. A full group clear (`removeNotificationsForDialog`, used by mute) passes through untouched.

## Tested

Nagram XF 12.10.1 plugin build, plugin SDK 1.4.6.7, on a Pixel 6 Pro. A bot posted into three topics of a test forum group. I opened the first topic from inside the app, sent a message there, and then let the notification list rebuild.

* Plugin off: the other two topics' notifications vanished the moment I sent.
* Version 1.0.1: they vanished too, on the rebuild that followed the send.
* Version 1.0.2: both stayed, through the send and through a forced rebuild.

## License

MIT
