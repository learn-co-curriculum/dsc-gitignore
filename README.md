# .gitignore

## Introduction

So far our examples have assumed that we want Git to track changes to all files in our repository. In some cases, we want Git to ignore certain files, and this is accomplished using a `.gitignore` file.

## Objectives

You will be able to:

* Describe use cases for a `.gitignore` file
* Interpret and utilize the glob syntax of a `.gitignore`
* Use Git and `.gitignore` to ignore files that Git is currently tracking, or to track files that Git is currently ignoring

## Why `.gitignore`?

If version control systems like Git are designed to track all of your changes, why would we want to tell it to ignore some files? There are three main reasons:

1. Private files
2. Irrelevant files
3. Files that are too large for GitHub

### Private Files

<a title="© 2014 Andreas Kainz &amp; Uri Herrera &amp; Andrew Lake &amp; Marco Martin &amp; Harald Sitter &amp; Jonathan Riddell &amp; Ken Vermette &amp; Aleix Pol &amp; David Faure &amp; Albert Vaca &amp; Luca Beltrame &amp; Gleb Popov &amp; Nuno Pinheiro &amp; Alex Richardson &amp;  Jan Grulich &amp; Bernhard Landauer &amp; Heiko Becker &amp; Volker Krause &amp; David Rosca &amp; Phil Schaf / KDE" href="https://commons.wikimedia.org/wiki/File:Breezeicons-actions-22-view-certificate.svg"><img width="256" alt="Breezeicons-actions-22-view-certificate" src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e1/Breezeicons-actions-22-view-certificate.svg/256px-Breezeicons-actions-22-view-certificate.svg.png"></a>

In some cases, you will need credentials in order for your code to access some service. This includes free services! For example, if you are using a rate-limited API, the API needs to know who is making the request in order to determine whether you have exceeded your limit of requests.

These credentials are often stored in the form of _keys_ or _secrets_, often stored in JSON or YML file formats.

You want to avoid pushing these files to public GitHub repositories, because people have set up bots to crawl GitHub in order to steal these valuable credentials!

Using `.gitignore` you can make sure that your credentials are not pushed.

### Irrelevant Files

<a title="© 2014 Andreas Kainz &amp; Uri Herrera &amp; Andrew Lake &amp; Marco Martin &amp; Harald Sitter &amp; Jonathan Riddell &amp; Ken Vermette &amp; Aleix Pol &amp; David Faure &amp; Albert Vaca &amp; Luca Beltrame &amp; Gleb Popov &amp; Nuno Pinheiro &amp; Alex Richardson &amp;  Jan Grulich &amp; Bernhard Landauer &amp; Heiko Becker &amp; Volker Krause &amp; David Rosca &amp; Phil Schaf / KDE" href="https://commons.wikimedia.org/wiki/File:Breezeicons-actions-22-noisereduction.svg"><img width="256" alt="Breezeicons-actions-22-noisereduction" src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/be/Breezeicons-actions-22-noisereduction.svg/256px-Breezeicons-actions-22-noisereduction.svg.png"></a>

Some of the files generated in the process of coding are not actually relevant for future users of the repository.

These include:

* Log files
* Notebook checkpoint files (e.g. `.ipynb_checkpoints` from Jupyter Notebook)
* OS-specific files (e.g. `.DS_Store`, which is only applicable to Mac computers)
* Configuration files for code editors (e.g. `.vscode`, which is used by VS Code)

If you are working with collaborators, not only are these files _not useful_ to them, they can actually cause _merge conflicts_!

### Files That Are Too Large for GitHub

<a title="© 2014 Andreas Kainz &amp; Uri Herrera &amp; Andrew Lake &amp; Marco Martin &amp; Harald Sitter &amp; Jonathan Riddell &amp; Ken Vermette &amp; Aleix Pol &amp; David Faure &amp; Albert Vaca &amp; Luca Beltrame &amp; Gleb Popov &amp; Nuno Pinheiro &amp; Alex Richardson &amp;  Jan Grulich &amp; Bernhard Landauer &amp; Heiko Becker &amp; Volker Krause &amp; David Rosca &amp; Phil Schaf / KDE" href="https://commons.wikimedia.org/wiki/File:Breezeicons-actions-22-project-development-close.svg"><img width="256" alt="Breezeicons-actions-22-project-development-close" src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/8a/Breezeicons-actions-22-project-development-close.svg/256px-Breezeicons-actions-22-project-development-close.svg.png"></a>

GitHub [limits the size](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github) of files that can be pushed to its repositories. If you try to push a file that exceeds the 100 MB threshold, you will get an error message and the push will fail.

Often in a data science context, these large files are the data files themselves. Sometimes you will find that a compressed file (e.g. `.zip` file) is small enough to be pushed to GitHub, but the expanded version is too big. Then you can specify the expanded version in the `.gitignore` while still being able to share the compressed data on GitHub.

## `.gitignore` Syntax

Now that we understand some reasons that we might want to ignore certain files, how does that actually work?

To start with, you put a hidden text file called `.gitignore` at the root of the repository. It is hidden because it starts with `.`, which means that in order to view it you will need to type `ls -a` rather than just `ls` in the terminal. We demonstrate the `.gitignore` in this very repository below:


```python
!ls
```

    CONTRIBUTING.md LICENSE.md      README.md       index.ipynb



```python
!ls -a
```

    [34m.[m[m                  .gitignore         LICENSE.md
    [34m..[m[m                 [34m.ipynb_checkpoints[m[m README.md
    [34m.git[m[m               CONTRIBUTING.md    index.ipynb


### Basic Syntax

The file's content is just a list of things to ignore, as well as whitespace and comments (which start with `#`). The items in the list are separated by newlines.

For example, this could be the contents of a valid `.gitignore` file:

```
# a comment
secrets.json

# another comment
secrets.yml
```

This means that if the repository contained a file called `secrets.json` and/or a file called `secrets.yml`, Git would ignore them.

### Glob Syntax

`.gitignore` can also work with wildcard characters using [glob syntax](https://en.wikipedia.org/wiki/Glob_(programming)). For example, if you wanted to ignore all files ending with `.csv`, the `.gitignore` might look like this:


```
# ignore all CSV files
*.csv
```

Or, if you only wanted to ignore only CSV files in the `data/` folder, that might look like this:

```
# ignore CSV files in the data/ folder
data/*.csv
```

For more examples with comments, see the [Git documentation](https://git-scm.com/docs/gitignore).

### Useful Defaults

The examples above were specific to a particular project, and you will typically need to write your own `.gitignore` lines for these specialized use cases.

Outside of these narrow use cases, there are resources available for developing `.gitignore` files that are appropriate for your type of project in general. For example, GitHub maintains a [Python `.gitignore` template](https://github.com/github/gitignore/blob/main/Python.gitignore) (this can be added to your repository by selecting it from the drop-down when you initialize a repository on GitHub). There is also a tool called [gitignore.io](https://gitignore.io) that will help you identify useful `.gitignore` lines for your operating system or code editor. It's usually a good idea to use these resources first, before trying to write your own glob syntax!

### Ignoring a Tracked File

If you previously used `git add` on a file then ran `git commit`, that means that the file is already tracked, regardless of what the `.gitignore` says.

#### Irrelevant Files

If the file is just irrelevant for your collaborators, you can tell Git to stop tracking it. For example, if you wanted to tell Git to stop tracking `log.txt`, you could use this command:

```bash
git rm --cached log.txt
```

Note that this is a command you run in the terminal, _not_ a line to be added to the `.gitignore` file. For the changes to be reflected on GitHub, you will also need to run `git commit` and `git push`.

> In general `git rm` is a useful command that deletes a file from the directory and stages that file deletion with Git (i.e. combines the `rm file_name` and `git add file_name` steps). Adding `--cached` to `git rm` means that Git will no longer track the file, but that the file will not actually be deleted from the directory.

(You also probably want to add that file name to the `.gitignore` at the same time!)

#### Private or Too Large Files

If the file is private/secret or too large for GitHub, you will need to rewrite your Git history. This can be fairly complicated (see [this blog post](https://medium.com/analytics-vidhya/tutorial-removing-large-files-from-git-78dbf4cf83a?sk=c3763d466c7f2528008c3777192dfb95) for more details) so it's always better to add things to `.gitignore` sooner rather than later!

### Telling Git Not to Ignore

Many developer setups will include a [global `.gitignore` file](https://sebastiandedeyne.com/setting-up-a-global-gitignore-file/). This file is not located within a specific repository, and can be especially useful for OS-related settings (e.g. ignoring `.DS_Store` on a Mac).

Sometimes you might want to override these global settings for the current repository, particularly if your global `.gitignore` is relatively broad. To do that, you use the same glob syntax, placing a `!` at the beginning.

So, for example, if you want Git _not_ to ignore `log.txt` in a particular repository, you could add this to the `.gitignore`:

```
# do not ignore log.txt
!log.txt
```

## Summary

Sometimes you want Git to ignore certain files. The common reasons for this are that the files are private, the files are irrelevant to collaborators, or that the files are too big for GitHub. To tell Git to ignore the files, you use a hidden file called `.gitignore` at the root of the repository. This file uses glob syntax to specify which files should be ignored, including `*` to indicate a wildcard and `!` to indicate that a file should _not_ be ignored. We recommend that you use useful defaults rather than writing your own `.gitignore` lines most of the time, and that you always make sure to add private or too-large files to the `.gitignore` as soon as possible so you don't have to rewrite your Git history.
