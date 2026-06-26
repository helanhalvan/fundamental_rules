# About this document

Here follows my fundamental rules. I would recommend putting it towards the end of the rulebook, and have earlier sections refer to this section. Missing pieces that needs to be filled in for a particular game are marked with TODO. The actual rules are in the section [Definitions](#Definitions).

## Using this document

The intended use of this document is to take the following [Definitions](#Definitions) section and modifying it to fit your particular game. The license used is intended for software, but it was picked because it is a standard, permissive open source license.

I don't expect any attribution or similar for this work, alto I would appreciate referring back to the video and/or github where it is hosted.

## Motivation

I'm not a fan of ambiguous rules and a big fan of open standards. This document is an attempt at creating a freely available set of basic rules to make it easier for game makers everywhere to ensure that their rules are consistent.

## Further reading
The concepts that inspired this work are generally standard concepts used within software engineering. If you are looking for ways to change this system, and don't have the context from the field, I recommend looking at:

  * [My own youtube channel](https://www.youtube.com/@mrhelanhalvan).
  * Algorithms and data structures, [w3s website](https://www.w3schools.com/dsa/dsa_intro.php). These are the fundamental building blocks used to handle data within computers, they can be used in a lot of other places. Generally though, the field is focused on efficiently handling the data, which is unlikely what we want for making board games.
  * Erlang/Beam virtual machine eco system, [CodeSync on Youtube](https://www.youtube.com/@CodeSync). They have a pragmatic way of dealing with problems within distributed systems.
  * [Pax talk on boardgame rules](https://www.youtube.com/watch?v=vuThpe-Rgxs).

# Definitions
This section goes into details of the rules.

## Active Rules Text

TODO

## Operations
**Operation**(s) are units by which the game state is modified. **Operations** are carried out one at the time and cannot be interrupted, (i.e they are atomic). **operations** are carried out in the order written.

## Skipping

If a part of an **operation** cannot be carried out, **skip** the entire operation.
**Skipping** is the act of not carrying out any of it's effects.

Examples of **skipping** happening is a player being unable to draw cards because their deck does not have enough cards in it or being unable to pay because they don't have enough of a resource.

If a player has multiple options, they cannot chose an option they are unable to carry out, it is not available to them. If a player has no valid options in a choice (for example because they are unable to carry out any of them), the entire **operation** is **skipped**.

### Skipping Example

You have 2 cards left in your deck, and have the **operation**: "draw 3 cards" to carry out. You cannot carry out the **operation**, it is skipped, you draw no cards. If you instead have the **operation**: "draw a card or gain a gold 3 times", you can chose to draw at most twice, and hence have to chose to gain a gold at least once.

## Ability ordering
Whenever abilities could be applied at the same time, the **TODO** decides in which order abilities should be applied.

This choice is made per ability, so if there are 5 abilities to resolve, the player decides one of the 5 to fully resolves first, then decides one of the remaining 4 to fully resolve until all abilities are resolved. Note that this is different than deciding on the ordering of all 5 abilities before starting to resolve them.

## Triggered Abilities
**Triggered abilities** are abilities that are caused by some game state change. They are always formulated "whenever X, Y", where X is the **condition** and Y is the **operation** to carry out.

Once a **operation** concludes, all **triggered abilities** for which the **condition** was meet during the **operation** resolve one instance of their **operation** for each time the condition was meet, in order per [ability ordering](##Ability-ordering). Note that the order in which conditions are meet within the triggering **operation** is irrelevant.

If a **operation** causes **triggered abilities** to trigger while there are other **operation**(s) waiting to resolve, resolve the most recently added **operation**(s) first.

### Triggered Abilities Example
You have 2 **Triggered abilities**:

 1. whenever you draw 2 cards, gain 1 gold
 2. whenever you gain a gold, draw a card
 3. whenever you gain dark energy, gain a gold

You resolve an **operation** "gain 2 gold and one dark energy" as **active player**. This causes 2 to trigger twice, and 3 to trigger once, adding 3 new **atomic operations** "draw a card", "draw a card", and "gain a gold". You can chose to resolve them any order. If you resolve "gain a gold" first, that triggers 2 again, which causes another "draw a card" which have to resolve before the previously added ones. Regardless of ordering this does _not_ trigger 1, as no **operation** full filled it's **condition**.

## Replacement Abilities
**Replacement abilities** are abilities that replace effects of **operation**(s). The are always formulated "if you would X, Y instead", where X is the **source** and Y is the **target**. **Replacement abilities** are applied in order (as per [ability ordering](##Ability-ordering)), at most once per **operation**.

**Replacement abilities** replace the **source** effect with the **target** effect. Hence if multiple **replacement abilities** have the same **source** and one is applied that removes the **source** from the **operation**, no subsequent **replacement abilities** can be applied to that **operation** unless anther intermediate **replacement ability** re-inserts the **source** effect into the **operation**.

Numbers are used to save text on cards, so "draw 3 cards" for example is a shorted down version of "draw a card, draw a card, draw a card".

### Replacement Abilities Example

You have 3 **replacement abilities**:

 1. if you would gain 1 gold, draw 2 cards instead
 2. if you would draw a card, gain 2 gold instead
 3. if you would gain 1 gold, gain 1 dark energy instead

You are about to resolve an **operation** "gain 1 gold" as the player responsible for ability ordering. You can now chose to apply 1 or 3. We chose 1, the **operation** now states "draw 2 cards", you now have to apply 2, changing the **operation** to "gain 4 gold". At this point you cannot apply 1, as it have already been applied, so you have to apply 3, changing the **operation** to "gain 4 dark energy", at this point there are no more replacement effects to resolve, so you gain 4 dark energy.

## Variable Binding

Variables use single capital letters, (X, Y, Z, etc.). Variables are immutable and scoped per **operation** or if the originate from a **activated ability** for the **cost** and **effect** of that ability. This means that the value of a variable cannot change and that each variable is unique per **operation** or **cost**/**effect** pair.

The player responsible for **ability ordering** (see [ability ordering](##Ability-ordering)) decides a value for variables in cases where the variable could have multiple values the first time it is used.

### Variable Binding Example

You have a card ability "pay X dark energy:gain X gold", and 5 dark energy. When resolving the **cost**, you pick a value for X. Because you cannot cause **cost**s to be **skipped**, you have to pick a value for X lower than the amount of dark energy you have, in this case a integer from 0 to 5. Whatever number you pick, that is the value X will have once the **effect** resolves.

If some **triggered ability** causes another **operation** to happen between when the **cost** is payed and the **effect** is resolved, and that triggered ability also contains X, that is another X which is entirely independent of the X bound in the **cost**.

## Game Entities

TODO

## Note Taking

TODO

## Relaxing Strictness
The strict system for resolving **operation**(s) exists to ensure there is as little ambiguity as possible in the game. However in cases where the exact ordering of operations do not matter, feel free to speed up the game by doing things in parallel. However ensure all players understand what operations are carried out and in what order.

# Credits

This document is made by me "helanhalvan".