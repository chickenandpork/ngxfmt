# Ngxfmt

## Usage
```
usage: ngxfmt [-h] [-f nginx.conf] [-d conf.d]

nginx conf fmt tool

optional arguments:
  -h, --help     show this help message and exit
  -f nginx.conf  conf file
  -d conf.d      conf directory
```

## Demo

![demo0](./images/0.jpg)

![demo1](./images/1.jpg)

![demo2](./images/2.jpg)

## Bazel

I'm a fan of Bazel; this is how I bring this tool in for re-use:
`WORKSPACE`:
```python
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

...
    http_archive(
        name = "rules_python",
        ...
    )

http_archive(
    name = "com_github_fangpsh_ngxfmt",
    build_file_content = """exports_files(["ngxfmt.py"])""",
    sha256 = "c30c3f3afc33bff9f351b13545b88aa3056a28466cc684b1dfe3488d14bc3d11",
    strip_prefix = "ngxfmt-a294d143ff4d4825246a141cad2416392e115221",
    urls = [
        "https://github.com/fangpsh/ngxfmt/archive/a294d143ff4d4825246a141cad2416392e115221.tar.gz",
    ],
)
```

`BUILD.bazel`:
```python

load("@rules_python//python:defs.bzl", "py_binary")

py_binary(
    name = "ngxfmt",
    srcs = ["@com_github_fangpsh_ngxfmt//:ngxfmt.py"],
)
```

... then I'm currently running:

```
    bazel run //{dir}:ngxfmt -- -f {my nginx.conf file}
```
