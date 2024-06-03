## Make ZFS Pool
- log into TrueNAS
- go to strage tab and create new pool
- or import existing pool

## Create Users and Groups for Permissions
- go to groups, create a new group
- give group full file permissions
- go to users page to add new users, i like to have one for me, and one for apps
- give users in group full file permissions


## Create shares
- go to the shares tab and add new share
- use general and mostly default settings
- edit ACL groups

## edit permissions 
- add user group as owner
- add granular permissions for each user

## Test shares on windows
- map drive on windows, test permisisons
