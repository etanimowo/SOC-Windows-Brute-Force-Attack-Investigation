# Detection Rules

## Basic Detection Rule

Alert on possible RDP brute force when:

- Event ID 4625 appears 10 or more times
- within 5 minutes
- from the same source IP
- against the same username
- with Logon Type 10

## Investigation Pointers

Review:

- Account Name
- Source Network Address
- Failure Reason
- Logon Type
- Event timestamps

## Example Analyst Conclusion

The observed authentication pattern is consistent with a brute force attack due to the repeated failed logon attempts against the same user account from the same remote source in a short time period.
