# HCL-Specific Coding Questions for BTech Campus Recruitment

This document provides a curated list of coding questions commonly asked by HCL Technologies in BTech campus recruitments, based on patterns observed in recent placement drives (2023–2025). These questions test problem-solving skills, algorithmic thinking, and proficiency in programming languages like C, C++, Java, or Python. Each question includes a problem description, input/output examples, approach, sample code, and relevant metadata.

---

## 1. Move Hash to Front
- **Problem**: Given a string containing some '#' characters, write a function to move all the '#' characters to the front of the string and return the modified string.
- **Input**: A string, e.g., "Move#Hash#to#Front"
- **Output**: The string with all '#' moved to the front, e.g., "###MoveHashtoFront"
- **Approach**: 
  - Iterate through the string.
  - Collect '#' characters in one array and non-'#' characters in another.
  - Concatenate the '#' array with the non-'#' array to form the result.
- **Sample Code (C)**:
  ```c
  #include <stdio.h>
  #include <string.h>
  char* moveHash(char str[], int n) {
      char str1[100], str2[100];
      int j = 0, k = 0;
      for (int i = 0; str[i]; i++) {
          if (str[i] == '#') str1[j++] = str[i];
          else str2[k++] = str[i];
      }
      str1[j] = '\0';
      str2[k] = '\0';
      strcat(str1, str2);
      printf("%s", str1);
      return str1;
  }
  int main() {
      char a[100];
      scanf("%[^\n]s", a);
      moveHash(a, strlen(a));
      return 0;
  }
  ```
- **Companies**: HCL
- **Difficulty**: Easy
- **Topic**: String Manipulation

---

## 2. Borrow Number
- **Problem**: Given two numbers, number1 and number2, calculate the number of borrow operations needed when subtracting number2 from number1. If subtraction is not possible (number1 < number2), return "Not possible".
- **Input**: Two integers, e.g., number1 = 754, number2 = 658
- **Output**: Number of borrow operations, e.g., 2, or "Not possible"
- **Approach**: 
  - Compare digits of both numbers from right to left.
  - If a digit in number1 is less than the corresponding digit in number2 (or adjusted due to a previous borrow), increment the borrow count and adjust the next digit.
  - If number1 < number2, return "Not possible".
- **Sample Code (C)**:
  ```c
  #include <stdio.h>
  int main() {
      int number1, number2, count = 0, flag = 0;
      scanf("%d %d", &number1, &number2);
      if (number1 < number2) {
          printf("Not possible");
      } else {
          while (number1 != 0 && number2 != 0) {
              int temp1 = number1 % 10;
              int temp2 = number2 % 10;
              if (flag == 1) temp1 -= 1;
              if (temp1 < temp2) {
                  count++;
                  flag = 1;
              } else {
                  flag = 0;
              }
              number1 /= 10;
              number2 /= 10;
          }
          printf("%d", count);
      }
      return 0;
  }
  ```
- **Companies**: HCL
- **Difficulty**: Medium
- **Topic**: Number Theory

---

## 3. Capitalize/Decapitalize
- **Problem**: Given a string and a character c, replace all occurrences of c in the string. If c is uppercase, convert it to lowercase; if it is lowercase, convert it to uppercase.
- **Input**: A string and a character, e.g., str = "hello world", c = 'l'
- **Output**: Modified string, e.g., "heLLo worLd"
- **Approach**: 
  - Iterate through the string.
  - For each occurrence of the character c, check its case (ASCII range: 65–90 for uppercase, 97–122 for lowercase).
  - Toggle the case by adding or subtracting 32 to the ASCII value.
- **Sample Code (C)**:
  ```c
  #include <stdio.h>
  #include <string.h>
  void changeCharacter(char str[], char chr, int len) {
      for (int i = 0; i < len; i++) {
          if (chr == str[i]) {
              if (str[i] >= 65 && str[i] <= 90) str[i] += 32;
              else if (str[i] >= 97 && str[i] <= 122) str[i] -= 32;
          }
      }
      printf("%s", str);
  }
  int main() {
      char str[1000], chr;
      scanf("%[^\n]s", str);
      scanf(" %c", &chr);
      changeCharacter(str, chr, strlen(str));
      return 0;
  }
  ```
- **Companies**: HCL
- **Difficulty**: Easy
- **Topic**: String Manipulation

---

## 4. Reduce String with Repeated Characters
- **Problem**: Given a string with consecutively repeated characters, reduce it by representing each sequence of repeated characters as the character followed by its count (if count > 1).
- **Input**: A string, e.g., "abbccccc"
- **Output**: Reduced string, e.g., "ab2c5"
- **Approach**: 
  - Iterate through the string, counting consecutive occurrences of each character.
  - If a character appears once, output it as is; if more than once, output the character followed by its count.
- **Sample Code (C)**:
  ```c
  #include <stdio.h>
  int main() {
      char str[100], str1[100];
      scanf("%[^\n]s", str);
      int k = 0, count;
      for (int i = 0; str[i] != '\0'; i++) {
          count = 0;
          for (int j = 0; j <= i; j++) {
              if (str[i] == str[j]) count++;
          }
          if (count == 1) str1[k++] = str[i];
      }
      str1[k] = '\0';
      for (int i = 0; str1[i] != '\0'; i++) {
          count = 0;
          for (int j = 0; str[j] != '\0'; j++) {
              if (str1[i] == str[j]) count++;
          }
          if (count == 1) printf("%c", str1[i]);
          else printf("%c%d", str1[i], count);
      }
      return 0;
  }
  ```
- **Companies**: HCL
- **Difficulty**: Medium
- **Topic**: String Compression

---

## 5. Spiral Matrix
- **Problem**: Given a 2D matrix, traverse it in a spiral order (clockwise from top-left).
- **Input**: A 2D matrix, e.g., [[1,2,3,4], [5,6,7,8], [9,10,11,12], [13,14,15,16], [17,18,19,20]]
- **Output**: Elements in spiral order, e.g., [1,2,3,4,8,12,16,20,19,18,17,13,9,5,6,7,11,15,14,10]
- **Approach**: 
  - Use four pointers (row_start, row_end, col_start, col_end) to track boundaries.
  - Traverse right, down, left, and up while updating boundaries and checking for overlap.
- **Sample Code (C)**:
  ```c
  #include <stdio.h>
  int main() {
      int a[5][4] = {{1,2,3,4},{5,6,7,8},{9,10,11,12},{13,14,15,16},{17,18,19,20}};
      int rs = 0, re = 5, cs = 0, ce = 4;
      while (rs < re && cs < ce) {
          for (int i = cs; i < ce; i++) printf("%d ", a[rs][i]);
          rs++;
          for (int i = rs; i < re; i++) printf("%d ", a[i][ce-1]);
          ce--;
          if (rs < re) {
              for (int i = ce-1; i >= cs; i--) printf("%d ", a[re-1][i]);
              re--;
          }
          if (cs < ce) {
              for (int i = re-1; i >= rs; i--) printf("%d ", a[i][cs]);
              cs++;
          }
      }
      return 0;
  }
  ```
- **Companies**: HCL
- **Difficulty**: Hard
- **Topic**: Matrix Traversal

---

## 6. Sum of Digits in a String
- **Problem**: Given a string containing a mix of alphabets, digits, and special characters, calculate the sum of all digits.
- **Input**: A string, e.g., "abc123def45"
- **Output**: Sum of digits, e.g., 15
- **Approach**: 
  - Iterate through the string.
  - Check if each character is a digit using `isdigit()` or equivalent, and add its numeric value to the sum.
- **Sample Code (Python)**:
  ```python
  def sum_of_digits(s):
      total = 0
      for ch in s:
          if ch.isdigit():
              total += int(ch)
      return total
  print(sum_of_digits("abc123def45"))  # Output: 15
  ```
- **Companies**: HCL
- **Difficulty**: Easy
- **Topic**: String Processing

---

## 7. Reverse Each Word in a Sentence
- **Problem**: Reverse each word in a sentence while keeping the word order intact.
- **Input**: A sentence, e.g., "hello world"
- **Output**: Reversed words, e.g., "olleh dlrow"
- **Approach**: 
  - Split the sentence into words.
  - Reverse each word individually and join them back with spaces.
- **Sample Code (Python)**:
  ```python
  def reverse_words(sentence):
      words = sentence.split()
      result = [word[::-1] for word in words]
      return ' '.join(result)
  print(reverse_words("hello world"))  # Output: olleh dlrow
  ```
- **Companies**: HCL
- **Difficulty**: Easy
- **Topic**: String Manipulation

---

## 8. Count Vowels in a String
- **Problem**: Count the number of vowels (a, e, i, o, u) in a string, ignoring case.
- **Input**: A string, e.g., "Education"
- **Output**: Number of vowels, e.g., 5
- **Approach**: 
  - Iterate through the string.
  - Check if each character is in a predefined set of vowels (case-insensitive).
- **Sample Code (Python)**:
  ```python
  def count_vowels(s):
      vowels = 'aeiouAEIOU'
      count = 0
      for ch in s:
          if ch in vowels:
              count += 1
      return count
  print(count_vowels("Education"))  # Output: 5
  ```
- **Companies**: HCL
- **Difficulty**: Easy
- **Topic**: String Processing

---

## 9. Check Palindrome String
- **Problem**: Determine if a given string is a palindrome (reads the same forward and backward), ignoring case and assuming no spaces or symbols.
- **Input**: A string, e.g., "madam"
- **Output**: True if palindrome, else False
- **Approach**: 
  - Convert the string to lowercase.
  - Compare the string with its reverse.
- **Sample Code (Python)**:
  ```python
  def is_palindrome(s):
      return s.lower() == s[::-1].lower()
  print(is_palindrome("madam"))  # Output: True
  ```
- **Companies**: HCL
- **Difficulty**: Easy
- **Topic**: String Manipulation

---

## 10. Find Maximum Number in a List
- **Problem**: Given a list of integers, find the largest number using a simple loop.
- **Input**: A list of integers, e.g., [10, 20, 45, 6, 78]
- **Output**: Maximum number, e.g., 78
- **Approach**: 
  - Initialize the maximum as the first element.
  - Iterate through the list, updating the maximum if a larger number is found.
- **Sample Code (Python)**:
  ```python
  def find_max(numbers):
      max_val = numbers[0]
      for num in numbers:
          if num > max_val:
              max_val = num
      return max_val
  print(find_max([10, 20, 45, 6, 78]))  # Output: 78
  ```
- **Companies**: HCL
- **Difficulty**: Easy
- **Topic**: Array Processing

---

## HCL Coding Round Details
- **Format**: Typically includes 2 coding questions to be solved in 20 minutes, alongside 30 computer programming MCQs (30 minutes) covering topics like Data Structures, OOPS, OS, DBMS, and Networking.
- **Difficulty**: Easy to Medium, with occasional Hard questions like Spiral Matrix.
- **Languages Allowed**: C, C++, Java, Python (choose one based on comfort).
- **Eligibility Criteria**: B.Tech (CSE, IT, ECE, EN, EI, ME) with 75% throughout academics and no active backlogs.
- **Key Topics**:
  - **String Manipulation**: Reversing, palindrome checks, character counting, case conversion.
  - **Arrays**: Maximum/minimum elements, merging, or traversal.
  - **Number Theory**: Prime numbers, factorial, GCD, digit sums.
  - **Matrix**: Traversal (spiral, row-wise, column-wise).
  - **Basic Data Structures**: Linked lists, stacks, or queues may appear in MCQs or interviews.

---

## Preparation Tips
1. **Practice on Platforms**: Use PrepInsta, GeeksforGeeks, or HackerRank to practice HCL-specific questions. Focus on string and array problems, as they are frequent.
2. **Time Management**: With only 20 minutes for 2 coding questions, prioritize writing efficient code and testing edge cases (e.g., empty strings, negative numbers).
3. **Understand Basics**: Be thorough with string operations, loops, conditionals, and basic data structures. HCL emphasizes implementation over complex algorithms.
4. **Mock Tests**: Take HCL-specific mock tests on platforms like Sanfoundry or Unstop to simulate the actual test environment.
5. **Debugging Skills**: Ensure your code handles edge cases (e.g., invalid inputs, large numbers) and passes all test cases, as HCL’s coding platform evaluates based on test case accuracy.

---

## Additional Notes
- **Interview Rounds**: After the coding round, expect a technical interview focusing on coding, data structures, and CS fundamentals (OS, DBMS, Networking), followed by an HR round.
- **Resources**: 
  - PrepInsta’s HCL Coding Questions Dashboard for practice problems.
  - GeeksforGeeks HCL SDE Sheet for a comprehensive problem list.
  - NextGenKodingHub for Python and Java solutions to HCL questions.
- **Company Context**: HCL is a mass recruiter, and its coding questions are generally simpler than those of product-based companies like Google or Amazon, focusing on practical implementation skills.
