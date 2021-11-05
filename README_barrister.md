# Keeping up to date

As new releases of the mainline `Microsoft.AspNetCore.OData` package are available in github, it can be updated from the main page of this fork. It will indicate at the top of the main repo page if the fork is up to date or not:

```
This branch is up to date with OData:master.
```

The branch containing the aggregation fix is called `ef5` (for EF-core 5.0). The original branch as it existed in PR 2179 (see below in details) is renamed as `ef5-kosinsky` (name of author).

If master is updated, the `ef5` branch will need to be rebased. Steps to do that:

- Find the tagged commit number for the version to use as base (e.g. `7.5.11` is `e7bd92f2aaef6b550637673ea77db02afae8a505`) It should be in the log of master branch.
- run `git checkout ef5` to check out the ef5 branch
- run `git rebase <commit number>`, using the number from above. At least up to `7.5.11` there are no conflicts.
- edit `src/Microsoft.AspNetCore.OData/Microsoft.AspNetCore.OData.csproj` version `<Version>7.5.11-b</Version>` to base it on the new version.
- verify that you have the environment variables `github_user` and `github_token` (you might be able to use `cp-auto` with its nuget token, it's in keepass)
- switch to the directory `src/Microsoft.AspNetCore.OData`
- run:
```
dotnet build --configuration Release
dotnet pack --configuration Release
```
- the pack command should output something like:
```
Successfully created package '.../path/to/file.nupkg'.
```
use that file path in the following:
```
dotnet nuget push filePath --source "github"
```
That should load the file into github!

- once everything is done, run a final `git push --force` to push the rebased branch.


# Details
This mirror includes a branch which fixes aggregations with EntityFrameworkCore:
https://github.com/OData/WebApi/pull/2179

Here is at ticket discussing the aggregation problem:
https://github.com/OData/WebApi/issues/2293


7.x repo: (this is us)
Aggregation extensions in OData ASP.NET Core fail when using SQL Server · Issue #2293 · OData/WebApi

However there is a non-merged PR from over a year ago. Tested the 7.4.1 nightly build which includes this PR, and it does work. But it’s a nightly, and it’s old.
webapinetcore - Microsoft.AspNetCore.OData 7.4.1-Nightly202009221831 | MyGet
PoC for EF Core 5 (and may be 3.1) support by kosinsky · Pull Request #2179 · OData/WebApi
