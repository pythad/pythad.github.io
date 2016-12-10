---
layout: post
title: Editing remote files using SublimeText over SSH
categories: [Tutorial, SSH, SublimeText]
modified: 2016-12-11
comments: true
---

It happens that we need to edit a file on a remote server, but using nano/vim is not very practical, due to lags, impossibility of using cursor and speed of screen refreshes.

<!--more-->

# 1. Install the rsub plugin for ST2/ST3

Open SublimeText and install `rsub`  plugin: `Ctrl+Shift+P` > `Package Control: Install Package` > `rsub`

# 2. Add remote forwarding

Open you SSH config and add a remote forwarding line under the right host

`subl ~/.ssh/config`

    Host yourserver yourserver.com
    ...
    RemoteForward 52698 127.0.0.1:52698

# 3. SSH into you remote

    ssh yourserver

# 4. Install rmate and make it executable

    sudo wget -O /usr/local/bin/subl \
    https://raw.github.com/aurora/rmate/master/rmate
    sudo chmod a+x /usr/local/bin/subl

# 5. Make subl system wide(Optional)

If you will user `subl` from different users on the remote machine you can move it to `~/bin` with `sudo mv /usr/local/bin/subl ~bin/`

# 6. Test it

Now everything has to work just okay. Use `subl <filename>` on your remote machine to open the file in Sublime. **Don't forget that SublimeText has to be opened**.

    subl .profile

