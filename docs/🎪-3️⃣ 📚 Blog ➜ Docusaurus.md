---
sidebar_position: 130
title: 🎪-3️⃣📚 Blog ➜ Docusaurus
---

# Blog Build



🔵 WHY 

    blog is a proof of what you can.
    you will forget what you can -.-


🔵 Blog CHoose 

    ◎ gitbook              ➜  not bad 
    ◎ Docusaurus           ➜  like gitbook.

    use vscode             ➜ write    .md
    use Docusaurus         ➜ generate .html
    use github pages       ➜ host     .html blog


🔵 Docusaurus basic demo 

    npx create-docusaurus@latest xxxx classic
    npx create-docusaurus@latest Blog-NeverDEL classic
    cd Blog-NeverDEL
    npx docusaurus start



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Github page basic  

🔵 Github Page Desc 

    you upload html file,   nginx        help you display as website.
    you upload html file,   github pages help you display as website.
    github pages == nginx. 



🔵 Github Page Type 

    <organization>.github.io ➜ one organization     one name       ➜  one  github page 
    <user>.github.io         ➜ one account          one username   ➜  one  github page 
    <project>.github.io      ➜ one account          many project   ➜  many github page
    
        organization ➜ for company 
        user         ➜ for us 
        projectname  ➜ both can use 

            we usually use user type 




🔵 gitpage repo Desc  

    gitpage repo need two function
        ◎ save project file   (all file expect build folder)
        ◎ show project html   (all file inside build folder      after run npm build)

    you can use one repo for two function: one branch for file. one branch for html
    you can use two repo for two function: one repo   for file. one repo   for html
    here we choose one repo two function

           normal  repo only need one branch. ( project files branch )
        🔥 gitpage repo must need two branch. ( project files branch & project html branch )
        🔥 gitpage repo must need two branch. ( project files branch & project html branch )
        🔥 gitpage repo must need two branch. ( project files branch & project html branch )


    gitpages service not  help you run npm build.

    gitpages service only help show html under branch`s root folder.
        main     branch:  ➜ use main     branch`s root folder
        gh-pages branch:  ➜ use gh-pages branch`s root folder


    you must create a gh-pages branch for gitpages service
    and set branch in github repo`s setting.


    gitpages service not  help show html under repo`s subfolder
    
    we only need create gh-pages branch.
    and let gitpage repo use  gh-pages branch.
    as for what is side gh-pages branch (should be all file under build folder)
    docusaurus have a easy cmd for us.
    yarn deploy





🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 gitpage demo 


🔵 github prepair

    1. create username based github page repo 
    2. upload ssh key to github account.

    🔶 create repo 
        xxxxxxxxx.github.io
        mirandaxx.github.io

    🔶 ssh key 


🔵 local prepair

    npx create-docusaurus@latest xxxx          classic
    npx create-docusaurus@latest Blog-NeverDEL classic

    cd Blog-NeverDEL
    yarn install
    
    yarn start



🔵 push local folder 

    cd Blog-NeverDEL
    git init 

    git remote add origin git@github.com:MirandaXX/mirandaxx.github.io.git
       - here choose ssh type. no https type 

    git branch -M main
    git push -u origin main
        - check github if file uploaded



🔵 github branch setup

    1. create a branch:  gh-pages 
    2. let gitpage server use gh-pages branch.

    🔶 create  branch:  name must be gh-pages
    🔶 use new branch 

    github >> mirandaxx.github.io 
        >> setting >> pages 
            >> sources:  
                choose  gh-pages 
                and use /root 
                save 



🔵 edit docusaurus.config.js

    before use yarn deploy cmd, must config first 


    const config = {

    url: 'https://mirandaxx.github.io',     //  github page link url
    baseUrl: '/',                           //  no change 
    organizationName: 'mirandaxx',          //  your user name.
    projectName: 'mirandaxx.github.io',     //  your repo name.
    deploymentBranch: 'gh-pages',           //  no change.  must add this line.  



🔵 deploy 

    USE_SSH=true yarn deploy
        - just copy no edit

    visit test
    https://mirandaxx.github.io

    need wait few minutes. 





🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 Custom docusaurus

🔵 sidebar width

    (/src/css/custom.css) 

    :root {
        --doc-sidebar-width: 600px !important;


🔵 doc mode ✅

    i no need homepage. 
    only need doc function 

    https://docusaurus.io/docs/docs-introduction#docs-only-mode

    1. del/rename src/pages/index.js 

    2. add one line in docs 
        routeBasePath: '/', 


        docusaurus.config.js
            module.exports
                presets
                    docs


🔵 choose a markdown as homepage 

    any xx.md 

    add follow will be homepage. 

    ---
    slug: / 
    ---

