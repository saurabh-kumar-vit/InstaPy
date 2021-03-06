<img src="http://i.imgur.com/9ZjtveL.png" width="150" align="right">

# InstaPy
[![MIT license](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/timgrossmann/InstaPy/blob/master/LICENSE)
[![built with Selenium](https://img.shields.io/badge/built%20with-Selenium-red.svg)](https://github.com/SeleniumHQ/selenium)
[![built with Python3](https://img.shields.io/badge/built%20with-Python3-green.svg)](https://www.python.org/)

### Automation Script for “farming” Likes, Comments and Followers on Instagram
Implemented in Python using the Selenium module.

> **Think this tool is worth supporting?**  
Head over to https://github.com/timgrossmann/InstaPy/wiki/How-to-Contribute to find out how you can help.
**Become a part of InstaPy!**  

> **Have an issue**  
Head over to https://github.com/timgrossmann/InstaPy/wiki/Reporting-An-Issue to find out how to report this to us and get help.

> **Disclaimer**: Please Note that this is a research project. I am by no means responsible for any usage of this tool. Use on your own behalf. I’m also not responsible if your accounts get banned due to extensive use of this tool.

### Social

#### [How it works (Medium)](https://medium.freecodecamp.com/my-open-source-instagram-bot-got-me-2-500-real-followers-for-5-in-server-costs-e40491358340)
#### [Check out the talk (Twitter)](https://twitter.com/timigrossmann/status/869222353166839812)

#### [Support InstaPy!](https://www.patreon.com/InstaPy)

[<img src="http://www.comixlaunch.com/wp-content/uploads/2016/08/patreon-logo-05.jpg" width="100">](https://www.patreon.com/InstaPy)

### Example

```python
from instapy import InstaPy

InstaPy(username='test', password='test')\
  .login()\
  .set_do_comment(True, percentage=10)\
  .set_comments(['Cool!', 'Awesome!', 'Nice!'])\
  .set_dont_include(['friend1', 'friend2', 'friend3'])\
  .set_dont_like(['food', 'girl', 'hot'])\
  .set_ignore_if_contains(['pizza'])\
  .like_by_tags(['dog', '#cat'], amount=100)\
  .end()
```
### How not to be banned ?
Built in delays prevent your account from getting banned. (Just make sure you don't like 1000s of post/day)

## Getting started

### Guides:
**[How to Ubuntu](./docs/How_To_DO_Ubuntu.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**

**[ How to CentOS](./docs/How_To_DO_Centos.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**

**[How to Windows](./docs/How_to_Windows.md)**

### For a start you need:
1. To have [Python](https://www.python.org/downloads/) 2.7 or 3.6 installed.
2. Setted up path for PY command.
3. Installed [Selenium](http://selenium-python.readthedocs.io/) and [pyvirtualdriver](https://pypi.python.org/pypi/PyVirtualDisplay).

Make sure to get the right ```chromedriver``` for your system [from here](https://sites.google.com/a/chromium.org/chromedriver/downloads). Just put it in ```/assets```.

> If you're not too familiar with code and you're working on Windows, try out this tool to set up the settings: [InstaPy Windows GUI](https://github.com/Nemixalone/GUI-tool-for-InstaPy-script)

### Start

```bash
1. git clone https://github.com/timgrossmann/InstaPy.git
2. cd InstaPy
3. pip install .
or
3. python setup.py install
```

## API

### Script file example

```python
from instapy import InstaPy

#if you don't provide arguments, the script will look for INSTA_USER and INSTA_PW in the environment

session = InstaPy(username='test', password='test')
session.login()

#likes specified amount of posts for each hashtag in the array (the '#' is optional)
#in this case: 100 dog-posts and 100 cat-posts
session.like_by_tags(['#dog', 'cat'], amount=100)

#likes specified amount of posts for each location in the array
#in this case: 100 posts geotagged at the chrysler building and 100 posts geotagged at the salton sea
session.like_by_locations(['26429/chrysler-building/', '224442573/salton-sea/'], amount=100)

#gets tags from image passed as instagram-url and likes specified amount of images for each tag
session.like_from_image(url='www.instagram.com/p/BSrfITEFUAM/', amount=100)

#likes 100 posts for each tag of your latest image
session.like_from_image(amount=100)

#likes 50 photos of other animals

session.like_by_tags(['#animals'], amount=50, media='Photo')
session.like_from_image(url='www.instagram.com/image', amount=50, media='Photo')

#likes 15 videos of cats

session.like_by_tags(['#cat'], amount=15, media='Video')
session.like_from_image(url='www.instagram.com/image', amount=15, media='Video')

session.end()
```

If you want the script to get the username and password for your environment, you can do:

```
export INSTA_USER="<Your username>"
export INSTA_PW="<Your password>"
```

### Commenting

```python
#default enabled=False, ~ every 4th image will be commented on

session.set_do_comment(enabled=True, percentage=25)
session.set_comments(['Awesome', 'Really Cool', 'I like your stuff'])

# you can also set comments for specific media types (Photo / Video)
session.set_comments(['Nice shot!'], media='Photo')
session.set_comments(['Great Video!'], media='Video')
```

### Following

```python
#default enabled=False, follows ~ 10% of the users from the images, times=1 (only follows a user once (if unfollowed again))

session.set_do_follow(enabled=True, percentage=10, times=2)
```

### Following by a list

```python
#follows each account from a list of instagram nicknames (only follows a user once (if unfollowed again))
# would be useful for the precise targeting. For example, if one needs to get followbacks from followers of a chosen account/group of accounts.

accs = ['therock','natgeo']
session.follow_by_list(accs, times=1)
```

### Unfollowing

```python
#unfollows 10 of the accounts you're following -> instagram will only unfollow 10 before you'll be 'blocked for 10 minutes' (if you enter a higher number than 10 it will unfollow 10, then wait 10 minutes and will continue then)
# You can choose to only unfollow the user that Insta has followed by adding onlyInstapyFollowed = True otherwise it will unfollow all users

session.unfollow_users(amount=10, onlyInstapyFollowed = True )
```

### Interactions based on the number of followers a user has

```python
#This is used to check the number of followers a user has and if this number exceeds the number set then no further interaction happens

session.set_upper_follower_count(limit = 250)
```

```python
#This is used to check the number of followers a user has and if this number does not pass the number set then no further interaction happens

session.set_lower_follower_count(limit = 1)
```

### Locations

```python
session.like_by_locations(['224442573/salton-sea/'], amount=100)
or
session.like_by_locations(['224442573'], amount=100)
```

You can find locations for the `like_by_locations` function by:
- Browsing https://www.instagram.com/explore/locations/
- Regular instagram search.

Example:
* Search 'Salton Sea' and select the result with a location icon
* The url is: https://www.instagram.com/explore/locations/224442573/salton-sea/
* Use everything after 'locations/' or just the number

### Feeds

```python
#This is used to perform likes on your own feeds, amount specifies how many total like your want to perform

session.like_by_feed(amount=100)
```

### Restricting Likes

```python
session.set_dont_like('#exactmatch', '[startswith', ']endswith', 'broadmatch')
```

`.set_dont_like` searches the description and owner comments for hashtags and won't like the image if one of those hashtags are in there

You have 4 options to exclude posts from your InstaPy session:
* words starting with `#` will match only exact hashtags (e. g. `#cat` matches `#cat`, but not `#catpic`)
* words starting with `[` will match all hashtags starting with your word (e. g. `[cat` matches `#catpic`, `#caturday` and so on)
* words starting with `]` will match all hashtags ending with your word (e. g. `]cat` matches `#mycat`, `#instacat` and so on)
* words without these prefixes will match all hashtags that contain your word regardless if it is placed at the beginning, middle or end of the hashtag (e. g. `cat` will match `#cat`, `#mycat`, `#caturday`, `#rainingcatsanddogs` and so on)

### Ignoring Users

```python
#completely ignore liking images from certain users

session.set_ignore_users(['random_user', 'another_username'])
```

### Ignoring Restrictions

```python
#will ignore the don't like if the description contains
# one of the given words

session.set_ignore_if_contains(['glutenfree', 'french', 'tasty'])
```

### Excluding friends

```python
#will prevent commenting on and unfollowing your good friends (the images will still be liked)

session.set_dont_include(['friend1', 'friend2', 'friend3'])
```

### Follow/Unfollow/exclude not working?
If you notice that one or more of the above functionalities are not working as expected - e.g. you have specified:
```python
session.set_do_follow(enabled=True, percentage=10, times=2)
```
but none of the profiles are being followed - or any such functionality is misbehaving - then one thing you should check is the position/order of such methods in your script. Essentially, all the ```set_*``` methods have to be before ```like_by_tags``` or ```like_by_locations``` or ```unfollow```. This is also implicit in all the exmples and quickstart.py

### Emoji Support  
To use an emoji just add an `u` in front of the opening apostrophe:

```
session.set_comments([u'This post is 🔥',u'More emojis are always better 💯',u'I love your posts 😍😍😍']);
or
session.set_comments([u'Emoji text codes are also supported :100: :thumbsup: :thumbs_up: \u2764 💯💯']);
```

Emoji text codes are implemented using 2 different naming codes. A complete list of emojis codes can be found on the [Python Emoji Github](https://github.com/carpedm20/emoji/blob/master/emoji/unicode_codes.py), but you can use the alternate shorted naming scheme found for Emoji text codes [here](https://www.webpagefx.com/tools/emoji-cheat-sheet). Note: Every Emoji has not been tested. Please report any inconsistancies.

> **Legacy Emoji Support**  
>
> You can still use Unicode strings in your comments, but there are some limitations.
> 1. You can use only Unicode characters with no more than 4 characters and you have to use the unicode code (e. g. ```\u1234```). You find a list of emoji with unicode codes on [Wikipedia](https://en.wikipedia.org/wiki/Emoji#Unicode_blocks), but there is also a list of working emoji in ```/assets```  
>
> 2. You have to convert your comment to Unicode. This can safely be done by adding an u in front of the opening apostrophe: ```u'\u1234 some comment'```

## Running on a server

Use the `nogui` parameter to interact with virtual display

```
session = InstaPy(username='test', password='test', nogui=True)
```

### Automate

You can add InstaPy to your crontab, so that the script will be executed regularly. This is especially useful for servers, but be sure not to break Instagrams follow and like limits.

```
# Edit or create a crontab
crontab -e
# Add information to execute your InstaPy regularly.
# With cd you navigate to your InstaPy folder, with the part after && you execute your quickstart.py with python. Make sure that those paths match your environment.
45 */4 * * * cd /home/user/InstaPy && /usr/bin/python ./quickstart.py
```

## Clarifai ImageAPI
<img src="https://d1qb2nb5cznatu.cloudfront.net/startups/i/396673-2fb6e8026b393dddddc093c23d8cd866-medium_jpg.jpg?buster=1399901540" width="200" align="right">

###### Note: Head over to [https://developer.clarifai.com/signup/](https://developer.clarifai.com/signup/) and create a free account, once you’re logged in go to [https://developer.clarifai.com/account/applications/](https://developer.clarifai.com/account/applications/) and create a new application. You can find the client ID and Secret there. You get 5000 API-calls free/month.

If you want the script to get your Clarifai_ID and Clarifai_Secret for your environment, you can do:

```
export CLARIFAI_ID="<ProjectID>"
export CLARIFAI_SECRET="<Project Secret>"
```
### Example with Imagecontent handling

```python
from instapy import InstaPy

InstaPy(username='test', password='test')\
  .login()\
  .set_do_comment(True, percentage=10)\
  .set_comments(['Cool!', 'Awesome!', 'Nice!'])\
  .set_dont_include(['friend1', 'friend2', 'friend3'])\
  .set_dont_like(['food', 'girl', 'hot'])\
  .set_ignore_if_contains(['pizza'])\
  .set_use_clarifai(enabled=True)\
  .clarifai_check_img_for(['nsfw'])\
  .clarifai_check_img_for(['food', 'lunch', 'dinner'], comment=True, comments=['Tasty!', 'Nice!', 'Yum!'])\
  .like_by_tags(['dog', '#cat'], amount=100)\
  .end()
```
### Enabling Imagechecking

```python
#default enabled=False , enables the checking with the clarifai api (image tagging)
#if secret and proj_id are not set, it will get the environment Variables
# 'Clarifai_SECRET' and 'CLARIFAI_ID'

session.set_use_clarifai(enabled=True, secret='xyz', proj_id='123')
```
### Filtering inappropriate images

```python
# uses the clarifai api to check if the image contains nsfw content
# -> won't comment if image is nsfw

session.clarifai_check_img_for(['nsfw'])
```
### Specialized comments for images with specific content

```python
#checks the image for keywords food and lunch, if both are found,
#comments with the given comments. If full_match is False (default), it only
# requires a single tag to match Clarifai results.

session.clarifai_check_img_for(['food', 'lunch'], comment=True, comments=['Tasty!', 'Yum!'], full_match=True)
```

###### Check out [https://clarifai.com/demo](https://clarifai.com/demo) to see some of the available tags.</h6>

## Running with Docker microservices manual

Docker allows very easy and fast run instapy bot without any pain and tears.

### 0. Preparations

Install docker from official website [https://www.docker.com/](https://www.docker.com/)

Install VNC viewer if you haven't one. For windows good program is  [http://www.tightvnc.com/](http://www.tightvnc.com/)

### 1. Set your instagram login and password

Open `docker_quickstart.py` and fill quotes after insta_username and insta_password with your credentials.

Don't forget to make other changes for the file as you want to. Read documentation above for info.

### 2. Run and build containers with docker-compose

First you need to open your terminal, move to the root folder (usually with `cd` command) of instapy project and then type:
```bash
docker-compose up -d --build
```

That's all! At this step you are already successfully run you personal bot!

### 3. See what your bot do right now 

Run your VNC viewer, and type adress and port `localhost:5900`. The password is `secret`.

### 4. Stop your instapy bot

Use your terminal again, type in the same window:
```bash
docker-compose down
```

Your bot is stopped!

### 5. Further steps

That's just a basic steps for run instapy bot on your PC with docker. There are other docker-compose settings file in the root of project.

#### Development environment to run, test and debug by SSH

Use it to help us with development and test instapy! `docker-dev.yml` file.

```bash
docker-compose -f docker-dev.yml up -d
```

After strike this command, you can access your bot by VNC on the adress  `localhost:5901`, the password is `secret`. 

But there is more! There is fully accessible bash console with all code mounted at the path `/code`. When you hack some files they are dynamically updated inside your container.

To access yor container console to run bot type `localhost:22` in your favorite ssh client. The User is `root` and the password is `root` also.

#### Run in production without opened VNC port

 Suitable to run in a remote server. Attention! You can not view what happened through VNC on this configuration `docker-prod.yml` file.

```bash
docker-compose -f docker-prod.yml up -d
```

## Running all-in-one with Docker (obsolete)

### 1. Build the Image

First you need to build the image by running this in the Terminal:
```bash
docker build -t instapy ./docker_conf/all_in_one/Dockerfile
```

Make sure to use the `nogui` feature:
```python
#you can use the nogui parameter to use a virtual display

session = InstaPy(username='test', password='test', nogui=True)
```

### 2. Run in a Container

After the build succeeded, you can simply run the container with:
```bash
docker run --name=instapy -e INSTA_USER=<your-user> -e INSTA_PW=<your-pw> -d instapy
```

---
###### Have Fun & Feel Free to report any issues
