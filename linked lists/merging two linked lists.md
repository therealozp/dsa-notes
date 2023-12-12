here, we consider two separate and sorted linked lists, and our mission is to merge them into one linked **without** creating any extra space. 

Time complexity: $O(n + m)$ because it is iterating over 2 lists
Space complexity: $O(1)$ because it does not require any auxiliary space.

the idea is to use a **3-pointer approach**, initializing the `head` for the return, `curr` to point the previous node to, and the `prev` pointer to continue the iteration. the key point to understanding this is that we are *reassigning where each pointer points towards*, so careful pointer manipulation is required.

first, we always have to check the base condition of either `list1` or `list2` is present. while either of them exist, we iterate through the main body. 

```cpp
ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
	ListNode * head = NULL;
	ListNode * curr = NULL;
	ListNode * prev = NULL;
	while (list1 || list2 ) {
		if (!list2 || list1 && list1->val < list2->val) {
			curr = list1;
			list1 = list1->next;
		} else {
			curr = list2;
			list2 = list2->next;
		}

		if (prev) {
			prev->next = curr;
		} else {
			head = curr;
		}
		prev = curr;
	}

	return head;
}
```

pay attention to the original initialization of the `head` pointer. we check for whether the `prev` element exists, and if it does, that means we are NOT on the first iteration (meaning, we have initialized the head). this is the important part of the code: 

```cpp
if (prev) {
	prev->next = curr;
} else {
	head = curr;
}
prev = curr;
}
```

if a `prev` element is already pointing to something, we re-route the `prev->next` pointer to be the `curr` pointer yielded from iteration. if it isn't we simply initialize the `head`. and **always** re-assign the `prev` to `curr` afterwards, even if `head` is just initialized, because when `head` is pointing to the first element, it still makes sense that the trailing pointer `prev` should point to the value most recently initialized. 

the last pointer in the linked list will **never** point to anything (a `nullptr`) by the end of the loop, until the end of the next iteration where `next` is assigned to the `curr` pointer.