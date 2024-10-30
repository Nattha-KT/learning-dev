# How to set up a `pre-commit` hook in husky

### 1. **Manual Setup of Husky Hook**

After initializing Husky with `npx husky install`, manually create a new `pre-commit` file in the `.husky` directory with your desired commands.

1. **Initialize Husky (if you haven’t already):**

   ```bash
   npx husky install
   ```

2. **Create the `pre-commit` Hook Manually**

   Create a file in the `.husky` directory for `pre-commit`:

   ```bash
   echo -e '#!/bin/sh\n. "$(dirname "$0")/_/husky.sh"\nnpm run test && npx lint-staged' > .husky/pre-commit
   ```

3. **Make the Hook Executable**

   Ensure the new file is executable:

   ```bash
   chmod +x .husky/pre-commit
   ```

### 2. **Alternative: Use Package.json to Run Tests and Lint Staged Files**

You can set up `npm test` and `lint-staged` to run as part of your `pre-commit` script in `package.json`:

   ```json
   "husky": {
     "hooks": {
       "pre-commit": "npm run test && npx lint-staged"
     }
   }
   ```

This approach will avoid using deprecated Husky commands and will still enforce your tests and lint-staging checks on each commit.
