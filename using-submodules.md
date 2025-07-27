Your understanding is correct and follows best practices for git submodules:

1. Make changes inside the submodule (p3), then commit and push those changes in the submodule repo.
2. Go to the root repo, run `git add p3`, then commit and push in the root repo. This updates the pointer to the new commit in the submodule.

There is no “better” way—this is the standard workflow for submodules.  
Summary:
- Always commit and push in the submodule first.
- Then update, commit, and push the parent repo to record the new submodule state.

Tip: Always run `git submodule update --init --recursive` after cloning the parent repo to ensure submodules are checked out correctly.