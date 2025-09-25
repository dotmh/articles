---
title: "Learning C# in 2025"
datePublished: Thu Sep 25 2025 15:48:35 GMT+0000 (Coordinated Universal Time)
cuid: cmfzl86vj000002kw2dh42qyj
slug: learning-c-in-2025
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/505eectW54k/upload/e7faba071c82e840464ad07339c48184.jpeg
tags: csharp, learning, net

---

## The Why?

I try to regularly learn a new programming language, the reason for this can best be summed up with this quote from the [Pragmatic Programmer](https://amzn.eu/d/bCjcAYx)

> **Different languages solve the same problems in different ways. By learning several different approaches, you can help broaden your thinking and avoid getting stuck in a rut. Additionally, learning many languages is far easier now, thanks to the wealth of freely available software on the Internet.**

This year, I decided to learn C# and, in extension, the .NET Framework. There were several reasons for this, but the primary motivation was to access a language that was similar to lower-level languages like C, C++, and Rust. Using C# as a stepping stone to move to these lower languages, but with the convenience of the high-level constructs and easier syntax.

I had not been particularly interested in learning C# as it was tied to Microsoft Windows, that is, until 2014, when Microsoft decided to open-source the project. In the ten years since that release, .NET has invested considerable effort in becoming a truly cross-platform platform.

Another reason is that my daily language is TypeScript, as I mentioned in previous posts. TypeScript’s lead architect is [Anders Hejlsberg](https://articles.dotmh.dev/my-development-heroes#heading-anders-hejlsberg), the same lead architect as C#.

The last reason for learning C# is that it can be used by Godot and other game engines, as well as bindings to frameworks like SDL3 and Raylib. I have a keen interest in game development; having knowledge of the language would allow me to develop my own games, whether using or without those engines.

## The How!

When you are an experienced developer, finding resources for a new language can be a challenge. This is because many language learning resources are designed with beginners in mind. This is brilliant, don’t get me wrong, getting people into software development grows the pool of programmers and innovation. The downside of this, as a senior developer, is that there is a significant amount of duplicated information. I am familiar with the fundamentals of programming, including data types, control flow, and object-oriented programming.

With this in mind, I used the following in order to learn C#.

### **C# 12 Pocket Reference**

This is a [book](https://www.amazon.co.uk/12-Pocket-Reference-Instant-Programmers/dp/1098147545?crid=28IRD5IPJV6K3&dib=eyJ2IjoiMSJ9.QexFY6Z7heMOZ76bi8e_BRaTLC0HiPm92ykK-MgY0RzXAJwq8x7BpPaOMD4TKOHJ0ZmrEj1WN62SL05lOHQveVOdEpsxfTzR1i15HzxbvrzDTPLEM2OoTz-x6Hq__c1Qzy-_z7URiuyMfX1wfXOx7Vmp8HIoDSjOV08XzK8ysQoV45_baI2gGVVZsLg_t2fuQ_VWh4oOGetxvOjsxr7DFaQdIxvUXPfIsZYRyHwBDxA.vmwygz_g8NAnK0EdR6-G8k2PfewoCz0EDVqT8Gka1YU&dib_tag=se&keywords=C%23&qid=1758795242&sprefix=c%23,aps,109&sr=8-18) from O’Reilly, where many books attempt to provide the reader with both programming and language information for the language they are teaching; the Pocket Reference is simply that —a reference to the language. I make a point of reading a section of this book every day. This gives me an idea of what the C# language is capable of. What features and paradigms can I use when building software? It doesn’t go into detail about them, however, but for that, it is a compromise I can work with, as I can often find more detailed information from other sources. I use this book more as an index of the tools I have in the C# toolbox, rather than a user's guide to those tools, although it will contain all the simple concepts I need.

### Jetbrains Rider

[JetBrains Rider](https://www.jetbrains.com/rider/) is an IDE from JetBrains designed for the .NET ecosystem, incorporating some of the best tools. These tools include ReSharper, dotPeek, dotCover, dotTrace, and dotMemory, all of which are available in a single IDE. If you are accustomed to JetBrains tooling, this setup will be very familiar to you. This makes it easy to get started with building projects, as all the tooling is made available in an easy-to-use and visual manner. Of course, all this can be done through the command line tooling provided by .NET with the `dotnet` command.

### LINQ Pad

[LINQ Pad](https://www.linqpad.net/) is a tool designed as a playground for .NET and [LINQ,](https://learn.microsoft.com/en-us/dotnet/standard/linq/) which is a built-in SQL-like language in .NET. It supports the major .NET CLI languages, including Visual Basic, F#, and, of course, the one I am interested in, C#. The basic tool is free; unfortunately, the free version is missing NuGet (.NET version of tools like NPM) support, which limits its usefulness beyond basic use. I quickly upgraded my version to Developer to get the extras, including the NuGet support. This allows me to build small code blocks to experiment with ideas and concepts without needing to spin up a full .NET project.

### Github Co-Pilot

This might be slightly controversial, but I have found Copilot really handy. I have been very disciplined when using it; however, I don’t want to vibe code in C#. I want to learn. I have a rule when adding code to a project that I don’t directly author, whether it comes from Stack Overflow, GitHub, a blog, Copilot, or anywhere else: I won't add it unless I understand it. This means that if I do use Copilot, I review what it's suggesting, try to understand why it is doing it, and consider what it is doing before adding it. This means that I do learn and improve my C# when using Copilot. The other thing I will use it for is a semi-smart search tool, so I will ask it things like “how do I check the type of a generic?”, “How do I load a mock from a file in an XUnit test?” or “How do I get a list of all environment variables?”.

### Writing Code

The only way to learn a new language is to write it, and to write lots of it. If you don’t work with the language and the ecosystem (and the .NET ecosystem is massive), you won't learn and develop skills in that language. So, I write projects and ideas that have been floating around in my head, and I start building them in C# instead of a language with which I am more familiar. The same goes if I need a small tool or service; I will build it in C# instead of Typescript. Every line of code I write in C# or problem I solve teaches me something. I also write tests for everything; this is not only good practice but also helps build more familiarity with the language. To that end, I created a new [template](https://github.com/dotmh/dotnet) in GitHub to make spinning up projects even more effortless, allowing me to write more code.

## What is next

I am just at the beginning of my journey learning C# and the .NET ecosystem. I find writing code in C# to be very nice, and the fact that I can run it pretty much anywhere is quite cool. I plan to keep learning C# for the time being. I want to build more project types, such as audio, GUI, api applications, as well as build games. I am hoping to add it to my toolbox for solving problems in the future, so expect more posts on my C# adventures.