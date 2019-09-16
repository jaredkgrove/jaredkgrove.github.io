---
layout: post
title:      "Making Fetch Happen"
date:       2019-09-16 13:30:25 -0400
permalink:  making_fetch_happen
---


I had been proud of my rails project for organizing pick-up-games of basketball and I was excited to expand on the application with JavaScript. I chose to use vanilla JS and fetch for my api requests. Admittedly, this turned out to be one of the more difficult projects me. The code necessary to meet the project requirement wasn't particularly challenging; I knew how to make an api call, it isn't too difficult to convert the response to JSON and create JS objects with that JSON, and basic DOM manipulation with JS made sense. What I struggled to grasp was *when* and *how* I would actually want to use AJAX when I'm not specifically being prompted to do so. This is an ongoing question, but here are some things I've learned so far in no particular order. ***Warning: I still have more questions than answers

### Form Submissions

This was probably the easiest *why* for me to grasp. It makes perfect sense sense that reloading a page with dozens (or hundreds) of html elements in order to render my new comment or display my new 'like' is doing too much. Appending a new <li> or altering the text in an existing element is easy to understand and simple to perform. But what about when my form submission renders an entirely new view? 

### Creating Templates with JS

My solution to creating entirely new views with JS was to create templates of html in my js classes that defined the view's wrappers and static elements (filling in page specific information with template literals). Then tags displaying 'has many' relationships (and other variable data) were added dynamically based on the response of the AJAX request. I'm not totally satisfied with this approach. I like it for my simple views. For example, here is the JS to initialize my Courts index view:
```
const initializeCourtsIndexView = () => {
setBodyClass("courts index")
let main = document.querySelector('main')
main.innerHTML = `
<h1> Courts </h1>
<div class='courts-list'></div>
` 
}
```

And the code to add elements representing each court:

```
        addCourtWrapper = () => {
            let wrapper = createElement('a', {class: "content-box js-court", href: "#", court_id: this.id})
            let courtNameTag = createElement('h3',{}, this.name)
            wrapper.appendChild(courtNameTag)
            wrapper.innerHTML += `${this.location} <br> ${this.upcoming_game_count} upcoming games`
            this.addClickListener(wrapper)
            courtsWrapper.appendChild(wrapper)
        }
```

But when the views got more complex, it became easy to see how this approach could be a big pain if I ever wanted to make significant update to my view. Here is the initialization of a court show page rendered using JS:

```
        initializeCourtShowView = () => {
            setBodyClass("courts show")
            let main = document.querySelector('main')
            main.innerHTML = 
            `
                <h1 id='js-court-name'>${this.name}</h1>
                <div class="court-info">
                    <h3 id='js-court-location'>${this.location}</h3>
                    <div class="favorite">
                        <form class="button_to" method="post" action="/courts/${this.id}">
                            <input type="hidden" name="_method" value="patch">
                            <input type="submit" value="${currentUser.isFavoriteCourt(this.id) ? "REMOVE FROM FAVORITES" : "ADD TO FAVORITES"}">
                            <input type="hidden" name="authenticity_token" value="${token}">
                        </form>
                        <h3 id='js-favorite-count'>Favorited by ${this.favorite_count} Players</h3>
                    </div>
            
                    <div class="content-box">
                        <h2>Upcoming Games</h2>
                        <div class="games_list"></div>
                    </div>
                    <h2>Create New Game</h2>
                    <form class="new-game" action="/courts/${this.id}/games" accept-charset="UTF-8" method="post">
                        <input name="utf8" type="hidden" value="✓">
                        <input type="hidden" name="authenticity_token" value="${token}">
                        <input type="hidden" name="court_id" id="game_court_id" value="${this.id}">
                        Skill Level <br>
                        <select name="skill_level" id="game_skill_level">
                            <option selected="selected" value="Everyone Welcome">Everyone Welcome</option>
                            <option value="Weekend Hustlers">Weekend Hustlers</option>
                            <option value="Offseason Reps">Offseason Reps</option>
                            <option value="Run the Court (Girls)">Run the Court (Girls)</option>
                            <option value="Still Got It">Still Got It</option></select> <br>
                        Time <br> 
                        <input type="datetime-local" name="time" id="game_time" value="2019-09-13T14:57"> <br>
                        <input type="submit" name="commit" value="Create Game" data-disable-with="Create Game">
                    </form>
                </div>`
        }
```

Yuck. And this is still a simple view structure. Discovering a better solution is still on my to-do list, but my gut says I shouldn't have to write massive amounts of HTML in JS. 

### Populating Templates with JS

Another possibility is to load a page containing an HTML template with a traditional HTTP request and then populate the view with resource specific information from an API request. The obvious advantage is that I get to write my html in a .html file, but why not just populate all the info on the server side? Because... well that's what I'm still figuring out. One reason that comes to mind if that if the content of the displayed resource needs to be frequently updated and refreshed.

Though I feel less satisfied with my end product than I did for other projects, I still learned a ton, grew much more comfortable with JS, and have added some valuable items to my list for ongoing learning.



