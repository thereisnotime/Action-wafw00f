# GitHub Action: wafw00f

This action runs `wafw00f` to scan a . Recommended use is with a @vX tag to ensure a stable version is used instead of @master. Versions can be found here [tags](https://github.com/thereisnotime/Action-wafw00f/tags) and wafw00f documentation for all parameters [here](https://github.com/enablesecurity/wafw00f/wiki/).

## Examples

### Start detection scan

```yaml
- name: Scan with wafw00f
  uses: thereisnotime/action-wafw00f@master
  with:
    url: "https://blackfox-security.com/vuln.php?id=1"
```

### Weekly scheduled detection scans

```yaml
on:
  schedule:
    - cron: 0 11 * * 1 # Monday at 11 UTC

jobs:
  scan_wafw00f:
    runs-on: ubuntu-latest
    name: SCAN | wafw00f DAST
    steps:
      - name: Scan with wafw00f
        uses: thereisnotime/action-wafw00f@master
        with:
          url: "https://blackfox-security.com"
          additional_args: "-a"
```

### Use latest version in a job

```yaml
scan_wafw00f:
  runs-on: ubuntu-latest
  name: SCAN | wafw00f DAST
  steps:
    - name: Scan with wafw00f
      uses: thereisnotime/action-wafw00f@master
      with:
        url: "https://blackfox-security.com"
        additional_args: "-a"
```

### Use latest version in a job and upload the results back to the repository

```yaml
scan_wafw00f:
  runs-on: ubuntu-latest
  name: SCAN | wafw00f DAST
  steps:
    - name: Prepare environment
      run: |
        echo "START_DATE=$(date +'%d-%m-%YT%H-%M-%S-%Z')" >> $GITHUB_ENV
        echo "START_TIMESTAMP=$(date +'%s')" >> $GITHUB_ENV
        TMP_TARGET="https://blackfox-security.com"
        echo "TARGET=$TMP_TARGET" >> $GITHUB_ENV
    - name: Checkout
      uses: actions/checkout@v3
    - name: Scan with wafw00f
      uses: thereisnotime/action-wafw00f@master
      with:
        url: ${{ env.TARGET }}
        additional_args: "-a"
    - name: Commit and push changes
      uses: EndBug/add-and-commit@v9
      with:
        author_name: GitHub Actions
        author_email: 41898282+github-actions[bot]@users.noreply.github.com
        message: "👷 chore(wafw00f): add report for ${{ env.TARGET }}-${{ env.START_DATE }}-${{ env.START_TIMESTAMP }}"
        add: "./reports/osint/*"
        pull: "-a"
```

## Inputs

### `url`

**Required**. Target URL (e.g. <http://blackfox-security.com/vuln.php?id=1>)

### `additional_args`

Additional arguments to pass to wafw00f. Default is `--batch` to run in non-interactive mode.

## Outputs

### `result`

JSON scan results.

### `resultb64`

JSON scan results, base64 encoded.

## Roadmap

If there is enough time or interest, I will add the following features (feel free to open PRs):

- [ ] Finish support for outputs (e.g. `result` and `resultb64`).
- [ ] Add support for notification via Slack.
- [ ] Add support for notification via Telegram.
- [ ] Add support for notification via email.
- [ ] Add support for notification via custom webhooks.
- [ ] Improve argument handling.# GitHub Action: wafw00f

This action runs `wafw00f` to scan a URL with the given parameters. Recommended use is with a @vX tag to ensure a stable version is used instead of @master. Versions can be found here [tags](https://github.com/thereisnotime/Action-wafw00f/tags) and wafw00f documentation for all parameters [here](https://github.com/wafw00fproject/wafw00f/wiki/Usage).

## Examples

### Start a scan

```yaml
- name: Scan with wafw00f
  uses: thereisnotime/action-wafw00f@master
  with:
    url: "https://blackfox-security.com/vuln.php?id=1"
```

### Weekly scheduled scans

```yaml
on:
  schedule:
    - cron: 0 11 * * 1 # Monday at 11 UTC

jobs:
  scan_wafw00f:
    runs-on: ubuntu-latest
    name: SCAN | wafw00f DAST
    steps:
      - name: Scan with wafw00f
        uses: thereisnotime/action-wafw00f@master
        with:
          url: "https://blackfox-security.com"
          additional_args: "--batch --crawl=2 --random-agent --forms --level=5 --risk=3"
```

### Use latest version in a job

```yaml
scan_wafw00f:
  runs-on: ubuntu-latest
  name: SCAN | wafw00f DAST
  steps:
    - name: Scan with wafw00f
      uses: thereisnotime/action-wafw00f@master
      with:
        url: "https://blackfox-security.com"
        additional_args: "--batch --crawl=2 --random-agent --forms --level=5 --risk=3"
```

### Use latest version in a job and upload the results back to the repository

```yaml
scan_wafw00f:
  runs-on: ubuntu-latest
  name: SCAN | wafw00f DAST
  steps:
    - name: Prepare environment
      run: |
        echo "START_DATE=$(date +'%d-%m-%YT%H-%M-%S-%Z')" >> $GITHUB_ENV
        echo "START_TIMESTAMP=$(date +'%s')" >> $GITHUB_ENV
        TMP_TARGET="https://blackfox-security.com"
        echo "TARGET=$TMP_TARGET" >> $GITHUB_ENV
    - name: Checkout
      uses: actions/checkout@v3
    - name: Scan with wafw00f
      uses: thereisnotime/action-wafw00f@master
      with:
        url: ${{ env.TARGET }}
        additional_args: "--batch --crawl=2 --random-agent --forms --level=5 --risk=3"
    - name: Commit and push changes
      uses: EndBug/add-and-commit@v9
      with:
        author_name: GitHub Actions
        author_email: 41898282+github-actions[bot]@users.noreply.github.com
        message: "👷 chore(wafw00f): add report for ${{ env.TARGET }}-${{ env.START_DATE }}-${{ env.START_TIMESTAMP }}"
        add: "./reports/web/*"
        pull: "--rebase --autostash"
```

## Inputs

### `url`

**Required**. Target URL (e.g. <http://blackfox-security.com/vuln.php?id=1>)

### `additional_args`

Additional arguments to pass to wafw00f. Default is `--batch` to run in non-interactive mode.

## Outputs

### `result`

JSON scan results.

### `resultb64`

JSON scan results, base64 encoded.

## Roadmap

If there is enough time or interest, I will add the following features (feel free to open PRs):

- [ ] Finish support for outputs (e.g. `result` and `resultb64`).
- [ ] Add support for notification via Slack.
- [ ] Add support for notification via Telegram.
- [ ] Add support for notification via email.
- [ ] Add support for notification via custom webhooks.
- [ ] Improve argument handling.
