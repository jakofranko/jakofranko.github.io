---
title: "Hobgoblin + Thoughts on AI"
date: 2017-02-14 9:48am
---

It's been a little over a year since my last post (time flies!) and since then I've been up to a few things. I've been continuing on my [superhero-themed roguelike](http://github.com/jakofranko/hero) and have started [another roguelike with a friend of mine that's set in the Monster Hunter International universe](http://github.com/jakofranko/MonsterHunterRL). For both games, I'm using ROT.js, a roguelike toolkit for JavaScript, and it's awesome. One thing that I've found as I've read tutorials and written a couple of my own games is that there is a way that I like to organize my projects, and this is generally informed by several people who's work I've followed closely. Since I really like organizing my projects this way, I decided to build a framework that uses ROT.js and write a little tool to initialize this scaffolding for new games.

Enter [Hobgoblin JS](http://github.com/jakofranko/hobgoblinjs).

Hobgoblin is just a little node.js script that you run from the command line, and all it does is create some very simple scaffolding, allowing a quick start to a new roguelike. In fact, if you run `hobgoblin --examples`, and open the index.html file in the browser, you will have a very simple, but playable, game, right out of the box.

As I build out MonsterHunterRL and Justice, I hope to add to this framework as I continue to develop generic functionality. I need to flesh out the documentation for Hobgoblin, and hope to include themed examples instead of the generic examples from Cave-Quest, but it works as-is nonetheless.

MonsterHunterRL was built with Hobgoblin, but Justice was not. I'm wanting to add AI to MHRL, and I have a kind of messy version of it in Justice. So I thought I'd try to learn from my mistakes and add it correctly to Hobgoblin.

## AI - Making monsters smart

Hobgoblin actually supports a limited form of AI with its `TaskActor` entity-mixin. When it's an entity's turn, it attempts to do the first task. If this fails, it tries to do the next task and so on. It's implemented kind of clumsily, with each task needing to be defined right there in the mixin. As tasks get more complex, this mixin would just get huge. I think there's a better way.

As I mentioned above, I have implemented this 'better way' to a certain degree in Justice. The general structure is as follows:

- `TaskActor` is now `JobActor`
- An entity can have any number of jobs
- A job consists of a series of tasks, and a way to prioritize how to do that job
- Every in-game hour, an entity re-prioritizes it's jobs,
- Every turn, the entity does the highest priority job
- A task is the actual nuts and bolts of the entity's behavior
- Jobs and tasks are each defined in different files
- When a job is performed, it is done like so: `Game.Jobs[job].doJob(entity);` the job essentially processing the entity (instead of the entity peforming the `job` function which is part of the entity object).

I like this abstraction much better. The way it works out in Justice however tends to have tasks being rather large, logic-wise. This also won't work more generically because of the way that I have the prioritization of jobs tied directly to the in-game time. The language ends up being confusing as well, because in Justice, I want most of my NPCs to have actual day jobs; so one of the "jobs" is "work" which consists of a number of tasks, one of which is "doWork" etc. You can see how my nomenclature is problematic in Justice.

I think I'll implement something similar in Hobgoblin, but instead of `JobActor` I'll create an `AIActor`, and house the behaviors in the `Game.AI` namespace, and the tasks for those behaviors under `Game.AI.Tasks`. Entities will be defined like so:

```
Game.EntityRepository.define('kobold', {
  ...
  ai: ['packhunt', 'scavenge', 'wander'],
  ...
});
```

When an entity is up to act, it will attempt to do each behavior based on that behavior's criteria, in priority of the order listed. Each behavior will be defined like so:

```
Game.AI.packhunt = function(entity) {
  if(<can't perform this task>)
    return false;
  else {
    if(<near target>)
      entity.attack(target);
    else
      Game.AI.Tasks.approach(entity);
  }
```

With `Game.AI.Tasks.approach` having the logic to actually find/select and then approach the entity's target. The idea here is that an AI behavior like 'thief' could also use the `Game.AI.Tasks.approach()` task, as could a more normal 'hunt' behavior.

Something that is not in Justice that I AM wanting to implement more is the Dijkstra map functionality that I recently added to Hobgoblin. These will be stored at the `Game` namespace level I think, and should not necessarily belong to a specific entity. The type of Dijkstra map to use should be determined by the AI behavior and then passed to the appropriate task, when needed. Otherwise normal pathfinding within a task could work.

The main thing that I want to avoid (that I'm having a hard time avoiding in Justice) is not adding attribute after attribute to the entity as they become needed by various tasks as I write them. This may be unavoidable, and I don't have a solution...I just want to avoid it :P

That's all I have for now! Just wanted to organize my thoughts on "paper" a bit.
