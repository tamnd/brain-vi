---
title: "CF 1118C - Ma trận Palindromic"
description: "Chúng ta được yêu cầu sắp xếp n^2 số nguyên thành một ma trận vuông n x n sao cho ma trận đối xứng dọc theo cả hai trục. Ma trận palindromic không thay đổi khi đảo ngược thứ tự hàng hoặc đảo ngược thứ tự cột."
date: "2026-06-12T04:32:44+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1118
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 540 (Div. 3)"
rating: 1700
weight: 1118
solve_time_s: 86
verified: false
draft: false
---

[CF 1118C - Ma trận Palindromic](https://codeforces.com/problemset/problem/1118/C) 

**Đánh giá:** 1700 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu sắp xếp`n^2`số nguyên thành một`n x n`ma trận vuông sao cho ma trận đối xứng dọc theo cả hai trục. Ma trận palindromic không thay đổi khi đảo ngược thứ tự hàng hoặc đảo ngược thứ tự cột. Một cách khác để thấy điều đó là mỗi ô`(i, j)`phải phản ánh các đối tác đối xứng của nó`(n-i-1, j)`,`(i, n-j-1)`, Và`(n-i-1, n-j-1)`. Ví dụ: bốn góc của ma trận đều phải chứa cùng một số. Nguyên tắc tương tự được áp dụng đệ quy cho các lớp bên trong. 

Đầu vào cho chúng ta kích thước`n`và một danh sách`n^2`số để đặt. Chúng ta cần xây dựng một ma trận như vậy bằng cách sử dụng chính xác mỗi số một lần hoặc báo cáo là không thể. Từ`n`nhiều nhất là 20, tổng số ô nhiều nhất là 400. Con số này đủ nhỏ cho một phương pháp kiểm tra sự kết hợp của các số vào các vị trí theo tần số, dưới dạng liệt kê mạnh mẽ tất cả các hoán vị (có thể là`400!`) rõ ràng là không thể thực hiện được. 

Các trường hợp cạnh không rõ ràng phát sinh khi`n`thật kỳ quặc. Trong ma trận cỡ lẻ, chỉ có một ô ở giữa và`2n-2`các ô dọc hàng giữa và cột giữa chỉ cần phản chiếu một lần chứ không phải bốn lần. Nếu những vị trí này không thể được lấp đầy bằng các số xuất hiện trong bội số bắt buộc thì việc triển khai đơn giản chỉ kiểm tra các bộ bốn sẽ thất bại. Ví dụ, đối với`n = 3`, ô trung tâm có thể lấy bất kỳ số nào xuất hiện ít nhất một lần, trong khi bốn góc cần số xuất hiện ít nhất bốn lần. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi hoán vị của`n^2`số và kiểm tra xem ma trận kết quả có phải là palindromic hay không. Điều này hoạt động rất nhỏ`n`, nhưng số lượng hoán vị tăng theo giai thừa và trở nên không khả thi ngay cả đối với`n = 5`, đó sẽ là`25!`hoán vị. 

Cái nhìn sâu sắc quan trọng là khai thác tính đối xứng. Mỗi vị trí trong ma trận thuộc về một nhóm gồm bốn, hai hoặc một ô, tất cả đều phải chứa cùng một số. Cụ thể, ngay cả đối với`n`, tất cả các vị trí tạo thành nhóm gồm bốn ô, vì vậy mỗi số phải xuất hiện ở dạng bội số của bốn. Đối với số lẻ`n`, ô trung tâm là một ô đơn, các vị trí trên hàng hoặc cột trung tâm không bao gồm các cặp biểu mẫu trung tâm và các vị trí còn lại tạo thành bốn phần. Quan sát này làm giảm vấn đề đếm tần số của các số và sắp xếp chúng thành các nhóm có kích thước 4, 2 hoặc 1, điều này không quan trọng về mặt tính toán đối với`n ≤ 20`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n^2)!) | O(n^2) | Quá chậm | 
| Nhóm tần số | O(n^2) | O(1000) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của từng số trong mảng đầu vào. Điều này cho chúng ta biết chúng ta có thể tạo thành bao nhiêu bộ tứ, cặp và số đơn từ mỗi số. 
2. Khởi tạo một`n x n`ma trận chứa đầy số không. Chúng ta sẽ tô từng lớp một từ ngoài cùng đến trong cùng. 
3. Nếu`n`chẵn, tất cả các ô đều thuộc nhóm bốn. Lặp lại trên góc phần tư phía trên bên trái và gán các số xuất hiện ít nhất bốn lần cho mỗi ô vuông 2x2 của góc phần tư. Sau khi đặt một số, hãy giảm tần số của nó đi bốn. Nếu không có số nào còn lại ít nhất bốn khi cần, hãy xuất "NO". 
4. Nếu`n`là số lẻ, trước tiên hãy xử lý các lớp bên ngoài chính xác như trong trường hợp chẵn. Sau đó xử lý hàng trung tâm và cột trung tâm (không bao gồm ô trung tâm) bằng cách sử dụng các số có ít nhất hai bản sao còn lại. Gán một số cho cặp được phản chiếu và giảm số lượng của nó đi hai. 
5. Cuối cùng, xử lý ô trung tâm duy nhất, ô này có thể được điền bằng bất kỳ số nào với ít nhất một số đếm còn lại. 
6. Xuất "CÓ" và ma trận đã hoàn thành. 

**Tại sao nó hoạt động:** Ở mọi giai đoạn, thuật toán đảm bảo rằng tần số của các số có thể đáp ứng các yêu cầu về tính đối xứng của vị trí của chúng. Nhóm bốn đảm bảo sự đối xứng ở các góc, nhóm cặp đảm bảo sự đối xứng giữa hàng/cột và nhóm đơn đảm bảo sự đối xứng ở giữa. Nếu ở bất kỳ giai đoạn nào nhóm yêu cầu không thể được hình thành thì không thể xây dựng được ma trận palindromic. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import Counter

n = int(input())
a = list(map(int, input().split()))
freq = Counter(a)
matrix = [[0]*n for _ in range(n)]

def place_quad(i, j):
    for num in list(freq):
        if freq[num] >= 4:
            matrix[i][j] = matrix[i][n-j-1] = matrix[n-i-1][j] = matrix[n-i-1][n-j-1] = num
            freq[num] -= 4
            if freq[num] == 0:
                del freq[num]
            return True
    return False

def place_pair(i, j, mirrored_i, mirrored_j):
    for num in list(freq):
        if freq[num] >= 2:
            matrix[i][j] = matrix[mirrored_i][mirrored_j] = num
            freq[num] -= 2
            if freq[num] == 0:
                del freq[num]
            return True
    return False

# Fill quadruples
for i in range(n//2):
    for j in range(n//2):
        if not place_quad(i, j):
            print("NO")
            sys.exit()

# Fill middle row/column if n is odd
if n % 2 == 1:
    mid = n//2
    for i in range(n//2):
        if not place_pair(mid, i, mid, n-i-1):
            print("NO")
            sys.exit()
    for i in range(n//2):
        if not place_pair(i, mid, n-i-1, mid):
            print("NO")
            sys.exit()
    # Fill center
    for num in list(freq):
        if freq[num] >= 1:
            matrix[mid][mid] = num
            freq[num] -= 1
            if freq[num] == 0:
                del freq[num]
            break
    else:
        print("NO")
        sys.exit()

print("YES")
for row in matrix:
    print(' '.join(map(str,row)))
```Giải pháp đầu tiên đếm tần số số, giúp bạn dễ dàng biết số nào có thể lấp đầy bộ bốn hoặc cặp. các`place_quad`Và`place_pair`các hàm trực tiếp xử lý tính đối xứng bằng cách điền vào các vị trí được phản chiếu. Đối với số lẻ`n`, việc xử lý cẩn thận hàng giữa, cột giữa và tâm đảm bảo rằng tất cả các điều kiện đối xứng đều được thỏa mãn. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:`4 1 8 8 1 2 2 2 2 2 2 2 2 1 8 8 1`| Bước | Hành động | Ma trận | 
| --- | --- | --- | 
| 1 | Điền vào các góc phần tư 2x2 phía trên bên trái bằng các số có tần số ≥ 4 | [[1,2,2,1],[8,2,2,8],[8,2,2,8],[1,2,2,1]] | 

Ma trận được lấp đầy; xuất ra "CÓ". Điều này xác nhận tăng gấp bốn lần là đủ cho thậm chí`n`. 

**Ví dụ kỳ lạ** 

đầu vào:`3 1 1 1 1 2 2 2 2 3`| Bước | Hành động | Ma trận | 
| --- | --- | --- | 
| 1 | Điền vào các góc (tăng gấp bốn lần) | [[1,2,1],[2,0,2],[1,2,1]] | 
| 2 | Điền vào trung tâm | [[1,2,1],[2,3,2],[1,2,1]] | 

Xác nhận thuật toán xử lý số lẻ`n`với một trung tâm duy nhất một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi ô ma trận được điền một lần và kiểm tra tần số là O(1) mỗi số | 
| Không gian | O(n^2 + U) | O(n^2) cho ma trận, O(U) cho Bộ đếm các số duy nhất (U ≤ 1000) | 

Vì n 20, n^2 ≤ 400 nên thuật toán dễ dàng chạy trong vòng 2 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open("solution.py").read())
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("4\n1 8 8 1 2 2 2 2 2 2 2 2 1 8 8 1\n") == "YES\n1 2 2 1\n8 2 2 8\n8 2 2 8\n1 2 2 1", "sample 1"

# Minimum size n=1
assert run("1\n5\n") == "YES\n5", "min size"

# Maximum size n=20, all equal numbers
all_equal = " ".join(["1"]*400)
assert run(f"20\n{all_equal}\n").startswith("YES"), "max size equal numbers"

# Odd n, impossible
assert run("3\n1 1 2 2 3 3 4 4 5\n") == "NO", "odd impossible"

# Odd n, exact frequencies
assert run("3\n1 1 1 1
```
