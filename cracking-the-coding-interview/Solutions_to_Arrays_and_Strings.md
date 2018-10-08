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
