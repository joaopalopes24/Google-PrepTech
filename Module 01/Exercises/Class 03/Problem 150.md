### 150. [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation)

##### Solução em PHP:

```php
class Solution
{
    function evalRPN(array $tokens): int
    {
        $result = [];

        foreach ($tokens as $token) {
            if (is_numeric($token)) {
                array_push($result, $token);
            } else {
                $num2 = (int) array_pop($result);
                $num1 = (int) array_pop($result);

                $calc = $this->calculate($num1, $num2, $token);

                array_push($result, $calc);
            }
        }

        return array_pop($result);
    }

    function calculate(int $num1, int $num2, string $operator): int
    {
        if ($operator === '-') {
            return $num1 - $num2;
        }

        if ($operator === '*') {
            return $num1 * $num2;
        }

        if ($operator === '/') {
            return (int) $num1 / $num2;
        }

        return $num1 + $num2;
    }
}
```