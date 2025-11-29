## Understanding ESC2

When a certificate template specifies the Any Purpose Extended Key Usage (EKU) or does not identify any Extended Key Usage, the certificate can be used for any purpose (client authentication, server authentication, code signing, etc.). If the template allows specifying a SAN in the CSR, a template vulnerable to ESC2 can be exploited similarly to ESC1 . In another scenario, if the requester cannot specify a SAN, it can be used as a requirement to request another certificate on behalf of any user.

## ESC2 Abuse Requirements

To abuse ESC2, the following conditions must be met:
1. The Enterprise CA must provide enrollment rights to low-privileged users.
2. Manager approval should be turned off.
3. No authorized signatures should be necessary.
4. The security descriptor of the certificate template must be excessively permissive, allowing low-privileged users to enroll for certificates.
5. The certificate template should define Any Purpose Extended Key Usage or have no Extended Key Usage specified.