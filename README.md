zip-password-cracker
====================

ZIP Password Cracking Toolkit for NVIDIA GPUs

Docker image
------------

Pull images using Docker Compose.

```sh
$ docker-compose pull
```

Usage
-----

1.  Install NVIDIA Driver.

2.  Extract the password hash from a ZIP file (`foo.zip`).

    ```sh
    $ docker-compose run \
        -v ${PWD}:/wd:ro --rm --entrypoint=zip2john \
        john /wd/foo.zip \
        | grep -e '^[^:]\+.zip:' \
        | cut -d ':' -f 2 \
        | tee foo.zip.hash.txt
    ```

3.  Crack the password with brute-force attack using a GPU.

    ```sh
    $ docker-compose run \
        -v ${PWD}:/wd:ro -w /opt/hashcat --rm --entrypoint=./hashcat.bin \
        hashcat -a 3 -w 4 -m 17220 /wd/foo.zip.hash.txt ?a?a?a?a?a?a?a?a
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
