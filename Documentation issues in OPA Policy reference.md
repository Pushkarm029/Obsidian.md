
# Reference Heads


fruit.apple.seeds = 12 if input == "apple"             # complete document (single value rule)

import future.keywords.contains
fruit.pineapple.colors contains x if x := "yellow"     # multi-value rule

fruit.banana.phone[x] = "bananular" if x := "cellular" # single value rule
fruit.banana.phone.cellular = "bananular" if true      # equivalent single value rule

fruit.orange.color(x) = true if x == "orange"          # function