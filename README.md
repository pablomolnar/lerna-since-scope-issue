# lerna-since-scope-issue

Lerna scope does not work always `@my-company/*` vs `@my-company/app-*`

## Lerna

 ```sh

# clone repo
$ cd /tmp
$ git clone https://github.com/pablomolnar/lerna-since-scope-issue && cd lerna-since-scope-issue

# pkg-test is a dependecy of app-test
$ tree .
.
├── README.md
├── lerna.json
├── package-lock.json
├── package.json
└── packages
    ├── app-test
    │   ├── index.js
    │   └── package.json
    └── pkg-test
        ├── index.js
        └── package.json

3 directories, 8 files

# create a branch "my-new-feature"
$ git checkout -b my-new-feature
Switched to a new branch 'my-new-feature'
 
# do a change in the dependecy
$ echo "console.log('this is a change')" >> packages/pkg-test/index.js

# lerna updated detect packages with changes since master (OK)
$ my-new-feature$ lerna updated --since master
lerna info version 2.11.0
lerna info Checking for updated packages...
lerna info Comparing with initial commit.
lerna info Checking for prereleased packages...
lerna info result
- @my-company/app-test
- @my-company/pkg-test

# lerna exec with scope "@my-company/*" runs in packages with changes since master (OK)
$ lerna exec --since master --scope @my-company/*  -- pwd
lerna info version 2.11.0
lerna info scope @my-company/*
lerna info Checking for updated packages...
lerna info Comparing with master.
lerna info Checking for prereleased packages...
/tmp/lerna-since-scope-issue/packages/pkg-test
/tmp/lerna-since-scope-issue/packages/app-test


# lerna exec FAILS to detec changes since master with scope "@my-company/app-*"  (OK)
$ lerna exec --since master --scope @my-company/app-*  -- pwd
lerna info version 2.11.0
lerna info scope @my-company/app-*
lerna info Checking for updated packages...
lerna info Comparing with master.
lerna info Checking for prereleased packages...
(Empty)
 ```
