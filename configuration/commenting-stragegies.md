# Commenting stragegies

* The way digger comments plan previews on PRs can be configured based on the requirement of verbosity or summary. The value of the summary can be configured using the `reporting-strategy` parameter: [https://github.com/diggerhq/digger/blob/develop/action.yml#L105](https://github.com/diggerhq/digger/blob/develop/action.yml#L105)
* If you have many projects and would like to not pollute your pull requests with several plan comments for every you should chose `comments per run` or `latest run` reporting strategies which will shrink comments to a single comment per push
* If you would like a more verbose output you should chose the `multiple comments` strategy
* More details about the reporting strategy can be seen in the reporter interface: [https://github.com/diggerhq/digger/blob/5815775095d7380281c71c7c3aa63ca1b374365f/pkg/reporting/reporting.go#L12](https://github.com/diggerhq/digger/blob/5815775095d7380281c71c7c3aa63ca1b374365f/pkg/reporting/reporting.go#L12)
