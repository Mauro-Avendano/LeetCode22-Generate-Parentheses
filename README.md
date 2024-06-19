# LeetCode22-Generate-Parentheses

We will discuss a solution for the [generate parentheses problem](https://leetcode.com/problems/generate-parentheses/description/).

The instruction **generate all combinations** is a really good hint about a possible recursion so we will use the backtracking algorithm. 

First note the condition for a valid solution, if n = 3 for example, we will need 3 open an 3 close parentheses so if the length of the current string is equal to n*2 we are in presence of a valid solution.
The rest is pretty much the standard template for a backtracking algorithm: 
  - a search function wich contains the recursion.
  - a getCandidates to keep expanding the tree with valid combinations.
  - an isValid to check if we are in a leaf (solution) of our recursion tree.

Please also note that the search function needs to return the state to a previous node of the recursion tree (backtrack).

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> output = new ArrayList<>();
        StringBuilder state = new StringBuilder();
        int open = 0;
        int close = 0;

        search(output, state, n, open, close);

        return output;
    }

    private void search(List<String> output, StringBuilder state, int n, int open, int close) {
        if (isValid(state, n)) {
            output.add(state.toString());
            return;
        }

        for (Character candidate : getCandidates(open, close, n)) {
            if (candidate == '(') open++;
            else close++;

            state.append(candidate);
            search(output, state, n, open, close);
            if(state.charAt(state.length() - 1) == '(') open--;
            state.deleteCharAt(state.length() - 1);
        }
    }

    private List<Character> getCandidates(int open, int close, int n) {
        List<Character> candidates = new ArrayList<>();
        
        if (open == 0) candidates.add('(');

        else {
            if (open < n) {
                candidates.add('(');
                if (open > close) candidates.add(')');
            } else if (close < open) {
                candidates.add(')');
            } 
        }

        return candidates;
    }

    private boolean isValid(StringBuilder state, int n) {
        if (state.length() == n*2) return true;
        return false;
    }
}
```

Another similar solution is also possible with much less code but maybe more hard to see:

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> result = new ArrayList<>();
        int open = 0;
        int close = 0;

        search(result, n, "", open, close);

        return result;   
    }

    private void search(List<String> result, int n, String state, int open, int close) {
        if (state.length() == n*2) {
            result.add(state.toString());
            return;
        }
        
        if (open < n) search(result, n, state + '(', open + 1, close);
        if (close < open) search(result, n, state + ')', open, close + 1);
    }
}
```

It is interesting to note that the return of the state to perform the backtrack is not being doing explicity like in the previous solution, this is because 
the String in Java are immutable so on each call to the search function we are passing a copy of the current string. You can read more about how String are managed in Java in the following [article](https://www.geeksforgeeks.org/passing-strings-by-reference-in-java/).

The time complexity here is O(2<sup>2n</sup>) where 2 is the max number of branches of each node and 2n is the depth of the recursion tree.

For the space complexity we need to check the max size of the stack which is 2n so space complexity is O(n). 

## More parentheses problems: 
   - [LeetCode20-Valid-Parentheses](https://github.com/Mauro-Avendano/LeetCode20-Valid-Parentheses)

## More recusion problems:
  - [LeetCode17-Letters-combinations-of-a-phone-number](https://github.com/Mauro-Avendano/LeetCode17-Letters-combinations-of-a-phone-number)
