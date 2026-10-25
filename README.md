# 💬🔕🔔 Topic Notifications

A plugin for Nagram XF. In a forum group, reading or replying in one topic clears that topic's notifications and nothing else. Every other topic keeps its own.

## Why I made it

I keep a forum group with a topic per project. In stock Nagram XF (and stock Telegram), opening one topic or sending a message in it wipes the notifications of every topic in that group. I lost track of messages that way. This plugin fixes it.

## What it changes

Forum groups only:

* Opening or reading a topic clears only that topic's notifications.
* Sending a message in a topic clears only that topic. This holds in groups that show topics as tabs too, which is where 1.0.2 still lost the other topics.
* New messages in other topics still notify while you sit in one topic. Stock stays silent for the whole group while any topic of it is open. Messages in the topic you are looking at stay silent.
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

Groups that show topics as tabs have one more trap. After a group wide read mark, the app raises every topic's own read mark in its database as well, so any check against the app's topic data says every topic was read. Since 1.0.3 the plugin only trusts reads that name a topic: opening it here, a topic read the server reports from another device, or sending into it.

For new messages, stock `processNewMessages` drops every message of a group while any chat of it is open on screen. The plugin hands messages from other topics to that method once more with the open group hidden for that one pass, so they notify like any other chat.

## Tested

Nagram XF 12.10.1 plugin build, plugin SDK 1.4.6.7, on a Pixel 6 Pro running the same build, settings and plugins as my daily phone. The test forum group used the tabs layout, like my real group, and a bot posted Read Aloud style replies into several topics while I sat in one topic and sent messages there.

* Version 1.0.2: sending in one topic wiped the notifications of the three other topics right away.
* Version 1.0.3: all of them stayed through the send and through the bot's reply in my topic. A reply that landed in another topic while I sat in mine showed up as a notification. Opening one other topic cleared only that topic.

Not covered by that test: the test group was muted on my account and unmuted on the test phone only, so my daily phone stayed quiet.

## License

MIT

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=firestarter1996/xf-topic-notifications&type=Date)](https://star-history.com/#firestarter1996/xf-topic-notifications&Date)
