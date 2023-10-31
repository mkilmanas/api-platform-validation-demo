This is a demo to reproduce API Platform issue [#5912](https://github.com/api-platform/core/issues/5912)

## Setup

```shell
composer install
```

## Reproduce

### Run

```shell
curl -X 'POST' \
  'http://localhost/api/dummies' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{"id": "foorbar", "title": null}'
```

### Expected result

```json
{
    "status": 422,
    "violations": [
        {
            "propertyPath": "title",
            "message": "This value should not be blank.",
            "code": "c1051bb4-d103-4f74-8988-acbcafc7fdc3"
        }
    ],
    "detail": "title: This value should not be blank.",
    "type": "\\/validation_errors\\/c1051bb4-d103-4f74-8988-acbcafc7fdc3",
    "title": "An error occurred"
}
```

### Actual Result

```json
{
    "status": 422,
    "violations": {
        "type": "https:\\/\\/symfony.com\\/errors\\/validation",
        "title": "Validation Failed",
        "detail": "title: This value should not be blank.",
        "violations": [
            {
                "propertyPath": "title",
                "title": "This value should not be blank.",
                "template": "This value should not be blank.",
                "parameters": {
                    "{{ value }}": "null"
                },
                "type": "urn:uuid:c1051bb4-d103-4f74-8988-acbcafc7fdc3"
            }
        ]
    },
    "detail": "title: This value should not be blank.",
    "type": "\\/validation_errors\\/c1051bb4-d103-4f74-8988-acbcafc7fdc3",
    "title": "An error occurred"
}
```

Note the top level `violations` being an object rather than array, and the actual `violations` array nested within it.

Note that this is specific to `Accept: application/json` format
