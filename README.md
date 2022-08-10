# travis-lein-s390x
Reproduce not able to install lein at a specified version on s390x

# Email Trail

```
Hi, I’m trying to port a clojure project to s390x and I’m running into a bug when travis is updating lein to a newer version. I create a github repo and travis build to show the error:

https://github.com/jonathan-albrecht-ibm/travis-lein-s390x
https://app.travis-ci.com/github/jonathan-albrecht-ibm/travis-lein-s390x/builds/254202191

The error looks like:

---
Updating leiningen to 2.9.1
$ sudo env LEIN_ROOT=true curl -L -o /usr/local/bin/lein https://raw.githubusercontent.com/technomancy/leiningen/2.9.1/bin/lein
######################################################################## 100.0%
$ lein self-install
/home/travis/.travis/functions: line 109: /usr/local/bin/lein: Permission denied
The command "lein self-install" failed and exited with 126 during .

I think its just a executable permissions problem and maybe something like the following would fix it:

---
diff --git a/lib/travis/build/script/clojure.rb b/lib/travis/build/script/clojure.rb
index df8e1d8d..b1f5c081 100644
--- a/lib/travis/build/script/clojure.rb
+++ b/lib/travis/build/script/clojure.rb
@@ -54,6 +54,7 @@ module Travis
                 sh.echo "Updating leiningen to #{version}", ansi: :yellow
                 sh.cmd "env LEIN_ROOT=true curl -L -o /usr/local/bin/lein https://raw.githubusercontent.com/technomancy/leiningen/#{version}/bin/lein", echo: true, assert: true, sudo: true
                 sh.cmd "rm -rf ${TRAVIS_HOME}/.lein", echo: false
+                sh.chmod "+x", "/usr/local/bin/lein", sudo: true
                 sh.cmd "lein self-install", echo: true, assert: true
               end
             end
--- 

Would be great if this could be fixed as I’m currently blocked on it.

Thanks,

Jonathan Albrecht
Advisory Software Developer
Linux on IBM Z Open Source Ecosystem
1 905 413 3577 Office
jonathan.albrecht@ibm.com

IBM

```

```
##- Please type your reply above this line -##
Hello, 
Thank you for reaching out to Travis-CI Support. This is an automated message to notify you that we have received your ticket (40106) and one of our support agent will get back to you to assist with this matter. In case you are seeing a bug and haven't shared this information already, please reply to this email by sharing the steps to reproduce along with expected and actual results and your system (environment) information to supplement the troubleshoot process. 
Thank you for your patience while one of the support agent gets in touch with you!
Cheers,
Your Friends @Travis-CI
This email is a service from Travis CI. Delivered by Zendesk 
```
