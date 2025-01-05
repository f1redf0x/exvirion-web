+++
title = 'Using fuzzy finder as an alternative file browser!'
date = 2025-01-04T16:43:38-07:00
draft = false
+++

Ok, this one is nerdy and short, but it's made my life immeasurably better. There is a utility called fuzzy finder on most unix-based systems. It's purpose is simple... it fuzzy finds.

Okay... but what does that mean?

Usually when you search for something, you typically do "string matching". You search for "photo" and if it's got the word photo in the name, it'll show up. Easy enough.

But what if the file you want has "Photo" in the name? That capital P is going to throw it off.

Yeah, you could use Regular expressions, which will allow you to write rules that make it easy to find something close. `*hoto` will yield both photo and Photo for example, but as anyone who's worked with regular expressions knows, it can get super complicated.

Fuzzy finding is lazy matching. If you type anything remotely like photo, it'll give you a list of all the things that share a few characters with your search. So looking for "photo" might give you:

* photo
* Photo
* PHOTO
* PHoto
* PHPto
* and literally anything in the ballpark.

It sorts the list based on likelihood. Now, I'm exaggerating a little bit about the PHP one, but it'll surprise you with how much it'll give you.

Now, how the heck is that useful? 

The way the fzf tool works is it pulls up a TUI (Text User Interface) menu of all the files close to the thing you want. Once you select a file, it dumps it into standard out.

What the heck does that mean?

It means that you can take the file name of a fuzzy search and dump that file into another tool. Here are a couple of use cases I use it for:

# Playing my favorite songs:

```
alias mu='results=$(find ~/Music -type f | fzf) && mpv "$results"'
```

The above is an alias in my ~/.zshrc file. When I type `mu` into my terminal and hit enter, the following happens:
1. The find command opens my Music folder
2. all of my music files get piped into fzf.
3. fzf gives me a nice interface to start typing in song titles.
4. Once it finds the song I want, I hit enter.
5. fzf send the filename to standard out.
6. The results variable grabs it from SO
7. mpv starts playing the file.

If I have a song I want to listen to, it's literally as easy as:

1. type "mu".
2. type "Cele".
3. hit enter.

And I'm listening to my favorite song, "Keeper of the Celestial Flame of Abernathy" from the terminal.

I DEFY YOU to listen to your favorite song faster. In fact, if I'm braced for the challenge, with the hands of the keyboard, I sincerely believe that I could get to my favorite song faster than Siri or Alexa could get to yours.

# Playing entire albums:

Yoy might think that the above was pretty nice, but you might be thinking that it might be cool to play an entire album instead of a song. I got you too!
```
alias md='results=$(find ~/Music -type d | fzf) && mpv "$results"'
```

The only difference in this version is that I'm telling find to give fzf a list of directories. My music happens to be in directories named after the artist and album names, so it's easy for fzf to identify the album.

1. Type "md".
2. Type "of Fife"
3. hit enter.

I'm now listening to the album "Return to the Kingdom of Fife". by Gloryhammer. And I don't even have to repeat myself because the AI voice assistant didn't hear me.

# Editing Documents:

One thing that's kind of annoying is you might have a couple documents with similar names, but you want to know which one you're editing before you open it.

```
alias docs='results=$(find ~/Documents -type f | fzf --preview "bat --color=always {}") && vim "$results"'
```

You're probably seeing the pattern here.
1. The find command pulls a list of all my documents.
2. All my docs get piped to fzf.
3. fzf provides me a list on the left of the documents' contents. As I scroll up and down, It shows me a preview.
4. When I select the file, fzf sends it to standard out.
5. The results variable catches the document name.
6. Vim opens the document that's stored in results.

Getting to see all of my files before I open them is so convenient. I don't even usually open up my file browser anymore because of this command.

# Potential Improvements:

I'm not even scratching the surface of the potential here. The ability to very quickly go through all the files in a directory turns your terminal into a search engine for local files.

## Ripgrep

Ripgrep is a tool that recursively (starts at the bottom and goes through all the files until it hits the bottom) searches through your files for key phrases. If you don't want to search titles, but search content, you can use ripgrep to search all the files for a keyword, then present the filenames to fzf so you can figure out which one it the file you want. The following is an excerpt from one of my scripts that does just that:

```
cat $(rg "$1" "${NOTES_DIRECTORY}" | awk -F':' {'print $1'} | sort -u| fzf --preview 'bat --color=always {}')
```

## other preview types
Maybe you want to preview pics with sxiv or pdfs with Zathura. I think you can do that (though I haven't really had a need to try). I don't know what kind of previews the fzf tool offers, but I'm sure it's more advanced than just using bat.

## Launching applications
I opened text files and music files, but you can be more adventurous. It takes the name of any file in Linux and pipes it to standard out.

...
You know what's a file in Linux?
...
Freaking everything. Your camera is a file, your drive is a file, your microphone is a file. There are even files that just act as a place for to files to talk together (pipefile). You can grab any of them and pipe them into any other thing. It's like a file specific version of dmenu, only just for the terminal.

Seriously, you should play around with fzf. It's lots of fun.
