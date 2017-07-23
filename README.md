# Keeping Dotfiles and Permissions in git

## Idea

This little snippet will help you keeping track of your dotfiles as a bare git
repository. It utilizes
[setgitperms.pl](https://github.com/git/git/blob/master/contrib/hooks/setgitperms.perl)
from git's contrib directory to manage permissions.

It was inspired by a [Atlassian developer blog entry](https://developer.atlassian.com/blog/2016/02/best-way-to-store-dotfiles-git-bare-repo/).

## HowTo

1. Fork and clone this repository (I assume that the path to you repository will
   be available as `$REPO`).
2. Either add `setgitperms.perl` to `.dothooks` (or simply reference it by path
   if you are sure that it will be at the same location on all your machines).
   Make sure that it's executable! Don't forget to commit and push after this :-)
3. Clone the modified repository to `~/.dotfiles` as a bare copy and protect it
   from access:

```
git clone --bare $REPO $HOME/.dotfiles
chmod 0700 $HOME/.dotfiles
```

4. Define an alias, checkout your data, enable hooks, fix permissions:

```bash
alias dot='git --git-dir="$HOME/.dotfiles" --work-tree="$HOME"'
dot checkout # for errors, see the Atlassian blog entry
dot config --local core.hooksPath $HOME/.dothooks
dot config --local status.showUntrackedFiles no
dot checkout # this time with hooks
```

5. Add the `dot` alias to your shell's initialization files and manage your
   dotfiles like any other git repository, only while typing `dot` instead of
   `git`.

## Caveats

Don't push any (dot)files containing passwords or any other data you don't want
to share to github - they will be world readable there.

Good luck and have fun.
