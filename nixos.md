## How to switch to flakes from path-based nix

1. drop this in `/etc/nixos/flake.nix`

```nix
{
  description = "system flake";
  inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs/nixos-unstable";
  };

  outputs = {nixpkgs, ...}: {
    nixosConfigurations = {
      hostname = nixpkgs.lib.nixosSystem {
        modules = [
          ./configuration.nix
        ];
      };
    };
  };
}
```

2. change `hostname` (and optionally, the `nixpkgs` url if e.g. you want to use a stable branch)
1. run `nix flake update`
1. rebuild your system (`nixos-rebuild boot`)
1. congrats you now use flakes

## How to update your system

1. run `nix flake update`
1. rebuild your system

You can also freely move the flake (the contents of `/etc/nixos` to any other location, and/or create a version-controlled (e.g. git-controlled) repo out of it.

If you do use git, **do not ignore the flake.lock; always include it in commits.** That's how you keep reproducibility of your config and allow rolling back in case of breakage.
