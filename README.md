**Vulristics** (from “Vulnerability” and “Heuristics”) is an extensible framework for analyzing publicly available information about vulnerabilities. It started as a Microsoft Patch Tuesday reporting tool. Now you can use it to analyse (classify, prioritize) any set of CVEs using the data from Vulners.com, Microsoft website, NVD and AttackerKB.

![vulristics logo](https://github.com/leonov-av/vulristics/blob/master/logo/vulristics_line.png)

## Why is this needed?
Let's say we have a vulnerability ID (CVE ID) and we need to decide whether it is really critical or not. We will probably go to some vulnerability databases (NVD, BDU, CVE page on the Microsoft website, Vulners.com, etc.) and somehow analyze the descriptions and parameters. Right? Such analysis can be quite complex and not so obvious. My idea is to formalize it and make it shareable. It may not be the most efficient way to process data, but it should reflect real human experience, the things that real vulnerability analysts do. This is the main goal.

## What's ready right now?
You can generate the report for the following entities:

* Arbitrary CVE list
```buildoutcfg
python3 vulristics.py --report-type "cve_list" --cve-project-name "New Project" --cve-list-path "analyze_cve_list.txt" --cve-comments-path "analyze_cve_comments.txt" --cve-data-sources "ms,nvd,bdu,epss,vulners,attackerkb" --rewrite-flag "True"
```
* Microsoft Patch Tuesday ([HTML report example](https://avleonov.com/vulristics_reports/ms_patch_tuesday_november2021_report_with_comments_ext_img.html))
```buildoutcfg
python3 vulristics.py --report-type "ms_patch_tuesday" --mspt-year 2021 --mspt-month "November" --rewrite-flag "True"
```

### Example of output
```buildoutcfg
$ python3 vulristics.py --report-type "cve_list" --cve-project-name "CVE-2024-20723" --cve-list-path "analyze_cve_list.txt" --cve-comments-path "analyze_cve_comments.txt" --cve-data-sources "ms,nvd,bdu,epss,vulners,attackerkb" --rewrite-flag "True"
                      /$$           /$$             /$$     /$$                    
                     | $$          |__/            | $$    |__/                    
 /$$    /$$ /$$   /$$| $$  /$$$$$$  /$$  /$$$$$$$ /$$$$$$   /$$  /$$$$$$$  /$$$$$$$
|  $$  /$$/| $$  | $$| $$ /$$__  $$| $$ /$$_____/|_  $$_/  | $$ /$$_____/ /$$_____/
 \  $$/$$/ | $$  | $$| $$| $$  \__/| $$|  $$$$$$   | $$    | $$| $$      |  $$$$$$ 
  \  $$$/  | $$  | $$| $$| $$      | $$ \____  $$  | $$ /$$| $$| $$       \____  $$
   \  $/   |  $$$$$$/| $$| $$      | $$ /$$$$$$$/  |  $$$$/| $$|  $$$$$$$ /$$$$$$$/
    \_/     \______/ |__/|__/      |__/|_______/    \___/  |__/ \_______/|_______/ 
Reading existing profile data/profiles/CVE-2024-20723_profile.json...
Exclude CVEs: 0
No specified products to analyze set in profile, reporting everything
All CVEs: 1
Enabled data sources: ['ms', 'nvd', 'bdu', 'epss', 'vulners', 'attackerkb']
Counting CVE scores...
Collecting MS CVE data...
1/1 - CVE-2024-20723
Requesting CVE-2024-20723 from Microsoft website
Collecting NVD CVE data...
1/1 - CVE-2024-20723
Requesting CVE-2024-20723 from NVD website WITH authorization key
Collecting EPSS CVE data...
1/1 - CVE-2024-20723
Requesting CVE-2024-20723 from epss website
Collecting AttackerKB CVE data...
1/1 - CVE-2024-20723
Requesting CVE-2024-20723 from AttackerKB website WITHOUT authorization key
Collecting Vulners CVE data...
1/1 - CVE-2024-20723
Requesting CVE-2024-20723 from Vulners website WITH authorization key
Collecting BDU CVE data...
Updating BDU FSTEC data...
1/1 - CVE-2024-20723
Combining CVE data...
1/1 CVE-2024-20723
Counting CVE scores...
Making vulnerability reports for each reports config...
Report config: with_comments_ext_img
HTML report generated: reports/cve-2024-20723_report_with_comments_ext_img.html
```

### Options
```buildoutcfg
$ python3 vulristics.py -h
usage: vulristics.py [-h] [--report-type REPORT_TYPE] [--mspt-year MSPT_YEAR] [--mspt-month MSPT_MONTH]
                     [--mspt-comments-links-path MSPT_COMMENTS_LINKS_PATH] [--cve-project-name CVE_PROJECT_NAME]
                     [--cve-list-path CVE_LIST_PATH] [--cve-comments-path CVE_COMMENTS_PATH]
                     [--cve-data-sources CVE_DATA_SOURCES] [--profile-json-path PROFILE_JSON_PATH]
                     [--result-formats RESULT_FORMATS] [--result-html-path RESULT_HTML_PATH]
                     [--result-json-path RESULT_JSON_PATH] [--rewrite-flag REWRITE_FLAG]
                     [--vulners-use-github-exploits-flag VULNERS_USE_GITHUB_EXPLOITS_FLAG]

An extensible framework for analyzing publicly available information about vulnerabilities

options:
  -h, --help            show this help message and exit
  --report-type REPORT_TYPE
                        Report type (ms_patch_tuesday, ms_patch_tuesday_extended, cve_list or custom_profile)
  --mspt-year MSPT_YEAR
                        Microsoft Patch Tuesday year
  --mspt-month MSPT_MONTH
                        Microsoft Patch Tuesday month
  --mspt-comments-links-path MSPT_COMMENTS_LINKS_PATH
                        Microsoft Patch Tuesday comments links file
  --cve-project-name CVE_PROJECT_NAME
                        Name of the CVE Project
  --cve-list-path CVE_LIST_PATH
                        Path to the list of CVE IDs
  --cve-comments-path CVE_COMMENTS_PATH
                        Path to the CVE comments file
  --cve-data-sources CVE_DATA_SOURCES
                        Data sources for analysis, e.g. "ms,nvd,bdu,epss,vulners,attackerkb,bdu,custom"
  --profile-json-path PROFILE_JSON_PATH
                        Custom profile for analysis
  --result-formats RESULT_FORMATS
                        Result formats, e.g. "html,json", Default - "html"
  --result-html-path RESULT_HTML_PATH
                        Path to the results file in html format (Default - will be created in reports directory)
  --result-json-path RESULT_JSON_PATH
                        Path to the results file in json format
  --rewrite-flag REWRITE_FLAG
                        Rewrite Flag (True/False, Default - False)
  --vulners-use-github-exploits-flag VULNERS_USE_GITHUB_EXPLOITS_FLAG
                        Use Vulners Github exploits data Flag (True/False, Default - True)
```

### Installation
```buildoutcfg
pip3 install -r requirements.txt
```

## Where to read more?
* My posts about Vulristics in [avleonov.com blog](https://avleonov.com/category/projects/vulristics/)
* My videos about Vulristics (and vulnerabilities) in [AVLEONOV Media YouTube Channel](https://www.youtube.com/playlist?list=PL2Viq8X7eAaZVQsVG1lcFoEOUr2wRpoha)

If you have good ideas please share them in the [AVLEONOV Chat](https://t.me/avleonovchat). 
The help in coding will be also pretty much appreciated! 😉

## Some obvious warnings (!!)
* This tool is NOT an interface to any particular database.
* The tool makes requests to third-party sources.

So keep in mind that if you actively use it for bulk operations, you may have problems with the owners of these third-party sources, for example, your IP address will simply be banned. So be careful and reasonable!
