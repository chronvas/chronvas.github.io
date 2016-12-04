---
layout: post
title: Realm for Android, pre-populating from Json and extracting the database
date: 2016-08-01
excerpt: "Playing with Realm and Android."
tags: [Android, Realm, code]
comments: false
project: true
---

Scroll to read, or continue reding in <a class="social-btn" href="https://medium.com/@chron.vas/realm-for-android-pre-populating-from-json-and-extracting-the-database-8709a2f8db18" target="_blank" rel="noopener noreferrer"><i class="fa fa-fw fa-medium"></i></a>

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

Now we need the Json file. A simple one for brevity reasons named “people.json” :

{% gist chronvas/6df49ddae05a094e9ffd8900bc251741 %}

The .json file has to be placed into raw resources in order to be accessible. Create the raw resource folder if it does not exist

<figure>
	<a href="https://github.com/chronvas/chronvas.github.io/raw/master/assets/img/realm2.png"><img src="https://github.com/chronvas/chronvas.github.io/raw/master/assets/img/realm2.png"></a>
</figure>

The representing Java class for parsing the Json:

{% gist chronvas/6fa279ec3fa74c2815678350a43f34b4 %}

### Ok! It’s time for parsing

Normally, we would use a helper class extending AsyncTask. But thanks to the Realm.executeTransaction we can just this built in method instead.

{% gist chronvas/8a1e513b96716c2cf26ce84acaadd97a %}

And a simple button to call .importFromJson()

Keep in mind that calling importFromJson does not delete the database or the data in it, it just adds the results from the Json parsing.

On Android monitor we can view the time it took to complete, and possibly saved from a published app.

`E:doInBackground:: Task completed in 9ms`

### Now, where is my .realm database file?

Once you run the application to a device, in Android Studio go to Tools>Android>Android Device Monitor, under the ` Data / Data / <Package Name> / files ` directory you can find the .realm file and pull it to your computer.

<figure>
	<a href="https://github.com/chronvas/chronvas.github.io/raw/master/assets/img/realm3.png"><img src="https://github.com/chronvas/chronvas.github.io/raw/master/assets/img/realm3.png"></a>
</figure>

### Did it work?

You can add another button for simplicity to return the number of Persons found in the database. The code of the two simple buttons could look like this:

{% gist chronvas/c9dee7f65198ff05fdba8a6952735b8c %}

Now just take the .realm file and place it under raw resources again to be used as the database.

### On the “to be published” app

Just put the .realm file in the raw resource directory and copy it to the appropriate place (FilesDir) using copyBundledRealmFile method on App.java .

I added an overwriteDatabase flag if you want to overwrite the database every time the app starts with a fresh one, or just keep the one that copyBundledRealmFile created the first time the app ran:

{% gist chronvas/c1c028ddc1670b39c2751598e4300d22 %}

### Keep in mind!

* With the overwriteDatabase flag set to true, every time your application starts, it copies or overwrites the .realm database from raw directory to the /files directory, so keep in mind that whatever change you make it is going to be lost upon App.java call. In order to demonstrate the “problem” I have added 2 more buttons to change and query the name of the first person. In order to avoid this, set the flag to false, which makes a file existence check before writing the .realm file.
* The .realm file still has 2 instances. In the raw resource directory and in the /files directory. So saving space is not done in fully.
* Don’t forget to use the correct data model when you read the database, or a realm migration if needed.

I hope you found this article useful. You can find the project here https://github.com/chronvas/RealmTesting
or give it some <a class="social-btn" href="https://medium.com/@chron.vas/realm-for-android-pre-populating-from-json-and-extracting-the-database-8709a2f8db18" target="_blank" rel="noopener noreferrer"><i class="fa fa-fw fa-heart"></i></a> on <a class="social-btn" href="https://medium.com/@chron.vas/realm-for-android-pre-populating-from-json-and-extracting-the-database-8709a2f8db18" target="_blank" rel="noopener noreferrer"><i class="fa fa-fw fa-medium"></i></a>
