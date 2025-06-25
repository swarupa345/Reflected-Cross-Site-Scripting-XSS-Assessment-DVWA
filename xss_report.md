# Reflected XSS Vulnerability Report - DVWA

## Target
DVWA - Reflected XSS module (Security Level: Low)

## Vulnerability Summary
The application fails to properly sanitize user input and reflects it back to the browser, resulting in an XSS vulnerability.

## Steps to Reproduce

1. Go to DVWA → Reflected XSS.
2. Enter the payload:
   ```html
   <script>alert('XSS')</script>
