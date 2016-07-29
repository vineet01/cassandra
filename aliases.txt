Following are the features which i have added with sligh modifications and additions in original code :-

1. Bash-like Aliases with full autocompletion support, custom aliases, listing aliases.

cassandra@cqlsh> show 
         ;        <enter>  ALIASES  HOST     SESSION  VERSION  
cassandra@cqlsh> show ALIASES ;
Aliases :> {'dk': 'desc keyspaces;', 'sl': 'select * from'}

now if you type dk and press <tab> it will auto complete it to "desc keyspace".

Adding alias

From shell :-

cassandra@cqlsh> alias slu=select * from login.user ;
Alias added successfully - sle:select * login.user ;
cassandra@cqlsh> show ALIASES ;
Aliases :> {'slu': 'select * from login.user ;', 'dk': 'desc keyspaces;', 'sl': 'select * from'}
cassandra@cqlsh> sle
Expanded alias to> select * from login.user ;
 username                 | blacklisted | lastlogin | password   


Directly in file :-

aliases will be kept in same cqlshrc file.
[aliases]
dk = desc keyspaces;
sl = select * from
sle = select * from login.user ;

now if you type just "sle" it will autocomplete rest of it and show next options.

2. Disable destructive operations. It's inspired from elasticsearch - https://github.com/elastic/elasticsearch/issues/4549 . I think we should have it in production atleast.

in cqlshrc :-

[misc]
safe_mode = true

if this mode is true, we can't do 'delete from table' or 'truncate table'. Delete can be done with a where clause and a prompt will be given. Same applies to update , you can't update all columns in one query without where clause.

And on EACH delete command it will prompt if we are sure.
It also applies when statements are imported from a source cql file.

cassandra@cqlsh> delete from login.user ;
Are you sure you want to do it? Disable "safe_mode" flag in cqlshrc to perform it but make sure you know what you are doing.

cassandra@cqlsh> truncate login.user ;;
Are you sure you want to do it? Disable "safe_mode" flag in cqlshrc to perform it but make sure you know what you are doing.

cassandra@cqlsh> delete from login.user where username = 'v';
Are you sure you want to do this? (y/n) > n
Cancelled deletion.

cassandra@cqlsh> updatee login.user set password=null;
Are you sure you want to do it? Disable "safe_mode" flag in cqlshrc to perform it but make sure you know what you are doing.

3. Press q to exit.
