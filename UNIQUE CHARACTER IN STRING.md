
---

### **Approach 1: Using a Hash Map**
- **Explanation**: This approach uses a hash map to store the frequency of each character. We then create a new string with only the characters that appear once.

```cpp
#include <iostream>
#include <unordered_map>
#include <string>

char* uniqueCharactersUsingMap(char* str) {
    std::unordered_map<char, int> charCount;
    int i = 0;
    // Count the occurrences of each character
    while (str[i] != '\0') {
        charCount[str[i]]++;
        i++;
    }
    
    // Create a new string with unique characters
    std::string result = "";
    i = 0;
    while (str[i] != '\0') {
        if (charCount[str[i]] == 1) {
            result += str[i];
        }
        i++;
    }

    // Return a pointer to a newly created char array
    char* uniqueChars = new char[result.length() + 1];
    std::strcpy(uniqueChars, result.c_str());
    return uniqueChars;
}

// Dry Run
// Input: "programming"
// Step 1: Count occurrences: p=1, r=2, o=1, g=2, m=2, i=1, n=1
// Step 2: Append unique characters: "poin" → Output: "poin"
```

---

### **Approach 2: Two-Pointer Technique (Left to Right)**
- **Explanation**: We use a slow and a fast pointer. The fast pointer traverses the string, and if the character at the fast pointer hasn’t been seen before, we move the slow pointer and overwrite the slow pointer with the unique character. This approach is typically used in-place.

```cpp
#include <iostream>
#include <unordered_set>
#include <string>

char* uniqueCharactersTwoPointer(char* str) {
    std::unordered_set<char> seen;
    int slow = 0, fast = 0;

    while (str[fast] != '\0') {
        if (seen.find(str[fast]) == seen.end()) {
            seen.insert(str[fast]);
            str[slow] = str[fast];
            slow++;
        }
        fast++;
    }

    str[slow] = '\0'; // Null-terminate the string after unique characters
    return str;
}

// Dry Run
// Input: "programming"
// Step 1: slow = 0, fast = 0; seen = {p}; str = "progr"
// Step 2: Continue moving fast, adding unique to seen and copying
// Result: str becomes "progain" with duplicates removed.
```

---

### **Approach 3: Reverse Two-Pointer Technique**
- **Explanation**: Here, we start from the end of the string and use two pointers moving toward each other. We check for uniqueness from the back of the string to find unique characters without needing additional memory.

```cpp
#include <iostream>
#include <unordered_set>
#include <string>

char* uniqueCharactersReverseTwoPointer(char* str) {
    std::unordered_set<char> seen;
    int slow = strlen(str) - 1;
    int fast = strlen(str) - 1;

    while (fast >= 0) {
        if (seen.find(str[fast]) == seen.end()) {
            seen.insert(str[fast]);
            str[slow] = str[fast];
            slow--;
        }
        fast--;
    }

    // Move unique characters to the start of the string
    int newStart = slow + 1;
    for (int i = 0; i < strlen(str) - newStart; i++) {
        str[i] = str[newStart + i];
    }
    str[strlen(str) - newStart] = '\0'; // Null-terminate

    return str;
}

// Dry Run
// Input: "programming"
// Step 1: Fast and slow pointers move from the end
// Seen = {g}, then {m}, {n}, etc. → str = "gnmrip"
// Result: str = "gnmrip" with duplicates removed.
```

---

### Summary
- **Hash Map**: Simple and effective but uses extra memory.
- **Two-Pointer (Left to Right)**: In-place, better memory use but slightly complex.
- **Reverse Two-Pointer**: Starts from the end, good for reducing duplicate scans.
