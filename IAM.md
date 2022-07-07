<link href="style.css" rel="stylesheet"></link>

# IAM #

## Policy type / Policy Statement ##

![polycy](assets/EffectivePermissions-session-boundary-id.png)
![policy](assets/EffectivePermissions-session-rbpsession-id.png)
![policy](assets/AccessPolicyLanguage_General_Policy_Structure.diagram.png)
- password policy
## MFA ##
Multi factor authentication
- activate in account
- virtual device
  - one time password
  - tokens for authentication
- U2F (universal 2nd Factor)
  - physical
  - 3rd party

IAM Access Key
User -> Security 
```aws cli 
aws iam list-users
```

IAM role for Servies
- allow service access other services
- common:
  - EC2 instance + IAM role -> call other service
  - Lambda
  - CloudFormation
<br/><br/>
## IAM Security tool ##
### Credential report
- CSV file
### IAM Access Advisor ###
- log
<br/><br/>
## AWS Shared Responsibilities
![AWSSR](assets/Shared_Responsibility_Model_V2.59d1eccec334b366627e9295b304202faf7b899b.jpg)
