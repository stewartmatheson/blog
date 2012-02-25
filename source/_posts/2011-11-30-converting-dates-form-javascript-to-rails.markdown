--- 
layout: post
title: Converting dates form Javascript to Rails
tags: 
- Coding
status: publish
type: post
published: true
meta: 
  aktt_notify_twitter: "no"
  _aktt_hash_meta: ""
  _edit_last: "1"
---
Time based web applications written in rails and javascript will sooner or later have to think about the following object life cycle.

1.  Javascript Object
2.  Request Params
3.  Rails object
4.  MySQL row

While rails has the last two covered and jQuery has mostly the second covered how do we get our data in to a form where this flow will be smooth. Javascript date objects don't directly fit so I extended the date prototype to allow for this. This porototype mirrors the rails date time form helper and creates the correct data with the keys rails would expect to have.

```javascript
  Date.prototype.toRails = function(name) {
    var dateParams;
    dateParams = {};
    dateParams[name + "(1i)"] = this.getFullYear();
    dateParams[name + "(2i)"] = this.getMonth() + 1;
    dateParams[name + "(3i)"] = this.getDate();
    dateParams[name + "(4i)"] = this.getHours();
    dateParams[name + "(5i)"] = this.getMinutes();
    return dateParams;
  };

  //Use

  d = new Date();
  date = d.toRails('start');

  //now we can put this object on the server
  $.ajax
      ...
      data : date
      ...
```

As far as rails is concerned we might as well be using the datetime helper.
