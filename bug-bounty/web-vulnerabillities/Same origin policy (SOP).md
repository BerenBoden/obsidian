##### ***What is SOP?***
Same origin policy is a security mechanism to ensure that information existing on a domain is restricted from being accessed by a different domain.

Examples of origin comparisons with the URL `http://example.com/page.html`:
|URL|Outcome|Reason|
|`http://example.com/other.html`|Same origin|Only the path differs|
|`http://example.com/inner/another.html`|Same origin|Only the path differs|
|`https://example.com/page.html`|Failure|Different protocol|
|`http://example.com:81/dir/page.html`|Failure|Different port|
|`http://sub.company.com/dir/page.html`|Failure|Different host|
##### ***Resources***
https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy
https://portswigger.net/web-security/cors/same-origin-policy