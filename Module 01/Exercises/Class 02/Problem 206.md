### 206. [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list)

##### Solução em PHP:

```php
class Solution
{
    function reverseList($head): ?ListNode
    {
        $pos = $head;

        while ($pos && $pos->next) {
            $aux = $pos->next;

            $pos->next = $aux->next;
            $aux->next = $head;

            $head = $aux;
        }

        return $head;
    }
}
```