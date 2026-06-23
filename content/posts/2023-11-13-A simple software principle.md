---
layout: post
title: A simple software principle 
date: 2023-11-13
excerpt: "Prefer language tools, if they exist, rather than relying on assumptions."
tags: [Software Principles,Software Engineering,Api Design]
comments: false
---
# A simple software principle 

_Prefer language tools, if they exist, rather than relying on assumptions._


<figure>
	<a href="/img/workshop.png"><img src="/img/workshop.png"></a
</figure>


Assume you are working on a project that consumes a third-party SDK that provides you with a simple list of fruits. Your application’s task is to take these fruits and make juice with them. You name the juice that you make with the name of the 1st fruit you get from this list.

Your application looks like this:

```
import fruitery.SDK
import fruitery.SDK.Fruit

fun main(args: Array<String>) {
    val sdk = SDK()
    val fruits = sdk.getFruits()
    val juice = makeJuice(fruits)
}

private fun makeJuice(fruits: List<Fruit>) =
    Juice(
        name = fruits.first().name,
        content = squash(fruits)
    )
}

private data class Juice(val name: String, val content: String)

private fun squash(fruits: List<Fruit>) = /* squash the fruits */
```

Do you see a problem with that? If not, look closer. More specifically “fruits.first()”. Kotlin docs state clearly that it can throw, but there is no compilation error. It is therefore up to you to decide if you build your app to defend against this possibility or not. Some other languages don’t tell you that.

<figure>
	<a href="/img/nosuchelement.png"><img src="/img/nosuchelement.png"></a
</figure>

Some will make the assumption that it will never return a list of empty fruits. Some of the most common arguments defending this approach are:

- The whole point of using the SDK to begin with is to get some fruits, not an empty list. You are, of course, correct. BUT: SDKs can fail, internal errors might occur, and one of them could result in the list returning being empty.
- I have used this SDK a million times before, and it never returned an empty list. You are again correct; no one can take away your experience, BUT: You don’t control the SDK. You probably don’t work for the SDK team either. The SDK team might also have a valid reason to return no fruit.
- It compiles, so it must be fine, BUT: You are forgetting that Kotlin is just a language. Remember the Java null pointer exception back in the Java days? It compiled fine too. Even if Kotlin is a million times better than Java, it is still not “perfect”, and just because Kotlin is able to take care of NPE (and many other issues) doesn’t mean that it takes care of everything and can make all the decisions for you.
- I have seen the SDK’s source code, and I can attest that this cannot happen, BUT: How are you sure that this is not going to change in the next SDK version? What is your team’s process on updating SDK versions? What are you going to do in your app if this changes?
- My other colleague also supports the idea that it’s fine, BUT: Will you both take the same responsibility if that crashes? Think about that for a second. Also, again, what does the language documentation tell you?
- The majority of the team believes that’s fine. This is probably the most valid argument against that, and it can self-portray as “common sense”, BUT: You should probably start checking how sense is your common, especially when your collective common sense contradicts something like the official documentation. This may also be derived from team culture (e.g., the fear of suggesting something new) and could be prevalent among less diverse teams (including technical or industry experience diversity, etc.) or other team culture factors in various degrees. But if the majority is truly happy, that is probably the most central point of a healthy codebase.
- There is no use case designed or specified for receiving empty fruit lists; therefore, we do not have a defined UI for this use case, BUT: Probably, this is not the first time something like that has come up. What do you normally do within your team in this case? Maybe do that here too?

## It’s not just lists

The same list/fruit example can be with e.g. casting:

```
open class Animal

class Dog : Animal()

class Cat : Animal() {
    fun meow() { /*..*/ }
}


fun someFunction(animal: Animal) {
    (animal as Cat).meow() // ClassCastException crash
}
```

In this case, there is not even directly linked documentation that you can look up for this ClassCastException. Keep in mind that Kotlin, or any language for that matter, is not perfect and can’t make decisions for you.

## It’s not just Kotlin

In Android, we are lucky that we have such a nice, strongly typed language. Similar problems might occur in other languages too, no matter if they are strongly typed or not, which could beg a different solution or a different principle than what I am proposing here.

## Where does it end?

There is no point in checking everything every time. It can lead to Analysis Paralysis, and it can slow down code progression without adding much meaningful impact, especially to external eyes.

But you have to be aware of these things. Think about employing the help of automated tools, and maybe have a way to connect those instances with real runtime problems. Ideally, talk with the SDK developers to clarify that (maybe suggest adding documentation) or let them know of a crash you observed around that.

## Architectural positioning

Let’s say that now the developer and the team are convinced to follow this principle, but where (in which architectural layer) should the check (filtering out bad fruit lists in the first example) take place?

..and, of course the answer is:

_It depends_

If you follow clean architecture, you might consider adding such checks in the data layer. But isn’t this filtering a domain matter? You might be considering the danger of spreading out your “domain” logic to other architectural layers.

It is important to take a glance at what this affects. Maybe have a look at pre-existing examples in the codebase you are working on; maybe have a look at adjacent teams and find out what they are doing. Ask your colleagues what they would be most fine with, and ideally agree and have clarity on how much you follow principles that affect that. How other languages deal with those problems can also be an inspiration.

## The principle

If you made it this far, thanks for reading my thoughts. The principle I propose is simple:

> Prefer language tools, if they exist, rather than relying on assumptions.

In the first Kotlin example, instead of calling list.first(), prefer calling list.firstOrNull(), which is using the Kotlin language tools to protect your app from unexpected crashes.

Some languages do not have those protections. Maybe ask around or make your own, but be pragmatic with extra care towards external APIs so your API can be robust without relying on assumptions for any of the API consumers.

## Another principle on the back of that is:

> Consider adding protections closer to the source.

Meaning, if this protection makes sense to be added, consider adding it at the layer closer to the source of what needs to be protected against instead of delegating to a layer above.

This could make your unit testing simpler and more robust, with fewer tests and simpler test cases for those other layers above, without losing any of the coverage. Notice the word “consider” here.

Thanks to Jodi Winters and Rob Duxbury for proof reading it.