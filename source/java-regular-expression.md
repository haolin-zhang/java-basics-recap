Java Regular Expression

Java regular expression functionality is included in package java.util.regex, within JDK. Class java.lang.String also has inbuilt regular expression support.

Package java.util.regex contains 3 classes: Pattern, Matcher, PatternSyntaxException.

Pattern compiles regular expression.
Matcher performs match operations between a Pattern and a String.
PatternSyntaxException indicates a syntax error within Pattern.

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

Meta Characters

// dot (".") means matching any character
Pattern pattern = Pattern.compile(".");
Matcher matcher = pattern.matcher("text");
matcher.find(); // return true

Character Combination

OR Relationship

// "[]" means set, any character appears within is target for matching
Pattern pattern = Pattern.compile("[abc]");
Matcher matcher = pattern.matcher("cab");

// matches = 3
int matches = 0;
while (matcher.find()) {
	matches++;
}

NOR Relationship

// "^" means if any character other than ones within set shows, return true
Pattern pattern = Pattern.compile("[^abc]");
Matcher matcher = pattern.matcher("cabds");

// matches = 2
int matches = 0;
while (matcher.find()) {
	matches++;
}

Range

// "[-]" is used to express range of matching scope
// all upper & lower case letters, number "10-15"
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

Intersection

// match the intersection between "1-6" and "3-9"
Pattern pattern = Pattern.compile("[1-6&&[3-9]]");
Matcher matcher = pattern.matcher("123456789");

// matches = 4
int matches = 0;
while (matcher.find()) {
	matches++;
}

Subtraction

// match set of "0-9" with number "2,4,6,8" removed
Pattern pattern = Pattern.compile("[0-9&&[^2468]]");
Matcher matcher = pattern.matcher("123456789");

// matches = 4
int matches = 0;
while (matcher.find()) {
	matches++;
}

Special Characters

A leading backslash must be added when using special characters.

// "\d" represents number
Pattern pattern = Pattern.compile("\\d");
Matcher matcher = pattern.matcher("123");

\d	number ([0-9])
\D	non-digit ([^0-9])
\s	white space
\S	non white space
\a	letter (a-zA-Z)
\w	letter & number ([a-zA-Z0-9])
\W	not letter nor number

Group Matching

Group matching treat multiple characters as a single unit.

Pattern pattern = Pattern.compile("(\\d\\d)");
Matcher matcher = pattern.matcher("1212");

// matches = 2
int matches = 0;
while (matcher.find()) {
	matches++;
}

Pattern pattern = Pattern.compile("(\\d\\d)\\1");
Matcher matcher = pattern.matcher("1212");

// matches = 1
int matches = 0;
while (matcher.find()) {
	matches++;
}

Boundary Matching

In regular expression, "^" means begining of the string, while "$" means the end of the string.

// false, since "dogs" appears at the end of the target string
Pattern pattern = Pattern.compile("^dog");
Matcher matcher = pattern.matcher("are dogs are friendly?");

// false, since "dogs" appears at the begining of the target string
Pattern pattern = Pattern.compile("dog$");
Matcher matcher = pattern.matcher("are dogs are friendly?");

Pattern Methods

Pattern's compile method can also accept a set of flags alongside the regular expression.

Pattern.CANON_EQ			enables canonical equivalence (as in "é" with "e")
Pattern.CASE_INSENSITIVE	enables matching regardless of case
Pattern.COMMENTS			comments are allowed with "#" in regular expression

Pattern pattern = Pattern.compile("dog$  #check for word dog at end of text", Pattern.COMMENTS);
Matcher matcher = pattern.matcher("This is a dog");
matcher.find(); // true

Pattern.DOTALL			match ignoring the line terminator (multiple lines)
Pattern.LITERAL			ignore any meta characters
Pattern.MULTILINE			match regarding line terminator

Matcher Methods

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
