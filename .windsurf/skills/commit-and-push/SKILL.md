---
name: commit-and-push-submodules
description: >
  This skill goes through every git submodule and the root repository.
  It checks for changes, creates a structured commit message with a type
  and a list of updated files, then commits and pushes those changes.
---

steps:
  - name: process-submodules
    run: |
      For each submodule in the project:

      - Print a separator and indicate which submodule is being processed.
      - Detect the current branch.
      - If the submodule is in a detached HEAD state, skip it.

      - Pull the latest changes from the remote repository using rebase.

      - Check if there are any uncommitted changes.
        - If there are changes:
          - List all modified files.
          - Build a bullet list describing each updated file.
          - Determine the type of change (fix, feat, test, build, ci, perf, style, docs, revert or chore)
            based on file names.
          - Stage all changes.
          - Create a commit using this format:

            type: <general-description> "<submodule-name>"

            - change 1 description
            - change 2 description
            - change 3 description
            - etc...

          - Push the changes to the current branch.

        - If there are no changes:
          - Print that there are no changes in the submodule.

  - name: process-root
    run: |
      For the root repository:

      - Print a separator and indicate that the root repository is being processed.
      - Detect the current branch.
      - Pull the latest changes from the remote using rebase.

      - Check if there are any uncommitted changes.
        - If there are changes:
          - List all modified files.
          - Build a bullet list describing each updated file.
          - Determine the type of change (fix, feat, test, build, ci, perf, style, docs, revert or chore)
          - Stage all changes.
          - Create a commit using this format:

            type: <general-description> "<submodule-name>"

            - change 1 description
            - change 2 description
            - change 3 description
            - etc...

          - Push the changes to the current branch.

        - If there are no changes:
          - Print that there are no changes in the root repository.

  - name: done
    run: |
      Print a final message indicating that all repositories are up to date and general description of each one.