---
title: "CF 252A - Nhỏ Xor"
description: "Chúng tôi được cung cấp một chuỗi các số nguyên không âm và chúng tôi muốn chọn một khối phần tử liền kề sao cho khi chúng tôi lấy XOR theo bit của mọi thứ bên trong khối đó, kết quả càng lớn càng tốt."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 252
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 153 (Div. 2)"
rating: 1100
weight: 252
solve_time_s: 81
verified: false
draft: false
---

[CF 252A - Little Xor](https://codeforces.com/problemset/problem/252/A) 

**Đánh giá:** 1100 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi các số nguyên không âm và chúng tôi muốn chọn một khối phần tử liền kề sao cho khi chúng tôi lấy XOR theo bit của mọi thứ bên trong khối đó, kết quả càng lớn càng tốt. Nhiệm vụ là trả về XOR tối đa có thể đạt được trên tất cả các mảng con có thể có. 

Kích thước đầu vào nhỏ, có tối đa 100 phần tử và mỗi giá trị vừa với 30 bit. Điều này ngay lập tức cho chúng ta biết rằng ngay cả các giải pháp bậc ba hoặc bậc hai cũng có thể chấp nhận được, bởi vì tổng số mảng con bị giới hạn bởi khoảng 5.000 và mỗi phép tính XOR có thể được thực hiện trong thời gian không đổi nếu chúng ta xử lý trước các XOR tiền tố. 

Cấu trúc của XOR trên các phân đoạn có một đặc tính tinh vi thường gây ra lỗi: XOR không đơn điệu, do đó việc mở rộng một phân đoạn có thể làm tăng hoặc giảm kết quả một cách khó lường. Điều này loại trừ các chiến lược tham lam cố gắng mở rộng một cơ hội tốt nhất. 

Một nỗ lực sai lầm phổ biến là duy trì một cửa sổ đang chạy và điều chỉnh các điểm cuối một cách tham lam. Ví dụ: trong một mảng như`[1, 2, 3, 0]`, một phần mở rộng tham lam có thể chọn`[1, 2] = 3`, sau đó thêm`3`cho`0`, trông tệ hơn và có thể bị loại bỏ sớm. Nhưng những đoạn sau như`[2, 3] = 1`hoặc`[1, 2, 3] = 0`cho thấy các quyết định cục bộ không phản ánh sự tối ưu toàn cầu. 

Một cạm bẫy khác là tính toán lại XOR của từng phân đoạn từ đầu trong thời gian O(n), dẫn đến hành vi O(n^3). Mặc dù về mặt kỹ thuật vẫn ổn với n = 100, nhưng nó không cần thiết và che giấu cấu trúc đơn giản hơn. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử từng cặp điểm cuối (l, r), tính XOR của phân đoạn đó và theo dõi mức tối đa. Có các phân đoạn O(n^2) và mỗi phép tính XOR có thể được thực hiện trong O(n) nếu được thực hiện một cách ngây thơ, sẽ cho ra O(n^3). Điều này hiệu quả vì n nhỏ nhưng nó lặp lại nhiều lần. 

Quan sát quan trọng là XOR trên một phân đoạn có thể được tính theo O(1) bằng cách sử dụng XOR tiền tố. Nếu chúng ta định nghĩa tiền tố[i] là XOR của các phần tử từ chỉ số 0 đến i, thì XOR của phân đoạn [l, r] chỉ đơn giản là tiền tố[r] tiền tố XOR[l - 1]. Điều này làm giảm chi phí tính toán bên trong và loại bỏ sự dư thừa. 

Khi điều này có sẵn, chúng tôi có thể đánh giá tất cả các mảng con trong O(1) mỗi mảng, làm cho toàn bộ quá trình quét trở thành O(n^2). Với n ≤ 100, điều này là quá đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tính toán lại từng đoạn) | O(n^3) | O(1) | Được chấp nhận nhưng không hiệu quả | 
| Tiền tố XOR + liệt kê | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng mảng XOR tiền tố trong đó mỗi vị trí lưu trữ XOR của tất cả các phần tử cho đến chỉ mục đó. Điều này cho phép truy vấn phân đoạn theo thời gian không đổi. 
2. Lặp lại tất cả các điểm cuối bên trái có thể có của một đoạn. Mỗi chỉ mục trở thành một khởi đầu ứng cử viên của một mảng con. 
3. Đối với mỗi chỉ mục bắt đầu, hãy lặp qua tất cả các điểm cuối bên phải có thể có từ điểm bắt đầu đó đến điểm cuối của mảng. Điều này liệt kê mọi phân đoạn liền kề chính xác một lần. 
4. Với mỗi cặp (l, r), hãy tính XOR của phân đoạn bằng các giá trị tiền tố. Điều này tránh việc tính toán lại XOR từ đầu và giữ mỗi đánh giá O(1). 
5. Duy trì một biến theo dõi XOR tối đa được thấy cho đến nay và cập nhật nó sau khi đánh giá từng phân đoạn. 

### Tại sao nó hoạt động 

Thuật toán đánh giá chính xác mọi phân đoạn liền kề có thể có một lần và tính toán XOR của nó một cách chính xác bằng cách sử dụng nhận dạng mà tiền tố XOR hủy bỏ các tiền tố được chia sẻ. Vì XOR có tính kết hợp và tự đảo ngược nên việc hủy tiền tố đảm bảo tính chính xác của việc trích xuất phân đoạn. Bởi vì tất cả các phân đoạn đều được kiểm tra nên mức tối đa được tìm thấy phải là mức tối đa toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    prefix = [0] * n
    prefix[0] = a[0]
    for i in range(1, n):
        prefix[i] = prefix[i - 1] ^ a[i]

    ans = 0

    for l in range(n):
        for r in range(l, n):
            if l == 0:
                cur = prefix[r]
            else:
                cur = prefix[r] ^ prefix[l - 1]
            if cur > ans:
                ans = cur

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng các giá trị XOR tiền tố để bất kỳ truy vấn phân đoạn nào cũng trở thành hoạt động liên tục. Các vòng lặp lồng nhau sau đó liệt kê tất cả các phân đoạn có thể. Nhánh có điều kiện bên trong xử lý trường hợp biên trong đó đoạn bắt đầu ở chỉ số 0, vì không có tiền tố để trừ. 

Sự so sánh`cur > ans`là an toàn vì tất cả các giá trị đều là số nguyên không âm, vì vậy việc khởi tạo`ans = 0`nắm bắt chính xác câu trả lời tối thiểu có thể. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 1 1 2
```Chúng tôi tính toán tiền tố XOR: 

| tôi | một [tôi] | tiền tố[i] | 
| --- | --- | --- | 
| 0 | 1 | 1 | 
| 1 | 2 | 3 | 
| 2 | 1 | 2 | 
| 3 | 1 | 3 | 
| 4 | 2 | 1 | 

Bây giờ chúng tôi đánh giá các phân đoạn: 

| tôi | r | XOR | 
| --- | --- | --- | 
| 0 | 0 | 1 | 
| 0 | 1 | 3 | 
| 1 | 2 | 3 | 
| 2 | 4 | 2 | 

Giá trị quan sát tối đa là 3, phù hợp với kết quả mong đợi. 

Dấu vết này cho thấy các phân đoạn khác nhau có thể tạo ra cùng một giá trị tối ưu như thế nào và tại sao cần phải kiểm tra toàn diện. 

### Ví dụ 2 

đầu vào:```
1
7
```| tôi | r | XOR | 
| --- | --- | --- | 
| 0 | 0 | 7 | 

Phân đoạn duy nhất có thể là chính mảng đó, vì vậy câu trả lời là 7. Điều này xác nhận rằng thuật toán xử lý chính xác các kích thước đầu vào tối thiểu mà không cần viết hoa đặc biệt ngoài việc khởi tạo tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | hai vòng lặp lồng nhau trên tất cả các mảng con, mỗi XOR được tính trong O(1) | 
| Không gian | O(n) | mảng XOR tiền tố | 

Với n ≤ 100, thuật toán thực hiện tối đa 10.000 đánh giá phân đoạn, mỗi lần đánh giá không đổi. Điều này là thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))

    prefix = [0] * n
    prefix[0] = a[0]
    for i in range(1, n):
        prefix[i] = prefix[i - 1] ^ a[i]

    ans = 0
    for l in range(n):
        for r in range(l, n):
            cur = prefix[r] if l == 0 else (prefix[r] ^ prefix[l - 1])
            ans = max(ans, cur)
    return str(ans)

# provided sample
assert run("5\n1 2 1 1 2\n") == "3"

# custom cases
assert run("1\n0\n") == "0", "single zero"
assert run("2\n5 5\n") == "5", "equal elements XOR cancels"
assert run("3\n1 2 4\n") == "7", "full range best"
assert run("4\n8 1 2 3\n") == "11", "mixed bits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`| 0 | kích thước tối thiểu, không xử lý | 
|`2 5 5`| 5 | Hành vi hủy XOR | 
|`1 2 4`| 7 | tối ưu toàn bộ phân khúc | 
|`8 1 2 3`| 11 | cấu trúc hỗn hợp không tầm thường | 

## Vỏ cạnh 

Đối với mảng một phần tử như`[0]`, mảng tiền tố là`[0]`và phân đoạn duy nhất được đánh giá là`(0, 0)`, tạo ra XOR`0`. Thuật toán trả về chính xác`0`mà không cần xử lý đặc biệt. 

Đối với các yếu tố giống hệt nhau lặp đi lặp lại như`[5, 5]`, giá trị tiền tố trở thành`[5, 0]`. Phân khúc`[0, 1]`sản lượng`5 XOR 5 = 0`, trong khi các phân đoạn một phần tử mang lại`5`. Thuật toán xác định chính xác rằng lựa chọn tốt nhất là bất kỳ phần tử đơn lẻ nào. 

Đối với các mẫu xen kẽ như`[1, 2, 3]`, giá trị tiền tố là`[1, 3, 0]`. Phân khúc`[0, 2]`sản lượng`0`, trong khi`[0, 1]`sản lượng`3`Và`[1, 2]`sản lượng`1`. Thuật toán khám phá chính xác tất cả các ứng viên và chọn`3`, cho thấy các đoạn tối ưu không nhất thiết phải là những đoạn dài nhất.
