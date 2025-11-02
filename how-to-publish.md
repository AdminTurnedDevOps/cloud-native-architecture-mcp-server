Want to contribute to this repo and push a new version of the MCP Server to PyPi?

```
pip install build twine
```

```
python -m build
```

You'll need an account to PyPi test as it'll ask you for an API token. You can sign up for free here: https://test.pypi.org/account/register/

**Generate an API token:**
1. Log in to https://test.pypi.org
2. Go to your account settings: https://test.pypi.org/manage/account/
3. Scroll down to "API tokens"
4. Click "Add API token"
5. Give it a name (e.g., "cloud-native-architecture-mcp")
6. Set scope to "Entire account" (for first upload) or specific project (after first upload)
7. Click "Add token"

```
twine upload --repository testpypi dist/*
```

```
twine upload dist/*
```