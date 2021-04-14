
# Personal Blog Site
Live at: https://prajwalnayak7.github.io/
Built using RAD SDLC Model


### Useful Commands
```
To run locally :

- git clone git@github.com:prajwalnayak7/mysite.git
- hugo server --verbose  --disableFastRender
- Replace the theme html files with the ones in archive


To build and deploy : 
./publish_to_ghpages.sh 


To Add a new article :
hugo new abc.md


Solutions for common issues:
- hugo mod clean
- git submodule init ; git submodule update


Steps to create your own static website :
- brew install hugo
- hugo new site mysite
- cd mysite
- git init
- // Add your favourite theme
- git submodule add https://github.com/nanxiaobei/hugo-paper themes/paper
- // Configure: theme="paper" in config.toml
- // Start the static server: hugo serve
```

WITH hugo:alpine 🤓

BUILD fast&cool
