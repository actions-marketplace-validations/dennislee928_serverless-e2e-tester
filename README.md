# Serverless E2E Tester (Robot Framework)

在 GitHub Actions 裡執行 Robot Framework API 測試，不需要先安裝 Python、Java 或測試相依套件。

[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## 使用方式

```yaml
name: API E2E Tests

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run Robot Framework Tests
        uses: dennislee928/serverless-e2e-tester@v0.1.0
        with:
          test_dir: tests
          target_url: https://your-api.workers.dev

      - name: Upload Test Reports
        uses: actions/upload-artifact@v4
        if: always()
        with:
          name: robot-framework-reports
          path: reports/
          retention-days: 7
```

GitHub Secrets 會以同名環境變數注入，並在 Robot Framework 裡變成變數。`target_url` 會同時提供 `${BASE_URL}` 與 `${TARGET_URL}`。

```yaml
- name: Run Robot Framework Tests
  uses: dennislee928/serverless-e2e-tester@v0.1.0
  env:
    API_KEY: ${{ secrets.API_KEY }}
    AUTH_TOKEN: ${{ secrets.AUTH_TOKEN }}
  with:
    test_dir: tests
    target_url: https://your-api.workers.dev
```

## Inputs

| Input | 說明 | 必填 | 預設值 |
| --- | --- | --- | --- |
| `test_dir` | 含 `.robot` 測試檔的目錄 | 是 | - |
| `target_url` | API 基礎 URL | 是 | - |
| `report_dir` | 報告輸出目錄 | 否 | `reports` |
| `include_tags` | 要執行的標籤，逗號分隔 | 否 | - |
| `exclude_tags` | 要排除的標籤，逗號分隔 | 否 | - |
| `variable_file` | Robot Framework 變數檔，多個路徑用逗號分隔 | 否 | - |
| `processes` | 平行行程數，`1` 為循序執行 | 否 | `1` |
| `timeout` | 測試逾時，例如 `5 minutes` | 否 | - |
| `log_level` | `TRACE`、`DEBUG`、`INFO`、`WARN`、`ERROR`、`NONE` | 否 | `INFO` |
| `artifact_name` | 報告 artifact 名稱 | 否 | `robot-framework-reports` |
| `artifact_retention_days` | artifact 保留天數 | 否 | `7` |

## Outputs

| Output | 說明 |
| --- | --- |
| `exit_code` | Robot Framework 結束碼，`0` 代表通過 |
| `report_path` | 報告目錄 |

## 授權

MIT
