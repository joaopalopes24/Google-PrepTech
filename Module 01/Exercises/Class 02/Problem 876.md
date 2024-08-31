### 876. [Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list)

##### Solução em PHP:

```php
class Solution
{
    function middleNode($head): ListNode
    {
        $slow = $fast = $head;

        while ($fast && $fast->next) {
            $slow = $slow->next;
            $fast = $fast->next->next;
        }

        return $slow;
    }
}
```