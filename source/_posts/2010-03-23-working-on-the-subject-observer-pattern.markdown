--- 
layout: post
title: Working on the subject observer pattern
tags: 
- Coding
status: publish
type: post
published: true
meta: 
  _edit_last: "1"
---
Lately I have been writing a lot of game oriented C++ code. As with any form of development it's both a rewarding and frustrating process. I am relatively new to C++ as well so I am still grappling with the ins and outs of the language.

One of the things I have come to love about C++ is the flexibility. You can code C++ in virtually any style you want. It supports a higher level object oriented design but you can also get down and fiddle with bits if you need to. At the moment I'm interested in the high-level features of the language, the low-level features can wait.

One thing I have been working a lot on is the understanding of programming patterns in relation to C++. For the kind of work I am doing at the moment understanding programming patterns is almost as important as understanding the language itself. You can code in an ad-hoc way however it's when you start to introduce programming patterns when things start to make sense and become... dare I say fun. Just look at the impact Ruby on Rails had to the Web development world. In a sea of ad hoc development one framework comes along that implements one gigantic programming pattern and it's hailed as the second coming of Christ.

One of the most important programming patterns is the subject observer pattern. This is what I am detailing in the post. Subject observer fits in with model view controller. Basically a change to the model occurs, the model notifies the views on the subject and the pattern is born. In this particular example the subject is the model and the Observer are the views. It doesn't have to be particular to any other class and the example I give allows you to inherit from the subject observer classes and implement them in any way you want. One of the goals with this exercise was to keep the solution is generic as possible.

The subject observer pattern is powerful because it allows you to loosely couple your views and your models. Who doesn't love that. In C++ terms the model contains a vector pointers to its observers. The model is dumb in this sense. It doesn't know who its observers are and nor does it care. Observers can be added and removed from the model at any time as long as some sort of a array/vector/list of the observers is maintained. Note that this "model" solution does not include away to detach subjects from observers. It should not be too difficult to work this out but if you have problems ask me.

The following example has been tested with the GNU C++ compiler. In this case I am running it on a Macintosh. The code should be fairly generic and you should be run at different compilers. If you can't let me know.

In the code the horn class observes the alarm class. Thus making the alarm class the subject. Note how the horn and alarm class inherit from observer and subject respectively. The alarm gets triggered by the yes you guessed it triggered method. This it is an update to the model and as a result calls the notify observer method. This method which is inherited from subject loops through a vector of observer pointers calling the notify method on each observer. The notify method is a member of observer and inherited in the Horn class. The notify method gets passed a subject. This is so the notified object can extract information from the subject. While some solutions may not require the notified object to obtain any data from the subject the solution allows the Observer complete access to the subject.

Note in the horn class that there is a typecast. This typecast converts the subject pointer and alarm pointer allowing methods on the alarm to be called from the Horn class. In this case the Horn class fetches the ID of the alarm but it could be any kind of data.

I hope this explanation of the subject observer pattern helps. If you have any questions please post a reply to this comment. I will try to answer questions to the best of my knowledge bearing in mind that I am new to C++ myself.

Anyway time for code.

```cpp
#include <vector>
#include <typeinfo>
#include <string>

class Subject;

class Observer
{
public:
    virtual void notify(Subject* s) = 0;
    virtual ~Observer() {};
};

class Subject
{
    std::vector observers;
protected:
    void notify_observers()
    {
        std::vector::iterator iter;
        for (iter = observers.begin(); iter != observers.end(); ++iter)
            (*iter)->notify(this);
    }

public:
    virtual ~Subject() {};
    void register_observer(Observer* o)
    {
        observers.push_back(o);
    }
};

class Alarm : public Subject
{
public:
    Alarm()
    {
        std::cout << "alarm created" << "\n";
    }

    void triggerd()
    {
        std::cout << "The alarm has been triggerd" << "\n";
        notify_observers();
    }

    int const get_alarm_id(){ return 100; }
};

class Horn : public Observer
{
public:
    virtual void notify(Subject* s)
    {
        Alarm *a;
        a = dynamic_cast<Alarm*>(s);
        std::cout << a->get_alarm_id() << "\n";
    }
};

int main ()
{
    Alarm a = Alarm();
    Horn h = Horn();
    a.register_observer(&h);
    a.triggerd();
    return 0;
}
```
