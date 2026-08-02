# BreadFactory-arch-PKGBUILD

> PKGBUILDs are files used by [Archlinux](https://www.archlinux.org/)'s package manage pacman.  A PKGBUILD is a bash file that is passed by makepkg and libalpm.

This repo simply hosts PKGBUILDs that I have written, and each individual software's PKGBUILD can be accessed through `branches`.

## In Practice

Packages are available through branches, and can be used as follows:
1. `git clone https://github.com/Nojjia/BreadFactory-arch-PKGBUILD.git --branch=zen-browser-bin`
    This clones the required branch of the git repository
2. `makepkg -si`
    This install dependencies, and install the package onto your system

List of available PKGBUILDs:
- []()

List of planned PKGBUILDs:
- [ ] affine-bin
- [ ] anytype-bin
- [ ] arduino-ide-v2-bin
- [ ] joplin-bin
- [ ] losslesscut-bin
- [ ] onlyoffice-desktopeditors-bin
- [ ] pureref-bin
- [ ] super=productivity-bin
- [ ] trilium-bin
- [ ] zen-browser-bin


## Steps that I should follow before commits and pushing

1. `makepkg`

2. `namcap PKGBUILD`

3. `pacman -Qpl pkgname.pkg.tar.zst`

4. `pacman -Qpi pkgname.pkg.tar.zst`

5. `namcap pkgname.pkg.tar.zst`

6. `pkgctl build`

7. `makepkg --printsrcinfo > .SRCINFO`
