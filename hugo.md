**Hugo: Deploy Static Site using GitHub Actions**
<P>
If you are using Hugo to generate static pages, you are familiar with CLI commands which are to build the static pages in your local machine and make push to your <username>.github.io repository.

</P>

- Step 1: **Setting up repo named <username>.github.io in GitHub**
> You need to create a repository named <your GitHub username>.github.io. Contents on that page will be accessible via url same name as the repo.

- Step 2: **Install and create a Hugo project**
> You need to install Hugo in your local machine and use that to create a site. Please take a look at the [official documentation](https://gohugo.io/getting-started/quick-start/) on how to do that.
- Step 3: **Push Hugo site code in GitHub**
  <P>
    Now create a new GitHub repo for that Hugo site, and push to master branch. Just follow this steps:
  </P>
  
 ```
 git init
 ```
 ```
git remote add origin git@github.com:<username>/<repo-name>
```
  ```
git add --all
 ```
  ```
git commit -m "initial commit"
```
  ```
git push origin master
  
 ```
  
 - Step 4: **Create GitHub token**
  
  > Now you need to generate a token with repo access from the [GitHub’s tokens page](https://github.com/settings/tokens/new). You will get a 40 character long token by generating the token. Store it somewhere securely.
  <p align="center">
  <img src="https://ruddra.com/content/images/2020/03/github_token_hu7fba9da1679ce4a56c592454604cb9c1_173034_720x0_resize_q100_box.jpg" width=800px;>
  </p>
