---
title: "LORE-friendliness: My design philosophy for everything IT"
date: 2022-07-24T16:09:26+02:00
draft: false
tags: ["Linux", "Software Engineering", "Design", "Script", "Development", "Systems Administration", "Reliability", "Servers", "Philosophy", "Opinion"]
---

Throughout my experiences in IT, I have managed to develop and uphold my own set of good practices on top of all the common one we see in the industry ; such as clean code, good documentation, methodology, etc. I have personally found that these were much more manageable long-term on projects of any size. Though it will sound like most of it is common sense, I've found that some design philosophies contradict my principles for no reason that sounded valid to me. I've heard reasons such as "it's an engineering standard", "it makes the codebase more manageable" or even worse, "historically, we have to do it this way".

I thought, "if these reasons aren't valid and make my job more annoying to do, I should find what exactly I think would be better". And after theorizing it a little bit, I've managed to isolate four core elements to my personal design philosophy in computer science:

## The LORE

**LORE** stands for **Lightweight, Open, Reliable, and Explicit**.

Obviously, a good, scalable tool that fits this principle entirely is hard to find. Therefore, every letter of this principle is in order, from the "most prone to compromises" to the "least prone to compromises". The wording here is essential: I do not mean that "being explicit is essential, or more important than any other principle", nor do I mean that "it's ok to completely ignore a principle if we get a better solution for a 'higher priority' principle". Let me explain by describing every one of these principles.

**DISCLAIMER: I am by no means a genius engineer, nor even a senior. I am merely starting my career, but thought I'd share some points and opinions I've noted during my time and experience in IT. Everything is subject to change as I advance my career, but I try to stand for these principles I've personally considered are right up to this point in my life.**

## Lightweight

A tool, software, service, library, server, or anything developed should be as lightweight as possible. This also regroups optimization: code runs fast, server and network usage are low, solution isn't a strain on your infrastructure and your workflow. In practice, this means:

### No over-engineering

If you need to run a bunch of scripts on a server to check or deploy some configurations, keep one file per job. Doing one megascript will not only be a pain to develop and maintain, but also is likely to bring confusion once it runs in production. It's basically the [KISS principle](https://en.wikipedia.org/wiki/KISS_principle). A side effect of this is also, "if you have a software to develop, think about the best ways to do it rather than starting to design objects with design patterns".

### Restrict your files size as much as possible

If you want your infrastructure to be scalable, you can't afford to send hundreds of megabytes to every newly deployed server to run the necessary scripts. Doing so will put a strain on your network, your storage, and ultimately your purse. It also slows the deployment of new infrastructures, which could be a pain in Infrastructure as Code/Infrastructure on Demand scenarios, by increasing the total setup time of your new (virtual) appliance.

### Keep it stored in one (backed up) place

I am obviously not saying that everything should be stored in one single place, prone to deletion ; I'm saying it's better to have links to a single place when looking for something than spreading it on different platforms or even worse, duplicating it. This one should be pretty straightforward, but you'd be surprised at how documentation, git repos, etc. can get spread out between GitHub, GitLab, Jira, Mattermost, etc. *Don't make your and your colleagues' jobs harder.*

### Don't be afraid to replace

Finally, you could (and should) replace a solution by another if it is more adapted. Migrations can be costly in terms of time and money, but on the long term it's more often than not worth it. Don't wait for the next project to tell yourself "we'll use this better tool", because all you'll do is having to maintain both tools, the old one and the new, theoretically better one.

---

However, sometimes complex/real-world solutions require some of these elements to not be fully respected. It's fine, and perfectly understandable. You can make some compromises on lightweightness in cases that would make your solution more:

## Open

Note that here, I don't necessarily mean "open-source", though it sure helps. What I mean can be summed up in two things:

### Auditability/testability

**Every software, solution, or appliance must be auditable**, and anyone telling you otherwise is either lying to you or trying to scam you. It is necessary, notably in terms of security, to be able to know what you are making, buying, and overall running on your own infrastructure. *No solution is a black box.* Whether your audits are made internally or externally through an auditing company, or even automatically though static code analysis or such, the key to a safe implementation is one that has been studied beforehand.

When auditing, there are common things to look for, depending on what you are auditing. For instance, when auditing a network appliance, you must be looking for specific configurations that would allow only SSH connections rather than Telnet, or firewall rules, or IP ranges. These are the type of rules that must be available to an admin with the correct authorizations through the input of a few commands in the terminal. Anything that looks shady and/or unaccessible is usually not a good sign that this solution can be trusted. In most cases, it's a configuration error and a fix is available ; but sometimes, it requires the replacement of an entire solution.

### Modularity

Or, in other terms, "open to extension". Every element of a solution should be replaceable easily, as well as being extendable. As an example, if an Ansible playbook causes an error on your deployment, it should be able to not be ran while you look into it without breaking the rest of the deployment. A good way of doing this is to have scripts, functions, libraries etc. as pure as possible (in the functional programming sense of the term). *One script, function, library, etc. must do one thing and one thing only*, and be easily replaceable if a better solution exists.

It is also better to be able to extend a solution's abilities, by adding new tools, new scripts, new appliances etc. seamlessly into what's existing. Once again, a software being open-source helps in respecting that point, but it is not a necessity: you could make tools that make use of your internal API without it necessarily being open-source.

---

You should now understand that it is possible to make compromises on the weight of your program if it means opening it, making it more complete and fit for your purpose on the long term. Therefore, the hierarchy I presented earlier should make more sense. Let's move to a more fundamental element, one where compromises are even harder to make:

## Reliable

This one could be exempt of explanation as it is pretty obvious, but it is there not as a "new thing you should respect", but rather as a "this is essential, don't forget about it". I won't give you a lesson of what makes something reliable, but keep in mind a reliable solution is one whose:

* authentification (and authorizations),
* availability,
* integrity,
* confidentiality, 
* accountability (traceability)

are respected. If one of these elements fail, your solution can't be considered as reliable anymore.

Ultimately, keep in mind that **a chain is only as reliable as its weakest link**.

## Explicit

Finally, the last point is, in my personal opinion, the one where compromises are the least acceptable: though I can accept why a process has to be heavy, slow, and buggy if it is necessary, I can hardly justify it not being able to repair it when it fails. It is also the most "meta" principle of the four, applying not only to computer science but to every field and, by extension, work in general. The way I define explicitness can be summed up as "when all else fails, the most important thing is to be able to know why".

This comes in a lot of forms:

### Explicit and accessible documentation

A very obvious point, a lots of bugs and misunderstandings can be explained and understood through correctly redacted documentation. A wrong implementation is one thing that can be solved by yourself, but a correct implementation of a wrongly documented solution requires communication, time and delays on the long term. Therefore, keeping such misunderstandings from happening is an easily attainable goal that can hardly be restricted by any other constraint than time.

### Explicit and detailed logs and debug messages

Your solution will fail. It always does at some point. The most important and easily implementable thing is to have correct logging of every action and error message, to make debugging even possible at all. Providing information on whatever data could've entered a function, where this data came from, and what caused this function to fail is essential. Save yourself some time later, and add proper logging systems, export your logs (while obviously respecting security guidelines), and put monitoring systems in place that inform you immediately when something fails, with information on why.

### Explicit and genuine communication

I've only started my career a few years ago, but countless issues I've encountered could've been avoided should one person have explained a problem or the constraints of a solution properly to their colleagues. Clear communication is essential, not only in meetings or emails, but also when reporting bugs or issues. Report bugs on the proper platform (support ticket, GitHub issues...), and only have one channel for these: don't use both Jira and Issues, for instance, as this will make bug tracking much harder. Include the logs, the error messages, the reproducability steps, etc. If you are solving an issue, *be sure to explicitly convey what caused the issue, how much tile it will take to resolve it and why, and what can be done to avoid it next time*. It seems so obvious as I write it, but it really has not been every time from my experience.

---

## Conclusion

With all that said, whenever I design a solution in IT, I ask myself: "*is it LORE-friendly?*" As in, "do I tick all four of these boxes, in order and as best as possible?"

I try to answer this question with "Yes" as much as possible.

Again, I am by no means an expert engineer, nor a visionary genius. As I was writing it, a lot of it sounded more and more like common sense to me ; but I thought it was useful to write it nonetheless.

This post serves as personal guidelines to everything I create in IT: I use the least tools possible, in the most efficient and optimized way possible, I make it easy for myself and others to work on it after me and determine whether it's a good solution or not, I make efforts in covering as many cases as possible while making it reliable and consistent, and most importantly, I know when and why it fails and I am able to understand and convey this information in a way that will make it easier for other people to do their job.

Maybe in 10 years I won't be able to justify some of these points anymore. Maybe some will be incomplete or straight up obsolete. Some may be gross approximations of computer engineering and its related fields.

But, for now, I think they are right, and I will keep on doing what I think is right. And when they'll be wrong (and it will obviously happen one day), I'll gladly adjust!

I shared these guidelines as I thought they could also help people thinking about the way they do their job. Feel free to discuss them with me and/or sharing them to anyone. And ultimately, don't entirely take my word for it, and use your own experience and critical thinking as to whether these principles can apply to you or your organisation.

I love imagining, designing and developing creative yet simple solutions to the problems I encounter. Hopefully the LORE gives me guidelines to help me do so for the years to come!