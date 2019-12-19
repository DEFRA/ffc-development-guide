# Install AWS AIM Authenticator

Amazon EKS uses IAM to provide authentication to your Kubernetes cluster through the AWS IAM Authenticator for Kubernetes.

## Prerequisites

* [AWS CLI](install-aws-cli.md)

## Install

Follow the instructions for your OS here:
* https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html

## Test

`aws-iam-authenticator token -i {cluster name}` should return a JSON doc with your token

## Notes

* Follow the Linux instructions when installing on WSL
