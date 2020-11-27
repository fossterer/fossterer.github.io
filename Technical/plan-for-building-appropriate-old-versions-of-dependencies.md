# Plan for building appropriate (old) versions of dependencies

The following has been written with Maven as a build tool and Git as a version control system in mind. Any equivalents in your ecosystem should also let you accomplish this.

Often, in projects that are being actively developed, it happens that snapshots of certain dependencies are not retained in version control or any other artifact repositories.

Existing developers can continue to work on while a new entrant might have to leverage the power of version control systems to travel back into time.

Here's how to do it:

- Checkout the relevant projects from source control
- Go to the instance of codebase with the version indicated by pom.xml at the time of your usage
  - Switch to particular branch or a commit that shows required version
    - Use git blame pom.xml - Note the version shown
    - To go to the version before that, use `git checkout commit-ID-here-followed-by-~1`
      - ~number indicates no. of commits to go behind the identifier - can be commit ID/HEAD/branch-name :+1:
        - Repeat until you can build/install successfully

If you encounter another dependency that couldn't be found, keep traversing down the new tree until you reach a leaf node in this dependency graph

**Tip:** use `pushd` and `popd` while you go deep on these graphs so you can get back to where you started from

Install into your local repository via mvn install

Use `-DskipTests` argument so you don't get stopped by failing tests. Check with the others in the team if this is alright

Thank VCS! :clap: