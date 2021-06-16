zip-password-cracker
====================

ZIP Password Cracking Toolkit for NVIDIA GPUs

[![CI to Docker Hub](https://github.com/dceoy/zip-password-cracker/actions/workflows/docker-publish.yml/badge.svg)](https://github.com/dceoy/zip-password-cracker/actions/workflows/docker-publish.yml)

Docker image
------------

```sh
$ docker image pull dceoy/john:latest
$ docker image pull dceoy/hashcat:latest
```

Usage
-----

1.  Install the dependencies:
    - Docker
    - NVIDIA Driver
    - NVIDIA Container Toolkit

2.  Extract the password hash from a ZIP file (`./foo.zip`).

    ```sh
    $ docker container run --rm --entrypoint=zip2john -v ${PWD}:/wd:ro \
        dceoy/john /wd/foo.zip \
        | grep -e '^[^:]\+.zip:' \
        | cut -d ':' -f 2 \
        | tee foo.zip.hash.txt
    ```

3.  Crack the password with brute-force attack using a GPU.

    ```sh
    $ nvidia-docker container run --rm -v ${PWD}:/wd:ro \
        dceoy/hashcat -a 3 -w 4 -m 17220 /wd/foo.zip.hash.txt ?a?a?a?a?a?a?a?a
    ```

    The hash types of ZIP files:

    | Hash-Mode | Hash-Name                                   |
    |:----------|:--------------------------------------------|
    | 13600     | WinZip                                      |
    | 17200     | PKZIP (Compressed)                          |
    | 17210     | PKZIP (Uncompressed)                        |
    | 17220     | PKZIP (Compressed Multi-File)               |
    | 17225     | PKZIP (Mixed Multi-File)                    |
    | 17230     | PKZIP (Compressed Multi-File Checksum-Only) |

    Reference: https://hashcat.net/wiki/doku.php?id=example_hashes
