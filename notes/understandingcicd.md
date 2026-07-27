# Continuous Integration + Continuous Delivery/Deployment (CI/CD)

The automation of build → test → deploy, enabling fast, reliable, low-risk software releases

## Continuous Integration (CI)
- Integrating code changes frequently  rather than everyone merging their work at the end
- Catches and fixes errors quickly, keeping development smooth

## Continuous Delivery (CD)
- Ensures code is **always in a deployable state** after integration
- Every change that passes the testing pipeline results in a release-ready version of the product
- Deployment to production is still a manual trigger/decision

## Continuous Deployment (CD)
- Takes continuous delivery further: every change that passes **all** pipeline stages is **automatically released to users**
- Fully automated — no manual deployment step at all

**In short:** CI/CD helps teams work together more effectively, catch errors early, and release updates quickly and safely

---

## The CI/CD Pipeline — Step by Step

1. **Change & commit** — developer makes changes, `git add` → `git commit` → `git push`
2. **Build trigger** — the push automatically triggers a build process
3. **Build** — code is compiled, dependencies assembled; team notified of success/failure
4. **Automated tests** — run to confirm changes don't break existing functionality; outcome communicated to team
5. **Deliver to staging** — if tests pass, build goes to a staging environment for further testing
6. **Deploy to production** — code goes live for users

---

## Why CI/CD Matters

| Benefit | Description |
|---|---|
| **Fast delivery** | Automation speeds up shipping features and fixes |
| **Improved quality** | Continuous testing/integration catches bugs early |
| **Reduced risk** | Small, incremental changes are easier to test and deploy than big-bang releases |
| **Better collaboration** | Frequent integration means fewer conflicts, better communication, shared responsibility |

---

## Popular CI/CD Tools

- **GitLab CI/CD** — built directly into GitLab; user-friendly; strong choice if already using GitLab for version control
- **Jenkins** — open source, extremely powerful/flexible with plugins, but known for complexity ("the eccentric uncle with a tool for everything but a messy garage"). Steep learning curve, big payoff once mastered
- **CircleCI** — cloud-based, fast, simple; integrates well with GitHub/Bitbucket; popular with smaller teams/startups for quick setup
- **TravisCI** — cloud-based, integrates with GitHub, simple and works out of the box.
- **GitHub Actions** — integrated directly into GitHub; deep integration with the most widely used version control platform, especially for open-source projects. **This is the tool used for the rest of the module.**
- **Cloud-native options** — AWS, Azure, and GCP each have their own built-in CI/CD services.

Choice of tool generally comes down to team workflow, existing version control platform, and project needs

---

## Role of CI/CD in DevOps

CI/CD is often visualized as an **infinity loop** — CI on the left, CD on the right — representing a continuous process:

**CI side:** Code → Build → Test
- Code: developers commit changes frequently to source control (GitHub, GitLab, Bitbucket, etc.)
- Build: code is automatically compiled, dependencies checked
- Test: automated tests verify no new bugs/issues introduced

**CD side:** Release → Deploy → Monitor
- Release: successfully tested code goes to staging/production based on feedback
- Deploy: application goes live to production for users
- Monitor: ongoing observation of the deployed code

### Benefits of CI/CD in DevOps
- **Collaboration** — breaks down silos, fosters shared responsibility
- **Automation** — reduces manual errors, frees developer time, ensures consistency
- **Continuous feedback** — immediate feedback on changes, early issue detection, faster dev cycles
- **Consistency** — automated pipelines behave the same across environments, solving the "works on my machine" problem

---

## CI/CD in the Broader DevOps Architecture

Three key stages, which can loop back and forth (e.g., a logging issue can send you back to fix code/pipeline):

1. **Source Control** — GitHub, GitLab, Bitbucket, etc. Enables multiple developers to collaborate without conflict, keeps history of changes.
2. **CI/CD** — automates build, test, and deployment; ensures continuous integration, testing, and deployment of code changes.
3. **Monitoring & Logging** — after deployment, tools like **Prometheus**, **Grafana**, and the **ELK stack** track performance, surface issues, and log important events — feeding back into the cycle for ongoing improvement.

---
