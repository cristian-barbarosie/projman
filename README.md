# projman

This place is dedicated to `projman`, a `python` script for propagating files and keeping them
up-to-date between different physical supports.

I started working on `projman` around 2000 and used it for a while for my own files,
then around 2010 I abandonned the idea.
In 2020 I decided to resume its development.

`git` is a well-known software which can be viewed as a competitor of `projman`
(or the other way around, if you wish), so I will try to give a quick idea about what `projman`
is and does by comparing it to `git`.

There are two main flavors of `projman` : with tracking activated and with no tracking.

#### Comparison between `git` and `projman` with no tracking

* `git` keeps old versions of the files, `projman` does not.
Thus, there are no branches in `projman`.

* `git` offers operations `pull` and `push`.
In `projman` there is only one such operation, called `sync`.

* `git status` warns you if a change has not been `add`ed by analysing the content of all files in
the current directory. (subdirectories too ?)
`projman` never looks inside the files, so it cannot tell you that.

* `git` registers changes in a two-stage process (whose usefulness is not entirely clear to me) :
`add` then `commit`.
In `projman` there is only one stage called `register`.

* You need to inform `git` about changes to a file __after__ you have edited (and saved) your file.
You change it again, you have to `add` it again to `git`.
With `projman`, you give this information only once, before or after you modify the file.
Again, `projman` never looks inside the files, so it does not matter if you have already saved
the file or if you make further changes later.
There is no such thing as `commit -a` in `projman`.
Of course, you must save all modified files before you `sync` them to some other physical support.
Also, after a `sync` operation, you must `register` any new changes again.

* When propagating changes, `git` tries to solve conflicts through a tree-way `diff`.
`projman` never looks inside the files, it only uses a system of tokens.
If changes have been made to the same file on different repositories,
`projman` will bluntly report a conflict which you will have to solve manually.

* `projman` treats a whole directory as one file only, that is, when `sync`hronizing, it looks at
the tokens describing the modification history of that directory and if it finds that they
are up-to-date, it does not descend into that directory for further analysis.
So, for instance, if you give a `sync` command between two mirrors of the same project
which are up-to-date, `projman` is extremely fast in deciding not to take any action
because it only compares tokens relative to the root directory.
Perhaps `git` is similar in that respect.

* `projman` never looks at timestamps of files or at the current time, 
so there is no need to synchronize clocks.
I believe `git` is similar in that respect.

#### Comparison between `git` and `projman` with tracking activated

* `git` keeps old versions of the files, `projman` does not.
Thus, there are no branches in `projman`.

* `git` offers operations `pull` and `push`.
In `projman` there is only one such operation, called `sync`.

* `git` registers changes in a two-stage process (whose usefulness is not entirely clear to me) :
`add` then `commit` (or the shortcut `commit -a`).
In `projman` there is only one stage called `register` but it is seldom used if tracking is active.
`git commit -a` is roughly equivalent to doing nothig in `projman` if tracking is active.

* Just as for `.gitignore`, the file `.projman/ignore` is important for controlling what will be propagated.

* `git status` warns you if a change has not been `add`ed by analysing the content of all files in
the current directory. (subdirectories too ?)
`projman` never looks inside the files, but it does compare file timestamps with the time of
the latest `sync` operation.
It uses the timestamp of a directory and, if that timestamp looks old enough,
it will not analyse further timestamps of files or subdirectories therein.
However, it never compares timestamps between remote machines, 
so there is no need to synchronize clocks.

* When propagating changes, `git` tries to solve conflicts through a tree-way `diff`.
`projman` never looks inside the files, it only uses a system of tokens.
If changes have been made to the same file on different repositories,
`projman` will bluntly report a conflict which you will have to solve manually.

* `projman` treats a whole directory as one file only, that is, when `sync`hronizing, it looks at
the tokens describing the modification history of that directory and if it finds that they
are up-to-date, it does not descend into that directory for further analysis.
So, for instance, if you give a `sync` command between two mirrors of the same project
which are up-to-date, `projman` is extremely fast in deciding not to take any action
because it only compares tokens relative to the root directory.
Perhaps `git` is similar in that respect.

#### In short 

`projman` should be useful for maintaining large collections
of files with many subdirectories, when different people make changes independently
of each other but rarely on the same file.
The size of individual files has no impact on `projman`s performance, only their number
(on a logarithmic scale if they are well distributed in a tree of directories).
Binary files are welcome, `projman` does not use `diff`
(actually it never looks at the content of the files).
`projman` does not keep track of previous versions of your files;
if you want that I guess you could somehow use `projman` on top of `CVS` but I'm not sure.

#### Plan of action 

I wrote `projman` in `python2`, so I guess my first move should be to translate it to `python3`.
This may take some time.

Don't look for the source file, I haven't published it yet.
