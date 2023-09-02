---
title: "Pair Programming for Technical job interviews"
datePublished: Sat Sep 02 2023 20:35:53 GMT+0000 (Coordinated Universal Time)
cuid: clm2hhd2d000108mo31up1pe0
slug: pair-programming-for-technical-job-interviews
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/fSWOVc3e06w/upload/5598d9d2d8239fd54a167983e0fd525a.jpeg
tags: hiring, pair-programming, interview-questions, technical-interview, interview-preparations

---

I was watching Dave Farley's video on [Software Developer Interview Advice](https://www.youtube.com/watch?v=osnOY5zgdMI) - (I am not looking for jobs, I just wanted to reflect on the interviews I've helped facilitate where I work. Multiple points resonated with me and brought back memories of prior discussions.

[The research says that job interviews are poor indicators](https://www.psychologytoday.com/us/blog/insight-therapy/202008/poor-predictors-job-interviews-are-useless-and-unfair), but they are our best right now. You want to see if a person. and as an interviewee, you want to see if the company is a fit. In software, it is important to gauge the technical abilities too - and this motivates the dreaded technical job interviews. They are often much like verbal exams; you are mainly gauging the interviewee's ability to handle job interviews. If you want to hire great interviewees, GREAT! if you want to hire great technical people, you will have data noise from nervousness, the knowledge about the specific questions you ask, etc.

# The majority of technical interviews

The interviews will often have some sort of coding exercise, and to make them fit in a 30/45-minute timeslot you have an algorithmic problem in the employer's preferred programming language, they are often not representable for an actual work process. As Dave Farley argues, a great employer will care less if a new employee knows C# (for instance) and more so their ability to learn and work together in general; language abstracts are mere technicalities. I speak from experience; at a prior job, I had to write an application in C# (over a two-week period), but due to time pressure we ended up going through it verbally. Between you and me, I hadn't used C# for years and was quite rusty. It went well; I got the job, but it surely wasn't representable, and I was probably lucky I could hide my rust by it being a verbal technical challenge.

# An extreme alternative

The consultancy I work for, is inspired by, among other things, [eXtreme Programming](https://en.wikipedia.org/wiki/Extreme_programming) (hence, the pun in the heading). This also includes pair programming. We don't use it religiously in my team, but being a remote-first company, it can be a great way to ensure team cohesion (see my other posts with perspectives on remote-first), furthermore, it is a great way to solve some of the more difficult technical or abstractional challenges.

Based on these inspirations, our technical interviews are also a test of how well people work in a pair-programming session. This, however, has a bunch of benefits.

# How we run our technical interviews

Prior to the interview, the applicant is asked to prepare any small problem in any project in any programming language. *Any* is an important word here. We then, for 45 minutes work through the problem, where the applicant controls the keyboard, and works on it (Like [the think-aloud practice from UX](https://en.wikipedia.org/wiki/Think_aloud_protocol)). We help solve the problems, much like we do when pair programming on actual problems for customers. We might help search relevant documentation for the applicant - spot missing semicolons or whatever might be.

Again - the ANY is crucial. When I had my interview I brought a C# API I was working on to familiarize myself with DDD and event sourcing. Not a tech stack that is in any way relevant to my new job. However, it gave me an opportunity to showcase myself at my best, as well as maximize the complexity of what to present. I wanted to refactor some parts of the code following TDD principles. We didn't actually finish it - it wouldn't even compile when the 45 minutes were up. But I was in a psychologically safe environment, where I was able to showcase my skills. My (now) awesome employer, was able to see how I could communicate problems, discuss different potential solutions as well as develop software "at my best". Furthermore, I got a small taste of collaborating with engineers inside the company - which I could use to influence my decision too.

# The differences it makes
The process creates an environment that is more enjoyable for the interviewee. [Psychological safety](https://hbr.org/2023/02/what-is-psychological-safety), has been on everyone's lips this year, and why not start at the very beginning? Both parties are able to experience working on an actual problem that one person is confident in - both parties experience all the pains one goes through in an actual development process and see how the other party reacts. And it's a whole lot more fun. I've learned something new after each interview, whether that be about the [Eight queens problem](https://en.wikipedia.org/wiki/Eight_queens_puzzle), [Explainable AI](https://en.wikipedia.org/wiki/Explainable_artificial_intelligence), weird biomechanical stuff that I barely even understood or just fun little frameworks in various languages.  

Also a small secret; it requires less preparation for us. Worst case we have a bit of reading up on a framework or project (some of my more mathematical coworkers have worked on some complex problems no amount of Wikipedia could help me understand, but that is also a great exam in communicating complex problems to simple-minded people, like me).

We aren't perfect though. In my opinion, we could improve our explanation of the process to the applicants - some applicants are stuck in the traditional perspective, and try to guess which tools we use, and create a problem that matches that. This has the consequence that they aren't as confident in what they are doing, and the benefits of the process are worsened. 

# The core value

When we are hiring, we aren't looking for an engineer who knows the exact stack we are using. We are looking for great minds who are eager to learn and are versatile. We also want diverse perspectives - to improve our conjoint knowledge base. Our whole mission is to be at the forefront of data engineering and machine learning - if that is the case, then years of experience isn't possible not possible. It's the classic meme, that reoccurs in the software field:

%[https://twitter.com/tiangolo/status/1281946592459853830]

Let's strive to create an interview process that is comfortable for the interviewee and interviewer, which actually brings to light the skills of the interviewee.