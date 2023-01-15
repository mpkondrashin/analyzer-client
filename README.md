# ACLI - Analyzer Command Line Interface

**Command line utility to use Deep Discovery Analyzer Web API**

## Parameters
Each parameter can be provided in three ways: 
1. Configuration file config.yaml. Acli seeks for this file in its current folder
2. Environment variables
3. Command line parameters (not all commands support all parameters):

| YAML option<br/>Command line<br/>Env variable | Description | 
| --------------------------------------------- | ----------- | 
| url<br/>--url<br/>url ACLI_URL | Analyzer address |
| api_key<br/>---api_key<br/>ACLI_API_KEY | Analyzer API key |
| ignore_tls_errors<br/>--ignore_tls_errors<br/>ACLI_IGNORE_TLS_ERRORS | Ignore TLS errors |
| client_id<br/>--client_id<br/>ACLI_CLIENT_ID | Client ID. Unique UUID for each submitter. After registering should be saved for future requests |
| sha1<br/>--sha1<br/>ACLI_SHA1 | SHA1 hash of requested file |
| filename<br/>--filename<br/>ACLI_FILENAME | Path to file |
| start<br/>--start<br/>ACLI_START | Start time in format YYYY-MM-DD-HH-MM |
| end<br>--end<br>ACLI_END | End time in format YYYY-MM-DD-HH-MM |
| device<br>--device<br>ACLI_DEVICE | Device type (SourceID) |
| sample<br>--sample<br>ACLI_SAMPLE | Sample ID returned by sample_list command |
| dry_run<br>--dry_run<br>ACLI_DRY_RUN | Show request and exit |


Any combination of parameters can be used with acli. For example, creating following configuration file (config.yaml):
```yaml
url: https://192.168.32.100
ignore_tls_error: true
client_id: 12341234-1234-1234-1234-123412341234
```
Following command can be used to generate report (example for Linux command line):
```
ACLI_API_KEY=12341234-1234-1234-1234-123412341234 ./acli report_raw --sha1 492bafdaa3cc57ffc8e0d7928d1d25a37d7d6d13
```

In this example, url, ignore_tls_error, and client_id are taken from configuration file; api_key from environment variable and sha1 from command line.

**Note:** If the same parameter is provided in two ways, command line parameters have higher priority than environment variable and the latter, higher priority than configuration file.

## Commands

### Testing Connection
Test connection to Analyzer

Required parameters: url, api_key
```commandline
acli test_connection <options>
```

### Register
Add client to Analyzer submitters list

Required parameters: url, api_key, client_id
```commandline
acli register <options>
```

### Unregister
Remove client from Analyzer submitters list

Required parameters: url, api_key, client_id
```commandline
acli unregister <options>
```

### Submit
Submit file to Analyzer for analysis

Required parameters: url, api_key, client_id, filename
```commandline
acli submit <options>
```

### Simple Submit
Submit file to Analyzer for analysis without accompanying metadata

Required parameters: url, api_key, client_id, filename
```commandline
acli simple <options>
```

### Brief Report
Get sample status and its risk level

Required parameters: url, api_key, client_id, sha1
```commandline
acli brief_report <options>
```

### Report Raw
Get analysis result report in XML format

Required parameters: url, api_key, client_id, sha1
```commandline
acli report_raw <options>
```

### Report
Get analysis result report parsed

Required parameters: url, api_key, client_id, sha1
```commandline
acli report <options>
```

### PDF Report
Get analysis result report in PDF format

Required parameters: url, api_key, client_id, sha1
```commandline
acli pdf_report <options>
```

### List Samples
List of samples by time interval

Required parameters: url, api_key, client_id

Optional paremeters: start, end, device
```commandline
acli sample_list <options>
```

### Sample Info as XML
Get sample information by its ID in XML format

Required parameters: url, api_key, client_id, sample
```commandline
acli sample_info <options>
```

### Sample Info parsed
Get sample information by its ID as parsed struct

Required parameters: url, api_key, client_id, sample
```commandline
acli sample_info_x <options>
```

### Download analysis package
Get all analysis information as one password-protected ZIP file.
Password for ZIP is "virus"

Required parameters: url, api_key, client_id, sha1
```commandline
acli package <options>
```

### Get Analyzer quota as XML
Get throughput of Analyzer for files and URLs in minute

Required parameters: url, api_key, client_id
```commandline
acli quota <options>
```

## Example

### 1. Configuration file
Create file config.yaml with following content:
```yaml
url: https://<your analyzer address>
apu_key: <your analyzer api key> # Help -> About on Analyzer Web console
ignore_tls_errors: true
client_id: 12341234-1234-1234-1234-123412341234 # or any other unique UUID
```

### 2. Test Connection
Run following command
```commandline
./acli test_connection
```

### 3. Register
Run following command
```commandline
./acli register
```
Check Submission -> Submitters on Analyzer Web UI to see that this tool successfully registered itself

### 4. Submit File
Run following command
```commandline
./acli submit --filename <file path>
```

### 5. Check Result
Get submitted file SHA1 has using following command:
```commandline
shasum -a 1  <file path>
```
Using this hash, run the following command
```commandline
./acli brief_report --sha1 <hash>
```
When status will be "4", RiskLevel will show whenever the file is malicious or not
### 6. Get Reports
Run following command for XML format
```commandline
./acli report_raw --sha1 <hash>
```
Or use following command for PDF
```commandline
acli pdf_report --sha1 <hash>
```
Or download full analysis package using following command:
```commandline
acli package --sha1 <hash>
```

### 7. Unregister
Run following command
```commandline
./acli unregister
```
Check Submissions -> Submitters on Analyzer Web UI to see that this tool successfully unregistered itself
