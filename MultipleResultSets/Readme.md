## Overview
Helper class for entity framework to help deal with multiple result sets returned by stored procedures. Code heavily borrowed from Khalid (http://www.khalidabuhakmeh.com/entity-framework-6-multiple-result-sets-with-stored-procedures#disqus_thread), changes were made to use sqlcommand instead of passing a string, which if using input parameters on stored procedure offers the potential for sql injection.

## Code Example
Code is executed in the same manner i.e.:
    
    var results = contect.MultipleResults("[dbo].[Pets] + whatever parameters")
                    .With<Cat>()
                    .With<Dog>()
                    .Execute();

Except now we define a sqlcommand and pass it in:

    var command = new SqlCommand()
    {   
        CommandText = "[dbo].[Pets]",
        CommandType = CommandType.StoredProcedure
    };

    var results = contect.MultipleResults(command)
                    .With<Cat>()
                    .With<Dog>()
                    .Execute();

You can then use the parameters function to add whatever params you require.

## Motivation
While testing App was found that clients could inject SQL. While the role the service account was given didnt have insert/delete etc access and the model prevented data being returned, changes were made to increase security as to prevent anyone being able to execute anything (hopefully).