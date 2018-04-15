---
layout: post
title:      "My First CLI Gem"
date:       2018-04-15 00:43:47 -0400
permalink:  my_first_cli_gem
---


This project took me forever, about 3 weeks to be exact that was filled with procrastination and denial. First off, I was very intimidated with the idea of having to create something from  scratch and then there were technical obstacles along the way. But, here I am, wrapping up my project for submission and I could not have been happier.

**Ideation**

Initially, my idea was to scrape a website to pull up a list of hiking trips and the details inside. However, the website I was interested in only provided a static list. I wanted something that was constantly being updated and something of my interest. So I decided to scrape the [BodyBuilding](https://www.bodybuilding.com) website. My gem would extract a list of latest articles in workout, then would go to a specific article's page the user picked to list out the workouts inside. 

**Setting up**

I think this was the hard and tricky part that kept me for days from starting the project. So far Learn had been holding our fingers and helping us walk. We had our directory already set up and a test suite to see if we were on the right track. But, here I was being asked to take the first step on my own. So after watching Avi's walkthrough video a couple of times and  reading about creating a gem, I hesitantly took my first step. It was shaky for sure but I had got my foot at the door. 

The following command is all it took:

`Bundle gem swole-news`

![](https://i.imgur.com/CIL0Hyv.jpg)

This set up a scaffold directory for my gem, with all the required files and folders such as bin folder, lib folder, gemfile, gemspec, rakefile and so on. You could also choose to have a code of conduct and license file. I then installed *Nokogiri*, *Open-Uri* and *Pry* as I would be using them in my program later. So, I did not have a clean directory anymore and that gave me a tiny sense of affirmation that I could pull this off...

...until I was instantly slammed with another obstacle -- GIT! Up until now, I had only been using Learn commands to save and push changes and now it was time to do without. I thought my struggle with git deserved its own post so please go [here](http://icodeyounot.com/oh_h_git)  to read about it. With a little practice and google search I finally got the git basics and I was off to build my classes.


**CLI**

I started by creating `CLI.rb`, `article.rb`, `scraper.rb` and `workout.rb` files in the lib folder. Then, as per Avi's video, I started coding outside-in or in other words, top to down. This made sense because I had an idea about how my CLI, the top-level component, would interact with the user, and behind-the-scene work would be carried out by other classes. 

So I laid out the CLI interface where a user would be prompted to choose from a list of articles, then would get an option to look further in to see the workouts. Throughout the program the user would have the option to exit if they wanted to. For the articles and the workouts, I used strings to mock the real objects. For example, instead of returning a list of articles that one could call a `title` method on and get their titles back, I just hard-coded `["Article_1", "Article_2", ...]`.

**Scraping**

Once I got my CLI to work with the mock objects, I moved onto bringing those objects to life. Both `Article` and `Workout` classes depended on `Scraper ` class, so before I could start building those classes, I had to start with `Scraper`. Inspired by the Student Scraper lab that we did before this project, I decided that my `Scraper` class would have two class methods -- one that would return an array of articles in the form of hashes and the other that would return an array of workouts also in the form of hashes. 

**Making my objects talk to each other**

For both `Workout` and `Article` classes, I then created class methods that would take in an array of hash as an argument and iterate over it, creating an instance of the class for each hash.

Then, I revisited my CLI to replace my hardcoded, mock objects with the methods that would return real objects.

```
def make_articles
    scraped_articles = Scraper.scrape_page
    Article.create_from_collection(scraped_articles)
    @articles = Article.all
    @articles
  end
```
For example, this helper method in the CLI class, returns an array of article objects. You can see how beautifully all three objects, `Scraper`, `Article` and `Workout` (implicitly) are collaborating with each other.. 

It did take a lot of going back and forth. One roadblock I came across during creating articles was that my articles did not realize they had a list of workouts or the workouts did not know they belonged to an article. And, that is the core of object relationships, *the objects gotta talk to each other.* I understood the concept very well but just did not know how to or where to code it. And, after a while of brainstorming and playing around with ideas I finally cracked it with the following code:
```
def self.create_from_collection(article_array)
    article_array.map{|article_hash|self.new(article_hash)}

    @@all.map do |article|
      scraped_workouts = Scraper.scrape_workouts(article.url)
      article.workouts = Workout.create_from_collection(scraped_workouts)
      article.workouts.each {|workout| workout.article = article}
    end
  end
```
In the method above for Article class, I told each article being instantiated that it also had a list of workouts, that were being instantiated alongside the article and that they belonged (`workout.article = article`) to the article.

**Publishing**

Finally, eveything was working as I intended. To wrap it up, I revised my code again to make sure I did not see any anti-patterns or if I could write something in more elegant manner. The final step was publishing my gem, which consisted of two major steps -- building and pushing the gem. However, I kept coming across *InvalidURIError* error and thanks to this [article](http://domckellar.com/2017/01/27/publishing_a_gem/) I realized that I needed to modify my gemspec. So, after multiple attempts and keeping my fingers crossed, my gem finally published and now can be found [here](https://rubygems.org/gems/swole-news).

**Conclusion**

Starting from a clean directory was not easy, coding away without a test suite to check your work was daunting but looking back and seeing how far I have come, I can only conclude that I would rather be here than where I was ~17 days ago. That is when I first opened the lesson. Now I feel more knowledgeable and more confident. Hell, I now know git!!  One thing that really stuck with me after this lesson is that in order to succeed, hard work and persistance are necessary but so is a little bit of courage. Take the plunge and leave your self-doubt and fear behind and work your way through. 

Please make sure to check out my Github [repo](https://github.com/kriti-rai/swole-news.git) and my walkthrough [video](https://www.youtube.com/watch?v=BlPmqdfd7JQ). 
	
	














