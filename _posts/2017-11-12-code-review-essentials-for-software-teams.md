---
layout: single
title:  "代码评审的核心"
date:   2017-11-12 21:20:57
categories: Technology CodeReview
toc: true
toc_label: 目录
toc_icon: group
header:
  overlay_image: http://7xil47.com1.z0.glb.clouddn.com/CodeReviewCommonImg.jpg
  overlay_filter: 0.5
---

代码评审是协作型软件项目的核心，确保项目的成员和代码本身在正确的方向上前进。

Code Review Essentials for Software Teams

Code Review is an essential part of any collaborative software project. Large software systems are usually written by more than one person, and so a highly functioning software team needs a robust process to keep its members, as well as the code base itself moving in the right direction.

Code Review is a powerful tool that:

- Helps team members adapt their mental model of the system as it’s changing
- Ensures the change correctly solves the problem
- Opens discussion for strengths and weaknesses of a design
- Catches bugs before they get to production
- Keeps the code style and organization consistent

It’s helpful to think of these benefits as a hierarchy of needs.

Code Review: Hiercharchy of Needs

![代码评审关注点等级](http://7xil47.com1.z0.glb.clouddn.com/code_review_hierarchy.png)

## 团结团队

Keeping Team Members Together

The most critical function of a Code Review is to keep every member of the team moving in the right direction. You can’t safetly change a system you don’t understand, and so Code Review keeps the team mentally aligned together. When Bob submits a pull request for the accounting subsystem, Amy keeps her mental model of that system updated as she reviews Bob’s code. Amy gets the chance to ask questions about pieces she doesn’t understand, and Bob gets the benefit of clarifying his design decisions as well as teaching someone else about his work. When Amy has to make a change to the accounting subsystem a month later, her mental model of the system is already up to date and ready for action. She spends less time reading code and trying to piece the system together in her brain, and more time thinking about higher level abstractions and designs. Everyone wins, because everyone stays together.

> 代码评审能够让每个成员都在正确的方向上前进，让每个成员都能了解到系统的全貌

## 重构决策

Executing a Good Pull Request

Before you even write a line of code changing the system, ask yourself the following questions:

* “Is this the right thing to be working on?” There will always be competing needs from customers, internal team members and other parties. This can be a good way to keep priorities straight in your head before you dive in deep. Other processes like iteration / sprint planning meetings also help keep this on track.
* “Does the team already agree that the change is the right one?” If not, it’d be better to start a design discussion by email or maybe in person. Your changes are more likely to get accepted when people are in agreement about the design before you change it.
* “How can I break this change into digestable chunks that are easy to review and understand?” Small changes are easier to think about and understand. Good discussions flow from the team being able to comprehend your change quickly. If your change is massive, team-mate's eyes will glaze over, and you might only get a few style nitpicks from them.
* “How am I going to test this change to kill bugs and ensure correctness?” You might have a QA department, but it’s still your job as the developer to ship quality working software. Easily testable software is usually more decoupled, is broken into smaller chunks, and easier to reason about. You need to have a testing plan for all your changes.

写代码之前，问一下自己如下问题：

* 是在“做正确的事吗”？
* 技术方案是团队认可的吗？
* 如何让变更对代码的改变更容易评审和理解？
* 如何测试代码变革以消除bug和错误？

I’ve found that answering these questions ahead of time has saved me a lot of headache in the long run. The last thing you want to do is spend days coding up a change only to have it rejected based on fundamental design flaws or team disagreement. Or for your change to get held up because no one can verify it for correctness. Again, the goal is to change the system while keeping other team members up to date with your changes. Asking yourself about how you’re going to break your work into bite-sized chunks and test those chunks is helpful on many fronts:

* It reduces risk
* It makes changes easier to reason about
* It pushes you towards a better design

如果变更动作是为了修复系统瑕疵，但却不被认可，也可以自问所做的改动是否有如下价值：

* 降低了风险
* 使变更更加容易
* 驱使我们做更好的设计

You’d rather make many small, precise cuts with a scalpel than one giant gash with a machete. I prefer scalpel driven development in most additive cases, and like to save the machete for when it’s time to delete large blocks of dead code.

> 尽量做小的重构，大规模的重构一般用于删除大量的无用代码

## 请求评审

Sending the Pull Request

Ok, so you’ve gotten buy-in from the team that your changes are good, and you’ve achieved the design you set out to build. What’s the most effective way to actually send your pull request? You’ve been working hard on your changes, and you want other members of your team to pay attention to your pull request and give you fast feedback. How can you do that?

Remember earlier when I said that the most important part of a code review is to keep the team’s collective mental model well aligned? Your other team-mates are probably working on something completely different than you, maybe in a completely different part of the system. Their brains are in a completely different context, so you must overcome this by giving them the helpful guidance they need to do the review. This means writing a well-organized description of the changes you made, why you made them and any other relevant information they won’t get from just reading the code alone. Don’t make your team-mates do more mental work than they have to.

> 在请求同事对你的代码进行评审之前，你需要把代码变更部分尽可能用文字描述清楚，不要让评审人员做更多不必要的思考

Let’s look at some good and bad examples of pull request descriptions:

Bad Example:

Title: Fix uninitialized memory bug

Description: 

This is the bug Bob and I talked about earlier. I had trouble with

the compiler but managed to make this work. Let me know what you 

guys think.

> 较差的例子

    标题：修复未初始化内存bug

    描述：
    这是Bob和我之前讲过的bug，我在编译的时候遇到了麻烦，不过还是搞
    定了，你看看我这个做法行不行。

If you’re the developer of this pull request, stop and put yourself in your code reviewer’s shoes for a second. The title is vague, and raises more questions than answers: Where is the memory bug? How critical is this change? What was the bug that Bob and you talked about earlier? What trouble did you have with the compiler? The description doesn’t provide any helpful context about the problem, nor does it provide any helpful description of your changes. If this pull request is long, the reviewer is going to have to dive into the code and do mental gymnastics in an attempt to gain context before even getting a chance to think about how this change fits in to the coherent design of the system.

> 这个评审请求的标题比较模糊，很多问题没有解答：内存bug在哪里？这个变更有多重要？Bob之前和你讨论过的bug是什么？你在编译的时候碰过过什么麻烦？而其描述部分也没有提供任何有益的前因后果说明，评审人员不得不花费很多精力钻研代码才可能获得足够的信息，来评判这个改动是否合适。

Here’s a improved example that helps clarify the changeset:

Title: Fix process crash on startup from uninitialized memory [#54633]
Description:

This bug was causing process crashes on boot due to a memory
initialization error in our statistics Counter class. I talked this
over with Bob, and we both agree the crashes are a rare edge case
that don't warrant a hot-fix release. Here's a summary of the
changes:

  - Moved the underlying int variable into the class initializer
    to prevent ununitialized memory in the Counter.
  - Reworked the Counter interface to simplify caller conditional
    logic and prevent further off-by-one counting problems.
  - Added a unit test that exposes the crash

Testing: I've verified the test suite still passes, and verified
manually that the crash doesn't happen locally.

> 优化后的例子

    标题：修复在启动时由于未初始化内存导致的进程崩溃问题[#54633]

    描述：
    由于在我们用于统计的Counter类里的内存初始化错误，会在启动应用程序时会
    导致进程崩溃。之前我和Bob讨论过，一致认为这个崩溃时比较少见的边界类错
    误，没有必要立即进行修复上线。如下内容概述了本次变更的内容：
        - 将隐含的整形变量移到类的初始化方法中，避免在Counter类中的不执行
          内存初始化行为。
        - 重构Counter的接口，简化调用者的执行逻辑，避免后续的off-by-one计
          数问题。
        - 增加了一个单元测试来验证这个crash。

A few things that have improved in this pull request description: The title is descriptive enough to give the reviewer some short context, and entice them to click on the email and learn more. The reviewer knows it’s a process crash (which is usually a really bad thing) and there’s also a bug report number where the reviewer can read more details about the bug report if they’re interested in understand more about the problem. The new description also outlines where the problem was occuring, and helps give some context about how critical the fix is. There’s a summary that lists the high level structural changes in the pull request, which will give the reviewer a mental picture of the changes before they even look at the code. Code reviewers should not be surprised with what they find. This example also outlines testing that was performed, which will give reviewers confidence that the changes were well thought out, well tested and ready to be merged. Set your reviewers up for success by making it stupid easy for them to click ‘merge’.

> 这个评审请求的说明比较到位了：标题部分足够详细，使评审人员快速获知具体问题位置，而且包含了记录的问题ID入口，可以直接点进去看进一步的问题描述。

> 描述部分指出问题发生的位置，帮助明确问题的严重程度；并且给出了较高层级的对架构的方面的变动说明，这非常有助于评审人员在看代码之前就能有比较明确的感知变更的重要程度；同时给出了验证这个缺陷的测试用例，使评审人员知悉这个变更的考虑是周全的、已测试并且做好了合并入主干的准备。

> 良好的代码评审请求说明能够让评审人员更加容易允许“merge”。

## 评审者：给出建设性的反馈

Reviewers: Giving Constructive Feedback

The next part of the review process to consider is giving constructive feedback to your peers. If the goal is clarity and alignment, giving quality constructive feedback helps everyone understand the system better and push towards better code.

> 评审人员要对评审结果给出有建设性的反馈建议，帮助大家更好地理解系统、产出更高质量的代码

Avoid saying things like:

- “This design is broken.” Why is it broken? How can it be made better? Statements like this hurt confidence and can bruise egos.
- “I don’t like this change.” Why don’t you like it? What would you like instead? It’s okay to not like something, but you should articulate your thoughts and provide helpful clues about how the code can be improved.
- “Can you rewrite this to be more clear?” What’s wrong with what I have now? How should I rewrite it? What’s unclear? Comments like this are themselves unclear, and don’t give a simple path forward.

避免说这样的话：

- “这个设计不好。” _这个设计为什么不好？怎样才能做的更好？类似这样的语言会伤害开发者的信息和自尊。_
- “我不喜欢这个变更” _为什么你不喜欢？你更喜欢哪个方案？可以不喜欢现在的方案，但是应该清晰地把自己的想法表达出来，并提供有价值的建议指导如何提升这部分的代码。_
- “能重写地更加清晰么？” _现在的这部分代码具体有什么不好？应该如何重写才能更好？哪里写得不清楚？应该把这些不清晰的地方明确出来，而不是仅仅给出简单的方向。_

Instead, say things like:

- “How does this code handle negative integers?” This feedback is specific, and causes the developer to think through the outcomes themselves. As a reviewer, you might know that the code in question crashes with negative integers, but it’s better to have the developer intuit this themselves. Questions like this might also be indicative of a testing gap, or a need for further specification of scope.
- “This section is confusing to me, I don’t understand why class A is talking to class B” If the developer hasn’t provided a helpful description, and the code isn’t neatly structured, this kind of comment helps drive for clarity of design and cleaner code.
- “It looks like you broke an interface boundary here. How will that affect the user?” You’ve pointed out an issue you noticed, giving them the benefit of the doubt that they meant to do it. Now they can think through the unseen ramifications of breaking the interface boundary and either provide a rationale for their decision, or choose to change it.

相反，应该这样给出反馈：

- “这段代码如何处理负整数？” _这个反馈是明确的，能让开发工程师自己找到解决方案。作为评审人员，你可能知道负数处理的代码导致了崩溃，不过让开发者凭直觉知道这一点是更好。_
- “这段代码有点疑惑，我不能理解为什么A类要和A类通讯。” _如果开发人员没有提供有价值的文字说明，而且代码可读性较差，这样的反馈有助于促使开发人员完成更简洁清晰的设计。_
- “看起来你这里打破了一个接口的边界，这样做将会怎样影响到用户？” _你在这里指出发现的问题，给出质疑的好处是会促使他们进行变化。这样的话他们就能思考打破接口边界所带来的潜在影响，或者再陈述一下决策的依据，或者改变这个设计。_


In general, framing feedback as questions is a good way to drive for clarity, correctness, and at the same time help the developer improve their designs in the future. This is usually how quality creative writing groups give each other feedback. In a creative writing setting, it’s harmful to say things like, “I don’t like this character.” whereas the same comment can be reframed more clearly: “In chapter one your character was warm and compassionate and now he’s cold and icey. He doesn’t seem like a real person to me.” Now there’s specific feedback that can be clarified to suss out the problem.

> 将反馈的信息包装成问题而不是简单陈述，有助于达成清晰、正确的目标，同时能够帮助开发人员在未来优化设计。

Programmers like to solve problems and point out issues, so by nature they like to discover and point out flaws. It’s tempting to see code reviews as a way to prove how smart you are by finding problems in your peer’s code. Don’t do it. Code review is a way to get more eyes on a change and suss out critical problems, but your goal should be to review in a way that encourages your team members to improve their skills while fixing the problems at hand.

> 我期望的代码评审的目标是在修复问题的同时提升自己的技能

## 代码风格

Style Points

Brace positions, variable / function names, indentation and spacing issues should be addressed, but they are not the central purpose of good code review (notice that I put them at the top the code review pyramid). If you find that your team is spending 90% of it’s time nitpicking indentation and variable names, you’re probably wasting everyone’s time on something that could be mostly automated. Write up a style guide, enforce indentation and spacing issues on check-in and spend your time focusing on higher value issues. I don’t want to diminish the importance of a consistent style. On the contrary, having a consistent idiomatic style is one of the easiest ways to make your codebase easy to read and comprehend. Still, if you spend all of your code review focused on these simplistic tasks, ask yourself whether you’re avoiding the harder and more important work of keeping your team mentally aligned and thinking about higher level designs.

> 代码风格不是评审的重要关注点，这个可以通过自动化工具实现。评审的重点是设计决策和提升团队能力。