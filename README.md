# Personal Blog Site - WITH hugo:alpine 🤓

### To run locally
git clone
hugo serve

### Steps to create your own static website
brew install hugo
hugo new site mysite
cd mysite
git init
// Add your favourite theme
git submodule add https://github.com/nanxiaobei/hugo-paper themes/paper
// Configure: theme="paper" in config.toml
// Start the static server
hugo serve
