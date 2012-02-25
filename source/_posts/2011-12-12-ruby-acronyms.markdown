--- 
layout: post
title: Ruby Acronyms
tags: 
- acronym
- array
- Coding
- core_ext
- faker
- rails
- Rambling
- ruby
status: publish
type: post
published: true
meta: 
  aktt_notify_twitter: "no"
  _aktt_hash_meta: ""
  _edit_last: "1"
---
I love extending the ruby core! Makes code so neat and works so well with rails initializers. This method allows you to fetch the Acronym of an array of strings. It would be kinda cool to write something that works the other way taking a single string. Make using <a href="http://rubygems.org/gems/faker" title="http://rubygems.org/gems/faker">Faker</a>... but I can't think of any use. Then again IBM could not understand why people needed computers in their home so who knows. Anyway... it's code time.

```ruby
class Array
  def acronym
    result = ""
    self.each do |item|
      result << item[0]
    end
    result
  end
end
```
