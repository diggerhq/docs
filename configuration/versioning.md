# Specifying version

{% hint style="warning" %}
For serious usecases always use a pinned version which is of the form @vX.Y.Z since this will download compiled binary. Addition to being faster to run, it is also more secure than using a commit from a branch
{% endhint %}

## Use a pinned stable version of Digger

To pin a specific release of Digger, you can use `@vX.Y.Z` tag in your workflow file:

```
- name: digger
  uses: diggerhq/digger@v0.1.10
  env:
    ...
```

## Use latest commit from a branch

You can also run latest commit from a specific branch

{% hint style="warning" %}
Only use this at your own risk in non-production scenarios. This can break things!
{% endhint %}

```
- name: digger
  uses: diggerhq/digger@yolo-lets-do-it
  env:
    ...
```
