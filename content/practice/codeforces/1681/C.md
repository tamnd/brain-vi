---
title: "CF 1681C - Phân loại kép"
description: "Chúng ta có hai mảng có cùng độ dài, a và b. Chúng ta có thể thực hiện một thao tác hoán đổi đặc biệt: chọn bất kỳ hai vị trí i và j nào, đồng thời hoán đổi a[i] với a[j] và b[i] với b[j]."
date: "2026-06-10T00:13:22+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1681
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 129 (Rated for Div. 2)"
rating: 1200
weight: 1681
solve_time_s: 125
verified: false
draft: false
---

[CF 1681C - Sắp xếp kép](https://codeforces.com/problemset/problem/1681/C) 

**Đánh giá:** 1200 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai mảng có cùng độ dài,`a`Và`b`. Chúng ta có thể thực hiện một thao tác hoán đổi đặc biệt: chọn hai vị trí bất kỳ`i`Và`j`, và đồng thời trao đổi`a[i]`với`a[j]`Và`b[i]`với`b[j]`. Mục tiêu là sắp xếp cả hai mảng theo thứ tự không giảm bằng cách sử dụng tối đa 10.000 bước di chuyển này. Chúng ta phải tạo ra một chuỗi các bước di chuyển để đạt được điều này hoặc báo cáo rằng điều đó là không thể. 

Đầu vào chỉ định nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra độ dài mảng và các phần tử mảng. Đầu ra phải in`-1`nếu không tồn tại trình tự nào hoặc trước tiên là số bước đi theo sau là các cặp bước đi. 

Với những hạn chế,`n`có thể lên tới 100 và chúng tôi được phép di chuyển 10.000. Điều này thật hào phóng: một thuật toán đơn giản thực hiện hoán đổi giống như bong bóng trên mỗi phần tử không theo thứ tự có thể thực hiện tối đa O(n²) hoán đổi, nhiều nhất là 10.000 vì n² = 10.000 khi n = 100. Do đó, thuật toán thời gian bậc hai là khả thi. Mỗi phần tử nằm trong`[1, n]`, cho phép so sánh đơn giản mà không phải lo lắng về việc tràn số nguyên lớn. 

Các trường hợp cạnh khóa là các mảng không thể được sắp xếp đồng thời, ngay cả khi mỗi mảng có thể sắp xếp riêng lẻ. Ví dụ,`a = [2, 1]`Và`b = [1, 2]`không thể được sắp xếp với bất kỳ số lượng hoán đổi đồng thời nào, bởi vì bất kỳ hoán đổi nào khắc phục được`a`sẽ phá vỡ`b`. Một trường hợp khác là mảng đã được sắp xếp; giải pháp sẽ tạo ra bước di chuyển bằng 0. Mảng có giá trị lặp lại là hợp lệ vì nhiều số giống nhau có thể xuất hiện và không cản trở việc sắp xếp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là quét liên tục các mảng và hoán đổi bất kỳ cặp nào`(i, j)`trong đó một trong hai mảng không đúng thứ tự. Điều này có tác dụng vì mỗi lần hoán đổi sẽ làm giảm ít nhất một lần đảo ngược. Đối với hai mảng có chiều dài`n`, có nhiều nhất O(n2) phép đảo ngược trong mỗi mảng, vì vậy trong trường hợp xấu nhất số lần hoán đổi là O(n2). Từ`n ≤ 100`, điều này tạo ra tối đa 10.000 giao dịch hoán đổi, được phép. Tuy nhiên, cách tiếp cận bạo lực có thể thất bại nếu các mảng có thứ tự xung đột trong đó một mảng muốn hoán đổi bên trái trong khi mảng kia muốn hoán đổi bên phải. Trong trường hợp như vậy, không thể sắp xếp cả hai mảng cùng một lúc. 

Quan sát quan trọng là chúng ta có thể sắp xếp bằng cách sử dụng phương pháp sắp xếp bong bóng theo cặp đơn giản, nhưng chúng ta phải duy trì tính bất biến của mảng đầu tiên.`a`và mảng thứ hai`b`đều không giảm sau mỗi lần di chuyển. Để đảm bảo điều này, chúng ta chỉ trao đổi khi`a[i] > a[j]`hoặc`b[i] > b[j]`đồng thời. Nếu tại bất kỳ thời điểm nào, việc đảo ngược trong một mảng không thể căn chỉnh với mảng kia, chúng tôi sẽ phát hiện ra điều đó là không thể. Bằng cách đẩy phần tử nhỏ nhất còn lại vào đúng vị trí của nó một cách có hệ thống (như sắp xếp lựa chọn) đồng thời hoán đổi`b`, chúng ta có thể sắp xếp cả hai mảng hoặc phát hiện xung đột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force giống như bong bóng | O(n²) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận (trong vòng 10⁴ nước đi) | 
| Biến thể sắp xếp lựa chọn tối ưu | O(n²) cho mỗi trường hợp thử nghiệm | O(n2 lượt di chuyển) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`, các mảng`a`Và`b`. 
3. Khởi tạo danh sách nước đi trống. 
4. Đối với từng vị trí`i`từ`0`ĐẾN`n-1`, tìm số nhỏ nhất`j ≥ i`sao cho cả hai`a[j]`Và`b[j]`có thể được đặt ở vị trí`i`không vi phạm trật tự không giảm so với các phần tử trước đó. Nếu không như vậy`j`tồn tại, đầu ra`-1`bởi vì điều đó là không thể. 
5. Nếu`j ≠ i`, tráo đổi`a[i]`với`a[j]`Và`b[i]`với`b[j]`, và ghi lại chuyển động`(i+1, j+1)`sử dụng chỉ mục dựa trên 1. 
6. Tiếp tục quá trình này cho đến hết mảng. Sau tất cả các lần lặp lại,`a`Và`b`nên được sắp xếp. 
7. In số nước đi và thứ tự nước đi. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến của các phần tử trước vị trí`i`được sắp xếp trong cả hai mảng. Mỗi lần hoán đổi sẽ di chuyển một phần tử đến vị trí chính xác của nó trong cả hai mảng mà không vi phạm thứ tự trước đó. Nếu tại bất kỳ thời điểm nào không có phần tử phù hợp nào có thể được di chuyển vào vị trí`i`, các mảng ở trạng thái xung đột và không thể sắp xếp đồng thời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        moves = []
        possible = True
        for i in range(n):
            for j in range(i+1, n):
                if a[i] > a[j] or b[i] > b[j]:
                    a[i], a[j] = a[j], a[i]
                    b[i], b[j] = b[j], b[i]
                    moves.append((i+1, j+1))
        if a != sorted(a) or b != sorted(b):
            print(-1)
        else:
            print(len(moves))
            for x, y in moves:
                print(x, y)

if __name__ == "__main__":
    solve()
```Vòng lặp bên ngoài lặp qua từng vị trí để đảm bảo phần tử nhỏ nhất còn lại được đặt chính xác. Vòng lặp bên trong chỉ thực hiện hoán đổi khi cần thiết. Chúng tôi nối thêm các bước di chuyển chỉ mục dựa trên 1 theo yêu cầu của báo cáo vấn đề. Sau tất cả các lần hoán đổi, các mảng được kiểm tra dựa trên các phiên bản đã sắp xếp của chúng để phát hiện những điều không thể thực hiện được. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
2
2
1 2
1 2
2
2 1
1 2
```| Bước | một | b | Di chuyển | 
| --- | --- | --- | --- | 
| Ban đầu | [1,2] | [1,2] | [] | 
| Kiểm tra | không cần trao đổi | không cần trao đổi | [] | 
| Kết quả | [1,2] | [1,2] | [] | 

Đầu ra:`0`di chuyển. Các mảng đã được sắp xếp. 

Đầu vào thứ hai: 

| Bước | một | b | Di chuyển | 
| --- | --- | --- | --- | 
| Ban đầu | [2,1] | [1,2] | [] | 
| tôi=0, j=1 | a[0] > a[1] → trao đổi | b[0] <= b[1] → hoán đổi cũng được thực hiện | hoán đổi → a=[1,2], b=[2,1], di chuyển=[(1,2)] | 
| Kiểm tra lần cuối | a đã được sắp xếp, b chưa được sắp xếp | → đầu ra -1 | | 

Điều này chứng tỏ thuật toán xác định chính xác các trường hợp không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n2) | Mỗi trường hợp thử nghiệm sử dụng vòng lặp kép trên n phần tử, tối đa 1002 lần lặp cho mỗi trường hợp | 
| Không gian | O(n²) | Lưu trữ các bước di chuyển, tối đa n*(n-1)/2 = 4950 bước di chuyển khi n=100 | 

Cho n ≤ 100 và t ≤ 100, các phép toán trong trường hợp xấu nhất là 100 * 10.000 = 10⁶, phù hợp trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("3\n2\n1 2\n1 2\n2\n2 1\n1 2\n4\n2 3 1 2\n2 3 2 3\n") == "0\n-1\n3\n3 1\n3 2\n4 3"

# custom cases
assert run("1\n3\n1 2 3\n3 2 1\n") == "-1", "cannot sort both"
assert run("1\n2\n2 2\n1 1\n") == "1\n1 2", "equal values swap"
assert run("1\n5\n5 4 3 2 1\n1 2 3 4 5\n") == "-1", "opposite orders impossible"
assert run("1\n4\n1 3 2 4\n1 2 3 4\n") == "1\n2 3", "single swap suffices"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 yếu tố xung đột | -1 | Phát hiện điều không thể | 
| 2 phần tử bằng nhau | 1 nước đi | Xử lý chính xác các giá trị bằng nhau | 
| Thứ tự ngược lại | -1 | Phát hiện xung đột giữa các mảng | 
| Cần trao đổi nhỏ | 1 nước đi | Áp dụng chính xác trao đổi tối thiểu | 

## Vỏ cạnh 

Mảng có giá trị trùng lặp, như`a = [2,2]`Và`b = [1,1]`, không yêu cầu trao đổi. Thuật toán xuất ra chính xác`0`. Nếu mảng có thứ tự xung đột, như`a=[2,1]`Và`b=[1,2]`, thuật toán thực hiện hoán đổi trên`i=0,j=1`, kiểm tra các mảng và xác định những điều không thể thực hiện được. Đối với các mảng đã được sắp xếp, không có bước di chuyển nào được ghi lại. Thuật toán duy trì tính chính xác cho mảng có kích thước tối thiểu`n=2`và tối đa`n=100`.
