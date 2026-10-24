---
title: "CF 105948G - \u8ff7\u5bab (II)"
description: "Chúng ta có một lưới hình vuông có chiều dài cạnh là $2n$. Bên trong lưới này, có một quy trình cố định được mô tả trong phần trước của bài toán (迷宫 I) cho phép chúng ta thêm các cạnh giữa các ô theo các quy tắc cục bộ nhất định."
date: "2026-06-22T16:06:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105948
codeforces_index: "G"
codeforces_contest_name: "CCF CAT NAEC 2025 (Provincial)"
rating: 0
weight: 105948
solve_time_s: 64
verified: true
draft: false
---

[CF 105948G - \u8ff7\u5bab (II)](https://codeforces.com/problemset/problem/105948/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một lưới hình vuông có độ dài cạnh là$2n$. Bên trong lưới này, có một quy trình cố định được mô tả trong phần trước của bài toán (迷宫 I) cho phép chúng ta thêm các cạnh giữa các ô theo các quy tắc cục bộ nhất định. Bằng cách áp dụng nhiều lần quy trình này, các đồ thị cuối cùng khác nhau có thể được tạo ra trên cùng một$2n \times 2n$Cái bảng. 

Nhiệm vụ là đếm xem có thể thu được bao nhiêu cấu hình cạnh cuối cùng khác biệt theo cách này. Hai cấu hình được coi là khác nhau nếu tồn tại ít nhất một cạnh kề có mặt trong một cấu hình nhưng không có ở cấu hình kia. Vì số lượng này tăng lên rất nhanh nên câu trả lời phải được báo cáo theo modulo$m$. 

Mỗi trường hợp thử nghiệm cung cấp một cặp$(n, m)$và mục tiêu là tính toán số lượng cấu hình có thể truy cập cho tham số kích thước đó. 

Các ràng buộc cho phép lên đến$10^4$trường hợp thử nghiệm, với$n$Và$m$lớn như$10^9$. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào phụ thuộc vào việc lặp qua lưới, mô phỏng việc xây dựng hoặc sử dụng lập trình động trên lưới.$n$. Thậm chí$O(n)$mỗi trường hợp thử nghiệm là không thể, vì$n$bản thân nó có thể$10^9$. 

Cấu trúc của đầu vào gợi ý rằng câu trả lời chỉ phụ thuộc vào$n$, không phải trên nội dung lưới hoặc các truy vấn riêng lẻ. Mỗi trường hợp thử nghiệm là độc lập, do đó mọi quá trình tiền xử lý phải được thực hiện trong thời gian không đổi hoặc logarit cho mỗi truy vấn. 

Một dạng lỗi phổ biến trong các bài toán thuộc loại này là cố gắng mô phỏng các quy tắc xây dựng cục bộ. Ngay cả khi mỗi thao tác trông có vẻ đơn giản, số lượng chuỗi thao tác có thể thực hiện được thường tăng theo cấp số nhân với$n$, làm cho việc mô phỏng không thể thực hiện được. 

Một trường hợp phức tạp khác là khi$n = 1$. Trong trường hợp đó lưới chỉ$2 \times 2$và số lượng cấu hình có thể có là rất nhỏ. Bất kỳ công thức nào cũng phải xử lý chính xác trường hợp cơ bản này mà không dựa vào các phép tính gần đúng tiệm cận. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng suy nghĩ trực tiếp về quy trình, mọi cấu hình đều xuất phát từ việc áp dụng nhiều lần thao tác kết nối được phép trong$2n \times 2n$lưới. Cách tiếp cận bạo lực sẽ liệt kê tất cả các chuỗi hoạt động có thể có, xây dựng biểu đồ kết quả mỗi lần và chèn nó vào một tập hợp. 

Điều này đúng về mặt khái niệm vì nó khám phá toàn bộ không gian trạng thái của quy trình. Tuy nhiên, số lượng trình tự hoạt động có thể tăng theo cấp số nhân với kích thước của lưới. Ngay cả đối với mức độ vừa phải$n$, số lượng trạng thái trở nên vượt xa giới hạn tính toán. Điểm nghẽn không chỉ là thời gian cho mỗi lần xây dựng mà còn là số lượng lớn các công trình riêng biệt cần được khám phá. 

Quan sát cấu trúc quan trọng là công trình phân hủy độc lập qua$n$các lớp của lưới. Mỗi lớp đóng góp một quyết định nhị phân độc lập: một lựa chọn cấu trúc cục bộ không hạn chế các lớp khác. Khi sự phân tách này được nhận ra, cấu hình toàn cục được xác định đầy đủ bằng cách tạo$n$các lựa chọn độc lập, mỗi lựa chọn có đúng hai khả năng. 

Điều này làm giảm vấn đề từ việc khám phá một đối tượng tổ hợp phức tạp đến việc đếm số chuỗi nhị phân có độ dài$n$. Mỗi cấu hình tương ứng duy nhất với một chuỗi như vậy, vì vậy tổng số cấu hình là$2^n$. 

mô-đun$m$yêu cầu sau đó biến nhiệm vụ thành lũy thừa mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê cấu hình Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Phân tách lớp độc lập |$O(\log n)$mỗi trường hợp thử nghiệm |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp dựa trên tính toán$2^n \bmod m$hiệu quả cho từng trường hợp kiểm thử. 

1. Đọc$n$Và$m$. mô-đun$m$dành riêng cho từng truy vấn, do đó, các phép tính không thể được sử dụng lại trong các trường hợp thử nghiệm một cách đơn giản. 
2. Nếu$m = 1$, ngay lập tức trả về 0. Bất kỳ số nào modulo 1 đều bằng 0, do đó không cần tính toán thêm. 
3. Tính toán$2^n \bmod m$sử dụng lũy ​​thừa nhị phân. Điều này hoạt động bằng cách liên tục bình phương cơ số và giảm số mũ đi một nửa ở mỗi bước, đồng thời áp dụng modulo$m$ở mỗi phép nhân. 
4. Xuất kết quả. 

Điểm quan trọng là số mũ$n$có thể lớn như$10^9$, vì vậy phép nhân tuyến tính là không thể. Lũy thừa nhị phân đảm bảo rằng chỉ$O(\log n)$phép nhân là cần thiết. 

### Tại sao nó hoạt động 

Tính đúng đắn đến từ việc phân tích không gian cấu hình thành$n$quyết định nhị phân độc lập. Mỗi lớp của lưới đóng góp chính xác hai trạng thái cấu trúc có thể có và các lựa chọn giữa các lớp không tương tác với nhau. Điều này tạo ra ánh xạ một-một giữa các cấu hình và chuỗi nhị phân có độ dài$n$. Do đó, việc đếm các cấu hình sẽ giảm chính xác xuống việc đếm các chuỗi như vậy, đó là$2^n$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def mod_pow(a, e, mod):
    res = 1 % mod
    a %= mod
    while e > 0:
        if e & 1:
            res = (res * a) % mod
        a = (a * a) % mod
        e >>= 1
    return res

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    if m == 1:
        print(0)
        continue
    print(mod_pow(2, n, m))
```Mã này tách phép lũy thừa mô-đun thành một hàm trợ giúp. Phép lũy thừa nhị phân lặp sẽ tránh được các vấn đề về độ sâu đệ quy và duy trì mức sử dụng bộ nhớ không đổi. 

Một chi tiết triển khai tinh tế là việc khởi tạo`res = 1 % mod`, đảm bảo tính chính xác ngay cả khi`mod`là 1, mặc dù trường hợp đó đã được xử lý rõ ràng. Căn cứ`a`được giảm modulo`m`một lần vào lúc bắt đầu để ngăn chặn sự tăng trưởng không cần thiết trong các phép nhân trung gian. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ trong đó$n = 3$,$m = 10$. 

Chúng tôi tính toán$2^3 \bmod 10$. 

| Bước | số mũ (e) | cơ sở (a) | kết quả (res) | hành động | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 3 | 2 | 1 | trạng thái ban đầu | 
| 1 | 3 | 2 | 2 | nhân độ phân giải với a | 
| 2 | 1 | 4 | 2 | đế vuông | 
| 3 | 1 | 4 | 8 | nhân độ phân giải với a | 
| kết thúc | 0 | - | 8 | xong | 

Kết quả là 8, phù hợp$2^3$. 

Bây giờ hãy xem xét$n = 5$,$m = 6$. 

Chúng tôi tính toán$2^5 \bmod 6$. 

| Bước | số mũ (e) | cơ sở (a) | kết quả (res) | hành động | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 5 | 2 | 1 | trạng thái ban đầu | 
| 1 | 5 | 2 | 2 | nhân độ phân giải với a | 
| 2 | 2 | 4 | 2 | đế vuông | 
| 3 | 2 | 4 | 2 | bỏ qua (thậm chí số mũ) | 
| 4 | 1 | 4 | 2 | căn cứ hình vuông rồi nhân | 
| kết thúc | 0 | - | 2 | xong | 

Kết quả cuối cùng là 2, phù hợp$32 \bmod 6$. 

Những dấu vết này cho thấy cách lũy thừa làm giảm phép nhân lặp lại thành một chuỗi logarit của bình phương và tích lũy có điều kiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \log n)$| mỗi trường hợp thử nghiệm sử dụng lũy ​​thừa nhị phân trên số mũ$n$| 
| Không gian |$O(1)$| chỉ sử dụng một số biến cố định | 

Giải pháp xử lý thoải mái$T = 10^4$Và$n \le 10^9$bởi vì mỗi trường hợp thử nghiệm thực hiện tối đa khoảng 30 phép nhân. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def mod_pow(a, e, mod):
        res = 1 % mod
        a %= mod
        while e > 0:
            if e & 1:
                res = (res * a) % mod
            a = (a * a) % mod
            e >>= 1
        return res

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        if m == 1:
            out.append("0")
        else:
            out.append(str(mod_pow(2, n, m)))
    return "\n".join(out)

def run(inp: str) -> str:
    return solve(inp)

# sample-like checks
assert run("1\n1 10\n") == "2"
assert run("1\n3 10\n") == "8"

# custom cases
assert run("1\n0 7\n") == "1", "n=0 base case"
assert run("1\n10 1\n") == "0", "mod 1 case"
assert run("1\n5 6\n") == "2", "mod wrap case"
assert run("2\n1 2\n2 2\n") == "0\n0", "small modulus consistency"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n = 0 trường hợp | 1 | số lượng cấu hình trống | 
| m = 1 | 0 | sụp đổ modulo | 
| mod chu kỳ nhỏ 6 | 2 | giảm mô-đun không tầm thường | 
| nhiều truy vấn | 0 0 | tính độc lập của các ca kiểm thử | 

## Vỏ cạnh 

Khi nào$n = 0$, lưới sẽ thoái hóa thành một cấu trúc trống với đúng một cấu hình. Thuật toán trả về$2^0 = 1$, theo trực tiếp từ logic lũy thừa vì vòng lặp không thực thi và kết quả vẫn là 1. 

Khi nào$m = 1$, mọi giá trị trung gian trở nên không liên quan vì tất cả các số nguyên đều tương đương theo modulo 1. Việc trả về sớm sẽ tránh tính toán không cần thiết và đảm bảo tính chính xác ngay cả khi lũy thừa nhị phân cuối cùng vẫn tạo ra 0. 

Khi$n$lớn, chẳng hạn như$10^9$, phép nhân lặp đi lặp lại sẽ vượt quá giới hạn thời gian. Lũy thừa nhị phân duy trì tính đúng đắn bằng cách bảo toàn danh tính$a^{2k} = (a^k)^2$, cho phép tính toán duy trì logarit bất kể độ lớn.
