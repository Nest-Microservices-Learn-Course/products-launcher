---
name: commit-and-push
description: Commit and push all submodules with structured messages and update root repository
---

name: commit-and-push-submodules
description: >
  Commit and push all git submodules and the root repository
  using structured commit messages with bullet points.

steps:
  - name: process-submodules
    run: |
      git submodule foreach '

      echo "-----------------------------"
      echo "Processing submodule: $name"

      CURRENT_BRANCH=$(git branch --show-current)

      if [ -z "$CURRENT_BRANCH" ]; then
        echo "⚠️  Skipping $name (detached HEAD)"
        exit 0
      fi

      # Pull latest changes first
      git pull --rebase origin $CURRENT_BRANCH

      if [ -n "$(git status --porcelain)" ]; then
        echo "Changes detected in $name"

        CHANGED_FILES=$(git diff --name-only)

        CHANGE_LIST=""
        for file in $CHANGED_FILES; do
          CHANGE_LIST="$CHANGE_LIST\n- updated $file"
        done

        TYPE="chore"
        if echo "$CHANGED_FILES" | grep -qE "(fix|bug)"; then TYPE="fix"; fi
        if echo "$CHANGED_FILES" | grep -qE "(feat|feature)"; then TYPE="feat"; fi
        if echo "$CHANGED_FILES" | grep -qE "(test)"; then TYPE="test"; fi
        if echo "$CHANGED_FILES" | grep -qE "(build|config)"; then TYPE="build"; fi

        git add .

        git commit -m "$TYPE: update $name

      $CHANGE_LIST
      "

        git push origin $CURRENT_BRANCH
      else
        echo "No changes in $name"
      fi
      '

  - name: process-root
    run: |
      echo "-----------------------------"
      echo "Processing root repository"

      CURRENT_BRANCH=$(git branch --show-current)

      git pull --rebase origin $CURRENT_BRANCH

      if [ -n "$(git status --porcelain)" ]; then
        echo "Changes detected in root repo"

        CHANGED_FILES=$(git diff --name-only)

        CHANGE_LIST=""
        for file in $CHANGED_FILES; do
          CHANGE_LIST="$CHANGE_LIST\n- updated $file"
        done

        TYPE="chore"
        if echo "$CHANGED_FILES" | grep -qE "(fix|bug)"; then TYPE="fix"; fi
        if echo "$CHANGED_FILES" | grep -qE "(feat|feature)"; then TYPE="feat"; fi
        if echo "$CHANGED_FILES" | grep -qE "(test)"; then TYPE="test"; fi
        if echo "$CHANGED_FILES" | grep -qE "(build|config)"; then TYPE="build"; fi

        git add .

        git commit -m "$TYPE: update root repository

      $CHANGE_LIST
      "

        git push origin $CURRENT_BRANCH
      else
        echo "No changes in root repo"
      fi

  - name: done
    run: echo "✅ All repositories are up to date"