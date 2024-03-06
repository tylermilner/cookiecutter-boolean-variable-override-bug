# cookiecutter-boolean-variable-override-bug

A simple [Cookiecutter](https://github.com/cookiecutter/cookiecutter) example that showcases [a bug in Cookiecutter 2.6.0](https://github.com/cookiecutter/cookiecutter/issues/1973) where [boolean configuration variables](https://cookiecutter.readthedocs.io/en/latest/advanced/boolean_variables.html) are unable to be overridden properly.

## Steps to Reproduce

After installing Cookiecutter, see the repro steps below:

1. Clone and navigate to repo:

```Shell
git clone https://github.com/tylermilner/cookiecutter-boolean-variable-override-bug.git && cd cookiecutter-boolean-variable-override-bug
```

2. Run Cookiecutter with the `--no-input` flag:

```Shell
cookiecutter . --no-input
```

3. Inspect the contents of `test_boolean/README.md`:

```Shell
cat test_boolean/README.md
```

Notice that "Hello, World! is output, as expected (since `true` is the default value for `generate_text` in `cookiecutter.json`):

```text

Hello, world!

```

4. Run Cookiecutter again, using the `-f` flag to overwrite the previous run and setting the `generate_text` to `false`:

```Shell
cookiecutter . --no-input -f generate_text=false
```

5. Inspect the contents of `test_boolean/README.md` once again:

```Shell
cat test_boolean/README.md
```

Notice that "Hello, World! is still output, which is **NOT** expected:

```text

Hello, world!

```

Inspecting the `README.md` file in the `{{ cookiecutter.project_name }}` template directory, you can see that it's setup to only output the "Hello, world!" text if `generate_text` is `true`:

```Shell
cat \{\{\ cookiecutter.project_name\ \}\}/README.md
```

```text
{% if cookiecutter.generate_text %}
Hello, world!
{% endif %}
```

If everything was working properly, the `generate_text=false` override _should_ have prevented the text from being generated and resulted in an empty file.
