<!-- title: Running Tests using Lune -->

### Motiviation
Everybody loves running `runtime runTests`, and then see the test results right in front of them without the need for opening an entire engine, and surprise, surprise! I am one of them.

Unlike Web developers and other similar ones, Roblox developers are locked to using Roblox Studio to view their unit tests' results (I mean, this assumes you are a developer that uses unit tests, and those developers are rare).

### Vinum
You are Sinlernick, a dude that maintains Vinum. Suddenly, you thought of a very wild idea, and your spido-sense is annoying you to the point you will try all what you can do to just implement it.

Actually, this isnt "spido-sense", this is just you wanting to run tests through lune directly. Basically, you just confused yourself so that you can get the job done.

#### Hello Lune, I am Vinum!
Of course, your brain just wanted the easiest way to run tests through lune. And that way is just to completely port Vinum to lune. This was easy, because Vinum was runtime agnostic in a way, requiring you to only change `require` (pun intended) statements, and add some minor variables. 

So this worked, but basically, your conscience that usually sleeps all day, just woke up and is angry that you did that because well, ***you can't run Vinum in roblox now***.

#### Yo Roblox, I am sor-
And because you want Vinum on roblox, because thats basically the platform it largely intends to run on, you tried few stuff to solve that. There is a insanely simple solution to all of this, but of course, because you are already in this black hole, you won't realize this until its too late.

So as a good Maintainer, you decided to use Darklua's convert require rule. Simple right? No...

Of course, since basically the convert require rule didnt seem to work, and more importantly, ***types are no more***. You see, since Darklua hasnt implemented type support for luau yet, you are basically left with a mess that doesnt have types, which, I would like to remind you, ***TYPES ARE A FUNDEMENTAL SELLING POINT OF VINUM***.

Of course, you tried to test the Bundling feature that DarkLua provides. And the results were confusing because you are no Darklua experienced user, and types also seemed to not work at all.

And you got bored of Darklua, so you ditched it, and decided to do something like this in require statements:
```lua
    local x = if script then require(script.Parent.X) else script("x")
```

This should have worked, and it sort of did. But you see, since there is some weird logic about type resolving, ***types also did not work at all***. Of course, you could have solved this issue by investigating more, but you were already confused as heck, and just wanted any solution. So, you ditched this.

Another solution you thought of was automatic transformation of relative-file path requires to roblox path requires. This could have worked, but you were already tired, so you just said "NO".

#### I forgive you, if you...
Of course, the way you could have solved all of this, while preserving the original goal (that is running tests through lune), was simply: ***Run Code with Roblox requires in a environment with injected `require`***.

A more detailed explaination could be something like this: 
You have a `src` codebase that depends on Roblox requires, and a `test` folder that depends on file requires. All of this being processed by a sandbox, that wraps all files' contents in a function (this is from a previous design, therefore, is planned to be changed) that is being immediately run, and on top of each file a `requireImpl` and a `script`.

This `requireImpl` does the following:
1. if the input was string, replace all `src` substrings to `procSrc`
2. if the input was a table, read its `path`.
3. if a file to be found, require it.
```
INFO: Since *requireImpl* gets added only at the "compile-time", all `require` calls are automatically *string.gsubed*.
``` 

And of course, the `script` concept requires some implementation-details to understand, but just be aware that each object that is returned when indexing `script` needs to be 100% unique in some cases.

#### I forgave you!- But...

And now, after some weird stuff you implemented, tests are now working, while maintaining direct Roblox compatibility.

Of course, this also means that Lune users wont be able to use Vinum, however, there is also this detail that you (cough cough INTENTIONALLY) forgot, was that Lune's maintainer was already planning to add built-in support for the `script` global.

So, all of what you did was basically useless, but hey, that will take time to implement, and your "totally legit" spido sense was already annoying you to the point of CRAZINESS *(insert some dramatic effects here)*.
