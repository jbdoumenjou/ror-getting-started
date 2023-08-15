# On the Road to Adventure

The time has come to discover new adventures, to explore new spaces. So I'm about to enter uncharted territory, or to be precise, a land I only glimpsed over 10 years ago, Ruby on Rails.
But first, let's go to the [getting started page](https://guides.rubyonrails.org/getting_started.html).

# First step

Discovering Ruby On Rails requires a few prerequisites, which I'm about to validate on my good old Manjaro Linux distribution.

## Is Ruby installed ?
```shell
ruby --version
ruby 3.0.5p211 (2022-11-24 revision ba5cf0f7c5) [x86_64-linux]
```
**Yes**

## IS SQLite installed ?

```shell
sqlite3 --version
3.42.0 2023-05-16 12:36:15 831d0fb2836b71c9bc51067c49fee4b8f18047814f2ff22d817d25195cf3alt
```

**Yes**


All that's left to do is install rails

```shell
gem install rails
```

## Is Rails successfuly installed ?

```
rails --version                                                                 
zsh: correct 'rails' to '_rails' [nyae]? 
```

**No**, damned...
Let's check the installation log:

```shell
WARNING:  You don't have /home/jbd/.local/share/gem/ruby/3.0.0/bin in your PATH,
          gem executables will not run.
```

Sounds like a good avenue to investigate.

Let's edit our PATH to add gem executable
```shell
export PATH=/home/jbd/.local/share/gem/ruby/3.0.0/bin:$PATH
```

Now, we can retry

```shell
rails -version                                                                        Rails 7.0.6
```

Tada !! \o/

As I was afraid of the gem configuration, I find a command to check the environement:

```shell
gem environment
```

# Let's Play with RoR

## Create a New Blog Application

```shell
rails new blog
```

Wow, it creates a full structure with a lot of files, and sadly it outputs errors.

```shell
# ...
Bundler::PermissionError: There was an error while trying to write to `/usr/lib/ruby/gems/3.0.0/cache`. It is likely that you need to grant
write permissions for that path.
# ...
```

After reading https://bundler.io/doc/troubleshooting.html, some articles on Stack Overflow and Github.
I realized that I have several choices:

* use sudo to run the command: which seems a bad idea to use root privilege for something I don't master. 
* use sudo to install bundler, in my opinion, it is not better than the previous solution.
* change the permission of that folder so that the current user may write in to it, still not a fan.
* install bundler for the user and update the path, it seems a way better,
it avoids touching the system installation and giving rights without knowing what they're going to be used for.





