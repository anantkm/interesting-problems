### Generate HTML Tag

Link: https://leetcode.com/discuss/interview-question/258474/generate-valid-html-string
Playground: https://leetcode.com/playground/Wzs3AdM5

```python
import collections
class Solution(object):

	def generate_valid_html(self, s, format):
		tags = []
		for tag, intervals in format.items():
			for start, end in intervals:
				tags.append((start, 1, ord(tag) - ord('a'), '(', tag))
				tags.append((end, 0, -(ord(tag)- ord('a')), ')', tag))
		tags.sort(key=lambda x: (x[0],x[1],x[2]))
		tags = collections.deque([(idx, paren, tag) for idx,_,_,paren,tag in tags])
		output = []
		opens = []
        
		for i, c in enumerate(s):
			while tags and i == tags[0][0]:
				idx, paren, tag = tags.popleft()
				if paren == '(':
					output.append(f'<{tag}>')
					opens.append(tag)
				else:
					while opens[-1] != tag:
						output.append(f'</{opens[-1]}>')
						tags.appendleft((idx, '(', opens[-1]))
						opens.pop()
					output.append(f'</{tag}>')
					opens.pop()
			output.append(c)
		return ''.join(output)

text = 'ABCDEFGHIJ'
format1 = {
	'b': [(0,4)],
	'i': [(0,4)],
    'p': [(0,4)]
}
# <b>AB<i>CD</i></b><i>EF</i>GHIJ
format2 = {
	'b': [(0,4)],
	'i': [(4,6)]
}
# <b>ABCD</b><i>EF</i>GHIJ
format3 = {
	'b': [(0,2)],
	'i': [(0,2)]
}
# <b><i>AB</i></b>CDEFGHIJ
text4 = 'ABCDEFGHIJK'
format4 = {
	'b': [(0,3)],
	'i': [(4,6),(7,10)]
}
# <b>ABC</b>D<i>EF</i>G<i>HIJ</i>K
s = Solution()
print(s.generate_valid_html(text, format1))
print(s.generate_valid_html(text, format2))
print(s.generate_valid_html(text, format3))
print(s.generate_valid_html(text4, format4))
```
