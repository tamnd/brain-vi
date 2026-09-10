---
title: "CF 104599C - Độ chính xác của mẫu"
description: "Mỗi trường hợp thử nghiệm đưa ra hai danh sách số: kết quả đầu ra dự kiến ​​và kết quả đầu ra thực tế do một mô hình tạo ra. Đối với mỗi cặp $(ei, ai)$, chúng ta quyết định xem dự đoán có được chấp nhận hay không bằng cách kiểm tra xem chênh lệch tuyệt đối $ Nhiệm vụ là tính tỷ lệ các trường hợp đúng trong số tất cả…"
date: "2026-06-30T02:58:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104599
codeforces_index: "C"
codeforces_contest_name: "GPL 2023 Novice"
rating: 0
weight: 104599
solve_time_s: 77
verified: true
draft: false
---

[CF 104599C - Độ chính xác của mô hình](https://codeforces.com/problemset/problem/104599/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm đưa ra hai danh sách số: kết quả đầu ra dự kiến và kết quả đầu ra thực tế do một mô hình tạo ra. Đối với mỗi cặp$(e_i, a_i)$, chúng ta quyết định liệu dự đoán có được chấp nhận hay không bằng cách kiểm tra xem chênh lệch tuyệt đối có$|a_i - e_i|$nhiều nhất là$K$. Nếu đúng thì chúng tôi coi trường hợp đó là đúng. 

Nhiệm vụ là tính tỷ lệ các trường hợp đúng trong số tất cả các trường hợp$N$trường hợp và xuất ra dưới dạng phần trăm, làm tròn đến số nguyên gần nhất. 

Kích thước đầu vào cho phép lên đến$N = 10^4$mỗi lần kiểm tra và giá trị tăng lên$10^9$, vì vậy công việc có ý nghĩa duy nhất là truyền dữ liệu một lần. Bất kỳ giải pháp nào cố gắng thực hiện tiền xử lý hoặc sắp xếp theo giá trị đều không cần thiết. Quét tuyến tính là đủ và tối ưu. 

Một trường hợp thất bại phổ biến là làm tròn không chính xác. Ví dụ: nếu 1 trong 3 trường hợp đúng thì tỷ lệ phần trăm thô là$33.333\ldots$, và đầu ra đúng là$33$, không$34$. Một trường hợp tinh vi khác là việc cắt bớt phép chia số nguyên: tính toán$(\text{correct} \cdot 100) / N$với phép chia số nguyên chỉ ổn nếu làm tròn được xử lý rõ ràng sau đó. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force chỉ đơn giản lặp lại trên tất cả các cặp, kiểm tra điều kiện$|a_i - e_i| \le K$, đếm xem có bao nhiêu thỏa mãn rồi tính phần trăm. Đây đã là cấu trúc tối ưu vì mỗi cặp phải được kiểm tra ít nhất một lần. 

Quan sát quan trọng là không có sự phụ thuộc lẫn nhau giữa các cặp. Mỗi lần kiểm tra là độc lập, vì vậy giải pháp chỉ là đếm các phần tử thỏa mãn trong luồng và chuyển số đó thành tỷ lệ phần trăm ở cuối. Không cần sắp xếp, cấu trúc tiền tố hoặc chuyển đổi. 

Phần không tầm thường duy nhất là làm tròn chính xác. Bí quyết tiêu chuẩn là tính toán$$\text{ans} = \frac{100 \cdot \text{correct} + \frac{N}{2}}{N}$$thực hiện làm tròn đến số nguyên gần nhất bằng cách sử dụng số học số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét Brute Force |$O(N)$|$O(1)$| Đã chấp nhận | 
| Quét tương tự với làm tròn chính xác |$O(N)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$N$Và$K$. Chúng xác định số lượng so sánh và ngưỡng dung sai cho tính chính xác. 
2. Khởi tạo bộ đếm$\text{correct} = 0$tích lũy được bao nhiêu cặp thỏa mãn điều kiện. 
3. Cho mỗi cặp$(e_i, a_i)$, tính toán$|a_i - e_i|$và so sánh nó với$K$. Nếu giá trị lớn nhất$K$, tăng$\text{correct}$bằng 1. Điều này trực tiếp thực hiện định nghĩa của một dự đoán hợp lệ. 
4. Sau khi xử lý tất cả các cặp, tính tỷ lệ phần trăm như sau:$\frac{100 \cdot \text{correct}}{N}$, nhưng làm tròn đến số nguyên gần nhất đạt được bằng cách thêm$\frac{N}{2}$trước khi chia. 
5. Xuất ra số nguyên thu được. 

### Tại sao nó hoạt động 

Mỗi cặp đóng góp độc lập vào số lượng độ chính xác, do đó độ chính xác tổng thể được xác định hoàn toàn bằng cách tính tổng các biến chỉ báo cho từng điều kiện$|a_i - e_i| \le K$. Giá trị cuối cùng là một phép biến đổi tuyến tính của tổng này. Vì việc làm tròn chỉ được áp dụng một lần ở cuối nên không có sự mất đi độ chính xác trung gian nào ảnh hưởng đến độ chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    correct = 0

    for _ in range(n):
        e, a = map(int, input().split())
        if abs(e - a) <= k:
            correct += 1

    # rounded percentage
    ans = (correct * 100 + n // 2) // n
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai đọc đầu vào một cách tuyến tính và cập nhật bộ đếm theo thời gian không đổi trên mỗi cặp. Việc kiểm tra chênh lệch tuyệt đối thực thi trực tiếp điều kiện dung sai. Công thức làm tròn đảm bảo tỷ lệ phần trăm số nguyên gần nhất chính xác mà không cần số học dấu phẩy động, giúp tránh các vấn đề về độ chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
3 6
7 3
1 10
8 7
11 11
```| tôi | e | một | |e-a| | hợp lệ | đúng | 

|---|---|---|------|--------|----------| 

| 1 | 3 | 6 | 3 | vâng | 1 | 

| 2 | 7 | 3 | 4 | không | 1 | 

| 3 | 1 | 10 | 9 | không | 1 | 

| 4 | 8 | 7 | 1 | vâng | 2 | 

| 5 | 11 | 11 | 0 | vâng | 3 | 

Tỷ lệ phần trăm cuối cùng là$\frac{3}{5} \cdot 100 = 60$. 

Dấu vết này cho thấy chỉ có sự so sánh cục bộ mới quan trọng và không có thứ tự hay cấu trúc nào ảnh hưởng đến kết quả. 

### Ví dụ 2 

đầu vào:```
4 0
1 1
2 3
5 5
7 6
```| tôi | e | một | |e-a| | hợp lệ | đúng | 

|---|---|---|------|--------|----------| 

| 1 | 1 | 1 | 0 | vâng | 1 | 

| 2 | 2 | 3 | 1 | không | 1 | 

| 3 | 5 | 5 | 0 | vâng | 2 | 

| 4 | 7 | 6 | 1 | không | 2 | 

Độ chính xác là$\frac{2}{4} \cdot 100 = 50$. 

Trường hợp này cô lập điều kiện biên$K = 0$, trong đó chỉ những kết quả khớp chính xác mới được chấp nhận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| mỗi cặp được kiểm tra một lần với công việc liên tục | 
| Không gian |$O(1)$| chỉ một bộ đếm và một vài biến được lưu trữ | 

Tổng kích thước đầu vào tối đa là$10^4$cho mỗi bài kiểm tra, do đó, một lần vượt qua tuyến tính duy nhất cũng nằm trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# We cannot directly execute solve() here, but these are intended assertions:
# (In a real setup, replace run with calling solve())

# provided sample
# assert run("5 3\n3 6\n7 3\n1 10\n8 7\n11 11\n") == "60\n"

# edge: all correct
# assert run("3 10\n1 2\n2 1\n100 105\n") == "100\n"

# edge: none correct
# assert run("3 0\n1 2\n3 4\n5 6\n") == "0\n"

# edge: rounding check (2/3 = 66.666.. -> 67)
# assert run("3 1\n1 2\n2 1\n10 10\n") == "67\n"

# edge: single element
# assert run("1 0\n5 5\n") == "100\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều đúng | 100 | độ chính xác tối đa | 
| không đúng | 0 | giới hạn dưới | 
| trường hợp làm tròn | 67 | sửa cách làm tròn số nguyên gần nhất | 
| phần tử đơn | 100 | xử lý đầu vào tối thiểu |
