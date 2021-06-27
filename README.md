# projman

This place is dedicated to `projman`, a `python` script for propagating files and keeping them
up-to-date between different physical supports.

I started working on `projman` around 2000 and used it for a while for my own files,
then around 2010 I abandonned the idea.
In 2020 I decided to resume its development.

`git` is a well-known software which can be considered as a competitor of `projman`
(or the other way around, if you wish), so I will try to give a quick idea about what `projman`
is and does by comparing it to `git`.

* `git` keeps old versions of the files, `projman` does not.

* `git` warns you if a change has not been `add`ed by analysing the content of all files in
the current directory. (subdirectories too ?)
`projman` never looks inside the files, so it cannot tell you that.
You can activate tracking in `projman` but even then it does not look at the contents of the
files but only at its timestamp.
It uses the timestamp of a directory and, if that timestamp looks old enough,
it will not analyse further timestamps of files or subdirectories therein.

* `git` registers changes in a two-way process (whose usefulness is not entirely clear to me) :
`add` then `commit`.
In `projman` there is only one stage called `register`.

* You need to inform `git` about changes to a file after you have edited (and saved) your file.
You change it again, you have to `add` it again to `git`.
With `projman`, you give this information only once, before or after you modify the file.
Again, `projman` never looks inside the files, so it does not matter if you have already saved
the file or if you make further changes later.
Of course, after you `sync` your files to some other physical support, you must `register`
any new changes again.
There is no such thing as `commit -a` in `projman`.

* When propagating changes, `git` tries to solve conflicts through a tree-way `diff`.
`projman` never looks inside the files, it only uses a system of tokens.
If two different people have made changes to the same file between two `sync`s,
`projman` will bluntly report a conflict which you will have to solve manually.

* `projman` treats a whole directory as one file only, that is, when doing `sync`, it looks at
the tokens describing the modification history of that directory and if it finds that they
are up-to-date, it does not descend into that directory for further analysis.
So, for instance, if you give a `sync` command between two mirrors of the same project
which are up-to-date, `projman` is extremely fast in deciding not to take any action
because it only compares tokens relative to the root directory.
Perhaps `git` is similar in that respect.

* `projman` only looks at the timestamp of the files if you activate tracking,
and even then it only compares times of local changes and local `register` operations,
so there is no need to synchronize clocks among machines.
I believe `git` never looks at timestamps
(it makes `diff`s on the content of the files for the purpose of tracking).

In short : `projman` should be useful for maintaining large collections
of files with many subdirectories, when different people make changes independently
of each other but rarely on the same file.
The size of individual files does matter for `projman`s performance, only their number
(on a logarithmic scale if they are well distributed in a tree of directories).
Binary files are welcome, `projman` does not use `diff`
(actually it never looks at the content of the files).
`projman` does not keep track of previous versions of your files;
if you want that I guess you could somehow use `projman` on top of `CVS` but I'm not sure.

Plan of action : I wrote `projman` in `python2`, so I guess my first move should be
to translate it to `python3`.
This may take some time.

