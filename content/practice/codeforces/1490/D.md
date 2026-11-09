---
title: "CF 1490D - Phép biến đổi hoán vị"
description: "Chúng ta được cho một hoán vị có độ dài $n$, là một mảng chứa mọi số nguyên từ 1 đến $n$ đúng một lần. Nhiệm vụ là xem hoán vị này như một cách để xây dựng cây nhị phân: gốc là phần tử lớn nhất, các phần tử ở bên trái của nó tạo thành cây con bên trái…"
date: "2026-06-10T22:39:20+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "divide-and-conquer", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1490
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 702 (Div. 3)"
rating: 1200
weight: 1490
solve_time_s: 152
verified: false
draft: false
---

[CF 1490D - Chuyển đổi hoán vị](https://codeforces.com/problemset/problem/1490/D) 

**Đánh giá:** 1200 
**Tags:** dfs và tương tự, phân chia và chinh phục, thực hiện 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị độ dài$n$, là một mảng chứa mọi số nguyên từ 1 đến$n$đúng một lần. Nhiệm vụ là xem hoán vị này như một cách để xây dựng cây nhị phân: gốc là phần tử lớn nhất, các phần tử bên trái của nó tạo thành cây con bên trái theo cách đệ quy và các phần tử ở bên phải tạo thành cây con bên phải theo cách đệ quy. Đối với mỗi phần tử trong hoán vị, chúng ta muốn tính độ sâu của nó trong cây này, trong đó gốc có độ sâu 0, phần tử con của nó có độ sâu 1, v.v. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Với mỗi test, dòng đầu tiên là độ dài hoán vị$n$và dòng thứ hai chứa chính hoán vị đó. Đầu ra là danh sách độ sâu tương ứng với từng phần tử trong hoán vị. 

Cho rằng$n \le 100$và có thể có tới 100 trường hợp thử nghiệm, thuật toán phải xử lý tổng cộng khoảng 10.000 phần tử một cách hiệu quả. Bất kỳ cách tiếp cận nào có độ phức tạp tệ hơn$O(n^2)$mỗi trường hợp thử nghiệm có thể bắt đầu cảm thấy chậm, nhưng đối với vấn đề này, ngay cả một cách tiếp cận đệ quy đơn giản cũng có thể chấp nhận được vì$n$là nhỏ. Các trường hợp cạnh chính bao gồm các hoán vị có độ dài 1, trong đó phần tử duy nhất là gốc và có độ sâu 0, cũng như các hoán vị đã được sắp xếp tăng dần hoặc giảm dần, tạo ra các cây bị lệch. 

Việc triển khai đơn giản có thể thất bại nếu nó không phân chia chính xác mảng xung quanh phần tử tối đa hoặc quên theo dõi độ sâu trong quá trình đệ quy. Ví dụ, đối với một hoán vị`[1,2,3,4]`, tối đa là 4, vì vậy độ sâu của nó là 0, 1, 2, 3 cho các phần tử`[1,2,3,4]`tương ứng nếu chúng ta tái diễn sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng việc xây dựng cây theo đúng nghĩa đen. Đối với mỗi mảng con, chúng tôi tìm mức tối đa, gán độ sâu của nó và gọi đệ quy các mảng con bên trái và bên phải. Điều này hiệu quả vì định nghĩa cây có tính đệ quy và phần tử tối đa luôn xác định gốc. Chi phí chính là quét liên tục các mảng con để tìm mức tối đa. Vì$n=100$, trường hợp xấu nhất là quét các mảng con có độ dài$n, n-1, n-2, \dots, 1$, tổng cộng là khoảng$n^2/2$hoạt động cho mỗi trường hợp thử nghiệm. Điều này có thể chấp nhận được do các giới hạn của vấn đề. 

Cách tiếp cận tối ưu về cơ bản là giống nhau vì kích thước đầu vào nhỏ. Không cần cấu trúc dữ liệu nâng cao. Chúng ta có thể triển khai nó một cách rõ ràng dưới dạng hàm đệ quy lấy một mảng con và độ sâu, tìm phần tử lớn nhất, gán độ sâu của nó và đệ quy trên các mảng con bên trái và bên phải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Đã chấp nhận | 
| DFS đệ quy | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, hãy đọc hoán vị của độ dài$n$. 
2. Khởi tạo một mảng`depths`chiều dài$n$để lưu trữ kết quả. 
3. Xác định hàm đệ quy`assign_depth(l, r, d)`Ở đâu`l`Và`r`là các chỉ số mảng con hiện tại (đã bao gồm) và`d`là độ sâu hiện tại. 
4. Trong`assign_depth`, nếu như`l > r`, trả về ngay vì mảng con trống. 
5. Tìm chỉ số của phần tử lớn nhất trong mảng con`a[l:r+1]`. Giao phó`depths[max_index] = d`. 
6. Gọi đệ quy`assign_depth(l, max_index - 1, d + 1)`để gán độ sâu cho cây con bên trái. 
7. Gọi đệ quy`assign_depth(max_index + 1, r, d + 1)`để gán độ sâu cho cây con bên phải. 
8. Sau khi quá trình đệ quy hoàn tất, hãy in`depths`mảng cho trường hợp thử nghiệm. 

Tại sao nó hoạt động: Mỗi lệnh gọi đệ quy đảm bảo rằng phần tử lớn nhất của mảng con hiện tại trở thành gốc của cây con đó. Bằng cách vượt qua độ sâu hiện tại`d`vào đệ quy, chúng tôi chỉ định chính xác độ sâu cho từng phần tử. Phép đệ quy chia mảng thành các phần con bên trái và bên phải một cách tự nhiên, giữ nguyên cấu trúc cây được xác định trong bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        depths = [0] * n

        def assign_depth(l, r, d):
            if l > r:
                return
            # find max element in current subarray
            max_index = l
            for i in range(l, r + 1):
                if a[i] > a[max_index]:
                    max_index = i
            depths[max_index] = d
            # recurse on left and right subarrays
            assign_depth(l, max_index - 1, d + 1)
            assign_depth(max_index + 1, r, d + 1)

        assign_depth(0, n - 1, 0)
        print(' '.join(map(str, depths)))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó đọc hoán vị và khởi tạo một`depths`mảng. các`assign_depth`hàm xử lý việc gán đệ quy độ sâu. Nó tìm kiếm phần tử lớn nhất trong mảng con hiện tại, gán độ sâu và xử lý đệ quy các phân vùng bên trái và bên phải. Việc sử dụng chỉ mục thay vì cắt mảng sẽ tránh tạo danh sách mới ở mỗi bước đệ quy, tiết kiệm bộ nhớ và đơn giản hóa việc theo dõi độ sâu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[3, 5, 2, 1, 4]`Độ dài hoán vị: 5 

| mảng con | Phần tử tối đa | Độ sâu được chỉ định | Đệ quy trái | Đệ quy đúng | 
| --- | --- | --- | --- | --- | 
|`[3,5,2,1,4]`| 5 ở chỉ số 1 | 0 |`[3]`|`[2,1,4]`| 
|`[3]`| 3 ở chỉ số 0 | 1 |`[]`|`[]`| 
|`[2,1,4]`| 4 ở chỉ số 4 | 1 |`[2,1]`|`[]`| 
|`[2,1]`| 2 ở chỉ số 2 | 2 |`[]`|`[1]`| 
|`[1]`| 1 ở chỉ số 3 | 3 |`[]`|`[]`| 

Độ sâu đầu ra:`[1,0,2,3,1]`### Ví dụ 2 

đầu vào:`[4,3,1,2]`| mảng con | Phần tử tối đa | Độ sâu được chỉ định | Đệ quy trái | Đệ quy đúng | 
| --- | --- | --- | --- | --- | 
|`[4,3,1,2]`| 4 ở chỉ số 0 | 0 |`[]`|`[3,1,2]`| 
|`[3,1,2]`| 3 ở chỉ số 1 | 1 |`[]`|`[1,2]`| 
|`[1,2]`| 2 ở chỉ số 3 | 2 |`[1]`|`[]`| 
|`[1]`| 1 ở chỉ số 2 | 3 |`[]`|`[]`| 

Độ sâu đầu ra:`[0,1,3,2]`Các bảng này cho thấy cách đệ quy gán độ sâu một cách chính xác, xác nhận thuật toán xử lý cả phân vùng lệch trái và lệch phải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi phép đệ quy tìm phần tử lớn nhất bằng cách quét mảng con. Trường hợp xấu nhất là n + (n-1) + ... + 1 = O(n^2) | 
| Không gian | O(n) | Độ sâu đệ quy tối đa là n, cộng với`depths`mảng có kích thước n | 

Với những hạn chế$n \le 100$Và$t \le 100$, độ phức tạp này nằm trong giới hạn. Việc sử dụng bộ nhớ ở mức tối thiểu và độ sâu đệ quy sẽ không vượt quá giới hạn đệ quy mặc định của Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("3\n5\n3 5 2 1 4\n1\n1\n4\n4 3 1 2\n") == "1 0 2 3 1\n0\n0 1 3 2"

# Minimum size
assert run("1\n1\n1\n") == "0"

# Increasing sequence
assert run("1\n5\n1 2 3 4 5\n") == "4 3 2 1 0"

# Decreasing sequence
assert run("1\n5\n5 4 3 2 1\n") == "0 1 2 3 4"

# Random permutation
assert run("1\n6\n2 6 1 4 5 3\n") == "1 0 3 2 1 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1`|`0`| Cây phần tử đơn, độ sâu 0 | 
|`1\n5\n1 2 3 4 5`| `4 3 2 1 | |
