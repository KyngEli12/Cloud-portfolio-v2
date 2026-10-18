# Module 04 — Federation

## What We Built

### Component 1: Enterprise SSO with IAM Identity Center
- Enabled IAM Identity Center (organization instance `ssoins-72233fcc40415898`)
- Created identity store user `eli.test`
- Created permission set `EliReadOnly` with AWS managed `ReadOnlyAccess` policy
- Assigned `eli.test` + `EliReadOnly` to AWS account `994788128952`
- Verified assignment in Console: IAM Identity Center → AWS accounts → Kyng

### Component 2: Web App with Federated Social Login
- Created Cognito User Pool `eli-federation-demo` with hosted UI
- Enabled email sign-in and self-registration
- Verified Cognito Hosted UI live at `5cm4mv.auth.us-east-1.amazoncognito.com`

## Key Concepts
- **IAM Identity Center**: org-wide SSO — one login to access multiple AWS accounts
- **Permission Sets**: define what IAM permissions a federated user gets
- **Account Assignments**: link user + permission set to a specific AWS account
- **Cognito User Pool**: managed user directory with hosted login UI for web apps
- **Federated Identity**: users authenticate via external IdP, not IAM users

## Status: Complete
