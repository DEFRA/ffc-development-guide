# Code review
When code reviews are conducted to a high standard, they provide a valuable learning opportunity for the author, reviewer and any observers of the review process. FFC projects implement strict controls such that all changes must be made in feature branches and merged via pull request (PR). In order to merge a PR, it must first be approved by at least one reviewer.

When reviewing a pull request on any FFC project, the following guidance should be followed.

## Tone of code review comments
The tone of communications is extremely important in fostering an inclusive, collaborative atmosphere within teams.

Remember that your colleagues put a lot of effort into their work and may feel offended by harsh criticism, particularly if you make assumptions or imply a lack of thought. Approach code reviews with kindness and empathy, assuming the best intentions and applauding good solutions as well as making suggestions for improvement.

Comments should be used to give praise for good solutions and to point out potential improvements. They should be not be used to criticise your colleagues or make strongly opinionated statements. Always be mindful of your tone, considering how others might perceive your comments, and be explicit about when a comment is non-blocking or unimportant.

### Be constructive
Don't use harsh language or criticise without making constructive suggestions.
Do suggest alternative approaches and explain your reasoning.

### Be specific
Don't make vague statements about the changes as a whole.
Do point out specific issues and offer specific ideas for how the changes can be improved.

### Avoid strong opinions
Don't make strong, opinionated statements or dictate specific changes.
Do ask open-ended questions and keep an open mind about alternative solutions and reasoning that you may not have thought of.

## Scope of a code review
Code reviews should focus on what is being changed and whether the change is appropriate.

Expanding the scope of a pull request at the review stage is not acceptable. It is generally more valuable to swiftly conclude a piece of work that the team has prioritised than to opportunistically seek additional changes, such as refactoring related code. That said, it can be useful to comment on refactoring opportunities without blocking the pull request. It may be that the author has some spare time they can use to make those changes, or can incorporate them with their next piece of work.

Examples of things to look for and comment on in a code review:

### What has changed
Are the changes focussed on a specific issue, referenced in the pull request description? If the changes go beyond the intended scope, should they be broken up to make the code review more manageable? If the code review is still a manageable size, consider making a non-blocking comment to remind the author that they could control the scope of future pull requests more carefully.

### Maintainability
Is all new code extensible and easy for other developers to understand? Does it follow common design patterns? Look for unnecessary complexity and remember that this is subjective so take care not to be overly critical or opinionated when commenting on maintainability. Try to make specific suggestions for improvement, rather than simply stating a problem, but use open-ended language to encourage discussion.

### Duplication
Is there duplication within new code or between new and existing code? Could an existing abstraction be reused or should a new abstraction be created? When proposing an abstraction, consider using a non-blocking comment to open a dialogue about what the abstraction should be and whether it needs addressing now or as a separate piece of work.

Identifying useful abstractions at the review stage may indicate that there wasn't enough collaboration before coding began. See this as a trigger to review the team's ways of working, rather than blocking a pull request unnecessarily.

### Reusability
Have any new abstractions been introduced? Are they sufficiently reusable? If other parts of the system could be updated to use the new abstractions, consider suggesting this in a non-blocking comment so it can be discussed and either addressed in the same pull request or added to the product backlog as technical debt.

### Impact on other parts of the system
Will the changes have knock-on effects or otherwise necessitate changes to other parts of the system?

### Unit test coverage
Is all new code covered by detailed unit tests? Have any edge cases been missed? Don't rely on metrics such as code coverage. Inspect the code thoroughly and ensure that the tests contain appropriate assertions to confirm that all the intended functionality works as expected.

### Integration tests
Have integration tests been added to cover all changes to functionality?

## Suggesting improvements
When concluding a review, there are three options:

1. Comment
2. Approve the PR
3. Block the PR by requesting changes

### When to comment
You should conclude with a comment when your review asks questions which need answering before you can determine whether the pull request is acceptable. If you haven't proposed a solution to a specific problem in the pull request, it's generally better to leave a neutral review than to block the PR with a request for change.

If you have raised concerns or questions through inline comments, it may be helpful to give a concise summary in your concluding comment.

### When to approve
You should approve a pull request when you have confirmed that:

1. it meets the objectives set out in its description
2. it doesn't introduce new defects or code that is hard to maintain
3. all new and modified code has thorough unit test coverage
4. integration tests have been added where appropriate
5. there are no unresolved questions or comments against the pull request

Occasionally, it may be prudent to accept a pull request which does not meet all of the above requirements, such as to resolve an urgent issue with the live product. Such cases must always be agreed between the product owner, author(s) and reviewer(s).

If comments or questions on a pull request have been addressed elsewhere (e.g. face-to-face or on Slack), ensure that the outcome is recorded in comment replies so that it is visible to anyone looking back at the pull request in future for information.

Use the Resolve button to make it clear which of your concerns have been addressed and which still need attention. A pull request should only be approved after all reviewers have explicitly indicated that each of their comments have been resolved. If several reviewers have replied to a comment thread, it may appropriate for any of them to resolve that conversation on behalf of the group.

Sometimes, a reviewer may become unavailable after commenting on a pull request. When that happens, a second reviewer may accept the pull request without resolving the first reviewer's comments, as long as the author and second reviewer agree that they believe all legitimate concerns have been addressed. Before accepting the pull request, the second reviewer should reply to each unresolved comment to indicate that they believe it has been addressed.

### When to request changes
You should request changes to a pull request if any of the following are true:

1. it creates a defect in the product
2. it exacerbates an existing defect
3. it doesn't meet the objectives set out in its description
4. it would make the product more difficult to maintain
5. new or modified code lacks thorough unit tests
6. required integration tests have not been included

### When **not** to request changes
> Perfection is the enemy of good

A pull request does not need to be perfect to be good enough. Often, there are many solutions to a problem and one which is not the best is still good enough to meet current needs. Perhaps an alternative approach would be more efficient or an abstraction could reduce duplication, but those things may be less important than the next item in the product backlog.

Make suggestions for improvement as comments without explicitly approving or rejecting the pull request. This way, your suggestions can open dialogue with the author about how important your suggestions are compared to other work you could each move on to.
