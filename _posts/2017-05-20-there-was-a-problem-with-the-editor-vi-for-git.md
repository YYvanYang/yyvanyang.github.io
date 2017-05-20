# Fixed "There was a problem with the editor vi" for Git

Today I've discovered an error after an installation of Yarn package via brew (`brew install yarn`). It seems also upgrade all the other packages(e.g., Git). This error happened when Git uses Vi to edit commit message.

This message is fired every time i write the message and quit vim. Pretty annoying!

Here is the error :

```
error: There was a problem with the editor 'vi' 
```

After a little research i've found a damn easy:

```
$ git config --global core.editor /usr/bin/vim
```