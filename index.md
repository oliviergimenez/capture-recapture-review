---
title: rCharts NYT Interactive Home Price
author: Timely Portfolio  (all credit to NYTimes)
github: {user: timelyportfolio, repo: rCharts_nyt_home_price, branch: "gh-pages"}
framework: bootstrap
mode: selfcontained
widgets: "nyt_home"
highlighter: prettify
hitheme: twitter-bootstrap
assets:
  css:
    - "http://fonts.googleapis.com/css?family=Raleway:300"
    - "http://fonts.googleapis.com/css?family=Oxygen"
---

<style>
body{
  font-family: 'Oxygen', sans-serif;
  font-size: 16px;
  line-height: 24px;
}

h1,h2,h3,h4 {
  font-family: 'Raleway', sans-serif;
}

.container { width: 1000px; }
h3 {
  background-color: #D4DAEC;
  text-indent: 100px; 
}
h4 {
  text-indent: 100px;
}

g-table-intro h4 {
  text-indent: 0px;
}
</style>

<a href="https://github.com/timelyportfolio/rCharts_nyt_home_price"><img style="position: absolute; top: 0; right: 0; border: 0;" src="https://s3.amazonaws.com/github/ribbons/forkme_right_darkblue_121621.png" alt="Fork me on GitHub"></a>

# Great NYT Interactive -- Now Reusable with rCharts

---
<br/>
### Disclaimer and Attribution

**I claim absolutely no credit for this visualization, which I consider one of the most best I have ever seen.  All credit belongs to the <a href = "http://www.nytimes.com/interactive/2011/05/31/business/economy/case-shiller-index.html">original source</a>.  If anybody believes this to be not fair use, I will take it down immediately.  I am implicitly assuming approval for this fork due to the <a href = "http://datastori.es/data-stories-22-nyt-graphics-and-d3-with-mike-bostock-and-shan-carter/">data.stories interview</a>.** 




---
<br/>
### Another Favorite from NYT
  
I think we all know the [data visualization team at NYT](http://blog.visual.ly/10-things-you-can-learn-from-the-new-york-times-data-visualizations/) is simply amazing.  Earlier this year in my post [d3 <- R with rCharts and slidify](http://timelyportfolio.blogspot.com/2013/04/d3-r-with-rcharts-and-slidify.html) I adapted and recreated the [512 Paths to the White House](http://www.nytimes.com/interactive/2012/11/02/us/politics/paths-to-the-white-house.html) to work with `R` data through [`rCharts`](http://rcharts.io/site).  Unfortunately, I was not creative enough to think of other data sets to plug into the visualization.  When Scott Murray tweeted, 

<br/>
<blockquote class="twitter-tweet"><p>Over at <a href="https://twitter.com/nytgraphics">@nytgraphics</a>, <a href="https://twitter.com/KevinQ">@KevinQ</a> and <a href="https://twitter.com/shancarter">@shancarter</a> really know how to wiggle a baseline. <a href="http://t.co/gS9gHrSLIu">http://t.co/gS9gHrSLIu</a></p>&mdash; Scott Murray (@alignedleft) <a href="https://twitter.com/alignedleft/statuses/349647895122804738">June 25, 2013</a></blockquote>
<script async src="http://platform.twitter.com/widgets.js" charset="utf-8"></script>
<br/>

I immediately knew the [Case Shiller Home Price Index visualization](http://www.nytimes.com/interactive/2011/05/31/business/economy/case-shiller-index.html) would be perfect for reuse with any cumulative growth time series data.   This is a bit of a hack of `rCharts` and should not be considered best practices, but it is a demonstration of the very flexible design of the package.  In the spirit of this [discussion](http://datastori.es/data-stories-23-inspiration-or-plagiarism/), I did not want to just copy entirely.  I was able to add a couple key innovations to the visualization:

- Generalize the d3 code a little more to adapt to the data
- Build in `R` with `rCharts` to make it reusable.


---
<br/>
### Reusable Version in rCharts
As I mentioned above, this visualization works well with any cumulative growth time series, so let's apply it to the `managers` dataset supplied by the `PerformanceAnalytics` package.




<h4>Get Data and Transform</h4>

```r
#get the data and convert to a format that we would expect from melted xts
#will be typical
#also original only uses a single value (val) and not other 
require(reshape2)
require(PerformanceAnalytics)

data(managers)
managers <- na.omit(managers)
managers.melt <- melt(
  data.frame( index( managers ), coredata(cumprod( managers+1 )*100 ) ),
  id.vars = 1
)
colnames(managers.melt) <- c("date", "manager","val")
managers.melt[,"date"] <- format(managers.melt[,"date"],format = "%Y-%m-%d")
```


<h4>Draw The Graph</h4>

```r
require(rCharts)
p2 <- rCharts$new()
p2$setLib('libraries/widgets/nyt_home')
p2$setTemplate(script = "libraries/widgets/nyt_home/layouts/nyt_home.html")

p2$set(
  description = "This data comes from the managers dataset included in the R package PerformanceAnalytics.",
  data = managers.melt,
  groups = "manager"
)
cat(noquote(p2$html()))
```

<!--Attribution:NYT Interactive http://www.nytimes.com/interactive/2011/05/31/business/economy/case-shiller-index.html-->

<div id="interactiveFreeFormMain">
<div class="g-shell">
<div class="g-tooltip">
<span class="g-playername-tooltip"></span>
<span class="g-dltype-tooltip"></span>
<span class="g-salary-tooltip"></span>
</div>
<h5 class="g-intro-sentence">
If you bought
<span class="g-selector g-city-selector"></span>
around
<span class="g-selector g-month-selector"></span>
<span class="g-selector g-date-selector"></span>
it would be worth
<span class="g-answer">24 percent less</span> today.
</h5>
<div class="g-chart"></div>
<div class="g-table-container">
<div class="g-table-intro">
<h4 style = "text-indent:0px;font-family:arial,sans-serif;">Behind the data </h4>
<p class="g-table-readin"></p>
</div>
<table class="g-table">
<tr>
<th class="g-proper-city"></th>
<th class="g-yoy-chg" colspan="2">Year-over-year change</th>
<th class="g-peak-month-td" colspan="2">Since peak</th>
<th class="g-change-since-selected" colspan="2"> <span class="g-date-compare">Since Jan. 2000</span></th>
</tr>
</table>
</div>
</div>


<script>
(function() {
  var params = {
 "dom": "chart4f86c8f743c",
"width":    800,
"height":    400,
"description": "This data comes from the managers dataset included in the R package PerformanceAnalytics.",
"data": {
 "date": [ "2001-09-30", "2001-10-31", "2001-11-30", "2001-12-31", "2002-01-31", "2002-02-28", "2002-03-31", "2002-04-30", "2002-05-31", "2002-06-30", "2002-07-31", "2002-08-31", "2002-09-30", "2002-10-31", "2002-11-30", "2002-12-31", "2003-01-31", "2003-02-28", "2003-03-31", "2003-04-30", "2003-05-31", "2003-06-30", "2003-07-31", "2003-08-31", "2003-09-30", "2003-10-31", "2003-11-30", "2003-12-31", "2004-01-31", "2004-02-29", "2004-03-31", "2004-04-30", "2004-05-31", "2004-06-30", "2004-07-31", "2004-08-31", "2004-09-30", "2004-10-31", "2004-11-30", "2004-12-31", "2005-01-31", "2005-02-28", "2005-03-31", "2005-04-30", "2005-05-31", "2005-06-30", "2005-07-31", "2005-08-31", "2005-09-30", "2005-10-31", "2005-11-30", "2005-12-31", "2006-01-31", "2006-02-28", "2006-03-31", "2006-04-30", "2006-05-31", "2006-06-30", "2006-07-31", "2006-08-31", "2006-09-30", "2006-10-31", "2006-11-30", "2006-12-31", "2001-09-30", "2001-10-31", "2001-11-30", "2001-12-31", "2002-01-31", "2002-02-28", "2002-03-31", "2002-04-30", "2002-05-31", "2002-06-30", "2002-07-31", "2002-08-31", "2002-09-30", "2002-10-31", "2002-11-30", "2002-12-31", "2003-01-31", "2003-02-28", "2003-03-31", "2003-04-30", "2003-05-31", "2003-06-30", "2003-07-31", "2003-08-31", "2003-09-30", "2003-10-31", "2003-11-30", "2003-12-31", "2004-01-31", "2004-02-29", "2004-03-31", "2004-04-30", "2004-05-31", "2004-06-30", "2004-07-31", "2004-08-31", "2004-09-30", "2004-10-31", "2004-11-30", "2004-12-31", "2005-01-31", "2005-02-28", "2005-03-31", "2005-04-30", "2005-05-31", "2005-06-30", "2005-07-31", "2005-08-31", "2005-09-30", "2005-10-31", "2005-11-30", "2005-12-31", "2006-01-31", "2006-02-28", "2006-03-31", "2006-04-30", "2006-05-31", "2006-06-30", "2006-07-31", "2006-08-31", "2006-09-30", "2006-10-31", "2006-11-30", "2006-12-31", "2001-09-30", "2001-10-31", "2001-11-30", "2001-12-31", "2002-01-31", "2002-02-28", "2002-03-31", "2002-04-30", "2002-05-31", "2002-06-30", "2002-07-31", "2002-08-31", "2002-09-30", "2002-10-31", "2002-11-30", "2002-12-31", "2003-01-31", "2003-02-28", "2003-03-31", "2003-04-30", "2003-05-31", "2003-06-30", "2003-07-31", "2003-08-31", "2003-09-30", "2003-10-31", "2003-11-30", "2003-12-31", "2004-01-31", "2004-02-29", "2004-03-31", "2004-04-30", "2004-05-31", "2004-06-30", "2004-07-31", "2004-08-31", "2004-09-30", "2004-10-31", "2004-11-30", "2004-12-31", "2005-01-31", "2005-02-28", "2005-03-31", "2005-04-30", "2005-05-31", "2005-06-30", "2005-07-31", "2005-08-31", "2005-09-30", "2005-10-31", "2005-11-30", "2005-12-31", "2006-01-31", "2006-02-28", "2006-03-31", "2006-04-30", "2006-05-31", "2006-06-30", "2006-07-31", "2006-08-31", "2006-09-30", "2006-10-31", "2006-11-30", "2006-12-31", "2001-09-30", "2001-10-31", "2001-11-30", "2001-12-31", "2002-01-31", "2002-02-28", "2002-03-31", "2002-04-30", "2002-05-31", "2002-06-30", "2002-07-31", "2002-08-31", "2002-09-30", "2002-10-31", "2002-11-30", "2002-12-31", "2003-01-31", "2003-02-28", "2003-03-31", "2003-04-30", "2003-05-31", "2003-06-30", "2003-07-31", "2003-08-31", "2003-09-30", "2003-10-31", "2003-11-30", "2003-12-31", "2004-01-31", "2004-02-29", "2004-03-31", "2004-04-30", "2004-05-31", "2004-06-30", "2004-07-31", "2004-08-31", "2004-09-30", "2004-10-31", "2004-11-30", "2004-12-31", "2005-01-31", "2005-02-28", "2005-03-31", "2005-04-30", "2005-05-31", "2005-06-30", "2005-07-31", "2005-08-31", "2005-09-30", "2005-10-31", "2005-11-30", "2005-12-31", "2006-01-31", "2006-02-28", "2006-03-31", "2006-04-30", "2006-05-31", "2006-06-30", "2006-07-31", "2006-08-31", "2006-09-30", "2006-10-31", "2006-11-30", "2006-12-31", "2001-09-30", "2001-10-31", "2001-11-30", "2001-12-31", "2002-01-31", "2002-02-28", "2002-03-31", "2002-04-30", "2002-05-31", "2002-06-30", "2002-07-31", "2002-08-31", "2002-09-30", "2002-10-31", "2002-11-30", "2002-12-31", "2003-01-31", "2003-02-28", "2003-03-31", "2003-04-30", "2003-05-31", "2003-06-30", "2003-07-31", "2003-08-31", "2003-09-30", "2003-10-31", "2003-11-30", "2003-12-31", "2004-01-31", "2004-02-29", "2004-03-31", "2004-04-30", "2004-05-31", "2004-06-30", "2004-07-31", "2004-08-31", "2004-09-30", "2004-10-31", "2004-11-30", "2004-12-31", "2005-01-31", "2005-02-28", "2005-03-31", "2005-04-30", "2005-05-31", "2005-06-30", "2005-07-31", "2005-08-31", "2005-09-30", "2005-10-31", "2005-11-30", "2005-12-31", "2006-01-31", "2006-02-28", "2006-03-31", "2006-04-30", "2006-05-31", "2006-06-30", "2006-07-31", "2006-08-31", "2006-09-30", "2006-10-31", "2006-11-30", "2006-12-31", "2001-09-30", "2001-10-31", "2001-11-30", "2001-12-31", "2002-01-31", "2002-02-28", "2002-03-31", "2002-04-30", "2002-05-31", "2002-06-30", "2002-07-31", "2002-08-31", "2002-09-30", "2002-10-31", "2002-11-30", "2002-12-31", "2003-01-31", "2003-02-28", "2003-03-31", "2003-04-30", "2003-05-31", "2003-06-30", "2003-07-31", "2003-08-31", "2003-09-30", "2003-10-31", "2003-11-30", "2003-12-31", "2004-01-31", "2004-02-29", "2004-03-31", "2004-04-30", "2004-05-31", "2004-06-30", "2004-07-31", "2004-08-31", "2004-09-30", "2004-10-31", "2004-11-30", "2004-12-31", "2005-01-31", "2005-02-28", "2005-03-31", "2005-04-30", "2005-05-31", "2005-06-30", "2005-07-31", "2005-08-31", "2005-09-30", "2005-10-31", "2005-11-30", "2005-12-31", "2006-01-31", "2006-02-28", "2006-03-31", "2006-04-30", "2006-05-31", "2006-06-30", "2006-07-31", "2006-08-31", "2006-09-30", "2006-10-31", "2006-11-30", "2006-12-31", "2001-09-30", "2001-10-31", "2001-11-30", "2001-12-31", "2002-01-31", "2002-02-28", "2002-03-31", "2002-04-30", "2002-05-31", "2002-06-30", "2002-07-31", "2002-08-31", "2002-09-30", "2002-10-31", "2002-11-30", "2002-12-31", "2003-01-31", "2003-02-28", "2003-03-31", "2003-04-30", "2003-05-31", "2003-06-30", "2003-07-31", "2003-08-31", "2003-09-30", "2003-10-31", "2003-11-30", "2003-12-31", "2004-01-31", "2004-02-29", "2004-03-31", "2004-04-30", "2004-05-31", "2004-06-30", "2004-07-31", "2004-08-31", "2004-09-30", "2004-10-31", "2004-11-30", "2004-12-31", "2005-01-31", "2005-02-28", "2005-03-31", "2005-04-30", "2005-05-31", "2005-06-30", "2005-07-31", "2005-08-31", "2005-09-30", "2005-10-31", "2005-11-30", "2005-12-31", "2006-01-31", "2006-02-28", "2006-03-31", "2006-04-30", "2006-05-31", "2006-06-30", "2006-07-31", "2006-08-31", "2006-09-30", "2006-10-31", "2006-11-30", "2006-12-31", "2001-09-30", "2001-10-31", "2001-11-30", "2001-12-31", "2002-01-31", "2002-02-28", "2002-03-31", "2002-04-30", "2002-05-31", "2002-06-30", "2002-07-31", "2002-08-31", "2002-09-30", "2002-10-31", "2002-11-30", "2002-12-31", "2003-01-31", "2003-02-28", "2003-03-31", "2003-04-30", "2003-05-31", "2003-06-30", "2003-07-31", "2003-08-31", "2003-09-30", "2003-10-31", "2003-11-30", "2003-12-31", "2004-01-31", "2004-02-29", "2004-03-31", "2004-04-30", "2004-05-31", "2004-06-30", "2004-07-31", "2004-08-31", "2004-09-30", "2004-10-31", "2004-11-30", "2004-12-31", "2005-01-31", "2005-02-28", "2005-03-31", "2005-04-30", "2005-05-31", "2005-06-30", "2005-07-31", "2005-08-31", "2005-09-30", "2005-10-31", "2005-11-30", "2005-12-31", "2006-01-31", "2006-02-28", "2006-03-31", "2006-04-30", "2006-05-31", "2006-06-30", "2006-07-31", "2006-08-31", "2006-09-30", "2006-10-31", "2006-11-30", "2006-12-31", "2001-09-30", "2001-10-31", "2001-11-30", "2001-12-31", "2002-01-31", "2002-02-28", "2002-03-31", "2002-04-30", "2002-05-31", "2002-06-30", "2002-07-31", "2002-08-31", "2002-09-30", "2002-10-31", "2002-11-30", "2002-12-31", "2003-01-31", "2003-02-28", "2003-03-31", "2003-04-30", "2003-05-31", "2003-06-30", "2003-07-31", "2003-08-31", "2003-09-30", "2003-10-31", "2003-11-30", "2003-12-31", "2004-01-31", "2004-02-29", "2004-03-31", "2004-04-30", "2004-05-31", "2004-06-30", "2004-07-31", "2004-08-31", "2004-09-30", "2004-10-31", "2004-11-30", "2004-12-31", "2005-01-31", "2005-02-28", "2005-03-31", "2005-04-30", "2005-05-31", "2005-06-30", "2005-07-31", "2005-08-31", "2005-09-30", "2005-10-31", "2005-11-30", "2005-12-31", "2006-01-31", "2006-02-28", "2006-03-31", "2006-04-30", "2006-05-31", "2006-06-30", "2006-07-31", "2006-08-31", "2006-09-30", "2006-10-31", "2006-11-30", "2006-12-31", "2001-09-30", "2001-10-31", "2001-11-30", "2001-12-31", "2002-01-31", "2002-02-28", "2002-03-31", "2002-04-30", "2002-05-31", "2002-06-30", "2002-07-31", "2002-08-31", "2002-09-30", "2002-10-31", "2002-11-30", "2002-12-31", "2003-01-31", "2003-02-28", "2003-03-31", "2003-04-30", "2003-05-31", "2003-06-30", "2003-07-31", "2003-08-31", "2003-09-30", "2003-10-31", "2003-11-30", "2003-12-31", "2004-01-31", "2004-02-29", "2004-03-31", "2004-04-30", "2004-05-31", "2004-06-30", "2004-07-31", "2004-08-31", "2004-09-30", "2004-10-31", "2004-11-30", "2004-12-31", "2005-01-31", "2005-02-28", "2005-03-31", "2005-04-30", "2005-05-31", "2005-06-30", "2005-07-31", "2005-08-31", "2005-09-30", "2005-10-31", "2005-11-30", "2005-12-31", "2006-01-31", "2006-02-28", "2006-03-31", "2006-04-30", "2006-05-31", "2006-06-30", "2006-07-31", "2006-08-31", "2006-09-30", "2006-10-31", "2006-11-30", "2006-12-31" ],
"manager": [ "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM1", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM2", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM3", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM4", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM5", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "HAM6", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "EDHEC.LS.EQ", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "SP500.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.10Y.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR", "US.3m.TR" ],
"val": [  96.88, 96.987, 100.28, 107.06, 108.51, 107.16, 107.84, 108.33, 108.17, 105.56, 97.595, 98.346, 92.691, 95.444, 101.75, 98.466,  94.41,  92.04,  95.39,  101.6, 105.02, 108.26, 110.16,  110.2, 111.19, 116.54, 118.51, 121.77, 122.41,  122.4, 123.46, 122.93, 123.94, 127.15, 127.15, 127.85, 128.98,  128.9, 133.98, 139.87, 139.89, 142.89, 139.94, 137.01,  137.6, 139.81, 141.09, 142.68, 146.41, 143.67, 146.99, 150.82, 161.26,  163.6, 170.09, 169.91, 165.37, 168.94, 166.51, 169.19, 170.34, 177.61, 179.69, 181.76, 103.33, 101.06, 101.89, 101.85,  99.94, 96.232, 98.359, 97.218,  94.35, 91.161, 90.031, 89.995, 89.842, 90.228, 89.479, 89.282, 89.255, 87.925, 87.433, 86.393, 90.842, 92.977, 95.152, 94.353, 93.853, 95.111, 96.195, 98.975, 100.87, 101.74, 103.05, 103.89, 104.67, 106.11, 106.56, 105.87, 106.93,  108.5, 111.23, 112.15, 111.55, 113.89, 113.11, 112.66,  111.1, 113.25, 115.51, 116.93, 119.03, 116.82, 117.92, 118.92, 128.58, 124.38, 126.26, 128.43, 127.77, 126.28, 124.63, 123.22, 120.37, 122.39, 124.91, 124.13,  99.32, 97.284, 99.697, 99.876, 98.428, 94.353, 96.306, 93.869, 93.644, 89.421, 86.381, 86.588, 82.787, 83.631, 86.558, 80.915, 80.793, 80.963, 81.862, 86.159, 89.804, 90.783, 91.563, 94.329, 94.093, 99.785, 100.96, 101.92, 102.47, 103.94, 103.54, 101.47, 100.79, 100.89, 97.392, 96.846, 96.537,  96.43, 101.77, 103.61, 104.37, 108.72, 109.73, 109.53, 112.08, 112.47, 114.75, 114.52, 116.78, 115.48, 117.35, 119.48, 123.67, 126.96, 128.47, 128.87, 126.14, 123.76, 125.02, 128.18,  129.1, 131.47,    135, 136.49,  89.25, 93.418, 100.27, 111.69, 107.84, 105.12,  119.6, 126.87,  127.1, 121.09, 104.06, 102.07, 90.736, 91.562, 98.978, 102.48, 98.977, 98.512, 100.99, 112.58, 122.64, 128.53, 132.66, 131.55, 132.77, 141.18, 153.34, 160.82, 162.94, 170.13, 173.63, 158.72, 155.86, 156.58,  151.3,  155.9, 162.23, 161.11, 174.87, 183.07, 183.84,    178, 174.12, 172.48, 174.63, 187.21, 199.04, 193.83, 187.39,  181.9, 193.49,  200.3, 211.35, 199.96,  202.1, 205.74, 201.95, 205.06,  202.6,  198.9, 202.81, 213.32, 221.28, 225.83,   99.3, 86.192, 88.261, 92.533, 100.36, 99.498, 92.294, 96.531, 91.289, 86.989, 80.987, 87.264, 83.599, 81.032, 82.272, 82.519, 82.403, 81.851, 85.542, 86.697, 92.107, 97.376, 93.442, 92.377, 95.776,  101.4, 104.59, 100.24,    100, 102.94,  102.3, 98.353,  99.78, 100.45, 98.158, 95.537, 98.967, 97.848, 101.69, 107.23, 107.18, 107.86, 106.79, 103.65, 106.85, 107.21, 111.03, 113.98, 114.77, 113.47, 116.08, 114.33, 120.51, 120.09, 124.43, 127.49, 124.15, 122.74, 120.73, 122.77, 124.39,  127.7, 128.18, 132.24, 100.23, 103.69, 108.81, 114.26, 116.49,    118, 123.88, 124.46, 122.23, 119.47, 114.66, 115.75, 117.22, 116.15, 118.09, 117.59, 117.79, 118.18,  120.3, 120.25, 123.28, 123.19, 125.81,  128.8, 126.39, 133.76, 135.12, 142.02, 145.59, 146.67, 148.84, 146.06, 143.81, 145.52, 140.27,  138.7, 141.32, 142.82,  148.2, 152.41, 150.28, 154.66, 152.16, 146.06, 149.77, 151.56, 158.91, 161.22, 165.36, 158.68, 164.01, 168.29, 175.98,  176.9, 180.38, 186.54, 187.93, 189.27, 185.01, 188.58, 185.24, 188.74, 194.41, 198.59,  96.52, 97.476, 99.425, 101.21, 100.84,   99.6, 101.14, 100.72, 100.38, 97.877,  94.07, 94.455, 92.944, 94.087, 96.195, 94.762, 94.809, 94.458, 94.647, 97.467,    101, 102.29,  103.5, 105.35, 106.34, 109.52, 110.94, 113.06, 115.23, 116.65, 117.13, 115.19, 114.79, 115.84, 114.05,  113.8, 116.19, 117.05, 120.66,  122.8, 122.59, 125.17, 123.97, 121.69, 123.09, 125.49, 128.81, 130.06, 132.95, 130.64, 133.39, 136.71, 141.92, 142.15, 145.53, 148.04, 144.36, 143.47, 143.02, 144.65, 144.67, 147.48, 150.43, 152.73,  91.92, 93.676, 100.86, 101.75, 100.26, 98.328, 102.02, 95.842, 95.133, 88.359, 81.467, 82.005, 73.091, 79.523, 84.207, 79.264, 77.187, 76.029, 76.767, 83.093, 87.472, 88.591,  90.15, 91.908, 90.934, 96.081, 96.926, 102.01, 103.88, 105.33, 103.74, 102.11, 103.51, 105.52, 102.03, 102.45, 103.56, 105.14,  109.4, 113.12, 110.36, 112.68, 110.69, 108.58, 112.04,  112.2, 116.37, 115.31, 116.25, 114.31, 118.63, 118.66, 121.81, 122.14, 123.66, 125.32, 121.71, 121.88, 122.64, 125.56, 128.79, 132.99, 135.52, 137.42, 102.28, 105.14, 101.74, 99.954, 100.45, 101.73, 97.955, 100.76, 101.61,  103.7, 106.96, 110.15, 115.31, 112.99, 110.65, 114.57, 113.57, 116.71, 115.88, 115.92, 121.32, 119.89, 111.39, 112.58,  117.7, 114.78, 115.12, 116.08, 117.67, 119.64, 121.46, 115.59,  114.8, 115.57, 117.28, 120.93,  121.4, 122.68, 119.89, 121.68, 122.92, 121.05, 120.18, 123.45,  125.6, 126.63, 123.68, 126.48,  123.8, 122.01, 122.65,  124.1, 123.29, 123.12, 120.67, 119.14, 119.04,  119.3, 121.19, 123.84, 125.25, 125.98, 127.77, 125.79, 100.43, 100.69, 100.91, 101.07, 101.21, 101.35,  101.5, 101.66, 101.82, 101.97, 102.12, 102.27, 102.43, 102.58, 102.75, 102.87, 102.97, 103.06, 103.18, 103.28, 103.38, 103.52, 103.59, 103.68, 103.78, 103.86, 103.95, 104.05, 104.13, 104.21, 104.29, 104.38, 104.47, 104.54, 104.66, 104.79, 104.92, 105.06,  105.2, 105.43,  105.6, 105.77, 106.03, 106.27, 106.54, 106.79, 107.04, 107.35, 107.67, 107.96, 108.31, 108.66,    109, 109.35, 109.77, 110.18, 110.62, 111.05, 111.51, 112.01, 112.52, 112.95, 113.43, 113.93 ] 
},
"groups": "manager",
"id": "chart4f86c8f743c" 
}
  
  //set description of the data provided in params
  d3.select('.g-table-readin').text( params.description );
  //http://stackoverflow.com/questions/196972/convert-string-to-title-case-with-javascript
  function toTitleCase(str)
    {
        return str.replace(/\w\S*/g, function(txt){return txt.charAt(0).toUpperCase() + txt.substr(1).toLowerCase();});
    }
  
  //set table heading to groups
  d3.select('.g-proper-city').text( toTitleCase(params.groups) );
  
  var nytMonths = {"Jan": "Jan.",
                   "Feb": "Feb.",
                   "Mar": "March",
                   "Apr": "April",
                   "May": "May",
                   "Jun": "June",
                   "Jul": "July",
                   "Aug": "Aug.",
                   "Sep": "Sept",
                   "Oct": "Oct.",
                   "Nov": "Nov.",
                   "Dec": "Dec."
                    }

  d3.selection.prototype.moveToFront = function() {
    return this.each(function(){
      this.parentNode.appendChild(this);
    });
  };

  var margin = {top: 20, right: 10, bottom: 1, left: 10},
      width = 950 - margin.left - margin.right,
      height = 500 - margin.top - margin.bottom;

  var formatYear = d3.time.format("'%y"),
      format = d3.time.format("%m/%d/%y"),
      bisectDate = d3.bisector(function(d) { return d.date; }).left,
      formatToMonth = d3.time.format("%B %Y"),
      formatPercent = d3.format(".1%"),
      formatVal = d3.format(".1f"),
      formatMonAbb = d3.time.format("%b. %Y"),
      formatMonAbb2 = d3.time.format("%b"),
      formatMonthOnly = d3.time.format("%b"),
      formatYearOnly = d3.time.format("%Y"),
      formatPercentChange = d3.format("+%"),
      formatChangeInWords = function (val) {
        var change =  100 - formatNumberWords(val);
        var str = change > 0 ? " percent less" : " percent more";
        if (change < 1 && change > -1 ) {
          return "about the same";
        };
        return (Math.abs(change) + str);
      },
      formatNumberWords = d3.format("0f"),
      formatPercentChange2 = function(d) { return d/100},
      formatFullYear = d3.time.format("%Y"),
      defaultCity = params.data[params.groups][0],
      currentCity,
      hoverCity,
      currentDateIndex,
      defaultDateIndex = 12, //TODO Calculate it!
      currentPriceTier = "val";

  var formatAxes = function(d) {
    var adj = d - 100;
    if (adj == 0) { return "Starting price"; }
    if (adj < 0) { return Math.abs(adj) + "% less"; }
    if (adj > 0) { return Math.abs(adj) + "% more"; }
  };

  var x = d3.time.scale()
      .range([0, width]);

  var y = d3.scale.log()
      .range([height, 20]);

  var barScale = d3.scale.linear()
      .range([0,75]);
      //set domain later with max
      //.domain([0,75]);

  var line = d3.svg.line()
      .x(function(d) { return x(d.date); })
      .y(function(d) { return y(d.adjustedVal); })
      .defined(function(d) { return !isNaN(d.adjustedVal); })

  var slider = d3.select(".g-chart").append("div")
      .attr("class", "g-slider")
      .style("width", width + "px")
      .style("margin-left", margin.left + "px");

  slider.append("div")
      .attr("class", "g-slider-tray")

  var sliderFill = slider.append("div")
      .attr("class", "g-slider-fill")

  var sliderHandle = slider.append("div")
      .attr("class", "g-slider-handle");

    sliderHandle.append("img")
      .attr("class", "g-slider-handle-icon")
      .attr("src", "libraries/widgets/nyt_home/css/handle@2x.png")

  var svg = d3.select(".g-chart").append("svg")
      .attr("width", width + margin.left + margin.right)
      .attr("height", height + margin.top + margin.bottom)
      .attr("class", "g-svg")
    .append("g")
      .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

  var xAxis = d3.svg.axis()
      .scale(x)
      .orient("top")
      .ticks(d3.time.years)
      .tickSize(4)
      .tickPadding(2)
      .tickFormat(formatYear);

  var yAxis = d3.svg.axis()
      .scale(y)
      .orient("left")
      .ticks(7)
//      .tickValues( [ 35, 50, 75, 100, 150, 200, 250] )
      .tickSize(-width - margin.left - margin.right)
      .tickFormat(formatAxes);

//  queue()
//      .defer(d3.csv, "http://graphics8.nytimes.com/newsgraphics/2013/05/28/case-shiller/2a8c44bdaf0703a2361add917782a6dd88c7d81e/lookup2.csv")
//      .defer(d3.csv, "http://graphics8.nytimes.com/newsgraphics/2013/05/28/case-shiller/2a8c44bdaf0703a2361add917782a6dd88c7d81e/case-shiller-tiered2.csv")
//      .await(ready)
//
//  function ready(err, lookup, data) {
    data = params.data;
    
    var dataArray = [];
    
    data[d3.keys(data)[0]].forEach(function(d,i) {
      var dataItem = {};
      d3.keys(data).forEach(function(dd) {
        if(dd === 'date'){
          dataItem[dd] = new Date(data[dd][i]);
        } else dataItem[dd] = data[dd][i];
      })
      dataArray.push(dataItem);
    });

    cityNameByKey = {};
    
    //if lookup is undefined just get key:key for each unique
    if (!(typeof(params.lookup) === 'undefined')) {
      lookup = params.lookup;
      lookup.forEach(function(d) {
        cityNameByKey[d.code] = d.city;
      });
    } else {
      lookup = d3.keys(d3.nest().key(function(d){return d[params.groups]}).map(dataArray));
      lookup.forEach(function(d) {
        cityNameByKey[d] = d;
      });
    };

    nested = d3.nest()
        .key(function(d) { return d[params.groups]; })
        .entries(dataArray);

    nested.forEach(function(city) {
      city.updatedMonth = city.values[nested[1].values.length-1].date;
      city.mostRecentValue = city.values[nested[1].values.length-1].val;
      city.maxVal = d3.max(city.values, function(d) { return d.val;});
      city.peakidx = city.values.map(function(d) { return d.val}).indexOf(city.maxVal);
      city.peakMonth = city.values.map(function(d) { return d.date})[city.peakidx];
      city.yearAgoDate = city.values[nested[1].values.length-13].date;
      city.yearAgoVal = city.values[nested[1].values.length-13].val;
      city.yearOnYearChange = (city.mostRecentValue - city.yearAgoVal) / city.yearAgoVal;
      city.changeFromPeak = (city.mostRecentValue - city.maxVal)/(city.maxVal);
      city.changeSinceBeginning = (city.mostRecentValue - city.values[0].val)/city.values[0].val;
      city.proper = cityNameByKey[city.key];
    });

    nested.sort(function(a,b) {
      return b.yearOnYearChange - a.yearOnYearChange;
    });
    
    x.domain(d3.extent(dataArray, function(d) { return d.date; }));
    y.domain([50,d3.max(dataArray, function(d){return d.val}) * 1.2]); 
    
    //set domains for each of the barScales
    barScale.domain([0,d3.max(nested,function(d){return Math.abs(d.changeSinceBeginning)})*100]);
    var indexFromMouseX = d3.scale.linear()
        .domain([0, width])
        .rangeRound([1, nested[1].values.length - 1])
        .clamp(true)

    svg.append("rect")
        .attr("class", "background")
        .attr("width", width)
        .attr("height", height);

    svg.append("g")
        .attr("class", "x axis")
        .attr("transform", "translate(0," + 5 + ")")
        .call(xAxis)
      .selectAll("g")
        .classed("minor", function(d, i) { return d.getFullYear() % 1; })
        .filter(".minor")
      .select("line")
        .attr("y2", 2);

        d3.selectAll(".x.axis text").attr("dy", 20)

    // lines
    var linecontainer = svg.append("g")
        .classed("g-linecontainer", true);

    var screeny = svg.append("rect")
        .classed("g-screen", true)
        .attr("width", 0)
        .attr("height", height)
        .attr("x", 0 - margin.left);

    var yAxisMarker = svg.append("g")
        .attr("class", "y axis")
        .attr("transform", "translate(" + (0) + ",0)")
        .call(yAxis)
      .selectAll("g")
      .classed("minor", true)

        // .classed("minor", function(d,i) { return i !== 0; })
        .classed("g-baseline", function(d) { return d == 100 })
        // .classed("g-first", function(d,i) { return i == 0; })
      .select("text")
        .attr("x", 30)
        .attr("y", -5)
        .attr("dy", null);

    var lines = linecontainer.selectAll("path")
        .data(nested)
      .enter().append("path")
        .attr("class", (function(d) { return ( "g-city-line " + d.key)}) )
        .classed("g-highlight-line", function(d,i) { return d.key == currentCity});

    var hoverbar = svg.append("rect")
        .classed("g-hover-bar", true)
        .attr("width", 2)
        .attr("height", height)
        .attr("x", 0 - margin.left);

    var focus = svg.append("g")
        .attr("class", "focus")
        .attr("transform", "translate(" + x(nested[0].values[0].date) + ", " + y(100) + ")")

    focus.append("circle")
        .attr("r", 5)

    focus.append("text")
        .attr("x", 9)
        .attr("dy", ".35em");

    var currentMonth = slider.append("div")
        .classed("g-current-month", true);

    var mostRecentMonth = slider.append("div")
        .classed("g-most-recent-month", true)
        .html("...to <b>" + d3.time.format("%b %Y")(nested[0].updatedMonth) + "</b>");

    var endpoint = svg.append("g")
        .attr("class", "g-endpoint")
        .attr("transform", "translate(" + x(nested[0].values[nested[0].values.length-1].date) + ", " + y(100) + ")")

    endpoint.append("circle")
        .attr("r", 5);

    endpoint.append("text")
        .classed("g-end-label g-selected-city-endpoint-label", true)
        .attr("y", -45)
        .attr("x", - 0)
        .text("U.S. 20-city index")

    endpoint.append("text")
        .classed("g-end-label g-selected-city-endpoint-value", true)
        .attr("y", -20)
        .attr("x", - 0)
        .text("+12%")

    //
    //  Init Table
    //
    function initTable() {
      var rows = d3.select(".g-table").selectAll(".table-row")
          .data(nested)
        .enter()
          .append("tr")
          .attr("class", function(d) { return "g-table-row " + d.key + "-row"; })
          .classed("g-selected-row", function(d) { return d.key == "SPCSIND20"});

      var cityNames = rows.append("td")
          .text(function(d) { return d.proper; })
          .classed("g-proper-city", true);

      var yearlyChange = rows.append("td")
          .classed("g-num-td", true)
          .text(function(d) { return formatPercentChange(d.yearOnYearChange); });

      var yearlyChangeBarTd = rows.append("td")
          .classed("g-bar-td", true);

      var yearChangeBarContainer = yearlyChangeBarTd.append("div")
          .classed("g-bar-container", true);

      var changeBar = yearChangeBarContainer.append("div")
          .classed("g-yoy-bar", true)
             .style("width", function(d) {
                var chg = 100 * d.yearOnYearChange;
                return Math.abs(barScale(chg)).toString() + "px";
            })
            .style("left", function(d) {
               var chg = 100 * d.yearOnYearChange;
              if (chg<0) {
                var nudge = Math.abs(barScale(chg));
                return (barScale(barScale.domain()[1]) - nudge).toString() + "px";
              }
              else {
                return barScale(barScale.domain()[1]).toString() + "px";
              }
           });

      var zeroMarker3 = yearChangeBarContainer.append("div")
          .classed("g-zeromarker", true)
          .style("left", barScale(barScale.domain()[1]) + "px");

      var peakMonthTd = rows.append("td")
          .classed("g-num-td", true)
          .text(function(d) {
            return formatPercentChange(d.changeFromPeak);
          });

      var peakMonthBarTd = rows.append("td")
          .classed("g-bar-td", true);

      var peakMonthBarContainer = peakMonthBarTd.append("div")
          .classed("g-bar-container", true);

      var changeBar = peakMonthBarContainer.append("div")
          .classed("g-yoy-bar", true)
          .style("width", function(d) {
             var chg = 100 * d.changeFromPeak;
             return Math.abs(barScale(chg)).toString() + "px";
          })
          .style("left", function(d) {
             var chg = 100 * d.changeFromPeak;
            if (chg < 0) {
              var nudge = Math.abs(barScale(chg));
              return (barScale(barScale.domain()[1]) - nudge).toString() + "px";
            }
            else {
              return barScale(barScale.domain()[1]).toString() + "px";
            }
          });

      var zeroMarker2 = peakMonthBarContainer.append("div")
          .classed("g-zeromarker", true)
          .style("left", barScale(barScale.domain()[1]) + "px");

      var changeSinceSelectedTd = rows.append("td")
          .classed("g-change-since-selected-td g-num-td", true);

      var changeSelectedBarTD = rows.append("td")
          .classed("g-live-change-bar-td", true);

      var liveBarContainer = changeSelectedBarTD.append("div")
          .classed("g-live-bar-container", true);

      var liveChangeBar = liveBarContainer.append("div")
          .classed("g-live-bar", true);

      var zeroMarker = liveBarContainer.append("div")
          .classed("g-zeromarker", true)
          .style("left", barScale(barScale.domain()[1]) + "px");
    }

    initTable();

    //
    // Init Dropdowns
    //
    var dropDowns = d3.map();
    dropDowns.set("price", {
      class:".g-price-selector",
      data:[["an average priced", "val"], ["a low priced", "low"], ["a medium priced", "mid"], ["a high priced", "high"]],
      change: selectPrice

    });
    dropDowns.set("city", {
      class:".g-city-selector",
      data:nested.map(function(d) {
//
//        if(d.key == "SPCS20R") {
//          return ["a major city", d.key ];
//        }
//        else {
          return [cityNameByKey[d.key], d.key];
//
 //       }

      }).sort(),
      change: selectCity
    });
    dropDowns.set("date", {
      class:".g-date-selector",
      data:nested[0].values.map(function(d, i) { return [formatMonAbb(d.date), i]; }),
      change: selectDateWithIndex
    });

    dropDowns.forEach(function(key, d) {
      d.select = d3.select(d.class)
          .datum(d.data)
        .append("select")
          .on("change", function() { d.change(this.value); });

      d.select.selectAll("option")
          .data(function(d) { return d; })
        .enter().append("option")
          .text(function(d) { return d[0]; })
          .attr("value", function(d) { return d[1]; })
    });

    // change to 12 periods; need to generalize
    //NYT comments below
    // TODO make this find the 10-year index instead of hardcode 41 and animate it
    // selectDateWithIndex(1);
    selectCity(defaultCity);
    d3.transition()
        .duration(1000)
        .ease("out")
        .tween("intro", function() {
          return function(t) {
              selectDateWithIndex(Math.round(t * defaultDateIndex));
            };
        })

    //
    // Mouse Events
    //
    svg.on("mousemove", hover);
    svg.on("mouseout", function() {
      unHighlightCity();

      d3.select(".g-tooltip")
          .classed("g-hovering-tooltip", false)
    })

    slider.call(d3.behavior.drag()
        .on("dragstart", dragstarted)
        .on("drag", dragged));

    svg.on("click", function(d) {
      selectCity(hoverCity);
    });

    d3.selectAll(".g-table-row").on("click", function(d) {
      selectCity(d.key);
    }).on("mouseover", function(d) {
      highlightCity(d.key)
    }).on("mouseout", function(d) {
      unHighlightCity(d.key)
    });

    function selectPrice(priceKey) {
      if (currentPriceTier !== priceKey) {
        dropDowns.get("price").select.node().value = priceKey;
        currentPriceTier = priceKey;
        adjustData();
      }
    }

    function selectDateWithIndex(dateIndex) {
      if (currentDateIndex !== dateIndex) {
        currentDateIndex = dateIndex;
        dropDowns.get("date").select.node().value = currentDateIndex;
        adjustData();
      }
    }

    function selectCity(cityKey) {
      cityKey = cityKey ? cityKey : defaultCity;
      if (currentCity !== cityKey) {
        if (isNaN(nested.filter(function(d) { return d.key == cityKey; })[0].values[0][currentPriceTier])) selectPrice("val");
        dropDowns.get("price").select.property("disabled", isNaN(nested.filter(function(d) { return d.key == cityKey; })[0].values[0].low));
        dropDowns.get("city").select.node().value = cityKey;

        currentCity = cityKey;
        d3.selectAll(".g-city-line").classed("g-highlight-line", function(d) { return d.key == cityKey; });
        d3.selectAll(".g-table-row").classed("g-selected-row", function(d) { return d.key == cityKey; });
        d3.select("." + cityKey).moveToFront();
        d3.select(".g-selected-city-endpoint-label").text(cityNameByKey[cityKey])

        updateLabels();
      }
    }

    function updateLabels() {
      var obj = nested.filter(function(d) { return d.key == currentCity})[0]
      var lastVal = obj.values[obj.values.length-1].adjustedVal;
      d3.select(".g-answer").text(formatChangeInWords(lastVal));
      d3.select(".g-selected-city-endpoint-value").text(formatChangeInWords(lastVal))
      d3.select(".g-endpoint")
          .attr("transform", "translate(" + x(obj.values[obj.values.length-1].date) + ","+ y(lastVal)+")" );
    }

    function highlightCity(cityKey) {
      if (hoverCity !== cityKey) {

        hoverCity = cityKey;
        d3.selectAll(".g-city-line").classed("g-hover-line", function(d) { return d.key == cityKey; });
        d3.selectAll(".g-table-row").classed("g-hover-row", function(d) { return d.key == cityKey; });
      }
    }

    function unHighlightCity() {
      if (hoverCity !== null) {
        hoverCity = null;
        d3.selectAll(".g-city-line").classed("g-hover-line", false);
        d3.selectAll(".g-table-row").classed("g-hover-row", false);

        d3.select(".g-tooltip")
            .classed("g-hovering-tooltip", false)
      }
    }

    function dragstarted() {
      unHighlightCity();
      dragged();
      d3.event.sourceEvent.preventDefault();
    }

    function dragged() {
      selectDateWithIndex(indexFromMouseX(d3.mouse(svg.node())[0]));
    }

    function hover() {
      var m = d3.mouse(svg.node()),
          index = indexFromMouseX(m[0]),
          pct = y.invert(m[1]);

      var closestDifference = Infinity,
          closestIndex = -1,
          currentDifference;

      nested.forEach(function(d, i) {
        currentDifference = Math.abs(pct - d.values[index].adjustedVal);
        if (currentDifference < closestDifference) {
          closestDifference = currentDifference;
          closestIndex = i;
        }
      });

      if (closestDifference < 30) {
        highlightCity(nested[closestIndex].key);

        var m = d3.mouse(svg.node());
        d3.select(".g-tooltip")
            .classed("g-hovering-tooltip", true)
            .style("left", m[0] + "px")
            .style("top", m[1] + 70 + "px")
            .text(cityNameByKey[nested[closestIndex].key]);

        svg.style("cursor", "pointer");
      } else {
        svg.style("cursor", "inherit");

        unHighlightCity();
      }
    }

    function adjustData() {
      nested.forEach(function(d) {
        d.values.forEach(function(j) {
          j.adjustedVal = 100 * (j[currentPriceTier] / d.values[currentDateIndex][currentPriceTier]);
        })
      });

      lines.attr("d", function(d) { return line(d.values); } );

      var thisDate = nested[0].values[currentDateIndex].date,
          xPos = x(thisDate)

      var newpos = xPos < 100 ? 60 : (xPos - 40);
      // when xpos is less than 50, freeze it at 100
      d3.selectAll(".y.axis .tick.minor text")
        .attr("transform", "translate(" + newpos + ",0)")

      focus.attr("transform", "translate(" + (xPos - 2) + "," + y(100) + ")");

      d3.select(".g-date-compare")
          .text(function(d) { return ("Since " + nytMonths[formatMonthOnly(thisDate)] + " " + formatYearOnly(thisDate)   )})

      d3.select(".g-screen").attr("width", xPos + 10 );

      if (xPos) {};

      currentMonth
          .style("left", Math.min(Math.max(xPos - 160, 0), width - 320) + "px")
          .html(function(d) {  return "Price changes from <b>" + nytMonths[formatMonAbb2(thisDate)] + " " +  formatYearOnly(thisDate) +  "</b>..." });

      sliderFill.style("width",  (width - x(thisDate)) + "px" );

      sliderHandle.style("left", xPos + "px")

      d3.select(".g-hover-bar").attr("x", xPos - 2 );

      d3.selectAll(".g-change-since-selected-td")
          .text(function(d) {
            var current = d.values[d.values.length-1].val
            var compareVal = d.values[currentDateIndex].val;
            var chg = (current - compareVal ) / compareVal;
            return formatPercentChange(chg);
          });

      var thisCityObj = nested.filter(function(d) { return d.key == currentCity})[0];

      updateLabels();

      d3.selectAll(".g-live-bar")
          .style("width", function(d) {
              var current = d.values[d.values.length-1].val
              var compareVal = d.values[currentDateIndex].val;
              var chg = 100 * (current - compareVal ) / compareVal;
              return Math.abs(barScale(chg)).toString() + "px";
          })
          .style("left", function(d) {
            var current = d.values[d.values.length-1].val
            var compareVal = d.values[currentDateIndex].val;
            var chg = 100 * (current - compareVal ) / compareVal;

            if (chg < 0) {
              var nudge = Math.abs(barScale(chg));
              return (barScale(barScale.domain()[1]) - nudge).toString() + "px";
              // what fraction of the width in pixels is this?
              var nudge = starterBarWidth * Math.abs(chg)/100;
              return (starterBarWidth - nudge).toString() + "px";
            }
            else {
              return barScale(barScale.domain()[1]).toString() + "px";
            }
         });
    }
  //}

})()</script>


### Thanks
As I hope you can tell, this post was more a function of the efforts of others than of my own.

Thanks specifically:
- Ramnath Vaidyanathan for [rCharts](http://rcharts.io/site) and [slidify](http://slidify.org).
- NYT Visualization Design Team for all their inspiring examples.
- Google fonts [Raleway](http://www.google.com/fonts/specimen/Raleway) and [Oxygen](http://www.google.com/fonts/specimen/Oxygen)
