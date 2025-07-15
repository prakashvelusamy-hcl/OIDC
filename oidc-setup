# GitHub Actions with AWS OIDC Integration

This guide will walk you through the process of configuring GitHub Actions with AWS using OIDC (OpenID Connect) for authentication. By doing so, your GitHub workflows can assume roles in AWS securely without the need to manage long-term AWS credentials.

---

## Step 1: Create an OIDC Identity Provider in AWS

1. Go to the [IAM console](https://console.aws.amazon.com/iam/) → **Identity providers**.
2. Click **Add provider**.
3. Choose **OpenID Connect**.
4. Set the following values:
   - **Provider URL**: `https://token.actions.githubusercontent.com`
   - **Audience**: `sts.amazonaws.com`
5. Click **Get thumbprint** and then click **Add provider**.

This tells AWS to trust GitHub’s OIDC tokens.

---

## Step 2: Create an IAM Role for GitHub Actions

1. Go to the [IAM Roles](https://console.aws.amazon.com/iam/home#/roles) section and click **Create role**.
2. Choose **Web Identity** as the trusted entity.
3. Select the OIDC provider you just created (GitHub Actions).
4. Set **Audience** to `sts.amazonaws.com`.
5. Add the following trust policy to allow GitHub Actions to assume the role:

```json
{
  "Effect": "Allow",
  "Principal": {
    "Federated": "arn:aws:iam::<ACCOUNT_ID>:oidc-provider/token.actions.githubusercontent.com"
  },
  "Action": "sts:AssumeRoleWithWebIdentity",
  "Condition": {
    "StringEquals": {
      "token.actions.githubusercontent.com:aud": "sts.amazonaws.com",
      "token.actions.githubusercontent.com:sub": "repo:<ORG>/<REPO>:ref:refs/heads/<BRANCH>"
    }
  }
}



```

- name: Verify AWS identity
  run: aws sts get-caller-identity
