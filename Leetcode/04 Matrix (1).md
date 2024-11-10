### 36. Valid Sudoku
```python
class Solution:
	def isValidSudoku(self, board: List[List[str]]) -> bool:
		return self.isLegalRow(board) and self.isLegalCol(board) and self.isLegalBlock(board)
	
	def isLegalRow(self, board):
		for row in range(len(board)):
			rowList = set()
			for col in range(len(board)):
				if board[row][col] in rowList:
					return False
				elif board[row][col] not in rowList and board[row][col] != ".":
					rowList.add(board[row][col])
		return True
	  
	def isLegalCol(self, board):
		for col in range(len(board)):
			colList = set()
			for row in range(len(board)):
				if board[row][col] in colList:
					return False
				elif board[row][col] not in colList and board[row][col] != ".":
					colList.add(board[row][col])
		return True
	
	def isLegalBlock(self, board):
		for row_bl in range(0, len(board), 3):
			for col_bl in range(0, len(board), 3):
				blockList = set()
				for row in range(row_bl, row_bl + 3):
					for col in range(col_bl, col_bl + 3):
						if board[row][col] in blockList:
							return False
						elif board[row][col] not in blockList and board[row][col] != ".":
							blockList.add(board[row][col])
		return True
```
