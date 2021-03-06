![Timeywimey Device](http://25.media.tumblr.com/tumblr_lh1tjkLSkG1qa0q13o1_500.jpg)

[Timeywimey](http://www.youtube.com/watch?v=LakwV3P3qII) [Device](http://www.youtube.com/watch?v=8LqmAQwHNA0)

A task scheduler and idle-detector for JavaScript.

## Purpose

Sometimes, a task takes a long time to run. During those sometimes, we also sometimes need to update the data which will power that task. (Animations, for example.) As such, we want to be able to execute the task, but only after we have the most up to date data.

Or, if it's an expensive task, we only want to do it when we think the JS engine isn't in the middle of anything, and the user isn't doing anything we'd cause to hang.

In other words, we need a scheduler with a li'l bit of brains in it.

## Usage

Scheduling a task is simple:

```javascript
TW.queueTask('breakfast', getSomeCoffee);
```

Timeywimey is smart enough to execute the task as soon as it can; either when it thinks your app is idle, or when the timer for that specific task occurs, but only if no new calls have been made to it in the meanwhile.

Given the above example, if another task is scheduled for `'breakfast'` before it's been triggered:

```javascript
TW.queueTask('breakfast', getSomeYogurt, {queue: true});
```

The timer is reset, and when it fires it'll trigger both tasks.

Also, if timeywimey thinks that the JS engine isn't doing anything intensive, it'll go ahead and execute your tasks for you.

## Options

`TW.idleThreshold`
Determines how many ticks pass before timeywimey calls the system "idle".

`TW.tick`
Determines how often to check whether the system is working or not.

`TW.tasks`
A hash of all the tasks registered for timeywimey.  It's possible to browse this hash for various information about the tasks, or to modify them, if for some reason you need to do so.

`TW.defaultInterval`
The default amount of time to wait before executing a task.  Increasing this provides more time for all tasks to wait before attempting to execute.

## API

`bower install timeywimey`
and/or
`npm install timeywimey`

Pop it in a script tag, use an async loader, or whathaveyou.  Provided in vanilla and coffee for your tasting preference.

`TW.queueTask(label, callback [, options]);`
Schedule `callback` to be executed when the `label` task is executed.  An optional `options` object may be passed as a third argument, containing boolean flags for `queue` and `animation`.  If, in the options, `queue: true` then the callback will be added to a queue of other callbacks to be executed when the task is executed.  If `animation: false` is passed as one of the options, then a `setTimeout` will be used instead of `requestAnimationFrame`, if for some reason a specific time interval is needed before executing the task.  The `setTimeout` default interval is set to the `TW.defaultInterval`.  `queue` defaults to `false`, and `animation` defaults to `true`.

`TW.executeTasks(label);`
Executes all queued tasks for a given queue.

`TW.executeIdleTasks();`
Used to execute all the queued tasks when the system is idle, but can be called to immediately execute all tasks.

`TW.working();`
Overrride this with whatever kind of logic and/or graphics you'd like to show when a long-running task occurs.

`TW.finished();`
Override this with the logic and/or graphics to show when a long-running task completes.

`TW.nextTask();`
Executes the next task in a given queue immediately, and removes it from the array.

`TW.lastTask();`
Executes the last (most recently) added task to the queue, and removes it from the array.

`TW.flushTask();`
Clears out a task, removing any items in its queue along with metadata.  You get to decide when this happens; TimeyWimey won't do it by default.

`TW.flushAll();`
Clears all tasks, removing all queued items and metadata.  Start fresh!  Again, this is up to you to decide where to implement; TimeyWimey won't make that decision for you.

## Notes

"Also, it can boil an egg at 30 paces, whether you want it to or not, actually, so I've learned to stay away from hens."

TimeyWimey uses requestAnimationFrame where it can, for most of the time-oriented things required of it.  However, if the environment in which it's loaded doesn't support that, it'll fall back to using setTimeout, which will tax the CPU a little more, and thus drain the battery faster (laptops, mobile devices, etc.).  May or may not matter for you.

## Special Thanks

[@drpowell](https://github.com/drpowell) had the original idea, and the entire [OpenBio Codefest 2013](http://www.open-bio.org/wiki/Codefest_2013) group, from where this sprung.
