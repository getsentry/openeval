# XML Reporting

The XML extension provides a standardized way to report the results of Evals to a service which supports the common reporting implementations based on jUnit. It is a superset of information applied on top of the existing XML formats.

The purpose of this specification is to aid existing test infrastructure that may wish to utilize Eval's in a traditional testing manner. For example, within continuous integration to determine if a change should be allowed into mainline. Additionally the reporting mechanism ensures that existing test analytics services can easily adapt to report on Evals, rather than a simple pass/fail mechanism.

## Properties

We follow the `properties` specification to maximize existing compatibility. All Eval-specific properties should be prefixed with `eval:`, such as `eval:score`.

The following properties **must** be made available on `testcase`:

`eval:score` - the aggregate score, of all scorers run for this Eval

The following properties **may** be made available on `testcase`:

- `eval[ScorerName]:score` - the score for `ScorerName`

### Scores

Scores must be a floating point number from `0.0` to `1.0`.

### Scorer Names

Scorer names have the following requirements:

- They **must** be uniquely named and are case-insensitive.
- They **must** contain only alphanumeric characters, periods, hyphens, and underscores.

The following regular expression satisfies the name requirements:

```ts
const re = /^[0-9a-z\.-_]$/i;
re.match(scorerName);
```

## Example

```xml
<?xml version="1.0" encoding="UTF-8"?>
<testsuites time="15.682687">
    <testsuite name="src/evals.test.ts" time="6.605871">
        <testcase name="is never sad about the weather" classname="sentiment" time="2.113871">
            <properties>
                <property name="eval:score" value="0.4" />
                <property name="eval:score[Factuality]" value="0.4" />
            </properties>
            <failure message="An explanation of the scorer(s) responses.">
                <!-- Call stack printed here -->
            </failure>
        </testcase>
    </testsuite>
</testsuites>
```
