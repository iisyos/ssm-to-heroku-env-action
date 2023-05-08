## ssm-to-heroku-env-action
<img src="./thumbnail.png">

GitHub Action that sets Heroku environment variables from AWS SSM Parameter Store.
This Action was created to efficiently synchronize when using the same environment variables on AWS and Heroku.

## Inputs

| Input                      | Required | Default                    | Info                                           |
|----------------------------|----------|----------------------------|------------------------------------------------|
| path                    | Yes      | N/A                        | The path to the environment variable in the AWS SSM Parameter Store that you want to set in Heroku                                  |
| email                | Yes       | N/A | Heroku email                             |
| token | Yes       | N/A                      | Heroku token
| app        | Yes       | N/A                       | Heroku app name                      |

**And you need to set AWS credentials by using like [AWS Credentials for GitHub Actions](https://github.com/aws-actions/configure-aws-credentials).**

## Description

First of all, this actions retrieve environment variables from AWS SSM Parameter Store specifying `path` input. This mean when you set like under CLI command, you can retrieve under `'/test/'`　recursively if you specified path as `'/test/'`.

```bash
aws ssm put-parameter \
    --name "/test/VALUE1" \
    --value "test1" \
    --type String \
    --region "ap-northeast-3"
```

Authentication to heroku is done by writing `email` and `token` in the `.netrc` file as written in the [Heroku CLI Authentication](https://devcenter.heroku.com/articles/authentication).You can get your token from [here](https://dashboard.heroku.com/account). And you need to set your app name to `app` input.

## Usage

You can perform this action whenever you like. Push is fine, and PR comments are also fine.

```yaml
    steps:
      - name: Checkout repository
        uses: actions/checkout@v2
      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ap-northeast-3
      - name: Sets Heroku environment
        uses: iisyos/ssm-to-heroku-env-action@v1.0
        with:
          path: ${{ secrets.PARAMETER_STORE_PATH }}
          email: ${{ secrets.HEROKU_EMAIL }}
          token: ${{ secrets.HEROKU_TOKEN }}
          app: ${{ secrets.HEROKU_APP_NAME }}
```