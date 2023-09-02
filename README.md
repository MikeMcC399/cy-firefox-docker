Cypress Feature Request
https://github.com/cypress-io/cypress/issues/27121

Better error message for Firefox in GitHub Actions with Cypress Docker

What would you like?

If an attempt is made to run Cypress against the Mozilla Firefox browser in a [Cypress Docker container](https://github.com/cypress-io/cypress-docker-images) runner under [GitHub Actions](https://docs.github.com/en/actions) when the user is `root` Cypress should log the error message from Firefox:

"Running Firefox as root in a regular user's session is not supported.  ($HOME is /github/home which is owned by uid 1001.)"

This is the error message which is produced when
`firefox --version`
is executed directly in a GitHub Actions workflow.

Cypress however outputs the message:

"Browser: firefox was not found on your system or is not supported by Cypress."

Why is this needed?

The message "firefox was not found on your system" is misleading. This message is output even though Firefox is installed.

The resolution necessary is to use the GitHub Action [`jobs.<job_id>.container.options`](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idcontaineroptions) with `--user 1001` to match the ownership of the `$HOME` directory.

Other

Repo https://github.com/MikeMcC399/cy-firefox-docker demonstrates this issue using

[cypress/browsers:node-20.5.0-chrome-114.0.5735.133-1-ff-114.0.2-edge-114.0.1823.51-1](https://hub.docker.com/layers/cypress/browsers/node-18.16.0-chrome-114.0.5735.133-1-ff-114.0.2-edge-114.0.1823.51-1/images/sha256-e4c1a47c8107c37ca47398d8936743965d871c7285f58b852d5cb2658c400922?context=explore)

See [.github/workflows/firefox.yml](https://github.com/MikeMcC399/cy-firefox-docker/blob/master/.github/workflows/firefox.yml)
