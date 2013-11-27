---
layout: post
title: "重新编译nginx"
category: linux
tags: []
---
{% include JB/setup %}

许多时候我们需要增加新的模块到nginx，这时需要重新编译nginx。

---

>Get package sources
    
    sudo apt-get build-dep nginx         # Install the dependencies
    cd /tmp                              # let's build here: /tmp
    sudo apt-get source nginx 

>Add some additional modules

    cd /tmp
    git clone https://github.com/wandenberg/nginx-push-stream-module.git

    cd /tmp/nginx-1.2.1/debian/modules     # Go to Nginx "modules" dir
    ln -s /tmp/nginx-push-stream-module    # and add a link to your module dir

Now add this module into Nginx configuration - edit "/tmp/nginx-1.2.1/debian/rules"
Add a line to the "Full" configuration:

    ...
    --add-module=$(MODULESDIR)/nginx-push-stream-module \
    ...

>Compile the package

    cd /tmp/nginx-1.2.1 && dpkg-buildpackage -uc -b -j2

    # -uc  - do not sign the .changes file
    # -b   - binary-only build, no sources
    # -j4  - number of parallel jobs (just in case)

Note that .deb file appeared in a parent dir.
    dpkg-deb -I ../nginx-full_1.2.1-2.2ubuntu0.1_amd64.deb      # Information about the package

Install recompiled .deb file:
    dpkg -i /tmp/nginx-common_1.2.1-2.2ubuntu0.1_all.deb
    dpkg -i /tmp/nginx-full_1.2.1-2.2ubuntu0.1_amd64.deb

>System update may reinstall Nginx and remove your modifications

Аfter you upgrade to a new version nginx, this recompiled package will be overwritten.
To prevent this use:
    sudo aptitude hold nginx
    sudo aptitude hold nginx-full