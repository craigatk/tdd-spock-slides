# TDD/Spock slides

## Slides

A PDF of the slides is [available here](https://github.com/craigatk/tdd-spock-slides/blob/master/tdd-spock-slides.pdf)

## Presentation Setup

1) Install [Node](http://nodejs.org/download/)

2) Install [Grunt](http://gruntjs.com/getting-started#installing-the-cli)

3) Install dependencies with ```npm install```

## Running Presentation

```grunt serve```

The slides will be available at [http://localhost:8000](http://localhost:8000)

## Content

The presentation is based on the [Reveal.js framework](https://github.com/hakimel/reveal.js/). Slides are in Markdown in ```slides.md```

## Generating PDF

To generate a PDF of the slides:

1) Run the presentation with ```grunt serve```

2) Open the presentation in Chrome

3) Append ?print-pdf to the URL [http://localhost:8000?print-pdf](http://localhost:8000?print-pdf)

4) Print to PDF from Chrome