---
layout: post
title:  "Gitblit working with go get command"
date:   2015-04-26 12:59:48
categories: git golang gitblit groovy
---

### Summary

What required to make it work:

- set your `~/.gitconfig`: `git config --global url."git@domain.io:".insteadOf git://domain.io`
- having a ssh access to the server git account
- project must use the groovy hook

### Wants to work on the project

**Never work on the branch given by go get!** 

simply set the git url to the gitblit one:

{% highlight sh %}
```
git remote set-url origin ssh://username@domain.io:portnum/~ownername/project_name.git
```
{% endhighlight %}

and you're done!

### How it works? Why so much pain?

`go get` command manage your project dependencies and expect url like:

{% highlight sh %}
domain.io/username/project_name.git
{% endhighlight %}

in your `import` directives or in the CLI arguments,

and will try the following options -in this order-:

{% highlight sh %}
git://domain.io/username/project_name.git
https://domain.io/username/project_name.git
http://domain.io/username/project_name.git
{% endhighlight %}

but private repositories in `gitblit` have the following url syntax:

{% highlight sh %}
ssh://username@domain.io:portnum/~ownername/project_name.git
{% endhighlight %}

and as you can guess `go get` doesn't understand this kind of syntax,

So the plan is to clone the repository using a post-received hook, to the traditional git server over ssh, so the syntax will be:

{% highlight sh %}
git@domain.io/git/ownername/project_name.git
{% endhighlight %}

this syntax is not understood by `go get`, to fix that add this to your `~/.gitconfig`:

{% highlight sh %}
[url "git@domain.io:"]
    insteadOf = git://domain.io
{% endhighlight %}

this way the previous url is used as:

{% highlight sh %}
git://chaahk.com/git/ownername/project_name.git
{% endhighlight %}

Great!

here is the groovy script I use -yes gitblit allow hooks only in groovy script-:

{% highlight java %}
import com.gitblit.GitBlit
import com.gitblit.Keys
import com.gitblit.models.RepositoryModel
import com.gitblit.models.TeamModel
import com.gitblit.models.UserModel
import com.gitblit.utils.JGitUtils
import com.gitblit.utils.StringUtils
import java.text.SimpleDateFormat
import org.eclipse.jgit.api.CloneCommand
import org.eclipse.jgit.api.Git
import org.eclipse.jgit.lib.Repository
import org.eclipse.jgit.lib.Config
import org.eclipse.jgit.revwalk.RevCommit
import org.eclipse.jgit.transport.ReceiveCommand
import org.eclipse.jgit.transport.ReceiveCommand.Result
import org.eclipse.jgit.util.FileUtils
import org.slf4j.Logger

// Indicate we have started the script
logger.info("localclone hook triggered by ${user.username} for ${repository.name}")

def rootFolder = '/srv/apps/git'
def bare = true
def cloneAllBranches = false
def cloneBranch = 'refs/heads/master'
def includeSubmodules = false

def repoName = repository.name
def destinationFolder = new File(rootFolder, StringUtils.stripDotGit(repoName))
def srcUrl = 'file://' + new File(gitblit.getRepositoriesFolder(), repoName).absolutePath

// delete any previous clone
if (destinationFolder.exists()) {
        FileUtils.delete(destinationFolder, FileUtils.RECURSIVE)
}

// clone the repository
logger.info("cloning ${srcUrl} to ${destinationFolder}")
CloneCommand cmd = Git.cloneRepository();
cmd.setBare(bare)
if (cloneAllBranches)
        cmd.setCloneAllBranches(true)
else
        cmd.setBranch(cloneBranch)
cmd.setCloneSubmodules(includeSubmodules)
cmd.setURI(srcUrl)
cmd.setDirectory(destinationFolder)
Git git = cmd.call();
git.repository.close()

// set files permissions
"chown -R git: ${rootFolder}".execute()

// report clone operation success back to pushing Git client
clientLogger.info("${repoName} cloned to ${destinationFolder}")
{% endhighlight %}

this is mostly the same script as `$GITBLIT_BASE_FOLDER/groovy/localclone.groovy` except I change the reposirory's owner to git, because I don't want users logging as root using ssh

So save this script in `$GITBLIT_BASE_FOLDER/groovy/` and call it from `post-receive scripts` in each projects receive preference that you want to be clone and accessible thru `go get`

Additionnaly I've made a symlink to `/srv/apps/git` in `/git` to avoid too long path

To keep an eye on those repositories I've keep my hold `cgit` instance on it

<hr/>
Sources:

- [go documentation on import path](http://golang.org/cmd/go/#hdr-Remote_import_paths)
- [git url.insteadOf usage - not official](https://coderwall.com/p/sitezg/force-git-to-clone-with-https-instead-of-git-urls)
