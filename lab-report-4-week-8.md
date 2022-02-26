# Lab Report 4

[My group's repository](https://github.com/vs2961/markdown-parse)

[Reviewed repository](https://github.com/CatFish47/markdown-parse)

## Test Case 1

Test code:
```
@Test
public void labTestOne() throws IOException {
    Path file = Path.of("lab-test-1.md");
    String contents = Files.readString(file);

    List<String> expected = List.of("`google.com", "google.com", "ucsd.edu");
    assertEquals(expected, MarkdownParse.getLinks(contents));
}
```

For my implementation, the test fails:
```
1) labTestOne(MarkdownParseTest)
java.lang.AssertionError: expected:<[`google.com, google.com, ucsd.edu]> but was:<[url.com, `google.com, google.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.labTestOne(MarkdownParseTest.java:131)
```

For the reviewed implementation, the test also fails:
```
1) labTestOne(MarkdownParseTest)
java.lang.AssertionError: expected:<[`google.com, google.com, ucsd.edu]> but was:<[url.com, `google.com, google.com, ucsd.edu]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.labTestOne(MarkdownParseTest.java:86)
```

## Test Case 2

Test code:
```
@Test
public void labTestTwo() throws IOException {
    Path file = Path.of("lab-test-2.md");
    String contents = Files.readString(file);

    List<String> expected = List.of("a.com", "a.com(())", "example.com");
    assertEquals(expected, MarkdownParse.getLinks(contents));
}
```

For my implementation, the test fails:
```
2) labTestTwo(MarkdownParseTest)
java.lang.AssertionError: expected:<[a.com, a.com(()), example.com]> but was:<[a.com, a.com((]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.labTestTwo(MarkdownParseTest.java:139)
```

For the reviewed implementation, the test fails:
```
2) labTestTwo(MarkdownParseTest)
java.lang.AssertionError: expected:<[a.com, a.com(()), example.com]> but was:<[a.com, a.com((, example.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.labTestTwo(MarkdownParseTest.java:94)
```

## Test Case 3

Test code:
```
@Test
public void labTestThree() throws IOException {
    Path file = Path.of("lab-test-3.md");
    String contents = Files.readString(file);

    List<String> expected = List.of("https://www.twitter.com", "https://ucsd-cse15l-w22.github.io/", "https://cse.ucsd.edu/");
    assertEquals(expected, MarkdownParse.getLinks(contents));
}
```

For my implementation, the test passes.

For the reviewed implemenation, the test fails:
```
3) labTestThree(MarkdownParseTest)
java.lang.AssertionError: expected:<[https://www.twitter.com, https://ucsd-cse15l-w22.github.io/, https://cse.ucsd.edu/]> but was:<[]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.labTestThree(MarkdownParseTest.java:102)
```

## Questions

1. Yes, I think there is a short code change that will allow my code to work with backticks. I think a boolean can be used that simply tracks whether or not the main while loop is going over a code block or not. This can be implemented by setting a boolean to true when it sees a  backtick, and setting it to false when it sees another backtick. As long as the brackets that declare a link are not in the code segments, they are declared links.

2. No, I do not think there is a small code change that will allow us to deal with nested paranthesis, brackets, and escaped brackets. This would require a more complicated parser that, whenever it sees an open bracket, looks ahead to find the one that closes it before it can be declared an open bracket. This could be implemented with a tree structure, including one that can deal with escaped brackets correctly, but this is a more in-depth change.

3. Our code already works with newlines in brackets and paranthesis. It works because we read the input text file character-by-character instead of line-by-line, so a newline character is not treated specially.