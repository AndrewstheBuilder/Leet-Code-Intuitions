1. Delete the kth node from a singly linked list
# Given the head of a linked list, remove the nth node from the end of the list and return its head.
# 1 -> 2 -> 3 -> 4 -> None

# use a two pointer approach
# two pointer both start head.
# move right pointer k steps. Then start moving both pointers forward.
# when right pointer hits end, left pointer is at node to delete 
    # or something like that depending on math
    
# 1 -> 2 -> 3 -> 4 -> None, k=1
# right=1,left=1,i=0 -> right=2, left=1, i=1==k -> right=3,left=2 -> right=4,left=3 3.next=4.next 1->2->3 -> None

# 1 -> 2 -> 3 -> 4 -> None, k=1
# right=1,left=1,i=0 -> right=2, left=1, i=1==k -> right=3,left=2 -> right=4,left=3 -> right=None, left=4
# My implementation
def kthNode(root:Node, k:int):
    left,right = root
    i = 0
    while right.next:
        if i == k:
           left = left.next 
        else:
            i += 1
        right = right.next
    # left should be the node to delete the next node from
    left.next = left.next.next
    
# actual solution
# 1 -> 2 -> 3 -> 4 -> None, k=1
# 1 -> None
# i is not correct if k=1 and the first node is also the last node.
# i would be 0 instead of being 1. We are supposed to delete the 1st node, with the first node being the end.
# so adding a dummy node will create the condition for 1 being the first node.
# The pointer is technically one node away on the first node. Which is what we wanted
def removeNthFromEnd(self, root: Optional[ListNode], k: int) -> Optional[ListNode]:
    if root==None:
        return
    dummy = ListNode(-1, root)
    left = right = dummy
    i = 0
    while right.next:
        if i == k:
            left = left.next 
        else:
            i += 1
        right = right.next
    # left should be the node to delete the next node from
    left.next = left.next.next if left.next else None

    # return the head of the singly linked list after removal
    return dummy.next
