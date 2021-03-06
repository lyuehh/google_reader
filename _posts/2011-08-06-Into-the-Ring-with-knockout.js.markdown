---
layout: post
title:  "Into the Ring with knockout.js"
date:   2011-08-06 00:23:01
author: Dan Wellman
categories: program
---

## Into the Ring with knockout.js
### by Dan Wellman
### at 2011-08-06 00:23:01
### original <http://feedproxy.google.com/~r/nettuts/~3/1M4gl3IBKJg/>

<a href="http://rss.buysellads.com/click.php?z=1260013&amp;k=d754f1e9ba63a736ba8ff5ece958f7dd&amp;a=21239&amp;c=609030087"><img src="http://rss.buysellads.com/img.php?z=1260013&amp;k=d754f1e9ba63a736ba8ff5ece958f7dd&amp;a=21239&amp;c=609030087" border="0" alt=""></a><p>In the red corner, weighing in at just 29Kb (uncompressed), is <a href="http://knockoutjs.com/">knockout.js</a>; a pure JavaScript library that simplifies the creation of dynamic user interfaces. Knockout is library agnostic so can easily be used with any of the most popular JavaScript libraries already out there, but it works particularly well with jQuery, and uses jQuery.tmpl as its default templating engine.</p>
<p><span></span></p>
<blockquote>
<p><a href="http://knockoutjs.com/">Knockout</a> is not meant to be a replacement for jQuery.</p>
</blockquote>
<p><a href="http://knockoutjs.com/">Knockout</a> is not meant to be a replacement for jQuery; jQuery is hugely popular, as you all know I’m a huge fan of it myself, and it’s very good at what it does. But it’s difficult to create complex user interfaces using jQuery alone; the bigger the application behind the interface, and the more the user can interact with it, the harder it gets to keep some semblance of order. Event handlers abound, and you quickly end up with literally hundreds of lines of code.</p>
<blockquote>
<p>It’s perfectly possible to build complex and highly dynamic UIs with jQuery alone, but does your project’s budget have the time required to write and debug 800+ lines of code? What about in 6 months time when something needs to change, or be added? This is where knockout comes in.</p>
</blockquote>
<h3>Overview </h3>
<p>In this tutorial we’ll build a simple interface that displays a list of contacts and then allows the visitor to interact with the UI to change how the data is displayed, such as filtering the list, or sorting it. We’ll use knockout as a layer between our data and the page to simplify the creation and management or our UI.</p>
<hr>
<h2>Round 1 – Getting Started</h2>
<p>Knockout uses a View-model-view model architecture. The visible list of contacts we use in this example and the elements on the page that they consist of, can be thought of as a view. The data that is displayed on the page is the model. The view model is a representation of the current state of the UI, a combination of the data and the view which also contains the behavior used to interact with the model and update the view.</p>
<p>Let’s get started by creating the folder structure we’ll need and the basic page that we’ll be working with. Create a new folder called knockout somewhere on your system, then within this folder create three new folders called <code>css</code>, <code>img</code> and <code>js</code>. The <code>css</code> folder will be used to hold the simple style sheet we’ll use, and the <code>img</code> folder the single image. The <code>js</code> folder will contain the script file we create, as well as the libraries we’re dependent on. Initially this folder will need to contain the following files:</p>
<ul>
<li>jquery.tmpl.js</li>
<li>jquery-1.6.2.js</li>
<li>knockout-1.2.1.js</li>
</ul>
<p>Now, in your text editor, create the following basic page:</p>
<pre name="code">&lt;!DOCTYPE html&gt;
&lt;html&gt;
    &lt;head&gt;
        &lt;title&gt;Knockout&lt;/title&gt;
        &lt;link rel=&quot;stylesheet&quot; href=&quot;css/styles.css&quot; /&gt;
    &lt;/head&gt;
    &lt;body&gt;
        &lt;script src=&quot;js/jquery-1.6.2.min.js&quot;&gt;&lt;/script&gt;
        &lt;script src=&quot;js/jquery.tmpl.js&quot;&gt;&lt;/script&gt;
        &lt;script src=&quot;js/knockout-1.2.1.js&quot;&gt;&lt;/script&gt;
        &lt;script src=&quot;js/behavior.js&quot;&gt;&lt;/script&gt;
    &lt;/body&gt;
&lt;/html&gt;</pre>
<p>Save this page as <code>index.html</code> in the root <code>knockout</code> folder. So far, there’s nothing note-worthy here other than the use of HTML5. Although knockout.js is compatible with earlier versions of HTML, the attributes we’ll be adding to our elements are not part of the standard HTML 4.01 standard and the page will therefore be invalid. This is not the case with HTML5, which defines <code>data-*</code> attributes for embedding custom data.</p>
<p>We also use a basic style sheet for this example, but it’s only used for this particular example and is completely arbitrary. As this isn’t a CSS tutorial, I’ll avoid showing it here, but if you’re curious, take a look at the file in the demo.</p>
<h3>The Behavior File </h3>
<p>Next, we can create our behavior file; in a new page in your text editor add the following code:</p>
<pre name="code">(function ($) { var model = [{  name: "John",  address: "1, a road, a town, a county, a postcode",  tel: "1234567890",  site: "www.aurl.com", pic: "/img/john.jpg",  deleteMe: function () { viewModel.people.remove(this); }
    }, {  name: "Jane",  address: "2, a street, a city, a county, a postcode",  tel: "1234567890",  site: "www.aurl.com",  pic: "/img/jane.jpg",  deleteMe: function () { viewModel.people.remove(this); }
    }, {  name: "Fred",  address: "3, an avenue, a village, a county, a postcode",  tel: "1234567890",  site: "www.aurl.com",  pic: "/img/fred.jpg",  deleteMe: function () { viewModel.people.remove(this); }
    }, {  name: "Freda",  address: "4, a street, a suburb, a county, a postcode",  tel: "1234567890",  site: "www.aurl.com",  pic: "/img/jane.jpg",  deleteMe: function () { viewModel.people.remove(this); }
    }], viewModel = { people: ko.observableArray(model),
    }
  }; ko.applyBindings(viewModel);

})(jQuery);</pre>
<p>Save this file as <code>behavior.js</code> in the <code>js</code> folder. We start out by defining a self-invoking function, which we pass jQuery into in order to alias the <code>$</code> character.</p>
<p>We then define the model that we’ll use. In this example it’s a local array, but we could get exactly the same format of data from a web service easily enough. Our <code>array</code> contains a series of people <code>object</code>s, which correspond to individual entries in a <code>contacts</code> database. Mostly, our data consists of simple strings, but each <code>object</code> also contains a <code>deleteMe</code> <code>method</code>, which is used to remove the <code>object</code> from the <code>viewModel</code>.</p>
<p>Remember, the <code>viewModel</code> refers to the current state of the UI. It’s an object, and the first item we add to it is our <code>array</code> containing the people objects. We use the knockout <code>ko.observableArray()</code> <code>method</code> to add our <code>array</code> to the <code>viewModel</code> <code>object</code>. Observables are a fundamental aspect of knockout.js; we are instructing knockout to allow other entities to observe these items and react when they change.</p>
<p>This is all our view Model contains at the moment, although we’ve left a hanging comma after the people property’s value for when we add more properties.</p>
<p>After the <code>object</code> <code>object</code>, we use the <code>ko.applyBindings()</code> <code>method</code> to apply any bindings we’ve created and begin managing the <code>viewModel</code>. At this point in the example, we haven’t yet added any bindings. To create bindings between our <code>view</code> and <code>viewModel</code>, we need to add some more HTML.</p>
<hr>
<h2>Round 2 – Creating a View</h2>
<blockquote>
<p>Knockout works brilliantly with jQuery templating.</p>
</blockquote>
<p>We now have our <code>model</code> and a simple <code>viewModel</code> in place. The next thing we should do is display the data from the <code>viewModel</code> on the page. Knockout works brilliantly with jQuery templating. This allows us to use the <a href="http://api.jquery.com/jquery.tmpl/">tmpl</a> plugin to build the required HTML. Add the following code to the <code>&lt;body&gt;</code> element of the page, directly before the <code>&lt;script&gt;</code> elements:</p>
<pre name="code">&lt;div id=&quot;people&quot; data-bind=&quot;template: { name: &#39;personTemplate&#39;, foreach: people }&quot;&gt;
&lt;/div&gt;
&lt;script id=&quot;personTemplate&quot; type=&quot;text/x-jquery-tmpl&quot;&gt;
    &lt;section class=&quot;person&quot;&gt;
        &lt;img src=&quot;../img/person.png&quot; alt=&quot;${ name }&quot; /&gt;
        &lt;h1&gt;${ name }&lt;/h1&gt;
        &lt;address&gt;${ address }&lt;/address&gt;
        &lt;span class=&quot;tel&quot;&gt;${ tel }&lt;/span&gt;
        &lt;a href=&quot;http://${ site }&quot; title=&quot;Visit site&quot;&gt;${ site }&lt;/a&gt;
        &lt;div class=&quot;tools&quot;&gt;
            &lt;button data-bind=&quot;click: deleteMe&quot;&gt;Delete&lt;/button&gt;
        &lt;/div&gt;
    &lt;/section&gt;
&lt;/script&gt;</pre>
<p>We first add an empty <code>&lt;div&gt;</code> element with an <code>id</code> – mostly for styling purposes. This element also has a special attribute – <code>data-bind</code>. This attribute tells knockout that the element stores its data in the <code>viewModel</code>. When we called <code>ko.applyBindings()</code> in our JS, this is a binding that is applied. In this case, we use the template binding which allows us to specify the name of a template that we’d like to use in a configuration object passed to the binding.</p>
<p>We also use the <code>foreach</code> property in this configuration object and specify the name of our people <code>observableArray</code> as the source of our data. We could use the standard tmpl syntax, <code>{{each}}</code>, to iterate over our people data, but it is more efficient to use knockout’s syntax instead. Because our people data is contained within an observable <code>array</code>, knockout will monitor the <code>array</code> for changes, and, when any occur, it will automatically update any templates that are displaying the data. If we use tmpl syntax, our entire template will be re-rendered each time the data changes, but when we use knockout’s <code>foreach</code> property, only the single instance corresponding to the item that has changed is re-rendered.</p>
<p>Following the container<code> &lt;div&gt;</code> we then define our template. This is done in the same way as a normal tmpl template. Within the template, we specify the elements that we would like repeated for each object in our data source. We have a <code>&lt;section&gt;</code> element as a container, followed by an appropriate element for each item within <code>person</code> <code>object</code>. One thing to note is that we can supply bindings in our template code. We add a <code>data-bind</code> attribute to a delete button; this time we use the <code>click</code> binding and specify the name of the <code>person</code> found within each <code>person</code> <code>object</code>.</p>
<p>When we run the page in a browser, we should find that our page contains the data from our <code>viewModel</code>, nicely rendered using our template:</p>
<div>
   <img src="http://d2o0t5hpnwv4c1.cloudfront.net/1027_backbone/1.png">
</div>
<p>So that’s pretty cool right? But it’s not that dissimilar to using the tmpl plugin. </p>
<blockquote>
<p>The really cool thing is that not only is the <code>view</code> updated accordingly when the <code>viewModel</code> changes, the <code>viewModel</code> is also updated when the view changes. So if we click one of the delete buttons on our page, the <code>people</code> <code>array</code> will also have the corresponding <code>person</code> <code>object</code> removed from it!
</p>
</blockquote>
<p>The original <code>array</code> which we passed into the <code>ko.observable()</code> <code>method</code> isn’t actually updated, but normally, we’d probably get our data from an AJAX request instead of hard-coding it into the page, so all we’d need to do is resubmit the data, with the <code>person</code> removed.</p>
<hr>
<h2>Round 3 – Adding new Data</h2>
<p>We’ve got the ability to remove a <code>person</code> <code>object</code>; next, we can add the ability to add a new person into our <code>dataModel</code>; Update the container <code>&lt;div&gt;</code> we added to the page earlier so that it contains the following new elements:</p>
<pre name="code">&lt;a href=&quot;#&quot; title=&quot;Add new person&quot; data-bind=&quot;click: showForm, visible: displayButton&quot;&gt;Add person&lt;/a&gt;
&lt;fieldset data-bind=&quot;visible: displayForm&quot;&gt;
    &lt;div class=&quot;details&quot;&gt;
        &lt;label&gt;Name: &lt;input id=&quot;name&quot; /&gt;&lt;/label&gt;
        &lt;label&gt;Address: &lt;input id=&quot;address&quot; /&gt;&lt;/label&gt;
        &lt;label&gt;Tel: &lt;input id=&quot;tel&quot; /&gt;&lt;/label&gt;
        &lt;label&gt;Site: &lt;input id=&quot;site&quot; /&gt;&lt;/label&gt;
    &lt;div&gt;
    &lt;div class=&quot;img&quot;&gt;
        &lt;label&gt;Picture: &lt;input id=&quot;pic&quot; type=&quot;file&quot; /&gt;&lt;/label&gt;
    &lt;/div&gt;
    &lt;div class=&quot;tools&quot;&gt;
        &lt;button data-bind=&quot;click: addPerson&quot;&gt;Add&lt;/button&gt;
        &lt;button data-bind=&quot;click: hideForm&quot;&gt;Cancel&lt;/button&gt;
    &lt;/div&gt;
&lt;/fieldset&gt;</pre>
<p>The first new element we add is an<code> &lt;a&gt;</code> tag, which is used to open up the form that will accept the new data. This is similar to how we’d do it in a regular jQuery implementation, except that we’d also have to add an event handler to listen for clicks on the element, and do things such as stopping the event. With knockout, we don’t have to worry about any of that. All we need to do is specify the name of a <code>method</code> within our <code>viewModel</code>, which we’d like to execute whenever the element is clicked. Knockout will attach the handler and stop the link being followed for us.</p>
<p>As you can see, we can specify multiple bindings on an element. Our <code>&lt;a&gt;</code> element also uses the visible binding. Again, we specify a property of our <code>viewModel</code>, except that this time, it isn’t a function but a simple variable containing a <code>boolean</code>; you’ll see how this works when we come to add the JS for our new functionality in a moment.</p>
<p>After the link, we also add a <code>&lt;fieldset&gt;</code> containing labels and inputs that we can use to add the relevant data to make a new <code>object</code> in our <code>people</code> <code>array</code>. At the end of our new HTML, we add two new <code>&lt;button&gt;</code> elements; both of these have click bindings added to them. The first links to the <code>addPerson</code> <code>method</code>, the second to the <code>hideForm</code> <code>method</code>. The image uploading doesn’t actually work in this example, it’s only there for show.</p>
<p>Now let’s take a look at the new JavaScript we need; add the following code directly after the <code>people</code> property of our <code>viewModel</code> (we left a hanging comma ready to add these new properties and methods):</p>
<pre name="code">displayButton: ko.observable(true), displayForm: ko.observable(false), showForm: function () { viewModel.displayForm(true).displayButton(false);
}, hideForm: function () { viewModel.displayForm(false).displayButton(true);
}, addPerson: function () { viewModel.displayForm(false).displayButton(true).people.push({ name: $("#name").val(), address: $("#address").val(), tel: $("#tel").val(), site: $("#site").val(), pic: "", deleteMe: function () { viewModel.people.remove(this); }
    });
}</pre>
<p>The first property is <code>displayButton</code>, which is an observable property (its value may be observed) by other entities. The entity that observes its value is our <code>&lt;a&gt;</code> element in the view. We initially set it to <code>true</code>, so when the page loads (or rather when the <code>applyBindings()</code> method is called), the link will be visible.</p>
<p>The next property is called <code>displayForm</code>, which is also an observable, except, this time, we set it to <code>false</code>, so the element in our view which is observing it (the <code>fieldset</code>) will initially be hidden.</p>
<p>We then add two methods: <code>showForm()</code> and <code>hideForm()</code>. These two simple methods are used to, obviously, show or hide the form respectively, and to do that, all they need to do is set the <code>displayForm</code> observable property to <code>true</code> or <code>false</code>. Because the value is being observed, any time their value changes, our view will be updated automatically.</p>
<p>We also adjust the <code>showButton</code> property whenever the state of the form changes. If the <code>fieldset</code> is visible, we hide the link, and if we hide the <code>fieldset</code>, the button is made visible again. As you can see, knockout supports chaining, which makes updating multiple properties in our <code>viewModel</code> extremely easy. The view should appear like this when the form is visible:</p>
<div>
   <img src="http://d2o0t5hpnwv4c1.cloudfront.net/1027_backbone/2.png">
</div>
<p>The last method we add is the <code>addPerson()</code> <code>method</code>, which is used to update our <code>viewModel</code>  with the details of the new person.  All we do in this method is hide the form and show the button, and create an object literal containing the values entered into the text fields and then push this <code>object</code> into our <code>people</code> <code>array</code>.</p>
<p>To retrieve the updated <code>people array</code> from our <code>viewModel</code>, we can use knockout’s built in JSON serialiser to write the observable <code>array</code> to a JSON <code>object</code>. We would normally do this in order to pass the data back to the server, but, to test it out, we could add this line of code to the end of the <code>addPerson()</code> <code>method</code>:</p>
<pre name="code">console.log(ko.toJSON(viewModel.people));</pre>
<p>The <code>ko.toJSON()</code> <code>method</code> helpfully generates a JSON <code>object</code> containing the current contents of the <code>people</code> <code>array</code>, which we can see in Firebug (other DOM explorers are available):</p>
<div>
   <img src="http://d2o0t5hpnwv4c1.cloudfront.net/1027_backbone/3.png">
</div>
<hr>
<h2>Post Fight Review</h2>
<blockquote><p>In this tutorial, we covered two major aspects of knockout.js – declarative bindings and observables. </p>
</blockquote>
<p>The bindings are applied in our HTML and specify properties and arrays of data whose values should be observed for changes. When these values do change, the elements in the view that are observing them will be updated automatically, either by applying a new iteration of a template, or by showing or hiding an element, as in this example.</p>
<p>There are other bindings we can use as well to perform different actions when the view is interacted with, or the data in the <code>viewModel</code> is updated.</p>
<p>Knockout.js is an extremely helpful layer that sits between our UI’s interface and its underlying data, and manages interactions and state changes for us. It does so much work for us, although we’ve really only scratched the surface of what it is capable of in this basic example. What are your thoughts on knockout.js?</p><p><iframe src="http://feedads.g.doubleclick.net/~ah/f/8olmjno1k05rb1som1frr6u854/300/250?ca=1&amp;fh=280#http%3A%2F%2Fnet.tutsplus.com%2Ftutorials%2Fjavascript-ajax%2Finto-the-ring-with-knockout-js%2F" width="100%" height="280" frameborder="0" scrolling="no" marginwidth="0" marginheight="0"></iframe></p><div>
<a href="http://feeds.feedburner.com/~ff/nettuts?a=1M4gl3IBKJg:onPO1Qfn0rY:yIl2AUoC8zA"><img src="http://feeds.feedburner.com/~ff/nettuts?d=yIl2AUoC8zA" border="0"></a> <a href="http://feeds.feedburner.com/~ff/nettuts?a=1M4gl3IBKJg:onPO1Qfn0rY:F7zBnMyn0Lo"><img src="http://feeds.feedburner.com/~ff/nettuts?i=1M4gl3IBKJg:onPO1Qfn0rY:F7zBnMyn0Lo" border="0"></a> <a href="http://feeds.feedburner.com/~ff/nettuts?a=1M4gl3IBKJg:onPO1Qfn0rY:V_sGLiPBpWU"><img src="http://feeds.feedburner.com/~ff/nettuts?i=1M4gl3IBKJg:onPO1Qfn0rY:V_sGLiPBpWU" border="0"></a> <a href="http://feeds.feedburner.com/~ff/nettuts?a=1M4gl3IBKJg:onPO1Qfn0rY:gIN9vFwOqvQ"><img src="http://feeds.feedburner.com/~ff/nettuts?i=1M4gl3IBKJg:onPO1Qfn0rY:gIN9vFwOqvQ" border="0"></a> <a href="http://feeds.feedburner.com/~ff/nettuts?a=1M4gl3IBKJg:onPO1Qfn0rY:TzevzKxY174"><img src="http://feeds.feedburner.com/~ff/nettuts?d=TzevzKxY174" border="0"></a>
</div><img src="http://feeds.feedburner.com/~r/nettuts/~4/1M4gl3IBKJg" height="1" width="1">