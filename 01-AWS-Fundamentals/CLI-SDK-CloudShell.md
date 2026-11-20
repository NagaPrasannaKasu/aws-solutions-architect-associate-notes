## Ways to Access AWS

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

### AWS CloudShell

- AWS CloudShell is a browser-based command-line environment provided by AWS.
- You can access it directly from the AWS Management Console.
- It allows you to manage AWS services using commands without installing AWS CLI on your local computer.
- It comes with AWS CLI pre-installed and configured.
- It is pre-authenticated using your AWS Console credentials.

