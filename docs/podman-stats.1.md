% podman-stats(1)

## NAME
podman\-stats - Display a live stream of 1 or more containers' resource usage statistics

## SYNOPSIS
**podman stats** [*options*] [*container*]

## DESCRIPTION
Display a live stream of one or more containers' resource usage statistics

Note:  Podman stats will not work in rootless environments that use CGroups V1.
Podman stats relies on CGroup information for statistics, and CGroup v1 is not
supported for rootless use cases.

## OPTIONS

**--all**, **-a**

Show all containers.  Only running containers are shown by default

**--latest**, **-l**

Instead of providing the container name or ID, use the last created container. If you use methods other than Podman
to run containers such as CRI-O, the last started container could be from either of those methods.

The latest option is not supported on the remote client.

**--no-reset**

Do not clear the terminal/screen in between reporting intervals

**--no-stream**

Disable streaming stats and only pull the first result, default setting is false

**--format**=*template*

Pretty-print container statistics to JSON or using a Go template

Valid placeholders for the Go template are listed below:

| **Placeholder** | **Description**   |
| --------------- | ---------------   |
| .Pod            | Pod ID      |
| .ID             | Container ID      |
| .Name           | Container Name    |
| .CPU            | CPU percentage    |
| .MemUsage       | Memory usage      |
| .Mem            | Memory percentage |
| .NetIO          | Network IO        |
| .BlockIO        | Block IO          |
| .PIDS           | Number of PIDs    |

When using a GO template, you may precede the format with `table` to print headers.

## EXAMPLE

```
# podman stats -a --no-stream
ID             NAME              CPU %   MEM USAGE / LIMIT   MEM %   NET IO    BLOCK IO   PIDS
a9f807ffaacd   frosty_hodgkin    --      3.092MB / 16.7GB    0.02%   -- / --   -- / --    2
3b33001239ee   sleepy_stallman   --      -- / --             --      -- / --   -- / --    --
```

```
# podman stats --no-stream a9f80
ID             NAME             CPU %   MEM USAGE / LIMIT   MEM %   NET IO    BLOCK IO   PIDS
a9f807ffaacd   frosty_hodgkin   --      3.092MB / 16.7GB    0.02%   -- / --   -- / --    2
```

```
# podman stats --no-stream --format=json a9f80
[
    {
	"id": "a9f807ffaacd",
	"name": "frosty_hodgkin",
	"cpu_percent": "--",
	"mem_usage": "3.092MB / 16.7GB",
	"mem_percent": "0.02%",
	"netio": "-- / --",
	"blocki": "-- / --",
	"pids": "2"
    }
]
```

```
# podman stats --no-stream --format "table {{.ID}} {{.Name}} {{.MemUsage}}" 6eae
ID             NAME           MEM USAGE / LIMIT
6eae9e25a564   clever_bassi   3.031MB / 16.7GB
```

## SEE ALSO
podman(1)

## HISTORY
July 2017, Originally compiled by Ryan Cole <rycole@redhat.com>
