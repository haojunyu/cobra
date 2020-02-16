![cobra logo](https://cloud.githubusercontent.com/assets/173412/10886352/ad566232-814f-11e5-9cd0-aa101788c117.png)

Cobra 既是用于创建功能强大的现代 CLI 应用程序的库，又是用于生成应用程序和命令文件的程序。

许多使用最广泛的Go项目都是使用Cobra构建的，例如：
[Kubernetes](http://kubernetes.io/),
[Hugo](http://gohugo.io),
[rkt](https://github.com/coreos/rkt),
[etcd](https://github.com/coreos/etcd),
[Moby (former Docker)](https://github.com/moby/moby),
[Docker (distribution)](https://github.com/docker/distribution),
[OpenShift](https://www.openshift.com/),
[Delve](https://github.com/derekparker/delve),
[GopherJS](http://www.gopherjs.org/),
[CockroachDB](http://www.cockroachlabs.com/),
[Bleve](http://www.blevesearch.com/),
[ProjectAtomic (enterprise)](http://www.projectatomic.io/),
[Giant Swarm's gsctl](https://github.com/giantswarm/gsctl),
[Nanobox](https://github.com/nanobox-io/nanobox)/[Nanopack](https://github.com/nanopack),
[rclone](http://rclone.org/),
[nehm](https://github.com/bogem/nehm),
[Pouch](https://github.com/alibaba/pouch),
[Istio](https://istio.io),
[Prototool](https://github.com/uber/prototool),
[mattermost-server](https://github.com/mattermost/mattermost-server),
[Gardener](https://github.com/gardener/gardenctl),
[Linkerd](https://linkerd.io/),
等.

[![Build Status](https://travis-ci.org/spf13/cobra.svg "Travis CI status")](https://travis-ci.org/spf13/cobra)
[![CircleCI status](https://circleci.com/gh/spf13/cobra.png?circle-token=:circle-token "CircleCI status")](https://circleci.com/gh/spf13/cobra)
[![GoDoc](https://godoc.org/github.com/spf13/cobra?status.svg)](https://godoc.org/github.com/spf13/cobra)
[![Go Report Card](https://goreportcard.com/badge/github.com/spf13/cobra)](https://goreportcard.com/report/github.com/spf13/cobra)

# Table of Contents

- [Table of Contents](#table-of-contents)
- [总览](#%e6%80%bb%e8%a7%88)
- [Concepts](#concepts)
  - [Commands](#commands)
  - [Flags](#flags)
- [Installing](#installing)
- [入门](#%e5%85%a5%e9%97%a8)
  - [使用 `Cobra` 生成器](#%e4%bd%bf%e7%94%a8-cobra-%e7%94%9f%e6%88%90%e5%99%a8)
  - [使用 `Cobra` 库](#%e4%bd%bf%e7%94%a8-cobra-%e5%ba%93)
    - [创建 rootCmd](#%e5%88%9b%e5%bb%ba-rootcmd)
    - [创建 main.go](#%e5%88%9b%e5%bb%ba-maingo)
    - [创建子命令](#%e5%88%9b%e5%bb%ba%e5%ad%90%e5%91%bd%e4%bb%a4)
  - [使用命令行参数](#%e4%bd%bf%e7%94%a8%e5%91%bd%e4%bb%a4%e8%a1%8c%e5%8f%82%e6%95%b0)
    - [分配参数给命令](#%e5%88%86%e9%85%8d%e5%8f%82%e6%95%b0%e7%bb%99%e5%91%bd%e4%bb%a4)
    - [Persistent Flags](#persistent-flags)
    - [Local Flags](#local-flags)
    - [Local Flag on Parent Commands](#local-flag-on-parent-commands)
    - [通过配置设置命令行参数](#%e9%80%9a%e8%bf%87%e9%85%8d%e7%bd%ae%e8%ae%be%e7%bd%ae%e5%91%bd%e4%bb%a4%e8%a1%8c%e5%8f%82%e6%95%b0)
    - [Required flags](#required-flags)
  - [位置和自定义参数](#%e4%bd%8d%e7%bd%ae%e5%92%8c%e8%87%aa%e5%ae%9a%e4%b9%89%e5%8f%82%e6%95%b0)
  - [样例](#%e6%a0%b7%e4%be%8b)
  - [帮助命令](#%e5%b8%ae%e5%8a%a9%e5%91%bd%e4%bb%a4)
    - [样例](#%e6%a0%b7%e4%be%8b-1)
    - [定义自己的帮助命令](#%e5%ae%9a%e4%b9%89%e8%87%aa%e5%b7%b1%e7%9a%84%e5%b8%ae%e5%8a%a9%e5%91%bd%e4%bb%a4)
  - [使用信息](#%e4%bd%bf%e7%94%a8%e4%bf%a1%e6%81%af)
    - [样例](#%e6%a0%b7%e4%be%8b-2)
    - [自定义用法](#%e8%87%aa%e5%ae%9a%e4%b9%89%e7%94%a8%e6%b3%95)
  - [版本参数](#%e7%89%88%e6%9c%ac%e5%8f%82%e6%95%b0)
  - [运行前和运行后的钩子](#%e8%bf%90%e8%a1%8c%e5%89%8d%e5%92%8c%e8%bf%90%e8%a1%8c%e5%90%8e%e7%9a%84%e9%92%a9%e5%ad%90)
  - [发生 `unknown command` 的建议](#%e5%8f%91%e7%94%9f-unknown-command-%e7%9a%84%e5%bb%ba%e8%ae%ae)
  - [生成命令文档](#%e7%94%9f%e6%88%90%e5%91%bd%e4%bb%a4%e6%96%87%e6%a1%a3)
  - [生成 bash 自动补全](#%e7%94%9f%e6%88%90-bash-%e8%87%aa%e5%8a%a8%e8%a1%a5%e5%85%a8)
  - [生成 zsh 自动补全](#%e7%94%9f%e6%88%90-zsh-%e8%87%aa%e5%8a%a8%e8%a1%a5%e5%85%a8)
- [贡献](#%e8%b4%a1%e7%8c%ae)
- [许可证](#%e8%ae%b8%e5%8f%af%e8%af%81)

# 总览

Cobra 是一个工具库，它能够提供简单接口来创建类似于 git & go 工具的现代 CLI 界面。

Cobra 也是一个应用程序，它能构建你的应用程序框架来帮助你快速
开发一个基于 Cobra 的应用程序。

Cobra 有以下特点:
* 简单的子命令行接口： `app server`， `app fetch` 等
* 
* 完全符合 POSIX 的命令行参数（包括完整和简化模式）
* 嵌套子命令
* 全局，局部和级联的命令行参数
* 使用 `cobra init appname` 和 `cobra add cmdname` 可以轻松生成应用程序和命令
* 自动提示 (`app srver`... 你是指 `app server` ?)
* Automatic help generation for commands and flags
* Automatic help flag recognition of `-h`, `--help`, etc.
* 为应用程序自动生成 bash 自动补全功能
* 为应用程序自动生成 man 帮助文档
* 命令别名，以便你可以更改内容而不会破坏它们
* 灵活的定义自己的帮助，用法等
* 与 [viper](http://github.com/spf13/viper) 的可选紧密集成，可用于 12-factor 应用程序

# Concepts

Cobra is built on a structure of commands, arguments & flags.
Cobra 

**Commands** represent actions, **Args** are things and **Flags** are modifiers for those actions.

最好的应用程序的用法就像读一个句子一样。用户会知道
如何使用该应用程序，因为他们自然而然的懂的用法。

一般遵循的模式是 `APPNAME VERB NOUN --ADJECTIVE` 或者 `APPNAME COMMAND ARG --FLAG`。

一些现实的例子可以更好地说明这一点。

在下面的示例中，`server` 是命令，`port` 是命令行参数：
> hugo server --port=1313

在这个命令中，我们使用 `git` 来克隆 `URL` 的内容
> git clone URL --bare 


## Commands

命令是应用程序的核心。应用程序支持的每次互动
都体现在命令上。一个命令可以有子命令并且有选择的执行操作。

在上面的示例中，`server` 是命令。

[有关 `cobra.Command` 的更多信息](https://godoc.org/github.com/spf13/cobra#Command)

## Flags

命令行参数是一种变更命令行为的方法。 `cobra` 支持完全兼容POSIX的用法以及Go [flag 包]](https://golang.org/pkg/flag/)。
cobra 命令支持为子命令定义命令参数并且该参数只适用于该子命令。

在上面的示例中，`port` 是命令行参数。

命令行参数功能是由 [pflag
库](https://github.com/spf13/pflag) 支持。它是 flag 标准库的一个分支，在保证了相同接口的同时添加了 POSIX 特性。


# Installing

使用 `Cobra` 很容易。首先，使用 `go get` 安装最新版本。这个命令将安装 `cobra` 可执行文件以及库和相关依赖项：
```bash
go get -u github.com/spf13/cobra/cobra
```
下一步是包含 `Cobra` 到你的应用程序中

    去得到-u github.com/spf13/cobra/cobra

接下来，将Cobra包含在您的应用程序中：

```go
import "github.com/spf13/cobra"
```

# 入门

While you are welcome to provide your own organization, typically a Cobra-based
application will follow the following organizational structure:
我们欢迎你提供自己的应用程序组织结构，一个典型的基于 `Cobra` 的应用程序将遵循以下组织结构：

```
  ▾ appName/
    ▾ cmd/
        add.go
        your.go
        commands.go
        here.go
      main.go
```

在基于 `Cobra` 的应用中， 通常 main.go 文件很简单。它只做一件事： 初始化`Cobra`。

```go
package main

import (
  "{pathToYourApp}/cmd"
)

func main() {
  cmd.Execute()
}
```

## 使用 `Cobra` 生成器

`Cobra` 提供了自己的程序，该程序将创建您的应用程序并添加任何
您想要的命令。这是将 `Cobra` 集成到你的应用程序中的最简单方法。

[这里](https://github.com/spf13/cobra/blob/master/cobra/README.md) 你可以找到关于生成基于 `Cobra` 应用程序的更多信息。

## 使用 `Cobra` 库

要手动实现基于 `Cobra` 的应用程序，你需要创建一个 main.go 文件和 rootCmd 文件。
您可以选择提供合适的命令。

### 创建 rootCmd

`Cobra` 不需要任何特殊的构造函数。只需创建命令即可。

正常情况下，将其放置在 app/cmd/root.go 目录下:

```go
var rootCmd = &cobra.Command{
  Use:   "hugo",
  Short: "Hugo is a very fast static site generator",
  Long: `A Fast and Flexible Static Site Generator built with
    love by spf13 and friends in Go.
    Complete documentation is available at http://hugo.spf13.com`,
  Run: func(cmd *cobra.Command, args []string) {
    // Do Stuff Here
  },
}

func Execute() {
  if err := rootCmd.Execute(); err != nil {
    fmt.Println(err)
    os.Exit(1)
  }
}
```

您将在 `init()` 函数中定义命令行参数并处理相关配置。

比如 cmd/root.go:

```go
package cmd

import (
	"fmt"

	homedir "github.com/mitchellh/go-homedir"
	"github.com/spf13/cobra"
	"github.com/spf13/viper"
)

var (
	// Used for flags.
	cfgFile     string
	userLicense string

	rootCmd = &cobra.Command{
		Use:   "cobra",
		Short: "A generator for Cobra based Applications",
		Long: `Cobra is a CLI library for Go that empowers applications.
This application is a tool to generate the needed files
to quickly create a Cobra application.`,
	}
)

// Execute executes the root command.
func Execute() error {
	return rootCmd.Execute()
}

func init() {
	cobra.OnInitialize(initConfig)

	rootCmd.PersistentFlags().StringVar(&cfgFile, "config", "", "config file (default is $HOME/.cobra.yaml)")
	rootCmd.PersistentFlags().StringP("author", "a", "YOUR NAME", "author name for copyright attribution")
	rootCmd.PersistentFlags().StringVarP(&userLicense, "license", "l", "", "name of license for the project")
	rootCmd.PersistentFlags().Bool("viper", true, "use Viper for configuration")
	viper.BindPFlag("author", rootCmd.PersistentFlags().Lookup("author"))
	viper.BindPFlag("useViper", rootCmd.PersistentFlags().Lookup("viper"))
	viper.SetDefault("author", "NAME HERE <EMAIL ADDRESS>")
	viper.SetDefault("license", "apache")

	rootCmd.AddCommand(addCmd)
	rootCmd.AddCommand(initCmd)
}

func initConfig() {
	if cfgFile != "" {
		// Use config file from the flag.
		viper.SetConfigFile(cfgFile)
	} else {
		// Find home directory.
		home, err := homedir.Dir()
		if err != nil {
			er(err)
		}

		// Search config in home directory with name ".cobra" (without extension).
		viper.AddConfigPath(home)
		viper.SetConfigName(".cobra")
	}

	viper.AutomaticEnv()

	if err := viper.ReadInConfig(); err == nil {
		fmt.Println("Using config file:", viper.ConfigFileUsed())
	}
}
```

### 创建 main.go

With the root command you need to have your main function execute it.
Execute should be run on the root for clarity, though it can be called on any command.


在 `Cobra` 应用程序中，通常 main.go 文件非常简单。它的目的就是是初始化 `Cobra`。

```go
package main

import (
  "{pathToYourApp}/cmd"
)

func main() {
  cmd.Execute()
}
```

### 创建子命令

`Cobra` 可以定义子命令。通常每个子命令都有各自的文件，会放在
cmd/ 目录中。

如果要创建显示版本的子命令，则可以创建文件 cmd/version.go 并
用以下内容填充它：

```go
package cmd

import (
  "fmt"

  "github.com/spf13/cobra"
)

func init() {
  rootCmd.AddCommand(versionCmd)
}

var versionCmd = &cobra.Command{
  Use:   "version",
  Short: "Print the version number of Hugo",
  Long:  `All software has versions. This is Hugo's`,
  Run: func(cmd *cobra.Command, args []string) {
    fmt.Println("Hugo Static Site Generator v0.9 -- HEAD")
  },
}
```

## 使用命令行参数

命令行参数提供参数以控制命令的操作方式。

### 分配参数给命令

由于命令行参数是在不同的位置定义和使用的，因此我们需要
在正确的范围里定义变量并将其分配给对应的参数。 

```go
var Verbose bool
var Source string
```

有两种不同的方法来分配命令行参数。

### Persistent Flags

命令行参数可以是"persistent"的，这意味着该参数可用于其分配的命令及其以下的子命令。对于全局命令行， 可以以 root 分配 persistent 参数。

```go
rootCmd.PersistentFlags().BoolVarP(&Verbose, "verbose", "v", false, "verbose output")
```

### Local Flags

命令行参数可以本地分配，也就是说它只能搭配特定的命令。

```go
localCmd.Flags().StringVarP(&Source, "source", "s", "", "Source directory to read from")
```

### Local Flag on Parent Commands

默认情况下，`Cobra` 仅解析目标命令上的本地参数，任何本地参数对于
父命令而言都将被忽略。通过启用 `Command.TraverseChildren` ，`Cobra` 将在执行目标命令之前，解析每个命令上的本地参数。

```go
command := cobra.Command{
  Use: "print [OPTIONS] [COMMANDS]",
  TraverseChildren: true,
}
```

### 通过配置设置命令行参数

你也可以通过 [viper](https://github.com/spf13/viper) 来绑定你的命令行参数：
```go
var author string

func init() {
  rootCmd.PersistentFlags().StringVar(&author, "author", "YOUR NAME", "Author name for copyright attribution")
  viper.BindPFlag("author", rootCmd.PersistentFlags().Lookup("author"))
}
```

在这个例子中，持久标记 `author` 通过 `viper` 进行绑定。
**注意**，当用户未提供 `--author` 参数时， `author` 将不会设置为config中的值。

更多有关 [viper文档](https://github.com/spf13/viper#working-with-flags) 。 

### Required flags

默认情况下，参数是可选的。相反，如果你希望当你没使用参数，命令会报错，可以将其设置为 required：
```go
rootCmd.Flags().StringVarP(&Region, "region", "r", "", "AWS region (required)")
rootCmd.MarkFlagRequired("region")
```

## 位置和自定义参数

可以使用命令的 `Args` 字段来指定位置参数的有效性。

下面的参数验证是内置的:

- `NoArgs` - the command will report an error if there are any positional args.
- `ArbitraryArgs` - the command will accept any args.
- `OnlyValidArgs` - the command will report an error if there are any positional args that are not in the `ValidArgs` field of `Command`.
- `MinimumNArgs(int)` - the command will report an error if there are not at least N positional args.
- `MaximumNArgs(int)` - the command will report an error if there are more than N positional args.
- `ExactArgs(int)` - the command will report an error if there are not exactly N positional args.
- `ExactValidArgs(int)` - the command will report an error if there are not exactly N positional args OR if there are any positional args that are not in the `ValidArgs` field of `Command`
- `RangeArgs(min, max)` - the command will report an error if the number of args is not between the minimum and maximum number of expected args.

设置参数验证的样例如下:

```go
var cmd = &cobra.Command{
  Short: "hello",
  Args: func(cmd *cobra.Command, args []string) error {
    if len(args) < 1 {
      return errors.New("requires a color argument")
    }
    if myapp.IsValidColor(args[0]) {
      return nil
    }
    return fmt.Errorf("invalid color specified: %s", args[0])
  },
  Run: func(cmd *cobra.Command, args []string) {
    fmt.Println("Hello, World!")
  },
}
```

## 样例

在下面的示例中，我们定义了三个命令。两个在最高层
一个（`cmdTimes`）是其中一个最高级命令的子命令。在这种情况下，根
命令不能满足需求，需要子命令来实现。 This is accomplished
by not providing a 'Run' for the 'rootCmd'.

我们只为一个命令定义了一个标志。

更多有关命令行参数的文档可在 https://github.com/spf13/pflag 获得。 


```go
package main

import (
  "fmt"
  "strings"

  "github.com/spf13/cobra"
)

func main() {
  var echoTimes int

  var cmdPrint = &cobra.Command{
    Use:   "print [string to print]",
    Short: "Print anything to the screen",
    Long: `print is for printing anything back to the screen.
For many years people have printed back to the screen.`,
    Args: cobra.MinimumNArgs(1),
    Run: func(cmd *cobra.Command, args []string) {
      fmt.Println("Print: " + strings.Join(args, " "))
    },
  }

  var cmdEcho = &cobra.Command{
    Use:   "echo [string to echo]",
    Short: "Echo anything to the screen",
    Long: `echo is for echoing anything back.
Echo works a lot like print, except it has a child command.`,
    Args: cobra.MinimumNArgs(1),
    Run: func(cmd *cobra.Command, args []string) {
      fmt.Println("Echo: " + strings.Join(args, " "))
    },
  }

  var cmdTimes = &cobra.Command{
    Use:   "times [string to echo]",
    Short: "Echo anything to the screen more times",
    Long: `echo things multiple times back to the user by providing
a count and a string.`,
    Args: cobra.MinimumNArgs(1),
    Run: func(cmd *cobra.Command, args []string) {
      for i := 0; i < echoTimes; i++ {
        fmt.Println("Echo: " + strings.Join(args, " "))
      }
    },
  }

  cmdTimes.Flags().IntVarP(&echoTimes, "times", "t", 1, "times to echo the input")

  var rootCmd = &cobra.Command{Use: "app"}
  rootCmd.AddCommand(cmdPrint, cmdEcho)
  cmdEcho.AddCommand(cmdTimes)
  rootCmd.Execute()
}
```
有关大型应用程序的更完整示例，请查看 [Hugo](http://gohugo.io/)。 


## 帮助命令

当应用程序有子命令时，Cobra 会自动将帮助命令添加到应用程序中。
当用户运行 `app help` 时，将调用此方法。此外，帮助子命令还
支持将其他命令作为输入。也就是说，当你有个子命令叫 `create`，并且该命令无任何其他配置， `Cobra` 将能够使用 `app help create`。每个命令都会自动添加 `--help` 参数。 

### 样例

以下输出是由 `Cobra` 自动生成的，并没有定义任何的子命令和参数。

```bash
$ cobra help
  Cobra is a CLI library for Go that empowers applications.
  This application is a tool to generate the needed files
  to quickly create a Cobra application.

  Usage:
    cobra [command]

  Available Commands:
    add         Add a command to a Cobra Application
    help        Help about any command
    init        Initialize a Cobra Application

  Flags:
    -a, --author string    author name for copyright attribution (default "YOUR NAME")
        --config string    config file (default is $HOME/.cobra.yaml)
    -h, --help             help for cobra
    -l, --license string   name of license for the project
        --viper            use Viper for configuration (default true)

  Use "cobra [command] --help" for more information about a command.
```

    
帮助命令和其他命令一样，并没有特殊的逻辑或行为。实际上，你可以根据自己的需要提供相对应的帮助。 


### 定义自己的帮助命令

你可以提供自己的帮助命令或模板，以供默认命令使用：

```go
cmd.SetHelpCommand(cmd *Command)
cmd.SetHelpFunc(f func(*Command, []string))
cmd.SetHelpTemplate(s string)
```
后面的两个函数对任何子命令有效。

## 使用信息
当用户提供无效的参数或无效的命令时，`Cobra` 会返回对应的`usage` 给用户。

### 样例
你可能从上面的帮助命令中意识到了这一点。因为默认的帮助命令
将用法嵌入为其输出的一部分。
```bash
  $ cobra --invalid
  Error: unknown flag: --invalid
  Usage:
    cobra [command]

  Available Commands:
    add         Add a command to a Cobra Application
    help        Help about any command
    init        Initialize a Cobra Application

  Flags:
    -a, --author string    author name for copyright attribution (default "YOUR NAME")
        --config string    config file (default is $HOME/.cobra.yaml)
    -h, --help             help for cobra
    -l, --license string   name of license for the project
        --viper            use Viper for configuration (default true)

  Use "cobra [command] --help" for more information about a command.
```

### 自定义用法
你可以提供自己的用法函数或模板以供 `Cobra` 使用。
就像帮助命令一样，函数和模板可以通过公共方法来重写：


```go
cmd.SetUsageFunc(f func(*Command) error)
cmd.SetUsageTemplate(s string)
```

## 版本参数

如果在根命令上设置 `Version` 字段，那么 `Cobra` 会添加一个顶级的 `--version`  参数。
使用 `--version` 参数运行应用程序将会将版本信息以特定的模板输出到标准输出 `stdout` 上。对应的模板可以用使用函数 `cmd.SetVersionTemplate(s string)` 个性化。


## 运行前和运行后的钩子

如果想在应用程序运行 `Run` 命令前或后执行其他函数也是可以的。`PersistentPreRun` 和 `PreRun`  函数是在 `Run` 之前运行的。 `PersistentPostRun` 和 `PostRun` 是在 `Run` 之后执行的。The `Persistent*Run` functions will be inherited by children if they do not declare their own. 这些函数是按照下面的顺序运行的：

- `PersistentPreRun`
- `PreRun`
- `Run`
- `PostRun`
- `PersistentPostRun`

下面是使用所有这些功能的两个命令的示例。当执行子命令时，它将运行根命令的 `PersistentPreRun`，而不是根命令的 `PersistentPostRun`。

```go
package main

import (
  "fmt"

  "github.com/spf13/cobra"
)

func main() {

  var rootCmd = &cobra.Command{
    Use:   "root [sub]",
    Short: "My root command",
    PersistentPreRun: func(cmd *cobra.Command, args []string) {
      fmt.Printf("Inside rootCmd PersistentPreRun with args: %v\n", args)
    },
    PreRun: func(cmd *cobra.Command, args []string) {
      fmt.Printf("Inside rootCmd PreRun with args: %v\n", args)
    },
    Run: func(cmd *cobra.Command, args []string) {
      fmt.Printf("Inside rootCmd Run with args: %v\n", args)
    },
    PostRun: func(cmd *cobra.Command, args []string) {
      fmt.Printf("Inside rootCmd PostRun with args: %v\n", args)
    },
    PersistentPostRun: func(cmd *cobra.Command, args []string) {
      fmt.Printf("Inside rootCmd PersistentPostRun with args: %v\n", args)
    },
  }

  var subCmd = &cobra.Command{
    Use:   "sub [no options!]",
    Short: "My subcommand",
    PreRun: func(cmd *cobra.Command, args []string) {
      fmt.Printf("Inside subCmd PreRun with args: %v\n", args)
    },
    Run: func(cmd *cobra.Command, args []string) {
      fmt.Printf("Inside subCmd Run with args: %v\n", args)
    },
    PostRun: func(cmd *cobra.Command, args []string) {
      fmt.Printf("Inside subCmd PostRun with args: %v\n", args)
    },
    PersistentPostRun: func(cmd *cobra.Command, args []string) {
      fmt.Printf("Inside subCmd PersistentPostRun with args: %v\n", args)
    },
  }

  rootCmd.AddCommand(subCmd)

  rootCmd.SetArgs([]string{""})
  rootCmd.Execute()
  fmt.Println()
  rootCmd.SetArgs([]string{"sub", "arg1", "arg2"})
  rootCmd.Execute()
}
```

Output:
```
Inside rootCmd PersistentPreRun with args: []
Inside rootCmd PreRun with args: []
Inside rootCmd Run with args: []
Inside rootCmd PostRun with args: []
Inside rootCmd PersistentPostRun with args: []

Inside rootCmd PersistentPreRun with args: [arg1 arg2]
Inside subCmd PreRun with args: [arg1 arg2]
Inside subCmd Run with args: [arg1 arg2]
Inside subCmd PostRun with args: [arg1 arg2]
Inside subCmd PersistentPostRun with args: [arg1 arg2]
```

## 发生 `unknown command` 的建议

当发生 `unknown command` 的错误时，`Cobra` 将自动给出建议。这使得 `Cobra` 能够像 `git` 命令碰到错误输入的反应一样。例如：


```
$ hugo srever
Error: unknown command "srever" for "hugo"

Did you mean this?
        server

Run 'hugo --help' for usage.
```

命令给出的提示是根据每个子命令注册时实现的 [Levenshtein distance]（http://en.wikipedia.org/wiki/Levenshtein_distance）接口。每个匹配最小距离为2（忽略大小写）的注册命令都将显示为建议。

如果需要禁用提示或在命令中调整字符串距离，请使用：

```go
command.DisableSuggestions = true
```

或者

```go
command.SuggestionsMinimumDistance = 1
```

你也可以使用 `SuggestFor` 属性来显式设置提示以供给定的命令使用 。这样就可以给出字符串距离不是很紧密的提示，但是在你的命令集中以及不希望使用别名的命令中都是有意义的。例： 

```
$ kubectl remove
Error: unknown command "remove" for "kubectl"

Did you mean this?
        delete

Run 'kubectl help' for usage.
```

## 生成命令文档

Cobra 基于子命令，命令参数等生成文档可以can generate docu。生成的对应文档见下面：

- [Markdown](doc/md_docs.md)
- [ReStructured Text](doc/rest_docs.md)
- [Man Page](doc/man_docs.md)

## 生成 bash 自动补全

`Cobra` 可以生成 bash 补全文件。如果你在命令中添加更多信息，这些补全功能将非常强大和灵活。你可以在[Bash Completions](bash_completions.md)中阅读有关它的更多信息

Cobra can generate a bash-completion file. If you add more information to your command, these completions can be amazingly powerful and flexible.  Read more about it in [Bash Completions](bash_completions.md).

## 生成 zsh 自动补全

Cobra 可以生成一个 zsh 补全文件。 更多的信息可以读[Zsh Completions](zsh_completions.md).

# 贡献

1. 在github上 Fork 它
2. 下载你github上的源码到你电脑上 (`git clone https://github.com/your_username/cobra && cd cobra`)
3. 创建你的新分支 (`git checkout -b my-new-feature`)
4. 进行更改并添加它们 (`git add .`)
5. 提交你的变更 (`git commit -m 'Add some feature'`)
6. 将变更推送到github厂库 (`git push origin my-new-feature`)
7. 创建一个 pull request

# 许可证

Cobra 是基于 Apache 2.0 许可证的. 详情请看 [LICENSE.txt](https://github.com/spf13/cobra/blob/master/LICENSE.txt)
