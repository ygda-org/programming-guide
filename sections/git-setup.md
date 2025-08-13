# Setting Up GitHub

In this section, we will be creating a GitHub repository and committing your work there to share with an officer! 
You're at the very last step of this tutorial, and this is how we can verify you have completed all your work.

---

### First things first: Why GitHub?
<p align="center" width="100%"><img src="../images/git-setup/github_logo.png" width=50%></p>
GitHub is a widely used cloud-based platform for version control and collaboration in software development. It allows you to easily track changes in your code and also makes collaboration between developers a lot easier! GitHub maintains a central repository, while you work off a copy that you "clone" to your local machine. When you want to share changes they have made locally, you can "commit" and "push" the changes back to this central repo.

### Installing Git & Creating a GitHub Account
Firstly, GitHub requires Git, a version-control system (VCS). Depending on what platform you use, you may follow the respective tutorial for Git installation [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

In the meantime, if you don't have a GitHub account already, you should sign up for one at [github.com](https://github.com/)!

### Creating a new repository
In GitHub, navigate to `Your Repositories`. You should see a green `New` button that will allow you to create a new repository. 
<p align="center" width="100%"><img src="../images/git-setup/new_repo.png" width=80%></p>
You can provide a name (i.e. "ygda-guide-game") and set its visibility. We will ask you to keep this repository public so officers are able to easily access your game.
<p align="center" width="100%"><img src="../images/git-setup/new_repo_creation.png" width=50%></p>

Congrats! You've just made a repository! Now, copy the link to your repository, since it will come in handy soon.

### GitHub Desktop vs. Command Line
You have two different methods you can go about working with GitHub now: GitHub Desktop or via a terminal. 
There honestly isn't a choice that is "better" than the other, so it just depends on your own preferences.

Navigate to the [GitHub CLI tutorial page](./git-cli-setup.md) if you'd prefer using it over GitHub Desktop.

### GitHub Desktop Installation:
You can navigate to [github.com/apps/desktop](https://github.com/apps/desktop) and follow the installation process. 
Sign in with your GitHub account, and you're all set!

### Cloning your repository
In GitHub Desktop, go to `File -> Clone Repository -> URL` and paste the link to the repository you just created. You can specify the local path you want it to point to as well!
<p align="center" width="100%"><img src="../images/git-setup/clone_repo.png" width=50%></p>
Now, in File Explorer, navigate to this repository's file path.

### Pushing your work!
Save all your work and close out of Godot. Move your Godot project files, including its parent folder, into this repository file. Note that afterwards, you will need to reimport this file in the Godot Project Manager.

In GitHub Desktop, you should now see these Godot files appear in the `Changes` tab.
Provide a summary of the work you've done (i.e. "Completed YGDA Programming Guide"), and commit these files to `main`.
<p align="center" width="100%"><img src="../images/git-setup/committing.png" width=50%></p>

However, you've only committed these changes locally! Click `Publish branch` to push your changes to the remote, or central, repository.
<p align="center" width="100%"><img src="../images/git-setup/publish_branch.png" width=80%></p>

If you check the `History` tab, you should be able to see your newly made commit message and changes made.

### Final steps
In the Godot Project Manager, you can reimport your project with the repository file path. This issue can be avoided in the future if you just create a repository project path prior to creating a new game in Godot.
<p align="center" width="100%"><img src="../images/git-setup/navigate_reimport.png" width=50%></p>

You're officially done with our programming guide! Share your GitHub repository link with an officer to show off your amazing work!

### Furthering your knowledge

talk about other git stuff like pulling, branches, etc etc etc in preparation for tj rpg