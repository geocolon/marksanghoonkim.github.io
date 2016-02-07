---
layout: post
title: Backbone and MVC
description: Using Backbone and MVC patterns, with a recap from MakerSquare Weeks 3 and 4.
keywords: blog, post, makersquare, los angeles, D3, MVC, frameworks, Backbone.js, Backbone, React, React.js, node.js, models, views, collections, servers, REST, SQL, databases
---

Last week we delved into MVC architectural patterns and worked with Backbone during a two-day sprint. It was my first experience with a framework, and while it was difficult getting started, by day two of the sprint, my partner and I made significant progress and were adding additional features to our Backbone project. I wanted to share a little more about what I learned during the Backbone sprint.

<a href="http://backbonejs.org/" target="_blank">Backbone</a> is a JavaScript framework with a RESTful JSON interface. It employs the model-view-presenter (MVP) pattern, which is a derivation of the well-known model-view-controller (MVC) pattern. Backbone is described as being lightweight and unopinionated. It allows code modularity by separating the models and views. My project was to create a web audio player that could play songs and queue other songs that would play automatically once the currently playing song ended.

<h5>Models</h5>

Backbone uses Models to represent the data in a web application. These models can be created, validated, destroyed, and saved to the server. Whenever an action changes an attribute of a model, a 'change' event is fired. The Views in the application can pick up on the change event and can respond by re-rendering themselves with the new information. In our project, we used two models - a SongModel and an AppModel.

~~~~~~~~
var SongModel = Backbone.Model.extend({

  play: function(){
    this.trigger('play', this);
  },

  enqueue: function() {
    this.trigger('enqueue', this);
  }

});
~~~~~~~~

In this partial code snippet, you can see the SongModel has a few associated functions that can be invoked on a Song. For example, the song has a 'play' function and an 'enqueue' function. 

~~~~~~~~
var AppModel = Backbone.Model.extend({

  initialize: function(params){
    this.set('currentSong', new SongModel());
    this.set('songQueue', new SongQueue());

    params.library.on('play', function(song){
      if (!this.get('currentSong').has('title'))
      {
        this.set('currentSong', song);
      }
    }, this);

    params.library.on('enqueue', function(song) {
      if (this.get('currentSong') !== song) {
        this.get('songQueue').add(song);
      }
    }, this);
  }

});
~~~~~~~~

The AppModel is more interesting. The AppModel's attributes have listeners that detect changes to their values. In this example, the AppModel has a currentSong attribute and a songQueue attribute. We will see later how a LibraryEntryView listens for click events on the Songs, which will fire off the play() and enqueue() functions. Those functions will change the attributes on the AppModel. The AppView will listen for changes in the AppModel's currentSong and update the NowPlayingView and the PlayerView.

<h5>Views</h5>

A Backbone View is a component of user interface. Views listen for changes and render the UI. They send input information to the Models and are used to display what your applications' data Models look like. 

~~~~~~~~
var LibraryEntryView = Backbone.View.extend({

  template: _.template('<div>(<%= artist %>) - <%= title %></div>'),

  events: {
    'click': function() {
      // if a song is currently playing, then the song will be queued
      this.model.play();
      this.model.enqueue();
    },
  },

  render: function(){
    return this.$el.html(this.template(this.model.attributes));
  }

});
~~~~~~~~

In our code snippet here, we can see that the LibraryEntryView listens for click events on the songs in the library. When a song is clicked, the SongModel's play() and enqueue() functions are triggered. We can see flow of information from the LibraryEntryView to the SongModel to the AppModel.

~~~~~~~~
var AppView = Backbone.View.extend({

  initialize: function(params){
    this.nowPlayingView = new NowPlayingView({model: this.model.get('currentSong')});
    this.playerView = new PlayerView({model: this.model.get('currentSong')});
    this.libraryView = new LibraryView({collection: this.model.get('library')});
    this.songQueueView = new SongQueueView({collection: this.model.get('songQueue')});

    this.model.on('change:currentSong', function(model){
      this.playerView.setSong(model.get('currentSong'));
      this.nowPlayingView.displaySong(model.get('currentSong'));
    }, this);
  },

  render: function(){
    return this.$el.html([
      this.nowPlayingView.$el,
      this.playerView.$el,
      this.libraryView.$el,
      this.songQueueView.$el
    ]);
  }

});
~~~~~~~~

Now we have our AppView which is a Backbone View for the whole music app. As you can see from the line with <em>this.model.on('change:currentSong')</em>, when the AppModel's currentSong attribute is updated, either from a clicked song or an queued song being played, the AppView will direct the playerView to set its song to the new currentSong. The AppView will also tell the nowPlayingView to update and display the new currentSong.

~~~~~~~~
var NowPlayingView = Backbone.View.extend({

  tagName: 'div',

  template: _.template('<div class="nowPlaying"><b>Now Playing:</b> (<%= artist %>) - <%= title %></div>'),

  initialize: function() {
    this.render();
  },

  displaySong: function(song){
    this.model = song;
    this.render();
  },

  render: function(){
    if (this.model.attributes.artist === undefined) {
      return this.$el.html('<div class="nowPlaying"><span style="opacity: 0">PLACEHOLDER</span></div>');
    } else {
      return this.$el.html(this.template(this.model.attributes));
    }
  }

});
~~~~~~~~

Now we have the NowPlayingView. From the AppView, we saw that when currentSong was updated, the AppView called the NowPlayingView's displaySong function. In here we can see calling the displaySong function will cause the NowPlayingView to re-render, which basically updates the DOM and updates the displayed information.

You can visit our Backbone player <a href="http://www.marksanghoonkim.com/backbone-player/" target="_blank">here</a> along with the <a href="https://github.com/marksanghoonkim/backbone-player" target="_blank">source code</a>. The Backbone player has play, enqueue, and dequeue functionality. I had a fantastic time working with my pair, Rick Yeh, whose website can be found <a href="http://www.rickyeh.com/" target="_blank">here</a>.

This project opened my eyes to how many different things occur in the background from <b>one</b> click. My former self would often complain: "Why can't they just change this about a product? It would be so easy and would make the website so much better." My current self now recognizes more the hard work involved in implementing even a small change. Backbone was frustrating in the beginning, but I appreciate now how the models and views work together. I am eager to see how other MVC frameworks work now that I have a taste.

<h5>MakerSquare Weeks 3 and 4</h5>

Over the past two weeks, we went over many, many topics. Some of the topics include:

* D3
* Backbone
* MVC
* Frameworks
* React.js
* node.js
* REST
* HTTP
* SQL

It was a packed two weeks. I'm still digesting all that I went through and have to review a few of those topics during the solo week we have. We were introduced to node.js and got a glimpse of callback hell. We only have another week of curriculum, and we'll be soon starting on our projects. Time sure travels fast, and I am excited to soon apply all the knowledge I have picked up over the last 4-5 weeks and build some great products! Until next time!