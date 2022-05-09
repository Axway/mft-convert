{
    "title": "Define additional environment variables ",
    "linkTitle": "Define start environment variables",
    "weight": "300"
}****Unix****

When loading the Transfer CFT profile, files that are stored in the profile.d directory are also executed, and all of the defined environment variables are then available in the current environment. This enables you to use these variables in the Transfer CFT configuration or processing scripts.

Procedure
---------

1. Create a new file in the `$CFTDIRRUNTIME/profile.d` directory, and add your customized variables as follows. For example:  
    _EXPORT MYVARIABLE01 TheVariableValue01  
    _EXPORT MYVARIABLE02 TheVariableValue02
1. Execute the profile command.
