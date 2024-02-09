# Risk Management UI design

## The problem

The old safetyfolder offers many options to manage safety. It is however hard to navigate the site. The purpose is to get your safety measure in order and to keep them in order, but there are many pages you have to visit and it is not clear where to find them. The users do not actively search for pages to update. They are not intrinsically interested in the tool. They use it because they have to. So, the question is how can we design the website so that this becomes easier, it becomes more of a logical flow.

## Some thoughts on possible solutions

### The problem with star rewards
In an earlier version of the site stars were used to motivate users to do things. This is a kind of gamification. A problem with this system is that users would get to the point where they got the maximum three stars and did not update the site after that. What we would want is that users keep the site at a certain level.

The star rewards were some kind of final reward for an action. Instead of getting a reward for the action itself, the 'reward' should be for the general state of the system. So, we should do a query to determine the state at that moment in time. This means something could be all fine today, but not anymore next week. 

Possible solutions: 

- Rate the quality taking into account the current date. This would take into account equipment expiring or drills that need to be done with some frequency.
- Present the users with upcoming actions they need to take. So, if something expires next week it should be easy to see that coming.
- Require and update of expired equipment some specified number of days (or weeks or months) before the expiry. How many days could be configurable. If you have not replaced the equipment at that point you have failed your own check. 

### Different levels of managements
Not everyone will have it's risk management at the same level, some will be strict some will be more loose. The level of strictness you want to work with need to be configurable. See, my later remark about 'booleans' versus 'scores'

### Taking initiative
Users need to take action to update their risk management. Some ways this could be stimulated:

- Have a page to give a forecast into the future. Like a weather forecast.
- Send notifications before things need to be done. 
- Require them early to plan some action later. So, say, today you notice something needs to be done in two months. You could wait two months, and everything will be fine all that time, but in two months you will suddenly be in violation. You could require users to plan their action. If they don't plan it they fail their check today. If they plan it they will need to complete that action before that day or they will fail their check on that planned date.

### Getting to perfect is hard 
We could have a general indication of the quality level of the state of maintenance. This could be a number of stars, or some color system (green, yellow, orange, red), a number between 0 and 10. A problem with this is that it is hard to get a perfect score. Users will usually be somewhere in between all and none. When users realize it it is too much effort to get to a perfect score they will become less motivated. 

#### Broken windows theory
In the context of unit testing people often mention the [broken windows theory](https://en.wikipedia.org/wiki/Broken_windows_theory), when some windows are broken people care less about other windows getting broken and the general state deteriorates. This is why in unit testing it is important to keep all tests green all the time. There is nothing in between. This works well, developers are motivate the keep everything green all the time. When tests fail it could be made impossible to merge. So, it would be nice to have a boolean indication of success.

#### Separate the *boolean* indication from the level of quality *score*
Users are motivated to get to a perfect situation (how can I get my 3 stars?!) but in reality the quality level will not be perfect, certainly not in the early stages of using our system. A possible solution is to have two indicators:

- One is for the level als quality we want to achieve, a score
- The other is whether you conform to that level of quality, a boolean

Two examples:

- **ISO certification**: When getting an ISO certificate the company itself describes which quality it wants to achieve (the strictness of this description varies in a range, it is kind of score). The auditor checks if the company complies to it's own quality requirements (boolean).
- **.editorconfig**: When getting a code base to conform to the strict rules of the .editorconfig it is a good strategy to start out with an .editorconfig with low requirements (you add/remove or disable checks, which checks are enabled affects the quality score). The entire codebase should conform to the current .editorconfig all the time (boolean). In a later stage more rules can be enabled.

I understood that there are two categories of users, those who define the measures, and those who have to follow up on the measure. Perhaps those who define the measure should get a score to indicate how good their measures are. Those who follow up on them get the boolean indication.

### A few examples of systems that have boolean checks

- [Unit tests](https://xunit.net/images/getting-started/common/test-explorer-failure2-vs2019.png)
- [TeamCity build monitoring](https://www.jetbrains.com/teamcity/img/screenshots/projects-overview.png)
- GitHub notifications (handled or not)
- Emails (unread or read)

## A somewhat more concrete solution

### Boolean checks in an hierarchical structure
**Hierarchy**: There are many pages which could all be visited individually in a random order (perhaps by just using the url), but conceptually they should have some hierarchical order. This hierarcy could be visible in the path crumbs, but perhaps we need other means to help make that hierarchy clear.

**Drill down**: Where ever there is a status icon it should be either:

- A leaf in the hierarchy, which should indicate its status.
- A branch in the hierarchy, from which it should be possible to drill down to the leaves.

### Clearly indicate something is wrong

In this image the red/green icons from unit tests are used in the left most column. In all UI elements related to risk management the same icons should be used, in the same column. Perhaps the left most column would make it the most prominent. On all pages it should look similar although we may not always want to use the exact same controls, because different pages can be different in nature. So, perhaps some pages do not have a grid at all. Still the icons should be styles in a similar way.
![image](.images/../images/riskmanagement.png)

### Issue overview page
Instead of navigating through the site to visit all individual issues it should also be possible to visit a page where you get just a list of all the current issues. This could be similar to the github notification page. The items should have information about what it relates to and what the problem is. Clicking an item should take you to the related page where you could fix it.

### Translate all issues to a boolean
For some issues it is easy to see it is wrong. Perhaps some registration is missing. For other issues it might not be to clear cut. For instance if things need to be done before a certain time. If it needs to be done before tomorrow you want to alert in advance. I would not advise to use a warning icon in this case, because it is likely users will have warning icons all the time, which takes away their motivation to get to all-green. One solution is to specify how many days in advance something needs to be arranged, and only make it red after that moment. You might want to show upcoming issues when going to the issue overview page. Perhaps it should also be possible to postpone an action, making things green again. When you postpone you will get a notification on the day (or the day before) to complete that postponed action. 

### Status over all vessels
So far we were discussing the risk management status of an individual vessel. In an overview over all vessels it would also be possible to indicate the status of each vessel as an icon in a column. The page itself could also indicate it needs attending. This way the entire site would fall under this hierarchical structure.