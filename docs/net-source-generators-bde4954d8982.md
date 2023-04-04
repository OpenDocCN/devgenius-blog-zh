# 。净源发电机

> 原文：<https://blog.devgenius.io/net-source-generators-bde4954d8982?source=collection_archive---------0----------------------->

![](img/ebb50cb126166ab58f35f50a9318bd7d.png)

# 代码生成

代码生成是在整个解决方案中一致地应用模式，或者基于一些外部输入文件创建多个相似的类，甚至是同一个解决方案中的其他类的好方法。这些年来工具发生了变化，但结果是一样的…许多代码我不必手写，也不必在模式变化和发展时更新。

从早期开始，我就一直在生成源代码。Net 1.0，当时我们使用一些插件，根据 Visio 中绘制的类图生成源代码。成功了，但这是一次单程旅行，因为。Net 还没有分部类。我们很快就把它丢在了 Borland 的一个叫做“Together”的工具后面，这个工具可以让我们的代码和图表保持同步，就像你直接在任何一部分上工作一样。这是一个很棒的工具，但是当 2.0 版本的价格突然飙升时，它就消失了。

然后，在 Visual Studio 2008 时间框架的某个地方，我发现了 T4 模板。一位同事编写了一个命令行程序，围绕 Linq to SQL 上下文生成可模仿的测试垫片，我对它进行了修改，使其在 T4 模板中运行，这样我们就不必一直手动运行它，并在每次对模型进行更改时手动将结果复制到我们的解决方案中。这些模板作为工件存储在我们的解决方案中，并直接将代码添加到我们的项目中，这是一个巨大的胜利。我迷上了这种方法，并使用它来消除其他重复的编码任务，例如创建测试支持的构建器类，这仍然是我今天的主要用例。

随着的发布，T4 模板变得难以为继。Net Core，或者至少是我使用它们的方式，因为我所依赖的反射代码不再工作了。T4 模板运行在完整的框架下，不能反射。Net 核心代码，或者至少他们当时不能。

我转而使用 Roslyn 模板和一个名为“Scripty.msbuild”的项目，在构建时对它们进行转换。的。驱动这种方法的 csx 文件做得很好，我对它们的工作方式很满意。我可以保持我的基于反射的代码基本不变，因为我只是换出了 runner。不幸的是，Scripty 在 2017 年或多或少被抛弃了，当它也开始出现一些问题时，我需要一个新的替代方案。

接下来，我尝试使用 TypeWriter 来生成我的构建器代码。TypeWriter 是为了用。Net 代码，但它可以用来创建任何类型的文本文件。尽管如此，它显然专注于生成 JavaScript 和 TypeScript，所以需要做更多的工作来生成。Net 代码。

最近，我又开始使用 T4，但是没有了反射部分。T4 发动机仍然工作得很好。如果我们要在. Net 核心项目中使用它，我们只需要一个不同于反射的输入。我目前的项目是使用外部设计师来创建业务实体，就像 Visio 和 Together 以前所做的那样。来自这个设计器的 XML 文件被用作模板的输入，而不是反射，所以 T4 似乎工作得很好。

这很好，解决了我们在新代码中的问题，但不祥之兆似乎很清楚，T4 模板不是微软有兴趣进一步开发的东西，我想它们会在某个时候完全过时，现在我们已经过时了。净源发电机。

源代码生成器在 Visual Studio 中运行，可以在您工作时实时生成代码，这使得它们比过去的系统更容易使用。它们被实现为 Visual Studio analyzers，您通常使用它来做一些事情，例如在代码中添加弯曲的下划线来建议更改，甚至可能实现“快速修复”来为您进行这些更改。源代码生成器更进了一步，让您创建全新的*代码，成为项目的一部分，而不仅仅是在编译时。生成器创建的代码立即可用。您可以在自己的类中引用生成的代码，甚至在同一个项目中，而不必先重新编译所有内容。*

尽管并不完美。该机制仍然相当年轻，Visual Studio 体验还处于早期阶段，所以*创建*源生成器的过程还不完全顺利，但是一旦*编写了*自己的生成器，使用的*行为就相当轻松了。*

# 规划

与直觉相反，生成代码的第一步是手工编写代码。首先，试着写下你想要生成的东西，并确保它按照你期望的方式工作。在尝试自动化之前，找出任何缺陷或问题，并进行测试。在创建工厂进行大规模生产之前，仔细考虑一下这个想法。写代码通常比写能写代码的代码更容易。

生成代码的理想候选模式是您已经确定下来并且厌倦了一遍又一遍编写的模式。如果你发现自己又在说“我再也不想写另一个{东西}”，那么这个东西可能是一代人的理想候选。

随着您的进展，将您生成的输出与您开始使用的示例类进行比较，以便识别出不太正确的部分或者您还没有完成的部分。一次处理一个特性，当您的生成器能够输出与您手写的相同(或足够相似)的代码时，您就知道您完成了。

在这个演示中，我不会让你实时观察整个系统的发展过程，所以我会继续前进。只需知道这是我的例子是如何发展起来的，一次一件。

# 第一步

创建一个新的类库项目来保存源代码生成器本身。我称我的为“建筑发电机”。这个项目不需要以特定的框架版本为目标，所以在创建项目时选择“netstandard2.0 ”,或者更改 csproj 文件中的 TargetFramework 节点，如下所示。在我写这篇文章的时候，你还必须指定语言版本为“预览”，尽管当你读到这篇文章的时候，情况可能并不是这样。

此外，您还需要 Microsoft 的参考资料。CSharp 和微软。代码分析。分析器包。您可以通过 Visual Studio 的 NuGet 包管理器(可能会找到较新的版本)来完成此操作，或者您可以从下面的示例中复制并粘贴这些条目，然后查找更新。选择权在你。

```
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
    <LangVersion>preview</LangVersion>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.CodeAnalysis.Analyzers" Version="3.3.2" PrivateAssets="All" />
    <PackageReference Include="Microsoft.CodeAnalysis.CSharp" Version="3.9.0" PrivateAssets="All" />
  </ItemGroup>

</Project>
```

请记住在继续之前保存您的项目文件，否则 Visual Studio 可能会感到困惑。该项目现在可以用作源代码生成器。我们只需要填写一些东西让它生成。让我们创建我们的“BuilderGenerator”类。您可以随意调用这个类，但是我将把它命名为项目和解决方案，只是为了简单起见。用“微软”装饰它。“CodeAnalysis.Generator”属性，实现“ISourceGenerator”接口，您应该得到如下结果。

```
using System;
using Microsoft.CodeAnalysis;

namespace BuilderGenerator
{
    [Generator]
    public class BuilderGenerator : ISourceGenerator
    {
        public void Initialize(GeneratorInitializationContext context)
        {
            throw new NotImplementedException();
        }

        public void Execute(GeneratorExecutionContext context)
        {
            throw new NotImplementedException();
        }
    }
}
```

与其在运行时抛出异常(这不会让我们走得很远)，不如让我们开始构建一些代码。我们将从小处开始，生成一个说“你好，世界”的类。

```
using System.Text;
using Microsoft.CodeAnalysis;
using Microsoft.CodeAnalysis.Text;

namespace BuilderGenerator
{
    [Generator]
    public class BuilderGenerator : ISourceGenerator
    {
        public void Initialize(GeneratorInitializationContext context)
        {
            // Nothing to do... yet.
        }

        public void Execute(GeneratorExecutionContext context)
        {
            var greetings = @"using System;

namespace Generated
{
    public class Greetings
    {
        public static HelloWorld()
        {
            return ""Hello World!"";
        }
    }
}";

            context.AddSource("Greetings.cs", SourceText.From(greetings, Encoding.UTF8));
        }
    }
}
```

这项工作是在“执行”方法中完成的，该方法只是将包含整个问候语类内容的字符串输出到一个文件中。这是最简单的代码生成形式，只需将预定义的内容添加到使用生成器的项目中。

# 使用源生成器

现在我们有了一个源代码生成器，虽然很无聊，但是让我们把它添加到一个项目中，看看它是如何运行的。向解决方案中添加第二个项目。我称我的为“域名”。它将取代典型的领域层，并定义一些示例实体。这个项目可以像往常一样安全地瞄准特定的核心框架版本。编辑生成的 csproj 文件，如下所示:

```
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netcoreapp3.1</TargetFramework>
    <LangVersion>preview</LangVersion>
  </PropertyGroup>

  <ItemGroup>
    <ProjectReference Include="..\BuilderGenerator\BuilderGenerator.csproj" OutputItemType="Analyzer" ReferenceOutputAssembly="false" />
  </ItemGroup>

</Project>
```

请注意，对 BuilderGenerator 项目的引用有一些附加属性。第一个是告诉 Visual Studio 被引用的项目是一个分析器，而不是一个库。第二个属性“ReferenceOutputAssembly = 'false '”更明确地说明了这一点。到目前为止，您不能将一个项目同时作为一个分析器和一个库来引用，尽管我希望这种情况在将来会有所改变，因为它会使像基类或属性这样的东西更容易实现。

这个新的分析器将生成存在于领域项目本身中的代码，尽管您看到它的能力可能是有限的。对查看生成代码的支持一直在改进，因此根据您的 Visual Studio 安装版本，它可能会也可能不会让您轻松地浏览到生成的代码。但是，您可以从其他类和/或项目中引用它，所以让我们看看是否能找到它。将单元测试项目添加到解决方案中。我将把我的称为“单元测试”。添加对域项目的引用，然后创建我们的第一个测试，名为“GreetingsTests”。

```
using NUnit.Framework;

namespace UnitTests
{
    [TestFixture]
    public class GreetingsTests
    {
        [Test]
        public void SayHello_returns_expected_string()
        {
            Assert.AreEqual("Hello World!", Generated.Greetings.HelloWorld());
        }
    }
}
```

根据您的 Visual Studio 版本，此时您可能会看到一些 Intellisense 错误，并且可能需要重新启动 Visual Studio 才能继续。在我看来，Visual Studio 在加载解决方案时会加载一次分析器，并且会在该会话的其余部分继续使用该分析器。这可能或可能不准确地描述了幕后真正发生的事情，但这肯定是我观察到的效果。每当我对生成器本身进行更改时，我都必须重新启动 Visual Studio 来利用这些更改。

一旦发电机完成，你应该可以毫无问题地使用它，但是在发电机*本身*上工作的过程可能有点麻烦。如果您遇到此问题，请重新启动 Visual Studio，Intellisense 应该会再次开始工作，包括自动完成。希望这些错误会在以后的版本中消失，当你读到这篇文章时，它可能已经被一劳永逸地解决了。

# 生成实代码

现在我们有了一个工作的源代码生成器，让我们让它做一些比返回“Hello World”更有用的事情。我在过去已经发布过几次关于我对构建器模式的热爱，以及它如何帮助你的测试更容易编写和阅读，更清楚地向其他开发人员传达他们的意图。因为这是一个我已经使用。这似乎是我第一个基于源代码生成器的项目的自然选择。

先说简单的；对于每个实体类(例如 User)，我们希望生成一个相应的“Builder”类(例如 UserBuilder)。内容还不重要，只是我们可以生成 builder 类，我们可以从同一个项目中的其他类或者从测试中引用它。

首先，我们将创建一个属性来标记我们想要为其生成构建器的类。不幸的是，我们不能只在 BuilderGenerator 项目本身中定义属性。我们的域项目引用了 BuilderGenerator 项目，但是它是作为一个分析器而不是库来做的，所以我们不能直接使用那里定义的类。相反，我们可以创建另一个项目来容纳属性和其他公共元素，如基类，或者我们可以让生成器简单地输出属性类，就像我们在上面生成 Greetings 类一样。事实上，我们可以简单地替换 BuilderGenerator 类的现有内容来发出这个新的属性类。

```
using System.Text;
using Microsoft.CodeAnalysis;
using Microsoft.CodeAnalysis.Text;

namespace BuilderGenerator
{
    [Generator]
    public class BuilderGenerator : ISourceGenerator
    {
        public void Initialize(GeneratorInitializationContext context)
        {
            // Nothing to do... yet.
        }

        public void Execute(GeneratorExecutionContext context)
        {
            var attribute = @"namespace BuilderGenerator
{
    [System.AttributeUsage(System.AttributeTargets.Class)]
    public class GenerateBuilderAttribute : System.Attribute
    {
    }
}";

            context.AddSource("GenerateBuilderAttribute.cs", SourceText.From(attribute, Encoding.UTF8));
        }
    }
}
```

我们将重用同样的机制来生成我们需要的任何其他属性或基类。

接下来，在域项目中，创建一个文件夹来保存我们的实体定义。我把我的叫做“实体”。在“Entities”文件夹中创建一个名为“User”的新实体，赋予它几个属性，并用“GenerateBuilder”属性修饰它，如果一切正常，这个属性应该已经可用了。

```
using BuilderGenerator;

namespace Domain.Entities
{
    [GenerateBuilder]
    public class User
    {
        public string FirstName { get; set; }
        public string LastName { get; set; }
    }
}
```

同样，在这个阶段，内容并不重要。我们只是想要一些可以生成构建器的东西。由于这不是一个关于 Roslyn 语法的教程，所以我不会在这里深入讨论它的工作原理，但是我们将需要一个所谓的“SyntaxReceiver ”,它充当一种过滤器，对表示我们项目所有代码的语法树进行排序，并将我们感兴趣的部分转发给我们的生成器。添加一个名为“buildergeneratorsyntaxereceiver”的新类，其内容如下。

```
using System.Collections.Generic;
using System.Linq;
using Microsoft.CodeAnalysis;
using Microsoft.CodeAnalysis.CSharp.Syntax;namespace Generator
{
    internal class BuilderGeneratorSyntaxReceiver : ISyntaxReceiver
    {
        public List<ClassDeclarationSyntax> Classes { get; } = new(); public void OnVisitSyntaxNode(SyntaxNode syntaxNode)
        {
            if (syntaxNode is ClassDeclarationSyntax classDeclaration && classDeclaration.AttributeLists.Any(x => x.Attributes.Any(a => a.Name + "Attribute" == "GenerateBuilderAttribute"))) Classes.Add(classDeclaration);
        }
    }
}
```

这个名字实际上并不重要，但是我们将根据生成器来命名它，以保持它的清晰。该过滤器在语法树中查找带有 GenerateBuilder 属性的所有类，并将它们添加到一个名为“classes”的列表中。

替换 BuilderGenerator 的内容。使用以下内容执行方法:

```
public void Execute(GeneratorExecutionContext context)
        {
            if (context.SyntaxReceiver is not BuilderGeneratorSyntaxReceiver receiver) return;

            foreach (var targetClass in receiver.Classes.Where(x => x != null))
            {
                var targetClassName = targetClass.Identifier.Text;
                var targetClassFullName = targetClass.FullName();

                var builderClassNamespace = targetClass.Namespace() + ".Builders";
                var builderClassName = $"{targetClassName}Builder";
                var builderClassUsingBlock = ((CompilationUnitSyntax) targetClass.SyntaxTree.GetRoot()).Usings.ToString();
                var builder = $@"using System;
using System.CodeDom.Compiler;
{builderClassUsingBlock}
using Domain.Entities;

namespace {builderClassNamespace}
{{
    public partial class {builderClassName} : Builder<{targetClassName}>
    {{
        // TODO: Write the actual Builder
    }}
}}";

                context.AddSource($"{targetClassName}Builder.cs", SourceText.From(builder, Encoding.UTF8));
            }
        }
```

这里发生了一些事情。首先，我们确保我们只关注通过我们之前定义的 SyntaxReceiver 传递的类。对于通过过滤器的每个类，我称之为“目标类”，我们提取一些基本信息，并将其注入到模板中，以便创建一个构建器类，然后将其添加到消费项目中。这个类现在是空的，因为我们的第一个目标是为每个修饰的实体类生成一个构建器。

因为我们已经对生成器本身进行了更改，所以在 Visual Studio 开始使用新版本之前，您可能需要重新启动它，但是如果一切正常，一个名为 UserGenerator 的新类将被添加到域项目中。就像属性一样，这个文件不会出现在解决方案资源管理器中，但是它*应该*出现在类视图中的 Domain 下。如果双击打开它，您应该会看到一个新的“UserBuilder”类，带有一个注释“TODO: Write the actual Builder”。

您已经可以在其他代码中引用该类，例如在单元测试中。删除旧的 GreetingsTests 类，并替换为新的“BuilderTests”类，如下所示:

```
using Domain.Entities.Builders;
using NUnit.Framework;

namespace UnitTests
{
    [TestFixture]
    public class BuilderTests
    {
        [Test]
        public void UserBuilder_exists()
        {
            var actual = new UserBuilder();
            Assert.IsInstanceOf<UserBuilder>(actual);
        }
    }
}
```

现在我们已经生成了构建器的框架，我们将开始填充使其工作的部分，从目标类的公共属性的支持属性开始。我们首先从目标类中提取可设置属性的列表，并为每个属性添加一个属性到构建器中。在 Execute 方法中的 targetClassFullName 声明之后添加以下内容。

```
var targetClassProperties = targetClass.DescendantNodes()
    .OfType<PropertyDeclarationSyntax>()
    .Where(x => x.IsInstance() && x.HasSetter())
    .ToArray();
```

然后，将 TODO 注释替换为对名为“BuildProperties”的新方法的调用。

```
public partial class {builderClassName} : Builder<{targetClassName}>
    {{
        {BuildProperties(targetClassProperties)}
    }}
```

最后，如下定义 BuildProperties 方法。

```
private static string BuildProperties(IEnumerable<PropertyDeclarationSyntax> properties)
        {
            var result = string.Join(Environment.NewLine,
                properties.Select(x =>
                    {
                        var propertyName = x.Identifier.ToString();
                        var propertyType = x.Type.ToString(); return @"        public Lazy<{propertyType}> {propertyName} = new Lazy<{propertyType}>(() => default({propertyType}));"
                    })); return result;
        }
```

我喜欢让我的后台属性懒惰<t>,因为它们不仅仅保存已完成对象的最终属性值。建造者代表*计划*如何*建造*那个物体，而计划是可以改变的。使用惰性值意味着设置最终值的代码直到在运行时构建对象后才会运行。</t>

# 后续步骤

这里我不打算让您通读我的构建器模式的整个发展，因为这只是一个特定的用例，但是下一步将添加一个名为“Build”的方法来创建最终的对象，以及一组方便的方法来支持流畅的接口。如果你想看组成我自己的 BuilderGenerator 的代码，你可以在它的 [GitHub 页面](https://github.com/MelGrubb/BuilderGenerator)上找到项目源代码。

有许多不同的构建器类的例子，有些比其他的更复杂，但这就是代码生成的美妙之处。如果我扩展我的模式，添加新的特性，我只需要更新模板，这些变化将反映在我所有的类中，甚至是我所有的项目中。

我在这里展示的内容演示了源生成器的基本框架，以及如何使用项目中的现有代码来驱动该生成器的输出。从这里开始，您将一次迭代地添加一个特性，直到生成的代码与您开始的手写示例相匹配。您可以按照这种模式在项目中生成任何类型的高度重复的代码。

## **-梅尔格拉布，软件开发团队负责人**在 [AWH](http://awh.net) 。我们正在帮助企业通过技术推动增长。