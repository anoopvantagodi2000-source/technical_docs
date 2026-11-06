# Linux `find` Command

## 1. Introduction

The `find` command is used to search for files and directories in a specified location.

It can search based on file name, type, size, and modification time.

## 2. Syntax

```bash
find [location] [conditions]
```

- `location` – Directory where the search starts.
- `conditions` – Rules used to find files or directories.

## 3. Common Usage

| Command | Description |
|---|---|
| `find .` | Find all files and directories in the current directory. |
| `find /home` | Search inside the `/home` directory. |
| `find . -name "file.txt"` | Find a file by name. |
| `find . -iname "file.txt"` | Find a file by name (case-insensitive). |
| `find . -name "*.log"` | Find all files with the `.log` extension. |
| `find . -type f` | Find only files. |
| `find . -type d` | Find only directories. |
| `find . -empty` | Find empty files and directories. |

## 4. Search by File Size

| Command | Description |
|---|---|
| `find . -size +10M` | Find files larger than 10 MB. |
| `find . -size -1M` | Find files smaller than 1 MB. |
| `find . -size 5M` | Find files approximately 5 MB in size. |

## 5. Search by Modification Time

| Command | Description |
|---|---|
| `find . -mtime -1` | Find items modified within the last 24 hours. |
| `find . -mtime +7` | Find items modified more than 7 days ago. |
| `find . -mmin -30` | Find items modified within the last 30 minutes. |

## 6. Important Options

| Option | Description |
|---|---|
| `-name` | Search by name. |
| `-iname` | Search by name without case sensitivity. |
| `-type f` | Search for files. |
| `-type d` | Search for directories. |
| `-size` | Search by file size. |
| `-mtime` | Search by modification time in days. |
| `-mmin` | Search by modification time in minutes. |
| `-empty` | Search for empty files or directories. |

## 7. Key Points

- `.` represents the current directory.
- `/` represents the root directory.
- `*` is used as a wildcard.
- `find` searches recursively through subdirectories.
- `-type f` is used for files.
- `-type d` is used for directories.


## Examples
1. -name (finds exact matches directory and file)
```bash
find . -name "python"
./assignement/.venv/bin/python
./webcrawling/.venv/bin/python
./python_projects/ipl/.venv/bin/python
./python_projects/company-master-project/.venv/bin/python
./python_projects/population_analysis_project/.venv/bin/python
./python_projects/unit_testing_project/.venv/bin/python
./python_projects/space_invader_game/.venv/bin/python
./PycharmProjects/WelcomeScreen/.venv/bin/python
./python_basics/.venv/bin/python

```
2 -iname (finds exact matches directory and files of given name by ignoring text cases)
```bash
find -iname "src"
./python_projects/ipl/.venv/lib/python3.14/site-packages/numpy-2.5.4.dist-info/licenses/numpy/random/src
./python_projects/ipl/.venv/lib/python3.14/site-packages/numpy-2.5.4.dist-info/licenses/numpy/_core/src
./python_projects/ipl/.venv/lib/python3.14/site-packages/numpy/f2py/src
./python_projects/ipl/.venv/lib/python3.14/site-packages/numpy/f2py/tests/src
./python_projects/ipl/src
./python_projects/company-master-project/.venv/lib/python3.14/site-packages/numpy-2.5.4.dist-info/licenses/numpy/random/src
./python_projects/company-master-project/.venv/lib/python3.14/site-packages/numpy-2.5.4.dist-info/licenses/numpy/_core/src
./python_projects/company-master-project/.venv/lib/python3.14/site-packages/numpy/f2py/src
./python_projects/company-master-project/.venv/lib/python3.14/site-packages/numpy/f2py/tests/src
./python_projects/company-master-project/src
./python_projects/population_analysis_project/.venv/lib/python3.14/site-packages/numpy-2.5.4.dist-info/licenses/numpy/random/src
./python_projects/population_analysis_project/.venv/lib/python3.14/site-packages/numpy-2.5.4.dist-info/licenses/numpy/_core/src
./python_projects/population_analysis_project/.venv/lib/python3.14/site-packages/numpy/f2py/src
./python_projects/population_analysis_project/.venv/lib/python3.14/site-packages/numpy/f2py/tests/src
./python_projects/population_analysis_project/src
./python_projects/unit_testing_project/.venv/lib/python3.14/site-packages/numpy-2.5.4.dist-info/licenses/numpy/random/src
./python_projects/unit_testing_project/.venv/lib/python3.14/site-packages/numpy-2.5.4.dist-info/licenses/numpy/_core/src
./python_projects/unit_testing_project/.venv/lib/python3.14/site-packages/numpy/f2py/src
./python_projects/unit_testing_project/.venv/lib/python3.14/site-packages/numpy/f2py/tests/src
./python_projects/unit_testing_project/src
./python_projects/space_invader_game/.venv/lib/python3.14/site-packages/pip-26.3.dist-info/licenses/src
./python_projects/space_invader_game/src
./PycharmProjects/WelcomeScreen/.venv/lib/python3.14/site-packages/pip-26.0.1.dist-info/licenses/src
./.cache/virtualenv/wheel/3.14/image/1/CopyPipInstall/pip-26.0.1-py3-none-any/pip-26.0.1.dist-info/licenses/src
./.config/google-chrome/Default/Extensions/kbfnbcaeplbcioakkpcpgfkobkghlhen/14.1344.0_0/src
```

3. usage of astric(wid card entry) to find files and directories.
```bash
$ find -iname "**java**"
./.local/share/JetBrains/Daemon/bundles/current/jetbrainsd/licenses/third-party/net.java.dev.jna-jna-platform-5.17.0
./.local/share/JetBrains/Daemon/bundles/current.backup/jetbrainsd/licenses/third-party/net.java.dev.jna-jna-platform-5.17.0
./HelloWorld.java
./.java
./python_projects/unit_testing_project/.venv/lib/python3.14/site-packages/pygments/lexers/__pycache__/javascript.cpython-314.pyc
./python_projects/unit_testing_project/.venv/lib/python3.14/site-packages/pygments/lexers/javascript.py

```
4. Find files by sizes

```bash
find -size +200M
./Downloads/code_1.143.1-1792691907_amd64.deb
./python_projects/company-master-project/resources/csv_files/companies_registration_in_maharastra.csv
```

5. Find recently modified files 
```bash
$ find -mmin  -1
./.cache/JetBrains/PyCharm2026.2/index/write-ahead-log.wal
./.cache/JetBrains/PyCharm2026.2/log/idea.log
./.cache/JetBrains/PyCharm2026.2/log/telemetry
./.cache/JetBrains/PyCharm2026.2/log/telemetry/open-telemetry-metrics.2026-11-06-09-12-49.csv
./.cache/JetBrains/PyCharm2026.2/log/telemetry/open-telemetry-meters.2026-11-06-09-43-49.json
./.cache/JetBrains/PyCharm2026.2/log/indexing-diagnostic/storage-diagnostic-2026-11-06-09-12-48.482.json
./.cache/JetBrains/PyCharm2026.2/LocalHistory/changes.storageRecordIndex
./.cache/google-chrome/Default/Cache/Cache_Data/index-dir
./.cache/google-chrome/Default/Cache/Cache_Data/index-dir/the-real-index
./.cache/google-chrome/Default/Cache/Cache_Data/f54a5248391ee6c5_0
./.config/google-chrome/Default/Cookies
./.config/google-chrome/Default/DawnWebGPUCache/data_1
./.config/google-chrome/Default/Reporting and NEL-journal
./.config/google-chrome/Default/WebStorage/QuotaManager-journal
./.config/google-chrome/Default/WebStorage/QuotaManager
./.config/google-chrome/Default/Local Extension Settings/kbfnbcaeplbcioakkpcpgfkobkghlhen/000033.log
./.config/google-chrome/Default/Local Extension Settings/nbcojefnccbanplpoffopkoepjmhgdgh/000003.log
./.config/google-chrome/Default/Reporting and NEL
./.config/google-chrome/Default/Cookies-journal
./.config/google-chrome/Default/DawnGraphiteCache/data_1
./.config/google-chrome/Default/GPUCache/data_1
./.config/google-chrome/Default/Local Storage/leveldb/000260.log
./.config/google-chrome/BrowserMetrics/BrowserMetrics-6AED4941-12D6.pma

```