# Minimum-Number-of-Pushes-to-Type-Word-II

You are given a string word containing lowercase English letters.

Telephone keypads have keys mapped with distinct collections of lowercase English letters, which can be used to form words by pushing them. For example, the key 2 is mapped with ["a","b","c"], we need to push the key one time to type "a", two times to type "b", and three times to type "c" .

It is allowed to remap the keys numbered 2 to 9 to distinct collections of letters. The keys can be remapped to any amount of letters, but each letter must be mapped to exactly one key. You need to find the minimum number of times the keys will be pushed to type the string word.

Return the minimum number of pushes needed to type word after remapping the keys.

An example mapping of letters to keys on a telephone keypad is given below. Note that 1, *, #, and 0 do not map to any letters.

Example 1:


Input: word = "abcde"
Output: 5
Explanation: The remapped keypad given in the image provides the minimum cost.
"a" -> one push on key 2
"b" -> one push on key 3
"c" -> one push on key 4
"d" -> one push on key 5
"e" -> one push on key 6
Total cost is 1 + 1 + 1 + 1 + 1 = 5.
It can be shown that no other mapping can provide a lower cost.
Example 2:


Input: word = "xyzxyzxyzxyz"
Output: 12
Explanation: The remapped keypad given in the image provides the minimum cost.
"x" -> one push on key 2
"y" -> one push on key 3
"z" -> one push on key 4
Total cost is 1 * 4 + 1 * 4 + 1 * 4 = 12
It can be shown that no other mapping can provide a lower cost.
Note that the key 9 is not mapped to any letter: it is not necessary to map letters to every key, but to map all the letters.
Example 3:


Input: word = "aabbccddeeffgghhiiiiii"
Output: 24
Explanation: The remapped keypad given in the image provides the minimum cost.
"a" -> one push on key 2
"b" -> one push on key 3
"c" -> one push on key 4
"d" -> one push on key 5
"e" -> one push on key 6
"f" -> one push on key 7
"g" -> one push on key 8
"h" -> two pushes on key 9
"i" -> one push on key 9
Total cost is 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 2 * 2 + 6 * 1 = 24.
It can be shown that no other mapping can provide a lower cost.
 

Constraints:

1 <= word.length <= 105
word consists of lowercase English letters.


# SOLUTION 

The intuition behind this solution is to prioritize the characters with the highest frequency first, as they will require the most pushes. By processing the characters in descending order of frequency, we can minimize the total number of pushes required.

# Approach
Count the frequency of each character in the input string using a hash map .
Create a max heap (priority queue) based on the character frequencies.
Iterate through the characters in the max heap, calculating the minimum pushes required for each character based on its priority.
Return the total number of pushes calculated.

Time and Space Complexity

Time Complexity: O(n log k), where n is the length of the input string and k is the number of unique characters. The time complexity is dominated by the priority queue operations, which take O(log k) time for each character.

Space Complexity: O(k), where k is the number of unique characters. The space is used to store the character frequencies in the hash map and the priority queue.
Step-by-Step Explanation
(for word ="xyzxyzxyzxyz")

Character Counting: The frequency of each character is counted using a hash map:

'x': 4
'y': 4
'z': 4
Max Heap Creation: A max heap is created using the character frequencies:

The heap will contain the pairs: ('x', 4), ('y', 4), ('z', 4). The order may vary since they have the same frequency.
Push Calculation:

We will process the characters in order of their frequency and calculate the pushes based on the priority rules defined in the code.
Iteration Breakdown
Iteration 1 (j = 1):

Pop the first character (let's say 'x' with frequency 4).
Count = 0 + 4 = 4 (since j <= 8)
Iteration 2 (j = 2):

Pop the next character (let's say 'y' with frequency 4).
Count = 4 + 4 = 8 (since j <= 8)
Iteration 3 (j = 3):

Pop the next character (let's say 'z' with frequency 4).
Count = 8 + 4 = 12 (since j <= 8)
Iteration 4 (j = 4):

Pop 'x' again (frequency 4).
Count = 12 + 4 = 16 (since j <= 8)
Iteration 5 (j = 5):

Pop 'y' again (frequency 4).
Count = 16 + 4 = 20 (since j <= 8)
Iteration 6 (j = 6):

Pop 'z' again (frequency 4).
Count = 20 + 4 = 24 (since j <= 8)
Iteration 7 (j = 7):

Pop 'x' again (frequency 4).
Count = 24 + 4 = 28 (since j <= 8)
Iteration 8 (j = 8):

Pop 'y' again (frequency 4).
Count = 28 + 4 = 32 (since j <= 8)
Iteration 9 (j = 9):

Pop 'z' again (frequency 4).
Count = 32 + 4 = 36 (since j <= 8)
Iteration 10 (j = 10):

At this point, we have already processed all characters, and the counts are based on their frequencies.
Final Count
After processing all characters, the total count of pushes required is 12.

# JAVA CODE

class Solution {

    public int minimumPushes(String word) {

         HashMap<Character, Integer> map = new HashMap<>();
         
        for(int i=0; i<word.length(); i++){
        
            char c = word.charAt(i);
            
            map.put(c, map.getOrDefault(c, 0) + 1);

        }
        PriorityQueue<Map.Entry<Character, Integer>> maxHeap =
        
              new PriorityQueue<>((entry1, entry2) -> entry2.getValue().compareTo(entry1.getValue()));

        maxHeap.addAll(map.entrySet());
        
        int count = 0;
        
        for(int j=1; j <= map.size(); j++){
        
            Map.Entry<Character, Integer> entry = maxHeap.poll();
            
            if (j <= 8)  /*calculates pushes for used character that should be priotised first and so on for rest of condition according to priority*/
            
                count += entry.getValue();
                
            else if(j > 8 && j <= 16)
            
                count += entry.getValue()*2;
                
            else if(j > 16 && j <= 24)
            
                count += entry.getValue()*3;
                
            else
            
            count += entry.getValue()*4;
        }
        
        return count;
        
    }
}
