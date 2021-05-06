# Signing commits

You can sign commits and tags locally, so other people can verify that your work comes from a trusted source. If a commit or tag has a GPG or S/MIME signature that is cryptographically verifiable, GitHub marks the commit or tag as verified.

If a commit or tag has a signature that cannot be verified, GitHub marks the commit or tag as unverified.

You can check the verification status of your signed commits or tags on GitHub and view why your commit signatures might be unverified. 

## Follow these instuctuctions to generate a new GPG Key
https://docs.github.com/en/github/authenticating-to-github/generating-a-new-gpg-key

## Then add the Key to github
https://docs.github.com/en/github/authenticating-to-github/adding-a-new-gpg-key-to-your-github-account

## Signing commits when using the UI
https://docs.github.com/en/github/authenticating-to-github/signing-commits

## Signing commits via VS code
To turn on signed commits in the UI make sure the "Enable commit signing with GPG or X.509" is ticked. For the JSON version of the settings use `"git.enableCommitSigning": true`

## Auto sign commits
To automatically sign commits run the following command:
`git config --global commit.gpgsign true`