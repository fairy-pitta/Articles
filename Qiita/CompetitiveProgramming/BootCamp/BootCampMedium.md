
## 001. Trained?

https://atcoder.jp/contests/abc065/tasks/abc065_b

<details><summary>ポイント</summary>

グラフ問題に見えるが配列を順番に追っていけば解ける。

</details>


<details><summary>コード</summary>

```python
def main():
    N = int(input())
    Light = []

    # [Light, idx]
    for i in range(N):
        a = int(input())
        Light.append([a-1, i])
    
    cur_idx = 0
    cur_light = Light[0][0]
    cnt = 0
    
    while True:

        if cur_idx == 1:
            break
        if cnt > N:
            break

        cur_idx = Light[cur_light][1]
        cur_light = Light[cur_light][0]
        cnt += 1
    
    if cnt > N:
        print(-1)
    else:
        print(cnt)
        
main()

```

</details>

## 002. Irreevrsible Operation 

https://atcoder.jp/contests/agc029/tasks/agc029_a

<details><summary>ポイント</summary>

何回か実験をしてみると全てのBが右に来るまで操作が続くことがわかるので、そうなるまでの操作回数を（Bが全て右に来た時のインデックスの合計-現在のBのインデックスの合計）で計算する。

</details>


<details><summary>コード</summary>

```python
def my_ceil(N, W):
    return (-(-N//W))

def main():
    S = list(input())

    idx_cnt = 0
    for i in range(len(S)):
        if S[i] == "B":
            idx_cnt += i
    
    right_cnt = 0
    for j in range(S.count("B")):
        right_cnt += len(S)-j-1
    
    print(right_cnt-idx_cnt)

main()
```

</details>

## 003. >< 

https://atcoder.jp/contests/agc040/tasks/agc040_a

<details><summary>コード</summary>

```python
def main():
    S = input()

    more_than_count = [(0) for _ in range(len(S)+1)]
    less_than_count = [(0) for _ in range(len(S)+1)]

    for i in range(len(S)):
        if S[i] == "<":
            more_than_count[i+1] = more_than_count[i] + 1
    
    for j in reversed(range(len(S))):
        if S[j] == ">":
            less_than_count[j] = less_than_count[j+1] + 1
    
    ans = 0
    for k in range(len(S)+1):
        ans += max(more_than_count[k], less_than_count[k])
    
    print(ans)
    
main()
```

</details>


## 004. ss 

https://atcoder.jp/contests/abc066/tasks/abc066_b

<details><summary>コード</summary>

```python
def check_ss(s: list) -> bool:
    if len(s)%2:
        return False
    
    mid_point = len(s)//2
    if s[:mid_point] == s[mid_point:]:
        return True
    
    return False


def main():
    S = list(input())
    for _ in range(len(S)):
        S.pop()
        if check_ss(S):
            return len(S)

print(main())
    

```

</details>

## 005. ModSum

https://atcoder.jp/contests/abc139/tasks/abc139_d

<details><summary>ポイント</summary>

一回全探索するコードを書いて実験してみるとパターンが見えてくる。

```python:全探索コード

def greedy(n):
    from itertools import permutations 
    seq = list(range(1,n+1))
    perm_list = list(permutations(seq))
    
    ans = 0
    ans_seq = seq
    for s in perm_list:
        temp_ans = 0
        for i in range(len(s)):
            temp_ans += seq[i]%s[i]
        
        if temp_ans > ans:
            ans_seq = s
            ans = temp_ans
        else:
            continue
    
    print(ans)
    print(ans_seq)

```

</details>


<details><summary>コード</summary>

```python
def main():
    N = int(input())
    print((N)*(N-1)//2)

main()
```

</details>


## 006. Minesweeper 

https://atcoder.jp/contests/abc075/tasks/abc075_b

<details><summary>ポイント</summary>

上下左右斜めのマスをそれぞれ確認するときには`dir`のようなリストを使うと楽。

```python: 方向のリスト
dir = [(-1, -1), (-1, 0), (-1, 1), (0, -1), (0, 1), (1, -1), (1, 0), (1, 1)]
```

</details>

<details><summary>コード</summary>

```python

dir = [(-1, -1), (-1, 0), (-1, 1), (0, -1), (0, 1), (1, -1), (1, 0), (1, 1)]

def main():
    H, W = map(int, input().split())
    S = [list(input()) for _ in range(H)]
    grids = [[(0) for _ in range(W)] for _ in range(H)]


    for row in range(H):
        for col in range(W):
            if S[row][col] == "#":
                grids[row][col] = "#"
            
                for r, c in dir:
                    adj_row = row+r
                    adj_col = col+c
                    if (0 <= adj_row < H and \
                        0 <= adj_col < W and \
                            S[adj_row][adj_col] == "."):
                        
                        grids[adj_row][adj_col] += 1
    
    for i in grids:
        ans = "".join([str(z) for z in i])
        print(ans)


main()
```

</details>

## 007. Cut and Count 

https://atcoder.jp/contests/abc098/tasks/abc098_b

<details><summary>コード</summary>

```python
def main():
    N = int(input())
    S = list(input())

    ans = 0
    for i in range(1, N-1):
        first_set = set(S[:i])
        second_set = set(S[i:])
        cnt = 0
        for item in first_set:
            if item in second_set:
                cnt += 1
        
        ans = max(cnt, ans)
    
    print(ans)

main()
```

</details>

## 008. Colorful Leaderboard 

https://atcoder.jp/contests/abc064/tasks/abc064_c


<details><summary>コード</summary>

なんか楽しかったのでしっかり書いたが、解説にもあるようにレートをリストで持っておけばもっと簡単に書ける。なぜこの問題が茶中位diffなんだろう？


```python
def main():
    N = int(input())
    A = list(map(int, input().split()))

    top_coder = 0
    color_set = set()

    for i in range(N):
        if A[i] < 400:
            color_set.add("grey")
        elif A[i] < 800:
            color_set.add("brown")
        elif A[i] < 1200:
            color_set.add("green")
        elif A[i] < 1600:
            color_set.add("skyblue")
        elif A[i] < 2000:
            color_set.add("blue")
        elif A[i] < 2400:
            color_set.add("yellow")
        elif A[i] < 2800:
            color_set.add("orange")
        elif A[i] < 3200:
            color_set.add("red")
        else:
            top_coder += 1 
    
    max_num = len(color_set) + top_coder
    min_num = 1 if len(color_set) == 0 else len(color_set)

    print(min_num, max_num)

main()
```

</details>

## 009. Choose Integers 

https://atcoder.jp/contests/abc060/tasks/abc060_b 


<details><summary>ポイント</summary>

立式するとユークリッドの互除法を使う問題に帰着できる。
細かい解説は[こちら](https://qiita.com/drken/items/0c88a37eec520f82b788#%E5%95%8F%E9%A1%8C-6-abc-060-b---choose-integers-%E6%94%B9%E9%A1%8C%E5%85%83%E3%81%AF-200-%E7%82%B9)の6-1 ベズーの等式のセクションで発見できた。

</details>


<details><summary>コード</summary>

```python
# A, B の最大公約数を返す関数
def GCD(A, B):
    if B == 0:
        return A
    else:
        return GCD(B, A % B)

def main():
    A, B, C = map(int, input().split())
    g = GCD(A, B)
    if C%g == 0:
        print("YES")
    else:
        print("NO")

main()
```

</details>

## 010. Together 

https://atcoder.jp/contests/abc072/tasks/arc082_a

<details><summary>コード</summary>

```python
from collections import defaultdict

def main():
    N = int(input())
    A = list(map(int, input().split()))
    d = defaultdict(int)
    for i in A:
        d[i] += 1
    
    ans = 0
    key_list = list(d.keys())
    
    for k in key_list:
        cnt = d[k] + d[k-1] + d[k+1]
        ans = max(ans, cnt)
    
    print(ans)

main()
```

</details>


## 011. Checkpoints 

https://atcoder.jp/contests/abc057/tasks/abc057_b

<details><summary>コード</summary>

```python
def calc_dist(x1,y1,x2,y2):
    return abs(x1-x2)+abs(y1-y2)

def main():
    N, M = map(int, input().split())
    students = [list(map(int, input().split())) for _ in range(N)]
    points = [list(map(int, input().split())) for _ in range(M)]

    for person in students:
        dist = 10**18
        checkpoint = 0
        for p in range(len(points)):
            if (d := calc_dist(person[0], person[1], points[p][0], points[p][1])) < dist:
                dist = d
                checkpoint = p+1
        print(checkpoint)
    

main()
```

</details>


## 012. Grid Compression 

https://atcoder.jp/contests/abc107/tasks/abc107_b

<details><summary>コード</summary>

```python
def solve():
    H, W = map(int, input().split())
    A = [list(input()) for _ in range(H)]

    row_list = set()
    col_list = set()

    for row in range(H):
        for col in range(W):
            if A[row][col] == "#":
                row_list.add(row)
                col_list.add(col)
    
    for r in range(H):
        ans = ""
        for c in range(W):
            if r in row_list and c in col_list:
                ans += A[r][c]
        if ans == "":
            continue
        else:
            print(ans)

solve()
```

</details>

## 013. Traveling 

https://atcoder.jp/contests/abc086/tasks/arc089_a


<details><summary>コード</summary>

```python
def check(x1,y1,x2,y2,t1,t2):
    dist = abs(x1-x2)+abs(y1-y2)
    time = t2-t1
    if dist > time:
        return False
    elif (time-dist) % 2 == 1:
        return False
    return True


def main():
    N = int(input())
    prev_t = 0
    prev_x = 0
    prev_y = 0
    for _ in range(N):
        t, x, y = map(int, input().split())
        if check(prev_x, prev_y, x, y, prev_t, t):
            prev_x = x
            prev_y = y
            prev_t = t
        else:
            return False
    
    return True 

if main():
    print("Yes")
else:
    print("No")


```

</details>


## 014. Grid Repaingting 2

https://atcoder.jp/contests/abc096/tasks/abc096_c


<details><summary>コード</summary>

```python
def main():
    dir = [(-1, 0), (0, -1), (0, 1), (1, 0)]
    H, W = map(int, input().split())
    S = [list(input()) for _ in range(H)]

    for row in range(H):
        for col in range(W):
            if S[row][col] == "#":
                flag = False
                for r, c in dir:
                    adj_r = row + r
                    adj_c = col + c
                    if (0 <= adj_r < H and\
                        0 <= adj_c < W and\
                            S[adj_r][adj_c] == "#"):
                        flag = True
                
                if not flag: return flag 
                        
    
    return True

if main():
    print("Yes")
else:
    print("No")
```

</details>

## 015. KEYENCE String 

https://atcoder.jp/contests/keyence2019/tasks/keyence2019_b

<details><summary>コード</summary>

```python
def main():
    S = list(input())
    for i in range(len(S)):
        for j in range(i, len(S)):
            bef_i = S[:i]
            aft_j  =S[j:]
            if "".join(bef_i + aft_j) == "keyence":
                return True
    
    return False

if main():
    print("YES")
else:
    print("NO")
```

</details>

## 016. Bugged 


https://atcoder.jp/contests/abc063/tasks/arc075_a

<details><summary>感想</summary>

点数がすべて5の倍数と勘違いをしていたせいで３WAほど積んでしまった。

</details>

<details><summary>コード</summary>

```python
def main():
    N = int(input())
    S = []
    for _ in range(N):
        s = int(input())
        S.append(s)

    S_sum = sum(S)

    non_zero = []
    for i in S:
        if i % 10 != 0:
            non_zero.append(i)
    
    if S_sum % 10 != 0:
        print(S_sum)
    else:
        if len(non_zero) > 0:
            S_sum -= min(non_zero)
            print(S_sum)
        else:
            print(0)

main()
```

</details>


## 017. Sentou 

https://atcoder.jp/contests/abc060/tasks/arc073_a

<details><summary>コード</summary>

```python
def main():
    N, T = map(int, input().split())
    t = list(map(int, input().split()))
    end_time = T
    total_time = T

    for i in range(1, N):
        if t[i] < end_time:
            total_time += T - (end_time - t[i])
            end_time = t[i] + T
        else:
            end_time = t[i] + T
            total_time += T
        
    print(total_time)

main()
```

</details>

## 018. Takahashi's information

https://atcoder.jp/contests/abc088/tasks/abc088_c

<details><summary>ポイント</summary>

証明も何もできていないコードを投げたら通りました。
多分嘘解法です。解説を読んでください。

</details>


<details><summary>コード</summary>

```python
def main():
    C = [list(map(int, input().split())) for _ in range(3)]

    a1B = sum(C[0])
    a2B = sum(C[1])
    a3B = sum(C[2])

    b1A = sum([C[i][0] for i in range(3)])
    b2A = sum([C[j][0] for j in range(3)])
    b3A = sum([C[k][0] for k in range(3)])

    if abs(a1B-a2B)%3 != 0:
        return False
    if abs(a2B-a3B)%3 != 0:
        return False
    if abs(a1B-a3B)%3 != 0:
        return False
    if abs(b1A-b2A)%3 != 0:
        return False
    if abs(b1A-b3A)%3 != 0:
        return False
    if abs(b2A-b3A)%3 != 0:
        return False
    

    cross = C[0][0] + C[1][1] + C[2][2]
    if cross * 3 != (sum(C[0])+sum(C[1])+sum(C[2])):
        return False
    
    return True 

if main():
    print("Yes")
else:
    print("No")
    
    
```

</details>


## 019. RGB Boxes 

https://atcoder.jp/contests/diverta2019/tasks/diverta2019_b


<details><summary>コード</summary>

```python
def main():
    R, G, B, N = map(int, input().split())
    cnt = 0

    for red in range(3001):
        for green in range(3001):
            if N-red*R-green*G >= 0 and (N-red*R-green*G)%B == 0:
                cnt += 1
    
    print(cnt)

main()
```

</details>


## 020. Write and Erase 

https://atcoder.jp/contests/abc073/tasks/abc073_c


<details><summary>コード</summary>

```python
from collections import defaultdict 
from operator import mul
from functools import reduce

def cmb(n,r):
    if n <= 1: return 0 
    r = min(n-r,r)
    if r == 0: return 1
    over = reduce(mul, range(n, n - r, -1))
    under = reduce(mul, range(1,r + 1))
    return over // under

def main():
    N = int(input())
    A = list(map(int, input().split()))
    d = defaultdict(int)

    for i in A:
        d[i] += 1
    
    cnt = 0
    for v in d.values():
        if v >= 2:
            cnt += cmb(v, 2)
    
    for j in A:
        # print(j, 'j', cnt, d[j])
        ans = cnt
        if d[j] >= 3:
            ans -= cmb(d[j], 2)
            ans += cmb(d[j]-1, 2)
        elif d[j] == 2:
            ans -= 1
        print(ans)

main()



```

</details>


## 021. Banned K

https://atcoder.jp/contests/abc159/tasks/abc159_d

<details><summary>コード</summary>

```python
from collections import defaultdict 
from operator import mul
from functools import reduce

def cmb(n,r):
    if n <= 1: return 0 
    r = min(n-r,r)
    if r == 0: return 1
    over = reduce(mul, range(n, n - r, -1))
    under = reduce(mul, range(1,r + 1))
    return over // under

def main():
    N = int(input())
    A = list(map(int, input().split()))
    d = defaultdict(int)

    for i in A:
        d[i] += 1
    
    cnt = 0
    for v in d.values():
        if v >= 2:
            cnt += cmb(v, 2)
    
    for j in A:
        # print(j, 'j', cnt, d[j])
        ans = cnt
        if d[j] >= 3:
            ans -= cmb(d[j], 2)
            ans += cmb(d[j]-1, 2)
        elif d[j] == 2:
            ans -= 1
        print(ans)

main()



```

</details>

## 022. Dice and Coin 

https://atcoder.jp/contests/abc126/tasks/abc126_c

<details><summary>コード</summary>

```python

def main():
    N, K = map(int, input().split())

    prob = 0

    for i in range(1, N+1):

        cnt = 0
        n = i
        while True:
            if n < K:
                n *= 2 
                cnt += 1
            else:
                break 
        
        prob += (1/N) * (1/2) ** cnt
    
    print(prob)

main()

```

</details>


## 023. Guess The Number 

https://atcoder.jp/contests/abc157/tasks/abc157_c

<details><summary>供養</summary>

馬鹿正直に書いたが一つWAが取れなくてわからないので供養する

```python
def main():
    N, M = map(int, input().split())
    num = [(-1) for _ in range(N)]
    for _ in range(M):
        s, c = map(int, input().split())
        if num[s-1] == -1 or num[s-1] == c:
            num[s-1] = c
        else:
            print(-1)
            return 
    
    if N == 1 and num[0] == 0:
        print(0)
        return 
    
    for i in range(N):
        if i == 0:
            if num[0] == -1:
                num[0] = 1
            elif num[0] == 0:
                print(-1)
                return
        else:
            if num[i] == -1:
                num[i] = 0
    
    print("".join([str(z) for z in num]))
    return 

main()
           
```

</details>


<details><summary>コード</summary>

```python

def main():
    N, M = map(int, input().split())
    S = [list(map(int, input().split())) for _ in range(M)]
    start_N = [0, 10, 100]

    for i in range(start_N[N-1], 10**N):
        num = list(str(i))
        flag = True
        for j in range(M):
            s, c = S[j][0], S[j][1]
            if num[s-1] != str(c):
                flag = False
                break
        if flag:
            print(i)
            return
    
    print(-1)
    return

main()      
```

</details>


## 024. A+...+B Problem

https://atcoder.jp/contests/agc015/tasks/agc015_a


<details><summary>コード</summary>

```python

def main():
    N, A, B = map(int, input().split())

    # check A < B
    if A > B:
        print(0)
        return 
    
    min_sum = (N-1)*A+B
    max_sum = (N-1)*B+A
    print(max_sum-min_sum+1 if (max_sum-min_sum) >= 0 else 0)
    return

main()
```

</details>


## 025. Not so Diverse 

https://atcoder.jp/contests/abc081/tasks/arc086_a

<details><summary>コード</summary>

```python
from collections import defaultdict

def main():
    N, K = map(int, input().split())
    A = list(map(int, input().split()))
    d = defaultdict(int)
    for i in A:
        d[i] += 1 
    value_list = sorted(list(d.values()), reverse=True)
    ans = 0
    for j in range(len(value_list)):
        if j >= K:
            ans += value_list[j]
    
    print(ans)

main()


```

</details>


## 026. Sorted Arrays 

https://atcoder.jp/contests/agc013/tasks/agc013_a


<details><summary>コード</summary>

```python
# 尺取り

from collections import deque
def main():
    N = int(input())
    A = deque(list(map(int, input().split())))

    que = deque([])
    flag = 0 # 0: Undetermined, 1: Increasing, 2: Decreasing 
    cnt = 0

    cur = A.popleft()

    while True:

        if len(A) == 0:
            break

        next = A.popleft()

        if flag == 0: # Undetermined 
            if cur < next: # incresing 
                flag = 1 
                que.append(cur)
                cur = next
            elif cur == next:
                que.append(cur)
                cur = next 
            else:
                # cur > next # decresing 
                flag = 2
                que.append(cur)
                cur = next 
        
        elif flag == 1: # increasing 
            if cur <= next: # increasing 
                que.append(cur)
                cur = next
            else:
                cnt += 1
                que.append(cur)
                cur = next
                flag = 0
        
        else:
            # flag == 2 decreasing 
            if cur >= next: 
                que.append(cur)
                cur = next
            else:
                cnt += 1
                que.append(cur)
                cur = next
                flag = 0

    print(cnt+1)

main()
```

</details>


## 027. Prefix and Suffix 


https://atcoder.jp/contests/agc006/tasks/agc006_a

<details><summary>コード</summary>

```python

def main():
    N = int(input())
    s = list(input())
    t = list(input())

    cnt = 0
    for i in range(N):
        if s[i:] == t[:N-i]:
            cnt = max(cnt, len(s[i:]))
    
    print(2*N-cnt)

main()

```

</details>

## 028. Dice in Line 


https://atcoder.jp/contests/abc154/tasks/abc154_d

<details><summary>コード</summary>

```python

def main():
    N, K = map(int, input().split())
    P = list(map(int, input().split()))
    Q = [0]
    for i in range(N):
        Q.append(Q[i]+P[i])

    s = 0
    for i in range(1, N-K+2):
        temp = Q[i+K-1] - Q[i-1]
        s = max(s, temp)

    
    print((s+K) / 2)
    
main()

```

</details>

## 029. Two Anagrams 

https://atcoder.jp/contests/abc082/tasks/abc082_b

<details><summary>コード</summary>

```python

def main():
    s = "".join(sorted(list(input())))
    t = "".join(sorted(list(input()), reverse=True))

    if s < t:
        print("Yes")
    else:
        print("No")


main()
```

</details>


## 030. Flip, Flip, and Flip...

https://atcoder.jp/contests/abc090/tasks/arc091_a


<details><summary>コード</summary>

```python

def main():
    N, M = map(int, input().split())

    if N==M==1:
        print(1)
    elif N==1 or M==1:
        print(max(N,M)-2)
    else:
        print((N-2)*(M-2))

main()
```

</details>


## 031. 4-adjacent 

https://atcoder.jp/contests/abc069/tasks/arc080_a

<details><summary>コード</summary>

```python

def main():
    N = int(input())
    A = list(map(int, input().split()))
    odd = 0
    four = 0
    even = 0

    for i in A:
        if i % 4 == 0:
            four += 1
        elif i % 2 == 1:
            odd += 1
        else:
            even += 1
    
    if four >= odd:
        print("Yes")
    elif four == odd-1 and even == 0:
        print("Yes")
    else:
        print("No")
main()
```

</details>


## 032. Otoshidama 

https://atcoder.jp/contests/abc085/tasks/abc085_c


<details><summary>コード</summary>

```python

def main():
    N,Y = map(int, input().split())

    for i in range(N+1):
        for j in range(N+1):
            if N-i-j >= 0 and 10000*i+5000*j+1000*(N-i-j) == Y:
                print(i, j, N-i-j)
                return 
    
    print(-1, -1, -1)
    return 

main()
```

</details>

## 032. Unhappy Hacking (ABC Edit)

https://atcoder.jp/contests/abc043/tasks/abc043_b

<details><summary>コード</summary>

```python
from collections import deque

def main():
    S = list(input())
    A = deque([])

    for i in S:
        if i == "0":
            A.append("0")
        elif i == "1":
            A.append("1")
        else:
            if len(A) > 0:
                A.pop()
            else:
                continue
    
    ans = "".join(list(A))
    print(ans)

main()
```

</details>


## 034. Minimization 

https://atcoder.jp/contests/abc101/tasks/arc099_a


<details><summary>コード</summary>

```python
def my_ceil(N, W):
    return (-(-N//W))

def main():
    N, K = map(int, input().split())
    A = list(map(int, input().split()))

    ans = my_ceil(N-1, K-1)
    print(ans)

main()

```

</details>

## 035. Anti-Division 

https://atcoder.jp/contests/abc131/tasks/abc131_c

<details><summary>コード</summary>

```python
import math 
def my_lcm(x, y):
    return (x * y) // math.gcd(x, y)

def main():
    A, B, C, D = map(int, input().split())
    cd = my_lcm(C, D)
    less_A = A-1

    a_num = (less_A//C) + (less_A//D) - (less_A//cd)
    b_num = (B//C) + (B//D) - (B//cd)

    
    print(B-A+1-b_num+a_num)

main()
```

</details>

## 036. Skip 

https://atcoder.jp/contests/abc109/tasks/abc109_c

<details><summary>ポイント</summary>

リストの全要素の最大公約数が`functools`の`reduce`関数を使うと簡単に求められるようです。

[参考ブログ](https://flytech.work/blog/21511/)

</details>


<details><summary>コード</summary>

```python
from math import gcd
from functools import reduce 

def main():
    N, X = map(int, input().split())
    x = list(map(int, input().split()))
    x.append(X)
    x = sorted(x)
    gap = []
    ans = 0
    for i in range(1, len(x)):
        gap.append(x[i]-x[i-1])

    ans = reduce(gcd, gap)

    print(ans)

main()
```

</details>



## 037. Grand Garden 

https://atcoder.jp/contests/abc116/tasks/abc116_c


<details><summary>コード</summary>

```python
def main():
    N = int(input())
    h = list(map(int, input().split()))

    h.insert(0,0)
    h.append(0)

    cnt = 0
    for i in range(1, len(h)):
        cnt += abs(h[i]-h[i-1])
    
    print(cnt // 2)

main()
```

</details>

## 038. Problem Set

https://atcoder.jp/contests/code-festival-2017-qualb/tasks/code_festival_2017_qualb_b


<details><summary>ポイント</summary>

`sortedcontainers`を使うととてもシンプルに実装できます。

[参考記事](https://qiita.com/Shirotsume/items/706742162db68c481c3c)

</details>

<details><summary>コード</summary>

```python
from sortedcontainers import SortedSet, SortedList, SortedDict

def main():
    N = int(input())
    D = list(map(int, input().split()))
    M = int(input())
    T = list(map(int, input().split()))

    S = SortedList(D)

    for i in range(M):
        if T[i] in S:
            S.discard(T[i])
        else:
            print("NO")
            return 
    
    print("YES")
    return 

main()  
```

</details>


## 039. When I hit my pocket...

https://atcoder.jp/contests/yahoo-procon2019-qual/tasks/yahoo_procon2019_qual_c


<details><summary>コード</summary>

```python
def main():
    K, A, B = map(int, input().split())

    if A >= B-2:
        print(K+1)
    else:
        if K + 1 >= A:
            print(((K-A+1)//2) * (B-A) + (A+(K-A+1-(K-A+1)//2*2) if (K-A+1)%2 != 0 else A))
        else:
            print(K+1)
            

main()
```

</details>

## 040. Streamline 

https://atcoder.jp/contests/abc117/tasks/abc117_c

<details><summary>コード</summary>

```python
def main():
    N, M = map(int, input().split())
    X = sorted(list(map(int, input().split())))

    gap = []
    for i in range(1, M):
        gap.append(abs(X[i]-X[i-1]))
    
    if N >= M:
        print(0)
    else:
        ans = 0
        gap = sorted(gap)
        for i in range(M-N):
            ans += gap[i]
        
        print(ans)


main()
```

</details>

## 041. Shrinking 

https://atcoder.jp/contests/agc016/tasks/agc016_a

解説AC


<details><summary>コード</summary>

```python

```

</details>



## 042. Evilator 

https://atcoder.jp/contests/agc015/tasks/agc015_b


<details><summary>コード</summary>

```python
def main():
    S = input()
    ans = 0
    for i in range(len(S)):
        if S[i] == "U":
            # going up
            ans += len(S)-(i+1)
            # going down
            ans += 2 * i
        else:
            # going up
            ans += 2 * (len(S)-(i+1))
            ans += i
    
    print(ans)

main()
```

</details>

## 043. Go Home 

https://atcoder.jp/contests/abc056/tasks/arc070_a

<details><summary>コード</summary>

```python

def main():
    X = abs(int(input()))
    i = 0

    while True:
        if (0+i) * (i-0+1) // 2 >= X:
            break 
        i += 1
    
    print(i)

main()


```

</details>

## 044. pushpush

https://atcoder.jp/contests/abc066/tasks/arc077_a

<details><summary>コード</summary>

```python
from collections import deque

def main():
    n = int(input())
    a = list(map(int, input().split()))
    b = deque([])

    if n % 2 == 0:
        for i in range(n):
            if i % 2 == 0:
                b.append(a[i])
            else:
                b.appendleft(a[i])
    else:
        for i in range(n):
            if i % 2 == 0:
                b.appendleft(a[i])
            else:
                b.append(a[i])
    
    print(*b)

main()
```

</details>

## 045. Scc puzzle 

https://atcoder.jp/contests/abc055/tasks/arc069_a


<details><summary>コード</summary>

```python

def main():
    N, M = map(int, input().split())
    ans = 0

    if N*2 <= M:
        ans += N
        M -= N*2
    else:
        print(M//2)
        return 
    
    ans += M//4

    print(ans)

main()
```

</details>


## 046. Palindrome-phobia 

https://atcoder.jp/contests/cf17-final/tasks/cf17_final_b


<details><summary>コード</summary>

```python
def main():
    S = input()
    a = S.count('a')
    b = S.count('b')
    c = S.count('c')

    if abs(a-b) <= 1 and abs(b-c) <= 1 and abs(a-c) <= 1:
        print("YES")
    else:
        print("NO")

main()

```

</details>


## 047. Biscuits 

https://atcoder.jp/contests/agc017/tasks/agc017_a

<details><summary>コード</summary>

```python
def main():
    N, P = map(int, input().split())
    A = list(map(int, input().split()))

    odd = 0
    even = 0

    for i in range(N):
        if A[i] % 2:
            odd += 1
        else:
            even += 1
    
    if odd == 0:
        if P == 1:
            print(0)
        else:
            print(2**even)
    else: # 奇数が含まれている
        print(2**(N-1))

main()


```

</details>


## 048. Rectangle Cutting 

https://atcoder.jp/contests/abc130/tasks/abc130_c

<details><summary>コード</summary>

```python
def main():
    W, H, x, y = map(int, input().split())

    if x*2 == W and y*2 == H:
        print(H*W/2, 1)
    else:
        print(H*W/2, 0)

main()
```

</details>

## 049. Make a Rectangle 

https://atcoder.jp/contests/abc071/tasks/arc081_a

<details><summary>コード</summary>

```python
from collections import Counter

def main():
    N = int(input())
    A = list(map(int, input().split()))
    C = sorted(Counter(A).items(), reverse=True)
    ans = []
    for i in C:
        if i[1] >= 4:
            ans.append(i[0])
            ans.append(i[0])
        elif i[1] >= 2:
            ans.append(i[0])
    
    if len(ans) < 2:
        print(0)
    else:
        print(ans[0]*ans[1])

main()
```

</details>


## 050. Remainder Minimization 2019 

https://atcoder.jp/contests/abc133/tasks/abc133_c

<details><summary>コード</summary>

```python
def main():
    L, R = map(int, input().split())

    if R-L >= 2019:
        return 0
    
    l = L%2019
    r = R%2019

    if l >= r:
        return 0
    else:
        mod_set = set()
        for i in range(l, r+1):
            mod_set.add(i)
        
        if 0 in mod_set:
            return 0
        else:
            ans = 2019
            mod_set = list(mod_set)
            for j in range(len(mod_set)):
                for k in range(j+1, len(mod_set)):
                    ans = min(ans, (mod_set[j]*mod_set[k])%2019)
            
            return ans
        
print(main())
```

</details>

## 051. Many Medians

https://atcoder.jp/contests/abc094/tasks/arc095_a

<details><summary>コード</summary>

```python

def main():
    N = int(input())
    X = list(map(int, input().split()))

    li = []
    for i in range(N):
        li.append([X[i], i])
    
    li = sorted(li)
    ans = [(0) for _ in range(N)]

    for j in range(N):
        if j < N//2:
            ans[li[j][1]] = li[N//2][0]
        else:
            ans[li[j][1]] = li[N//2-1][0]
    
    for k in ans:
        print(k)

main()


```

</details>


## 052. Megalomania

https://atcoder.jp/contests/abc131/tasks/abc131_d

<details><summary>コード</summary>

```python
from operator import itemgetter 
from collections import deque

def main():
    N = int(input())
    W = []
    for _ in range(N):
        a, b = map(int, input().split())
        W.append((a,b))
    
    W = deque(sorted(W, key=itemgetter(1,0)))
    cur, limit = W.popleft()

    while True:

        if len(W) == 0:
            break

        if cur <= limit:
            next_task, next_limit = W.popleft()
            cur += next_task
            limit = next_limit
        else:
            break
    
    if cur > limit:
        print("No")
    elif len(W) > 0:
        print("No")
    else:
        print("Yes")


main()
    

```

</details>

## 053. Travelling Plan 

https://atcoder.jp/contests/abc092/tasks/arc093_a

<details><summary>コード</summary>

```python
def main():
    N = int(input())
    A = list(map(int, input().split()))
    # 出発地点を追加
    A.insert(0,0)
    # 最終地点を追加
    A.append(0)

    total_dist = 0
    for j in range(1, len(A)):
        total_dist += abs(A[j]-A[j-1])
    
    for i in range(1, len(A)-1):
        prev = A[i-1]
        cur = A[i]
        nex = A[i+1]
        ans = total_dist - abs(cur-prev) - abs(nex-cur)
        ans += abs(nex-prev)
        print(ans)
            

main()
```

</details>


## 054. Airport Bus 

https://atcoder.jp/contests/agc011/tasks/agc011_a

<details><summary>コード</summary>

```python
from bisect import bisect_right 

def main():
    N, C, K = map(int, input().split())
    time = []
    for i in range(N):
        t = int(input())
        time.append(t)
    
    time = sorted(time)
    p = 0
    bus_cnt = 0

    while p < N: # while there are still people
        arrival = time[p]
        cap = 0
        while p < N and time[p] <= arrival+K and cap < C:
            cap += 1
            p += 1
        bus_cnt += 1
    
    print(bus_cnt)

main()
```

</details>


## 055. Cat Snuke and a Voyage 

https://atcoder.jp/contests/abc068/tasks/arc079_a


<details><summary>コード</summary>

```python
def main():
    N, M = map(int, input().split())
    graph = [set() for _ in range(N+1)]
    for _ in range(M):
        a, b = map(int, input().split())
        graph[a].add(b)
        graph[b].add(a)

    for i in graph[1]:
        if N in graph[i]:
            print("POSSIBLE")
            return
    
    print("IMPOSSIBLE")
    return

main()
```

</details>

## 056. Subarray Sum

https://atcoder.jp/contests/keyence2020/tasks/keyence2020_c

<details><summary>ポイント</summary>

どうやって和がSの数列を作るのかではなく、どうやって和がSの数列を必要以上に作らないかがミソ。

</details>

<details><summary>コード</summary>

```python

def main():
    N, K, S = map(int, input().split())

    if S == 10**9:
        ans = [(10**9) for _ in range(K)] + [(1) for _ in range(N-K)]
    else:
        ans = [(S) for _ in range(K)] + [(10**9) for _ in range(N-K)]
    
    print(*ans)

main()
```

</details>


## 057. Green Bin 

https://atcoder.jp/contests/abc137/tasks/abc137_c

<details><summary>コード</summary>

```python
from collections import defaultdict
from operator import mul
from functools import reduce

def cmb(n,r):
    if n <= 1: return 0
    r = min(n-r,r)
    if r == 0: return 1
    nume = reduce(mul, range(n, n - r, -1))
    denom = reduce(mul, range(1, r + 1))
    return nume // denom

def main():
    N = int(input())
    d = defaultdict(int)

    for _ in range(N):
        s = sorted(list(input()))
        s = "".join(s)
        d[s] += 1 
    
    ans = 0
    for v in d.values():
        if v >= 2:
            ans += cmb(v, 2)
    
    print(ans)

main()
```

</details>

## 058. Kenken Race

https://atcoder.jp/contests/agc034/tasks/agc034_a

<details><summary>ポイント</summary>

19WAも積んだ。やばい。すぬけ君がふぬけ君を追い越す場合、空白地帯が三連続している必要があるのだが、それの存在可能範囲はB-Dの区間になる。しかしB-Dと単純な設定ではなくもう少し考えなければだめで、そこでWAを積みまくった。

</details>


<details><summary>コード</summary>

```python


def main():
    N, A, B, C, D = map(int, input().split())
    S = list(input())
    A -= 1
    B -= 1
    C -= 1
    D -= 1

    for i in range(A, max(C,D)):
        if (A < i < C or B < i < D) and S[i] == S[i+1] == "#":
            return False
        
    if C < D:
        return True
    
    else:
        for j in range(B-1, D):
            if S[j] == S[j+1] == S[j+2] == ".":
                return True
        
    
    return False


if main():
    print("Yes")
else:
    print("No")


```

</details>

## 059. Derangement 

https://atcoder.jp/contests/abc072/tasks/arc082_b

<details><summary>コード</summary>

```python

def main():
    N = int(input())
    p = list(map(int, input().split()))
    cnt = 0
    for i in range(N-1):
        if p[i] != i+1:
            continue
        p[i], p[i+1] = p[i+1], p[i]
        cnt += 1
    
    if p[-1] == N:
        cnt += 1
        
    print(cnt)

main()

```

</details>

## 060. March 

https://atcoder.jp/contests/abc089/tasks/abc089_c

<details><summary>コード</summary>

```python

from collections import defaultdict
from itertools import combinations

def main():
    N = int(input())
    d = defaultdict(int)
    for _ in range(N):
        s = input()
        d[s[0]] += 1
    
    letters = ["M", "A", "R", "C", "H"]
    letters_cmb = list(combinations(letters, 3))

    ans = 0
    for c in letters_cmb:
        if d[c[0]] > 0 and d[c[1]] > 0 and d[c[2]] > 0:
            ans += d[c[0]] * d[c[1]] * d[c[2]]
    
    print(ans)

main()


```

</details>

## 061. String Formation 

https://atcoder.jp/contests/abc158/tasks/abc158_d

<details><summary>コード</summary>

```python
from collections import deque 

def main():
    S = deque(input())
    Q = int(input())
    isreversed = False
    for _ in range(Q):
        t = list(map(str, input().split()))
        if t[0] == "1":
            isreversed = not isreversed 
        else:
            if t[1] == "1":
                if isreversed:
                    S.append(t[2])
                else:
                    S.appendleft(t[2])
            else:
                if isreversed:
                    S.appendleft(t[2])
                else:
                    S.append(t[2])

    ans = "".join(list(S)[::-1] if isreversed else list(S))
    print(ans)

main()

```

</details>

## 062. Monsters Battle Royale 

https://atcoder.jp/contests/abc118/tasks/abc118_c

<details><summary>コード</summary>

```python
from math import gcd
from functools import reduce

def main():
    N = int(input())
    A = sorted(list(map(int, input().split())))
    ans = reduce(gcd, A)
    print(ans)

main()
```

</details>

## 063. Snuke's Coloring 2 (ABC Edit)

https://atcoder.jp/contests/abc047/tasks/abc047_b

<details><summary>コード</summary>

```python

def main():
    W, H, N = map(int, input().split())
    x_upper = W
    x_lower = 0
    y_upper = H
    y_lower = 0

    for _ in range(N):
        x, y, a = map(int, input().split())
        if a == 1:
            x_lower = max(x_lower, x)
        elif a == 2:
            x_upper = min(x_upper, x)
        elif a == 3:
            y_lower = max(y_lower, y)
        else:
            y_upper = min(y_upper, y)
    
    # print(x_upper, x_lower, y_upper, y_lower)
    x_range = r if (r := x_upper - x_lower) > 0 else 0
    y_domain = d if (d := y_upper - y_lower) > 0 else 0

    print(x_range*y_domain)


main()
```

</details>

## 064. Good Sequence 


https://atcoder.jp/contests/abc082/tasks/arc087_a

<details><summary>コード</summary>

```python
from collections import defaultdict

def main():
    N = int(input())
    a = list(map(int, input().split()))
    d = defaultdict(int)
    for i in a:
        d[i] += 1
    
    cnt = 0
    for k, v in d.items():
        if v >= k:
            cnt += (v-k)
        else:
            cnt += v
    
    print(cnt)

main()

```

</details>

## 065. Two Abbreviations 

https://atcoder.jp/contests/agc028/tasks/agc028_a

<details><summary>コード</summary>

```python
from math import lcm

def main():
    n, m = map(int, input().split())
    s = input()
    t = input()
    l = lcm(n,m)
    div_n = l // n
    div_m = l // m
    r = lcm(div_n, div_m)
    time_n = r // div_n
    time_m = r // div_m

    ncur = 0
    mcur = 0
    flag = True
    while True:
        if ncur*div_n+1 > l or mcur*div_m+1 > l:
            break 

        if s[ncur] != t[mcur]:
            flag = False
            break
        ncur += time_n
        mcur += time_m
    
    if flag:
        print(l)
    else:
        print(-1)

main()


```

</details>

## 066. Multiple Array 

https://atcoder.jp/contests/agc009/tasks/agc009_a

<details><summary>コード</summary>

```python
from math import ceil 

def main():
    N = int(input())
    A = []
    B = []
    for _ in range(N):
        a, b = map(int, input().split())
        A.append(a)
        B.append(b)
    
    cnt = 0
    for i in reversed(range(N)):
        A[i] += cnt
        if A[i]%B[i] == 0:
            continue

        diff = B[i] - (A[i]%B[i])
        cnt += diff

    print(cnt)

main()
```

</details>


## 067. Card Game for Three (ABC Edit) 

https://atcoder.jp/contests/abc045/tasks/abc045_b

<details><summary>コード</summary>

```python
from collections import deque

def main():
    A = deque(list(input()))
    B = deque(list(input()))
    C = deque(list(input()))

    winner = ""
    next = "a"

    while True:

        if next == "a":
            if len(A) == 0:
                winner = "A"
                break 
            next = A.popleft()

        elif next == "b":
            if len(B) == 0:
                winner = "B"
                break 
            next = B.popleft()

        else:
            if len(C) == 0:
                winner = "C"
                break 
            next = C.popleft()

    print(winner)

main()
            


```

</details>  

## 068. Shik and Stone 


https://atcoder.jp/contests/agc007/tasks/agc007_a

<details><summary>ポイント</summary>

常に右か下に動き続ける場合はどのルートを通っても通るタイルの数は一定。

</details>


<details><summary>コード</summary>

```python
def main():
    H, W = map(int, input().split())
    cnt = 0
    for _ in range(H):
        a = input()
        cnt += a.count("#")
    
    if cnt == (H+W-1):
        print("Possible")
    else:
        print("Impossible")

main()
```

</details>


## 069. Five Transportations 


https://atcoder.jp/contests/abc123/tasks/abc123_c

<details><summary>ポイント</summary>

ボトルネック地点を全員が通過するのにかかる時間だけ考えればよい。

</details>


<details><summary>コード</summary>

```python
from math import ceil

def main():
    N = int(input())
    T = [int(input()) for _ in range(5)]
    cnt = 4 + ceil(N/min(T))
    print(cnt)

main()
```

</details>


## 070. Simple Calculator 

https://atcoder.jp/contests/agc008/tasks/agc008_a

<details><summary>コード</summary>

```python

def main():
    x, y = map(int, input().split())
    xm = -x
    ans = 10**18

    if x + abs(x-y) == y:
        ans = min(ans, abs(x-y))
    if -(x + abs(x-y)) == y:
        ans = min(ans, abs(x-y)+1)
    if -x + abs(x-y) == y:
        ans = min(ans, abs(x-y)+1)
    if -x + abs(x-y) == -y:
        ans = min(ans, abs(x-y)+2)
    if x + abs(x+y) == y:
        ans = min(ans, abs(x+y))
    if -(x + abs(x+y)) == y:
        ans = min(ans, abs(x+y)+1)
    if -x + abs(x+y) == y:
        ans = min(ans, abs(x+y)+1)
    if -x + abs(x+y) == -y:
        ans = min(ans, abs(x+y)+2)
    
    print(ans)

main()

```

</details>

## 071. Spliting Pie 


https://atcoder.jp/contests/abc067/tasks/arc078_a


<details><summary>コード</summary>

```python

def main():
    N = int(input())
    A = list(map(int, input().split()))

    sm = sum(A)
    ans = 10**18
    snuke = 0

    for i in range(N-1):
        snuke += A[i]
        ans = min(ans, abs(sm-2*snuke))
    
    print(ans)

main()
```

</details>

## 072. Multiple Clocks 

https://atcoder.jp/contests/abc070/tasks/abc070_c

<details><summary>コード</summary>

```python
from math import lcm 
from functools import reduce 

def main():
    N = int(input())
    t = [int(input()) for _ in range(N)]
    ans = reduce(lcm, t)
    print(ans)

main()
```

</details>

## 073. Sqrt Inquality 

https://atcoder.jp/contests/panasonic2020/tasks/panasonic2020_c

<details><summary>ポイント</summary>

`Decimal`を用いた解法は嘘解法です、解説を読むまで知りませんでした。


</details>



<details><summary>コード</summary>

```python
def main():
    a, b, c = map(int, input().split())
    if c-a-b > 0 and 4*a*b < (c-a-b)**2:
        print("Yes")
    else:
        print("No")

main()
```

</details>

## 074. Dubious Document 

https://atcoder.jp/contests/abc058/tasks/arc071_a

<details><summary>コード</summary>

```python
from collections import defaultdict

def main():
    n = int(input())
    d = defaultdict(list)
    alphabet_letters = list('abcdefghijklmnopqrstuvwxyz')

    for _ in range(n):
        s = input()
        for i in range(26):
            d[alphabet_letters[i]].append(s.count(alphabet_letters[i]))
    
    ans = []

    for k, v in d.items():
        if len(v) == 0:
            continue
        for _ in range(min(v)):
            ans.append(k)

    ans = "".join(sorted(ans))

    print(ans)

main()

```

</details>

## 075. Prediction and Restriction 

https://atcoder.jp/contests/abc149/tasks/abc149_d

<details><summary>コード</summary>

```python
def main():
    N, K = map(int, input().split())
    R, S, P = map(int, input().split())
    T = list(input())

    ans = 0
    hand_li = []
    for i in range(N):
        if i < K:
            if T[i] == "r":
                hand_li.append("p")
                ans += P
            elif T[i] == "p":
                hand_li.append("s")
                ans += S
            else:
                hand_li.append("r")
                ans += R
        else:
            if T[i] == "r" and hand_li[i-K] != "p":
                hand_li.append("p")
                ans += P
            elif T[i] == "p" and hand_li[i-K] != "s":
                hand_li.append("s")
                ans += S
            elif T[i] == "s" and hand_li[i-K] != "r":
                hand_li.append("r")
                ans += R   
            else:
                hand_li.append("f")
    
    print(ans)

main()
```

</details>



## 076. Attention 

https://atcoder.jp/contests/abc098/tasks/arc098_a

<details><summary>コード</summary>

```python
def main():
    N = int(input())
    S = list(input())
    east = [0]
    west = [0]
    for i in S:
        if i == "E":
            east.append(east[-1]+1)
            west.append(west[-1])
        else:
            east.append(east[-1])
            west.append(west[-1]+1)
    
    ans = 10**18
    for j in range(1, N+1):
        num = east[-1] - east[j]
        num += west[j-1]
        ans = min(ans, num)
    
    print(ans)

main()
```

</details>


## 077. HSI

https://atcoder.jp/contests/abc078/tasks/arc085_a

<details><summary>コード</summary>

```python
def main():
    N, M = map(int, input().split())
    time_per_submission = 1900 * M + 100 * (N-M)
    ans = time_per_submission * 2 ** M
    print(ans)

main()

```

</details>

## 078. Connection and Disconnection 

https://atcoder.jp/contests/agc039/tasks/agc039_a


<details><summary>ポイント</summary>

ランレングス圧縮。コードは[こちら](https://qiita.com/Kept1994/items/e9179d1dd7c6455d6883)からお借りしました。ありがとうございます。

</details>

<details><summary>コード</summary>

```python
from itertools import groupby

def runLengthEncode(S: str):
    grouped = groupby(S)
    res = []
    for k, v in grouped:
        res.append((k, int(len(list(v)))))
    return res

def main():
    S = input()
    K = int(input())

    if len(set(S)) == 1:
        print((len(S)*K)//2)
        return
    
    S_next = S*2

    s_runlength = runLengthEncode(S)
    sn_runLength = runLengthEncode(S_next)

    s_cnt = 0
    sn_cnt = 0

    for i in s_runlength:
        s_cnt += i[1] // 2
    
    for j in sn_runLength:
        sn_cnt += j[1] // 2
    
    if K == 1:
        print(s_cnt)
    else:
        print(s_cnt + (sn_cnt-s_cnt) * (K-1))

main()


```

</details>


## 079. Reconciled

https://atcoder.jp/contests/abc065/tasks/arc076_a

<details><summary>ポイント</summary>

差は1が限界。1多いと外側に来るのが決まるので階乗取って掛け合わせるだけ。同数ならどちらが先にならんでもいいので階乗に2をかける。

</details>

<details><summary>コード</summary>

```python
from math import factorial 

def main():
    N, M = map(int, input().split())
    MOD = 10**9+7

    if abs(N-M) > 1:
        return 0
    
    if N == M:
        return ((factorial(N)%MOD)**2%MOD)*2%MOD
    
    return (factorial(N)%MOD * factorial(M)%MOD)%MOD

print(main())
```

</details>

## 080. Colorful Subsequence 

https://atcoder.jp/contests/agc031/tasks/agc031_a

<details><summary>ポイント</summary>

`aa`とかは、1個目のaを選ぶ、2個目のaを選ぶ、どちらも選ばないの3通りなので辞書で持ってvalue+1の積をとる、最後に何も選ばない1通りを引く

</details>

<details><summary>コード</summary>

```python
from collections import defaultdict

def main():
    N = int(input())
    S = input()
    d = defaultdict(int)
    MOD = 10**9+7
    for i in S:
        d[i] += 1

    ans = 1
    for v in d.values():
        ans *= v+1
        ans %= MOD
    
    print((ans-1)%MOD)

main()

    

```

</details>

## 081. 1D Reversi 

<details><summary>ポイント</summary>

ランレングス圧縮。コードは[こちら](https://qiita.com/Kept1994/items/e9179d1dd7c6455d6883)からお借りしました。ありがとうございます。

</details>

<details><summary>コード</summary>

```python
from itertools import groupby


def runLengthEncode(S: str):
    grouped = groupby(S)
    res = []
    for k, v in grouped:
        res.append((k, int(len(list(v)))))
    return res

def main():
    S = input()
    S_encoded = runLengthEncode(S)
    print(len(S_encoded)-1)

main()
```

</details>

## 082. Be Together 

https://atcoder.jp/contests/arc059/tasks/arc059_a

<details><summary>コード</summary>

```python
def main():
    N = int(input())
    A = list(map(int, input().split()))
    
    ans = 10**18
    for i in range(min(A), max(A)+1):
        cost = 0
        for j in range(N):
            cost += abs(i-A[j])**2
        ans = min(ans, cost)
    
    print(ans)

main()
```

</details>


## 083. Limited Insertion 

https://atcoder.jp/contests/agc032/tasks/agc032_a

解説AC

<details><summary>コード</summary>

```python
def main():
    N = int(input())
    B = list(map(int, input().split()))

    ans = []
    for _ in range(N):
        for j in reversed(range(len(B))):

            if B[j] == j+1:
                ans.append(j+1)
                del B[j]
                break
    
    if len(B) > 0:
        print(-1)
    else:
        for k in ans[::-1]:
            print(k)

main()

```

</details>


## 084. Triangles

https://atcoder.jp/contests/abc143/tasks/abc143_d


<details><summary>コード</summary>

```python

from bisect import bisect_left

def main():
    N = int(input())
    L = sorted(list(map(int, input().split())))
    ans = 0
    for i in range(N):
        for j in range(i+1, N):
            # i+j=kでは直線になってしまう
            idx = bisect_left(L, L[i]+L[j])-1
            #ｊより小さい場合を削除
            if idx-j > 0:
                ans += idx-j
    
    print(ans)


main()
```

</details>

## 085. Diverse World 

https://atcoder.jp/contests/agc022/tasks/agc022_a




## 086. XOR Circle 

https://atcoder.jp/contests/agc035/tasks/agc035_a

<details><summary>コード</summary>

```python
from collections import Counter

def main():
    N = int(input())
    A = list(map(int, input().split()))

    c = sorted(list(Counter(A).items()))
    if len(c) == 1 and c[0][0] == 0:
        return True
    if len(c) == 2 and c[0][0] == 0 and c[0][1] * 2 == c[1][1]:
        return True
    if len(c) == 3 and c[0][1] == c[1][1] == c[2][1] and c[0][0]^c[1][0]^c[2][0] == 0:
        return True
    return False


if main():
    print("Yes")
else:
    print("No")
```

</details>


## 087. Exam and Wizard 

https://atcoder.jp/contests/keyence2019/tasks/keyence2019_c

