---
layout: post
title:  "Vue.js Submit Button Component"
date:   2019-04-28 03:45:02 -0700
categories: blog
tags: vue javascript 
---

In the book "[Getting Real](https://basecamp.com/books/Getting%20Real.pdf)", 37Signals suggests incorporating administrative functionality into the public interfaces, rather than having dedicated administrative pages. One of the reasons given is that this means that you have to develop/maintain only half as many interfaces. Instead of one to display an item and one to edit it, you have only one, where users can view the thing, and adminstrators can access admin functionality. I have moving toward this model [Small Archives](https://smallarchives.com), and as a result have greatly reduced the number of pages I have to maintain, and also reduced the amount of variability between these pages. This morning I took this simplication a step further by creating a simple "Submit Button" Vue component to further standardize the UI/UX between various pages. Before today, I had about four forms that woud need updating any time I decided to make a change to form submission mechanics. Perhaps I made a change to the buttons colors, then kept working and forgot to take the time to make that same change everywhere else. As a result, there was drift in the consistency of these four forms and over time they all became quite different. This is a perfect use for a Vue component. I can build this interace once, and include it everywhere it needs to be. 

While it is great to simplify things and create unified systems, there is still the need for some nuanced behavior. In this component, I have set it up so that rather than actually doing logic when a button is pressed, it just submits an event back to the parent, which can then decide what to do with the info. This keeps the interface decisions in the component, but leaves the logic for each button to be determined in the parent view.

Here is the SubmitButton.vue component:

{% highlight javascript %}
<template>
    <div v-if="!loading"> <!-- an if else to determine if we should display the buttons or the loading animation -->
        <hr class="my-4" /> <!-- an interface element that will be consistent across all views that use this component -->
        <!-- the submit button that, when clicked, switches to the loading animation and emits a 'submit' event to the parent -->
        <div class="btn btn-primary mr-2" v-on:click="loading = !loading; $emit('submit')">Submit</div> 
        <!-- the cancel button that, when clicked, switches to the loading animation and emits a 'cancel' event to the parent -->
        <a class="btn btn-outline-primary" v-on:click="loading = !loading; $emit('cancel')">Cancel</a>
    </div>
    <div v-else> <!-- show the loading animation to indicate to users that the button push was successful and the app is working on the task -->
        <hr class="my-4" />
        <div class="spinner-border" role="status">
        <span class="sr-only">Loading...</span>
        </div>
    </div>
</template>

<script>
import firebase from 'firebase';

export default {
  name: "SubmitButton", 
  data() {
      return {
          loading: false
      }
  }
};
</script>

{% endhighlight %}

And the in the parent view, we include the component, with two `v-on` listeners for the two custom tasks. Each one listens for the emitted keyword (submit, cancel) from the component, and then calls a function (onSubmit, goBack) based on that. Maybe for one interface we want cancel to call goBack, but on another we want cancel to call "startOver", and we now have that flexibility, but with the consistency of a shared interface.

{% highlight html %}

<SubmitButton v-on:submit="onSubmit" v-on:cancel="goBack" />

{% endhighlight %}