# 为私人 GitHub Repos 创建自制水龙头

> 原文：<https://blog.devgenius.io/create-homebrew-taps-for-private-github-repos-44daf2f4cff8?source=collection_archive---------8----------------------->

![](img/56c80cfd785a8519c7f8f7d584a004cf.png)

[扎卡里·凯米格](https://unsplash.com/@zacharykeimig?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

我在 2022 年 7 月开始了一份新工作。虽然开发人员的经验很棒，但我在新工作中错过的一件事是用 CLI *来统治他们所有人*(就像我以前的工作一样)。我想构建一个易于扩展的 CLI 工具，我使用了 *go* 和 [cobra](https://github.com/spf13/cobra) 。为了处理对工程师的分配，我们决定同时使用`go install`和 [*自制*](https://brew.sh/) 。

自制程序是非常受 macOS 和 Linux 用户欢迎的包管理器。用户可以通过`brew install [PACKAGE]`轻松安装新的软件包。自制软件负责将软件包下载和安装到合适的目录中，然后创建到`/usr/local`的符号链接(在英特尔机器上)。

为了创建一个自制软件包，我们创建了名为*公式*的 Ruby 类。公式如下所示:

```
class Wget < Formula
  homepage "https://www.gnu.org/software/wget/"
  url "https://ftp.gnu.org/gnu/wget/wget-1.15.tar.gz"
  sha256 "52126be8cf1bddd7536886e74c053ad7d0ed2aa89b4b630f76785bac21695fcd" def install
    system "./configure", "--prefix=#{prefix}"
    system "make", "install"
  end
end
```

虽然添加一个开源包相当简单，但是对一个私有包做同样的事情需要一些黑客攻击。

# 第一步

首先要做的是创建一个 Github 版本，如果你使用 *go* 的话，我强烈建议使用 [*或者*](https://github.com/goreleaser/goreleaser) 。我在这里写了一篇关于如何做到这一点的教程。它确实让*中的发布变得容易。如果你在使用另一种语言，你应该看看如何在 GitHub 中管理发布版本[在这里](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository)。*

# 创建自制水龙头

接下来要做的是创建一个[自制 tap](https://docs.brew.sh/Taps) 。家酿 tap 是一个 GitHub 存储库，存放着我们的公式。要添加非自制核心 tap，我们需要使用`brew tap username/package`在本地将 tap 添加到自制。为了使用单参数形式的`brew tap`，我们需要遵循[命名约定](https://docs.brew.sh/Taps#repository-naming-conventions-and-assumptions)并将我们的 GitHub repo 命名为`homebrew-tap`，因为 GitHub repo 必须以`homebrew-`开头。

# 创建公式

如果 Github 中已经有一个版本，我们可以继续创建公式。在我们的`homebrew-tap` repo 中，我们将创建一个名为`Formula`的文件夹。在`Formula`目录中，我们将创建一个名为`mytool.rb`的文件。

现在，在我们的`Formula`中，我们不能使用标准的方式下载软件包，因为我们的软件包托管在一个私有的回购协议中。为此，我们只需添加一个新的下载策略，如下所示:

```
require "formula"
require_relative "../custom_download_strategy.rb"class Wget < Formula
  homepage "https://www.gnu.org/software/wget/"
  url "https://ftp.gnu.org/gnu/wget/wget-1.15.tar.gz", :using => GitHubPrivateRepositoryReleaseDownloadStrategy 
  sha256 "52126be8cf1bddd7536886e74c053ad7d0ed2aa89b4b630f76785bac21695fcd" def install
    system "./configure", "--prefix=#{prefix}"
    system "make", "install"
  end test do
      system "#{bin}/wget --help"
  end
end
```

注意在类的第三行增加了`, :using => GitHubPrivateRepositoryReleaseDownloadStrategy`。

在`homebrew-tap`的根文件夹中，我们将创建一个名为`custom_download_strategy.rb`的新文件。这里，我们将添加一些代码来支持我们的下载策略。

```
require "download_strategy"class GitHubPrivateRepositoryDownloadStrategy < CurlDownloadStrategy
  require "utils/formatter"
  require "utils/github" def initialize(url, name, version, **meta)
    super
    parse_url_pattern
    set_github_token
  end def parse_url_pattern
    unless match = url.match(%r{https://github.com/([^/]+)/([^/]+)/(\S+)})
      raise CurlDownloadStrategyError, "Invalid url pattern for GitHub Repository."
    end _, @owner, @repo, @filepath = *match
  end def download_url
    "https://#{@github_token}@github.com/#{@owner}/#{@repo}/#{@filepath}"
  end private def _fetch(url:, resolved_url:, timeout:)
    curl_download download_url, to: temporary_path, timeout: timeout
  end def set_github_token
    @github_token = ENV["HOMEBREW_GITHUB_API_TOKEN"]
    unless @github_token
      raise CurlDownloadStrategyError, "Environmental variable HOMEBREW_GITHUB_API_TOKEN is required."
    end validate_github_repository_access!
  end def validate_github_repository_access!
    # Test access to the repository
    GitHub.repository(@owner, @repo)
  rescue GitHub::HTTPNotFoundError
    # We only handle HTTPNotFoundError here,
    # becase AuthenticationFailedError is handled within util/github.
    message = <<~EOS
      HOMEBREW_GITHUB_API_TOKEN can not access the repository: #{@owner}/#{@repo}
      This token may not have permission to access the repository or the url of formula may be incorrect.
    EOS
    raise CurlDownloadStrategyError, message
  end
end# GitHubPrivateRepositoryReleaseDownloadStrategy downloads tarballs from GitHub
# Release assets. To use it, add
# `:using => GitHubPrivateRepositoryReleaseDownloadStrategy` to the URL section of
# your formula. This download strategy uses GitHub access tokens (in the
# environment variables HOMEBREW_GITHUB_API_TOKEN) to sign the request.
class GitHubPrivateRepositoryReleaseDownloadStrategy < GitHubPrivateRepositoryDownloadStrategy
  def initialize(url, name, version, **meta)
    super
  end def parse_url_pattern
    url_pattern = %r{https://github.com/([^/]+)/([^/]+)/releases/download/([^/]+)/(\S+)}
    unless @url =~ url_pattern
      raise CurlDownloadStrategyError, "Invalid url pattern for GitHub Release."
    end _, @owner, @repo, @tag, @filename = *@url.match(url_pattern)
  end def download_url
    "https://#{@github_token}@api.github.com/repos/#{@owner}/#{@repo}/releases/assets/#{asset_id}"
  end private def _fetch(url:, resolved_url:, timeout:)
    # HTTP request header `Accept: application/octet-stream` is required.
    # Without this, the GitHub API will respond with metadata, not binary.
    curl_download download_url, "--header", "Accept: application/octet-stream", to: temporary_path, timeout: timeout
  end def asset_id
    @asset_id ||= resolve_asset_id
  end def resolve_asset_id
    release_metadata = fetch_release_metadata
    assets = release_metadata["assets"].select { |a| a["name"] == @filename }
    raise CurlDownloadStrategyError, "Asset file not found." if assets.empty? assets.first["id"]
  end def fetch_release_metadata
    GitHub.get_release(@owner, @repo, @tag)
  end
end
```

我们应该有以下文件夹结构:

```
├── homebrew-tap
│   ├── Formula
│   │   ├── mytool.rb
└── custom_download_strategy.rb
```

# 使用

我们快完成了。要通过家酿安装我们的工具，我们需要导出一个 Github 令牌，提供对我们的私有 repos 的访问。在这里可以创建一个新的令牌。令牌需要有`repo`权限。我们需要将令牌导出为`HOMEBREW_GITHUB_API_TOKEN`。要导出令牌，我们运行:

```
export HOMEBREW_GITHUB_API_TOKEN=<GITHUB_TOKEN>
# if you're using fish shell like me, run `set -x HOMEBREW_GITHUB_API_TOKEN <GITHUB_TOKEN>`
```

然后，我们可以运行命令:

```
brew install username/homebrew-tap/mytool
```

这将自动添加自制水龙头，并安装我们的工具。

# 摘要

*   用二进制文件创建一个 GitHub 版本。在 *go* 的情况下，这可以通过 *goreleaser* 非常容易地完成。
*   创建一个名为`homebrew-tap`的 GitHub repo。这里的就是一个例子。
*   创建一个`Formula`文件夹。
*   在 repo 的根目录下，创建一个名为`custom_download_strategy.rb`的文件。内容如上所示
*   在`Formula`文件夹中，创建一个 ruby 文件，用你的工具名命名，比如`mytool.rb`
*   在这里创建一个 GitHub 令牌[，并授予`repo`对该令牌的权限。](https://github.com/settings/tokens)
*   用`export HOMEBREW_GITHUB_API_TOKEN=<GITHUB_TOKEN>`导出令牌
*   使用`brew install username/homebrew-tap/mytool`安装带有 brew 的工具。