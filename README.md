# ACLI - Analyzer Commad Line Interface

**Command line utility to use Deep Discovery Analyzer Web API**

## Parameters
Each parameter can be provided in three ways: 
1. Configuration file config.yaml. Acli seeks for this file in its current folder
2. Environment variables
3. Command line parameters

| YAML option<br/>Command line<br/>Env variable | Description | 
| --------------------------------------------- | ----------- | 
| url<br/>--url<br/>url ACLI_URL | Analyzer address |
| api_key<br/>---api_key<br/>ACLI_API_KEY | Analyzer API key |
| ignore_tls_errors<br/>--ignore_tls_errors<br/>ACLI_IGNORE_TLS_ERRORS | Ignore TLS errors |
| client_id<br/>--client_id<br/>ACLI_CLIENT_ID | Client ID. Unique UUID for each submitter. After registering should be saved for future requests |
| sha1<br/>--sha1<br/>ACLI_SHA1 | SHA1 hash of requested file |
| filename<br/>--filename<br/>ACLI_FILENAME | Path to file |

Any combination of parameters can be used with acli. For example, creating following configuration file (config.yaml):
```yaml
url: https://192.168.32.100
ignore_tls_error: true
client_id: 12341234-1234-1234-1234-123412341234
```
Following command can be used t ogenerate report (example for Linux command line):
```
ACLI_API_KEY=12341234-1234-1234-1234-123412341234 ./acli report_raw --sha1 492bafdaa3cc57ffc8e0d7928d1d25a37d7d6d13
```

In this example url, ignore_tls_error, and client_id are taken from configuration file; api_key from environment variable and sha1 from command line.

**Note:** If same parameter is provided in two ways, command line parameters have higher priority than environment variable and the latter, higher priority than configuration file.

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
Check Submitions -> Submitters on Analyzer Web UI to see that this tool successfully registered itself

### 4. Submit File
Run following command
```commandline
./acli submit --filename <file path>
```

### 5. Check Result
Get submitted file SHA1 has using following command:
```commandline
sha1sum -a 1  <file path>
```
Using this hash run following command
```commandline
./acli brief_report --sha1 <hash>
```
When status will be "Done", RiskLevel will show whenever the file is malicious or not
### 6. Get Full Report
Run following command
```commandline
./acli report_raw --sha1 <hash>
```
### 7. Unregister
Run following command
```commandline
./acli unregister
```
Check Submitions -> Submitters on Analyzer Web UI to see that this tool successfully unregistered itself
