# question #1
Given two strings s and t, both consisting of lowercase English letters and digits, 
your task is to calculate how many ways exactly one digit could be removed from one of the strings so that s is lexicographically smaller than t after the removal. 
Note that we are removing only a single instance of a single digit, rather than all instances (eg: removing 1 from the string a11b1c could result in a1b1c or a11bc, 
but not abc).

Also note that digits are considered lexicographically smaller than letters.

Example

For s = "ab12c" and t = "1zz456", the output should be solution(s, t) = 1.

Here are all the possible removals:

We can remove the first digit from s, obtaining "ab2c". "ab2c" > "1zz456", so we don't count this removal
We can remove the second digit from s, obtaining "ab1c". "ab1c" > "1zz456", so we don't count this removal
We can remove the first digit from t, obtaining "zz456". "ab12c" < "zz456", so we count this removal
We can remove the second digit from t, obtaining "1zz56". "ab12c" > "1zz56", so we don't count this removal
We can remove the third digit from t, obtaining "1zz46". "ab12c" > "1zz46", so we don't count this removal
We can remove the fourth digit from t, obtaining "1zz45". "ab12c" > "1zz45", so we don't count this removal
The only valid case where s < t after removing a digit is "ab12c" < "zz456". Therefore, the answer is 1.

# answers from github
https://github.com/nehalkarrar/codeSignal/blob/main/lexicographically%20smaller.py
```
def solution(s, t):
    num1 = ''.join(filter(str.isdigit, s))
    num1_lst = list(num1)
    
    num2 = ''.join(filter(str.isdigit, t))
    num2_lst = list(num2)
    
    count = 0
    
    for i in range(len(s)):
        if s[i] in num1_lst:
            new_s = s.replace(s[i], '')
            if new_s < t:
                count += 1
    
    for i in range(len(t)):
        if t[i] in num2_lst:
            new_t = t.replace(t[i], '')
            if s < new_t:
                count += 1
    
    return count

# my answers
```
int solution(String s, String t) {
    int result = 0;
    String compareString = "";
    int i;
    
    String[] s1 = removeDigits(s);
    for (i = 0; i < s1.length; i++) {
        compareString = s1[i];
        if (isLexicographicallySmaller(compareString, t)) {
            result++;
        }
    }
    
    String[] s2 = removeDigits(t);
    for (i = 0; i < s2.length; i++) {
        compareString = s2[i];
        if (isLexicographicallySmaller(s, compareString)) {
            result++;
        }
    }
    
    return result;
}

boolean isLexicographicallySmaller(String s, String t) {
    char[] charsOfS = s.toCharArray();
    char[] charsOfT = t.toCharArray();
    int lengthS = s.length();
    int lengthT = t.length();
    
    int length = Math.min(lengthS, lengthT);
    for (int i = 0; i < length; i++) {
        char charOfS = charsOfS[i];
        char charOfT = charsOfT[i];
        
        if (charOfS == charOfT) {
            continue;
        } else {
            return isLexicographicallySmaller(charOfS, charOfT);
        }
    }
    
    if (lengthS >= lengthT) {
        return false;
    }
    
    return true;
}

boolean isLexicographicallySmaller(char c1, char c2) {
    String allChars = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz";
    
    int indexOfC1 = allChars.indexOf(String.valueOf(c1));
    int indexOfC2 = allChars.indexOf(String.valueOf(c2));
    
    return indexOfC1 < indexOfC2;
}

String[] removeDigits(String s) {
    char[] charsOfOriginalString = s.toCharArray();
    List<String> resultList = new ArrayList<>();
    for (int i = 0; i < charsOfOriginalString.length; i++) {
        char c = charsOfOriginalString[i];
        if (isDigit(c)) {
            resultList.add(s.substring(0, i) + s.substring(i + 1));
        }
    }
    
    Object[] objs = resultList.toArray();
    String[] result = new String[objs.length];
    for (int i = 0; i < objs.length; i++) {
        result[i] = (String)objs[i];
    }
    
    return result;
}

boolean isDigit(char c) {
    String digits = "0123456789";
    return digits.indexOf(String.valueOf(c)) != -1;
}

```
