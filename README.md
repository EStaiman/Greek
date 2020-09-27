# Greek Mythology Family Tree
A Greek mythology family tree built in Prolog

To add a new person/god/nymph, you need 1-3 facts. If you are adding a person Bob, you must either have male(BoB) or female(Bob). If they have no parents (either they don't have it in mythology or you do not feel like adding that person), you are done, as the person is added. If they have parents, you must define one or two parent_of propositions. If Bob's mom is Alice, and father is Jim, you must define parent_of(Jim,Bob) and parent_of(Alice,Bob). Note the order of these two clauses do not matter.

Keep in mind that in order to avoid warnings from Prolog, you must have all facts of the male proposition before any definitions of female, which must be before any propositions of parent_of.

There are many rules in this project that you can use in your queries. Some rules are internal and should not be called unless you really understand what you are doing.

The first set of rules are the relationship rules of the form relationship_of(X,Y). These rules mean that: 
X is the <relationship> of Y
For example, parent_of(X,Y) means that X is the parent of Y
You can have either X or Y as a variable. If you have neither as a variable, it will test the truthfulnes of the relationship.

The next set of rules are the user friendly rules of the relationships. They return all elements that satisfy the relationship. These rules mean that:
Y are all the <relationship> of X
For example bros(X,Y) mean that Y will be an array with all of the brothers of X. Y must be a variable.

The next rule is whois(X), which will show all the basic info of X.

The next rules are the complex rules, and fall into two categories. Most of these rules are internal rules that are called by other rules and should not be called unless you are debugging. The other rules I will discuss and explain.

cas(X,Y,S) :- Finds all of the common ancestors of X and Y. S must be a variable

mrca(X,Y,Z) :- Finds most recent common ancestor(s) of X and Y. mrca goes through all cas, forms a path fron X to the ca to Y, and takes the length. The path(s) with the shortest length is the mrca(s). Z must be a variable

paths(X,Y) :- Generates the shortest path from X to a common ancestor to Y for all most recent common ancestors of X and Y. None of the arguments can be a variable.

all_paths(X) :- Generates all paths from X to their ancestors. X can not be a variable.

all_paths(X,Y) :- Generates the shortest path from X to a common ancestor to Y for all the common ancestors of X and Y. None of the arguments can be a variable.

rel(X,Y) :- Desribes the relationship between X and Y, using the most recent common ancestors as the common ancestor(s). Essentially calls paths(X,Y) as well. None of the arguments can be a variable.

all_rel(X,Y) :- Desribes all of the relationships between X and Y, using all of the common ancestors. Essentially calls all_paths(X,Y) as well. Some paths will pass through other common ancestors, and if these extra-long paths were used for the relationship the incorrect relationship would output. This is fixed internally however, so all relationships displayd are correct. None of the arguments can be a variable.

num(X) :- Simply displays the number of gods/monsters/nymphs/humans in the system. X must be a variable.