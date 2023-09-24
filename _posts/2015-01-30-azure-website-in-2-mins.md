---
layout: post
title: Azure Website in 2 mins
date: 2015-01-30 11:52
author: peted70
comments: true
categories: [Azure, azure, cli, cross-platform, website]
---
<p>Love that you can create a hosted website from the command line with a few simple commands:</p><iframe height="315" src="https://www.youtube.com/embed/V1Zu1kJKWbY" frameborder="0" width="420" allowfullscreen></iframe> <blockquote> <p>azure site create --git fromCmdLine</p> <p>copy con index.html  <p>&lt;!DOCTYPE html&gt;  <p>Hello Azure Website!  <p>[CTRL-Z]  <p>[ENTER]  <p>git add -A  <p>git commit -m "initial add"  <p>git push azure master </p></blockquote> <p>Using the –git parameter with azure site create will create a Git repository on Azure for the named website. This will also initialize a Git repository in the directory from which the command was ran if one does not already exist. It will also create a Git remote named <strong>azure</strong>, which can be used to push deployments to the Azure Website using the <code>git push azure master</code> command. If you are using bash replace the ‘copy con’ with ‘cat’.</p> <p>There are a few pre-requisites:</p> <p>Azure account – sign up for a <a href="http://azure.microsoft.com/en-us/pricing/free-trial/" target="_blank">free trial</a> if you don’t have one</p> <p>Azure cross platform CLI tools with instructions including how to configure the current subscription here - <a title="http://azure.microsoft.com/en-gb/documentation/articles/xplat-cli/" href="http://azure.microsoft.com/en-gb/documentation/articles/xplat-cli/">http://azure.microsoft.com/en-gb/documentation/articles/xplat-cli/</a></p>
