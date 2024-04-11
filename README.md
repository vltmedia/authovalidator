# Flask Autho

Basic Flask Auth0 validator to secure your endpoints.

Pulled from the Auth0 Docs: [Here](https://auth0.com/docs/quickstart/backend/python/interactive)


# Installation

```bash
pip install setup.py
```

## Environment Variables

| Name           | Value                   |
| -------------- | ----------------------- |
| AUTH0_DOMAIN   | yourdomain.us.auth0.com |
| AUTH0_AUDIENCE | https://yourdomainapi   |

# Usage

```python
from flaskauthovalidator.validator import Auth0JWTBearerTokenValidator

require_auth = ResourceProtector()
validator = Auth0JWTBearerTokenValidator(
    os.environ.get("AUTH0_DOMAIN"),
    os.environ.get("AUTH0_AUDIENCE")
)
require_auth.register_token_validator(validator)


APP = Flask(__name__)


@APP.route("/api/public")
def public():
    """No access token required."""
    response = (
        "Hello from a public endpoint! You don't need to be"
        " authenticated to see this."
    )
    return jsonify(message=response)


@APP.route("/api/private")
@require_auth(None)
def private():
    """A valid access token is required."""
    response = (
        "Hello from a private endpoint! You need to be"
        " authenticated to see this."
    )
    return jsonify(message=response)


@APP.route("/api/private-scoped")
@require_auth("read:messages")
def private_scoped():
    """A valid access token and scope are required."""
    response = (
        "Hello from a private endpoint! You need to be"
        " authenticated and have a scope of read:messages to see"
        " this."
    )
    return jsonify(message=response)

if __name__ == "__main__":
    # load_dotenv(find_dotenv())
    APP.run(port=env.get("PORT", 3000))



```
