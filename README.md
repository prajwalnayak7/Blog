
# Personal Blog Site
 

### To run locally
- git clone git@github.com:prajwalnayak7/mysite.git
- hugo serve


## To build and deploy
hugo -t paper
./deploy_to_gh_pages.sh 

### Steps to create your own static website
- brew install hugo
- hugo new site mysite
- cd mysite
- git init
- // Add your favourite theme
- git submodule add https://github.com/nanxiaobei/hugo-paper themes/paper
- // Configure: theme="paper" in config.toml
- // Start the static server
hugo serve

WITH hugo:alpine 🤓
BUILD fast&cool
