## Introduction

In this work, I take a look at the changes in subreddit activity levels, how they shifted and whether certain communities on reddit shifted together. 

## Abstract

The year 2019 to 2021 has seen countless events with global ramifications. As individuals become increasingly interconnected online through social media, it raises questions about how they are interacting with one another on these platforms and why. To understand these changes is to understand what people find most interesting or important, and how they feel about them. In this work, we will take a stab at answering this question by examining a subset of ~5000 reddit communities and the changes in activity level within them, as well as whether the sentiment associated with the activity is positive or negative. We will look at subreddits that have grown in size, and those that have contracted separately, and try to understand what sets them apart. We will then identify whether the activity through the grown/contraction phases is largely related to positive or negative sentiment, time permitting.

### Research questions:

1. What are the similarities shared between subreddits that experienced similar activity level changes?  
  * Did the ones that grew/contracted:
    - experience that growth during similar time periods, and are there any reasons why?  
    - share common topics?
2. What kind of sentiment can we relate to subreddits that experienced similar activity level changes?  
  * Was there any sentiment associated with these changes?  
  * Was growth/contraction associated with very positive/negative sentiments? (I.e. Was the change driven by love/attraction or by hate/repulsion?)  

### The data

Let's take a look at the first 5 rows of data in our two data sources, `text_comments.csv.gz` and `text_submissions.csv.gz`
#### Comment text data

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>score</th>
      <th>link_id</th>
      <th>author</th>
      <th>subreddit</th>
      <th>body</th>
      <th>created_utc</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>t1_ftjl56l</td>
      <td>4.0</td>
      <td>t3_gzv6so</td>
      <td>mega_trex</td>
      <td>BeautyGuruChatter</td>
      <td>Does anyone have a good cruelty free one? The ...</td>
      <td>1.591756e+09</td>
    </tr>
    <tr>
      <th>1</th>
      <td>t1_ftjpxmc</td>
      <td>6.0</td>
      <td>t3_gzv6so</td>
      <td>[deleted]</td>
      <td>BeautyGuruChatter</td>
      <td>(stares at my soft glam i've had for like 3 ye...</td>
      <td>1.591758e+09</td>
    </tr>
    <tr>
      <th>2</th>
      <td>t1_gzzxfyt</td>
      <td>22.0</td>
      <td>t3_nodb9e</td>
      <td>divadream</td>
      <td>BeautyGuruChatter</td>
      <td>When Jen’s initial reactions came out to the s...</td>
      <td>1.622398e+09</td>
    </tr>
    <tr>
      <th>3</th>
      <td>t1_gzzy7nc</td>
      <td>92.0</td>
      <td>t3_no6qaj</td>
      <td>Ziegenkoennenfliegen</td>
      <td>BeautyGuruChatter</td>
      <td>I think you mean a \n&gt;Highschool *fucking* bully</td>
      <td>1.622399e+09</td>
    </tr>
    <tr>
      <th>4</th>
      <td>t1_h00tpbp</td>
      <td>82.0</td>
      <td>t3_nolx7p</td>
      <td>meowrottenralph</td>
      <td>BeautyGuruChatter</td>
      <td>Ugh. I was honestly hoping that this brand wou...</td>
      <td>1.622415e+09</td>
    </tr>
  </tbody>
</table>
</div>
Which contains (46413725, 7) rows and columns respectively.

#### Submission/Post Text Data

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>author</th>
      <th>created_utc</th>
      <th>domain</th>
      <th>is_self</th>
      <th>score</th>
      <th>selftext</th>
      <th>title</th>
      <th>subreddit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>t3_npxigk</td>
      <td>All_Consuming_Void</td>
      <td>1622563615</td>
      <td>self.BeautyGuruChatter</td>
      <td>True</td>
      <td>0.0</td>
      <td>[removed]</td>
      <td>Hyram launches his own brand</td>
      <td>BeautyGuruChatter</td>
    </tr>
    <tr>
      <th>1</th>
      <td>t3_nqj6bf</td>
      <td>AutoModerator</td>
      <td>1622631621</td>
      <td>self.BeautyGuruChatter</td>
      <td>True</td>
      <td>38.0</td>
      <td>What are the influencers trying to influence y...</td>
      <td>What I'm not gonna buy Wednesday - Anti-haul</td>
      <td>BeautyGuruChatter</td>
    </tr>
    <tr>
      <th>2</th>
      <td>t3_nk0btr</td>
      <td>barrahhhh</td>
      <td>1621869439</td>
      <td>reddit.com</td>
      <td>False</td>
      <td>144.0</td>
      <td>NaN</td>
      <td>Plouise goes off in facebook group for 'bullying'</td>
      <td>BeautyGuruChatter</td>
    </tr>
    <tr>
      <th>3</th>
      <td>t3_nrbybs</td>
      <td>[deleted]</td>
      <td>1622722260</td>
      <td>self.BeautyGuruChatter</td>
      <td>True</td>
      <td>2.0</td>
      <td>[deleted]</td>
      <td>Is youtube algorithm against Susan Yara? She g...</td>
      <td>BeautyGuruChatter</td>
    </tr>
    <tr>
      <th>4</th>
      <td>t3_nl0ebd</td>
      <td>carlosShook</td>
      <td>1621977767</td>
      <td>vm.tiktok.com</td>
      <td>False</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>Sephora steals concept from Huntr Faulknr afte...</td>
      <td>BeautyGuruChatter</td>
    </tr>
  </tbody>
</table>
</div>

Which contains (3496180, 9) rows and columns respectively.


We have a **LOT** of data here. A brief look at our data tells us we have over **46 million comments**, which belong to almost **3.5 million posts**. Those 3.5 million posts belong to **4,465 subreddits**. (And this data is already a subset of all of reddit)

We clearly have a lot of data to work with, but due to the time contraints associated with this project it would be best to narrow down a research question and take a subset of this data.

I'll now take a look at the distribution of the number of posts per subreddit.

![Distribution of posts per subreddit]("")

# Reddit: Analysis of content shift

What I want to analyze is how attitudes shifted across the platform, and what the main topics discussed on reddit were in their given time periods.
The hypothesis is that 

