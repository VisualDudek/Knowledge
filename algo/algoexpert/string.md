# Strings

## Longest Palindromic Substring

Key-takeaways: combinations, substrings of string, odd and even palindrom

- pomyłka z time complexity, pierwsze rozwiązanie Time O\(n\*\*3\) a nie O\(n\*\*2\) + O\(n\)

- problem jaki miałem to jak oterować przez string aby sprawdzać centrum palindrom które może być pojedyńczą literą lub znajdować się pomiędzy literami, SOLUTION: dla każdej litery zaczynając od idx=1 a nie 0 generuj palindrom-check even and odd, ważne: palindrom-check musi sprawdzać czy jest in index-bound

```text
# pseudo-code

generate all substring (e.g. using combinations or double for-loop)
for each substring run isPalindrom
```

```text
# pseudo-code [a,b,c] OPTIMAL

need helper: getLongestPalindromFrom
 - check if idx in-bound
 - return at least one char
 
 smart traverse: start from idx=1 and always pass ab and abc
  check if we have longest if yes: update res
```

