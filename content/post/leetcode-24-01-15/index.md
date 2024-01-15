---
title: Jan 15 2024 LeetCode Daily
description: https://leetcode.cn/problems/remove-duplicates-from-sorted-list-ii/description/
slug: leetcode-daily-24-01-15
date: 2024-01-15 00:00:00+0000 
image: 1.png
categories:
    - Algorithm
tags:
    - C++
    - LeetCode
    - Linked List
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---


## 解法一(可读性):
```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        // Special Case 1: Empty List
        if (head == nullptr) {
            return nullptr;
        }

        // Special Case 2: Only One Element (No Duplicates)
        if (head->next == nullptr) {
            return head;
        }

        // Special Case 3: Two Elements (Return Either None or Self)
        if (head->next->next == nullptr) {
            if (head->val == head->next->val) {
                return nullptr;
            } else {
                return head;
            }
        }
        
        // Initialzing pointers
        ListNode* pseudo_head = new ListNode(-777, head);
        ListNode* last_different = pseudo_head;
        ListNode* candidate = head;
        ListNode* preception = candidate->next;
        
        // Initializing the indicator
        int indicator = candidate->val == preception->val ? candidate->val : -777;

        // Setting end-loop condition
        while (preception != nullptr || (candidate != nullptr 
                                     && candidate->val == indicator)) {
            // Deleting duplicates
            if (candidate->val == indicator) {
                // duplicate_head = indicator == head->val;
                last_different->next = preception;
                candidate = preception;
                preception = preception ? preception->next : nullptr;
            } else {
                last_different = candidate;
                candidate = preception;
                preception = preception->next;
            }

            // Updating indicator
            if (preception != nullptr && preception->val == candidate->val) {
                indicator = preception->val;
            }
        }

        return pseudo_head->next;
    }
};
```

## 解法二(简洁):
```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        // Special Case: Empty List
        if (head == nullptr) {
            return nullptr;
        }
        
        // Initialzing pointers
        ListNode* pseudo_head = new ListNode(-777, head);
        ListNode* cur = pseudo_head;

        while (cur->next != nullptr && cur->next->next != nullptr) {
            if (cur->next->val == cur->next->next->val) {
                int idx = cur->next->val;
                while (cur->next != nullptr && idx == cur->next->val) {
                    cur->next = cur->next->next;
                }
            } else {
                cur = cur->next;
            }
        }

        return pseudo_head->next;
    }
};
```
