# bountytpl – template generator cli

### description

This is a project created by [Frans Rosén](https://twitter.com/fransrosen). By using a template similar to the ones for [Template Generator](https://github.com/fransr/template-generator) you can combine it with a JSON to produce a proper report.

The idea is to put this in a pipeline where you get a JSON with the variables, combine it with the template and use the report generated to submit it automatically, for example using [bountyplz](https://github.com/fransr/bountyplz).

bountytpl will be able to validate the variables provided against the template file, to make sure all variables are filled in. It can either return the report using STDOUT or by outputting a file (using `-o <file>`).

<img src="https://github.com/fransr/bountytpl/raw/documentation-files/preview/preview.png" width="700" />

### install

```
brew install jq
brew install gnu-sed

ln -fs "$(pwd)/bountytpl" /usr/local/bin/bountytpl
```

### usage

The following will use one template and one JSON-file and output the result into the output-file.

```
bountytpl <markdown-file> -f <json-file> -o <output-file>
```

`-v` for validate

### howto

Generate the JSON with the corresponding fields inside the template. The template-file uses variables formatted like regular `{{handlebars}}`:

```md
---
weakness: xss reflected
asset: {{asset}}
---

# Subdomain Takeover – {{domain}} pointing to unclaimed bucket in AWS S3

Report description
```

The JSON-file then has corresponding properties named the same way:

```json
{
	"asset": "*.example.com",
	"domain": "mail.example.com",
	"lookup": "$ host xxx\nexample.example aaa.s3.amazonaws.com",
	"poc-before": "<img upload src=\u0022a.png\u0022>"
}
```

When running the command, these will be combined, either output in the STDOUT or by defining an output-file.

```bash
bountytpl test/example-report.md -f test/example-report.json -o test-report.md
```

You can also provide the JSON directly without a JSON-file:

```bash
bountytpl test/example-report.md '{"domain":"mail.example.com"}' -o test-report.md
```

### Errors

It will always need all variables which is contained within the template. If not, you will get the following error(s):

```bash
bountytpl test/example-report.md '{"domain":"mail.example.com"}' -o test-report.md

### asset not found in variables
### lookup not found in variables
### poc-before not found in variables
```

### Validate-only `-v`

Using `-v` you can make sure the proper values exists in the JSON:

```
bountytpl test/example-report.md '{"domain":"mail.example.com","asset":"x","lookup":"y","poc-before":"z"}' -v -o test-report.md && echo "all variables exist"

all variables exist
```