## IAM (Identity and Access Management)

* Root account is the account created when you first set up your AWS account, it has complete admin access by default
* Has no region, all settings are global.
* User interaction types (user can have one or the other or both):
 * Programmatic: use access key ID and secret access key
 * Console: use username and password
* User permissions (AKA “Policy Document” json file)
 * Can be assigned directly to user
 * Can be assigned to a Group, and user can be made a member of that group
* Power user: access to all AWS services EXCEPT management of users and groups within IAM
* You can disable and/or regenerate a user’s access key
Roles: allows one AWS service to interact with another
