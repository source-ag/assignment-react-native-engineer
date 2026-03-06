The [repository](https://github.com/source-ag/assignment-react-native-engineer) for the assignment 
is public and Github does not allow the creation of private forks for public repositories.

The correct way of creating a private fork by duplicating the repo is documented
[here](https://help.github.com/articles/duplicating-a-repository/).

For this assignment the commands are:

 1. Create a bare clone of the repository.
    (This is temporary and will be removed so just do it wherever.)
    ```bash
    git clone --bare git@github.com:source-ag/assignment-react-native-engineer.git
    ```

 2. [Create a new private repository on Github](https://help.github.com/articles/creating-a-new-repository/) and give 
    it a good name, for example `source-assignment-react-native-engineer`.

 3. Mirror-push your bare clone to your new `source-assignment-react-native-engineer` repository.
    > Replace `<your_username>` with your actual Github username in the url below.
    
    ```bash
    cd assignment-react-native-engineer
    git push --mirror git@github.com:<your_username>/source-assignment-react-native-engineer.git
    ```

 4. Remove the temporary local repository you created in step 1.
    ```bash
    cd ..
    rm -rf assignment-react-native-engineer
    ```
    
 5. You can now clone your `source-assignment-react-native-engineer` repository on your machine (in my case in the `code` folder).
    ```bash
    git clone git@github.com:<your_username>/source-assignment-react-native-engineer.git
    ```

 6. Add the reviewers mentioned in the invitation email for the tech assignment as collaborators to your new repository

However, if you want to go with GitHub WebUI only, you can perform the following steps:

 1. [Import this repository into new one](https://docs.github.com/en/get-started/importing-your-projects-to-github/importing-source-code-to-github/importing-a-repository-with-github-importer). As an old repository use the url to this repository; give it a good name, for example `source-assignment-react-native-engineer`; choose "Private" under "Privacy".

 2. You can now clone your `source-assignment-react-native-engineer` repository on your machine.
    ```bash
    git clone git@github.com:<your_username>/source-assignment-react-native-engineer.git
    ```
 3. Add the reviewers mentioned in the invitation email for the tech assignment as collaborators to your new repository
