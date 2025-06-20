# GoReleaser
GoReleaser是一款优秀的Go打包工具，他可以一键完成Go的打包工作
如官网所说：

Release engineering, simplified.
We handle the complexities of releasing so you can focus in building what really matters: your software - and it doesn't need to be written in Go!

### 参考yaml配置文件：
```
# This is an example .goreleaser.yml file with some sensible defaults.
# Make sure to check the documentation at https://goreleaser.com
before:
  hooks:
    # You may remove this if you don't use go modules.
    - go mod tidy
    # you may remove this if you don't need go generate
    - go generate ./...
builds:
  - env:
      - CGO_ENABLED=0
    goos:
      - linux
      - windows
      - darwin
    goarch:
      - amd64
      - arm
      - arm64

archives:
  - format: tar.gz
    # this name template makes the OS and Arch compatible with the results of uname.
    name_template: >-
      {{ .ProjectName }}_
      {{- title .Os }}_
      {{- if eq .Arch "amd64" }}x86_64
      {{- else if eq .Arch "386" }}i386
      {{- else }}{{ .Arch }}{{ end }}
      {{- if .Arm }}v{{ .Arm }}{{ end }}
    # use zip for windows archives
    format_overrides:
    - goos: windows
      format: zip
checksum:
  name_template: 'checksums.txt'
snapshot:
  name_template: "{{ incpatch .Version }}-next"
changelog:
  sort: asc
  filters:
    exclude:
      - '^docs:'
      - '^test:'

# The lines beneath this are called `modelines`. See `:help modeline`
# Feel free to remove those if you don't want/use them.
# yaml-language-server: $schema=https://goreleaser.com/static/schema.json
# vim: set ts=2 sw=2 tw=0 fo=cnqoj
```

这将一键打包linux windows macos 的 x64 arm32 arm64的版本
