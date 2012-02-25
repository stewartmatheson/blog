--- 
layout: post
title: Rounding time to the closest hour in Ruby
tags: 
- class
- Coding
- core_ext
- extend
- hour
- Rambling
- round
- ruby
- seconds
- time
- time.new
- time.now
status: publish
type: post
published: true
meta: 
  _aktt_hash_meta: ""
  aktt_notify_twitter: "no"
  _edit_last: "1"
---
Ahhh... its one of the moments in life that make you so happy your a ruby developer.

```ruby
class Time
  def round(seconds = 60)
    Time.at((self.to_f / seconds).round * seconds)
  end

  def floor(seconds = 60)
    Time.at((self.to_f / seconds).floor * seconds)
  end

  def round_to_closest_hour
    if self.min > 30
      self.round(1.hour)
    else
      self.floor(1.hour)
    end
  end
end
```

Source:
[http://stackoverflow.com/questions/449271/how-to-round-a-time-down-to-the-nearest-15-minutes-in-ruby](http://stackoverflow.com/questions/449271/how-to-round-a-time-down-to-the-nearest-15-minutes-in-ruby)
