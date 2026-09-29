# X-Forwarded-For Origin Access-Control Bypass

## Summary

During authorized security testing of `go-cms.tw.coupang.com`, I identified an access-control behavior where a client-controlled `X-Forwarded-For` header influenced whether the request could reach the origin application.

A baseline request returned `403 Access Denied`. When the same endpoint was tested with the observed `X-Forwarded-For` value, the response changed to `200 OK` and returned an origin application response.

## Target

`go-cms.tw.coupang.com`

## Vulnerability

**Client-Controlled `X-Forwarded-For` Trust / Access-Control Bypass**

The observed behavior indicates that the perimeter access-control mechanism relied on a forwarded client IP value that could be influenced by the requester.

---

## Methodology

### 1. Baseline Request

A normal request was sent to the target without an `X-Forwarded-For` header.

The response was:

```http
HTTP/1.1 403 Forbidden
```

The response indicated that access was denied by the observed perimeter layer.

![Baseline 403](screenshots/01-control-403.png)

---

### 2. `X-Forwarded-For` Test

The request was repeated with the tested forwarded IP value:

```http
X-Forwarded-For: 61.216.1.1
```

The response changed to:

```http
HTTP/1.1 200 OK
```

The response also returned:

```text
CMS-WEB-API: Hello world!
```

![X-Forwarded-For 200](screenshots/02-xff-200-origin.png)

This demonstrated that the supplied forwarded IP value influenced the observed access-control behavior and allowed the request to reach an origin application response that was not returned by the baseline request.

---

### 3. Request Metadata

The recorded test metadata contains the target host, request path, tested `X-Forwarded-For` value, and resulting HTTP status.

![Request Metadata](screenshots/03-xff-metadata.png)

---

## Request Flow

### Baseline

```text
Client
  |
  | GET /
  |
  v
Perimeter / Edge
  |
  | Access denied
  v
403 Forbidden
```

### With `X-Forwarded-For`

```text
Client
  |
  | GET /
  | X-Forwarded-For: 61.216.1.1
  |
  v
Perimeter / Edge
  |
  | Request accepted
  v
Origin Application
  |
  v
200 OK
```

---

## Impact

The finding demonstrates that a client-controlled forwarded IP value could influence the observed perimeter access-control decision.

The practical impact demonstrated during testing was **unauthorized origin reachability** through the observed access-control restriction.

The testing did **not** demonstrate:

* Authentication bypass
* Account takeover
* Access to customer data
* Credential disclosure
* Token disclosure
* Administrative access
* Data modification
* Remote Code Execution

The impact documented here is therefore limited to the access-control and origin-reachability behavior that was actually demonstrated.

---

## Root Cause

The observed behavior is consistent with an access-control mechanism trusting a client-influenced `X-Forwarded-For` value when determining whether a request should be allowed.

`X-Forwarded-For` is commonly used in reverse-proxy and load-balancer architectures, but it should not be treated as trustworthy solely because it is present in an HTTP request.

---

## Remediation

The infrastructure should ensure that forwarded client IP information used for access-control decisions can only originate from trusted proxy infrastructure.

Recommended controls:

1. Strip or overwrite client-supplied forwarding headers at the trusted proxy boundary.
2. Only trust `X-Forwarded-For` values added by known and trusted proxies.
3. Ensure origin access-control rules use a trustworthy source of client identity.
4. Prevent direct or unintended origin exposure where possible.
5. Re-test the access-control behavior after remediation.

---

## Security Testing Notes

Testing was limited to authorized security validation.

No destructive actions were performed, and no unnecessary access to sensitive information was attempted.

The finding is documented according to the behavior that was observed during testing, without claiming additional impact that was not demonstrated.

