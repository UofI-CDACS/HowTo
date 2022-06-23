# Tips for using git

## .gitignore
This allows you to keep locally generated files out of visibility by git tools. Some things you really don't want to put into a git repository. Let's start with Mac.

## Globally ignore .DS_Store
.DS_Store is created in Mac directories. I'm not entirely sure why. Perhaps someone could explain that here. You really don't want this file in a git repository.

```
echo .DS_Store >> ~/.gitignore_global
git config --global core.excludesfile ~/.gitignore_global
```

If you are using VSCode you will probably need to refresh the SOURCE CONTROL view, or reload VSCode.
