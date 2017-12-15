---
layout: post
title:  "Deploying-with-Capistrano-3"
date:   2017-06-06 12:34:07 +0000
categories: capistano deploy
---

gem install capistrano

mkdir ~/project
cd ~/project
cap install

edit

config/deploy.rb

Add stage info to file

For production - config/deploy/production.rb
For development - config/deploy/develop.rb


server 'hostname', user: 'username', roles: %w{web app}, port: 22

To deploy, must be in root of project

cd ~/project

cap develop deploy

Notes

- key based ssh logins need to be setup
- deploy key needs to be setup for git
