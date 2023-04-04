# (Java)用 300 行代码编写 JSON 解析器

> 原文：<https://blog.devgenius.io/java-writing-json-parser-in-300-lines-of-code-c3c1711acba6?source=collection_archive---------13----------------------->

## 这是用 Java 代码进行自顶向下解析的简短介绍

如果大多数人不认为编写编译器(和解析器)是一件苦差事，世界上可能会有更多的编程语言。我也分享了这个信念很长一段时间，直到我来到这个伟大的系列。这一点和最近缺乏编程挑战激发了我写这篇文章。

> **解析器**是一个软件组件，它获取输入数据(通常是文本)并构建[数据结构](https://en.wikipedia.org/wiki/Data_structure)——通常是某种[解析树](https://en.wikipedia.org/wiki/Parse_tree)、[抽象语法树](https://en.wikipedia.org/wiki/Abstract_syntax_tree)或其他层次结构，在检查语法是否正确的同时给出输入的结构表示。—维基百科

![](img/39e52442b6b3a35b68d3eefaee67d14b.png)

丹尼尔·施鲁迪在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在这篇文章中，我想展示以自顶向下的方式解析抽象语法树就像一堆方法在输入缓冲区中递归地互相调用一样简单。

# JSON

JSON 的简单性导致它出现在大多数微服务架构中。它的语法也非常简单。如此简单，以至于很容易在一天内完成解析器。

表达语法的常见方式是[巴克斯-诺尔形式](https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form)，这里是针对 JSON 的:

```
json ::= element

value ::= object | array | string | number | 'true' | 'false' | 'null'

object ::= '{' ws '}' | '{' members '}'

members ::= member | member ',' members

member ::= ws string ws ':' element

array ::= '[' ws ']' | '[' elements ']'

elements ::= element | element ',' elements

element ::= ws value ws

string ::= '"' characters '"'

characters ::= "" | character characters

character ::= <any printable character>

number ::= integer fraction exponent

integer ::= digit | onenine digits | '-' digit | '-' onenine digits

digits ::= digit | digit digits

digit ::= '0' | onenine

onenine ::= '1' . '9'

fraction ::= "" | '.' digits

exponent ::= "" | 'E' sign digits | 'e' sign digits

sign ::= "" | '+' | '-'

ws ::= <any whitespace character>
```

# 密码

解析的结果是嵌套结构的树。我们将借助多态来表示元素(想想[复合模式](https://refactoring.guru/design-patterns/composite))。每个元素都继承自共同的祖先，其中一些元素还可以包含其他元素。从 Java 14 开始，用记录表示这些结构变得非常容易。

```
public interface Element {
}

public record Object(Map<String, Element> members) implements Element {
}

public record Array(List<Element> elements) implements Element {
}

public record Str(String value) implements Element {
}

public record Number(String value) implements Element {
}

public record True() implements Element {
}

public record False() implements Element {
}

public record Null() implements Element {
}
```

大多数课程建议首先对输入进行标记化，然后在第二阶段对标记进行解析。但是为了简单起见，让我们将这两个阶段合并为一个阶段。除了设计的主要工作之外…

```
public static class ParseResult {

        public final int offset;
        public final Element content;
        public final boolean isError;
        public final boolean isEOF;
        private final int snapshot;

        public ParseResult(int offset) {
            this(offset, offset, null, false, false);
        }

        private ParseResult(int offset, int snapshot, Element content, 
                            boolean isEOF, boolean isError) {
            this.offset = offset;
            this.snapshot = snapshot;
            this.content = content;
            this.isEOF = isEOF;
            this.isError = isError;
            this.map = new HashMap<>();
            this.list = new ArrayList<>();
        }
}
```

这是所有奇迹发生的班级。而且是一个[单子](https://medium.com/@afcastano/monads-for-java-developers-part-1-the-optional-monad-aa6e797b8a6e)。简而言之，它是一种同时保存多个(矛盾的)值的容器类型，但是对所有这些值的方法调用都是相同的。

因此，与其用 ifs 检查它是否是一个错误或者文件的结尾，不如让两个元素，isError 和 isEOF 都嵌入到 monad 中，只是将一些解析调用链接在一起。

```
 public ParseResult acceptChar(char[] buffer, char ch) {
      if (isError || isEOF) return error();
      if (!isChar(buffer, ch)) return error();
      return advance(buffer, 1);
  }

  public ParseResult advance(char[] buffer, int plus) {
      if (isError || isEOF) return this;
      return new ParseResult(offset + plus, snapshot, 
              content, isEOF(buffer, plus), false);
  }

  public ParseResult acceptZeroOrMany(char[] buffer, 
                                      Predicate<Character> pred) {
      if (isError || isEOF) return this;
      int plus = 0;
      while (!isEOF(buffer, plus) 
              && pred.test(buffer[this.offset + plus])) plus++;
      return new ParseResult(this.offset + plus, snapshot, 
              content, isEOF(buffer, plus), false);
  }

  public boolean isEOF(char[] buffer, int plus) {
      return this.offset + plus >= buffer.length;
  }

  public boolean isChar(char[] buffer, Predicate<Character> pred) {
      if (isError || isEOF) return false;
      return pred.test(buffer[this.offset]);
  }

  public boolean isChar(char[] buffer, char c) {
      return isChar(buffer, ch -> ch == c);
  }
```

以上是在整个解析器中使用的助手函数。可以把它们看作解析逻辑的最基本的构件。请注意，它们在前奏中都有快速失效开关。我认为这是不言自明的——我们不断地从模式中接受字符，直到我们遇到文件的结尾。

```
ParseResult parseTrue(char[] buffer) {
    return parseLiteral(buffer, "true", True::new);
}

ParseResult parseNull(char[] buffer) {
    return parseLiteral(buffer, "null", Null::new);
}

ParseResult parseFalse(char[] buffer) {
    return parseLiteral(buffer, "false", False::new);
}

private ParseResult parseLiteral(char[] buffer, String label, 
                                         Supplier<Element> ctor) {
      if (isEOF || isError) return this;

      var pr = advance(buffer, label.length());

      if (!pr.isError && pr.token(buffer).equals(label))
          return new ParseResult(pr.offset, pr.snapshot, 
                  ctor.get(), pr.isEOF, false);
      else return error();
 }

private String token(char[] buffer) {
    return new String(buffer, this.snapshot, 
        Math.min(this.offset, buffer.length) - this.snapshot);
}
```

JSON 中有 3 种文字类型:true、false 和 null。只要用文字长度推进偏移量并检查相应的令牌(从快照到偏移量)是否与字符串匹配就足够了。

```
 ParseResult parseString(char[] buffer) {
      if (isEOF || isError) return this;

      return acceptChar(buffer, '"')
              .acceptZeroOrMany(buffer, c -> c != '"')
              .acceptChar(buffer, '"')
              .token(buffer, Str::new);
  }

  ParseResult parseNumber(char[] buffer) {
      if (isEOF || isError) return this;

      var o = this;
      if (o.isChar(buffer, '-')) o = o.acceptChar(buffer, '-');

      if (o.isError || o.isEOF) return error();

      if (o.isChar(buffer, '0')) {
          return o.acceptChar(buffer, '0').token(buffer, Number::new);
      } else if (o.isChar(buffer, Character::isDigit)) {
          o = o.acceptZeroOrMany(buffer, Character::isDigit);
      } else return error();

      if (o.isChar(buffer, '.')) {
          o = o.acceptChar(buffer, '.')
                  .acceptZeroOrMany(buffer, Character::isDigit);
      }

      if (o.isChar(buffer, 'E') || o.isChar(buffer, 'e')) {
          o = o.advance(buffer, 1);
          if (o.isChar(buffer, '-')) {
              o = o.advance(buffer, 1);
          }
          o = o.acceptZeroOrMany(buffer, Character::isDigit);
      }

      return o.token(buffer, Number::new);
  }

  ParseResult token(char[] buffer, Function<String, Element> ctor) {
      if (isError) return this;
      return new ParseResult(offset, snapshot, 
              ctor.apply(token(buffer).replace("\"", "")), 
              isEOF, false);
  }
```

现在我们看到了字符串和数字的更复杂的规则。数字可以是整数，也可以是带可选符号的小数。因此，如果括号是可选的，它应该是这样的:[-]10[.12[e30]]。

```
 public static class ParseResult {

        public final int offset;
        public final Element content;
        public final boolean isError;
        public final boolean isEOF;
        private final Map<String, Element> map;  // + 
        private final List<Element> list;  // +
        private final int snapshot;
}

ParseResult parseObject(char[] buffer) {
      if (isEOF || isError) return this;

      var o = acceptChar(buffer, '{')
              .acceptZeroOrMany(buffer, Character::isWhitespace);

      if (o.isChar(buffer, '}')) {
          return o.acceptChar(buffer, '}').container(() -> new Object(map));
      }

      o = o.parseMember(buffer, map);

      while (!o.isEOF && !o.isError && !o.isChar(buffer, '}')) {
          o = o.acceptChar(buffer, ',').parseMember(buffer, map);
      }
      o = o.acceptChar(buffer, '}');

      return o.container(() -> new Object(map));
  }

  ParseResult parseArray(char[] buffer) {
      if (isEOF || isError) return this;

      var o = acceptChar(buffer, '[')
              .acceptZeroOrMany(buffer, Character::isWhitespace);
      if (o.isChar(buffer, ']')) {
          return o.acceptChar(buffer, ']').container(() -> new Array(list));
      }

      o = o.parseElement(buffer, list);

      while (!o.isEOF && !o.isError && !o.isChar(buffer, ']')) {
          o = o.acceptChar(buffer, ',').parseElement(buffer, list);
      }
      return o.acceptChar(buffer, ']').container(() -> new Array(list));
  }

  ParseResult parseElement(char[] buffer, List<Element> list) {
      if (isEOF || isError) return this;

      var o = acceptZeroOrMany(buffer, Character::isWhitespace)
              .element(buffer, ParseResult::new)
              .acceptZeroOrMany(buffer, Character::isWhitespace);
      if (!o.isError && list != null) list.add(o.content);
      return o;
  }

  ParseResult parseMember(char[] buffer, Map<String, Element> map) {
      if (isEOF || isError) return this;

      String key;
      Element value;

      var o = acceptZeroOrMany(buffer, Character::isWhitespace)
              .snapshot().parseString(buffer);
      key = ((Str) o.content).value;

      o = o.acceptZeroOrMany(buffer, Character::isWhitespace)
              .acceptChar(buffer, ':').parseElement(buffer, null);
      value = o.content;

      if (!o.isError && map != null) {
          map.put(key, value);
      }
      return o;
  }

 ParseResult element(char[] buffer, Function<Integer, ParseResult> offset) {
      if (isError) return this;
      return offset.apply(this.offset).parseValue(buffer);
  }

 ParseResult container(Supplier<Element> ctor) {
      if (isError) return this;
      return new ParseResult(offset, snapshot, ctor.get(), isEOF, false);
  }
```

所以对象只是键值对的集合，数组是值的集合。这里的技巧是扩展一个 monad 来保存 Java 列表和映射。剩下的就不言自明了。

```
 ParseResult parseValue(char[] buffer) {
      if (isEOF || isError) return this;

      return parseTrue(buffer)
              .orElse(parseFalse(buffer))
              .orElse(parseNull(buffer))
              .orElse(parseString(buffer))
              .orElse(parseNumber(buffer))
              .orElse(parseArray(buffer))
              .orElse(parseObject(buffer)).
              orElse(new ParseResult(offset).error());
  }

  public ParseResult orElse(ParseResult other) {
      return isError ? other : this;
  }
```

还记得上一个示例中的 parseValue 吗？在这里！现在你可以看到整个语法的递归性。因为元素可以是这些变体中的任何一种，所以容器类型可以在其中嵌套其他容器类型。

```
 public Element read(String text) {
    char[] buffer = text.toCharArray();

    var result = new ParseResult(0).parseElement(buffer, null);
    if (result.isError || !result.isEOF) {
        throw new RuntimeException("Json parsing exception");
    }
    return result.content;
}

public String write(Element tree) {
    return switch (tree) {
        case True ignored -> "true";
        case False ignored -> "false";
        case Null ignored -> "null";
        case Number n -> n.value;
        case Str s -> "\"" + s.value + "\"";
        case Array a -> {
            var join = new StringJoiner(",", "[", "]");
            a.elements.forEach(e -> join.add(write(e)));
            yield join.toString();
        }
        case Object o -> {
            var join = new StringJoiner(",", "{", "}");
            o.members.forEach((k, v) -> join.add(k + ":" + write(v)));
            yield join.toString();
        }
        default -> throw new RuntimeException("Unknown tree element");
    };
}
```

终于锦上添花了。

# 结论

就这样，你成功了。这个帖子至少有两个要点。首先，编写解析器非常简单。当然，递归没有在循环中处理令牌那么快，但我认为解决方案的简单性仍然是优势。

第二个解析器的逻辑有点混乱。但是使用 monad，您可以封装检查并快速失败。那么剩下的逻辑就是调用链接在一起的问题。

你可以在这里获得全部代码[。](https://gist.github.com/dgawlik/c4c608e29f4757be8b004d67cc17ce32)

谢谢！