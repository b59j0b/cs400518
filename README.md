java c
CSE 8B:   Introduction to   Programming   II
Programming Assignment 5
Objects and   Classes
Due:   Monday,   February 24,   11:59   PM


Learning goals:
●          Implement a simplified Reddit   program using Java   classes
●          Use the Java ArrayList   class
●         Write unit tests for your   classes
This assignment must be   completed   INDIVIDUALLY.
Coding Style   [10   points]
For this   programming assignment, we will   be enforcing the CSE 8B and   11 Coding   Style   Guidelines. These guidelines can also be found on   Canvas   and   the   class website.   Please   ensure to   have complete file   headers, class   headers and   method   headers. Avoid using   magic   numbers,   use consistent   indentation and   keep your lines short.
Getting the Starter   Code
1.      You can download the starter code from   Piazza →   Resources   →   Homework   →   PA5.zip
2.      After compiling   Post.java   and User.java, you should see the following in   your   terminal


$ javac   Post.java
Post.java:65: error: cannot find   symbol






P1.upvoteCount =   5;
^
symbol:       variable upvoteCount
location: variable P1   of   type   Post
...




$ javac   User.java
User.java:55: error: cannot find   symbol
U1.karma   =   10;
^
symbol:       variable   karma
location: variable U1   of type   User
...

These compiler errors are expected, as the   method   unitTests   is trying to   reference   things that   have   not yet   been   implemented   in the starter code! Things should compile   cleanly once you've added in the instance   variables   for   each   class.
You   MUST   NOT   include any   package   statements   in your code.
Overview
Reddit   is a   popular social   news aggregation, content   rating, and discussion website.   Users submit content to the site such as links,   text   posts,   images,   and   videos,   which   are   then   voted   up   or down by other members.   In this   programming assignment, we will   create   a   simplified   version      of Reddit, where   users can create,   upvote, and downvote   posts. To   do   so,   you will   be completing the implementation of two classes:   Post.java   and User.java


Part   1:   Implementation   [80   points]
Post.java
On   Reddit, a   post can exist as a stand-alone   post   in   some   community   (subreddit)   or   as   a   comment on someone else's stand-alone post.   In this write-up, we   shall   refer to   the first   type   of   post as an "original   post" and the second type   of   post   as   a   "comment".   Both   types   of   posts   will   be   represented   by the   Post   class (we'll   use   Post's   instance variables to   help   us   indicate   whether a   particular Post   object   is representing an original   post   or a   comment)
In this   part of the assignment, you will   implement the   Post   class which represents the   properties   of a   reddit   post. The   UML diagram for the   Post   class   is:



To summarize, the   Post   class contains the following fields:
private String   title
●         The title of this   Post.   If this   Post   represents a comment, then title   must   be   null.   If   this   Post   represents an original   post, then title   should be   non-null.
private String   content
●         The content of this   reddit   Post.
private Post   replyTo
●         The   Post   object that this   Post   is   replying to.   If this   Post   is an original   post,   replyTo   must   be   null.   If this   Post   is a comment, then   replyTo   must   be   non-null.
private User   author
●         The author (represented as a   User   object) of this   Post.
private int   upvoteCount
●         The   number of upvotes that this   Post   has.
private int   downvoteCount
●         The   number of downvotes that this   Post   has.   and the following methods:
public Post(String title, String   content, User   author)
●         The constructor for initializing an original   post.
●         This constructor must set the title,   content, and   author   fields   of this   Post   object with   the values from the parameters (e.g., the title   field must   be   assigned   the   contents   of the   title   parameter).
●         Set   upvoteCount   and downvoteCount   to 0.
●          Set   replyTo   to   null.
public Post(String content,   Post   replyTo, User   author)
●         The constructor for initializing a   comment.
●         This constructor must set the   content,   replyTo, and   author   fields of this   Post   object   with the values from the parameters (e.g., the   content   field must   be   assigned   the contents of the   content   parameter).
●         Set   upvoteCount   and downvoteCount   to 0.
●          Set title   to   null.
public String getTitle()
●          Return the title   of this   Post.
public Post   get   ReplyTo()
●          Return the   Post   object that this   Post   is   replying to.
public User   getAuthor()
●          Return the   author   of this   Post.
public int   getUpvoteCount()
●          Return the   number of upvotes that this   Post   has.
public int   getDownvoteCount()
●          Return the   number of downvotes that this   Post   has.
public void updateUpvoteCount(boolean is   Increment)
●          Increment the   upvoteCount   of this   Post   by   1   if is   Increment   is true. Otherwise,   decrement   upvoteCount   by   1.
●         You   may assume that we will   not call   updateUpvoteCount   in such a way that would   result   in a   negative   upvoteCount   value   in any of our tests.


public void updateDownvoteCount(boolean is   Increment)
●          Increment the downvoteCount   of this   Post   by   1   if is   Increment   is true. Otherwise,   decrement downvoteCount   by 1.
●         You   may assume that we will   not call   updateDownvoteCount   in such a way that would   result   in a   negative downvoteCount   value   in any of our tests.
public ArrayList getThread()
●          Return an ArrayList   of Posts   in the current thread, starting with   the   original   post   and   ending with this   Post.
●         The original   post should   be the first   item   in the   ArrayList, followed   by one of   its   comments, followed   by another one of its comments, and   so   on   until this   Post   is   reached.
●          For example, suppose that   P1   is an original   post,   P2   is a   comment   on   P1,   and   P3   is   a   comment on   P2. Then   P3.getThread()   must   return   [P1, P2, P3].   In   other words,   the   "current thread"   is the collection of posts that originate from this   Post   and   "climbs   up"    through the   replyTo   fields to reach the original   post.
○          If P1   were to   have   more than one comment,   P3.getThread()   must still   return   [P1,   P2,   P3].
●         You   may   implement this   method   iteratively or recursively.
public String toString()
●          Return a String   representation of this   Post.
●          If this   Post   is an original   post,   its String   representation   must follow the format


[|]\t\n<br>\t<content><br><br><br>●          Original   Post   Example<br><br><br>[1   |0]            Title of Original   Post<br>Content of original   post<br><br><br><br><br><br><br>●          If this   Post   is a comment, the String   representation   must follow the format<br><br><br>[<upvoteCount>|<downvoteCount>]\t<content><br><br><br>●         Comment   Example<br><br><br>[2   |0]            Content of   comment<br><br><br>●          Implementation   Notes<br>○          \t   is a   tab   character.<br>○         You should   not   include angle   brackets (i.e.,   <>)   in the String   you   return.<br>○          In the above String   formats,   <field>   should be   populated with the<br>corresponding field value of   this   Post   (without the angle brackets). We strongly   recommend that you   use the   provided format string templates (along with      the String.format()   method)   in your implementation of this method.<b代 写CSE 8B: Introduction to Programming II Programming Assignment 5Java
代做程序编程语言r>User.java<br>In this   part of the assignment, you will   implement the User   class which represents   the   properties   of a   reddit   user. The   UML diagram for the User   class   is<br><br><br><br>To summarize, the User   class contains the following fields:<br>private String   username<br>●         The username of this   User.<br>private   int   karma<br>●         The   karma score of this   User.<br>private ArrayList<Post> posts<br>●         A list of Posts this User   has   authored,   including   original   posts   and   comments.<br>private ArrayList<Post> upvoted<br>●         A   list of other User's   Posts that this User   has   upvoted.<br>private ArrayList<Post> downvoted<br>●         A list of other User's   Posts that this   User   has   downvoted.   and the following methods:<br>public User(String   username)<br>●         The constructor for initializing a   User.<br>●         This constructor must set the   username   field of this   Post   object with the contents   of the   username   parameter.<br>●          Set   karma   to 0.<br>●          Initialize   posts,   upvoted, and downvoted   to empty ArrayLists using   the   no-argument   ArrayList   constructor.<br>public void addPost(Post   post)<br>●         Add   post   to the end of this User's lists   of authored   posts.<br>●          If post   is   null, you   must   not   add   it   to   posts.<br>●          Update this User's   karma   by calling   updateKarma(). You   must do this   regardless of the   value of post.   If post   is   non-null,   you   must call   updateKarma()   after adding   post   to posts.<br>●         You   may assume that we will only call   addPost   on the User   that   has   authored   post.<br>public void   updateKarma()<br>●          Update this User's   karma   score   by going through this User's   authored   posts   and summing   upvoteCount–downvoteCount   for each   post.   In other words, after calling   updateKarma()   this User's   karma   must   be equal to<br>   <br>where N   is the   number of posts   this   user   has authored.<br>public   int get   Karma()<br>●          Return this User's   karma   score.<br>public void upvote(Post   post)<br>●          If post   is   null, this   method   must do   nothing and   immediately   return.<br>●          If post   has already   been   upvoted   by this User   OR   if the author   of   post   is   this   User,   this   method   must do   nothing and   immediately return.<br>●          If post   already exists   in this   User's   list of downvoted   posts   (downvoted),   remove   it   from   downvoted   and update the downvoteCount   of post   accordingly.<br>●         Add   post   to the end of this User's   list of   upvoted   posts   (upvoted)   and   update   post's   upvoteCount   accordingly.<br>●          Update the   karma   score of post's author by   calling   updateKarma()   accordingly. You   must do this   if this   method does   not   immediately   return.<br>public void downvote(Post   post)<br>●          If post   is   null, this   method   must do   nothing and   immediately   return.<br>●          If post   has already   been downvoted   by this User   OR   if the   author   of   post   is   this   User,   this   method   must do   nothing and   immediately return.<br>●          If post   already exists   in this User's   list of   upvoted   posts   (upvoted),   remove   it   from   upvoted   and update the   upvoteCount   of post   accordingly.<br>●         Add   post   to the end of this User's list of downvoted   posts   (downvoted)   and   update   post's downvoteCount   accordingly.<br>●          Update the   karma   score of post's author by   calling   updateKarma()   accordingly. You   must do this   if this   method does   not   immediately   return.<br>public Post getTopOriginal   Post()<br>●          Return this User's top original   post. That   is, the original   post   authored   by   this   User   with   the greatest   upvoteCount–downvoteCount   value.<br>●          If this User   does   not   have any original   posts,   return   null.<br>●          If the greatest   upvoteCount–downvoteCount   value   is shared by multiple original   posts,   return the first original   post   in   posts   with this value.<br>public Post   getTopComment()<br>●          Return this User's top comment. This   is, the comment   authored   by   this   User   with   the   greatest   upvoteCount–downvoteCount   value.<br>●          If this User   does   not   have any comments,   return   null.<br>●          If the greatest   upvoteCount–downvoteCount   value   is shared by multiple comments,   return the first comment   in   posts   with this value.<br>public ArrayList<Post> get   Posts()<br>●          Return the   list of posts   authored   by   this   User.<br>public String toString()<br>●          Return the String   representation of this User. The String   representation   of this   User   must follow the format<br><br><br>u/<username> Karma:   <karma><br><br><br>●          Example<br><br><br>u/Widogast Karma:   9<br><br><br>●          Implementation   Notes<br>○         You should   not   include angle   brackets (i.e.,   <>)   in the String   you   return.<br>○          In the above String   formats,   <field>   should be   populated with the<br>corresponding field value of this User   (without   the angle brackets). We strongly   recommend that you   use the   provided format string templates (along with   the String.format()   method)   in your implementation of this method.<br><br><br>Part 2:   Unit Testing   [10   points]<br>In this   part of the assignment, you will   need to   implement your own test cases   in   the   method unitTests   in   both   Post.java   and User.java.   Each file   has   its own   unitTests   method that you   must fill out. To get full credit for this section, you   must follow   the   instructions   below<br>For the   Post   class, you must:<br>●          Implement one test for getUpvoteCount()<br>●          Implement one test for updateUpvoteCount()<br>●          Implement one test for updateDownvoteCount()   For the User   class, you   must:<br>●          Implement one test for   get   Karma()<br>●          Implement one test for addPost(Post   post)One simple test for each of the above methods   is   provided for you.   Thus, you   must   have   a   total of two   unit tests for each of the above methods. You   may   use   the   provided   tests   as   inspiration for implementing your own test cases. As always, you are welcome   (and   encouraged) to add   more test cases but, as   long   as you follow   the   above   instructions,   you   will   receive full credit for this part of the   assignment.<br>Submission<br>VERY   IMPORTANT:   Please follow the   instructions   below carefully and make the exact   submission format.<br>1.       Go to Gradescope via Canvas   and   click   on   PA5.<br><br><br>2.       Click the   DRAG    DROP section and directly select   the   required files   Post.java   and   User.java.   Please   make sure you   DO   NOT   submit a zip, just the two files   in one Gradescope submission.   Make sure the names of the files are   correct.<br>3.       Following the   previous step, our submission should   look   like the   screenshot   below.   Click   upload to submit your file.<br><br>4.      You can   resubmit an   unlimited   number of times   before the due date. Your score will   depend on your final submission, even if your former submissions have   a   higher   score.<br>5.       The autograder is for grading your uploaded files automatically.   Make   sure   your   code   can compile on Gradescope.   If your code fails to compile on   gradescope   you   will   receive zero   points on ALL autograded   components of your submission.<br>NOTE: The Gradescope Autograder you see   is a minimal autograder.   For this   particular assignment,   it will only show compilation results and   the   results   of   basic   tests   (from the write-up). After the assignment deadline, a thorough Autograder will be   used   to determine the final grade of the assignment. Thus, to ensure that you would   receive full   points from the thorough Autograder,   it   is your   job to extensively test   your code for correctness (make   use of      unitTests!)<br><br>         <br>加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
