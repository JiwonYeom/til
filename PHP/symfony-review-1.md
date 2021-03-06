## Reviews on Trying out Symfony(1)
I have been trying out Symfony today, and while I was working on it, I faced some difficulties and issues.
I decided to leave some progress review on this.

#### Installing Symfony
This is simple if you follow the [official guide](https://symfony.com/doc/current/setup.html). But one thing to remember: `symfony` command might NOT work if you don't turn off & on your terminal. Mine didn't. If your command doesn't work, turn your terminal off & on again.
> symfony new [projectName]

Should be enough to create your new project. Do NOT forget to change your root in `nginx.conf` file (If you are doing this on a nginx web server).

#### The console
Symfony can be managed by its own console and its commands, such as running its server or checking status, etc.

* Useful commands
```
//check routed url list
php bin/console debug:router

//refresh cache
 php bin/console cache:clear --env=prod

```

#### File Directory
* src-AppBundle contains Controller.
* Resources-views contains view files.
Which are two main places where you might want to begin.

#### Twig engine
Symfony uses html twig engine for its view as long as I have seen. I skimmed through the configs, and it seems like different rendering engine can be chosen, but this is interesting for me. So far, PHP frameworks that I used had plain HTMLs as its view templates.

#### Routing
Routing is quite different from that of CI or Drupal, which are frameworks that I am familiar with. It uses a sort of *annotation*, which looks like the following
```
/**
 * @Route("/happy/name")
 */
```
This defines url pattern. But I think the file name and controller / function names are correlated since I got many errors while trying to get this work, and it seemed like the file name and function names must be right.
This might be a mistake, so I'll try to update if it turns out differently.

#### Unresolved Issues (trying to find out)
* I am having issues (or this might not be an issue) with cache. From the beginning, when I was trying to see if my page has changed after changing some parts in `index.html.twig`, it didn't change.
Newly added controller wasn't recognized too, until I did this:
>php bin/console cache:clear --no-warmup --env=prod

I'm not sure if this is something that I must do every time I do some changes. There might be some way, so I'll keep on searching.

* Debugging
It said debugging toolbar should show up, but I haven't seen it yet :(
I think I might have set up something wrong, and trying to figure this out.
**update!**
==> I found this out. My website seems like it's on a production mode instead of development mode. I accessed app_dev.php by explicitly using its url, and the bar showed up.
I think this is because of my web server setting, which is not the built-in php server. I'll have to check up a bit more.

* Dev & Prod
I think Symfony divides these two quite strictly. I'm still trying to find out the exact mechanism, but there are two files -`app.php` and `app_dev.php`. While I was setting up `nginx.conf`, it also separated these two environments, which is good. But I should take more in-depth look to see exact settings and usages.

* Memory issue
While I was trying to install FOS\\rest-bundle, I encountered memory issue. Allocated memory exceeded, so I tried to increase the `memory_limit` parameter in `php.ini` file to 1G, but it didn't work.
I though there might be another issue, but today I tried it again by putting `-1` as its value. As a result, the memory alert was gone but now I had allocation exception in `Composer`. This was solved after I entered few commands suggested [here](https://getcomposer.org/doc/articles/troubleshooting.md#proc-open-fork-failed-errors)
