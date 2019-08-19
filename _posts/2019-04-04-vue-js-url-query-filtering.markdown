---
layout: post
title:  "Vue.js URL Query Parameter Filtering"
date:   2019-04-03 03:45:02 -0700
categories: blog
tags: vue javascript
---
One project that I am currently working on is a simple publishing platform for archival collections called [Small Archives](https://smallarchives.com), built with Vue.js. Users create archives of items such as images, videos, and PDFs, which are then displayed, along with metadata, in a variety of ways. Currently, I am working on building out some of the filtering functionality so that visitors can easily view only certain facets of a given collection.

An archive has a main view for the header info, and then each type of listing (list, grid, etc) has its own separate component. For the initial filters, which only appear on the main archive page, it was easy enough to pass values from the various filter form fields to the components using Vue's `props`. However, an additional complexity arose when trying to add filters for "tags". Like the others, a filter existed on the main archive page, which was fairly straight forward, but the filters also can be triggered from a single item view by clicking on one of the listed tags. Clicking a tag on a single item view should take the user to the main collection listing, but be pre-filtered to show only items tagged with that same value. So now, rather than sending a value only between a parent and child, the child component needs to have already received the value before the parent loaded. Possibly this would be a problem well suited to VueX, which is a store for an application's "state", but I decided that using URL parameters would be a better user experience, since users would be able to share links, bookmarks, etc, that are pre-filtered. I have also decided to re-write the other existing filters so that the full state of a filtered list can be shared and saved via URL.

{% highlight javascript %}
{% endhighlight %}

First, I created a computed value that checks the url for a query called "tag" via `this.$route`, and return the value if it does:

{% highlight javascript %}
computed: {
  tag() {
    if(this.$route.query.tag === undefined) {
      return 'None' 
    } else {
      return this.$route.query.tag
    }
  }  
}
{% endhighlight %}

Then, check to see if that computed value (`this.tag`) exists (is not "None"), and filter the list of items accordingly:
{% highlight javascript %}
if(this.tag != 'None'){ // "None" is the default value that I selected for no tag. Might be a problem if a tag called "None" is used
  this.renderedAssets = this.renderedAssets.filter((item) => { // use .filter to reduce the initial array
    if(item.assetTags) { // check that an item has tags
      return item.assetTags.includes(this.tag)  // only add it to the new array if it includes the tag we are currently looking at
    }
  })  
} 
{% endhighlight %}

And Vue automatically updates the display because renderedAssets is part of the data object. So now we have our child component checking to see if a tag query parameter is defined, and that the value isn't "None", and displaying a filtered list as appropriate.

Next, we need to make sure that the filter form elements know about these values too. This is done on the parent view. First, check for an existing query parameter when the page loads, the same as on the child view.

{% highlight javascript %}
computed: {
  selectedTag() {
    if(this.$route.query.tag === undefined) {
      return 'None' 
    } else {
      return this.$route.query.tag
    }
  }
}
{% endhighlight %}

Now in our template we use the value of selectedTag to determine the state of the form element, such as what value is pre-loaded, and whether to display the "clear" button:

{% highlight html %}
<form>
  <div class="form-group">                
    <div class="input-group input-group-sm">
      <div class="input-group-prepend">
        <label class="input-group-text" for="inputGroupSelect02">Tags</label>
      </div> 
      <select class="custom-select tagFilter" v-on:change="filteredTag" v-model="selectFormTag"> <!-- use v-model to prepopulate with a tag value-->
        <option>None</option>
        <option v-for="tag in tags">{{tag.tagTitle}}</option>
      </select>
      <div class="input-group-append" v-if="selectedTag != 'None'"> <!-- Show the clear button if there is a value -->
        <button class="btn btn-danger" type="button" id="button-addon1" @click="clearTagFilter()"><font-awesome-icon icon="times" size="1x" /></button>
      </div>
    </div>
  </div>
</form>
{% endhighlight %}

So now when a page loads, it should be displaying the correct tag filter state based on the query parameter. But now what if the user, who has just arrived on the page, wants to change this filter? Notice in the snippet above that the select element has a `v-on:change="filteredTag"` property. When this field is changed, the filteredTag function gets called, which updates the route with the new tag query.

{% highlight javascript %}
filteredTag(e) {
  this.$router.push({
    name: 'PublicArchive',
    query: { tag: e.target.value }
  })
},  
{% endhighlight %}

The route has been updated, but the child element won't automatically detect that, so we need to have it "watch" the query for changes, and re-filter the results array like so:

{% highlight javascript %}
watch: {
    //watch tag for changes, and change the asset array when it does
    '$route.query.tag': function() { // watch it
      this.filterAssetArray()
    }
}    
{% endhighlight %}
