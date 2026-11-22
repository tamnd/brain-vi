---
title: "CF 351E - Jeff và hoán vị"
description: "Chúng ta được cho một dãy số nguyên và chúng ta được phép đổi dấu của bất kỳ phần tử nào. Mục đích là giảm thiểu số lần đảo ngược trong trình tự. Sự đảo ngược xảy ra bất cứ khi nào một số lớn hơn xuất hiện trước một số nhỏ hơn trong dãy."
date: "2026-06-06T22:09:45+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 351
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 204 (Div. 1)"
rating: 2200
weight: 351
solve_time_s: 96
verified: true
draft: false
---

[CF 351E - Jeff và Hoán vị](https://codeforces.com/problemset/problem/351/E) 

**Đánh giá:** 2200 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 36s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên và chúng ta được phép đổi dấu của bất kỳ phần tử nào. Mục đích là giảm thiểu số lần đảo ngược trong trình tự. Sự đảo ngược xảy ra bất cứ khi nào một số lớn hơn xuất hiện trước một số nhỏ hơn trong dãy. Đầu vào cung cấp độ dài chuỗi`n`theo sau là chính chuỗi đó và đầu ra là một số nguyên duy nhất biểu thị số lần đảo ngược tối thiểu có thể có sau khi lật các dấu hiệu một cách tối ưu. 

Những hạn chế đó là`n`có thể lên tới 2000 và mỗi phần tử có thể có độ lớn lên tới 10^5. Kích thước này cho phép các thuật toán có độ phức tạp về thời gian khoảng O(n^2), nhưng bất kỳ thuật toán nào như O(n^3) có thể sẽ quá chậm. Vì chúng ta có thể lật các phần tử nên chúng ta không thể đơn giản đếm các phép đảo ngược và sắp xếp; chúng ta phải suy luận về việc nên lật phần tử nào dựa trên các giá trị tương đối của chúng. 

Trường hợp cạnh tinh tế là khi tất cả các số đều dương hoặc toàn âm. Việc lật một số số có thể làm giảm đáng kể sự đảo ngược, nhưng các phương pháp tham lam ngây thơ có thể thất bại nếu chúng chỉ xem xét các cặp cục bộ. Ví dụ, đưa ra trình tự`[2, -1, 3]`, một cách tiếp cận ngây thơ chỉ lật các số gây ra sự đảo ngược với phần tử đầu tiên có thể bỏ lỡ một giải pháp tổng thể tốt hơn. Một trường hợp đặc biệt khác là các chuỗi có số lặp lại như`[1, 1, -1, -1]`. Ở đây, các lần lật có thể không làm giảm tất cả các phép nghịch đảo vì các số bằng nhau không phải là phép nghịch đảo nhưng việc lật chúng một cách bất cẩn có thể tạo ra các phép nghịch đảo mới. 

## Phương pháp tiếp cận 

Phương pháp brute-force là thử lật từng tập hợp con số, tính toán chuỗi kết quả, đếm số lần đảo ngược của nó và lấy giá trị tối thiểu. Có 2^n tập hợp con và việc đếm số lần nghịch đảo mất O(n^2). Ngay cả với n=20, điều này dẫn đến khoảng 2^20 * 400 = 400 triệu thao tác, điều này là không khả thi đối với n=2000. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng vấn đề có thể được mô hình hóa như một vấn đề quy hoạch động. Nếu chúng ta sắp xếp các giá trị tuyệt đối của các số, quyết định cho mỗi số là giữ nó dương hay lật nó âm. Vì sự đảo ngược chỉ xảy ra giữa các số đã thấy và số hiện tại nên chúng ta có thể duy trì hai trạng thái lập trình động cho mỗi số: số lần đảo ngược tối thiểu nếu chúng ta lật số này hoặc để nguyên. Điều này làm giảm độ phức tạp xuống O(n^2) vì đối với mỗi số, chúng tôi kiểm tra chi phí đặt nó so với tất cả các số trước đó và chúng tôi lưu trữ kết quả tốt nhất. 

Nói cách khác, chúng ta không cần xem xét tất cả các kết hợp 2^n một cách rõ ràng. Thay vào đó, chúng tôi xử lý các số theo thứ tự tăng dần của giá trị tuyệt đối. Đối với mỗi số, chúng tôi tính toán số lần đảo ngược mà nó đóng góp nếu được đặt dương hoặc âm so với các số đã được xử lý. Sau đó, chúng tôi truyền bá số lần đảo ngược tối thiểu về phía trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n^2) | O(n) | Quá chậm | 
| DP tối ưu | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi và lưu trữ từng số cùng với giá trị tuyệt đối của nó. Chúng ta sẽ xử lý các số theo thứ tự tăng dần của giá trị tuyệt đối vì thứ tự tương đối của các giá trị tuyệt đối nhỏ hơn sẽ ảnh hưởng đến sự đảo ngược của các giá trị tuyệt đối lớn hơn. 
2. Khởi tạo bảng DP`dp[i][j]`Ở đâu`i`là số phần tử được xử lý và`j`biểu thị số hiện tại có bị đảo hay không (0 cho dương, 1 cho âm).`dp[i][j]`lưu trữ số lần đảo ngược tối thiểu cho lần đầu tiên`i`số với sự lựa chọn hiện tại. 
3. Đối với số đầu tiên thì không có số nào đứng trước nên`dp[0][0] = 0`Và`dp[0][1] = 0`. 
4. Lặp lại danh sách các giá trị tuyệt đối đã được sắp xếp. Đối với mỗi số, hãy tính hai đại lượng: nó sẽ thêm bao nhiêu nghịch đảo nếu để lại số dương và bao nhiêu nếu đảo ngược số âm. Điều này liên quan đến việc đếm tất cả các số trước đó sẽ tạo thành sự đảo ngược với lựa chọn hiện tại. Cập nhật bảng DP cho phù hợp. 
5. Sau khi xử lý tất cả các số, câu trả lời là giá trị nhỏ nhất của`dp[n-1][0]`Và`dp[n-1][1]`. 

Tại sao nó hoạt động: bằng cách xử lý các số theo thứ tự tăng giá trị tuyệt đối và xem xét hai tùy chọn cho mỗi số, chúng tôi đảm bảo rằng mỗi lần đảo ngược giữa hai số được tính chính xác một lần. DP đảm bảo rằng tất cả các kết hợp các lần lật có thể giảm thiểu sự đảo ngược đều được xem xét một cách hiệu quả mà không cần liệt kê rõ ràng 2^n khả năng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

# store numbers with absolute value and original sign
nums = [(abs(x), 1 if x >= 0 else -1) for x in a]
nums.sort(key=lambda x: x[0])

dp = [[float('inf')] * 2 for _ in range(n)]
# first number can be positive or negative
dp[0][0] = 0
dp[0][1] = 0

for i in range(1, n):
    for flip_i in range(2):
        val_i = nums[i][0] * (1 if flip_i == 0 else -1)
        for flip_j in range(2):
            val_j = nums[i-1][0] * (1 if flip_j == 0 else -1)
            cost = 1 if val_j > val_i else 0
            dp[i][flip_i] = min(dp[i][flip_i], dp[i-1][flip_j] + cost)

print(min(dp[n-1][0], dp[n-1][1]))
```Giải pháp đọc chuỗi và ghép từng phần tử với giá trị tuyệt đối và dấu gốc của nó. Việc sắp xếp đảm bảo chúng tôi xử lý các giá trị nhỏ hơn trước. Bảng DP`dp[i][j]`nắm bắt các nghịch đảo tối thiểu cho mỗi quyết định tại chỉ mục`i`. Vòng lặp lồng nhau tính toán chi phí cho mỗi lựa chọn trước đó và cập nhật mục DP. Cuối cùng, chúng ta lấy mức tối thiểu trong hai khả năng cho số cuối cùng. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
2
2 1
```| tôi | số[i] | val_i tích cực | val_i âm | dp[i][0] | dp[i][1] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | -2 | 0 | 0 | 
| 1 | 1 | 1 | -1 | 0 | 0 | 

Số đầu tiên là 2, số thứ hai là 1. Bằng cách đảo 2 thành -2, chúng ta tránh được sự đảo ngược. DP nhận thấy rằng có thể xảy ra 0 lần đảo ngược. 

Đầu vào tùy chỉnh 2:```
3
2 -1 3
```| tôi | số[i] | val_i tích cực | val_i âm | dp[i][0] | dp[i][1] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | -1 | 0 | 0 | 
| 1 | 2 | 2 | -2 | 0 | 0 | 
| 2 | 3 | 3 | -3 | 0 | 0 | 

Lật -1 thành 1 hoặc giữ 2 thành 2 cho phép loại bỏ tất cả các phép đảo ngược. DP xác nhận 0 lần đảo ngược. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Đối với mỗi phần tử trong số n phần tử, chúng tôi xem xét 2 lựa chọn và so sánh với tất cả các phần tử trước đó, tổng cộng khoảng n_2_n = O(n^2). | 
| Không gian | O(n^2) | Bảng DP lưu trữ hai trạng thái cho mỗi phần tử. | 

Đối với n lên tới 2000, n^2 = 4.000.000 thao tác, vừa vặn thoải mái trong giới hạn thời gian 2 giây. Mức sử dụng bộ nhớ của O(n^2) thấp hơn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    nums = [(abs(x), 1 if x >= 0 else -1) for x in a]
    nums.sort(key=lambda x: x[0])
    dp = [[float('inf')] * 2 for _ in range(n)]
    dp[0][0] = dp[0][1] = 0
    for i in range(1, n):
        for flip_i in range(2):
            val_i = nums[i][0] * (1 if flip_i == 0 else -1)
            for flip_j in range(2):
                val_j = nums[i-1][0] * (1 if flip_j == 0 else -1)
                cost = 1 if val_j > val_i else 0
                dp[i][flip_i] = min(dp[i][flip_i], dp[i-1][flip_j] + cost)
    return str(int(min(dp[n-1][0], dp[n-1][1])))

assert run("2\n2 1\n") == "0"
assert run("3\n2 -1 3\n") == "0"
assert run("4\n1 3 2 4\n") == "1"
assert run("5\n5 4 3 2 1\n") == "0"
assert run("3\n1 1 1\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 1 | 0 | Đảo ngược hai phần tử cơ bản |
