{
    "title": "cache",
    "linkTitle": "cache",
    "weight": "340"
}<span id="cache"></span>

### cache

#### CFTCAT

**[CACHE =  n
]    **{1..32000}

Size parameter for the cache memory buffer size, containing
the transfers waiting for resources.

Expressed as a number of catalog entries, each entry being 62 bytes
long. A large buffer makes it possible to limit catalog file accesses.

The default value of this parameter is 512.

The cache extends automatically by size blocks corresponding to the
initial value.

#### CFTSSL

**[CACHE =  NO
&#124; YES ]   **

Activate the SSL cache.

[Return to Command index](../../)
