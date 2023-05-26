<p align="center">
  <a href="https://github.com/actions/typescript-action/actions"><img alt="typescript-action status" src="https://github.com/actions/typescript-action/workflows/build-test/badge.svg"></a>
</p>

# Pupilfirst Grade Action
The grading action accepts a single input `report_file_path` which is the relative path to the `report.json` file in the checked out repository. The `report.json` should have the keys `feedback` which stores the feedback for the submission and `status` which determines whether to pass, fail or skip grading. The action expects the `REVIEW_END_POINT` and `REVIEW_BOT_USER_TOKEN` as env , the first being the GraphQL API endpoint of your school and the latter being the user token for the user created for bot actions. It is recommended to keep both of these as secrets. Here is a basic example:

```yaml
# Using the grading action to review a submission in the LMS.
- name: Grade a submission in the LMS
  uses: pupilfirstr/actions/grading@v1
  with:
    # File path when the report.json is in submission directory in the checked out repo.
    report_file_path: "submission/report.json"
  env:
    # GraphQL API endpoint for your school in the LMS.
    REVIEW_END_POINT: ${{ secrets.REVIEW_END_POINT }}
    # User API token of the review bot user.
    REVIEW_BOT_USER_TOKEN: ${{ secrets.REVIEW_BOT_USER_TOKEN }}
```

The valid statuses for grading in `report.json` file are `skip`, `pass`, `fail`. In the absence of a valid `report.json` file or valid `status` keys or missing `feedback`, grading will be skipped.

## How to build and release

When releasing a new version, you should always push two tags - the full version tag - `v1.2.3` for example, and the corresponding major version tag - `v1`, for `v1.2.3`. You will need to delete the existing major version tag, and push a replacement tag for each release.

```bash
# Build the action.
npm run all

# Delete the old local tag, and create it anew.
git tag -d v1
git tag v1
git tag v1.2.3

# Delete the old tag on origin before pushing updated ones.
git push origin :refs/tags/v1
git push origin v1
git push origin v1.2.3
```
