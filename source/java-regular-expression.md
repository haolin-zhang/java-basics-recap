# Java Regular Expression

Java regular expression functionality is included in package java.util.regex, within JDK. Class java.lang.String also has inbuilt regular expression support.

Package java.util.regex contains 3 classes: Pattern, Matcher, PatternSyntaxException.

* **Pattern** compiles regular expression.
* **Matcher** performs match operations between a Pattern and a String.
* **PatternSyntaxException** indicates a syntax error within Pattern.

```java
// check the exact match, return true if found
Pattern pattern = Pattern.compile("foofoo");
Matcher matcher = pattern.matcher("foo");

// return true
matcher.find();

// matches = 2
int matches = 0;
while (matcher.find()) {
	matches++;
}
```

## Meta Characters

A leading backslash must be added when using special characters.

* **"."**	any character
* **"\d"**	number ([0-9])
* **"\D"**	non-digit ([^0-9])
* **"\s"**	white space
* **"\S"**	non white space
* **"\a"**	letter (a-zA-Z)
* **"\w"**	letter & number ([a-zA-Z0-9])
* **"\W"**	not letter nor number

```java
// match any character
Pattern pattern = Pattern.compile(".");
Matcher matcher = pattern.matcher("text");
matcher.find(); // return true

// match any digit
Pattern pattern = Pattern.compile("\\d");
Matcher matcher = pattern.matcher("123");
matcher.find(); // return true
```

## Character Combination

### OR Relationship

```java
// "[]" means set, any character appears within is target for matching
Pattern pattern = Pattern.compile("[abc]");
Matcher matcher = pattern.matcher("cab");

// matches = 3
int matches = 0;
while (matcher.find()) {
	matches++;
}
```

### NOR Relationship

```java
// "^" means if any character other than ones within set shows, return true
Pattern pattern = Pattern.compile("[^abc]");
Matcher matcher = pattern.matcher("cabds");

// matches = 2
int matches = 0;
while (matcher.find()) {
	matches++;
}
```

### Range

```java
// "[-]" is used to express range of matching scope
// here is matching all upper & lower case letters, number "10-15"
Pattern pattern = Pattern.compile("[a-zA-Z10-15]");
Matcher matcher = pattern.matcher("Cabds12");

// matches = 6
int matches = 0;
while (matcher.find()) {
	matches++;
}

// matches number "1-3" and "7-9"
Pattern pattern = Pattern.compile("[1-3[7-9]]");
Matcher matcher = pattern.matcher("123456789");

// matches = 6
int matches = 0;
while (matcher.find()) {
	matches++;
}
```

### Intersection

```java
// match the intersection between "1-6" and "3-9"
Pattern pattern = Pattern.compile("[1-6&&[3-9]]");
Matcher matcher = pattern.matcher("123456789");

// matches = 4
int matches = 0;
while (matcher.find()) {
	matches++;
}
```

### Subtraction

```java
// match set of "0-9" with number "2,4,6,8" removed
Pattern pattern = Pattern.compile("[0-9&&[^2468]]");
Matcher matcher = pattern.matcher("123456789");

// matches = 4
int matches = 0;
while (matcher.find()) {
	matches++;
}
```

## Group Matching

Group matching treat multiple characters as a single unit.

```java
// match a combination of two-digit
Pattern pattern = Pattern.compile("(\\d\\d)");
Matcher matcher = pattern.matcher("1212");

// matches = 2
int matches = 0;
while (matcher.find()) {
	matches++;
}

// match a combination of two-digit and a "1" followed by
Pattern pattern = Pattern.compile("(\\d\\d)\\1");
Matcher matcher = pattern.matcher("1212");

// matches = 1
int matches = 0;
while (matcher.find()) {
	matches++;
}
```

## Quantifier

Quantifier is used to match by specifying the number of occurrences.

* **"{X}"** occurs X number of times
* **"{X,Y}"** occurs between X and Y times
* **"*"** occurs zero or more times, short for {0,}
* **"+"** occurs one or more times, short for {1,}
* **"?"** occurs no or one times, short for {0,1}.

```java
// "?" stands for {0,1}, match will find 3 digits matches and a "zero-length match"
Pattern pattern = Pattern.compile("[0-9]?");
Matcher matcher = pattern.matcher("123");

// matches = 4
int matches = 0;
while (matcher.find()) {
	matches++;
}

// "+" stands for {1,}, and greedy type match will pick the largest match as possible
Pattern pattern = Pattern.compile("[0-9]+");
Matcher matcher = pattern.matcher("abc");

// matches = 1
int matches = 0;
while (matcher.find()) {
	matches++;
}
```

Java regular expression has a "zero-length matches" concept, it always matches everything in the text including an empty String at the end of every input. This means even if input is empty string, it will still return one match.

```java
// match an empty string with "?"
Pattern pattern = Pattern.compile("\\d?");
Matcher matcher = pattern.matcher("");

// matches = 1, as for "zero-length match"
int matches = 0;
while (matcher.find()) {
	matches++;
}
```

By default, Java regular expression takes "greedy" type match, which considers the largest occurrences as one match.

```java
// match "a" three times in a row, and take it as a single match
Pattern pattern = Pattern.compile("a{3}");
Matcher matcher = pattern.matcher("aaaaa");

// matches = 1
int matches = 0;
while (matcher.find()) {
	matches++;
}

// match "a" two or three times in a row, and take it as a single match
// greedy type match, and it takes "3" since there exists 3-time match
Pattern pattern = Pattern.compile("a{2,3}");
Matcher matcher = pattern.matcher("aaaaa");

// matches = 1
int matches = 0;
while (matcher.find()) {
	matches++;
}

// match "a" two or three times in a row, and take it as a single match
// "?" here appoints to lazy type match, and it takes "2" since there exists 2-time match
Pattern pattern = Pattern.compile("a{2,3}?");
Matcher matcher = pattern.matcher("aaaaa");

// matches = 2
int matches = 0;
while (matcher.find()) {
	matches++;
}
```

## Boundary Matching

In regular expression, "^" means begining of the string, while "$" means the end of the string.

```java
// false, since "dogs" appears at the end of the target string
Pattern pattern = Pattern.compile("^dog");
Matcher matcher = pattern.matcher("are dogs are friendly?");

// false, since "dogs" appears at the begining of the target string
Pattern pattern = Pattern.compile("dog$");
Matcher matcher = pattern.matcher("are dogs are friendly?");
```

## Pattern Methods

Pattern's compile method can also accept a set of flags alongside the regular expression.

* **Pattern.CANON_EQ** enables canonical equivalence (as in "é" with "e")
* **Pattern.CASE_INSENSITIVE** enables matching regardless of case
* **Pattern.COMMENTS** comments are allowed with "#" in regular expression

```java
Pattern pattern = Pattern.compile("dog$  #check for word dog at end of text", Pattern.COMMENTS);
Matcher matcher = pattern.matcher("This is a dog");
matcher.find(); // true
```

* **Pattern.DOTALL** match ignoring the line terminator (multiple lines)
* **Pattern.LITERAL** ignore any meta characters
* **Pattern.MULTILINE** match regarding line terminator

## Matcher Methods

```java
// index method: find the index of matched string
Pattern pattern = Pattern.compile("dog");
Matcher matcher = pattern.matcher("This dog is mine");
matcher.start(); // 5
matcher.end();   // 8

// replacement method: replace characters within target string
Pattern pattern = Pattern.compile("dog");
Matcher matcher = pattern.matcher("dogs are domestic animals, dogs are friendly");

// result = "cats are domestic animals, dogs are friendly"
String result = matcher.replaceFirst("cat");

// result = "cats are domestic animals, cats are friendly"
String newStr = matcher.replaceAll("cat");
```
