# GitHub Actions 

## GitHub Actions — CI/CD Workflow in Practice

The end-to-end process, from writing code to deployment:

1. **Code** — developers write features/fixes, make changes to the codebase.
2. **Commit** — changes are committed/pushed to the repository.
3. **GitHub Actions workflow triggered** — the commit kicks off a workflow, defined in a **YAML file**, which specifies what actions to run on which events (e.g. on commit/push).
4. **CI pipeline runs:**
   - **Build** — code is compiled and dependencies resolved.
   - **Automated tests** — verify new code doesn't break existing functionality and new features work as expected. Critical for maintaining code quality.
   - **Test outcome check** — pass → workflow continues 
                            — fail → process stops and developers are notified to fix issues
5. **Package** — if build and tests succeed, the code is packaged into a deployable version (e.g. a Docker image or compiled binary).
6. **Deploy** — packaged code is deployed to staging, testing, or production, depending on the workflow.
7. **Monitor** — once live, the application is continuously monitored to catch and address issues quickly.

---

## Common Use Cases for GitHub Actions

| Use case | What it does | Real example |
|---|---|---|
| **Continuous Integration** | Automatically builds and tests code on every push, catching issues early and keeping the codebase in a good state | Running unit tests on every pull request |
| **Continuous Deployment** | Automatically deploys code to production (or another environment) once all tests pass — speeds up releases, cuts manual intervention | Auto-deploying a web app to AWS, Azure, or GCP after tests pass |
| **General Automation** | Automates repetitive workflow tasks beyond just build/test/deploy | Auto-moving cards between columns on a GitHub project board based on issue/PR activity |

---