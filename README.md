# How to test the Optimizations done on this project

1. Go to [My Project](https://gustavolastra.github.io/Website-Optimization/).
2. Paste the links of a page from my project on [Google's PageSpeed tester](https://developers.google.com/speed/pagespeed/insights/).


# Task
![Alt text](img/beforeOne.png?raw=true "Optional Title")

## Goal (Homepage)
![Alt text](img/rubric1.png?raw=true "Optional Title")

# Critical Rendering Path Analysis (Loading Performance)

### Tasks:
1. Document Object Model (DOM)  construction
![Alt text](img/DOM.png?raw=true "Optional Title")
2. CSS Object Model (CSSOM) construction
![Alt text](img/CSSOM.png?raw=true "Optional Title")
3. Render Tree construction
![Alt text](img/RenderTree.png?raw=true "Optional Title")
4. Javascript on the game:
### Worst case scenario:
![Alt text](img/worstcase.png?raw=true "Optional Title")
### Best case scenario:
![Alt text](img/bestcase.png?raw=true "Optional Title")

## PageSpeed recommendations and rules:
### JavaScript- und CSS-Code, die das Rendern blockieren, eliminieren
1. JavaScript-Nutzung optimieren
2. Asynchrone JavaScript-Ressourcen bevorzugen
3. Parsen von JavaScript zurückstellen
4. Lang ausgeführten JavaScript-Code vermeiden
### CSS-Nutzung optimieren
1. CSS in den Dokumentenkopf einfügen
2. CSS-Importe vermeiden
3. Rendering-blockierendes CSS inline einfügen

## Procedure
1. Data compression: Eliminating unnecessary data.
2. Minification: deleting comments and whitespaces.
3. Text compression with Gzip in js, css and html files.
4. link to style with media= "print" so that it does not blocks
5. Images minimized
6. Convert images to binary files.
7. Open Sans asynchronously

## Result
![Alt text](img/pageSpeedOne.png?raw=true "Optional Title")

# Task
![Alt text](img/problem2.png?raw=true "Optional Title")

## Goal (Pizzapage)
![Alt text](img/rubric2.png?raw=true "Optional Title")

## Pixel Pipeline Analysis

![Alt text](img/Pipeline.png?raw=true "Optional Title")
- Javascript: used to handle work that will result in visual changes, whether it’s jQuery’s animate function, sorting a data set, or adding DOM elements to the page.
- Style calculations: process of figuring out which CSS rules apply to which elements based on matching selectors, for example, .headline or .nav > .nav__item.
- Layout: Once the browser knows which rules apply to an element it can begin to calculate how much space it takes up and where it is on screen.
- Paint: process of filling in pixels
- Compositing: Since the parts of the page were drawn into potentially multiple layers they need to be drawn to the screen in the correct order so that the page renders correctly.

## Considerations for JavaScript Execution:

- Avoid setTimeout or setInterval for visual updates; always use requestAnimationFrame instead.
- Move long-running JavaScript off the main thread to Web Workers.
- Use micro-tasks to make DOM changes over several frames.
- Use Chrome DevTools’ Timeline and JavaScript Profiler to assess the impact of JavaScript.

## Considerations for style calculations

- Reduce the complexity of your selectors; use a class-centric methodology like BEM.
- Reduce the number of elements on which style calculation must be calculated.
### Not desired
```
.box:nth-last-child(-n+1) .title {
  /* styles */
}
```
### Desired
```
.final-box-title {
  /* styles */
}
```

### Considerations for Layout

- Layout is normally scoped to the whole document.
- The number of DOM elements will affect performance; you should avoid triggering layout wherever possible.
- Assess layout model performance; new Flexbox is typically faster than older Flexbox or float-based layout models.
The layout cost when using floats on 1,300 boxes. It is, admittedly, a contrived example, because most applications will use a variety of means to position elements.
1. Without Flexbox:
![Alt text](img/noFlexbox.png?raw=true "Optional Title")
2. With Flexbox:
![Alt text](img/yesFlexbox.png?raw=true "Optional Title")
- Avoid forced synchronous layouts and layout thrashing; read style values then make style changes.


### Considerations for Paint

- Changing any property apart from transforms or opacity always triggers paint.
- Paint is often the most expensive part of the pixel pipeline; avoid it where you can.
- Reduce paint areas through layer promotion and orchestration of animations.
- Use the Chrome DevTools paint profiler to assess paint complexity and cost; reduce where you can.

### Considerations for Compositing

- Stick to transform and opacity changes for your animations.
- Promote moving elements with will-change or translateZ.
- Avoid overusing promotion rules; layers require memory and management.

## Procedure
1. Minimize main.js
2. Code optimization

```
/ This for-loop actually creates and appends all of the pizzas when the page loads
var randomPizzas = document.getElementById("randomPizzas");   //Here I add the search to a variable

for (var i = 2; i < 100; i++) {
  var pizzasDiv = randomPizzas;
  pizzasDiv.appendChild(pizzaElementGenerator(i));
}
```
and

```
// Moves the sliding background pizzas based on scroll position
function updatePositions() {
  frame++;
  window.performance.mark("mark_start_frame");

  var items = document.querySelectorAll('.mover'); //Here I add the other searches to more variables
  var itemsLength = items.length;
  var docScrollTop = document.body.scrollTop;


  for (var i = 0; i < itemsLength; i++) {
    var phase = Math.sin((docScrollTop / 1250) + (i % 5));
    items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
  }

  // User Timing API to the rescue again. Seriously, it's worth learning.
  // Super easy to create custom metrics.
  window.performance.mark("mark_end_frame");
  window.performance.measure("measure_frame_duration", "mark_start_frame", "mark_end_frame");
  if (frame % 10 === 0) {
    var timesToUpdatePosition = window.performance.getEntriesByName("measure_frame_duration");
    logAverageFrame(timesToUpdatePosition);
  }
}

```
#### Resize Problem
```
function changePizzaSizes(size) {

 // Here I determine the propper new width depending on the slide position
  var nwidth;
    switch(size) {
      case "1":
        nwidth = 25;
        break;
      case "2":
        nwidth = 33.33;
        break;
      case "3":
        nwidth = 50;
        break;
      default:
        console.log("bug in sizeSwitcher");
    }

  var randomPizzaContainer = document.querySelectorAll(".randomPizzaContainer"); // Here I add another search to another variable.

  var randomPizzaContainerLength = randomPizzaContainer.length;
  for (var i = 0; i < randomPizzaContainerLength; i++) {
    randomPizzaContainer[i].style.width = nwidth + "%";

  }
}
```

## Result
![Alt text](img/after2and3.png?raw=true "Optional Title")

Resize takes less than 5 ms

![Alt text](img/less5.png?raw=true "Optional Title")


# References

1. [Rubric](https://review.udacity.com/#!/rubrics/16/view).
2. [Css inliner](https://www.myintervals.com/emogrifier.php).
3. [Google's Pagespeed tester](https://developers.google.com/speed/pagespeed/insights/).



# Instructions from Udacity

## Website Performance Optimization portfolio project

Your challenge, if you wish to accept it (and we sure hope you will), is to optimize this online portfolio for speed! In particular, optimize the critical rendering path and make this page render as quickly as possible by applying the techniques you've picked up in the [Critical Rendering Path course](https://www.udacity.com/course/ud884).

To get started, check out the repository and inspect the code.

### Getting started

#### Part 1: Optimize PageSpeed Insights score for index.html

Some useful tips to help you get started:

1. Check out the repository
1. To inspect the site on your phone, you can run a local server

  ```bash
  $> cd /path/to/your-project-folder
  $> python -m SimpleHTTPServer 8080
  ```

1. Open a browser and visit localhost:8080
1. Download and install [ngrok](https://ngrok.com/) to the top-level of your project directory to make your local server accessible remotely.

  ``` bash
  $> cd /path/to/your-project-folder
  $> ./ngrok http 8080
  ```

1. Copy the public URL ngrok gives you and try running it through PageSpeed Insights! Optional: [More on integrating ngrok, Grunt and PageSpeed.](http://www.jamescryer.com/2014/06/12/grunt-pagespeed-and-ngrok-locally-testing/)

Profile, optimize, measure... and then lather, rinse, and repeat. Good luck!

#### Part 2: Optimize Frames per Second in pizza.html

To optimize views/pizza.html, you will need to modify views/js/main.js until your frames per second rate is 60 fps or higher. You will find instructive comments in main.js.

You might find the FPS Counter/HUD Display useful in Chrome developer tools described here: [Chrome Dev Tools tips-and-tricks](https://developer.chrome.com/devtools/docs/tips-and-tricks).

### Optimization Tips and Tricks
* [Optimizing Performance](https://developers.google.com/web/fundamentals/performance/ "web performance")
* [Analyzing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp.html "analyzing crp")
* [Optimizing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/optimizing-critical-rendering-path.html "optimize the crp!")
* [Avoiding Rendering Blocking CSS](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-blocking-css.html "render blocking css")
* [Optimizing JavaScript](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/adding-interactivity-with-javascript.html "javascript")
* [Measuring with Navigation Timing](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/measure-crp.html "nav timing api"). We didn't cover the Navigation Timing API in the first two lessons but it's an incredibly useful tool for automated page profiling. I highly recommend reading.
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/eliminate-downloads.html">The fewer the downloads, the better</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer.html">Reduce the size of text</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/image-optimization.html">Optimize images</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching.html">HTTP caching</a>

### Customization with Bootstrap
The portfolio was built on Twitter's <a href="http://getbootstrap.com/">Bootstrap</a> framework. All custom styles are in `dist/css/portfolio.css` in the portfolio repo.

* <a href="http://getbootstrap.com/css/">Bootstrap's CSS Classes</a>
* <a href="http://getbootstrap.com/components/">Bootstrap's Components</a>