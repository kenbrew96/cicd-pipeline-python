## PROJECT 3: CI/CD Pipeline with GitHub Actions

### 📁 GitHub Structure
```
cicd-pipeline-python/
├── app.py
├── requirements.txt
├── .github/workflows/deploy.yml
```

### 📄 `app.py`
```python
def handler(event, context):
    return {
        'statusCode': 200,
        'body': 'Hello from Lambda via GitHub Actions!'
    }
```
**Explanation**: A simple Lambda-compatible Python function.

### 📄 `requirements.txt`
```
# No dependencies for this example. Add boto3 or others if needed.
```
**Explanation**: Lists dependencies (leave blank if not required).

### 📄 `deploy.yml`
```yaml
name: Deploy to AWS Lambda
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Setup Python
        uses: actions/setup-python@v2
        with:
          python-version: '3.9'
      - run: pip install -r requirements.txt
      - run: zip -r app.zip app.py
      - run: aws lambda update-function-code --function-name my-lambda --zip-file fileb://app.zip
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          AWS_REGION: ${{ secrets.AWS_REGION }}
```
**Explanation**: Automates deployment of Python Lambda app when code is pushed to GitHub.
