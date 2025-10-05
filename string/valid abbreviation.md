#simulation

```python
def validAbbreviation (s, abbr):
    i = 0
    j = 0

    while i < len(s) and j < len(abbr):
        if abbr[j].isdigit():
            if abbr[j] == '0':
                return False
            curr_number = ''
            while j < len(abbr) and abbr[j].isdigit():
                curr_number += abbr[j]
                j += 1
            i += int(curr_number)
        else:
            if abbr[j] != s[i]:
                return False
            else:
                j += 1
                i += 1

    if i == len(s) and j == len(abbr):
        return True
    return False
```
