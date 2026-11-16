# Reading the CRM

Reach for this when you call `sesame api hubspot`.

- A path is HubSpot's v3 CRM API with its query string, quoted: `sesame api hubspot '/crm/v3/objects/<object>?limit=<n>&properties=<a>,<b>'`; it is a GET, since you pass no `-d` and no `-X`. {#get-path}
- One page holds at most 100 records; a list goes on with `after=` set to the response's `paging.next.after`, and ends where `paging` is absent. Page only as far as the item needs. {#paging}
- Ask for the properties you need by name: without `properties`, HubSpot returns a few defaults only, and a property not defined is left out, one defined but empty comes back `null`. {#name-properties}
- One record by a unique property, an email, is a GET too: `/crm/v3/objects/contacts/<email>?idProperty=email`; HubSpot's search endpoint is a POST, which you do not send. {#one-by-id-property}
- A non-2xx status exits non-zero: 401 or 403 is the credential's or its scopes', asked of the foreman; 429 is HubSpot's rate limit, waited out once and retried, never looped; 404 is said as not found. {#status-means}
- Check the body before you hand it in: the field the item names (`results`, `id`, a property) is there, and the first lines pasted show it. {#check-the-body}
