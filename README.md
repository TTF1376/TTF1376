# 初始化棋盘
def initialize_board():
    return [[' ' for _ in range(3)] for _ in range(3)]

# 打印棋盘
def print_board(board):
    for row in board:
        print('|'.join(row))
        print('-' * 5)

# 检查是否获胜
def check_win(board, player):
    # 检查行
    for row in board:
        if all(s == player for s in row):
            return True
    # 检查列
    for col in range(3):
        if all(board[row][col] == player for row in range(3)):
            return True
    # 检查对角线
    if board[0][0] == board[1][1] == board[2][2] == player:
        return True
    if board[0][2] == board[1][1] == board[2][0] == player:
        return True
    return False

# 检查棋盘是否满了（平局）
def check_draw(board):
    return all(board[row][col] != ' ' for row in range(3) for col in range(3))

# 移动棋子
def move_piece(board, player):
    while True:
        try:
            old_x, old_y = map(int, input(f"{player}玩家, 输入要移动的棋子位置 (行 列): ").split())
            new_x, new_y = map(int, input(f"{player}玩家, 输入要移动到的位置 (行 列): ").split())
            if board[old_x][old_y] == player and board[new_x][new_y] == ' ':
                board[old_x][old_y] = ' '
                board[new_x][new_y] = player
                break
            else:
                print("无效的移动，请重试。")
        except (ValueError, IndexError):
            print("输入错误，请输入有效的行和列 (0-2)。")

# 游戏主循环
def play_game():
    board = initialize_board()
    players = ['X', 'O']  # 两个玩家
    current_player = 0
    steps = 0  # 记录步数

    # 初始阶段每个玩家放置3个棋子
    while steps < 6:
        print_board(board)
        row, col = map(int, input(f"{players[current_player]}玩家, 输入下棋位置 (行 列): ").split())
        if board[row][col] == ' ':
            board[row][col] = players[current_player]
            if check_win(board, players[current_player]):
                print_board(board)
                print(f"{players[current_player]}获胜!")
                return
            current_player = 1 - current_player
            steps += 1
        else:
            print("该位置已被占用，请重试。")

    # 移动阶段
    while True:
        print_board(board)
        move_piece(board, players[current_player])
        if check_win(board, players[current_player]):
            print_board(board)
            print(f"{players[current_player]}获胜!")
            return
        if check_draw(board):
            print_board(board)
            print("平局!")
            return
        current_player = 1 - current_player

# 运行游戏
play_game()
