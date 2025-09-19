[![Open in Codespaces](https://classroom.github.com/assets/launch-codespace-2972f46106e565e64193e422d61a12cf1da4916b45550586e14ef0a7c637dd04.svg)](https://classroom.github.com/open-in-codespaces?assignment_repo_id=20296421)
# PA1 - Basic Song Stats

# Motivation
You have been provided data of various users rating various songs on a scale of 1 to 5. The end goal of this semester long project is to build a song application that recommends songs to users.

In this PA, we want to parse this data and find some basic stats on the data. You will be finding the number of ratings, mean, and standard deviation of the songs in the data file.

# Tasks

The data comes as a comma separated values (CSV) file which contains song, user, and rating information. We have a `database/files/…` directory of some CSV files of ratings for songs by users that we wish to process. Each rating of a song by a user is a separate entry in a CSV. The entries are not in any order. The rating is an integer value between 1 (the user hated the song) and 5 (they loved it). There is not necessarily a rating for every song by every user, there can be gaps in the data. 

```
song1,user1,rating 
song1,user2,rating 
song2,user1,rating 
...
```

The files provided in the `database/files/…` directory are just some files to get you started. To further test your program you must create your own CSV files that follow the same format.

You may use the [Apache Commons CSV library](https://commons.apache.org/proper/commons-csv/user-guide.html) to read the CSV. The library dependency has already been added for you in `build.gradle`. Otherwise any other method of reading and writing to a CSV is acceptable.

As you read the file, you should maintain a list of the song names and the number of times they occur, and a list of user names and the number of times they occur. You will also need to maintain values to compute the running mean and the _population_ standard deviation for ratings using a method like [Welford's online algorithm](https://en.wikipedia.org/wiki/Algorithms_for_calculating_variance). This algorithm will be more efficient compared to making two passes through the data, once to compute the mean and a second time to compute the standard deviation. Consider what objects and or data structures you might need to handle all of this data.

The output will be a CSV file. If the file doesn't exist, your program must create the file and write to it. Otherwise if the file exists just rewrite the contents of it. The output file will contain a CSV header at the top in the format of `song,number of ratings,mean,standard deviation`. Each row after the header will contain the songs in lexicographical order with each song name, number of ratings for that song, rating mean, and rating standard deviation as comma separated values.

When outputting the mean and standard deviation do not round or truncate the values. In other words print the double as is.

| song | number of ratings | mean | standard deviation |
| ---- | ----------------- | ---- | ------------------ |
| song1 | 3 | 3.14 | 1.8032432 |
| song2 | 5 | 2.98432 | 1.234 |
| ... |

## Floating Point Precision
* You should use either `double` or `Double` for floating-point numbers.
* Floating-point results must match the expected output to **6** decimal places. In other words, the absolute difference between the expected and actual results must not exceed **9e-7**.
* To verify your results with JUnit, see [`assertEquals(double expected, double actual, double delta)`](https://junit.org/junit5/docs/5.0.1/api/org/junit/jupiter/api/Assertions.html#assertEquals-double-double-double-).

## Error Handling
* While there are many potential error cases to handle, for this PA you can assume the input file, number of args, CSV entries are just as the program expects.
* In the next PA you will explore the potential issues and how to handle those errors.

## Running Your Program
Your program must accept exactly 2 arguments. The first argument is the input CSV file and the second argument is the output CSV file. To accept command line arguments, utilize the `String[] args` parameter in your main method. 

Do not hard code the input or output files. The user of your program must be able to specify which input CSV they want to read from and where and what name they want for the output CSV.

If the user does not specify a path and only puts the file name then your program your program must create the file and write to it, if it doesn't already exist. Otherwise if the file exists just rewrite the contents of it. A file with no path will go to the current directory the user runs your program from, in other words your `pa1-<username>` directory.

# How to Compile and Run

`gradle build`

`gradle run -q --args="<input-file> <output-file>"`

Some examples of arguments to run your program
* `gradle run -q --args="'database/files/file1.csv' 'output/output.csv'"`
* `gradle run -q --args="'database/files/file2.csv' 'myOutput.csv'"`
* `gradle run -q --args="'database/files/file2.csv' 'directory/anotherDirectory/songStats.csv'"`

# Example Input/Output

file1.csv
```
song1,user1,3
song1,user2,2
song1,user3,3
song1,user4,5
song2,user1,5
song2,user2,4
song2,user3,2
song2,user4,5
```

output1.csv
```
song,number of ratings,mean,standard deviation
song1,4,3.25,1.0897247358851685
song2,4,4.0,1.224744871391589

```

file2.csv
```
Bohemian Rhapsody,charlie,4
Sweet Home Alabama,alex,4
All Star,alex,4
All Star,charlie,2
Sweet Home Alabama,cameron,2
Sweet Home Alabama,charlie,3
```

output2.csv
```
song,number of ratings,mean,standard deviation
All Star,2,3.0,1.0
Bohemian Rhapsody,1,4.0,0.0
Sweet Home Alabama,3,3.0,0.816496580927726
```

# Submitting Your Homework
Your submission must be in the main branch of your GitHub repository.

# Grading Your Homework
We will pull your code from your repository. It must contain, at a minimum two Java files. One must be named `Cs214Project.java`. We will run your program by starting it with that class name. The second file must be named `TestCs214Project.java`. This will be used to run your JUnit tests. Read more on the [JUnit 5 documentation](https://junit.org/junit5/docs/current/user-guide/#writing-tests) on how to write more tests for your assignment. All future assignments must contain these two files at the very least. Although they may contain different code as needed by the particulars of that assignment.

However, we suggest adding more classes/functions as you think about the design of your program. To write clean, maintainable code, you should aim to break your solution into multiple, logically-organized classes and functions where appropriate. Here are a few guidelines to follow:
  * Use multiple classes to encapsulate related behaviors and data.
  * Create functions to avoid repetitive code and to make your solution more modular and readable.
  * Follow the Single Responsibility Principle: each class and function should have one clear purpose.
  * Keep your main method focused on orchestrating the flow of the program, rather than handling all the details.

By doing so, your code will not only be easier to read and debug, but it will also help you build a strong foundation in software design principles.

# Hints
* Use the biased sample variance when you calculate your standard deviation, so divide by $N$, not $N - 1$. This is population standard deviation, as opposed to sample standard deviation.
* When writing JUnit tests for testing different CSV files be sure to use the _relative_ path to the CSV, not the _absolute_ path. Meaning do not define the path to your input CSV all the way from the root directory. Define it from where you are to run your gradle commands. For example, you can use the relative path `./database/files/` rather than `/s/bach/l/under/<linux-username>/cs214/<pa1-<github-username>/database/files/`. The TAs must be able to run your code in their environment. Your code will not compile on a TA's machine if you include the absolute path.
* **Do not round the mean or standard deviation**. Keep in mind that `printf` methods and `String.format` will round your numbers.
* We have provided you some CSVs to test your program. To further test your program you must create your own CSV files.
* Test and develop your program on the department Linux machines. That is where we will evaluate it. Your grade depends on how your program performs on those machines. (Warning: Java may behave differently across different environments.)
