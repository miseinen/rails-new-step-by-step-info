## Rubocop + RSpec workflow

- create file in root directory *.github/workflows/test.yml*

- add env variables (use your ruby project's version)
```
env:
  RUBY_VERSION: 2.7
```

- add test name
```
name: Rails tests
```

- add event triggering tests
```
on: [push,pull_request]
```

now we need to describe jobs

- add rubocop job:
```
jobs:
  rubocop-test:
    name: Rubocop
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: ruby/setup-ruby@v1
        with:
          ruby-version: ${{ env.RUBY_VERSION }}
      - name: Install Rubocop
        run: gem install rubocop-rails
      - name: Check code
        run: rubocop --require rubocop-rails   
```

...✏️to be continued

