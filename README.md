![cooltext399899734189845](https://user-images.githubusercontent.com/86920820/145670160-be3ab034-6af2-412f-8e19-910007c75e06.png)

# [github visitors badge](https://github-visitors-badge.glitch.me) ![visitor badge](https://github-visitors-badge.glitch.me/badge?page_id=SenuGamerBoy.github-visitors-badge&left_text=MyPageVisitors)
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2FSenuGamerBoy%2Fgithub-visitors-badge.svg?type=shield)](https://app.fossa.com/projects/git%2Bgithub.com%2FSenuGamerBoy%2Fgithub-visitors-badge?ref=badge_shield)

A badge generator service to count visitors of your markdown file.

Hello every one!

In this post, I will tell you the story of me to creating the github-visitors-badge, it's a svg image that can count your visitors for your GitHub README.md, issues, PRs in just one line markdown code.

# Why
All the story starting from I migrate all my blog posts from Hexo GitHub Pages to a GitHub issue based repository. After a painful migration, I found that there is no visitor tracking for the repository, though basically I myself am the only one visitor in most time :( , I still want a visitor counting service for my every GitHub Issue and the README.

# How
After a lot of searching, brainstorming, prototyping, I put my eye on the little badge in many other repository. Those ```badges``` can show us:

- How much stars of the repository
- How much opened issues
- How much PRs
- ...


and all the badges is just a svg image file with a dynamic content in it.

After more searching, I found the ```pybadge``` library which will generate a GitHub badge style dynamically with a very simple api.

So I can setup a python server, receive a svg file request, generate a dynamic svg file, return it, so it will display on the README.md.

What else? A database to store the previous count of each page so that in the next time the same request from the page received, I can increment 1 based on the previous count.

Here it is: ```https://countapi.xyz/```, a free counting API allows you to create simple numeric counters. IaaS, Integer as a Service.

CountAPI is a perfect chosen for this use case, and it's easy to use, I don't need to prepare a database(SQLite, MySQL, etc.), I just send a http request to the API, and can get the incremented number.

Also can avoid concurrent updating issue if too many visitors to your page in the same time, it will count them correctly, because the CountAPI is based on Redis.

So, for now, we have all we needed, just coding:

[![Readme Card](https://github-readme-stats.vercel.app/api/pin/?username=SenuGamerBoy&repo=github-visitors-badge)](https://github.com/SenuGamerBoy/github-visitors-badge)


# Some tricks
Why you have to pass a ```page_id``` as a query parameter?
For the first version, I plan to use the ```Referrer``` header in http request which is more convenient but GitHub proxy all the image request via its camo image server:

> Your browser -> Github Camo -> My server

But Camo does not pass the Referrer header to my server for some reason, so I change to the query parameter solution.

How to deal with the image cache?
As you know, browser often caches images and in our case GitHub Camo also caches images.

Cache is good for us in most time, but for a badge to count visitor, it is a disaster because if the previous badge image are cached, there is no new request to my server and the count will not have chance the increment until the cache is invalid, that's not what we want.

We want every time every one visitor our README.md the count will increment 1, so I did a trick thing:

Disable cache by adding a response header: ```'Cache-Control': 'no-cache,max-age=0'```

Set a passed expire time to 10 minutes AGO of current time: ``` 'Expires': <10 minutes ago> ```

That's it, after this little tricks every time you visitor the README.md, the browser(and camo) will know that the cached image is invalidated then send a request to my server to get the latest count.

Examples:

- default style

```markdown
![visitor badge](https://github-visitors-badge.glitch.me/badge?page_id=SenuGamerBoy.github-visitors-badge)
```

![visitor badge](https://github-visitors-badge.glitch.me/badge?page_id=SenuGamerBoy.github-visitors-badge)

- customized left text (default is `visitors`)

```markdown
![visitor badge](https://github-visitors-badge.glitch.me/badge?page_id=SenuGamerBoy.github-visitors-badge&left_text=MyPageVisitors)
```
![visitor badge](https://github-visitors-badge.glitch.me/badge?page_id=SenuGamerBoy.github-visitors-badge&left_text=MyPageVisitors)

- customized left text with a space between words

```markdown
![visitor badge](https://github-visitors-badge.glitch.me/badge?page_id=SenuGamerBoy.github-visitors-badge&left_text=My%20Page%20Visitors)
```
![visitor badge](https://github-visitors-badge.glitch.me/badge?page_id=SenuGamerBoy.github-visitors-badge&left_text=My%20Page%20Visitors)

- customzied color

```markdown
![visitor badge](https://github-visitors-badge.glitch.me/badge?page_id=SenuGamerBoy.github-visitors-badge&left_color=red&right_color=green) 
```

![visitor badge](https://github-visitors-badge.glitch.me/badge?page_id=SenuGamerBoy.github-visitors-badge&left_color=red&right_color=green) (left_color=red, right_color=green)

- customized color and left text

```markdown
![visitor badge](https://github-visitors-badge.glitch.me/badge?page_id=SenuGamerBoy.github-visitors-badge&left_color=red&right_color=green&left_text=HelloVisitors)
```

![visitor badge](https://github-visitors-badge.glitch.me/badge?page_id=SenuGamerBoy.github-visitors-badge&left_color=red&right_color=green&left_text=HelloVisitors) <code>(left_color=red, right_color=green, left_text=HelloVisitors)</code>

- customized color and a space between words in left text

```markdown
![visitor badge](https://github-visitors-badge.glitch.me/badge?page_id=SenuGamerBoy.github-visitors-badge&left_color=red&right_color=green&left_text=Hello%20Visitors)
```
![visitor badge](https://github-visitors-badge.glitch.me/badge?page_id=SenuGamerBoy.github-visitors-badge&left_color=red&right_color=green&left_text=Hello%20Visitors) <code>(left_color=red, right_color=green, left_text=Hello%20Visitors)</code>

# CONTRIBUTING

Contributions are very much appreciated!

Pull requests should be based on and submitted to the "main" branch

Please raise an issue to discuss what you plan to implement or change before 
you start if it is going to involve a lot of work on your part.

Please keep pull requests specific, do not make many disparate changes or
new features in one request.  A separate pull request for each feature change
is preferred.

Please ensure your changes work in Python 3.7+ 

Please add your github Username to the AUTHORS file

## powered by

<img src="https://user-images.githubusercontent.com/86920820/145670327-27b47a1f-1bf8-4642-a0d2-8f44192ef1b8.png" width="240"></a>

## License
Code released under [The GNU General Public License](LICENSE).


[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2FSenuGamerBoy%2Fgithub-visitors-badge.svg?type=large)](https://app.fossa.com/projects/git%2Bgithub.com%2FSenuGamerBoy%2Fgithub-visitors-badge?ref=badge_large)