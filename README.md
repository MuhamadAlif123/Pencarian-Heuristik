# Pencarian-Heuristik
KB 3  - Pencarian Heuristik 


**no.1**
```python
class Node:
    def __init__(self, state, parent, cost):
        self.state = state
        self.parent = parent
        self.cost = cost

    def __str__(self):
        return str(self.state)


def best_first_search(initial_state, is_goal, get_neighbors, heuristic):
    open_list = []
    closed_set = set()

    # Inisialisasi node awal
    start_node = Node(state=initial_state, parent=None, cost=0)
    open_list.append(start_node)

    while open_list:
        # Mengambil node dengan heuristic terendah
        open_list.sort(key=lambda x: heuristic(x.state))
        current_node = open_list.pop(0)

        # Jika node adalah tujuan, kembalikan jalur
        if is_goal(current_node.state):
            path = []
            while current_node:
                path.append(current_node.state)
                current_node = current_node.parent
            return path[::-1]

        # Tandai node saat ini sebagai node yang telah dijelajahi
        closed_set.add(current_node.state)

        # Expand node saat ini dan tambahkan tetangganya ke open_list
        neighbors = get_neighbors(current_node.state)
        for neighbor_state, cost in neighbors:
            if neighbor_state not in closed_set:
                neighbor_node = Node(state=neighbor_state, parent=current_node, cost=cost)
                open_list.append(neighbor_node)

    # Jika tidak ada solusi ditemukan
    return None


# Contoh penggunaan
def is_goal(state):
    return state == 'G'


def get_neighbors(state):
    if state == 'A':
        return [('B', 1), ('C', 3)]
    elif state == 'B':
        return [('D', 5)]
    elif state == 'C':
        return [('E', 2)]
    elif state == 'D':
        return [('G', 4)]
    elif state == 'E':
        return [('G', 3)]
    else:
        return []


def heuristic(state):
    # Heuristic sederhana: jarak langsung ke tujuan
    heuristic_values = {'A': 6, 'B': 4, 'C': 5, 'D': 3, 'E': 2, 'G': 0}
    return heuristic_values[state]


initial_state = 'A'
path = best_first_search(initial_state, is_goal, get_neighbors, heuristic)
print("Jalur terpendek:", path)
```

Dalam contoh di atas, fungsi `best_first_search` menerima parameter `initial_state` (keadaan awal), `is_goal` (fungsi yang memeriksa apakah keadaan adalah keadaan tujuan), `get_neighbors` (fungsi yang mengembalikan tetangga dari suatu keadaan beserta biaya untuk mencapainya), dan `heuristic` (fungsi heuristik yang mengestimasi biaya yang tersisa untuk mencapai tujuan dari suatu keadaan). 

Fungsi ini kemudian mencari jalur terpendek dari keadaan awal ke keadaan tujuan dengan menggunakan algoritma Best First Search.

Kemudian, untuk menjalankan algoritma ini, kita menyediakan keadaan awal `'A'`, fungsi `is_goal` untuk menentukan apakah keadaan adalah keadaan tujuan (dalam kasus ini keadaan `'G'`), fungsi `get_neighbors` untuk mendapatkan tetangga dari suatu keadaan, dan fungsi `heuristic` untuk menghitung nilai heuristik dari suatu keadaan.

Output akan berupa jalur terpendek yang ditemukan dari keadaan awal ke keadaan tujuan.

**no.2**
ini adalah contoh implementasi Hill Climbing (HC) untuk permainan catur:

```python
import random

class ChessBoard:
    def __init__(self):
        self.board = [[0 for _ in range(8)] for _ in range(8)]
        self.randomly_place_queens()

    def randomly_place_queens(self):
        for col in range(8):
            row = random.randint(0, 7)
            self.board[row][col] = 1

    def heuristic(self):
        conflicts = 0
        for col in range(8):
            for row in range(8):
                if self.board[row][col] == 1:
                    conflicts += self.count_conflicts(row, col)
        return conflicts

    def count_conflicts(self, row, col):
        conflicts = 0
        # Check horizontal conflicts
        for i in range(8):
            if self.board[row][i] == 1 and i != col:
                conflicts += 1

        # Check vertical conflicts
        for i in range(8):
            if self.board[i][col] == 1 and i != row:
                conflicts += 1

        # Check diagonal conflicts
        for i, j in zip(range(row-1, -1, -1), range(col+1, 8)):
            if self.board[i][j] == 1:
                conflicts += 1
        for i, j in zip(range(row+1, 8), range(col+1, 8)):
            if self.board[i][j] == 1:
                conflicts += 1
        for i, j in zip(range(row-1, -1, -1), range(col-1, -1, -1)):
            if self.board[i][j] == 1:
                conflicts += 1
        for i, j in zip(range(row+1, 8), range(col-1, -1, -1)):
            if self.board[i][j] == 1:
                conflicts += 1

        return conflicts

    def print_board(self):
        for row in self.board:
            print(row)


def hill_climbing():
    current_board = ChessBoard()
    while True:
        print("Current Heuristic:", current_board.heuristic())
        current_board.print_board()
        if current_board.heuristic() == 0:
            print("Goal state reached!")
            break
        next_board = ChessBoard()
        if next_board.heuristic() < current_board.heuristic():
            current_board = next_board

hill_climbing()
```

Dalam contoh di atas, kita membuat kelas `ChessBoard` untuk merepresentasikan papan catur. Metode `randomly_place_queens()` digunakan untuk menempatkan ratu secara acak di papan catur. Metode `heuristic()` menghitung jumlah konflik pada papan catur saat ini, sedangkan metode `count_conflicts()` digunakan untuk menghitung konflik untuk setiap ratu.

Fungsi `hill_climbing()` adalah implementasi algoritma Hill Climbing. Ini terus mencetak papan catur saat ini dan heuristiknya, dan menghasilkan papan catur berikutnya jika heuristiknya lebih rendah dari heuristik papan catur saat ini. Proses berakhir ketika heuristik mencapai nol, yang menunjukkan pencapaian keadaan tujuan.
