# swift-docs
# Swift Package 使用过程遇到的问题

### `1.error: manifest parse error(s): Invalid version string: 2.0`
- 问题：tag标签版本号不规范，不足三位数
- [x] 解决：
  - 设置三位数的版本号，如：1.9.0
  - 具体设置依赖关系如下：.package(url: "https://github.com/kawoou/FlexibleImage", .upToNextMajor(from: "1.8.0"))


## `2.error: dependency graph is unresolvable; found these conflicting requirements`
- > Dependencies:  https://github.com/kawoou/FlexibleImage @ 1.8.0..<2.0.0
- 问题：git参数的tag不存在三位数的标签
- [x] 解决：
  - 在git中新建tag v1.8.0 并推到远程到仓库中,不是自己的仓库需要fork至自己个人仓库在推送。
  - 具体设置依赖关系如下： .package(url: "https://github.com/tomlee130/FlexibleImage", .upToNextMajor(from: "1.8.0"))
  - 然后重试swfit package udpate

## `3.warning: target 'FlexibleImage' in package 'FlexibleImage' contains no valid source files`
 - > error: target 'FlexibleImage' referenced in product 'FlexibleImage' could not be found
 - > error: product dependency 'FlexibleImage' not found
- 问题：依赖库工程FlexibleImage不是一个规范的swift library结构，少了一级FlexibleImage目录
    - |____Sources
    | |____Abstract
      | | |____ImageDevice.swift
    | | |____ImageFilter.swift
    | |____Device
    | | |____ImageMetalDevice.swift
    | | |____ImageNoneDevice.swift
    | |____Filter
    | | |____AddFilter.metal
    | | |____AddFilter.swift
    | |____FlexibleImage.swift
- [x] 解决：调整工程目录
    - $> mkdir -p FlexibleImage && cd FlexibleImage
    - $> swift package init --type library    # 初始化为一个库
    - $> cp Package.swift  tomlee130/FlexibleImage
    - $> mkdir -p Sources/FlexibleImage
    - $> cp tomlee130/FlexibleImage/Sources/*  tomlee130/FlexibleImage/Sources/FlexibleImage/
    - so，目录结构如下，
    - |____Sources
      | |____FlexibleImage
      | | |____Abstract
      | | | |____ImageDevice.swift
      | | | |____ImageFilter.swift
      | | |____Device
      | | | |____ImageMetalDevice.swift
      | | | |____ImageNoneDevice.swift
      | | |____Filter
      | | | |____AddFilter.metal
      | | | |____AddFilter.swift
      | | |____FlexibleImage.swift
      | | |____Type
