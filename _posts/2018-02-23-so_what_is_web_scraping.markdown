---
layout: post
title:      "So what is Web scraping?"
date:       2018-02-23 15:36:36 -0500
permalink:  so_what_is_web_scraping
---

Just finished a chapter on web scraping and boy, was I lost...until I sat down to blog about it. So to agree with Avi (from the [video review](https://youtu.be/h2618jCo_eA) towards the end of OO), "stop plowing through the lessons". Instead, take a step back, revise, re-do the lab, ponder on it for a while. 

So I decided to follow the advice, take a moment, breathe, and jot down what I have understood. After rummaging through a couple of online sources and referencing back to the code-along lab in Learn, I sat down to implement the idea of data scraping to extract the list of top 100 songs from the [Billboard](https://www.billboard.com/charts/hot-100). So, bear with me as I walk you through my lengthy code-along.

But first, let's go over what the fuss is all about. Web scraping is a technique of extracting selective data out of the XML/HTML that makes up a webpage so you can use it in your program. If you've seen a page's HTML which you can simply do by right-clicking on the page and clicking on *page inspect* or *view page source*, you know that it has a whole lot of information and might even look gibberish to untrained eyes. To make sense of the gibberish and extract the information we are interested in is a meticulous task. So why go through all that pain? Well, you might be interested in some information from a webpage but it does not provide an API or RSS feed, making web scraping worth the pain.

So, let's start! Sure, there are different ways to do this but the method I use here references the one shown in codealong lab in Learn.  We are going to use a couple of Ruby gems -- *Open-uri* and *Nokogiri* to help us with scraping. Per [RubyDoc](http://ruby-doc.org/stdlib-2.1.3/libdoc/open-uri/rdoc/OpenURI.html), *open-URI*  is an easy-to-use wrapper for Net::HTTP, Net::HTTPS and Net::FTP, whereas *Nokogiri* is an HTML, XML, SAX, and Reader parser. In other words, *Nokogiri* helps with scraping and parsing the HTML we import using *Open-uri*. *Open-uri* is a pretty standard gem and should already be available in Ruby. We can, however, install *Nokogiri* with the following command line in the terminal.

```
gem install nokogiri
```
Now, onto the coding!! Let's create a *Scraper.rb* file. On top of the page, we will *require* the aforementioned gems, alongside *pry* so you can play around with your code and see how it works. In our *Scraper.rb*, we will type:
```
require 'open-uri'
require 'nokogiri'
require 'pry'
```
Then, let's create a *Scraper* class and inside it create a local variable that equals to the data parsed out by the help of *Open-uri* and *Nokogiri*. 
```
class Scraper
    doc= Nokogiri::HTML(open('https://www.billboard.com/charts/hot-100'))
end
```
Now, if we run the file in the terminal, with the help of *pry* we will notice that *doc* equals to a bunch of data but if we look into it close enough we will notice that the data contains songs, artist and ranks and such -- the stuff we are interested in, and are wrapped up inside an array. So, how do we grab the elements? What is that one thing that goes hand-in-hand with XML/HTML to style elements.... that has selectors? 

**Insert CSS!! **

CSS stands for *Cascading Style Sheet*. It has a whole bunch of selectors that wraps XML/HTML elements  in order to style them. So, with these selectors we could grab the elements we want from the page. Sweet! This part does require some knowledge of CSS. So given you have that, you can hover the cursor around the webpage while you are in the "inspecting" mode, to figure out the selectors you are interested in. Listed below are the selectors for the components we are interested in, like the song-title, its artist and the rank.

```
song_title: doc.css(".chart-row").first.css("h2").text
artist: doc.css(".chart-row").first.css(".chart-row__artist").text.strip
rank: doc.css(".chart-row").first.css(".chart-row__rank span.chart-row__current-week").text
```
This is not all of the code, but we will use it later so let's save it somewhere or simply comment it out. Heading back to our code file in *Scraper* class, let's create a few methods for each task we want the class to do -- initialize with a parsed XML/HTML (#initialize), get the song titles from the parsed doc (#get_songs), get the artists of the respective songs (#get_artists) and the songs' rankings (#get_ranks). And, this is where we will use the info we saved above.

```
class Scraper
  attr_accessor :doc
  def initialize
    @doc= Nokogiri::HTML(open('https://www.billboard.com/charts/hot-100'))
  end
  def get_songs
    @doc.css(".chart-row").css("h2").map {|song| song.text}
  end
  def get_artists
    @doc.css(".chart-row").css(".chart-row__artist").map {|artist| artist.text.strip}
  end
  def get_ranks
    @doc.css(".chart-row").css(".chart-row__rank span.chart-row__current-week").map {|rank| rank.text}
  end	
  scraper = Scraper.new
  songs = scraper.get_songs
  artists = scraper.get_artists
  ranks = scraper.get_ranks
  puts "THE BILLBOARD'S TOP 100 SONGS"
  (0..(ranks.size-1)).each_with_index do |index|
      puts "--- Number: #{index+1} ---"
      puts "#{songs[index].upcase} by #{artists[index]}"
  end
end
```
As you notice, we used the array#map method to iterate over the array returned by the parser to grab a value. Then, at the end, we simply created a new Scraper instance and assigned attributes to the instance to puts the intended data. Below is the output:

```
// ♥ ruby scraper.rb
THE BILLBOARD'S TOP 100 SONGS
-- Song: 1 ---
GOD'S PLAN by Drake
--- Song: 2 ---
PERFECT by Ed Sheeran
--- Song: 3 ---
FINESSE by Bruno Mars & Cardi B
--- Song: 4 ---
HAVANA by Camila Cabello Featuring Young Thug
--- Song: 5 ---
ROCKSTAR by Post Malone Featuring 21 Savage
--- Song: 6 ---
LOOK ALIVE by BlocBoy JB Featuring Drake
--- Song: 7 ---
MEANT TO BE by Bebe Rexha & Florida Georgia Line
--- Song: 8 ---
NEW RULES by Dua Lipa
--- Song: 9 ---
ALL THE STARS by Kendrick Lamar & SZA
--- Song: 10 ---
STIR FRY by Migos
--- Song: 11 ---
PRAY FOR ME by The Weeknd & Kendrick Lamar
--- Song: 12 ---
LET YOU DOWN by NF
--- Song: 13 ---
LOVE. by Kendrick Lamar Featuring Zacari
--- Song: 14 ---
HIM & I by G-Eazy & Halsey
--- Song: 15 ---
THUNDER by Imagine Dragons
--- Song: 16 ---
BAD AT LOVE by Halsey
--- Song: 17 ---
THE MIDDLE by Zedd, Maren Morris & Grey
--- Song: 18 ---
MOTORSPORT by Migos, Nicki Minaj & Cardi B
--- Song: 19 ---
I FALL APART by Post Malone
--- Song: 20 ---
NO LIMIT by G-Eazy Featuring A$AP Rocky & Cardi B
--- Song: 21 ---
HOW LONG by Charlie Puth
--- Song: 22 ---
MINE by Bazzi
--- Song: 23 ---
BARTIER CARDI by Cardi B Featuring 21 Savage
--- Song: 24 ---
SHAPE OF YOU by Ed Sheeran
--- Song: 25 ---
BODAK YELLOW (MONEY MOVES) by Cardi B
--- Song: 26 ---
WOLVES by Selena Gomez X Marshmello
--- Song: 27 ---
LIGHTS DOWN LOW by MAX Featuring gnash
--- Song: 28 ---
GUMMO by 6ix9ine
--- Song: 29 ---
NEVER BE THE SAME by Camila Cabello
--- Song: 30 ---
FEEL IT STILL by Portugal. The Man
--- Song: 31 ---
SAY SOMETHING by Justin Timberlake Featuring Chris Stapleton
--- Song: 32 ---
YOUNG DUMB & BROKE by Khalid
--- Song: 33 ---
SKY WALKER by Miguel Featuring Travis Scott
--- Song: 34 ---
PLAIN JANE by A$AP Ferg Featuring Nicki Minaj
--- Song: 35 ---
RIVER by Eminem Featuring Ed Sheeran
--- Song: 36 ---
MARRY ME by Thomas Rhett
--- Song: 37 ---
OUTSIDE TODAY by YoungBoy Never Broke Again
--- Song: 38 ---
KING'S DEAD by Jay Rock, Kendrick Lamar, Future & James Blake
--- Song: 39 ---
TOO GOOD AT GOODBYES by Sam Smith
--- Song: 40 ---
SORRY NOT SORRY by Demi Lovato
--- Song: 41 ---
RIC FLAIR DRIP by Offset & Metro Boomin
--- Song: 42 ---
GUCCI GANG by Lil Pump
--- Song: 43 ---
WAIT by Maroon 5
--- Song: 44 ---
I GET THE BAG by Gucci Mane Featuring Migos
--- Song: 45 ---
1-800-273-8255 by Logic Featuring Alessia Cara & Khalid
--- Song: 46 ---
LEMON by N*E*R*D & Rihanna
--- Song: 47 ---
YOU MAKE IT EASY by Jason Aldean
--- Song: 48 ---
LET ME GO by Hailee Steinfeld & Alesso Featuring Florida Georgia Line & Watt
--- Song: 49 ---
X by ScHoolboy Q, 2 Chainz & Saudi
--- Song: 50 ---
ROLL IN PEACE by Kodak Black Featuring XXXTENTACION
--- Song: 51 ---
FIVE MORE MINUTES by Scotty McCreery
--- Song: 52 ---
WALK IT TALK IT by Migos Featuring Drake
--- Song: 53 ---
NEW FREEZER by Rich The Kid Featuring Kendrick Lamar
--- Song: 54 ---
GOOD OLD DAYS by Macklemore Featuring Kesha
--- Song: 55 ---
WRITTEN IN THE SAND by Old Dominion
--- Song: 56 ---
EL FARSANTE by Ozuna & Romeo Santos
--- Song: 57 ---
HEAVEN by Kane Brown
--- Song: 58 ---
NARCOS by Migos
--- Song: 59 ---
BROKEN HALOS by Chris Stapleton
--- Song: 60 ---
PICK IT UP by Famous Dex Featuring A$AP Rocky
--- Song: 61 ---
DURA by Daddy Yankee
--- Song: 62 ---
PLUG WALK by Rich The Kid
--- Song: 63 ---
THE WAYS by Khalid & Swae Lee
--- Song: 64 ---
YOURS by Russell Dickerson
--- Song: 65 ---
END GAME by Taylor Swift Featuring Ed Sheeran & Future
--- Song: 66 ---
YOU BROKE UP WITH ME by Walker Hayes
--- Song: 67 ---
PARAMEDIC! by SOB X RBE
--- Song: 68 ---
LEGENDS by Kelsea Ballerini
--- Song: 69 ---
BETRAYED by Lil Xan
--- Song: 70 ---
NOWADAYS by Lil Skies Featuring Landon Cube
--- Song: 71 ---
BIG SHOT by Kendrick Lamar & Travis Scott
--- Song: 72 ---
FRIENDS by Marshmello & Anne-Marie
--- Song: 73 ---
ALL ON ME by Devin Dawson
--- Song: 74 ---
NOTICE ME by Migos Featuring Post Malone
--- Song: 75 ---
WHATEVER IT TAKES by Imagine Dragons
--- Song: 76 ---
FOR YOU (FIFTY SHADES FREED) by Liam Payne & Rita Ora
--- Song: 77 ---
KEKE by 6ix9ine, Fetty Wap & A Boogie Wit da Hoodie
--- Song: 78 ---
NO SMOKE by YoungBoy Never Broke Again
--- Song: 79 ---
ECHAME LA CULPA by Luis Fonsi & Demi Lovato
--- Song: 80 ---
TELL ME YOU LOVE ME by Demi Lovato
--- Song: 81 ---
RED ROSES by Lil Skies Featuring Landon Cube
--- Song: 82 ---
MOST PEOPLE ARE GOOD by Luke Bryan
--- Song: 83 ---
THIS IS ME by Keala Settle & The Greatest Showman Ensemble
--- Song: 84 ---
CANDY PAINT by Post Malone
--- Song: 85 ---
HARDAWAY by Derez De'Shon
--- Song: 86 ---
LA MODELO by Ozuna x Cardi B
--- Song: 87 ---
ONE FOOT by WALK THE MOON
--- Song: 88 ---
BEAUTIFUL TRAUMA by P!nk
--- Song: 89 ---
SINGLES YOU UP by Jordan Davis
--- Song: 90 ---
FILTHY by Justin Timberlake
--- Song: 91 ---
BLACK PANTHER by Kendrick Lamar
--- Song: 92 ---
AT THE CLUB by Jacquees X Dej Loaf
--- Song: 93 ---
CODEINE DREAMING by Kodak Black Featuring Lil Wayne
--- Song: 94 ---
THE LONG WAY by Brett Eldredge
--- Song: 95 ---
ROCK by Plies
--- Song: 96 ---
I LIKE ME BETTER by Lauv
--- Song: 97 ---
WHEN WE by Tank
--- Song: 98 ---
CRIMINAL by Natti Natasha x Ozuna
--- Song: 99 ---
IDGAF by Dua Lipa
--- Song: 100 ---
REWRITE THE STARS by Zac Efron & Zendaya
[18:14:56] temporary
// ♥

```

**WE DID IT!!**



