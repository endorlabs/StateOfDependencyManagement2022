# State of Dependency Management 2022

This repository contains the data set on the basis of which the statistics and
charts in Endor Labs' [2022 State of Dependency
Management](https://www.endorlabs.com/state-of-dependency-management) report
have been created.

Starting point are the packages mentioned in Appendices A-H of [Census
II](https://www.linuxfoundation.org/press/press-release/the-linux-foundation-and-harvards-lab-for-innovation-science-release-census-of-most-widely-used-open-source-application-libraries).
Kudos to The Linux Foundation and Harvard’s Lab for Innovation Science for
having collected, processed and shared this invaluable data set.

Please use and share this information as you see fit. We would be happy to hear
from you about questions, extensions, suggestions or corrections.

# CII Best Practice Program, OpenSSF Scorecard and Criticality (all ecosystems)

The files `Appendix_A.csv` - `Appendix_H.csv` correspond to the [respective
files in the Census II data
set](https://data.world/thelinuxfoundation/census-ii-of-free-and-open-source-software).

The first five columns are from the original files, the others have been added
as follows:
- Columns `lio_...` relate to calls of [Libraries.io](https://libraries.io/) to
  get a package's source code repository (if any) and additional infomation,
- Columns `cii_...` relate to calls of
[Coreinfrastructure.org](https://bestpractices.coreinfrastructure.org) to get a
package's CII Best Practice entry (if any),
- Columns `scorecard_v480_...` relate to the computation of the [OpenSSF
  Security Scorecard](https://github.com/ossf/scorecard) rating (version
  v4.8.0), and
- Columns `criticality_...` relate to the computation of the [OpenSSF Project
  Criticality Score (Beta)](https://github.com/ossf/criticality_score/).  

Additional notes:
- A valid repository URL from Libraries.io is a prerequisite for obtaining and
  computing all the other information, thus, those columns will only be
  populated if the `lio_repo_url` is non-empty and a HTTP response code
  `lio_repo_url_rc` of 200 is received.
- Some packages are comprised in multiple Census II appendices.
  `Appendices_A-H_<count>.csv` is a union of those files with `<count>` unique
  packages from ecosystems represented with more than 10 packages (`maven`,
  `pypi`, `npm`, `go`, `nuget`, `rubygems` and `cargo`).
- Columns marked with asterisk are not further analyzed (yet).

Column Name | Description
--- | ---
`platform` | Ecosystem (from Census II)
`name` | Package name (from Census II)
`version` | Package version (from Census II, only in files `Appendix_E.csv` - `Appendix_H.csv`, `None` otherwise)
`zscore_combined` | Z-score (from Census II)
`openssf_badge_tiered_percentage` | Tiered percentage of CII entry (historical, as reported by Census II)
`appendix` | The first appendix in which a package is mentioned (only in the union file `Appendices_A-H_<count>.csv`)
`lio_timestamp` | Timestamp of the request to Libraries.io
`lio_url` | URL of the project in Libraries.io
`lio_rc` |  Response code
`lio_rank`* | Rank from Libraries.io
`lio_dependents_count`* | Number of dependents from Libraries.io
`lio_dependent_repos_count`* | Number of dependent repositories from Libraries.io
`lio_repo_url` | URL of the source code repository from Libraries.io
`lio_repo_url_rc` | Return code received when calling `lio_repo_url`
`cii_timestamp` | Timestamp of request to bestpractices.coreinfrastructure.org to obtain CII Best Practice entry (if any)
`cii_url` | URL used to obtain CII Best Practice info
`cii_rc` | Return code
`cii_updated_at` | CII entry creation
`cii_created_at` | CII entry last modified
`cii_tiered_percentage` | Tiered percentage of CII entry
`scorecard_v480_timestamp` | Timestamp of OpenSSF scorecard computation
`scorecard_v480_score` | Score
`criticality_timestamp` | Timestamp of OpenSSF criticality computation
`criticality_score` | Score
`criticality_dependents_count`* | Number of dependents

# Dependency Information (Maven only)

The files `maven_...` contain dependency and vulnerability information about the
Maven packages mentioned in Census II appendices B, D, F and H.

This information has been compiled in the week of October 24, 2022, thus, it
does not cover versions or vulnerabilities published thereafter.

## Dependencies and Vulnerabilities

The file `maven_latest_10_versions.csv` contains information about the latest 10
releases of all Maven packages mentioned in Census II (where available,
excluding `alpha`, `beta`, `rc` versions etc.). The respective latest version of
all packages is contained in `maven_latest_version.csv`.

Notes:
- Dependency information about each package is obtained by running `mvn org.apache.maven.plugins:maven-dependency-plugin:3.3.0:tree -DoutputType=graphml` on its `pom.xml`. 
- Release dates were read from Maven Central.
- Vulnerability information for individual packages is read from
  [OSV.dev](https://osv.dev/)
- The number of vulnerabilities is always reported once for all dependency
  scopes (columns `..._with_test`) and once excluding test scope (columns
  `..._wo_test`). 


Column Name | Description
--- | ---
`platform` | Ecosystem (from Census II)
`name` | Package name (from Census II)
`version` | Package version (from Census II, only exists for packages taken from appendices E-H)
`major_version` | The major version identifier of `version`
`release_datetime ` | Release date (from Maven Central)
`release_year` | The release year from `release_datetime`
`deps_count ` | Total number of dependencies (computed with `maven-dependency-plugin`)
`dir_deps_count ` | Number of direct dependencies
`trans_deps_count ` |  Number of transitive dependencies
`max_depth ` | Maximum depth of dependency tree
`own_vulns_count ` | Number of vulnerabilities in package from [OSV.dev](https://osv.dev/)
`dep_vulns_hist_count_with_test ` | Number of vulnerabilities in dependencies where vulnerability disclosure date < package release date (all dependency scopes)
`dep_vulns_hist_count_wo_test ` | Number of vulnerabilities in dependencies where vulnerability disclosure date < package release date (without test scope)
`dep_vulns_count_with_test ` | Number of vulnerabilities in dependencies (all dependency scopes)
`dep_vulns_count_wo_test ` | Number of vulnerabilities in dependencies (without test scope)
`dep_vulns_low_with_test ` | Subset of `dep_vulns_count_with_test` with low priority
`dep_vulns_low_wo_test ` | Subset of `dep_vulns_count_wo_test` with low priority
`dep_vulns_medium_with_test ` | ...
`dep_vulns_medium_wo_test ` | ...
`dep_vulns_high_with_test ` | ...
`dep_vulns_high_wo_test ` | ...
`dep_vulns_critical_with_test ` | ...
`dep_vulns_critical_wo_test ` | ...
`dep_vulns_unknown_with_test ` | ...
`dep_vulns_unknown_wo_test` | ...

## Vulnerable dependencies

The file `maven_dependency_vulns.csv` contains information about vulnerabilities
in dependencies. This information is collected by calling OSV for every node in
the dependency trees of the Census II packages.

Column Name | Description
--- | ---
`platform` | Ecosystem
`source_name` | Maven package from Census II
`source_version` | Version identifier
`source_release_datetime` | Release date
`target_name` | The vulnerable dependency
`target_version` | Version identifier
`target_scope` | The scope of the dependency relationship
`vuln` | OSV vulnerability identifier
`alias` | Alias (mostly CVE identifiers)
`path_length` | Path length from package to vulnerable package (`0` means that the package itself is vulnerable, `1` means the vulnerability is in a direct dependency of the Census II package)
`cvss_version` | CVSS version
`cvss_base_score` | CVSS Base Score
`cvss_base_severity`| CVSS Base Severity

## NVD Vulnerability Disclosure and Fix Release

The file `maven_disclosure_release_delta.csv` contains information about
vulnerability disclosure dates (read from [NVD](https://nvd.nist.gov)'s REST
API) and the patch release dates (read from Maven Central).

For every (`target_name`, `vuln`) exist `fixed_version_count` rows, one for
every fix mentioned by OSV.
[CVE-2020-13956](https://osv.dev/vulnerability/GHSA-7r82-7xv7-xcpj) in some
versions of `org.apache.httpcomponents:httpclient`, for instance, has two fixes
`4.5.13` and `5.0.3`, thus, there are two rows for every vulnerable occurence of
`httpclient` in the dependency trees of the Census II packages. Even though we
only used `closest_fix` and `fix_distance` for the report, it could be
interesting for other analyses to have the number of fixes per vulnerability and
the respective versions.

Column Name | Description
--- | ---
`platform` | Ecosystem
`source_name` | Maven package from Census II
`source_version` | Version identifier
`source_release_datetime` | Release date
`target_name` | The vulnerable dependency
`target_version` | Version identifier
`target_scope` | The scope of the dependency relationship
`vuln` | OSV vulnerability identifier
`alias` | Alias (mostly CVE identifiers)
`published` | Publication date (preferrably from from NVD, from OSV otherwise)
`fixed_version` | Version identifier of the patch (from OSV)
`fixed_version_count` | Indicates how many fixes are available according to OSV (> 1 if multiple major or minor releases are supported in parallel)
`closest_fix` | The patch version with the smallest distance to the version currently used by the source package
`fix_distance` | The distance from current version to the selected patch version (column `closest_fix`) in the format `index:distance`, where `index` denotes the highest element of the semver versioning scheme requiring a change (0 = major, 1 = minor, 2 = patch, etc.), and distance the delta between the respective current and patch version identifier. Example: The distance between `commons-io:2.6` and `commons-io:2.7` is `1:1` (i.e., the minor has to be incremented by 1). 
`release_date` | Release date of the selected patch version (column `closest_fix`)
`fixed_version_match` | Indicates how the patch version has been found (`exact-match` if the version identifier from OSV is identical the one in Maven Central, `starts-with` if the version identifier in Maven Central starts with the one provided in OSV, and `no-match` if no version can be found in Maven Central)
`fixed_version_found`| Only populated if previous column is `starts-with`

## License information of source data

The data set aggregates and remixes data obtained from the following sources:
- Data aggregated by OSV is [available under different Creative Commons (CC) licenses](https://github.com/google/osv.dev#current-data-sources)
- Data from Libraries.io, authored by Tidelift, is [available](https://libraries.io/data) under CC BY-SA 4.0
- Data from Maven Central has been obtained following their [terms of use](https://central.sonatype.org/terms.html)
