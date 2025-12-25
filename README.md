# Shodan API

[Video Tutorial](https://youtu.be/YvB9cmnktbc?si=RchzHeGbDMbsPUuV)

## Code

```python
# pip install shodan
import shodan
import sys
import os

# Configuration
API_KEY = os.environ["shodan_key"]

# The list of properties we want summary information on
FACETS = [
    "org",
    "domain",
    "port",
    "asn",
    ("country", 3),
]

FACET_TITLES = {
    "org": "Top 5 Organizations",
    "domain": "Top 5 Domains",
    "port": "Top 5 Ports",
    "asn": "Top 5 Autonomous Systems",
    "country": "Top 3 Countries",
}

# Input validation
if len(sys.argv) == 1:
    print(f"Usage: {sys.argv[0]} <search query>")
    sys.exit(1)

try:
    api = shodan.Shodan(API_KEY)

    # Generate a query string out of the command-line arguments
    query = " ".join(sys.argv[1:])

    # Free plan allows count search
    result = api.count(query, facets=FACETS)

    print("Shodan Summary Information")
    print(f"Query: {query}")
    print(f"Total Results: {result['total']}\n")

    for facet in result["facets"]:
        print(FACET_TITLES[facet])
        for term in result["facets"][facet]:
            print(f"{term['value']}: {term['count']}")
        print("")

except Exception as e:
    print(f"Error: {e}")
    sys.exit(1)
```

## Commands
python test2.py apache
python test2.py nginx
