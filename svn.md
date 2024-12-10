# SVN cheatsheet

## Mirror
### Initial creation

    cd /path/to/my/local/SvnMirror

    dir=myproject1
    url=https://my_favorite_svn.fr

    svnadmin create --fs-type fsfs $PWD/$dir
    cp -v _hooks/* $dir/hooks

    # copy the uuid
    svnadmin setuuid $dir $(svn info --no-newline --show-item=repos-uuid $url/$dir)

    svnsync initialize --sync-username syncuser file://$PWD/$dir $url/$dir
    svnsync synchronize file://$PWD/$dir

    # dir=myproject2, and repeat

### crontab update/sync

    #!/bin/bash -e
    # do_sync.sh: add this to your crontab to run every 10 min:
    # */10 * * * *      /path/to/my/local/SvnMirror/do_sync.sh

    cd /path/to/my/local/SvnMirror

    for dir in myproject1 myproject2; do
        echo `date` > $dir.out
        svn info file://$PWD/$dir >> $dir.out 2>&1
        svnsync --non-interactive --source-username=svnsync --sync-username=syncuser sync file://$PWD/$dir >> $dir.out 2>&1
    done



