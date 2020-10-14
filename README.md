# Greek Mythology Family Tree
A Greek mythology family tree built entirely Prolog

Welcome to my project. The goal of this project is to implement all of the Greek gods,
demigods, titans, monsters, and anything else that pops up in Greek myths. This readme will teach you to do two things. First, add your own information. Whether this is to add anyone I have missed or to change information that I have included, this next section is what you'll need. If this is because the information is objectively wrong, as in you cannot find anywhere that the relationship I described is true, please let me know. I have on occasion had to fill in some gaps where mythology was lacking or if the parents described made no sense based on the people they interacted with.

Please note. I recommend only using this if you are somewhat familiar with Prolog. Prolog is VERY different from every language I have used, and is not that intuitive. I will try my best to describe how to add information and use the system, but a background in Prolog will definitely not hurt. In general, when editing the code, end every line with a period. This functions like a semicolon in languages like Java and C++. When asking the system questions (will be referred to as querying from here on out), you must also end it in a period. If not, and you enter in the query, nothing will happen, as its not a query without a period. Finally, variables start with a capital letter, and parameters start with lowercase.

To install, please see install.txt.
This can also be accessed in browser at https://swish.swi-prolog.org/p/Greek.pl

NOTE: SWISH version may not be up to date. The most recent version will always be from this repository. SWISH will be updated every so often with any major additions/changes.


First, how to add a new person/god/nymph/thing. You will need 1-3 facts (statements). Lets assume you are adding a new person Bob. First, you must either have male(Bob) or female(Bob). If they have no parents (either because they do not have any in mythology or you do not feel like adding them), you are done. If they have parents, you must define one or two parent_of propositions. If Bob's mom is Alice, and father is Jim, you must define parent_of(Jim,Bob) and parent_of(Alice,Bob). Note the order of these two clauses do not matter. Additionally, while I have defined some categories in the code, these are purely for organizational purposes and will in no way affect the program.

Keep in mind that in order to avoid warnings from Prolog, you must have all facts of the male proposition before any definitions of female, which must be before any propositions of parent_of. Warnings won't stop the program from working, but you will be yelled at by Prolog every time you compile.

There are many rules in this project that you can use in your queries. Some rules are internal and should not be called unless you really understand what you are doing.


The first set of rules are the relationship rules of the form relationship_of(X,Y). These rules mean that:  
X is the <relationship> of Y  
For example, parent_of(X,Y) means that X is the parent of Y.  
You can have either X or Y as a variable. If you have neither as a variable, it will test the truthfulness of the relationship. For example, parent_of(Alice, Bob). will return true based off of the propositions we have listed in the previous section. parent_of(Bob, Alice). will likewise return false. If you queried parent_of(Alice, X)., Prolog will return Bob, as he is the first (and only) instantiation that satisfies this relationship. Entering in a semicolon will cause Prolog to search for another instantiation of X that will satisfy the relationship. You can keep doing this for all true values, or you can use the rules in the next section.


The next set of rules are the user-friendly rules of the relationships. They return all elements that satisfy the relationship. These rules mean that:  
Y are all the <relationship> of X.  
For example bros(X,Y) mean that Y will be an array with all of the brothers of X. Y must be a variable. These rule can check true or false, but will only return true if the second argument is a list of all things that satisfy the relationship in alphabetical order, so it is not recommended. Using actual Greek mythology, bros(Zeus, X). will return [hades, poseidon].

The next rule is whois(X), which will show all the basic info of X. X must not be a variable.

The next rules are the complex rules and fall into two categories. Most of these rules are internal rules that are called by other rules and should not be called unless you really know what you are doing and you are debugging. The other rules I will discuss and explain.

cas(X,Y,S) :- Finds all of the common ancestors of X and Y. S must be a variable and is returned as an array of people.

mrca(X,Y,Z) :- Finds most recent common ancestor(s) of X and Y. mrca goes through all cas, forms a path from X to the ca to Y, and takes the length. The path(s) with the shortest length are the ones whose common ancestor relates X and Y the closest. These common ancestor(s) are returned as the most recent common ancestor(s). Z must be a variable and is returned as an array of people (potentially of length 1).
Note: In a normal tree this process would be overly complex, as you should just pick the node at the greatest depth. But due to the convoluted and tangled tree that is Greek mythology, this method was proven necessary through experimentation.

paths(X,Y) :- Generates the shortest path from X to a common ancestor to Y for all most recent common ancestors of X and Y. None of the arguments can be a variable. The method will display the mrca(s), and then for every element list the element used and the paths formed.

all_paths(X) :- Generates all paths from X to their ancestors. X cannot be a variable. Similar format to paths, except it will return all cas, and list the same path twice (as you are implicitly calling all_paths(X,X)).

all_paths(X,Y) :- Generates the shortest path from X to a common ancestor to Y for all the common ancestors of X and Y. None of the arguments can be a variable. Same format as all_paths, except you will have two unique paths as X and Y are different.

rel(X,Y) :- Describes the relationship between X and Y, using the most recent common ancestors as the common ancestor(s).  None of the arguments can be a variable. This will essentially call paths(X,Y), and after every set of paths, it will list the relationship underneath.

all_rel(X,Y) :- Describes all of the relationships between X and Y, using all of the common ancestors. This has the same format as rel(X,Y), except its essentially calling all_paths(X,Y), so you have the relationship for every path. None of the arguments can be a variable.
Note: Some paths will pass through other common ancestors, and if these extra-long paths were used for the relationship the incorrect relationship would output. This is fixed internally however, so all relationships displayed are correct.

gen(X,N,Y) :- Lists all people N generations after X. For example, a N of 1 returns
all children, N of 2 returns all grandchildren, etc. N of 0 returns X, and N less
than 0 or not a number returns false. X must be a being, and N must be a number. Y must
be a variable.

num(X) :- Simply displays the number of beings in the system. X must be a variable.

random(X) :- Generates a random being. X must be a variable.

random_rel() :- Generates two random beings, and runs rel on those two beings. No arguments.

random_all_rel() :- Generates two random beings, and runs all_rel on those two beings. No arguments.
