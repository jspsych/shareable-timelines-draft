# Shareable, configurable timelines for jsPsych

## Where things currently stand

As of jsPsych version 7, the main shareable units of development are *plugins* and *extensions*. A researcher can publish their plugins and extensions on `npm` and there's a standardized way of interacting with these that is well documented.

Researchers are often after a different unit of shareability, which might be an entire task. The *hello, world* examples of this for psych research would be something like a Stroop task or a Big 5 inventory. Right now, if you want to share something like this you typically share the entire set of code for the experiment. The [Niv lab repository](https://nivlab.github.io/jspsych-demos/) is a wonderful example of this kind of resource. 

## Where we could go

Tasks/questionnaires/instruments/etc. should be shareable like a plugin. 

Imagine I want to administer the Big 5 as part of an experiment. I think I should be able to do something like this

```html
<script src="https://unpkg.com/@jspsych-timelines/instrument-big-5"></script>
```

```js
const big5 = jsPsychTimelineBig5; // assuming that this is some global import loaded by the script above

timeline.push(big5);
```

Importantly, I think I should also be able to configure this timeline through parameters that the original developer has control over. Here I'm not really sure what the right way to implement this would be. One way would be for a timeline package to include a function that generates the timeline, with an object of parameters:

```html
<script src="https://unpkg.com/@jspsych-timelines/task-arrow-flanker"></script>
```

```js
const flanker = jsPsychTimelineArrowFlanker({
  trials: 60,
  max_response_time: 2000,
  fixation: true,
  fixation_duration: 500
});
timeline.push(flanker);
```

The thing that I don't really like about the above is that it doesn't match how other objects are added to the timeline. Another option would be something more like:

```js
const flanker = {
  type: jsPsychTimelineArrowFlanker,
  trials: 60,
  max_response_time: 2000,
  fixation: true,
  fixation_duration: 500
}
timeline.push(flanker);
```

This behaves exactly like a plugin, which is both good and bad. Easy for people to use, but also potentially confusing. It would also require some changes to the core library to support this new kind of type. 

## Some problems

1. How do we handle plugin dependencies? If my shareable timeline uses plugin *X* then do I need to include that plugin in my root document? I don't think I should, but then there's the issue of when I load plugin *X* anyways because I'm using it in a different part of my timeline. What if there is a version conflict?
2. How do we pass the jsPsych reference in a way that isn't clunky? Presumably the timeline will need this.

## How to get there

A few steps I have in mind:

1. Spec out what this would require to work really well, both from a developer and consumer perspective.
2. Determine if we need changes to core jsPsych to make this feasible.
3. Create an analog of `jspsych-contrib` repo for hosting these timelines.
4. Publish a handful to show how they can be used.
5. Create a template repo for people who want to publish their own independently of a central repo.


