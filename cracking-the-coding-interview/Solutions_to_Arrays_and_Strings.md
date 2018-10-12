1. Is Unique: Implement an algorithm to determine if a string has all unique characters. What if you cannot use additional data structures?
#### Solution 1
```
boolean isUniqueChars(String str){
    int checker = 0;
    for(int i=0; i< str.length(); i++){
        int val = str.charAt(i) - 'a';
        if(checker & (1 << val) > 0){
            return false;
        }
        checker |= (1 << val);
    }
    return true;
}
```

2. Check Permutation: Given two strings, write a method to decide if one is a permutation of the other.
#### Solution 1: Sort the strings.
```
String sort(String s){
    char[] content = s.toCharArray();
    java.util.Arrays.sort(content);
    return new String(content);
}

boolean permutation(String s, String t){
    if(s.length() != t.length()){
        return false;
    }
    return sort(s).equals(sort(t));
}
```
#### Solution 2: Check if the two strings have identical character counts.
```
boolean permutation(String s, String t){
    if(s.length() != t.length()){
        return false;
    }
    int[] letters = new int[128];
    char[] s_array = s.toCharArray();
    for(char c: s_array){  // count number of each char in s.
        letters[c] ++;
    }
    for(int i=0; i<t.length();i++){
        int c = (int)t.charAt(i);
        letters[c] --;
        if(letters[c]<0){
            return false;
        }
    }
    return true;
}
```

3. URLify: wirte a method to replace all spaces in a string with '%20'.
```
    public static String URLify(String s) {
        int len = s.length();
        int count = 0;
        for(int i=0;i<len;i++){
            char val = s.charAt(i);
            if(val==' '){
                count ++;
            }
        }
        int new_len = count * 2 + len;
        char[] res = new char[new_len];
        int p1 = len-1;
        int p2 = new_len-1;
        while(p1>=0){
            if(s.charAt(p1)!=' '){
                res[p2--] = s.charAt(p1--);
            }
            else{
                res[p2--] = '0';
                res[p2--] = '2';
                res[p2--] = '%';
                p1 --;
            }
        }
        return new String(res);
    }
```

4. Palindrome Permutation: Given a string, write a function to check if it is a permutation of a palindrome. 
```
boolean isPermutationOfPalindrome(String phrase){
    int[] table = buildCharFrequencyTable(phrase);
    return checkMaxOneOdd(table);
}

/* Check that no more that one character has an odd count */
boolean checkMaxOneOdd(int[] table){
    boolean foundOdd = false;
    for(int count : table){
        if(count % 2 == 1){
            if(fountOdd){
                return false;
            }
            foundOdd = true;
        }
    }
    return true;
}


int getCharNumber(Character c){
    int a = Character.getNumericValue('a');
    int a = Character.getNumericValue('z');
    int val = Character.getNumericValue(c);
    if(a <= val && val<= z){
        return val - a;
    }
    return -1;
}

/* Count how many times each character appears */
int[] buildFrequencyTable(String phrase){
    int[] table = new int[Character.getNumericValue('z') - Character.getNumericValue('a')+1];
    for(char c: phrase.toCharArray()){
        int x  = getCharNumber(c);
        if(x!=-1){
            table[x] ++;
        }
    }
    return table;
}
```
5. One Away: Given two strings, write a function to check if they are One edit(or zero edit) away.
```
boolean oneEditAway(String first, String second){
    if(first.length() == second.length())
        return oneEditReplace(first,second);
    if(first.length()+1 == second.length())
        return oneEditInsert(first,second);
    if(first.length() == second.length())
        return oneEditInsert(second,first);
}

boolean oneEditReplace(String s1, String s2){
    boolean foundDifference = false;
    for(int i=0;i<s1.length();i++){
        if(s1.charAt(i)!=s2.charAt(i))
        {
            if(foundDifference){
                return false;
            }
            foundDifference = true;
        }
    }
    return true;
}

boolean oneEditInsert(String s1, String s2){
    int index1 = 0;
    int index2 = 0;
    while(index2 < s2.length() && index1 < s1.length()){
        if(s1.charAt(index1)!= s2.charAt(index2)){
            if(index1!=index2){
                return false;
            }
            index2 ++;
        }
        else{
            index1 ++;
            index2 ++;
        }
    }
    return true;
}

boolean OneEditAway(String first, String second){
    if(Math.abs(first.length() - second.lengyh())> 1) return false;
    String s1 = first.length() < second.length() ? first : second;
    String s2 = first.length() < second.length() ? second : first;
    
    int index1 = 0;
    int index2 = 0;
    boolean foundDifference = false;
    while(index2 < s2.length() && index1 < s1.length()){
        if(s1.charAt(index1) != s2.charAt(index2)){
            /* Ensure that this is the first difference found */
            if(foundDiffernce) return false;
            if(s1.length() == s2.length() ){  // On replace, move shorter pointer
                index1 ++;
            }
        }else{
            index1 ++; // If matching, move shorter pointer
        }
        index2 ++;  // Always move pointer for longer string
    }
    return true;
}
```

6. String Compression: Implement a method to perform basic string compression using the counts of repeated characters. For example, the
string aabcccccaaa would become a2b1c5a3. If the "compressed" string would not become smaller than the original string, your method would
return the original string.

```
String compress(String str){
    /* Check final length and return input string if it would be longer. */
    int finalLength = countCompression(str);
    if(finalLength >= str.length()) return str;

    StringBuilder compressed = new StringBuilder(finalLength);
    /* One benefit of this approach is that we can initialize StringBuilder to its necessary capacity up-front. 
    Without this, StringBuilder will(behind the scenes) need to double its capacity every time it hits capacity. 
    The capacity could be double what we ultimately need. 
    */
    int countConsecutive = 0;
    for(int i=0; i<str.length(); i++){
        countConsecutive ++;
        if(i+1 >= str.length() || str.charAt(i) != str.charAt(i+1)){
            compressed.append(str.charAt(i));
            compressed.append(countConsecutive);
            countConsecutive = 0;
        }
    }
    return compressed.toString();
}

int countCompression(String str){
    int compressedLength = 0;
    int countConsecutive = 0;
    for(int i=0;i< str.length(); i++){
        countConsecutive ++;
        /* If next character is different than current, increase the length. */
        if( i+1 > str.length() || str.charAt(i) != str.charAt(i+1)){
            compressedLength += 1 + String.valueOf(countConsecutive).length();
            countConsecutive = 0;
        }
    }
    return compressedLength;
}


```

7. Rorate Matrix: Given an image represented by an NxN matrix, where each pixel in the image is 4 bytes, write a method
to rorate the image by 90 degrees. Can you do this in place?
```
boolean rotate(int[][] matrix){
    if(matrix.length == 0 || matrix.length!= matrix[0].length) return false;
    int n = matrix.length;
    for(int layer = 0; layer < n/2 ;layer ++){
        int first = layer;
        int last = n - 1 - layer;
        for(int i=first; i< last ; i++){
            int offset = i - first;
            int top = matrix[first][i]; // save top
            
            // left -> top
            matrix[first][i] = matrix[last-offset][first];
            
            // bottom -> left
            matrix[last-offset][first] =matrix[last][last-offset];
            
            // right -> bottom
            matrix[last][last - offset] = matrix[i][last];

            // top -> right
            matrix[i][last] = top; // right <- saved top;
        }
    }
    return true;
}
This algorithm is O(N^2), which is the best we can do since any algorithm must touch all N^2 elements.
```

8. Zero Matrix: Wirte an algorithm such that if an element in an N x N matrix is 0, its entire row and column are set to 0.

9. String Rotation: Assume you have a method isSubstring which checks if one word is a substring of another. Given two strings,
s1 and s2, write code to check if s2 is a rotation of s1 using only one call to isSubstring(e.g, "waterbottle" is a rotation of "erbottlewat").
```
boolean isRotation(String s1, String s2){
    int len = s1.length();
    /* Check that s1 and s2 are equal length and not empty */
    if(len == s2.length() && len>0){
        /* Concatenate s1 and s1 within new buffer */
        String s1s1 = s1 + s1;
        return isSubstring(s1s1,s2);
    }
    return false;
}
```