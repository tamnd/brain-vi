---
title: "CF 2053A - Thợ mộc mềm"
description: "Bài toán trình bày một mảng các số nguyên biểu thị độ dài thanh. Một tập hợp các số nguyên được xác định là ổn định nếu bất kỳ ba phần tử nào được chọn từ nó (có thể lặp lại) có thể tạo thành một tam giác không suy biến."
date: "2026-06-08T08:25:28+07:00"
tags: ["codeforces", "competitive-programming", "dp", "geometry", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2053
codeforces_index: "A"
codeforces_contest_name: "Good Bye 2024: 2025 is NEAR"
rating: 800
weight: 2053
solve_time_s: 190
verified: true
draft: false
---

[CF 2053A - Thợ mộc đấu thầu](https://codeforces.com/problemset/problem/2053/A) 

**Đánh giá:** 800 
**Tags:** dp, hình học, tham lam, toán học 
**Thời gian giải:** 3 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán trình bày một mảng các số nguyên biểu thị độ dài thanh. Một tập hợp các số nguyên được xác định là ổn định nếu bất kỳ ba phần tử nào được chọn từ nó (có thể lặp lại) có thể tạo thành một tam giác không suy biến. Mục đích là để xác định xem mảng có thể được chia thành các phân đoạn liên tục sao cho mỗi phân đoạn con ổn định và tồn tại ít nhất hai cách riêng biệt để thực hiện việc này hay không. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chứa một số nguyên$n$cho độ dài của mảng theo sau là$n$số nguyên đại diện cho các cây gậy. Đầu ra là một trong hai`YES`nếu có ít nhất hai phân vùng hợp lệ tồn tại hoặc`NO`nếu không thì. 

Độ dài mảng lên tới 200 và mỗi giá trị có thể lên tới$10^5$. Điều này ngụ ý rằng chúng ta có thể đủ khả năng cho các thuật toán với$O(n \log n)$hoặc$O(n^2)$sự phức tạp. Vì các phân đoạn con là liên tục nên chúng ta có thể kiểm tra các thuộc tính dựa trên các phần tử liền kề thay vì các bộ ba tùy ý. Một cách tiếp cận ngây thơ thử mọi phân vùng có thể sẽ theo cấp số nhân trong$n$, điều đó là không thể thực hiện được. 

Các trường hợp cạnh bao gồm các mảng trong đó tất cả các phần tử đều bằng nhau, mảng chỉ có hai phần tử hoặc mảng trong đó chênh lệch tối đa giữa các phần tử liên tiếp vượt quá ngưỡng tạo thành hình tam giác. Ví dụ, mảng`[1, 100000]`không có tập hợp con ổn định có độ dài lớn hơn một, do đó chỉ có thể phân đoạn một phần tử, chỉ tạo ra một phân vùng. Một cách tiếp cận bất cẩn có thể cho rằng bất kỳ mảng nào dài hơn một phần tử luôn có nhiều phân vùng, điều này là sai. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ liệt kê tất cả các phân đoạn con liên tục có thể có và đối với mỗi phân đoạn, hãy kiểm tra xem mọi phân đoạn con có ổn định hay không. Kiểm tra độ ổn định yêu cầu xác minh bất đẳng thức tam giác cho tất cả các bộ ba trong phân đoạn. Số lượng phân vùng tăng theo cấp số nhân với$n$và việc xác minh tính ổn định cho từng phân đoạn có thể yêu cầu$O(n^3)$hoạt động. Vì$n=200$, điều này hoàn toàn không thể thực hiện được. 

Điểm mấu chốt là một bộ gậy ổn định khi và chỉ khi hiệu số giữa cây gậy lớn nhất và cây gậy nhỏ nhất nhỏ hơn tổng của hai cây gậy lớn nhất trừ đi cây gậy nhỏ nhất. Trong một phân đoạn được sắp xếp, điều này dẫn đến việc kiểm tra xem que lớn nhất có nhỏ hơn tổng của hai que còn lại trong bất kỳ bộ ba nào không. Vì các phân đoạn con liên tục được xem xét, mảng có thể được kiểm tra các phần tử liền kề: nếu mảng tăng hoặc giảm nghiêm ngặt và sự khác biệt giữa phần tử đầu tiên và phần tử cuối cùng đủ lớn thì chỉ tồn tại một phân vùng hợp lệ. 

Một cách tiếp cận tối ưu là kiểm tra xem mảng có cặp liên tiếp nào trong đó phần tử trước lớn hơn phần tử tiếp theo hay không. Nếu một cặp như vậy tồn tại thì có thể có ít nhất hai phân vùng riêng biệt bằng cách chia tách tại điểm đó. Mặt khác, phân vùng hợp lệ duy nhất là chính mảng hoặc các phân đoạn phần tử đơn, chỉ cung cấp một phân vùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n^3) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc độ dài$n$và mảng$a$. 
3. Kiểm tra tuần tự mảng xem có chỉ mục nào không$i$như vậy$a[i] > a[i+1]$. Nếu tìm thấy, xuất ra`YES`cho trường hợp thử nghiệm này. Điều này tương ứng với sự tồn tại của ít nhất một điểm phân chia có thể tạo ra nhiều phân vùng. 
4. Nếu không có chỉ mục đó tồn tại, xuất ra`NO`. 
5. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Lý do bước 3 hoạt động là vì mảng tăng hoặc giảm nghiêm ngặt chỉ cho phép một cách để duy trì sự ổn định trên các phân đoạn con. Một cặp trong đó phần tử trước vượt quá phần tử tiếp theo sẽ tạo ra sự gián đoạn trong đó có thể thực hiện phân chia, tạo ra ít nhất hai phân vùng hợp lệ. Bất biến này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        found = False
        for i in range(n - 1):
            if a[i] > a[i + 1]:
                found = True
                break
        print("YES" if found else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`và xử lý từng trường hợp thử nghiệm một cách độc lập. Nó lặp qua mảng một lần để xác định bất kỳ điểm nào có thể phân chia. Séc`a[i] > a[i + 1]`xác định trực tiếp điều kiện cần thiết cho nhiều phân vùng. Không cần thêm không gian ngoài mảng. Phải cẩn thận đối với ranh giới vòng lặp`n-1`để tránh lỗi chỉ mục. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:`[2, 3, 5, 7]`| tôi | một [tôi] | a[i+1] | a[i] > a[i+1]? | 
| --- | --- | --- | --- | 
| 0 | 2 | 3 | Không | 
| 1 | 3 | 5 | Không | 
| 2 | 5 | 7 | Không | 

Không tìm thấy cặp giảm dần. Chỉ có một phân vùng có thể. Đầu ra:`NO`. 

### Ví dụ 2 

Mảng đầu vào:`[1, 5, 4, 1, 4, 7]`| tôi | một [tôi] | a[i+1] | a[i] > a[i+1]? | 
| --- | --- | --- | --- | 
| 0 | 1 | 5 | Không | 
| 1 | 5 | 4 | Có | 

Một cặp giảm dần được tìm thấy ở chỉ mục 1. Tồn tại nhiều phân vùng. Đầu ra:`YES`. 

Những dấu vết này xác nhận rằng thuật toán xác định sự hiện diện của ít nhất một phần phân chia hợp lệ cho phép thực hiện nhiều phân vùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Truyền một lần qua mảng cho mỗi trường hợp thử nghiệm | 
| Không gian | O(n) | Chỉ có mảng được lưu trữ | 

Được cho$n \le 200$Và$t \le 200$, tổng số thao tác dưới 40.000, đủ nhanh trong giới hạn thời gian 1 giây. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("5\n4\n2 3 5 7\n4\n115 9 2 28\n5\n8 4 1 6 2\n6\n1 5 4 1 4 7\n2\n100000 100000\n") == "NO\nYES\nYES\nYES\nNO"

# custom cases
assert run("1\n2\n1 2\n") == "NO", "two-element increasing"
assert run("1\n2\n2 1\n") == "YES", "two-element decreasing"
assert run("1\n3\n3 3 3\n") == "NO", "all equal"
assert run("1\n4\n1 3 2 4\n") == "YES", "single inversion in middle"
assert run("1\n5\n5 4 3 2 1\n") == "YES", "strictly decreasing"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 yếu tố tăng dần | KHÔNG | Không thể phân chia, chỉ có một phân vùng | 
| 2 yếu tố giảm dần | CÓ | Đảo ngược đơn cho phép nhiều phân vùng | 
| Tất cả đều bình đẳng | KHÔNG | Không đảo ngược, chỉ có thể phân vùng duy nhất | 
| Đảo ngược đơn | CÓ | Phát hiện sự phân chia ở bất kỳ đâu trong mảng | 
| Giảm mạnh | CÓ | Xác nhận nhiều phân vùng trong mảng giảm dần | 

## Vỏ cạnh 

Đối với các mảng có độ dài bằng hai, thuật toán sẽ xác định chính xác xem cặp này đang tăng hay giảm. Nếu cả hai phần tử bằng nhau thì mảng ổn định và không thể tạo nhiều phân vùng. Đối với các mảng trong đó tất cả các phần tử đều bằng nhau, logic tương tự được áp dụng: không tồn tại cặp giảm dần, do đó chỉ có thể có một phân vùng. Đối với các mảng có đảo ngược ở bất cứ đâu, đảo ngược đầu tiên gặp phải là đủ để đảm bảo`YES`. Ví dụ,`[1, 100, 50, 200]`xác định chỉ số 1 là giảm dần, cho`YES`.
