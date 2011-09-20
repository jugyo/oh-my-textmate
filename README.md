oh-my-textmate
====

Install
----

    cd ~/Library/Application\ Support
    git clone http://github.com/jugyo/oh-my-textmate.git TextMate
    cd TextMate
    git submodule update --init
    osascript -e 'tell app "TextMate" to reload bundles'
