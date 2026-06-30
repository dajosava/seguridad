# React + Vite + Supabase Security Checklist

> **Version:** 1.0
>
> **Framework:** React + Vite + Supabase
>
> **Based on:**
>
> - OWASP ASVS
> - OWASP Top 10
> - Supabase Security Best Practices
> - Industry best practices from Google, Microsoft, Stripe, GitHub, Cloudflare, AWS, and GitHub Security.

---

# Objective

This checklist is intended to ensure that a **React + Vite + Supabase** application is production-ready and follows modern security best practices.

---

# ✅ 1. Authentication

## Users

- [ ] Use **Supabase Auth** exclusively.
- [ ] Never implement a custom authentication system.
- [ ] Require email verification.
- [ ] Enable secure password recovery.
- [ ] Enforce strong password policies.
- [ ] Limit failed login attempts.
- [ ] Expire inactive sessions.
- [ ] Implement secure logout.
- [ ] Revoke compromised refresh tokens.

## Multi-Factor Authentication (MFA)

- [ ] Support MFA (TOTP).
- [ ] Require MFA for administrator accounts.

---

# ✅ 2. Authorization

> **Never trust the React frontend.**

All authorization must be enforced in Supabase using Row Level Security (RLS).

- [ ] Enable RLS on every table.
- [ ] No public tables.
- [ ] Never use policies such as:

```sql
USING (true)
```

- [ ] Separate policies for:
  - [ ] SELECT
  - [ ] INSERT
  - [ ] UPDATE
  - [ ] DELETE
- [ ] Validate ownership using:

```sql
auth.uid()
```

- [ ] Validate user roles.
- [ ] Validate permissions.
- [ ] Prevent privilege escalation.

---

# ✅ 3. Database Security

- [ ] Row Level Security enabled.
- [ ] Audit logging enabled.
- [ ] Automatic backups configured.
- [ ] Proper database indexes.
- [ ] Foreign Keys.
- [ ] Constraints.
- [ ] UUID primary keys.
- [ ] Never expose sequential IDs publicly.

---

# ✅ 4. Secrets Management

> Never store secrets in React.

- [ ] Service Role Key only on backend.
- [ ] Never commit `.env` files.
- [ ] Rotate secrets regularly.
- [ ] Store sensitive variables only on the server.
- [ ] Never expose private API Keys.

---

# ✅ 5. Environment Variables

- [ ] `.env.local`
- [ ] `.env.production`
- [ ] `.gitignore`
- [ ] Never commit secrets.
- [ ] Scan Git history for leaked secrets.

---

# ✅ 6. HTTPS

- [ ] HTTPS only.
- [ ] TLS 1.2 or newer.
- [ ] Valid SSL certificates.
- [ ] Force HTTP → HTTPS redirects.
- [ ] Enable HSTS.

---

# ✅ 7. HTTP Security Headers

Configure:

- [ ] Content-Security-Policy
- [ ] X-Frame-Options
- [ ] X-Content-Type-Options
- [ ] Referrer-Policy
- [ ] Permissions-Policy
- [ ] Strict-Transport-Security

---

# ✅ 8. Content Security Policy (CSP)

Configure at least:

- [ ] default-src
- [ ] script-src
- [ ] img-src
- [ ] connect-src
- [ ] frame-src
- [ ] style-src
- [ ] object-src 'none'

---

# ✅ 9. Cross-Site Scripting (XSS)

- [ ] Never use `dangerouslySetInnerHTML`.
- [ ] Escape user-generated HTML.
- [ ] Sanitize HTML using a trusted library if necessary.
- [ ] Validate all user input.

---

# ✅ 10. SQL Injection

Even though Supabase mitigates most SQL injection risks:

- [ ] Never build SQL manually.
- [ ] Use official Supabase APIs.
- [ ] Parameterize queries.
- [ ] Validate input.

---

# ✅ 11. File Upload Security

- [ ] Validate MIME type.
- [ ] Validate file extension.
- [ ] Validate file size.
- [ ] Scan uploads for malware when applicable.
- [ ] Rename uploaded files.
- [ ] Prevent execution of uploaded files.

---

# ✅ 12. Supabase Storage

- [ ] Private buckets by default.
- [ ] Signed URLs for temporary access.
- [ ] RLS policies for Storage.
- [ ] Validate file ownership.

---

# ✅ 13. API Security

- [ ] Validate every request.
- [ ] Implement rate limiting.
- [ ] Configure request timeouts.
- [ ] Centralized logging.
- [ ] Standardized error handling.
- [ ] Never expose stack traces.
- [ ] Never expose internal implementation details.

---

# ✅ 14. React Security

- [ ] Avoid storing tokens in LocalStorage whenever possible.
- [ ] Keep dependencies updated.
- [ ] Remove dead code.
- [ ] Use Lazy Loading.
- [ ] Use Code Splitting.
- [ ] Never expose sensitive information in frontend bundles.

---

# ✅ 15. CORS

- [ ] Allow only trusted origins.
- [ ] Never use `*` in production.
- [ ] Allow only required HTTP methods.
- [ ] Restrict allowed headers.

---

# ✅ 16. Logging

Never log:

- [ ] Passwords.
- [ ] Full JWT tokens.
- [ ] Secrets.
- [ ] Credit card information.
- [ ] Sensitive personal information.

---

# ✅ 17. Dependencies

Review weekly:

- [ ] Update dependencies.
- [ ] Review CVEs.
- [ ] Remove abandoned libraries.
- [ ] Run:

```bash
npm audit
```

---

# ✅ 18. GitHub Security

- [ ] Secret Scanning enabled.
- [ ] Dependabot enabled.
- [ ] CodeQL enabled.
- [ ] Branch Protection Rules.
- [ ] Mandatory Pull Requests.

---

# ✅ 19. CI/CD Security

Before every deployment:

- [ ] Run Unit Tests.
- [ ] Run Integration Tests.
- [ ] Run Security Scans.
- [ ] Scan dependencies.
- [ ] Run ESLint.
- [ ] Validate TypeScript types.

---

# ✅ 20. Observability

- [ ] Centralized logs.
- [ ] Monitoring.
- [ ] Alerting.
- [ ] Audit trails.
- [ ] Distributed tracing.

---

# ✅ 21. Bot Protection

- [ ] CAPTCHA (or equivalent) on critical forms.
- [ ] Rate limiting by IP/User.
- [ ] Temporary lockout after repeated failed attempts.

---

# ✅ 22. Browser Security

- [ ] Secure cookies.
- [ ] SameSite=Lax or Strict.
- [ ] HttpOnly cookies (server-managed sessions).
- [ ] Clickjacking protection.
- [ ] MIME Sniffing protection.

---

# ✅ 23. Secure UX

- [ ] Generic error messages.
- [ ] Never reveal whether a user exists.
- [ ] Confirmation dialogs for destructive actions.
- [ ] Re-authenticate before sensitive operations.

---

# ✅ 24. Backup Strategy

- [ ] Automatic backups.
- [ ] Periodic restore testing.
- [ ] Backup retention policy.
- [ ] Disaster recovery documentation.

---

# ✅ 25. Penetration Testing

Before production release:

- [ ] Run OWASP ZAP.
- [ ] Check Mozilla Observatory.
- [ ] Check SecurityHeaders.
- [ ] Check SSL Labs.
- [ ] Fix all High and Critical findings.
- [ ] Manually review Authentication, Authorization, and RLS.

---

# ⭐ Enterprise Security

For enterprise-grade applications, implement:

- [ ] Zero Trust Architecture.
- [ ] Principle of Least Privilege.
- [ ] Complete audit trail.
- [ ] Centralized Secret Management.
- [ ] Web Application Firewall (WAF).
- [ ] DDoS Protection.
- [ ] Automatic vulnerability scanning in CI/CD.
- [ ] Periodic permission reviews.
- [ ] Responsible Vulnerability Disclosure Policy.
- [ ] Disaster Recovery Testing.
- [ ] Asset Inventory.
- [ ] Real-time Security Monitoring.
- [ ] Security Incident Response Plan.
- [ ] Infrastructure as Code security validation.
- [ ] Container image vulnerability scanning.
- [ ] Runtime security monitoring.
- [ ] Security dashboards.
- [ ] Compliance monitoring.
- [ ] Regular penetration testing.
- [ ] Third-party dependency governance.

---

# 🏆 Security Maturity Goal

Completing this checklist aligns your application with:

- ✅ OWASP ASVS Level 2
- ✅ OWASP Top 10
- ✅ Supabase Security Best Practices
- ✅ Modern React Security Practices
- ✅ Secure CI/CD Pipelines
- ✅ GitHub Security Best Practices

For financial, healthcare, government, or mission-critical systems, consider progressing toward **OWASP ASVS Level 3**, regular third-party penetration tests, continuous security monitoring, and formal compliance frameworks such as SOC 2, ISO 27001, or PCI DSS where applicable.

---

## License

MIT License
