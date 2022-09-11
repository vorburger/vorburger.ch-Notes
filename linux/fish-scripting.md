# Fish is a **GREAT** interactive shell, but... not yet suitable for scripting! :-((

I *almost* :) [started writing some shell scripts in Fish instead of Bash](https://github.com/vorburger/LearningLinux/compare/fish-scripts),
but then discovered https://github.com/fish-shell/fish-shell/issues/510.

This seems like a massive feature gap in Fish, at least to me.   I **CANNOT LIVE** without `set -e` in Bash.
[ALL my scripts everywhere have that](https://github.com/search?q=user%3Avorburger+%22set+-e%22&type=code) (code search seems to miss a lot of them).

Fish is great as an interactive Shell, but without this feature, I cannot start using it for more scripting.

I don't see any viable workaround.  Ending **EACH** line with `|| exit 1` is just... dumb.

Perhaps I'll have to properly learn and use... what, Pythonm, after all??? :))
