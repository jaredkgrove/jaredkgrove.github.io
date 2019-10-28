---
layout: post
title:      "Proactive React"
date:       2019-10-28 16:55:14 +0000
permalink:  proactive_react
---


I didn't take long for me to grow annoyed with rendering views and performing significant DOM manipulation using plain JavaScript. I understood how to do it, but my gut said there had to be a an easier way to render html with JavaScript. React is that better way. As soon as I began putting it to use I saw that it was the tool I'd wished I had when writing plain JS. Excited to devolop and demonstrate my skills with React/Redux, I chose an unconventional project to challenge myself. 

I created web application for creating and saving sketches using Scalable Vector Graphics. Users can select line color, fill color, shapes, line types, and line thickness. Keeping track of settings and sketch data was challenging and the value of Redux became apparent to me. I created currentSketchReducer to display a sketch and allow for editing/updating, sketchesReducer to manage saved sketches without the overhead of all the drawing elements, and sketchSettingsReducer to manage the user's selection of color, shape, and line settings. 

Deciding how to organize components and where to manage state was also challenging. I'm excited to continue to grow in my organizing and apllication of the single responsibility principle, but I'm relatively happy with where I ended up; categorizing my components as views, containers, or components. 

I didn't expect much hoopla on the Rails API side of things, but having many different types (e.g. line, circle, rectangle, polyline) of Elements proved difficult to manage on the back-end. I wanted my sketch to have a single has_many relationship with it's elements, but the data in varied between element types and I didn't want to have all the null values that would have been present utilizing Single Table Inheritance. After a lot of trial and error I ended up with a polymorphic association where an Element belongs to any one of the element type models and a Sketch has_many elements.


I have loved learning React and will be actively looking at jobs where I can continue to use it!
