# Code Review Notes

Fill this in as you work through the milestones. Each section mirrors the structure of a real GitHub pull request review.

---

## PR #1 — Bulk Purchase (`pr1_bulk_purchase.py`)

### Summary
*What does this PR do? (1–2 sentences in your own words)*

> This PR should create a function that marks all the remaining unpurchased items in a list to purchased by the requesting user.

### Issues

For each issue you find, note: where it is (file + function), what's wrong, and why it matters in production.

**Issue 1**
- Location: pr1_bulk_purchase.py -> purchase_all_items
- What's wrong: This function should mark all the items in a list as purchased, however it doesn't validate weather this state change is valid(that the items in the list are unpurchased).
- Why it matters: If a function passes in a list that has un purchased items it would change the user and time it was purchased and corrupt the data.
- Suggested fix: Validate each item as you are iterating the list

**Issue 2**
- Location: pr1_bulk_purchase.py -> purchase_all_items
- What's wrong: This function should return the number of items purchased in the operation. However the function grabs all items in a list regardless if they were purchased or unpurchased, this leads to the return count being the total items in the list instead of the count of items purchased in the operation.
- Why it matters: This matters because it returns the wrong count of items that the user has just bought. This could lead to further bugs in stats and mis leading the user to thinking they incorrectly bought something causes confusion.
- Suggested fix: When grabbing the data from the database, filter for unpurchased items.

**Issue 3** *(if found)*
- Location: pr1_bulk_purchase.py -> purchase_all
- What's wrong: The user_id was never validated to be correct
- Why it matters: This matters because an invalid user can be inserted into the data base and corrupt the data.
- Suggested fix: Validate the userid before passing it to the function to make sure the data is correct.

### Questions for the Author
*Things you're uncertain about — design choices that could be intentional or bugs depending on intent.*

>

### Verdict
- [ ] Approve — ship it
- [x] Request Changes — needs fixes before merging
- [ ] Comment — needs discussion before a verdict

**Rationale** *(1–2 sentences)*:

> The pr will introduce several data corrupting bugs as well as incorrect output. Removal of these bugs is necessary before we can ship it.

---

## PR #2 — List Stats (`pr2_list_stats.py`)

### Summary
*What does this PR do? (1–2 sentences in your own words)*

> This PR should return stats of a list. It should return a dictionary with the following fields:  list ID, total count of items on a list, count of purchased items, count of unpurchased items, unpurchased item count by category in a dictionary.

### Issues

**Issue 1**
- Location: pr2_list_stats.py -> get_list_stats()
- What's wrong: This function incorrectly calculates the count by category dictionary. It counts all items regardless if they are purchased or not, when the spec wants only the count by category of items not purchased.
- Why it matters: The user will be confused if they have a count of 4 items left to get in dairy when 3 of them are already in there cart
- Suggested fix: add a check in the iteration over items if the item is not purchased.

**Issue 2**
- Location: pr2_list_stats.py -> list_stats()
- What's wrong: The function never catches the case where the user passes in an incorrect list_id
- Why it matters: This matters because we will never know why the operation is failing unless we see the error that is thrown
- Suggested fix: Catch the bad list id error.

**Issue 3** *(if found)*
- Location:
- What's wrong:
- Why it matters:
- Suggested fix:

### Questions for the Author
*A good code review often surfaces design questions, not just bugs. What would you want to clarify before approving?*

>

### Verdict
- [ ] Approve — ship it
- [x] Request Changes — needs fixes before merging
- [ ] Comment — needs discussion before a verdict

**Rationale** *(1–2 sentences)*:

> The PR implements a function that produces incorrect stats, as well as doesn't properly handle input validation, these need to be fixed before PR can be approved.

---

## Reflection

*Answer after completing both reviews.*

**1.** Which issue was hardest to spot, and why?

> The hardest issue to spot was the user_id validation. I knew that they user_id must be validated but I had to look at the rest of the codebase to see where they have some database validation so that I can correctly identify which function in the PR should have it.

**2.** Which issues do you think an LLM reviewer (like Claude reviewing its own code) would most likely miss? Why?

> I think the issue the LLM reviewer most likely miss is the `remaining` total count. The prompt the user used was ambiguous, meaning someone who read over it briefly or a LLM who could misinterpret it would calculate it like it did in the PR and fail to consider to put `remaining` in the context of a user shopping.

**3.** One thing you'd add to a code review checklist for AI-generated backend code:

> I would add "Does this PR follow the structure and testing of the rest of the codebase" so the LLM would look at other functions and see that they are testing for some input and check if that same testing is in the proposed PR.
