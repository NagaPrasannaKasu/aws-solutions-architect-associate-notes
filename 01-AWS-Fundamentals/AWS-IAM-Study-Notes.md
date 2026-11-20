# AWS Identity and Access Management (IAM) - Study Notes

> AWS SAA-C03 preparation notes. These notes expand the points in the supplied `IAM.txt` reference and add a few clarifications to help avoid common exam misunderstandings.

## 1. What is AWS IAM?

- **AWS Identity and Access Management (IAM)** is a global AWS service used to control who can access AWS and what they are allowed to do.
- IAM helps manage identities, authentication, and permissions across an AWS account.
- IAM is not tied to one AWS Region. You use the same IAM identities and policies when working with resources in different Regions.
- **Authentication** answers: "Who are you?"
- **Authorization** answers: "What are you allowed to do?"

## 2. IAM Identities

IAM identities are used to represent people, applications, or services that need access to AWS resources.

### Root user

- The root user is created automatically when an AWS account is created.
- It uses the email address and password used to create the AWS account.
- It has full access to the account and its resources.
- Do not use the root user for everyday tasks. Use it only for tasks that specifically require root-user credentials.
- Protect the root user's password and enable MFA.
- Do not share root-user credentials and do not create root-user access keys for normal programmatic access.

### IAM users

- An IAM user is an identity created inside an AWS account.
- It can represent a person who needs access to AWS, or (in older designs) an application that needs long-term credentials.
- An IAM user can have console sign-in credentials, programmatic credentials, or both, depending on how it is configured.
- Permissions can be attached directly to a user or inherited from the groups the user belongs to.
- For human access in modern AWS environments, AWS generally recommends federation and temporary credentials rather than creating long-term IAM users wherever possible.

### IAM groups

- An IAM group is a collection of IAM users.
- You can attach permissions policies to a group so that its users receive the group's permissions.
- A user can belong to more than one group.
- Groups can make permission management easier; for example, you could create `Developers`, `Admins`, and `ReadOnly` groups.
- **An IAM group can contain users only. Groups cannot be nested inside other groups, and roles cannot be members of a group.**

### IAM roles

- An IAM role is an identity with a set of permissions that can be assumed by a trusted user, AWS service, or another account.
- A role does not use a permanent username/password or permanent access key in the same way as an IAM user.
- When a role is assumed, AWS provides temporary security credentials.
- Roles are commonly used by AWS services and applications so they can access other AWS resources securely.
- Common examples include:
  - **EC2 instance role:** allows an application running on an EC2 instance to access services such as S3.
  - **Lambda execution role:** gives a Lambda function permission to access resources it needs.
  - **CloudFormation service role:** lets CloudFormation perform specified actions when creating or updating a stack.
  - **Cross-account role:** lets a trusted identity in another AWS account assume a role and access permitted resources.

### Quick comparison

| IAM identity | Main purpose | Key point |
|---|---|---|
| Root user | Owns and controls the AWS account | Full account access; protect it and avoid daily use |
| IAM user | Identity within an AWS account | Can have console or programmatic credentials |
| IAM group | Organizes IAM users | Permissions attached to the group apply to its users |
| IAM role | Delegates access to trusted identities or services | Assumed to obtain temporary credentials |

## 3. IAM Policies and Permissions

- An **IAM policy** is a JSON document that defines permissions.
- Policies specify which actions are allowed or denied, on which resources, and under which conditions.
- Policies can be attached to IAM users, groups, and roles.
- A user can receive permissions from policies attached directly to the user and from policies attached to the groups the user belongs to.
- Follow the **principle of least privilege**: give an identity only the permissions it needs to perform its task, and no more.

### Main elements of an IAM policy

| Element | Required? | Meaning |
|---|---|---|
| `Version` | Yes in normal policy examples | Identifies the policy language version. The commonly used version is `2012-10-17`. |
| `Id` | Optional | An identifier for the policy document. |
| `Statement` | Required | Contains one or more permission statements. |
| `Sid` | Optional | A label that identifies an individual statement. |
| `Effect` | Required | Specifies `Allow` or `Deny`. |
| `Principal` | Depends on policy type | Identifies who is allowed or denied access in a resource-based policy or a role trust policy. It is not included in a normal identity-based permissions policy. |
| `Action` | Required in a typical permissions statement | Lists the API actions, such as `s3:GetObject`, that are allowed or denied. |
| `Resource` | Required in a typical permissions statement | Identifies the AWS resources to which the actions apply. The JSON key is singular: `Resource`. |
| `Condition` | Optional | Adds conditions that must be met for the statement to apply, such as source IP or MFA status. |

### Important policy clarification: Principal

- Your original notes describe `Principal` as the account, user, or role to which a policy is applied.
- More precisely, **identity-based policies** are attached to a user, group, or role, so the identity is already implied. These policies do not contain a `Principal` element.
- **Resource-based policies** specify a `Principal` because they are attached to a resource and must identify who can access that resource.
- A role's **trust policy** is a resource-based policy that specifies which principals are allowed to assume the role.

### Example: simple identity-based policy

This example allows an identity to list one S3 bucket and read objects inside it. The policy is illustrative; replace the example bucket name with a real bucket when practising.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowReadFromExampleBucket",
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket",
        "s3:GetObject"
      ],
      "Resource": [
        "arn:aws:s3:::example-bucket",
        "arn:aws:s3:::example-bucket/*"
      ]
    }
  ]
}
```

### Policy rules to remember

- `Allow` grants the specified permission, subject to the full policy evaluation.
- `Deny` explicitly blocks the specified permission.
- An **explicit Deny overrides an Allow**.
- If no applicable policy allows an action, access is denied by default (implicit deny).
- `Action` identifies what API operation can be performed; `Resource` identifies what the operation applies to.
- `Condition` can make access more restrictive. For example, a condition can require MFA or restrict requests to a particular source IP address.

### Managed and inline policies

- **AWS managed policy:** created and maintained by AWS.
- **Customer managed policy:** created and managed by you; can be reused across multiple users, groups, and roles.
- **Inline policy:** embedded directly in one user, group, or role and has a one-to-one relationship with that identity.

## 4. IAM Password Policy and MFA

### IAM password policy

- An AWS account can have a password policy for IAM users who sign in to the AWS Management Console.
- The policy can define password complexity requirements, minimum length, and (where configured) password expiration and reuse settings.
- The IAM password policy applies to IAM user passwords. It does **not** apply to the AWS account root-user password or to access keys.
- A strong password should be unique and difficult to guess.

### Multi-Factor Authentication (MFA)

- MFA adds another verification step when signing in.
- It combines something you know (such as a password) with something you have or control (such as an authenticator or security key).
- Enable MFA for the root user and for IAM users that sign in interactively, according to your security requirements.
- AWS recommends phishing-resistant options such as passkeys and FIDO security keys whenever possible.

Common MFA options include:

- **Virtual MFA device:** an authenticator application on a phone or other device that generates time-based one-time passwords (TOTP).
- **Hardware TOTP token:** a physical device that generates one-time codes.
- **FIDO security key / passkey:** a phishing-resistant authentication option based on public-key cryptography.

## 5. Ways to Access AWS

### AWS Management Console

- The console is the browser-based graphical interface for managing AWS resources.
- Users normally sign in with a username or email-based identity and password, and complete MFA when it is enabled or required.

### AWS Command Line Interface (CLI)

- The AWS CLI lets you manage AWS services by running commands in a terminal.
- It is useful for automation, scripts, and repeatable administration tasks.
- The CLI must be configured with valid AWS credentials or an authentication method such as IAM Identity Center sign-in.

### AWS Software Development Kits (SDKs)

- AWS SDKs let application code call AWS services using supported programming languages.
- Applications use an AWS credential provider to obtain credentials when making API requests.

### Access keys and credential safety

- A long-term IAM user access key consists of an **access key ID** and a **secret access key**.
- Both parts are used to sign programmatic requests from tools such as the CLI and SDKs.
- The secret access key is shown only when the key is created. Store it securely.
- Never share access keys, commit them to GitHub, place them directly in source code, or include them in screenshots or notes.
- If a key is exposed, deactivate or delete it and replace it as appropriate.
- **Preferred approach:** use temporary credentials from IAM roles or IAM Identity Center instead of long-term IAM user access keys whenever possible.
- For applications running on EC2 or Lambda, attach an appropriate IAM role rather than storing permanent access keys in the application.

## 6. IAM Roles for AWS Services

- Some AWS services need permission to call other AWS services on your behalf.
- Instead of saving a user's access keys in the service or application, create and assign an IAM role with the required permissions.
- The service or workload assumes the role and receives temporary credentials.
- A role has two important policy concepts:
  - **Trust policy:** defines who or what is allowed to assume the role (for example, the EC2 service or Lambda service).
  - **Permissions policy:** defines what actions the role is allowed to perform after it is assumed.
- The trust policy does not grant access to S3, DynamoDB, or other workload resources by itself. The permissions policy grants those actions.

### Example: EC2 instance accessing S3

1. Create an IAM role trusted by the EC2 service.
2. Attach a permissions policy that allows only the required S3 actions on the required bucket.
3. Attach the role to the EC2 instance through an instance profile.
4. The application on the instance obtains temporary credentials from the role.
5. The application uses those credentials to call S3 without storing long-term access keys.

## 7. IAM Security and Audit Tools

### IAM Credential Report (account-level)

- The credential report provides a report of IAM users in the account and the status of their credentials.
- It can show information such as whether a user has a console password, whether MFA is enabled, and the status and age of access keys.
- It helps administrators identify users with missing MFA, unused credentials, or old access keys.
- Remember: it is an account-level report, not just a report for one user.

### IAM Access Advisor / Last Accessed Information (identity or policy-level)

- Access Advisor helps review which AWS services an identity or policy has permissions to access and when those services were last accessed.
- This information can help identify permissions that are not being used.
- Administrators can use it as one input when reviewing and reducing permissions.
- Do not remove a permission solely because it appears unused without checking the workload and business requirements.

## 8. IAM Best Practices - Quick Checklist

- Protect the root user; enable MFA and avoid using it for everyday work.
- Do not share passwords, secret access keys, or MFA devices.
- Prefer IAM roles and temporary credentials over long-term access keys.
- Use IAM groups to manage permissions for users with similar responsibilities.
- Apply least-privilege permissions.
- Use customer managed policies when reusable, account-specific permissions are needed.
- Review users, roles, access keys, and permissions regularly.
- Remove or deactivate credentials that are no longer needed.
- Use MFA for interactive access.
- Never commit secrets or access keys to a public or private source-code repository.

## 9. AWS SAA-C03 Exam Reminders

- **IAM is a global service**; it is not created separately in each Region.
- **Users** are identities; **groups** organize users; **roles** are assumed to obtain temporary credentials.
- A group can contain users only; groups cannot contain other groups.
- A policy is a JSON permissions document.
- `Statement` is required; `Sid`, `Id`, and `Condition` are optional.
- `Effect` is `Allow` or `Deny`.
- Explicit Deny overrides Allow.
- `Principal` belongs in resource-based policies and role trust policies, not ordinary identity-based policies.
- A role's trust policy says **who can assume the role**; its permissions policy says **what the role can do**.
- For EC2 and Lambda workloads, use IAM roles instead of embedding long-term access keys.
- Credential Report = account-level credential status.
- Access Advisor = service access / last-accessed information used during permission reviews.

## 10. Quick Self-Check

1. **Which IAM identity is created automatically with an AWS account?**  
   The root user.

2. **Can an IAM group contain another IAM group?**  
   No. IAM groups contain users only.

3. **What is the difference between an IAM user and an IAM role?**  
   An IAM user is an identity in the account; a role is assumed by a trusted identity or service and provides temporary credentials.

4. **Which policy element specifies Allow or Deny?**  
   `Effect`.

5. **Does a normal identity-based policy contain `Principal`?**  
   No. The attached identity is implied. Resource-based policies and role trust policies specify a Principal.

6. **An EC2 application needs to read objects from S3. What is the recommended approach?**  
   Attach an IAM role to the EC2 instance with least-privilege S3 permissions.

7. **Which tool gives an account-level view of IAM credential status?**  
   IAM Credential Report.

8. **Which IAM feature helps identify services a user or role has accessed?**  
   Access Advisor / Last Accessed Information.

## 11. Official AWS References

- [What is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)
- [Policies and permissions in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)
- [IAM JSON policy element reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html)
- [Root user best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/root-user-best-practices.html)
- [Multi-factor authentication in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html)
- [Set an account password policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_passwords_account-policy.html)
- [Manage access keys for IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html)
- [IAM security best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [Generate credential reports](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html)
- [Reviewing last accessed information](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started-reduce-permissions-last-accessed.html)

---

**Study tip:** First understand the difference between users, groups, roles, and policies. Then practise reading simple JSON policies and deciding whether a user, role, or service should receive the permission. These distinctions appear frequently in AWS architecture and security scenarios.
