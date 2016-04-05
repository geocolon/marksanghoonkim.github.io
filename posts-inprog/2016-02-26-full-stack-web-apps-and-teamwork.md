---
layout: post
title: Full-Stack Web Applications and Teamwork
description: Lessons learned from building a full-stack web application and working in a team.
keywords: blog, post, angular.js, node.js, express.js, MySQL, full-stack, web application, databases, teamwork, empathy, makersquare, los angeles
---

I spent the last two weeks building out two projects. One was a solo project where I built <a href="http://www.marksanghoonkim.com/calcommute" target="_blank">Calcommute</a>, a commute calculator that takes your home and work addresses and presents the time and money costs associated with each commute option (driving, transit, bicycling, and walking). The other project was <a href="http://study-mate.herokuapp.com/" target="_blank">StudyMate</a>, a group project built over the course of a week. It was much, much larger in scope and was a full-stack web application built using angular, node, express, and MySQL. I was the product owner and project manager for our group and here are some of the lessons I learned while building out a full-stack application and working in a team.

<h5>Teamwork</h5>

During the last 5-6 weeks we spent pair programming, I learned firsthand how one's working chemistry with his/her pair has a profound impact on the results and experience of the project or sprint. It was no different working with a group of four. As the product owner, there were many things I had to consider in order to deliver a great product.

We started our first meeting talking about expectations - what we wanted to achieve as an individual and as a team. I shared that it was most important to me that we enjoyed our time working together and that we build a product that we could be proud of. To me, the teamwork aspect of the project was as important as the end result, and as the project lead, I focused on how I could facilitate effective communication and ensure everyone had opportunities to make meaningful contributions to the project.

We split our group into a frontend team and backend team, and I chose to work on the backend. While our frontend team made significant progress in the first two days, my partner and I struggled to set up our database. We first had to think about using NoSQL vs SQL, and we decided to go with relational because of how our users and events would function together. We then had to think about which DBMS (database management system) we wanted to use, and also if we wanted to use an ORM (object-relational mapping) or not. It was a slow, learning process, and there was a moment when our frontend team became restless in waiting for the backend team. There was some tension because the frontend team felt that the backend team was starting to hold the project back. I understood that the feeling of not being able to progress was causing some anxiety among the frontend members, and I listened to their concerns. I learned that effective communication consists of listening and thinking before speaking. While I felt that some of the concerns that were raised were not completely fair to my teammate and myself, we didn't let our bruised egos stop the project.

story about managing 


<h5>Good Engineering Practices</h5>



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