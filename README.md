# get-gitversion
This is a GitHub workflow which calculates the commit version based on Git history.

See [OpenTAP docs](https://doc.opentap.io/Developer%20Guide/Plugin%20Packaging%20and%20Versioning/#versioning) for details about the format.

To use in your workflow, add this job:

```yaml
jobs:
  GetVersion:
    runs-on: ubuntu-20.04
    outputs:
      ShortVersion: ${{ steps.gitversion.outputs.ShortVersion }}
      LongVersion: ${{ steps.gitversion.outputs.LongVersion }}
      GitVersion: ${{ steps.gitversion.outputs.GitVersion }}
      GitVersionPrerelease: ${{ steps.gitversion.outputs.GitVersionPrerelease }}
    steps:
      - name: GitVersion
        id: gitversion
        uses: opentap/get-gitversion@v1.2
```

You can then use the version in a job like this:

```yaml
jobs:
  ...
  Package:
    needs:
      - GetVersion
    steps:
      ...
      - run: tap package create package.xml -o ./MyPlugin.${{needs.GetVersion.outputs.GitVersion}}.TapPackage
```