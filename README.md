# COSC2759 Assignment 1

## Notes App - CI Pipeline

- Full Name/Names: Dao Duc Binh
- Student ID/IDs: s4117444

### Guidance (remove this section before final submission)

1. Refer for assignment specification `Marking Guide` for details of what should appear in this README.

2. If you do not see an `Actions` tab in your GitHub, email craig.anslow@rmit.edu.au with URL to your repository, so that it can be enabled.

3. Implement your CI pipeline in the directory `.github/workflows`.

4. Refer to [src/README.md](/src/README.md) for important details on building and testing the application.

5. Commit images to the `img` directory and add them like

   ```html
   <img src="/img/md.png" style="height: 70px;" />
   ```

   <img src="/img/md.png" style="height: 70px;"/>

6. Only edit THIS README.md - not the src/README.md

## 1. How the pipeline runs

### 1.1 What triggers the pipeline

- The pipeline is defined at `.github/workflows/ci-pipeline.yml` and runs automatically in response to git events.
- When a push event on any branch a pull request from main happens, the pipeline is triggered.
- When triggered on any branch, 3 jobs would always run being e2e, lint, and unit-test. The build job only runs when there are push requests/code merge onto main.

### 1.2 What each job does

| Job         | Command run                                   | Purpose                                                                                                                                                                        |
| ----------- | --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `lint`      | `npm run test:lint`                           | Performs static code analysis to catch style issues and common bugs                                                                                                            |
| `unit-test` | `npm run test:unit`                           | Runs unit tests against the application's models, with code coverage collected and enforced via a coverage threshold in `jest.config.js`                                       |
| `e2e`       | `npm run test:e2e`                            | Starts the application and a MongoDB instance, then runs Playwright browser tests that simulate a real user creating and deleting a note through the UI                        |
| `build`     | Packaging steps and `actions/upload-artifact` | Copies the application's runtime files (`app.js`, `routes/`, `models/`, `views/`, `package.json`, `package-lock.json`) into a folder and uploads it as a downloadable artifact |

## 2. Expected outcomes

### 2.1 Passing pipeline

- By default, all jobs in the pipeline should be successful, and every job should show a green checkmark in the "Checks" tab of a Pull Request, or the "Actions" tab of the repository.
- When successful, the build job would create a deployable artifact.

### 2.2 Failing pipeline

- When failed, a job would have a read x mark next to it and when clicked on would display the step that ran into an error.
- The possible problems that each job can detect may be:
  - `lint` failures list the exact file, line, and rule that was violated.
  - `unit-test` failures show the failing assertion, expected vs. received values, and the test file/line.
  - `e2e` failures show which step of the test failed. A detailed error log can be found when clicking on the job, and an html report would also be generated when the job is performed on main.
  - `build` failures typically occur if the packaging step cannot find an expected file or folder and would be detailed in the error log.
