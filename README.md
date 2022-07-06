# Git Cheatsheet

Currently mostly about adding existing code into a GitHub repository to start using source control.

July 6, 2022

Right now this cheatsheet covers the steps to add existing code into a new GitHub repository. It’s overly detailed because I want to know the details.

Use case: you’re (\*ahem\*) just starting to learn about using Git to collaborate on software development and you plan to familiarise yourself with that by by putting your own code into GitHub.

(This `README` will be updated as I learn more of the various Git commands and workflows.)

For starters, you need to have Git installed on your computer. (Mine comes from [https://git-scm.com/](https://git-scm.com/))

I’m using Git Bash as the terminal app on the Windows side of my computer, and I also use whichever terminal app it is that comes with [Ubuntu on WSL2](https://ubuntu.com/tutorials/install-ubuntu-on-wsl2-on-windows-10#1-overview). 

## Add existing code to a new GitHub repository

### 1. Make a new repository on GitHub

On your GitHub homepage, click into Repositories and then click the ‘New’ button.

![Cropped screenshot that shows the Repositories tab and the New repository button.](./assets/GitHub-repositories-new_656x104.png)

![Cropped screenshot of the Create a new repository page on GitHub.](./assets/GitHub-create-repository_720x288.png)

- Choose a name for your new repository and (optionally) add a description.
- Choose a Public or Private repository.
- Skip the step that adds a `README` file, a `.gitignore` file, and a `LICENSE` file.

On the next page you’ll see a page with information about your new repository. Keep this page open so you can copy-paste from it in step 4 of this process. You’ll see something like the screenshot here.

![Cropped screenshot of the repo setup info on GitHub.](./assets/GitHub-Quick-setup-crop_920x96.png)

### 2. Generate a GitHub Personal access token

You’ll use the token instead of a password. It’s a long string of text that looks like a complicated password. The token grants access to the repository, and it has an expiry date.

Log in at GitHub, go to the [Settings > Developer Settings > Personal access tokens page](https://github.com/settings/tokens), and click the ‘Generate new token’ button.

- Write a note about the purpose of this particular token.
- In the section titled ‘Select scopes’, select the ‘repo’ scope.
- Click the ‘Generate token’ button.
- The token will be displayed on the next page. Copy the token into your password manager, or leave the page open so you can copy-paste the token in step 5 of this process.

![Cropped screenshot of the personal access token Scope selection options on GitHub.](./assets/GitHub-personal-access-token-scopes_496x232.png)

### 3. Set up the local folder as a Git repository

Set up the local folder to be tracked by Git.

#### 3a. Initialise the local repository

If your code is stored in a folder named `git-cheatsheet`, open a terminal/prompt in that folder.

The following command initialises a Git repository with a branch named `main`. (You could call the branch whatever you like, but let’s stick with common practice.)

```
git init -b main
```

#### 3b. Configure your user name and email address for the new repository

When you push commits from your local repository to the remote repository, the user name and email address you enter here will be recorded as the author of the changes. (You can add the `--global` flag to set the user name and email address for all repositories on your computer as well, if you’re going to be using the same info for all of your projects.)

```
git config user.name "HHH-GH"
git config user.email "EMAIL ADDRESS HERE"
```

The following command lists the current configuration settings.

```
git config --list
```

Type `q` to exit the list. (sometimes `:q`)

#### 3c. Create your .gitignore, README.md, and LICENSE.md files, if needed

The `.gitignore` file tells Git which files to exclude from source control. I’ve often kept old copies of code in a folder named `old`. Those old copies can be excluded from source control with a line like this in the `.gitignore` file.

```
# Ignore anything in the 'old' directory, which were previous versions of files before source control
old/
```

`README.md` is usually an introduction to the project and some notes on how it all works. (Optional but recommended)

`LICENSE.md` is a license for use of the code. (Optional)

#### 3d. Add all the files and do a first commit

Add all the files to start tracking them in source control.

```
git add .
```

Make a first commit.

```
git commit -m "First commit"
```

The `-m` flag means ‘use the text inside the quotes as the commit message’. The commit message should describe any changes made to the files being committed. Leave off the `-m` flag, and Git will open a text editor in which you can write a longer commit message.

### 4. Add the remote GitHub repository as the origin for this repository

On the GitHub website page from step 1 you’ll find the https address for the remote repository. We’re going to link our local repository to that remote repository.

Copy the address of the remote repository and add it as the remote origin for the local repository with the following command. (We’re using this Git Cheatsheet repository for this example.)

```
git remote add origin https://github.com/HHH-GH/git-cheatsheet.git
```

Type `git remote -v` to list the remotes for the local repository, if you’re interested. The `-v` flag means ‘be verbose with the info returned’.

![Screenshot of the git remote -v command and the information returned.](./assets/Git-Bash-remote-v_472x64.png)

### 5. Copy all the branch info to the main branch

```
git branch -M main
```

The `-M` flag forces the move of the local repository’s config and <abbr title="Reference Log">reflog</abbr>. The command above is taking the updated config info added since step 3a, and using it to overwrite any of the previous info for the main branch. (Or something like that—see the explanation of the `-f` and `-M` flags in [the docs for git-branch](https://git-scm.com/docs/git-branch).)

(I’m assuming this only needs to be done this one time.)

### 6. Push the code in the local repository to GitHub

Send the code in your local repository up to the remote repository on GitHub.

You’ll be asked for a password. Use the Personal access token from step 2.

```
git push -u origin main
```

The `-u` flag means ‘add upstream (tracking) references’, according to the [docs for git-push](https://git-scm.com/docs/git-push). It sets the ‘upstream branch for the given branch’, so you can use `git push` and `git pull` and `git fetch` without the extra arguments like `origin main`.

(I’m assuming this also only needs to be done one time per branch.)

![Screenshot of git push -u origin main command and its results](./assets/Git-Bash-push_472x136.png)

### 7. Take a look at the results

The files in the local repository will now also be in the remote repository on GitHub.

---

## References

The ‘Add existing code to a new GitHub repository’ part of this `README` is based on:

- GitHub’s [Adding locally hosted code to GitHub docs](https://docs.github.com/en/get-started/importing-your-projects-to-github/importing-source-code-to-github/adding-locally-hosted-code-to-github#adding-a-local-repository-to-github-using-git) (github.com)
- Karl Broman’s [Start a new git repository guide](https://kbroman.org/github_tutorial/pages/init.html) (kbroman.org)
