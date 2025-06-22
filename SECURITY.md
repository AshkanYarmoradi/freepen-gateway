# Security Policy

## Supported Versions

Use this section to tell people about which versions of your project are currently being supported with security updates.

| Version | Supported          |
| ------- | ------------------ |
| 1.0.x   | :white_check_mark: |

## Reporting a Vulnerability

We take the security of FreePenGateway seriously. If you believe you've found a security vulnerability, please follow these steps:

1. **Do not disclose the vulnerability publicly** until it has been addressed by the maintainers.
2. Email the details to [security@example.com](mailto:security@example.com) with the subject "FreePenGateway Security Vulnerability".
3. Include the following information in your report:
   - Description of the vulnerability
   - Steps to reproduce the issue
   - Potential impact
   - Any suggestions for remediation if you have them

The maintainers will acknowledge your email within 48 hours and provide a detailed response about the next steps in handling your report. After the initial reply, the maintainers will keep you informed about the progress towards a fix and full announcement.

## Security Best Practices for Deployment

When deploying FreePenGateway, consider the following security best practices:

### Network Security

1. **Deploy behind a firewall**: Restrict access to the application to only necessary IP addresses and ports.
2. **Use a reverse proxy**: Consider placing the application behind a dedicated reverse proxy like Nginx or Apache for additional security layers.
3. **Network segmentation**: Deploy in a segmented network environment to limit lateral movement in case of a breach.

### Server Hardening

1. **Keep the host system updated**: Regularly apply security patches to the operating system and all installed software.
2. **Minimize installed packages**: Only install necessary software on the host system.
3. **Use secure configurations**: Follow security benchmarks like CIS (Center for Internet Security) for your operating system.

### Application Security

1. **Use HTTPS**: Always use HTTPS in production with a valid SSL certificate.
2. **Implement authentication**: Add authentication if the gateway is not intended for public use.
3. **Configure proper logging**: Enable comprehensive logging and regularly review logs for suspicious activities.
4. **Set appropriate permissions**: Ensure the application runs with the minimum necessary permissions.

### Monitoring and Incident Response

1. **Implement monitoring**: Set up monitoring for unusual traffic patterns or system behavior.
2. **Create an incident response plan**: Have a plan in place for responding to security incidents.
3. **Regular security assessments**: Conduct periodic security assessments of your deployment.

## Security Features

FreePenGateway includes several security features:

1. **HTTPS Redirection**: All HTTP requests are automatically redirected to HTTPS.
2. **HTTP Strict Transport Security (HSTS)**: Ensures browsers always use HTTPS for future requests.
3. **Rate Limiting**: Prevents abuse by limiting the number of requests from a single IP address.
4. **Security Headers**: Implements various security headers to protect against common web vulnerabilities:
   - Content-Security-Policy
   - X-Content-Type-Options
   - X-Frame-Options
   - X-XSS-Protection
   - Referrer-Policy
5. **Forwarded Headers Processing**: Properly handles forwarded headers for accurate client IP detection.

## Known Limitations

- The application does not currently implement authentication or authorization.
- There is no built-in mechanism for IP allowlisting/blocklisting.
- The application does not perform content inspection of proxied traffic.

## Security Updates

Security updates will be announced through:

1. GitHub releases and release notes
2. Updates to the SECURITY.md file
3. Notifications to registered users (if applicable)

## Third-Party Dependencies

FreePenGateway relies on the following major dependencies:

1. **ASP.NET Core**: Microsoft's web framework
2. **YARP**: Microsoft's reverse proxy library

It's recommended to regularly check for updates to these dependencies and apply them promptly.