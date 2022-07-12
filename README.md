# python_Project

Copyright &copy; 2022 Okuda Yuta. 

See the end of this file for further copyright and license information.

Python project for the fresh year exercise

# インストール

[Yuta012/python_project](https://github.com/Yuta012/python_sample/blob/main/README.md) の code からダウンロード

# 使用方
> python block-ball.py

# コード解説
```python
from tkinter import *
import random

# ゲーム中で使う変数の一覧
blocks = []
block_size = {"x": 75, "y": 30}
ball = {"dirx": 15, "diry": -15, "x": 350, "y": 300, "w": 10}
bar = {"x": 0, "w": 70}
is_gameover = False
point = 0

# ウィンドウの作成
win = Tk()
cv = Canvas(win, width = 600, height = 400)
cv.pack()

# ゲームの初期化
def init_game():
    global is_gameover, point
    is_gameover = False
    ball["y"] = 280
    ball["diry"] = -8
    point = 0
    # ブロックを配置する
    for iy in range(0, 6):
        for ix in range(0, 12):
            color = "black"
            if (iy + ix) % 2 == 1: color = "red"
            x1 = 4 + ix * block_size["x"]
            x2 = x1 + block_size["x"]
            y1 = 4 + iy * block_size["y"]
            y2 = y1 + block_size["y"]
            blocks.append([x1, y1, x2, y2, color])
    win.title("START")

# オブジェクトを描画する
def draw_objects():
    cv.delete('all') # 既存の描画を破棄
    cv.create_rectangle(0, 0, 600, 400, fill="white", width=0)
    # ブロックを一つずつ描画
    for w in blocks:
        x1, y1, x2, y2, c = w
        cv.create_rectangle(x1, y1, x2, y2, fill=c, width=0)
    # ボールを描画
    cv.create_oval(ball["x"] - ball["w"], ball["y"] - ball["w"],
        ball["x"] + ball["w"], ball["y"] + ball["w"], fill="blue")
    # バーを描画
    cv.create_rectangle(bar["x"], 390, bar["x"] + bar["w"], 400, 
        fill="green")

# ボールの移動
def move_ball():
    global is_gameover, point
    if is_gameover: return
    bx = ball["x"] + ball["dirx"]
    by = ball["y"] + ball["diry"]
    # 上左右の壁に当たった？
    if bx < 0 or bx > 600: ball["dirx"] *= -1.015
    if by < 0: ball["diry"] *= -1.015
    # プレイヤーの操作するバーに当たった？
    if by > 390 and (bar["x"] <= bx <= (bar["x"] + bar["w"])):
        ball["diry"] *= -1
        if random.randint(0, 1) == 0: ball["dirx"] *= -1
        by = 380
    # ボールがブロックに当たった？
    hit_i = -2
    for i, w in enumerate(blocks):
        x1, y1, x2, y2, color = w
        w3 = ball["w"] / 3
        if (x1-w3 <= bx <= x2+w3) and (y1-w3 <= by <= y2+w3):
            hit_i = i
            break
    if hit_i >= 0:
        del blocks[hit_i]
        if random.randint(0, 1) == 0: ball["dirx"] *= -1
        ball["diry"] *= -1
        point += 10
        win.title("GAME SCORE = " + str(point))
    # ゲームオーバー？
    if by >= 400:
        win.title("終了!! score=" + str(point))
        is_gameover = True
    if 0 <= bx <= 600: ball["x"] = bx
    if 0 <= by <= 400: ball["y"] = by

def game_loop():
    draw_objects()
    move_ball()
    win.after(25, game_loop)

# マウスイベントの処理
def motion(e): # マウスポインタの移動
    bar["x"] = e.x

# マウスイベントを登録
win.bind('<Motion>', motion)

# ゲームのメイン処理
init_game()
game_loop()
win.mainloop()
```

```python
### 改良した点
```python
def click(e): # クリックでリスタート
    if is_gameover: init_game()
win.bind('<Button-1>', click)
```
+ 私が参照したサイトではこのようなコードがあり、連続してゲームができるようにしてありましたが、バグがありballがブロックに当たっても消えなかったりするので面倒にはなりますがこのコードを消しました。

```python
if bx < 0 or bx > 600: ball["dirx"] *= -1.015
if by < 0: ball["diry"] *= -1.015
```
+ ここの数字を－1ではなく、少し刻むことでballが少しずつ早くなり、難易度が上がるようにしました。しかしこの数字を－1.5などにするとバグってしまい、ballがどこかに行ってしまうので加減しながらこの数字を出しました。

```python
ball["y"] = 280
ball["diry"] = -8
```
+ 参照したサイトではballの初期地点が少しやりづらかったのでわかりやすく、やりやすいように改良しました。
他にも色を変えてわかりやすくしたり、バーを短くしたろ、単純なゲームだけどできるだけ難しくしてやりがいが持てるように改良してみました。

### 感想
```
サイトのものとはできるだけ変えてみようと努力しました。例えばバーをマウスで操作するところをキーボードでできないか試行錯誤してみましたが、上手くいかず結果的にあまり変わっていなかったので自分としてはとても残念な結果でした。教科書の範囲だけではなく、個人的にもっと調べて自分がやりやすいように、あるいは相手が分かりすいようにすることは十分可能だったと思います。自分の実力のなさを実感しました。このコードを書くとこういった動きをするということをしっかり頭に入れて、自分のキャパを増やさなければならないと思った。
```

## 参照した記事
https://news.mynavi.jp/techplus/article/zeropython-10/
```


# Copyright and License Information

This project is licensed under the terms of the MIT license.
