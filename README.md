## Lambda Expressions & Functional Interfaces IceCreamParlorService - Lambda-flavored

### Introduction

We're returning to the `IceCreamParlorService` from a prior lesson. It's evolved
a bit since then, but some of the key elements are still the same.

**Key models:**
- `Carton`: A carton of ice cream of a particular flavor that is available in the
  parlor. If the carton `isEmpty()`, however, there is none of that flavor left!
- `Flavor`: A particular flavor of ice cream. Each flavor has a unique combination
  of `Ingredient`s, which are specified by a `Recipe`.
- `Ingredient`: An ingredient needed to make ice cream. Some ingredients are
  shared across flavors, some are unique to a flavor.
- `Recipe`: For a given flavor, the recipe indicates the ingredients to add to the
  mixture (as well as the order in which they should be added, in the form of a
  `Queue`).
- `Sundae`: What our customers order! They contain a list of scoops of `Flavor`
  objects representing each scoop in the sundae.

![Ice Cream Parlor Service Models class diagram. Carton: A carton of ice cream of a particular flavor that is available in the parlor. If the carton isEmpty(), however, there is none of that flavor left! Flavor: A particular flavor of ice cream. Each flavor has a unique combination of Ingredients, which are specified by a Recipe. Ingredient: An ingredient needed to make ice cream. Some ingredients are shared across flavors, some are unique to a flavor. Recipe: For a given flavor, the recipe indicates the ingredients to add to the mixture (as well as the order in which they should be added, in the form of a Queue). Sundae: What our customers order! They contain a list of scoops of Flavor objects representing each scoop in the sundae.](src/com/ata/lambdaexpressions/classroom/resources/IceCreamParlorModels_CD.png)

[Ice Cream Parlor Service Models diagram PlantUML source](src/com/ata/lambdaexpressions/classroom/resources/IceCreamParlorModels_CD.puml)

Customers can order a sundae by calling `getSundae()` with a list of flavors
they want in the sundae.

There is also a method on our service, `prepareFlavors()` for preparing more
ice cream by passing in a list of flavors to create. These are then looked
up in the recipe store, which gives us the ingredients to mix into the ice cream.

The `IceCreamParlorService` depends on:
- two DAOs, `CartonDao` and `RecipeDao`, for
  accessing `Carton`s and `Recipe`s, respectively.
- `IceCreamMaker`, which accepts ingredients and then does the mixing and
  freezing.

![Ice Cream Parlor Service class diagram, showing the service class's dependencies. The IceCreamParlorService depends on two DAOs, CartonDao and RecipeDao, for accessing Cartons and Recipes, respectively. It also depends on IceCreamMaker, which accepts ingredients and then does the mixing and freezing.](src/com/ata/lambdaexpressions/classroom/resources/IceCreamParlorService_CD.png)

[Ice Cream Parlor Service class diagram PlantUML source](src/com/ata/lambdaexpressions/classroom/resources/IceCreamParlorService_CD.puml)

There are several places in the code that need functional interfaces to be
created and passed into methods. Your job is to provide those functional
interface implementations, sometimes using lambda expressions, sometimes
with method references.

### Phase 0: It builds

1. Build your project. You may need to null out instantiations to your Dagger component to complete the initial build. 
1. Run the `Phase0Test` and verify that it passes

GOAL: Make sure that your project builds and the starter test passes.

Phase 0 is complete when:
- The `Phase0Test` runs and passes

### Phase 1: No empty scoops

You're going to make sure we don't deliver any "empty scoops" of ice cream in our
customers' sundaes by updating the `IceCreamParlorService.getSundae()` method.

You'll have a list of cartons of ice cream, and need to remove any cartons that
are empty (`isEmpty()` returns true). The functional interface that
you will be implementing with a lambda expression (and later a method reference)
accepts a `Carton` and returns `true` if the carton is empty, `false` if not.

1. Answer: **Which functional interface does this represent?**
1. When your group agrees on the answer to this question, find the appropriate
   place in `IceCreamParlorService.getSundae()` for this phase.
1. Add a call to `removeIf()` on the `cartons` parameter,
   passing in a functional interface implementation **using a lambda expression**
   (not a method reference).
1. Test that you've done it correctly by running the `lambdaexpressions-classroom-phase1`
   RDE workflow.
1. Compare your implementation with another member of your group.
1. Now change your implementation to use a method reference.
1. Test that this implementation works as well by running `Phase1Test`.

GOAL: use a method reference to remove empty cartons so we only add non-empty flavors
to the sundae.

Phase 1 is complete when:
- `Phase1Test` is passing

### Phase 2: Build that sundae!

Now you get to build the sundae of your customers' dreams by updating the
`IceCreamParlorService.buildSundae()` method. You'll create a lambda
expression that implements a functional interface. The functional
interface will accept each carton of ice cream and adds a scoop to the
`Sundae`.

The functional interface you will be implementing with a lambda expression
accepts a `Carton` and doesn't need to return anything.

1. Answer: **Which functional interface does this represent?**
1. When your group agrees on the answer to this question, find the appropriate
   place in `IceCreamParlorService.buildSundae()` for this phase.
1. Add a call to `forEach()` on the `cartons` argument, passing in a
   lambda expression implementing the appropriate functional interface.
1. Test that you've done it correctly by running `Phase1Test` and `Phase2Test`.
1. Compare your implementation with another member of your group.

GOAL: Use a lambda expression implementing a functional interface that adds
a scoop from each carton to the customer's sundae.

Phase 2 is complete when:
- `Phase1Test` and `Phase2Test` are passing

### Phase 3: Get the recipe

You're going to update `IceCreamParlor`'s `prepareFlavors()` method to convert a
list of `Recipe`s into a list of `Queue<Ingredient>`.

You'll create a lambda expression to do this. It will accept a `Recipe` and need to
return a `Queue<Ingredient>`.

We request that you use `RecipeConverter`'s static method to do the conversion.

1. Answer: **Which functional interface do you think it will require?**
1. When your group agrees on the answer to this question, find the appropriate place
   in `prepareFlavors()` to convert recipes to their ingredients. Implement the needed
   logic **using a lambda expression** (not method reference).
   Hint: You can take a look at the use of `map()` at the top of `prepareFlavors()`
1. Test that you've done it correctly by running the `lambdaexpressions-classroom-phase3`
   RDE workflow.
1. Compare your implementation with another member of your group.
1. Now change your implementation to use a method reference to the static method,
   `fromRecipeToIngredientQueue`
1. Test that this implementation works as well by running `Phase1Test`, `Phase2Test`, and `Phase3Test`.

GOAL: use a method reference to convert recipes to their `Queue<Ingredient>`
components

Phase 3 is complete when:
- `Phase1Test`, `Phase2Test`, and `Phase3Test` are passing.

### Phase 4: Make that ice cream

You're going to update `IceCreamService`'s `makeIceCreamCartons()` method to pass
a functional interface to an `IceCreamMaker` method it calls. The functional
interface will supply a new ingredient each time it's called by `IceCreamMaker`'s
method.

The functional interface you will implement with a lambda expression
returns a new `Ingredient` each time it is called (with no arguments).

1. Answer: **Before looking at the code, which functional interface does this
   represent?**
1. When your group agrees on the answer to this question, find the appropriate place
   in `IceCreamMaker.prepareIceCreamCarton()` to use the functional interface
   passed in as an argument.
1. Implement the change to use the functional interface to get each ingredient to
   add to the `Mixer`.
1. Verify this is working properly by running the `Phase4Test` method called
   `iceCreamMaker_prepareIceCreamCarton_callsFunctionalInterface`
   directly in IntelliJ.
   (Note: the other tests in the test class probably will still fail at this point).
1. Now return to `IceCreamParlorService.makeIceCreamCartons()` and provide the
   appropriate functional interface as a lambda expression to each call to
   `prepareIceCreamCarton()`.
   Hint: Feel free to mutate your `Queue` in the lambda expression!
1. Verify you've got it working by running the `lambdaexpressions-classroom-phase4`
   RDE workflow.
1. Compare your implementation with another member of your group.

GOAL: Implement logic that accepts a lambda expression and call that method with
a lambda expression, so that the proper ingredients are mixed in for each flavor.

Phase 4 is complete when:
- All tests are passing

### Extension

You've created a lambda expression for each of the four functional interfaces
we've studied. Time to write one on your own.

When a customer specifies a sundae with an empty flavor, the ice cream parlor
skips that scoop. Write a method that creates cartons for any missing flavors.
Call your method before creating the sundae.

**CAUTION**: This will cause tests for earlier phases to fail, because they
expect the sundae to omit scoops from empty cartons.
