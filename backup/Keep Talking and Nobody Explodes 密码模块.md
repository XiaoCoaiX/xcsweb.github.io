```python
# 定义单词列表
words = [
    "about", "after", "again", "below", "could",
    "every", "first", "found", "great", "house",
    "large", "learn", "never", "other", "place",
    "plant", "point", "right", "small", "sound",
    "spell", "still", "study", "their", "there",
    "these", "thing", "think", "three", "water",
    "where", "which", "world", "would", "write"
]

# 判断conditions每一项是否包含word对应位中的字母，是返回true，否则返回false
def matches_conditions(word, conditions):
    for i, candidates in enumerate(conditions): # 枚举conditions中的字母，i为索引，candidate为元素
        if word[i] not in candidates:
            return False
    return True

print("请输入每一位的候选字母")
conditions = []
for i in range(5):
    candidates = input(f"第{i + 1}位：")
    conditions.append(candidates)
    matched_words = [word for word in words if matches_conditions(word, conditions)] # 遍历words，并将每一项放入matches_conditions函数中判断

    if len(matched_words) == 1: # 候选单词只剩一个
        print(matched_words[0])
        break;
    elif len(matched_words) > 1: # 候选单词不止一个
        print("符合条件的单词有多个:")
        for matched_word in matched_words:
            print(matched_word)
    else:
        print("没有符合条件的单词")
``` 