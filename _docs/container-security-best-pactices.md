## Container Security Best Practices

1. Image Vulnerability Scanning

Image Vulnerability Scanning is the process of inspecting Docker container images to detect known security vulnerabilities (CVEs — Common Vulnerabilities and Exposures) in:

- Base OS packages (e.g., Ubuntu, Alpine)

- Application dependencies (e.g., Node.js, Python, Java packages)

- Open-source libraries

- System tools included in the image

In this project let us intergrate the popular well known container image scanner tool called `Snyk' using Github actions.

1. Create an account at [snyk.io](https://snyk.io).

2. Click [Snyk account settings](https://app.snyk.io/account).

3. Under **Auth Token**, copy your API key.

4. Go to your GitHub repository.

5. Navigate to **Settings** → **Secrets and variables** → **Actions**.

6. Click **New repository secret**.

7. Enter the following details:
- Name: SNYK_TOKEN
- Secret: Paste the API token you copied

8. Under root directory, create `.github/workflows/snyk-docker-scan.yml` file.

9. Add the following snyk configuration in snyk-docker-scan.yml:
```
name: Snyk Docker Scan

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
  workflow_dispatch:

jobs:
  snyk-docker-scan:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Snyk CLI
        uses: snyk/actions/setup@v3

      - name: Authenticate Snyk
        run: snyk auth ${{ secrets.SNYK_TOKEN }}

      - name: Test backend-flask Dockerfile
        run: snyk container test backend-flask/Dockerfile --file=backend-flask/Dockerfile

      - name: Test frontend-react-js Dockerfile
        run: snyk container test frontend-react-js/Dockerfile --file=frontend-react-js/Dockerfile
```






