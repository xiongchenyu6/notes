---
title: "nomad"
date: 2023-08-15
---

job "nixpkgs" { datacenters = ["dc1"] type = "service" group "nixpkgs" { network { port "www" { static = 8000 } } service { provider = "nomad" port = "www" } task "nixpkgs" { driver = "nix" resources { memory = 100 cpu = 300 } config { packages = [ "github:nixos/nixpkgs/nixos-unstable#python3", ] command = ["python3", "-m", "http.server" ,"\${NOMAD<sub>PORTwww</sub>}"] } } } }
