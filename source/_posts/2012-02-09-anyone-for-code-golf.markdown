--- 
layout: post
title: Anyone for code golf?
tags: []

status: publish
type: post
published: true
meta: 
  _aktt_hash_meta: ""
  aktt_notify_twitter: "no"
  _edit_last: "1"
---

It's been ages since I have posted anything. So busy with work. I just wrote this little snippet then and liked it. Was wondering if I could make it any shorter.

```ruby
def shorten_tweet(tweet)
  tweet.length > 140 ? "#{tweet[0..136]}..." : tweet
end
```

