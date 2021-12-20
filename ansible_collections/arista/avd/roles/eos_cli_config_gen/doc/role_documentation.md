# arista.avd.eos_cli_config_gen

## Data Model

### test_key

Some test to show the docs

```yaml
test_key: 
  # test_bool | Required
  test_bool: <bool>
  # test_string | Optional
  test_string: <str>
  # test_list_of_dicts | Optional
  test_list_of_dicts: 
      # element_test_key | Optional
    - element_test_key: <str>
  # test_list_of_strings | Optional
  test_list_of_strings: 
    - <str, options: [option1 | option2]>
```
<details>
<summary>Example</summary>

```yaml
test_key:
  test_bool: true
  test_list_of_dicts:
  -   element_test_key: list item dict key string
  test_list_of_strings:
  - option1
  - option2
  test_string: some test string
```
</details>
<details>
<summary>Schema</summary>

```yaml
test_key:
  description: Some test to show the docs
  example:
      test_bool: true
      test_list_of_dicts:
      -   element_test_key: list item dict key string
      test_list_of_strings:
      - option1
      - option2
      test_string: some test string
  options:
      test_bool:
          required: true
          type: bool
      test_list_of_dicts:
          elements: dict
          options:
              element_test_key:
                  type: str
          type: list
      test_list_of_strings:
          choices:
          - option1
          - option2
          elements: str
          type: str
      test_string:
          type: str
  type: dict
```
</details>
