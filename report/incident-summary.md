## Executive Summary

A simulated privilege escalation attack was performed by creating a new local account and assigning administrator privileges. Microsoft Sentinel successfully detected the activity through Windows Security Event ID 4732.

## Impact

Unauthorized administrative access could allow persistence, lateral movement, and system compromise.

## Conclusion

Detection rules monitoring privileged group membership effectively identify escalation attempts aligned with MITRE ATT&CK techniques.
