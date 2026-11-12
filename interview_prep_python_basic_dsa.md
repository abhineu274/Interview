# 10 Basic Python Programming Interview Questions

These beginner-friendly questions practice strings, loops, lists, and basic
searching. Try each problem yourself before reading the solution.

## 1. Reverse a string

**Question:** Given a string, return it in reverse order.

```python
def reverse_string(text):
    return text[::-1]


print(reverse_string("python"))  # nohtyp
```

**Complexity:** O(n) time and O(n) space.

## 2. Check whether a string is a palindrome

**Question:** Return `True` if a string reads the same forward and backward.
This version ignores letter case and non-alphanumeric characters.

```python
def is_palindrome(text):
    cleaned = "".join(char.lower() for char in text if char.isalnum())
    return cleaned == cleaned[::-1]


print(is_palindrome("A man, a plan, a canal: Panama"))  # True
```

**Complexity:** O(n) time and O(n) space.

## 3. Calculate a factorial

**Question:** Calculate `n!`, the product of all positive integers up to `n`.
For this solution, `n` must be a non-negative integer.

```python
def factorial(n):
    if n < 0:
        raise ValueError("n must be non-negative")

    result = 1
    for number in range(2, n + 1):
        result *= number
    return result


print(factorial(5))  # 120
```

**Complexity:** O(n) time and O(1) extra space.

## 4. Find the nth Fibonacci number

**Question:** Return the Fibonacci number at position `n`, where the sequence
starts `0, 1, 1, 2, 3, ...` and `n` is zero-based.

```python
def fibonacci(n):
    if n < 0:
        raise ValueError("n must be non-negative")

    previous, current = 0, 1
    for _ in range(n):
        previous, current = current, previous + current
    return previous


print(fibonacci(7))  # 13
```

**Complexity:** O(n) time and O(1) extra space.

## 5. Check whether a number is prime

**Question:** Return `True` if an integer greater than 1 has no divisors other
than 1 and itself.

```python
def is_prime(number):
    if number < 2:
        return False

    divisor = 2
    while divisor * divisor <= number:
        if number % divisor == 0:
            return False
        divisor += 1
    return True


print(is_prime(29))  # True
print(is_prime(21))  # False
```

**Complexity:** O(√n) time and O(1) extra space.

## 6. Find the largest number in a list

**Question:** Return the largest value without using Python's built-in `max`.
The list must not be empty.

```python
def find_largest(numbers):
    if not numbers:
        raise ValueError("numbers must not be empty")

    largest = numbers[0]
    for index in range(1, len(numbers)):
        number = numbers[index]
        if number > largest:
            largest = number
    return largest


print(find_largest([4, 9, 2, 7]))  # 9
```

**Complexity:** O(n) time and O(1) extra space.

## 7. Find the second-largest distinct number

**Question:** Return the second-largest *distinct* value in a list, or `None`
if there is no such value.

```python
def second_largest(numbers):
    largest = None
    second = None

    for number in numbers:
        if largest is None or number > largest:
            second = largest
            largest = number
        elif number != largest and (second is None or number > second):
            second = number

    return second


print(second_largest([4, 9, 2, 9, 7]))  # 7
print(second_largest([5, 5]))          # None
```

**Complexity:** O(n) time and O(1) extra space.

## 8. Count character frequencies

**Question:** Count how many times each character appears in a string.

```python
def character_frequencies(text):
    counts = {}
    for char in text:
        counts[char] = counts.get(char, 0) + 1
    return counts


print(character_frequencies("banana"))  # {'b': 1, 'a': 3, 'n': 2}
```

**Complexity:** O(n) time and O(k) space, where `k` is the number of distinct
characters.

## 9. Remove duplicates while preserving order

**Question:** Return a list containing each value once, in its original order.
This solution assumes the values are hashable (for example, numbers or strings).

```python
def remove_duplicates(items):
    seen = set()
    result = []

    for item in items:
        if item not in seen:
            seen.add(item)
            result.append(item)
    return result


print(remove_duplicates([3, 1, 3, 2, 1]))  # [3, 1, 2]
```

**Complexity:** O(n) average time and O(n) space.

## 10. Find two numbers that add up to a target

**Question:** Given a list and a target, return the indices of two different
items whose values add up to the target. Return `None` if no pair exists.

```python
def two_sum(numbers, target):
    indices_by_value = {}

    for index, number in enumerate(numbers):
        needed = target - number
        if needed in indices_by_value:
            return [indices_by_value[needed], index]
        indices_by_value[number] = index

    return None


print(two_sum([2, 7, 11, 15], 9))  # [0, 1]
```

**Complexity:** O(n) average time and O(n) space.

## More practice: lists, sets, and dictionaries

## 11. Return only the even numbers

**Question:** Given a list of integers, return a new list containing only its
even numbers.

```python
def get_even_numbers(numbers):
    return [number for number in numbers if number % 2 == 0]


print(get_even_numbers([1, 2, 3, 4, 6]))  # [2, 4, 6]
```

**Complexity:** O(n) time and O(k) space, where `k` is the number of even
numbers.

## 12. Rotate a list to the right

**Question:** Move the last `k` items to the front. Rotating an empty list
returns an empty list.

```python
def rotate_right(items, k):
    if not items:
        return []

    k %= len(items)
    if k == 0:
        return items[:]
    return items[-k:] + items[:-k]


print(rotate_right([1, 2, 3, 4, 5], 2))  # [4, 5, 1, 2, 3]
```

**Complexity:** O(n) time and O(n) space.

## 13. Find common items in two lists

**Question:** Return the items that appear in both lists, without duplicates
and in the order they first appear in the first list. Items must be hashable.

```python
def common_items(first, second):
    second_items = set(second)
    result = []
    added = set()

    for item in first:
        if item in second_items and item not in added:
            result.append(item)
            added.add(item)
    return result


print(common_items([1, 2, 2, 3, 4], [2, 4, 6]))  # [2, 4]
```

**Complexity:** O(n + m) average time and O(m + k) space, where `n` and `m`
are the list lengths and `k` is the number of results.

## 14. Separate unique and duplicate values

**Question:** Return the values that appear exactly once and the values that
appear more than once. The result order follows each value's first appearance.

```python
def separate_unique_and_duplicates(items):
    counts = {}
    for item in items:
        counts[item] = counts.get(item, 0) + 1

    unique = []
    duplicates = []
    for item in counts:
        if counts[item] == 1:
            unique.append(item)
        else:
            duplicates.append(item)
    return unique, duplicates


unique, duplicates = separate_unique_and_duplicates([1, 2, 2, 3, 1, 4])
print(unique)      # [3, 4]
print(duplicates)  # [1, 2]
```

**Complexity:** O(n) average time and O(k) space, where `k` is the number of
distinct values.

## 15. Merge two dictionaries and add matching values

**Question:** Combine two dictionaries of counts. If a key appears in both,
add its values.

```python
def merge_counts(first, second):
    merged = first.copy()
    for key, value in second.items():
        merged[key] = merged.get(key, 0) + value
    return merged


print(merge_counts({"apple": 2, "pear": 1}, {"apple": 3, "plum": 4}))
# {'apple': 5, 'pear': 1, 'plum': 4}
```

**Complexity:** O(n + m) average time and O(n + m) space.

## 16. Group words by their first letter

**Question:** Create a dictionary mapping each first letter to the words that
start with it. Empty strings are ignored.

```python
def group_by_first_letter(words):
    groups = {}
    for word in words:
        if word:
            first_letter = word[0].lower()
            groups.setdefault(first_letter, []).append(word)
    return groups


print(group_by_first_letter(["Apple", "apricot", "Banana", "blueberry"]))
# {'a': ['Apple', 'apricot'], 'b': ['Banana', 'blueberry']}
```

**Complexity:** O(n) time and O(n) space, where `n` is the total number of
words.

## 17. Find values present in exactly one set

**Question:** Given two sets, return the values that are in either set but not
both (the symmetric difference).

```python
def values_in_exactly_one(first, second):
    return first.symmetric_difference(second)


print(values_in_exactly_one({1, 2, 3}, {3, 4, 5}))  # {1, 2, 4, 5}
```

**Complexity:** O(n + m) average time and O(n + m) space.

## 18. Sort a dictionary by its values

**Question:** Return a list of `(key, value)` pairs sorted by value, smallest
first.

```python
def sort_by_value(scores):
    return sorted(scores.items(), key=lambda item: item[1])


print(sort_by_value({"Sam": 85, "Lee": 92, "Kai": 78}))
# [('Kai', 78), ('Sam', 85), ('Lee', 92)]
```

**Complexity:** O(n log n) time and O(n) space.
