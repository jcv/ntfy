# Known issues
This is an incomplete list of known issues with the ntfy server, web app, Android app, and iOS app. You can find a complete
list [on GitHub](https://github.com/binwiederhier/ntfy/labels/%F0%9F%AA%B2%20bug), but I thought it may be helpful
to have the prominent ones here to link to.

## iOS app not refreshing (see [#337](https://github.com/binwiederhier/ntfy/issues/337))
For some users, the iOS app does not refresh the view when new notifications come in. Until you manually
swipe down, you do not see the newly arrived messages, even though the popup appeared before.

The [iOS improvement plan](https://github.com/binwiederhier/ntfy/issues/1680) lists this as probably fixed. iOS app
v1.4.1 included a refresh fix for it, and v1.5.1 made background polling more reliable and polls subscriptions
whenever the topic list appears. If you still see this on the latest version, please comment on [#337](https://github.com/binwiederhier/ntfy/issues/337).

## iOS app not receiving notifications (anymore)
If notifications do not show up at all anymore, first update to the latest version of the iOS app. Several polling
and delivery fixes shipped in v1.5.1 and later. Beyond that, there are a few causes for it (that I know of):

**Firebase+APNS are being weird and buggy**:    
If this is the case, usually it helps to **remove the topic/subscription and re-add it**. That will force Firebase to 
re-subscribe to the Firebase topic.

**Self-hosted only: No `upstream-base-url` set, or `base-url` mismatch**:   
To make self-hosted servers work with the iOS
app, I had to do some horrible things (see [iOS instant notifications](config.md#ios-instant-notifications) for details).
Be sure that in your selfhosted server:

* Set `upstream-base-url: "https://ntfy.sh"` (**not your own hostname!**)
* Ensure that the URL you set in `base-url` **matches exactly** what you set the Default Server in iOS to 

## iOS app seeing "New message", but not real message content
If you see `New message` notifications on iOS, your iPhone can likely not talk to your self-hosted server. Be sure that
your iOS device and your ntfy server are either on the same network, or that your phone can actually reach the server.

Turn on tracing/debugging on the server (via `log-level: trace` or `log-level: debug`, see [troubleshooting](troubleshooting.md)),
and read docs on [iOS instant notifications](https://docs.ntfy.sh/config/#ios-instant-notifications).

## Safari does not play sounds for web push notifications
Safari does not support playing sounds for web push notifications, and treats them all as silent. This will be fixed with
iOS 17 / Safari 17, which will be released later in 2023.
