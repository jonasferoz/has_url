## Overview

| Developed by | Guardrails AI |
| --- | --- |
| Date of development | Feb 15, 2024 |
| Validator type | Format |
| Blog |  |
| License | Apache 2 |
| Input/Output | Output |

## Description
This validator ensures that a generated output contains a url.

## Requirements

* Dependencies: None

# Installation

```bash
guardrails hub install hub://guardrails/has_url
```

# Usage Examples

## Validating string output via Python

In this example, we apply the validator to a string output generated by an LLM.

```python
# Import Guard and Validator
from guardrails.hub import HasUrl
from guardrails import Guard

# Setup Guard
guard = Guard().use(
    HasUrl
)

guard.validate("guardrailsai.com")  # Validator passes
guard.validate("https://not-a-url")  # Validator fails
```

## Validating JSON output via Python

In this example, we apply the validator to a string field of a JSON output generated by an LLM.

```python
from pydantic import BaseModel, Field
from guardrails.hub import HasUrl
from guardrails import Guard


# Initialize Validator
val = HasUrl()

# Create Pydantic BaseModel
class LlmInteraction(BaseModel):
		prompt: str
		response: str = Field(validators=[val])

# Create a Guard to check for valid Pydantic output
guard = Guard.from_pydantic(output_class=LlmInteraction)

# Run LLM output generating JSON through guard
res = guard.parse("""
{
		"prompt": "Can you find the Guardrails docs for me?",
		"response": "Sure! Here's the link to the Guardrails docs: https://guardrailsai.com/docs"
}
""")

print(res.validated_output)
```

# API Reference

**`__init__(self, on_fail="noop")`**
<ul>
Initializes a new instance of the HasUrl class.

**Parameters**
- **`on_fail`** *(str, Callable)*: The policy to enact when a validator fails.  If `str`, must be one of `reask`, `fix`, `filter`, `refrain`, `noop`, `exception` or `fix_reask`. Otherwise, must be a function that is called when the validator fails.
</ul>
<br/>

**`validate(self, value, metadata) → ValidationResult`**
<ul>
Validates the given `value` using the rules defined in this validator, relying on the `metadata` provided to customize the validation process. This method is automatically invoked by `guard.parse(...)`, ensuring the validation logic is applied to the input data.

Note:

1. This method should not be called directly by the user. Instead, invoke `guard.parse(...)` where this method will be called internally for each associated Validator.
2. When invoking `guard.parse(...)`, ensure to pass the appropriate `metadata` dictionary that includes keys and values required by this validator. If `guard` is associated with multiple validators, combine all necessary metadata into a single dictionary.

**Parameters**
- **`value`** *(Any):* The input value to validate.
</ul>
