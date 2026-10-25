# 💬🔕🔔 Topic Notifications

A plugin for Nagram XF. In a forum group, reading or replying in one topic clears that topic's notifications and nothing else. Every other topic keeps its own.

## Why I made it

I keep a forum group with a topic per project. In stock Nagram XF (and stock Telegram), opening one topic or sending a message in it wipes the notifications of every topic in that group. I lost track of messages that way. This plugin fixes it.

## What it changes

Forum groups only:

* Opening or reading a topic clears only that topic's notifications.
* Sending a message in a topic clears only that topic.
* Reading a topic on another device clears only that topic here too.
* Muting or clearing the whole group still clears everything, same as stock.

Private chats, normal groups and channels behave exactly as before. The shade refresh is silent, so no sound or vibration replays.

## Install

1. Use the plugin build of Nagram XF and turn on the plugin engine (Settings, N Settings, Plugins).
2. Download `topic_notifications.plugin` from the latest release.
3. Send it to your Saved Messages, tap it and choose install. You can also use the plus button on the Plugins screen.
4. Make sure Topic Notifications is switched on.

## How it works

Telegram reports reads for a forum group as one number for the whole group, and the notification controller drops every waiting message up to that number, whatever topic it came from. The plugin hooks `NotificationsController.processReadMessages`, `processDialogsUpdateRead` and `processLoadedUnreadMessages`, plus `MessagesController.markDialogAsRead`, keeps the highest read message per topic, and removes only the notifications whose own topic was read. A full group clear (`removeNotificationsForDialog`, used by mute) passes through untouched.

## Tested

Nagram XF 12.10.1 plugin build, plugin SDK 1.4.6.7, on a Pixel 6 Pro. A bot posted into two topics of a test forum group. With the plugin on, I opened one topic and replied, and the other topic's notification stayed. With the plugin off, both were cleared.

## License

MIT
