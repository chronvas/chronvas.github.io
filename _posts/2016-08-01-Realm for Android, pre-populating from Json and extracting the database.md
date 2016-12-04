---
layout: post
title: Realm for Android, pre-populating from Json and extracting the database
date: 2016-08-01
excerpt: "Playing with Realm and Android."
tags: [sample post, code, highlighting]
comments: false
---

Scroll to read, or continue reding in <a class="social-btn" href="https://medium.com/index" target="_blank" rel="noopener noreferrer"><i class="fa fa-fw fa-medium"></i></a>

I’ve came across the “hustle” of creating an app that will use Realm, having the database previously populated with data and just include the created database file in the published app, omitting the Json (or any other file) that is being used to populate it.

Doing so for a “to be published” app saves time (parsing the file and populating a db) and space (omitting the file which used to create the database).

[^1]: <http://en.wikipedia.org/wiki/Syntax_highlighting>

### The main idea:
<figure>
	<a href="https://github.com/chronvas/chronvas.github.io/raw/master/assets/img/realm1.png"><img src="https://github.com/chronvas/chronvas.github.io/raw/master/assets/img/realm1.png"></a>
</figure>
I’m going to show the simplest possible way to achieve this, but this can be much improved (a separate Gradle task maybe?) and even be a part of the main project, not having to maintain 2 code bases and 2 different projects for one app.

### For the hasty
The project is on [Github](https://github.com/chronvas/RealmTesting), but check the “Keep in mind” section first

### Begin!
I am using the latest Android Studio (2.2) and Realm Java version 2.0.0 . The build.gradle of the project looks like this:

{% gist chronvas/cea8c32673394fde6d086e6a5f2a2987 %}

And the build.gradle of the app module looks like this:

{% gist chronvas/78f531b54a718d0441a65df5a4d8d275 %}

It’s time to add a class that extends android.app.Application, so our whole application starts from this class and Realm gets initialized and configured on OnCreate. It’s handy to set a name for your .realm file containing the database. More configurations can be [passed](https://realm.io/docs/java/latest/api/io/realm/RealmConfiguration.Builder.html) in this way. Just remember to point out this class in the Android Manifest.

{% gist chronvas/bd721b11d31cda98b61c04a6f3861d88 %}

### GitHub Gist Embed

An example of a Gist embed below.

{% gist chronvas/0e39355e36389e3992cbacc21bbe2596 %}
