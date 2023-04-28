# CloudFormation Module: Github OIDC
An AWS cloud formation module that will:
 - Configure AWS to trust GitHub's OIDC as a federated identity
 - Create a role that will be assumed by calls made from Github Actions if they are made within the scope of the given repository
 
The module will output a reference to the new role which can then be used to add further permissive policies

## Properties

| Name | Required? | Description |
| --- | :---: | --- |
| GitHubOrg | Y | For personal accounts, use your username instead |
| RepositoryName | Y | Repository where the Github Actions will be hosted |
| OIDCProviderArn | N | If omitted, CF will attempt to create the Identity Provider entry in AWS, if an ARN for an existing entry is provided that will be used instead. Note that AWS will only allow one Github configured Identity Provider to exist within an account |

## Example
```yaml
Resources:
  OIDC:
    Type: AndyToddDev::OIDC::Github::MODULE
    Properties:
      GitHubOrg: andy-todd-dev
      RepositoryName: firstplayer
  CloudBuilderPolicy:
    Type: AWS::IAM::ManagedPolicy
    Properties:
      PolicyDocument:
        Version: 2012-10-17
        Statement:
          - Effect: Allow
            Action:
              - cloudformation:*
              - s3:*
              - route53:*
              - cloudfront:*
            Resource: "*"
      Roles:
        - !Ref OIDC.Role
```
