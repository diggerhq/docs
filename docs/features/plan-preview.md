# Plan preview

* The default way of working with digger is creating a pull request, previewing the plan within the PR as a comment, approving and applying the change within the change and then merging the pull request to the default branch
* This guarantees that any pull request merged to the default branch reflects the infrastructure (since the apply will succeed before merging the pull request)
* By performing locks on pull request we guarantee that the plan preview on the pull request is not stale. i.e. the infrastructure was not touched by another subsequent change
* Code in github: [https://github.com/diggerhq/digger/blob/5815775095d7380281c71c7c3aa63ca1b374365f/pkg/digger/digger.go#L228](https://github.com/diggerhq/digger/blob/5815775095d7380281c71c7c3aa63ca1b374365f/pkg/digger/digger.go#L228)
