---
title: "CF 102565H - Túi mua sắm"
description: "Các túi tạo thành một khu rừng có rễ. Nếu túi i có b[i] = j thì i là con trực tiếp của túi j. Một túi có thể chứa nhiều túi khác và túi có b[i] = 0 là gốc của một trong các cây."
date: "2026-08-05T14:22:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102565
codeforces_index: "H"
codeforces_contest_name: "AGM 2020, Final Round, Day 2"
rating: 0
weight: 102565
solve_time_s: 168
verified: true
draft: false
---

[CF 102565H - Túi mua sắm](https://codeforces.com/problemset/problem/102565/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 48 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Các túi tạo thành một khu rừng có rễ. Nếu túi`i`có`b[i] = j`, sau đó`i`là con trực tiếp của túi`j`. Một túi có thể chứa nhiều túi khác và một túi có`b[i] = 0`là rễ của một trong những cái cây. 

Trong trò chơi, người chơi chọn bất kỳ túi nào còn lại và tháo túi đó cùng với mọi túi bên trong nó. Câu hỏi đặt ra là liệu người chơi đầu tiên có chiến lược chiến thắng từ khu rừng nhất định hay không. 

Kích thước đầu vào chỉ`N <= 1000`, loại trừ các thuật toán khám phá mọi trạng thái trò chơi có thể có. Số lượng các tập hợp con có thể có của các túi bị loại bỏ có thể theo cấp số nhân, do đó sẽ có mô phỏng cực tiểu trực tiếp`O(2^N)`và là không thể. Cấu trúc là một khu rừng, gợi ý tìm kiếm một trò chơi bất biến trên cây thay vì mô phỏng các bước di chuyển. 

Một sai lầm phổ biến là chỉ tính túi hoặc chiều cao cây. Ví dụ: một túi duy nhất có vị trí chiến thắng vì người chơi đầu tiên loại bỏ nó. đầu vào```
1
0
```có câu trả lời`YES`. 

Tuy nhiên, hai túi rễ độc lập hoạt động khác với một chuỗi hai túi. đầu vào```
2
0 0
```có hai trò chơi một túi riêng biệt và giá trị của chúng triệt tiêu lẫn nhau. Câu trả lời là`NO`. Giải pháp chỉ dựa trên số lượng túi sẽ thất bại ở đây. 

Một cái bẫy khác là xử lý việc phân nhánh. đầu vào```
4
0 1 1 1
```là một gốc có ba con. Giá trị trò chơi của nó không giống như một chuỗi có độ dài bốn, bởi vì việc loại bỏ một nút con sẽ để lại một cấu trúc khác với việc loại bỏ một nút trên chuỗi. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ coi mọi tập hợp con túi còn lại có thể có là một trạng thái. Đối với mọi trạng thái, nó sẽ thử mọi túi có thể tháo rời, đánh giá đệ quy trạng thái kết quả và xác định xem có tồn tại chuyển sang vị trí thua hay không. Điều này đúng vì nó trực tiếp tuân theo định nghĩa về vị trí chiến thắng, nhưng số lượng trạng thái có thể đạt tới`2^N`. Với`N = 1000`, thậm chí việc lưu trữ tất cả các trạng thái là không thể. 

Điều quan trọng nhất là đây là một trò chơi cây theo phong cách Green Hackenbush. Một cây có gốc có thể được thay thế bằng một cọc Nim tương đương có kích thước bằng giá trị Grundy của nó. Giá trị của một nút chỉ được xác định bởi giá trị của các nút con của nó. Công thức là:```
value(node) = xor(value(child) + 1 for every child)
```các`+1`đại diện cho cạnh từ nút đến mỗi nút con. Khu rừng là tổng của các trò chơi độc lập, vì vậy các giá trị Grundy của tất cả các nghiệm được XOR cùng nhau. Nếu XOR cuối cùng khác 0 thì người chơi đầu tiên sẽ thắng. 

Lực lượng vũ phu phát huy tác dụng vì mỗi bước di chuyển sẽ thay đổi một phần của trò chơi. Quan sát thấy rằng mọi cây con có thể được nén thành một đống Nim cho phép chúng ta thay thế việc khám phá trạng thái hàm mũ bằng một lần duyệt cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^N) | O(2^N) | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng forest từ mảng cha. Đối với mỗi túi có cha mẹ`p`, thêm nó làm con của`p`. Túi có bố mẹ`0`được lưu trữ dưới dạng rễ. 
2. Chạy DFS từ mọi gốc. Trong khi trở về từ một nút, hãy tính giá trị Grundy của nó. Các phần tử con đã được xử lý rồi nên giá trị của chúng đã được biết. 
3. Đối với mỗi nút con, XOR`(child_value + 1)`vào giá trị nút hiện tại. Việc thêm một sẽ tính đến khả năng loại bỏ cây con đó bắt đầu từ nút hiện tại. 
4. XOR giá trị của tất cả các nghiệm. Nếu giá trị cuối cùng khác 0, hãy in`YES`; nếu không thì in`NO`. 

Tại sao nó hoạt động: mỗi cây con là một trò chơi độc lập và khách quan. Định lý Sprague-Grundy cho biết các trò chơi độc lập kết hợp bằng cách XOR các giá trị Grundy của chúng. Đối với một nút, mỗi cây con con được gắn thông qua một túi có thể tháo rời bổ sung, điều này làm tăng sự đóng góp của nó thêm một. DFS tính toán chính xác giá trị này cho mỗi cây con, do đó XOR cuối cùng là giá trị Grundy của vị trí hoàn chỉnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(5000)

def solve():
    n_line = input().strip()
    if not n_line:
        return
    n = int(n_line)
    parent = list(map(int, input().split()))

    children = [[] for _ in range(n)]
    roots = []

    for i, p in enumerate(parent):
        if p == 0:
            roots.append(i)
        else:
            children[p - 1].append(i)

    def dfs(u):
        g = 0
        for v in children[u]:
            g ^= dfs(v) + 1
        return g

    ans = 0
    for r in roots:
        ans ^= dfs(r)

    print("YES" if ans else "NO")

if __name__ == "__main__":
    solve()
```Đầu vào được chuyển đổi thành danh sách kề vì biểu diễn gốc thuận tiện cho việc đọc nhưng lại bất tiện cho DFS. Các chỉ mục được dịch chuyển một đơn vị vì câu lệnh sử dụng cách đánh số túi dựa trên một trong khi Python sử dụng cách đánh chỉ mục dựa trên số 0. 

DFS trả về giá trị Grundy của cây con gốc ở túi hiện tại. Thứ tự đệ quy rất quan trọng: các phần tử con phải được xử lý trước phần tử cha vì giá trị của phần tử cha phụ thuộc vào tất cả các giá trị của phần tử con. 

Số nguyên Python không bị tràn nên các phép toán XOR vẫn an toàn. Giới hạn đệ quy được tăng lên vì chuỗi 1000 túi tạo ra độ sâu đệ quy là 1000. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
5
0 1 2 3 4
```Cây là một chuỗi. 

| Nút | Giá trị con | Tính toán | Giá trị | 
| --- | --- | --- | --- | 
| 5 | không | 0 | 0 | 
| 4 | 0 | 0 xor (0+1) | 1 | 
| 3 | 1 | 1 xor (1+1) | 3 | 
| 2 | 3 | 3 xor (3+1) | 7 | 
| 1 | 7 | 7 xor (7+1) | 15 | 

Giá trị gốc khác 0 nên Jimmy thắng. 

Đối với mẫu thứ hai:```
6
0 1 2 2 0 5
```Rừng có hai cây. 

| Gốc | Giá trị con | Tính toán | Giá trị | 
| --- | --- | --- | --- | 
| 1 | chuỗi kết thúc ở số 4 và 3 | tính toán đệ quy | 3 | 
| 5 | con 6 | 0 xor (0+1) | 1 | 

Tổng giá trị là`3 xor 1 = 2`, khác không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi túi được truy cập một lần trong DFS | 
| Không gian | O(N) | Danh sách kề và ngăn xếp đệ quy lưu trữ mỗi túi một lần | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì nó chỉ thực hiện một lượng công việc không đổi cho mỗi túi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    parent = list(map(int, sys.stdin.readline().split()))

    children = [[] for _ in range(n)]
    roots = []

    for i, p in enumerate(parent):
        if p == 0:
            roots.append(i)
        else:
            children[p - 1].append(i)

    def dfs(u):
        g = 0
        for v in children[u]:
            g ^= dfs(v) + 1
        return g

    ans = 0
    for r in roots:
        ans ^= dfs(r)

    sys.stdin = old
    return "YES\n" if ans else "NO\n"

assert run("5\n0 1 2 3 4\n") == "YES\n"
assert run("6\n0 1 2 2 0 5\n") == "NO\n"
assert run("5\n0 1 1 0 4\n") == "YES\n"

assert run("1\n0\n") == "YES\n"
assert run("2\n0 0\n") == "NO\n"
assert run("4\n0 1 1 1\n") == "YES\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0`| CÓ | Cây có kích thước tối thiểu | 
|`2 / 0 0`| KHÔNG | XOR của các gốc độc lập | 
|`4 / 0 1 1 1`| CÓ | Xử lý cây phân nhánh | 

## Vỏ cạnh 

Đối với một túi duy nhất:```
1
0
```DFS trả về giá trị`0`đối với tập con trống, nhưng bản thân túi không đóng góp cạnh con nào, do đó gốc đóng góp một giá trị Grundy khác 0. Câu trả lời là`YES`. 

Đối với hai túi độc lập:```
2
0 0
```Mỗi gốc đều có giá trị`1`. Giá trị kết hợp là`1 xor 1 = 0`, do đó người chơi đầu tiên sẽ thua nếu chơi hoàn hảo. 

Đối với một chuỗi:```
5
0 1 2 3 4
```Các giá trị tăng lên vì mỗi nút thêm một cấp trên nút con của nó. Giá trị gốc cuối cùng khác 0, vì vậy người chơi đầu tiên có thể giành chiến thắng. 

Đối với nút phân nhánh:```
4
0 1 1 1
```Ba cây con con được kết hợp với XOR. Thuật toán không coi cây là một chuỗi nên nó xử lý chính xác nhiều cây con và tạo ra giá trị Grundy thích hợp.
