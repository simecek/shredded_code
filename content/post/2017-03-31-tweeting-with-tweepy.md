---
title: Tweeting With Tweepy
date: '2017-03-31'
slug: tweeting-with-tweepy
---

How to manage your Twitter account with Python. The notebook can be accessed from [Github](https://gist.github.com/simecek/b7fa2778ab1d2b0bf383a616877514dc).


## Function: Get List Of Users That Liked A Status


```python
# copied from http://stackoverflow.com/questions/28982850/twitter-api-getting-list-of-users-who-favorited-a-status

import urllib2
import re

def get_user_ids_of_post_likes(post_id):
    try:
        json_data = urllib2.urlopen('https://twitter.com/i/activity/favorited_popup?id=' + str(post_id)).read()
        found_ids = re.findall(r'data-user-id=\\"+\d+', json_data)
        unique_ids = list(set([re.findall(r'\d+', match)[0] for match in found_ids]))
        return unique_ids
    except urllib2.HTTPError:
        return False

# Example: 
# https://twitter.com/golan/status/731770343052972032

print get_user_ids_of_post_likes(731770343052972032)

# ['13520332', '416273351', '284966399']
#
# 13520332 +> @TopLeftBrick
# 416273351 => @Berenger_r
# 284966399 => @FFrink
```

    ['472203302', '13520332', '2388203390', '732265223701286912', '416273351', '6490642', '284966399']


## Connect to Twitter with Tweepy


```python
import tweepy

# create file twkeys.py with our Twitter API keys
# see 
from twkeys import keys
 
SCREEN_NAME = keys['screen_name']
CONSUMER_KEY = keys['consumer_key']
CONSUMER_SECRET = keys['consumer_secret']
ACCESS_TOKEN = keys['access_token']
ACCESS_TOKEN_SECRET = keys['access_token_secret']

auth = tweepy.OAuthHandler(CONSUMER_KEY, CONSUMER_SECRET)
auth.set_access_token(ACCESS_TOKEN, ACCESS_TOKEN_SECRET)
api = tweepy.API(auth)
```

## Friends and Followers


```python
# get list of followers' and friends' (=following) ids
followers = api.followers_ids(SCREEN_NAME)
friends = api.friends_ids(SCREEN_NAME)
```


```python
# print all friends that do not follow us
for f in friends:
    if f not in followers:
        print(api.get_user(f).screen_name)
```

    J_the_Prepper
    EdEDock
    ArtrBautista
    Mrs_DarkDonado
    odd_wheel
    elearning_chad
    Angel_Cruijff
    mohhinder
    paerallax
    khalil_hughes
    super__shoot
    philippebiaut
    jamfpoz
    tedtedted
    datenteiler
    vikramcse10
    benjixx
    pro_cessor
    mrjohnmorrow
    aeroaks
    ViUX
    januszkowalczyk
    isizo
    gwuah_
    Containerhouse
    VaulsteinR
    DanielFritzEU
    allusernow
    ManikosN
    jdevoo
    SeattleDataGuy
    supreja_s



```python
# unfollow non-following friends
# for f in friends:
#     if f not in followers:
#        api.destroy_friendship(f)
```

## Users That Liked Any Of My Recent Tweets


```python
my_recent_tweets = api.user_timeline(screen_name = SCREEN_NAME, count=50)
len(my_recent_tweets)
```




    49




```python
# apply 'get_user_ids_of_post_likes' to get users ids
users_liking_recent_tweet = [get_user_ids_of_post_likes(my_tweet.id) for my_tweet in my_recent_tweets]

import itertools
users_liking_flat = set(list(itertools.chain.from_iterable(users_liking_recent_tweet)))
```


```python
# from the list, let us remove followers, friends and yourself
myself = api.get_user(screen_name = SCREEN_NAME).id
unknown_liking = set([int(f) for f in users_liking_flat]) - set(friends) - set(followers) - set([myself])
len(unknown_liking)
```




    34



## Be Nice To People That Like Your Tweets And Follow Them


```python
for f in unknown_liking:
    api.get_user(f).follow()
```
