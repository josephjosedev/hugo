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
  
  - Step 5: **Add token as secret in GitHub**
  > The token last step, you can store it in the Secrets setting of the repo. It can be accessible from https://github.com/<username>/<repo-name>/settings/secrets. Store it like this:
  <p align="center">
  <img src="https://ruddra.com/content/images/2020/03/github_secret_hu7fba9da1679ce4a56c592454604cb9c1_153648_720x0_resize_q100_box.jpg" width=800px;>
  </p>
  
 - Step 6: **Create A GitHub Action**
  > Now it is time to do the fun stuff. Let us create an action in .github/workflows/ folder inside the repo(hugo site repo) and name it main.yml.
  
  
  ```
 name: CI
on: push
jobs:
  deploy:
    runs-on: ubuntu-18.04
    steps:
      - name: Git checkout
        uses: actions/checkout

      - name: Update theme
        # (Optional)If you have the theme added as submodule, you can pull it and use the most updated version
        run: git submodule update --init --recursive

      - name: Setup hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: "0.64.0"

      - name: Build
        # remove --minify tag if you do not need it
        # docs: https://gohugo.io/hugo-pipes/minification/
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          personal_token: ${{ secrets.TOKEN }}
          external_repository: <username>/<username>.github.io
          publish_dir: ./public
          #   keep_files: true
          user_name: <username>
          user_email: <username@email.com>
          publish_branch: master
        #   cname: example.com
  ```
> As mentioned from main.yml file, it is named CI and this is going to be triggered when something is pushed to the repo. It will be using an ubuntu-18.04 based VPS to run the pipeline. Now let us go through steps to understand how it works:
<p>
In the Git checkout step, we are going to fetch the latest code of our repository which contains Hugo site.

In the Setup hugo step, we are going to use peaceiris/actions-hugo to install Hugo. You need to specify which hugo version you want to use. I would recommend using hugo version of your local machine(command: hugo version).

In the Build step, we are going to build the static contents using hugo --minify command. By using --minify, we are going to minify the assets in the site. For more information, checkout the hugo documentation.

Finally, the Deploy step. Now we are going to deploy the static contents from the last step. And we are going to use peaceiris/actions-gh-pages actions to run the deployment. Here, we used external_repository: <username>/<username>.github.io because otherwise the static contents would be pushed in the same repo(in a different branch). As we specified the external repository, the static contents will be pushed to <username>.github.io. For this step, we will use the personal token which we specified in Step 5. If you uncomment keep_files: true, then the deployment will keep old files from <username>.github.io, otherwise it will replace everything. Finally, if you have a custom domain, then configuring cname is necessary. For more information, please check documentation in GitHub marketplace.</P>
  - Step 7: **Push to GitHub**
  >Now push to your Hugo site repository and voila, your action will start automatically. You can check its progress in the **actions** tab.
   <p align="center">
  <img src="https://ruddra.com/content/images/2020/03/github_actions_hub1dc45346a8c2fa2bc41a08ab45dcb5f_26415_720x0_resize_q100_box.jpg" width=800px;>
  
  </p>
  
  > After each successful run, it will push the static contents to your static page repository.
