Chef Lesson: Managing node run-lists
===================================
This is the managing node run-list lesson in **Markdown**.

It will serve as a lesson of __Chef__ and a practice of _Markdown_
> I hope it goes well.

Adding a run list from the _knife_ command
------------------------------------------
In first place we will check with the server which nodes we have available. For this we will use the `knife node list` command. This will output a list of the available servers to manage.

Once we know the server for which we want to add a run list, we can use the following command to add it:

    $ knife node run_list add node.name 'recipe[cookbook]'
    
That will add the default recipe for the _cookbook_ to the node.

In order to know the run lists that a host may actually have assigned we have a couple of options, one may be to execute the command `knife node show node.name` which will show among other info that Chef has about the node, the run lists currently associated. If we want even more information, we can use the *-l* modifier on the last command that will give all the information that Chef has on the node, which could be a little bit overwhelming.

If we want to add more recipes to a run-list, we could re-run the last command specifying the recipe, that will add the recipe at the end of the run list:
    $ knife node run_list add node.name 'recipe[cookbook::recipeName]'
We can fine tune where we want to insert our recipe with the _-a_ and _-b_ options. The _-a_ option will append the recipe **after** the one that we specify, and the _-b_ **before**.

    $ knife node run_list add node.name 'recipe[cookbook::secondRecipeName]' -a 'recipe[cookbook::firstRecipeName]'
    $ knife node run_list add node.name 'recipe[cookbook::firstRecipeName]' -b 'recipe[cookbook::secondRecipeName]'

Finally, if we want to remove one or more recipes we can use the command in this way:

    $ knife mode run_list remove server.name 'recipe[cookbook::recipe1],recipe[cookbook::recipe2], ... recipe[cookbook::recipeN]'

When adding multiple recipes in one command, the order on which we specify them will be the order on which they will run. The order is indifferent when removing recipes.

Running the newly assigned run-list
----------------------------------
The next logical step will be to execute the run list. However, we have the risk of screwing something up by doing so. In order to prevent that, we can actually tell chef to execute the run-list in a _"why run"_ mode. In this mode Chef will go through all the run-list steps showing what would be done, but without performing any change. To use the _"why run"_ mode we pass the _--why-run_ or the _-W_ option to the _chef-client_ command on the client node:

    $ chef-client --why-run 
