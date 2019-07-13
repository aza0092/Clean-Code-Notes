# Clean Code
My personal notes on the book Clean Code - A Handbook of Agile Software Craftsmanship by Robert C. Martin

I only added notes that are new knowledge to me, and excluded info I already know and practise

# Index

1. [Clean Code](#clean-code)
2. [Meaningful Names](#meaningful-names)
3. [Functions](#functions)
4. [Comments](#comments)
5. [Formatting](#formatting) 
6. [Objects and Data Structures](#objects-and-data-structures) 
7. [Error Handling] (#error-handling)


# <a name="clean-code">1. Clean Code</a>
- There is, after all, a difference between code that is easy to read and code that is easy to change. 
 
- Code, without tests, is not clean. 

- Small code is better than large code 

- Clean code always looks like it was written by someone who cares. There is nothing obvious that you can do to make it better. 
 
- Simple code: contains no duplication - Minimizes the number of entities such as classes,methods, functions, and the like

- Reduced duplication, high expressiveness, and early building of simple abstractions. That’s what makes clean code for me
 

# <a name="meaningful-names">2. Meaningful Names</a>

If a name requires a comment, then the name does not reveal its intent.
```java
int d; // elapsed time in days
```

```java
int elapsedTimeInDays;
int daysSinceCreation;
int daysSinceModification;
int fileAgeInDays;
```

```java
public List<int[]> getThem() {
 List<int[]> list1 = new ArrayList<int[]>();
 for (int[] x : theList)
 if (x[0] == 4)
 list1.add(x);
 return list1;
 }
```
Above code is bad because:
1. What kinds of things are in theList?
2. What is the significance of the zeroth subscript of an item in theList?
3. What is the significance of the value 4?
4. How would I use the list being returned?

## Use Intention-Revealing Names
```java
 public List<int[]> getFlaggedCells() {
 List<int[]> flaggedCells = new ArrayList<int[]>();
 for (int[] cell : gameBoard)
 if (cell[STATUS_VALUE] == FLAGGED)
 flaggedCells.add(cell);
 return flaggedCells;
 }
```

We can go further and write a simple class for cells instead of using an array of ints

```java
 public List<Cell> getFlaggedCells() {
 List<Cell> flaggedCells = new ArrayList<Cell>();
 for (Cell cell : gameBoard)
 if (cell.isFlagged())
 flaggedCells.add(cell);
 return flaggedCells;
 }
```

## Avoid Disinformation 
- Do not refer to a grouping of **accounts** as an **accountList** unless it’s actually a *List*.
The word list means something specific to programmers. If the container holding the
accounts is not actually a List, it may lead to false conclusions.1 So **accountGroup** or
**bunchOfAccounts** or just plain **accounts** would be better.

- Beware of using names which vary in small ways. How long does it take to spot the
subtle difference between a *XYZControllerForEfficientHandlingOfStrings* in one module
and, somewhere a little more distant, *XYZControllerForEfficientStorageOfStrings*?


- A truly awful example of disinformative names would be the use of lower-case *L* or
uppercase *O* as variable names, especially in combination. The problem, of course, is that
they look almost entirely like the constants one and zero

```java
int a = l;
if ( O == l )
 a = O1;
else
 l = 01;
```

## Make Meaningful Distinctions
```java
 public static void copyChars(char a1[], char a2[]) {
 for (int i = 0; i < a1.length; i++) {
 a2[i] = a1[i];
 }
 }
```

- This function reads much better when source and destination are used for the argument
names.


- The word *variable* should never appear in a variable name. The word *table* should never appear in a table name. How is *NameString* better than
*Name*? Would a *Name* ever be a floating point number?


## Class Names
Classes and objects should have **noun or noun phrase** names like *Customer*, *WikiPage*,
*Account*, and *AddressParser*. Avoid words like *Manager*, *Processor*, *Data*, or *Info* in the name
of a class. A class name should not be a **verb**.

## Method Names
Methods should have **verb** or verb phrase names like *postPayment*, *deletePage*, or *save*.
Accessors, mutators, and predicates should be named for their value and prefixed with *get*,
*set*, and *is*

```java
string name = employee.getName();
customer.setName("mike");
if (paycheck.isPosted())...
```

When constructors are overloaded, use static factory methods with names that
describe the arguments. For example,

```java
Complex fulcrumPoint = Complex.FromRealNumber(23.0);
```

```java
is generally better than
```

```java
Complex fulcrumPoint = new Complex(23.0);
```

Consider enforcing their use by making the corresponding constructors private.

## Don’t Be Cute
- Choose clarity over entertainment value.

- Cuteness in code often appears in the form of colloquialisms or slang. For example,
don’t use the name *whack()* to mean *kill()*. Don’t tell little culture-dependent jokes like
*eatMyShorts()* to mean *abort()*.


## Pick One Word per Concept
Pick one word for one abstract concept and stick with it. For instance, it’s confusing to
have *fetch*, *retrieve*, and *get* as **equivalent methods of different classes**. How do you
remember which method name goes with which class?


## Don’t Pun
- Avoid using the same word for two purposes

- If you follow the “one word per concept” rule, you could end up with many classes
that have, for example, an *add* method. As long as the parameter lists and return values of
the various add methods are semantically **equivalent**, all is well.

# <a name="functions">3. Functions</a>

Minimize using nested blocks

## One Level of Abstraction per Function
In order to make sure our functions are doing “one thing,” we need to make sure that the
statements within our function are all at the same level of abstraction. It is easy to see how
Listing 3-1 violates this rule. There are concepts in there that are at a very high level of
abstraction, such as *getHtml()*; others that are at an intermediate level of abstraction, such
as: *String pagePathName = PathParser.render(pagePath)*; and still others that are remarkably low level, such as: *.append("\n")*.

## Reading Code from Top to Bottom: The Stepdown Rule
- We want the code to read like a top-down narrative. We want every function to be followed by 
those at the next level of abstraction so that we can read the program, descending
one level of abstraction at a time as we read down the list of functions. I call this The Stepdown Rule:

*To include the setups and teardowns, we include setups, then we include the test page content, and then we include the teardowns.
To include the setups, we include the suite setup if this is a suite, then we include the
regular setup.
To include the suite setup, we search the parent hierarchy for the “SuiteSetUp” page
and add an include statement with the path of that page.
To search the parent. . .*


## Function Arguments
- Use as much arguements as possible. Ideal is zero, one or two. Three or more should be avoided where possible
Testing function with no arguements is far easier

- If a function is going to transform its input argument, the transformation should appear as the return value. 
Indeed, *StringBuffer transform(StringBuffer in)* is better than *void transform-(StringBuffer out)*, even if the
implementation in the first case simply returns the input argument. At least it still follows
the form of a transformation. 


- Passing a boolean into a function is a truly terrible practice. It immediately complicates the signature of the method, loudly proclaiming that this function
does more than one thing. It does one thing if the flag is true and another if the flag is false!


- When a function seems to need more than two or three arguments, it is likely that some of
those arguments ought to be wrapped into a class of their own. Consider, for example, the
difference between the two following declarations:

*Circle makeCircle(double x, double y, double radius);
Circle makeCircle(Point center, double radius);*


*assertEquals* might be better written as *assertExpectedEqualsActual(expected, actual)*. This strongly mitigates the problem of having to remember the ordering of the arguments


## No Side Effects
- This function uses a standard algorithm to match a userName to a password. It returns true if they match
and false if anything goes wrong. But it also has a side effect

```java
public class UserValidator {
 private Cryptographer cryptographer;
 public boolean checkPassword(String userName, String password) {
 User user = UserGateway.findByName(userName);
 if (user != User.NULL) {
 String codedPhrase = user.getPhraseEncodedByPassword();
 String phrase = cryptographer.decrypt(codedPhrase, password);
 if ("Valid Password".equals(phrase)) {
 Session.initialize();
 return true;
 }
 }
 return false;
 }
}
```

- The side effect is the call to `Session.initialize()`, of course. The `checkPassword` function, by its name, says that it checks the password. The name does not imply that it initializes the session. So a caller who believes what the name of the function says runs the risk
of erasing the existing session data when he or she decides to check the validity of the user


- This side effect creates a **temporal coupling**. That is, *checkPassword* can only be called at certain times (in other words, when it is safe to initialize the session). 
If it is called out of order, session data may be inadvertently lost. 
Temporal couplings are confusing, especially when hidden as a side effect.
If you must have a temporal coupling, you should make it clear in the name of the function. 
In this case we might rename the function `checkPasswordAndInitializeSession`


## Output Arguments
Arguments are most naturally interpreted as inputs to a function. It's bad habit to take an argument that is actually an output rather than an input. 

## Command Query Separation
Functions should either do something or answer something, but not both. Either your
function should change the state of an object, or it should return some information about
that object. Doing both often leads to confusion

```java
public boolean set(String attribute, String value);
```

```java
if (set("username", "unclebob"))...
```

Imagine this from the point of view of the reader. What does it mean? Is it asking whether the “username” attribute was previously set to “unclebob”? Or is it asking whether the
“username” attribute was successfully set to “unclebob”?


The real solution is to separate the command from the query so that the ambiguity cannot occur.

```java
if (attributeExists("username")) {
setAttribute("username", "unclebob");
... }
```

## Extract Try/Catch Blocks
`Try/catch` blocks are ugly in their own right. They confuse the structure of the code and
mix error processing with normal processing. So it is better to extract the bodies of the try
and catch blocks out into functions of their own.

**Old Code:**
```java
try {
deletePage(page);
registry.deleteReference(page.name);
configKeys.deleteKey(page.name.makeKey());
}
catch (Exception e) {
logger.log(e.getMessage());
}
```

**New Code:**
```java
public void delete(Page page) {
 try {
 deletePageAndAllReferences(page);
 }
 catch (Exception e) {
 logError(e);
 }
 }
 private void deletePageAndAllReferences(Page page) throws Exception {
 deletePage(page);
 registry.deleteReference(page.name);
 configKeys.deleteKey(page.name.makeKey());
 }
 private void logError(Exception e) {
 logger.log(e.getMessage());
 }
```

## Error Handling Is One Thing
Functions should do one thing. Error handing is one thing. Thus, a function that handles
errors should do nothing else


## The Error.java Dependency Magnet
`Error.java` Classes are a dependency magnet; many other classes must import and use
them. Thus, when the Error enum changes, all those other classes need to be recompiled
and redeployed.

When you use **exceptions** rather than **error codes**, then new exceptions are derivatives of
the exception class. They can be added without forcing any recompilation or redeployment


# <a name="functions">3. Functions</a>

- Use comments only when your code fails to express itself

- Comments don't usually evolve and change when the code does

- One of the more common motivations for writing comments is bad code. We write a module and we know it is confusing and disorganized. We know it’s a mess. So we say to ourselves, “Ooh, I’d better comment that!” No! You’d better clean it! 

## Explain Yourself in Code
Which would you rather see?
```java
// Check to see if the employee is eligible for full benefits
if ((employee.flags & HOURLY_FLAG) &&
 (employee.age > 65)) 
```
or this?
```java
if (employee.isEligibleForFullBenefits())
```

## Informative Comments
```java
// Returns an instance of the Responder being tested.
protected abstract Responder responderInstance();
```

```java
// format matched kk:mm:ss EEE, MMM dd, yyyy
Pattern timeMatcher = Pattern.compile(
 "\\d*:\\d*:\\d* \\w*, \\w* \\d*, \\d*");
```
 
## Explanation of Intent
- Sometimes a comment goes beyond just useful information about the implementation and
provides the intent behind a decision

- In the example below, the author is comparing two objects, and he decided that he
wanted to sort objects of his class higher than objects of any other

```java
public int compareTo(Object o)
{
if(o instanceof WikiPagePath)
{
WikiPagePath p = (WikiPagePath) o;
String compressedName = StringUtil.join(names, "");
String compressedArgumentName = StringUtil.join(p.names, "");
return compressedName.compareTo(compressedArgumentName);
}
return 1; // we are greater because we are the right type.
}
```

### Clarification
Sometimes it's useful to clarify code when its part of the standard library, or in
code that you cannot alter

```java
public void testCompareTo() throws Exception
{
WikiPagePath a = PathParser.parse("PageA");
WikiPagePath ab = PathParser.parse("PageA.PageB");
WikiPagePath b = PathParser.parse("PageB");
WikiPagePath aa = PathParser.parse("PageA.PageA");
WikiPagePath bb = PathParser.parse("PageB.PageB");
WikiPagePath ba = PathParser.parse("PageB.PageA");
assertTrue(a.compareTo(a) == 0); // a == a
assertTrue(a.compareTo(b) != 0); // a != b
assertTrue(ab.compareTo(ab) == 0); // ab == ab
assertTrue(a.compareTo(b) == -1); // a < b
assertTrue(aa.compareTo(ab) == -1); // aa < ab
assertTrue(ba.compareTo(bb) == -1); // ba < bb
assertTrue(b.compareTo(a) == 1); // b > a
assertTrue(ab.compareTo(aa) == 1); // ab > aa
assertTrue(bb.compareTo(ba) == 1); // bb > ba
}
```

## Warning of Consequences
Sometimes it is useful to warn other programmers about certain consequences

```java
// Don't run unless you
// have some time to kill.
public void _testWithReallyBigFile()
{
writeLinesToFile(10000000);
response.setBody(testFile);
response.readyToSend(this);
String responseString = output.toString();
assertSubString("Content-Length: 1000000000", responseString);
assertTrue(bytesSent > 1000000000);
}
```

```java

public static SimpleDateFormat makeStandardHttpDateFormat()
{
//SimpleDateFormat is not thread safe,
//so we need to create each instance independently.
SimpleDateFormat df = new SimpleDateFormat("EEE, dd MMM yyyy HH:mm:ss z");
df.setTimeZone(TimeZone.getTimeZone("GMT"));
return df;
}
```

## TODO Comments
```java
//TODO-MdM these are not needed
// We expect this to go away when we do the checkout model
protected VersionInfo makeVersion() throws Exception
{
return null;
}
```

## Amplification
A comment may be used to amplify the importance of something that may otherwise seem
inconsequential.

```java
String listItemContent = match.group(3).trim();
// the trim is real important. It removes the starting
// spaces that could cause the item to be recognized
// as another list.
new ListItemWidget(this, listItemContent, this.level + 1);
return buildList(text.substring(match.end()));
```

## Bad Comments: Mumbling - Redundant - Undeleted commented-out comments - Nonlocal info
- Most comments fall into this category. Usually they are crutches or excuses for poor code
or justifications for insufficient decisions, amounting to little more than the programmer
talking to himself.

- Write comments that don't lie, and say what the code exactly does

## Redundant Comments
The below code shows a simple function with a header comment that is completely redundant. The comment probably takes longer to read than the code itself.

```java
// Utility method that returns when this.closed is true. Throws an exception
// if the timeout is reached.
public synchronized void waitForClose(final long timeoutMillis)
 throws Exception
{
if(!closed)
{
wait(timeoutMillis);
if(!closed)
throw new Exception("MockResponseSender could not be closed");
}
}
```

Don't write comments that clutter and obscure code
```java
/**
 * The processor delay for this component.
 */
 protected int backgroundProcessorDelay = -1;
```

It is just plain silly to have a rule that says that every function must have a javadoc, or
every variable must have a comment. Comments like this just clutter up the code

```java
 /**
 *
 * @param title The title of the CD
 * @param author The author of the CD
 * @param tracks The number of tracks on the CD
 * @param durationInMinutes The duration of the CD in minutes
 */
 public void addCD(String title, String author,
 int tracks, int durationInMinutes) {
 CD cd = new CD();
 cd.title = title;
 cd.author = author;
 cd.tracks = tracks;
 cd.duration = duration;
 cdList.add(cd);
 }
```
 
## Noise Comments
Sometimes you see comments that are nothing but noise. They restate the obvious and
provide no new information.

```java
/**
 * Returns the day of the month.
 *
 * @return the day of the month.
 */
public int getDayOfMonth() {
 return dayOfMonth;
}
```

## Don’t Use a Comment When You Can Use a Function or a Variable
Consider this:

```java
// does the module from the global list <mod> depend on the
// subsystem we are part of?
if (smodule.getDependSubsystems().contains(subSysMod.getSubSystem()))
```

Improve to:

```java
ArrayList moduleDependees = smodule.getDependSubsystems();
String ourSubSystem = subSysMod.getSubSystem();
if (moduleDependees.contains(ourSubSystem))
```

## Commented-Out Code
Others who see that commented-out code won’t have the courage to delete it. They’ll think
it is there for a reason and is too important to delete.


## Nonlocal Information
If you must write a comment, then make sure it describes the code it appears near. Don’t
offer systemwide information in the context of a local comment.

Consider code below, the comments offers information about the default port. And yet the function has absolutely no control over
what that default is

```java
/**
 * Port on which fitnesse would run. Defaults to <b>8082</b>.
 *
 * @param fitnessePort
 */
public void setFitnessePort(int fitnessePort)
{
this.fitnessePort = fitnessePort;
}
```

## Function Headers
Short functions don’t need much description. A well-chosen name for a small function that
does one thing is usually better than a comment header. 

## Javadocs
Below Comment header can be improved:

```java
/**
 * This class Generates prime numbers up to a user specified
 * maximum. The algorithm used is the Sieve of Eratosthenes.
 * <p>
 * Eratosthenes of Cyrene, b. c. 276 BC, Cyrene, Libya --
 * d. c. 194, Alexandria. The first man to calculate the
 * circumference of the Earth. Also known for working on
 * calendars with leap years and ran the library at Alexandria.
 * <p>
 * The algorithm is quite simple. Given an array of integers
 * starting at 2. Cross out all multiples of 2. Find the next
 * uncrossed integer, and cross out all of its multiples.
 * Repeat untilyou have passed the square root of the maximum
 * value.
 *
 * @author Alphonse
 * @v
```

improved code:
```java
/**
 * This class Generates prime numbers up to a user specified
 * maximum. The algorithm used is the Sieve of Eratosthenes.
 * Given an array of integers starting at 2:
 * Find the first uncrossed integer, and cross out all its
 * multiples. Repeat until there are no more multiples
 * in the array.
 */
```

# <a name="formatting">5. Formatting</a>

## Dependent Functions
If one function calls another, they should be vertically close,
and the caller should be above the callee, if at all possible. This gives the program a natural
flow. If the convention is followed reliably, readers will be able to trust that function definitions will follow shortly after their use.

## Horizontal Formatting
We should strive to keep our lines short. You should never have to scroll to the right

## Horizontal Openness and Density
- I surrounded the assignment operators with white space to accentuate them
I didn’t put spaces between the function names and the opening parenthesis
```java
 private void measureLine(String line) {
 lineCount++;
 int lineSize = line.length();
 totalChars += lineSize;
 lineWidthHistogram.addLine(lineSize, lineCount);
 recordWidestLine(lineSize);
 }
```

- Another use for white space is to accentuate the precedence of operators
```java
public class Quadratic {
 public static double root1(double a, double b, double c) {
 double determinant = determinant(a, b, c);
 return (-b + Math.sqrt(determinant)) / (2*a);
 }
 public static double root2(int a, int b, int c) {
 double determinant = determinant(a, b, c);
 return (-b - Math.sqrt(determinant)) / (2*a);
 }
 private static double determinant(double a, double b, double c) {
 return b*b - 4*a*c;
 }
}
```

## Indentation
Bad:
```java
public class CommentWidget extends TextWidget
{
public static final String REGEXP = "^#[^\r\n]*(?:(?:\r\n)|\n|\r)?";
public CommentWidget(ParentWidget parent, String text){super(parent, text);}
public String render() throws Exception {return ""; }
}
```

Good:
```java
public class CommentWidget extends TextWidget {
 public static final String REGEXP = "^#[^\r\n]*(?:(?:\r\n)|\n|\r)?";
 public CommentWidget(ParentWidget parent, String text) {
 super(parent, text);
 }
 public String render() throws Exception {
 return "";
 }
}
```

## Formatting Rules
```java
public class CodeAnalyzer implements JavaFileAnalysis {
 private int lineCount;
 private int maxLineWidth;
 private int widestLineNumber;
 private LineWidthHistogram lineWidthHistogram;
 private int totalChars;
 public CodeAnalyzer() {
 lineWidthHistogram = new LineWidthHistogram();
 }
 public static List<File> findJavaFiles(File parentDirectory) {
 List<File> files = new ArrayList<File>();
 findJavaFiles(parentDirectory, files);
 return files;
 }
 private static void findJavaFiles(File parentDirectory, List<File> files) {
 for (File file : parentDirectory.listFiles()) {
 if (file.getName().endsWith(".java"))
 files.add(file);
 else if (file.isDirectory())
 findJavaFiles(file, files);
 }
 }
 public void analyzeFile(File javaFile) throws Exception {
 BufferedReader br = new BufferedReader(new FileReader(javaFile));
 String line;
 while ((line = br.readLine()) != null)
 measureLine(line);
 }
 private void measureLine(String line) {
 lineCount++;
 int lineSize = line.length();
 totalChars += lineSize;
 lineWidthHistogram.addLine(lineSize, lineCount);
 recordWidestLine(lineSize);
 }
 private void recordWidestLine(int lineSize) {
 if (lineSize > maxLineWidth) {
 maxLineWidth = lineSize;
 widestLineNumber = lineCount;
 }
 }
 public int getLineCount() {
 return lineCount;
 }
 public int getMaxLineWidth() {
 return maxLineWidth;
 }
  public int getWidestLineNumber() {
 return widestLineNumber;
 }
 public LineWidthHistogram getLineWidthHistogram() {
 return lineWidthHistogram;
 }
 public double getMeanLineWidth() {
 return (double)totalChars/lineCount;
 }
 public int getMedianLineWidth() {
 Integer[] sortedWidths = getSortedWidths();
 int cumulativeLineCount = 0;
 for (int width : sortedWidths) {
 cumulativeLineCount += lineCountForWidth(width);
 if (cumulativeLineCount > lineCount/2)
 return width;
 }
 throw new Error("Cannot get here");
 }
 private int lineCountForWidth(int width) {
 return lineWidthHistogram.getLinesforWidth(width).size();
 }
 private Integer[] getSortedWidths() {
 Set<Integer> widths = lineWidthHistogram.getWidths();
 Integer[] sortedWidths = (widths.toArray(new Integer[0]));
 Arrays.sort(sortedWidths);
 return sortedWidths;
 }
}
```

# <a name="objects-and-data-structures">6. Objects and Data Structures</a>

## Data Abstraction
The first code snippet exposes implementation (bad thing), and the second code snippet hides it.

```java
public class Point {
 public double x;
 public double y;
}
```

```java
public interface Point {
 double getX();
 double getY();
 void setCartesian(double x, double y);
 double getR();
 double getTheta();
 void setPolar(double r, double theta);
}
```

- Hiding implementation is not just a matter of putting a layer of functions between
the variables. Hiding implementation is about abstractions! A class does not simply
push its variables out through getters and setters. Rather it exposes abstract interfaces
that allow its users to manipulate the essence of the data, without having to know its
implementation

- Look at the two cases below. The second option is preferable. We do not want to expose
the details of our data. Rather we want to express our data in abstract terms. This is not
merely accomplished by using interfaces and/or getters and setters. Serious thought needs
to be put into the best way to represent the data that an object contains. The worst option is
to blithely add getters and setters.

```java
public interface Vehicle {
 double getFuelTankCapacityInGallons();
 double getGallonsOfGasoline();
}
```

```java
public interface Vehicle {
 double getPercentFuelRemaining();
}
```

# <a name="error-handling">7. Error Handling</a>
Error handling is important, but if it obscures logic, it’s wrong.

## Use Exceptions Rather Than Return Codes
Bad:
```java
public class DeviceController {
 ...
 public void sendShutDown() {
 DeviceHandle handle = getHandle(DEV1);
 // Check the state of the device
 if (handle != DeviceHandle.INVALID) {
 // Save the device status to the record field
 retrieveDeviceRecord(handle);
 // If not suspended, shut down
 if (record.getStatus() != DEVICE_SUSPENDED) {
 pauseDevice(handle);
 clearDeviceWorkQueue(handle);
 closeDevice(handle);
 } else {
 logger.log("Device suspended. Unable to shut down");
 }
 } else {
 logger.log("Invalid handle for: " + DEV1.toString());
 }
 }
 ...
}
```

Good (Algorithm for device shutdown and error handling are now separated):
```java
public class DeviceController {
 ...
 public void sendShutDown() {
 try {
 tryToShutDown();
 } catch (DeviceShutDownError e) {
 logger.log(e);
 }
 }
  private void tryToShutDown() throws DeviceShutDownError {
 DeviceHandle handle = getHandle(DEV1);
 DeviceRecord record = retrieveDeviceRecord(handle);
 pauseDevice(handle);
 clearDeviceWorkQueue(handle);
 closeDevice(handle);
 }
 private DeviceHandle getHandle(DeviceID id) {
 ...
 throw new DeviceShutDownError("Invalid handle for: " + id.toString());
 ...
 }
 ...
}
```

## Write Your Try-Catch-Finally Statement First
- Narrow down your catch statement.
```java
catch (FileNotFoundException e) {
 throw new StorageException("retrieval error”, e);
 }
```
## Provide Context with Exceptions
- Create informative error messages and pass them along with your exceptions. Mention the operation that failed and the type of failure. If you are logging in your application,
pass along enough information to be able to log the error in your catch

## Define the Normal Flow
- Don't let your error-handling ruin the logic of your code. Using try-catch some

- The exception below clutters the logic
```java
try {
 MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
 m_total += expenses.getTotal();
} catch(MealExpensesNotFound e) {
 m_total += getMealPerDiem();
}
```
This is better:
```java
MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
m_total += expenses.getTotal();
```

- To make the code even simpler, we can create a class that handle special cases, aka SPECIAL CASE PATTERN. Here we can change the
ExpenseReportDAO so that it always returns a MealExpense object. If there are no meal
expenses, so it returns a MealExpense object that returns the per diem as its total:

```java
public class PerDiemMealExpenses implements MealExpenses {
 public int getTotal() {
 // return the per diem default
 }
}
```

## Don’t Return Null
- Writing code that checks for `null` every other line is bad code.
```java
 public void registerItem(Item item) {
 if (item != null) {
 ItemRegistry registry = peristentStore.getItemRegistry();
 if (registry != null) {
 Item existing = registry.getItem(item.getID());
 if (existing.getBillingPeriod().hasRetailOwner()) {
 existing.register(item);
 }
 }
 }
 }
```
- In the code above, all it takes is one missing null check to send an application spinning out of control.
- There is too much `null` to check. The solution is to **consider throwing an exception or returning a SPECIAL CASE object**

## Don’t Pass Null
- If you must pass `null` in your arguement, create a new exception type and throw it:
```java
public class MetricsCalculator
{
 public double xProjection(Point p1, Point p2) {
 if (p1 == null || p2 == null) {
 throw InvalidArgumentException(
 "Invalid argument for MetricsCalculator.xProjection");
 }
 return (p2.x – p1.x) * 1.5;
 }
}
```
Using `InvalidArgumentException` means we have to define a handler and that makes the solution more complex. A better approach is to **use set of assertions**:
```java
public class MetricsCalculator
{
 public double xProjection(Point p1, Point p2) {
 assert p1 != null : "p1 should not be null";
 assert p2 != null : "p2 should not be null";
 return (p2.x – p1.x) * 1.5;
 }
}
```
