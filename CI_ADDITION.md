## CI/CD Pipeline

### GitHub Actions Workflow

Add the following `.github/workflows/bdd-tests.yml` to enable CI for BDD tests:

```yaml
name: BDD Tests

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: ['3.8', '3.9', '3.10']
        browser: ['chrome', 'firefox']
    
    steps:
    - uses: actions/checkout@v3
    - name: Set up Python ${{ matrix.python-version }}
      uses: actions/setup-python@v4
      with:
        python-version: ${{ matrix.python-version }}
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
    - name: Run BDD tests
      run: |
        make test
    
    - name: Upload test results
      if: always()
      uses: actions/upload-artifact@v3
      with:
        name: test-results-${{ matrix.python-version }}-${{ matrix.browser }}
        path: reports/
```

### Selenium WebDriver Configuration

For CI environments, ensure appropriate WebDriver binaries are available. Use webdriver-manager or similar tools.
