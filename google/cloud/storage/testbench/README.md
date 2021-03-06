# Storage Test Bench for Python 3

This is a testbench for the Storage JSON API, with limited support for the XML API. While it emulates most of the behaviors of Google Cloud Storage, it was developed mostly as a testbench for the client library, the testbench, for example, performs far fewer error checks and no permission checks (ACL/IAM). Generally, the error codes are similar to the ones generated by GCS, but the error messages are not. In short, this is intended for testing, and not as a general purpose emulator.

## Install Dependencies

```bash
pip install -r requirements.txt
```

## Run Test Bench

```bash
python3 testbench.py --port 8080
```

For more information use: `python3 testbench.py -h`

## Force Failures

You can force the following failures by using the `x-goog-testbench-instructions` header.

### return-broken-stream

Set request headers with `x-goog-testbench-instructions: return-broken-stream`.
Emulator will fail after sending 1024*1024 bytes.

### return-corrupted-data

Set request headers with `x-goog-testbench-instructions: return-corrupted-data`.
Emulator will return corrupted data.

### stall-always

Set request headers with `x-goog-testbench-instructions: stall-always`.
Emulator will stall at the beginning.

### stall-at-256KiB

Set request headers with `x-goog-testbench-instructions: stall-at-256KiB`.
Emulator will stall at 256KiB bytes.

### return-503-after-256K

Set request headers with `x-goog-testbench-instructions: return-503-after-256K`.
Emulator will return a `HTTP 503` after sending 256KiB bytes.

### return-503-after-256K/retry-N

Set request headers with `x-goog-testbench-instructions: return-503-after-256K/retry-1` up to `x-goog-testbench-instructions: return-503-after-256K/retry-N`.

For N==1 and N==2 behave like `return-305-after-256K`, for `N>=3` ignore the failure instruction and return successfully. This is used to test failures during retry, the client cooperates by sending the retry counter in the failure instructions.
