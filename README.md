"codeql-analysis.yml"
![triangle_workflow](https://github.com/Lukasz8181/Lukasz/assets/160709095/c41786c0-2056-45b5-8724-5a47aa80b469)
name: "CodeQL"

on:
  push:
    branches: [main]
    paths:
    - '**.cs'
    - '**.csproj'
  pull_request:
    branches: [main]
    paths:
    - '**.cs'
    - '**.csproj'
  schedule:
    - cron: '0 8 * * 4'

jobs:
  analyze:

    name: analyze
    runs-on: ubuntu-latest

    strategy:
      fail-fast: false
      matrix:
        language: ['csharp']

    steps:
    - name: Checkout repository
      uses: actions/checkout@v3
      with:
        fetch-depth: 2

    - run: git checkout HEAD^2
      if: ${{ github.event_name == 'pull_request' }}

    - name: Initialize CodeQL
      uses: github/codeql-action/init@v1
      with:
        languages: ${{ matrix.language }}

    - name: Autobuild
      uses: github/codeql-action/autobuild@v1

    - name: Perform CodeQL Analysis
      uses: github/codeql-action/anal
