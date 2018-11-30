# Multi-Developer Setup on MacOS

A single development machine may need multiple user accounts. Due to solutions such as
Homebrew, this requires some additional configuration beyond just adding a new user account
on MacOS.

## Configure Homebrew

After creating and logging into the new user account with admin permissions complete the following:

    # check if non-root user is in the admin group
    echo $(groups $(whoami))

    # if not, add to admin group
    sudo dseditgroup -o edit -a $(whoami) -t user admin

    # verify that brew root is `/usr/local`
    echo $(brew --prefix)

    # change group of `/usr/local` subdirs to `admin`
    # (in High Sierra, changing the group of `/usr/local` itself is not permitted)
    `chgrp -R admin /usr/local/*`

    # make the contents of `/usr/local` writable by group
    `chmod -R g+w /usr/local/*`

    # verify
    `brew doctor`

    # Remove ZSH warnings due to unsafe extra permissions
    `chmod -R g-w /usr/local/share/zsh`
    `chmod -R o-w /usr/local/share/zsh`

    # verify other account
    `login other_user`
    `brew doctor`


Adapted from: https://stackoverflow.com/a/44481141
