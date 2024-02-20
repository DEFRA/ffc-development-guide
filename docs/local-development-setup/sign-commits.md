# Setup commit signing

You can sign commits and tags locally, so other people can verify that your work comes from a trusted source. If a commit or tag has a GPG or S/MIME signature that is cryptographically verifiable, GitHub marks the commit or tag as verified.

If a commit or tag has a signature that cannot be verified, GitHub marks the commit or tag as unverified.

You can check the verification status of your signed commits or tags on GitHub and view why your commit signatures might be unverified. 

## Setup

Follow GitHub's instructions for setting up and configuring a GPG key.

1. [Creating a new GPG key](https://docs.github.com/en/github/authenticating-to-github/generating-a-new-gpg-key)
1. [Adding a new GPG key to your GitHub account](https://docs.github.com/en/github/authenticating-to-github/adding-a-new-gpg-key-to-your-github-account)
1. [Setup Git to use new GPG key](https://docs.github.com/en/github/authenticating-to-github/telling-git-about-your-signing-key)

### Signing commits via VS code

To turn on signed commits in the VS Code UI, make sure the `Enable commit signing with GPG or X.509` is ticked. For the JSON version of the settings use `"git.enableCommitSigning": true`

