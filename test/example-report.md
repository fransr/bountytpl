---
weakness: privilege escalation
asset: {{asset}}
---

# Subdomain Takeover – {{domain}} pointing to unclaimed bucket in AWS S3

Hi,

I noticed that the following domain: `{{domain}}` was returning the following information:

```
{{lookup}}
```

It looked like this:

{{poc-before}}

...