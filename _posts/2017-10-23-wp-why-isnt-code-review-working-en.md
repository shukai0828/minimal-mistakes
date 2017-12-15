---
layout: single
title:  "为什么代码评审没有起到作用"
date:   2017-10-23 18:07:56 -0600
categories: Technology CodeReview
toc: true
toc_label: 目录
toc_icon: fa-code
header:
  overlay_image: http://7xil47.com1.z0.glb.clouddn.com/CodeReviewCommonImg.jpg
  overlay_filter: 0.5
---

本文重点介绍了如何进行有效的代码评审工作。

![代码评审](http://7xil47.com1.z0.glb.clouddn.com/wp-why-isnt-code-review-working-en.png)

*It’s late. Alice is the last person left in the office, desperately trying to finish a last-minute feature for tomorrow’s release. At last, her code compiles! She has a nagging feeling that there are some mistakes in the code, but she hopes it’s nothing serious. If there are any major problems, I’m sure the code reviewer will catch them tomorrow morning, she thinks.*

*She submits her code for review and leaves work exhausted.*

*The next morning, Alice arrives fresh and ready for the new workday.Excited, she takes a look at the status of the peer review. Bob, the reviewer,has already sent back a response: ”Looks good. Approved.”*

*Alice is elated. Her code made it into the build at the last minute, just in time for release!*

*Out of curiosity, she looks at the minor mistakes she thinks she made. Her jaw drops: Not only did she make a couple little errors, but there is an obvious shipstopper! The application now crashes at startup. “How did Bob miss this?” she asks.*

---------------------

This is an exaggerated example of how the peer review process can go horribly wrong. Code reviews catch bugs during the development cycle, but it only works if the review system does.

Why did Bob miss such an obvious mistake? How could Alice have avoided sending code containing a critical bug? What measures could the project manager have used to ensure that the code review worked as intended?

## 使工程师有评审的动机

Giving The Reviewer An Incentive

Alice confronts Bob with his mistake.

“I’m sorry,” he says. “But I hardly know anything about the code you sent me. I looked at it quickly, compiled it, and figured you knew what you were doing. After all, I have a feature of my own to take care of.”

In this case, the fundamental problem preventing effective code review is that the code reviewer has no incentive to do a thorough review. He doesn’t know the area, and he’s got his own job to do.

> 本例中，评审人员没有动机进行周密的评审，评审人员本身还有自己的工作要做。

How could Bob have been properly motivated to read the code and check that it works at runtime?

Alice had worked alone on her feature. If Bill had had background knowledge of the code, it certainly would have made understanding the code easier.However, Alice had sent each of part of the feature’s code to a different reviewer to spread the burden equally. So this final code relied heavily on code that Bill had never seen.

When a feature is started, it is important for the manager to assign clear responsibility for that feature. Which developers are accountable for problems in the code? Software project best practices vary from one source to the next. Some claim the best strategy is to have one “owner” per feature. Others refuse to assign ownership at all, thinking that the team will own the code collectively.

But if a project relies on a peer review system, having two or three people responsible for each feature ensures optimal code reviews. This is known as the “buddy system.”

> 结对、伙伴体系

If a developer owns a feature, he is accountable for that feature, whether coding or reviewing. The feature represents what he is capable of producing for the company, so he wants to ensure the feature shines.

When two developers own a feature, there is no question where the features go to for review: One developer sends features to the other, and vice versa. The reviewers see code evolve from the beginning, so spotting mistakes in new code becomes easy.

This buddy system can be thought of as a form of pair programming, except buddies don’t work at the same desk. But they still reap the benefits of having a second motivated pair of eyes to look over their work.

> 不是严格意义上的结对体系，但仍然能够得到双重的评审保证

What about small features that only require one developer? Same answer: Use the buddy system. One buddy develops the feature. The other reviews the code. The feature still reflects the reviewer’s work, and if the developer is reassigned, the reviewer can take over what his buddy started.

> 即使是小特性，也要使用伙伴体系

What if one developer has code to be reviewed, but his buddy is away? In this case, the project leader finds an alternate, but the new reviewer must still have an incentive to do a thorough review.

> 评审伙伴发生变动，领导需要安排另外一个人员替代，并确保此人的工作积极性。

The best case scenario is to send the code to an expert. An expert has experience with the particular type of code or technology used in the code.

For example: Joe is writing an XML parser with MSXML and his buddy Sanjay is out sick. So Joe sends his feature to Helen, who has programmed with XML for three years now. Helen will find the issues with the code, but can also show off by using her expertise. The expert is therefore still a motivated reviewer. And Joe learns some new tricks.

> 特定领域的专家在该领域的经验非常有用，善用已有的专家对领域代码进行评审

## 每次只评审少量的代码

Review Small Chunks

Nothing demotivates reviewers faster than receiving a huge wad of code to review.

> 一次评审一大堆的代码将会严重打击评审人员的积极性

A 20-page feature will make a reviewer—and his inbox—groan. His motivation will plummet to zero.

On the other hand, if it is only a page long, it won’t take any time at all, and there is no excuse not to look at it. It might be a welcome break from that monolithic feature he is working on. Developers who would normally not be motivated to review the code may actually gain interest, since reviewing a small code snippet is a convenient way of being introduced to a new feature.

> 务必保证每次只评审少量的代码

## 作者本人是最关键的评审人员

The Primary Reviewer Is The Author!

Back at her desk, Alice’s mind wanders back to the previous night. Why didn’t she test it herself before sending it for review? I was rushing to get out the door, that’s why, she thinks.

Code reviews provide a convenient safety net. In theory, the reviewer catches bugs that the developer, despite extensive testing, overlooked.

In practice, a developer may grow to rely on the safety net, deferring all testing to the reviewer so that he can continue working on his feature. By doing this, the developer makes the process less effective by eliminating an entire review iteration—his own.

> 工程师经常忽略自身应该执行测试工作，总是想依赖他人

A developer must understand that first responsibility for testing the code is his. A developer must produce the best code he can before it leaves his hands. Unfortunately, developers don’t always take the time to re-read their code!

> 工程师必须认识到自己要对代码的测试负首要责任

This practice produces a lot of issues in a review. Developers under time pressure often race to finish a code without re-reading what they have written, or worse yet, without testing the effects of their code at run-time.

But how can a project manager ensure developers re-read their code?

> 管理者如何确保开发人员能够重读自己的代码

Simple answer: Test cases.

> 引入测试用例

Before sending the code to a reviewer, developers must run test cases, developer-described scenarios that test the feature works as designed. For example, if the developer’s code fixes circle drawing in a drawing application, test cases might include:

* Draw a random circle
* Draw a big circle that goes out of the screen bounds
* Draw a circle that intersects with a square
* Draw a circle in each color of the color palette

When the developer writes the test cases, the reviewer reviews them as if they were code. The reviewer can reject the code if there are any scenarios that are too vague to be useful, or ones that are missing. In the above example, the reviewer might agree that testing for a big circle that goes out of the screen bounds is a worthwhile test, but he may still reject the code because the developer didn’t try drawing a tiny circle of size 0.

> 评审测试用例要比评审代码更有效率和效果

This motivates a developer to re-read and test because:

* It’s difficult to outright lie about all test cases.
* Vague test cases will be rejected by the reviewer. “Extensive testing of all application functions” will be rejected as too vague. What did you test, exactly?What made it extensive? Which application functions exactly? This forcesdevelopers to write detailed scenarios.
*Even if a developer fudges his way though the work, detailed test cases have a good chance of making the developer think twice about potential problems that they might have overlooked when producing code.

> 测试用例驱使开发人员更加谨慎地对待

Furthermore, reviewers shouldn’t fix the author’s work because then the author doesn’t learn from his mistakes, and the problem will likely re-occur. In fact, a reviewer that fixes code encourages a developer to test less! There is no cost to him for making a mistake: “Oh well, the reviewer will fix it for me.”

> 评审人员不负责修复开发人员产生的错误，应有代码作者承担犯错的成本

If a reviewer thinks he has to correct author’s mistakes or offer alternative solutions, he will be less motivated to do the review.

## 在关键时刻进行有效的代码审查

Effective Code Review At Crunch Time

### 种瓜得瓜

You Get What You Ask For

Developers are naturally motivated by company or management recognition. In general, developers will do as the company asks. If the manager mainly rewards developers for coding outstanding features and ignores reviewers, then the message received by developers is to put priority on coding. If the manager rewards development and reviewing equally, he encourages both aspects of the process. Management simply needs to draw out the natural incentive of its employees.

> 在管理上应该同时重视开发和评审，不应偏袒

### 开发阶段将结束时BUG会飙升

The Bug Rate Skyrockets In The Last Stage Of The Development Cycle

A manager should take into account that the many problems will be introduced in the last week leading up to a milestone.

If necessary, reviewers should accept minor mistakes or omissions if accompanied by a FIXME or TODO note in the code in place of the actual fix. After crunch time in the next cycle, the developer can go back to those notes later. It sounds illogical to accept bugs in order to raise the quality of the code, but major bugs should still earn a rejection, and if a developer gets stuck fixing a minor one, he might not have time to do that major fix.

> 必要的话，评审人员可以容忍已知的小问题

A manager may have to monitor the review process during the last week to avoid fake reviews, or assign someone else to do it. Any review where the comments are short and positive, such as “Good job, approved” make the reviewer’s motivation suspicious. Has he really looked at the details?

> 管理人员应想办法避免虚假的评审

Other flags include large code submissions and submissions that have been reviewed by someone who hasn’t reviewed that feature before and might not be an expert in it.

> 尤其注意判语即短小又积极的评审，大量的代码提交，评审人员以前并不了解功能，评审者不是该领域的专家

A manager shouldn’t push for last-minute fixes going into the release. These fixes themselves will be sloppy and may result in worse bugs than the ones they fix, as in our example with Alice and Bob. It would be better to negotiate an extension or to settle for fewer features.

> 不建议在最后时刻接受新修复的错误，可能会因为草率处理引起更大的错误

A manager must set a good example. A parent who tells a child that cigarettes will kill you through a haze of second-hand smoke is less than convincing. It’s the same with managers who take advantage of their position to squeeze in last-minute fixes without review.

-------

It’s late. Alice is the last person left in the office, desperately trying to finish a lastminute feature for tomorrow’s release. At last, her code compiles! She has a nagging feeling that there are some mistakes in the code, but she hopes it’s nothing serious. If there are any major problems, I’m sure the code reviewer will catch them tomorrow morning, she thinks.

“Wait a second,” she says to herself. Since Mark, her regular reviewer, is on holiday—lucky guy, to be on vacation during crunch!—the review will fall to Bob, who doesn’t know anything about this feature. I’ll have to raise a flag about that. Whoops! I forgot to run the test cases. I’d better put in a TODO and commit this to the next build instead of this one...