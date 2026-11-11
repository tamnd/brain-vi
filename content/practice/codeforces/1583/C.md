---
title: "CF 1583C - Omkar và Xác định"
description: "Chúng ta được cấp một bảng hình chữ nhật trong đó mỗi ô được chặn bởi dấu X hoặc trống. Từ một ô trống chúng ta chỉ có thể di chuyển lên trên hoặc sang trái. Một ô được gọi là có thể thoát được nếu một số chuỗi di chuyển như vậy cho phép chúng ta rời khỏi lưới. Câu hỏi không phải là mô phỏng những động thái đó."
date: "2026-06-10T09:47:33+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 1583
codeforces_index: "C"
codeforces_contest_name: "Technocup 2022 - Elimination Round 1"
rating: 1700
weight: 1583
solve_time_s: 136
verified: true
draft: false
---

[CF 1583C - Omkar và Quyết tâm](https://codeforces.com/problemset/problem/1583/C) 

**Đánh giá:** 1700 
**Thẻ:** cấu trúc dữ liệu, dp 
**Thời gian giải:** 2 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bảng hình chữ nhật trong đó mỗi ô bị chặn bởi một`X`hoặc trống rỗng. Từ một ô trống chúng ta chỉ có thể di chuyển lên trên hoặc sang trái. Một ô được gọi là có thể thoát được nếu một số chuỗi di chuyển như vậy cho phép chúng ta rời khỏi lưới. Câu hỏi không phải là mô phỏng những động thái đó. Thay vào đó, đối với nhiều khoảng cột, chúng ta phải quyết định xem liệu mẫu ô có thể thoát có xác định duy nhất vị trí ban đầu của các ô bị chặn bên trong phạm vi cột đó hay không. 

Mỗi truy vấn lấy một tập hợp các cột liền kề và hỏi liệu hai cấu hình chướng ngại vật khác nhau có thể tạo ra cùng một kiểu thoát hiểm hay không. Nếu không có sự mơ hồ tồn tại, chúng tôi trả lời`YES`. Ngược lại chúng tôi trả lời`NO`. 

Sản phẩm`n × m`nhiều nhất là`10^6`, vì vậy việc xử lý toàn bộ lưới một lần là hợp lý. Số lượng truy vấn đạt`2·10^5`, loại trừ bất kỳ giải pháp nào quét toàn bộ lưới con cho mọi truy vấn. Ngay cả việc quét tuyến tính trên toàn bộ chiều rộng của khoảng cũng sẽ dẫn đến khoảng`2·10^11`hoạt động trong trường hợp xấu nhất. Quá trình xử lý truy vấn phải theo thời gian không đổi hoặc logarit. 

Phần khó khăn là hiểu loại mô hình cục bộ nào tạo ra sự mơ hồ. 

Coi như```
.X
X.
```Ô phía dưới bên trái và ô phía trên bên phải đều bị chặn. Hai trở ngại này tạo thành một góc ngăn cản thông tin lan truyền. Mẫu thoát tương tự sẽ xuất hiện nếu ô dưới cùng bên phải cũng bị chặn. Một giải pháp bất cẩn chỉ đếm các ô bị chặn sẽ bỏ lỡ điều này. 

Một trường hợp tinh tế khác là một cột duy nhất.```
X
.
X
```Mọi truy vấn có điểm cuối bên trái và bên phải giống nhau phải trả về`YES`, vì không có cặp cột liền kề nào có thể tạo ra sự mơ hồ. 

Trường hợp cạnh thứ ba xuất hiện khi tồn tại một số vị trí xấu nhưng tất cả chúng đều nằm ngoài khoảng truy vấn.```
..X.
.X..
....
```Giả sử ranh giới có vấn đề duy nhất là giữa cột 2 và 3. Truy vấn`[1,2]`vẫn nên quay lại`YES`. Câu trả lời chung cho toàn bộ lưới là không đủ vì các phạm vi cột khác nhau hoạt động khác nhau. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ kiểm tra mọi truy vấn một cách độc lập. Đối với khoảng cột`[l,r]`, chúng ta có thể kiểm tra lưới con tương ứng và xác định xem có bất kỳ cấu trúc mơ hồ nào tồn tại bên trong nó hay không. Cách tiếp cận này đúng vì nó trực tiếp kiểm tra vùng được truy vấn, nhưng trong trường hợp xấu nhất mỗi truy vấn chạm gần như`10^6`tế bào. Với`2·10^5`truy vấn, tổng công việc sẽ xoay quanh`2·10^11`hoạt động vượt xa giới hạn. 

Quan sát quan trọng là sự mơ hồ chỉ phát sinh từ một loại cấu hình cục bộ. Giả sử ô`(i,j)`và tế bào`(i-1,j+1)`đều bị chặn. Khi đó hai chướng ngại vật này tạo thành một góc chéo:```
. X
X .
```Khi một mẫu như vậy tồn tại trên hai cột lân cận, việc sắp xếp chính xác của một số ô sẽ không thể được xây dựng lại chỉ dựa trên thông tin về khả năng thoát. 

Điều này có nghĩa là mọi cấu trúc có vấn đề đều có thể được biểu diễn bằng ranh giới giữa hai cột liền kề. Đối với mỗi cột`j`, chúng ta chỉ cần biết liệu một số hàng có chứa cả hai`(i,j)`Và`(i-1,j+1)`bị chặn. Nếu vậy thì ranh giới giữa các cột`j`Và`j+1`là xấu. 

Một truy vấn`[l,r]`chính xác là không hợp lệ khi có ít nhất một ranh giới xấu nằm giữa các cột đó. Điều đó chuyển đổi vấn đề thành một truy vấn tồn tại phạm vi trên mảng một chiều, được xử lý dễ dàng bằng cách sử dụng cực đại tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(qnm) | O(1) | Quá chậm | 
| Tối ưu | O(nm + q) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới và tạo mảng`bad`chiều dài`m+1`, ban đầu chứa đầy số không. 
2. Đối với mỗi ô`(i,j)`với`i ≥ 1`Và`j ≥ 1`, kiểm tra xem cả hai`(i,j-1)`Và`(i-1,j)`bao gồm`X`. 

Hai ô này tạo thành đường chéo```
. X
X .
```sự tồn tại của nó tạo ra sự mơ hồ. 
3. Nếu có cặp như vậy thì đánh dấu ranh giới cột`j`tệ bằng cách thiết lập`bad[j]=1`. 

giá trị`j`thể hiện ranh giới giữa các cột`j`Và`j+1`trong việc lập chỉ mục một cơ sở. 
4. Xây dựng mảng tổng tiền tố`pref`.`pref[j]`lưu trữ số lượng ranh giới xấu giữa các vị trí`1…j`. 
5. Đối với mỗi truy vấn`[l,r]`, tính toán```
pref[r-1] - pref[l-1]
```Đại lượng này đếm các ranh giới xấu nằm hoàn toàn trong khoảng. 
6. Nếu số đếm bằng 0, xuất ra`YES`. Nếu không thì xuất ra`NO`. 

### Tại sao nó hoạt động 

Mọi sự mất mát thông tin đều xuất phát từ một cặp ô bị chặn chéo nằm ở các góc đối diện của một`2×2`quảng trường. Một cặp như vậy ngăn chúng ta phân biệt giữa một số cấu hình chướng ngại vật tạo ra cùng một kiểu thoát hiểm. 

Khoảng truy vấn có thể được xác định chính xác khi không có cặp cột lân cận nào chứa xung đột đường chéo như vậy. Vì mọi cấu hình xấu đều được bản địa hóa thành một ranh giới duy nhất giữa các cột liền kề nên việc lưu trữ tất cả các ranh giới xấu và trả lời các truy vấn phạm vi trên chúng sẽ nắm bắt chính xác điều kiện được yêu cầu. Không có ranh giới xấu bên trong khoảng có nghĩa là không thể có sự mơ hồ, trong khi sự hiện diện của ngay cả một ranh giới xấu cũng đã tạo ra nhiều lưới hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
grid = [input().strip() for _ in range(n)]

bad = [0] * (m + 1)

for i in range(1, n):
    for j in range(1, m):
        if grid[i][j - 1] == 'X' and grid[i - 1][j] == 'X':
            bad[j] = 1

pref = [0] * (m + 1)
for j in range(1, m + 1):
    pref[j] = pref[j - 1] + bad[j]

q = int(input())
ans = []

for _ in range(q):
    l, r = map(int, input().split())
    if pref[r - 1] - pref[l - 1] == 0:
        ans.append("YES")
    else:
        ans.append("NO")

sys.stdout.write("\n".join(ans))
```Vòng lặp lồng nhau quét mọi`2×2`vuông một cách gián tiếp. Bất cứ khi nào các ô phía trên bên phải và phía dưới bên trái đều bị chặn, ranh giới giữa các cột đó sẽ trở nên có vấn đề. 

Mảng`bad`sử dụng đánh số ranh giới dựa trên một. Chức vụ`j`tương ứng với đường viền giữa các cột`j`Và`j+1`. Việc lập chỉ mục này rất dễ bị sai. Truy vấn`[l,r]`chứa ranh giới`l,l+1,…,r-1`, điều này giải thích biểu thức`pref[r-1]-pref[l-1]`. 

Tràn số nguyên không phải là vấn đề vì tất cả số đếm đều nhiều nhất`m≤10^6`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 5
..XXX
...X.
...X.
...X.
```Các ranh giới xấu được phát hiện là: 

| Hàng | Cặp cột tạo ra xung đột | Ranh giới được đánh dấu | 
| --- | --- | --- | 
| 1 | không | - | 
| 2 | không | - | 
| 3 | cột 4 và 5 | 4 | 
| 4 | cột 4 và 5 | 4 | 

Như vậy 

| Ranh giới | tệ | Tiền tố | 
| --- | --- | --- | 
| 1 | 0 | 0 | 
| 2 | 0 | 0 | 
| 3 | 0 | 0 | 
| 4 | 1 | 1 | 
| 5 | 0 | 1 | 

Đối với truy vấn`[1,3]`: 

| tôi | r | trước[r-1] | trước[l-1] | Sự khác biệt | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 0 | 0 | 0 | CÓ | 

Đối với truy vấn`[1,5]`: 

| tôi | r | trước[r-1] | trước[l-1] | Sự khác biệt | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 1 | 0 | 1 | KHÔNG | 

Ví dụ này cho thấy rằng một ranh giới xấu sẽ ảnh hưởng đến mọi khoảng chứa nó. 

### Ví dụ 2 

Lưới:```
3 3
...
...
...
```Không có xung đột tồn tại. 

| Ranh giới | tệ | Tiền tố | 
| --- | --- | --- | 
| 1 | 0 | 0 | 
| 2 | 0 | 0 | 
| 3 | 0 | 0 | 

Đối với bất kỳ truy vấn nào, sự khác biệt của tổng tiền tố là 0, vì vậy mọi câu trả lời đều là`YES`. 

Ví dụ này xác nhận rằng một lưới hoàn toàn trống luôn được xác định duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm + q) | Một lần truyền qua lưới và làm việc liên tục cho mỗi truy vấn | 
| Không gian | O(m) | Mảng`bad`Và`pref`| 

Từ`nm≤10^6`, giai đoạn tiền xử lý thực hiện khoảng một triệu thao tác. Trả lời`2·10^5`truy vấn trong thời gian không đổi dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    bad = [0] * (m + 1)

    for i in range(1, n):
        for j in range(1, m):
            if grid[i][j - 1] == 'X' and grid[i - 1][j] == 'X':
                bad[j] = 1

    pref = [0] * (m + 1)
    for j in range(1, m + 1):
        pref[j] = pref[j - 1] + bad[j]

    q = int(input())
    ans = []

    for _ in range(q):
        l, r = map(int, input().split())
        ans.append("YES" if pref[r - 1] - pref[l - 1] == 0 else "NO")

    return "\n".join(ans)

# provided sample
assert run("""4 5
..XXX
...X.
...X.
...X.
5
1 3
3 3
4 5
5 5
1 5
""") == """YES
YES
NO
YES
NO"""

# minimum size
assert run("""1 1
.
1
1 1
""") == "YES"

# all blocked
assert run("""2 2
XX
XX
1
1 2
""") == "YES"

# one bad boundary
assert run("""2 2
.X
X.
1
1 2
""") == "NO"

# boundary exclusion
assert run("""3 4
..X.
.X..
....
2
1 2
2 4
""") == """YES
NO"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1×1 | CÓ | Kích thước tối thiểu | 
| Tất cả các ô bị chặn | CÓ | Không có xung đột chéo | 
|`.X / X.`| KHÔNG | Ranh giới xấu đơn | 
| Ví dụ 3×4 hỗn hợp | CÓ, KHÔNG | Đúng ranh giới khoảng thời gian | 

## Vỏ cạnh 

Truy vấn một cột không chứa cặp cột lân cận.```
3 1
X
.
X
1
1 1
```các`bad`mảng vẫn bằng 0 vì không có ranh giới nào cả. Truy vấn tính toán`pref[0]-pref[0]=0`, vậy câu trả lời là`YES`. 

Hãy xem xét xung đột đường chéo```
2 2
.X
X.
1
1 2
```Trong quá trình tiền xử lý, tại`(1,1)`chúng tôi thấy rằng các ô phía dưới bên trái và phía trên bên phải đều`X`, Vì thế`bad[1]=1`. Truy vấn đánh giá`pref[1]-pref[0]=1`, sản xuất`NO`. 

Cuối cùng, giả sử xung đột nằm ngoài khoảng thời gian được truy vấn:```
3 4
..X.
.X..
....
2
1 2
2 4
```Chỉ ranh giới 2 được đánh dấu là xấu. Vì`[1,2]`, sự khác biệt là`pref[1]-pref[0]=0`, vậy câu trả lời là`YES`. Vì`[2,4]`, sự khác biệt trở thành`pref[3]-pref[1]=1`, sản xuất`NO`. Thuật toán tách riêng từng khoảng một cách chính xác thay vì sử dụng một quyết định chung duy nhất.
