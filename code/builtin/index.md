---
title: BuiltIn
pageindex: 10
active: 1
--- 

# BuiltIn
The framework allows to "build in" database records into C# source code under certain circumstances. These C# "BuiltIn" records can be used in the deployment process to populate the target database with new data records. Or the "BuiltIn" records can be referenced at compile time. Typical examples are:

## Language
The running application can be translated on the fly into new languages. The data is stored on the database. Later the new language should become part of the deployment process. Now the data needs to be copied into the C# "BuiltIn" zone.

## User, Role and Privilege
The application allows to add new users, roles and privileges to database tables. Later the user "Admin" might get referenced at compile time to evaluate certain access control. The same might be true of privileges.