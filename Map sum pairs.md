# Description
Design a map that allows you to do the following:

Maps a string key to a given value.
Returns the sum of the values that have a key with a prefix equal to a given string.
Implement the MapSum class:

MapSum() Initializes the MapSum object.
void insert(String key, int val) Inserts the key-val pair into the map. If the key already existed, the original key-value pair will be overridden to the new one.
int sum(string prefix) Returns the sum of all the pairs' value whose key starts with the prefix.
 

**Example 1:**

Input
```
["MapSum", "insert", "sum", "insert", "sum"]
[[], ["apple", 3], ["ap"], ["app", 2], ["ap"]]
```
Output
```
[null, null, 3, null, 5]
```
Explanation
```
MapSum mapSum = new MapSum();
mapSum.insert("apple", 3);  
mapSum.sum("ap");           // return 3 (apple = 3)
mapSum.insert("app", 2);    
mapSum.sum("ap");           // return 5 (apple + app = 3 + 2 = 5)
``` 

**Constraints:**
* 1 <= key.length, prefix.length <= 50
* key and prefix consist of only lowercase English letters.
* 1 <= val <= 1000
* At most 50 calls will be made to insert and sum.

# Solution2
Trie
```java
class MapSum {

    private Trie trie;
    
    HashMap<String, Integer> map;

    /** Initialize your data structure here. */

    private class Trie {
        int val;
        Trie[] children;

        public Trie(int val) {
            this.val = val;
            this.children = new Trie[26];
            for (Trie trie: children) {
                trie = null;
            }
        }
    }

    public MapSum() {
        this.trie = new Trie(0);
        map = new HashMap<>();
    }

    public void insert(String word, int val) {
        Trie cur = this.trie;
        for (int i = 0; i < word.length(); i++) {
            char ch = word.charAt(i);
            if (cur.children[ch - 'a'] == null) {
                cur.children[ch - 'a'] = new Trie(val);
            } else {
                if (!map.containsKey(word)) {
                    cur.children[ch - 'a'].val += val;
                } else {
                    cur.children[ch - 'a'].val -= map.get(word);
                    cur.children[ch - 'a'].val += val;
                }
                
            }
            cur = cur.children[ch - 'a'];
        }
        map.put(word, val);
    }
    
    public int sum(String prefix) {
        Trie cur = this.trie;
        for (int i = 0; i < prefix.length(); i++) {
            char ch = prefix.charAt(i);
            if (cur.children[ch - 'a'] == null) {
                return 0;
            }
            cur = cur.children[ch - 'a'];
        }
        return cur.val;
    }
}
```
# Complexity
```
Time complexity: O(m) where m is the maximun length of the inserted world
Space complexity: O(26 ^ m) where m is the maximum length of the inserted world
```

# Solution3
```java
class MapSum {

        public Node root;
        class Node {
            boolean isEnd;
            char letter;
            int val;
            int sum;
            Node[] next;
            public Node(char c) {
                this.letter = c;
                this.val = 0;
                this.sum = 0;
                this.next = new Node[26];
                isEnd = false;
            }
        }
        /** Initialize your data structure here. */
        public MapSum() {
            this.root = new Node('T');
        }

        public void insert(String key, int val) {
            dpsInsert(root, key, val, 0);
            print(root, 0);
        }

        private void print(Node curr, int prefix) {
            for (int i = 0; i < prefix; i++) {
                System.out.print(" ");
            }
            System.out.println(curr.letter + "\tval=" + curr.val + "\tsum=" +curr.sum);
            for(int i=0;i<26;i++){
                if(curr.next[i] != null) {
                    print(curr.next[i], prefix+2);
                }
            }
        }

        /**
         *
         * @param node father node
         * @param key string
         * @param val value
         * @param index index of key, the next char to insert
         */
        private int dpsInsert(Node node, String key, int val, int index) {
            if (index >= key.length()) return -1;
            char c = key.charAt(index);
            if (null == node.next[c-'a']) {
                node.next[c-'a'] = new Node(c);
            }
            if (index == key.length()-1) {
                // mark as end point
                if (!node.next[c-'a'].isEnd) {
                    node.next[c-'a'].isEnd = true;
                    node.next[c - 'a'].sum += val;
                    node.next[c - 'a'].val = val;
                    return val;
                } else {
                    int delta = val - node.next[c - 'a'].val;
                    node.next[c - 'a'].val = val;
                    node.next[c - 'a'].sum += delta;
                    return delta;
                }
            }
            int delta = dpsInsert(node.next[c - 'a'], key, val, index + 1);
            node.next[c - 'a'].sum += delta;
            return delta;
        }

        public int sum(String prefix) {
            Node curr = root;
            for (char c : prefix.toCharArray()) {
                if (curr.next[c-'a'] == null) return 0;
                curr = curr.next[c-'a'];
            }
            System.out.println(curr.sum);
            return curr.sum;
        }
    }
```

# Complexity3
```
Time complexity: O(m) where m is the maximun length of the inserted world
Space complexity: O(26 ^ m) where m is the maximum length of the inserted world
```