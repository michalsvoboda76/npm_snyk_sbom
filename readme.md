# snyk sbom --version value is not propagated for NPM

Aim of this repository is to demostrate the issue of the "snyk sbom" tool running over the NPM.

This repository contains minimalistic definition of  NPM package `"dummy-npm"` - see the files `package.json` and `package-lock.json`.

In `package.json`, there is defined version of the `"dummy-npm"` to `0.0.0`:

```json
{
  "name": "dummy-npm",
  "version": "0.0.0",
  ...
}
```

The command `snyk sbom --format=cyclonedx1.4+json --all-projects --version=9.9.9` (note the version argument is specified) generates sbom as its output.

**Expectation behavior of the "snyk sbom" tool** is the value of `metadata.component.version` in the generated sbom will be `9.9.9` as it is specified in the command.

**Actual behavior of the "snyk sbom" tool** is the version is always taken from the  `package.json` file and the version specified in the command is ignored.

```json
{
  ...
  "metadata": {
    "component": {
      "bom-ref": "1-dummy-npm@0.0.0",
      "type": "application",
      "name": "dummy-npm",
      "version": "0.0.0",
      "purl": "pkg:npm/dummy-npm@0.0.0"
    }
  },
  ...
}
```

The same is valid in the case the `package.json` file does not contain version attribute.

```json
{
  ...
  "metadata": {
    "component": {
      "bom-ref": "1-dummy-npm@",
      "type": "application",
      "name": "dummy-npm",
      "purl": "pkg:npm/dummy-npm"
    }
  },
  ...
}
```

I consider this the "snyk sbom" tool behavior as a bug.
