---
title: "CF 1426F - Số dãy con"
description: "Chúng ta được cung cấp một chuỗi gồm các chữ cái \"a\", \"b\", \"c\" và ký tự đặc biệt \"?\". Mỗi \"?\" có thể được thay thế độc lập bằng bất kỳ chữ cái nào \"a\", \"b\" hoặc \"c\"."
date: "2026-06-11T05:48:41+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "strings"]
categories: ["algorithms"]
codeforces_contest: 1426
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 674 (Div. 3)"
rating: 2000
weight: 1426
solve_time_s: 70
verified: true
draft: false
---

[CF 1426F - Số chuỗi con](https://codeforces.com/problemset/problem/1426/F) 

**Xếp hạng:** 2000 
**Tags:** tổ hợp, dp, chuỗi 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi gồm các chữ cái "a", "b", "c" và ký tự đặc biệt "?". Mỗi "?" có thể được thay thế độc lập bằng bất kỳ chữ cái nào "a", "b" hoặc "c". Sau khi thực hiện tất cả các thay thế có thể có, chúng ta cần đếm tổng số chuỗi con "abc" trên tất cả các chuỗi kết quả. Một dãy con không yêu cầu các chữ cái liền kề nhau nhưng phải duy trì trật tự tương đối. 

Kích thước đầu vào cho phép các chuỗi lên tới 200.000 ký tự, có nghĩa là bất kỳ thuật toán nào tạo ra tất cả$3^k$sự kết hợp của "?" sẽ không thể thực hiện được. Ngay cả một dấu "?" có thể tăng gấp ba số lượng chuỗi, vì vậy việc liệt kê lực lượng vũ phu là hoàn toàn không thể thực hiện được. Thách thức chính là tính đến tất cả các thay thế có thể có mà không lặp lại chúng một cách riêng lẻ, điều này đòi hỏi phải có lý luận tổ hợp hoặc lập trình động cẩn thận. 

Các trường hợp cạnh không rõ ràng bao gồm các chuỗi chỉ chứa dấu chấm hỏi, các chuỗi không có bất kỳ "a", "b" hoặc "c" nào và các chuỗi trong đó các chữ cái xuất hiện ở các vị trí ngăn cản việc hình thành "abc". Ví dụ: chuỗi "???" có thể tạo ra 27 chuỗi và kết quả đầu ra chính xác là 1×3 + 3×1 + … tùy thuộc vào cách phân phối, điều này minh họa rằng cách tính đơn giản có thể dễ dàng bỏ sót các đóng góp nhân từ dấu chấm hỏi. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là tạo ra mọi chuỗi có thể thu được bằng cách thay thế "?" bằng "a", "b" hoặc "c", sau đó đếm các chuỗi con "abc" trong mỗi chuỗi. Điều này đúng nhưng không thực tế, vì với k dấu chấm hỏi có$3^k$dây. Vì$k = 20$, điều này vượt quá$3^{20} \approx 3.5 \times 10^9$, vượt xa giới hạn khả thi cho chương trình 1 giây. 

Cái nhìn sâu sắc dẫn đến một giải pháp hiệu quả là duy trì số lượng các chuỗi con kết thúc bằng "a", "ab" và "abc" có thể được hình thành khi chúng ta xử lý chuỗi từ trái sang phải. Chúng tôi xử lý "?" dưới dạng ký tự đại diện có thể là một trong ba chữ cái. Mỗi "?" nhân các chuỗi con hiện có với ba vì mỗi chuỗi có thể tạo ra ba tùy chọn, nhưng nó cũng góp phần tạo ra các chuỗi con mới. Bằng cách theo dõi số lượng các chuỗi con một phần và chia tỷ lệ theo lũy thừa ba để tính các ký tự đại diện trước đó, chúng ta có thể tính toán câu trả lời theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(3^k \cdot n)$|$O(n)$| Quá chậm | 
| Lập trình động (đếm chuỗi con) |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo ba bộ đếm:`a_count`,`ab_count`,`abc_count`, tất cả được đặt thành 0. Chúng biểu thị số dãy con thuộc loại "a", "ab" và "abc" được hình thành cho đến nay. Khởi tạo`power_of_three`đến 1, biểu thị số cách có thể thay thế các dấu chấm hỏi trước đó. 
2. Lặp lại từng ký tự`ch`trong chuỗi. 
3. Nếu`ch`là "a", tăng dần`a_count`qua`power_of_three`. Điều này phản ánh rằng mọi kết hợp hiện có của các ký tự đại diện trước đó đều có thể được mở rộng bằng "a" này. 
4. Nếu`ch`là "b", tăng dần`ab_count`qua`a_count`. Giờ đây, mỗi chữ "a" trước đó có thể ghép nối với chữ "b" này để tạo thành một chuỗi con "ab" một phần. 
5. Nếu`ch`là "c", tăng dần`abc_count`qua`ab_count`. Giờ đây, mỗi "ab" trước đó có thể ghép nối với "c" này để hoàn thành chuỗi con "abc". 
6. Nếu`ch`là "?", nó có thể được coi là bất kỳ trong số "a", "b" hoặc "c". Trước khi cập nhật số đếm, hãy lưu trữ các giá trị hiện tại của`a_count`,`ab_count`, Và`abc_count`trong các biến tạm thời. Cập nhật từng số theo sự đóng góp của việc chọn "a", "b" hoặc "c": 

- Mới`abc_count`= 3 × cũ`abc_count`+ cũ`ab_count`(đóng góp nếu "?" là "c") 
- Mới`ab_count`= 3 × cũ`ab_count`+ cũ`a_count`(đóng góp nếu "?" là "b") 
- Mới`a_count`= 3 × cũ`a_count`+ power_of_ba (đóng góp nếu "?" là "a") 
- Nhân mỗi số cũ với 3 để tính ba tùy chọn của dấu "?" hiện tại. và thêm các đóng góp vào dãy con mới. 
7. Sau khi xử lý dấu "?", hãy nhân lên`power_of_three`bằng 3 để phản ánh ký tự đại diện bổ sung. 
8. Trở về`abc_count`modulo$10^9 + 7$sau khi xử lý toàn bộ chuỗi. 

**Tại sao nó hoạt động**: Ở mỗi bước, số đếm phản ánh tất cả các chuỗi con có thể được hình thành bằng tất cả các ký tự trước đó và phần mở rộng ký tự đại diện. Bằng cách cập nhật số lượng nhân cho mỗi dấu "?" và bổ sung cho những đóng góp cho các chuỗi con mới, chúng tôi đảm bảo không có sự kết hợp nào bị bỏ sót hoặc được tính hai lần. Thuật toán duy trì một bất biến:`a_count`= số cách chọn "a" cho đến nay,`ab_count`= số cách chọn "ab" và`abc_count`= số cách chọn "abc", xem xét tất cả các thay thế của "?" nhìn thấy đến mức đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

n = int(input())
s = input().strip()

a_count = 0
ab_count = 0
abc_count = 0
power_of_three = 1  # 3^number_of_question_marks_so_far

for ch in s:
    if ch == 'a':
        a_count = (a_count + power_of_three) % MOD
    elif ch == 'b':
        ab_count = (ab_count + a_count) % MOD
    elif ch == 'c':
        abc_count = (abc_count + ab_count) % MOD
    elif ch == '?':
        new_abc = (3 * abc_count + ab_count) % MOD
        new_ab = (3 * ab_count + a_count) % MOD
        new_a = (3 * a_count + power_of_three) % MOD
        a_count, ab_count, abc_count = new_a, new_ab, new_abc
        power_of_three = (power_of_three * 3) % MOD

print(abc_count)
```Giải pháp khởi tạo bộ đếm và lặp lại chuỗi một lần, cập nhật số đếm theo quy tắc cho chữ cái và ký tự đại diện. Sự tinh tế nằm ở việc nhân chính xác với ba cho các chuỗi trước đó khi có dấu "?" gặp phải, sau đó thêm đóng góp cho các chuỗi con mới được hình thành nếu dấu "?" trở thành "a", "b" hoặc "c". 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:`"ac?b?c"`| Chỉ mục | Char | a_count | ab_count | abc_count | sức mạnh của ba | 
| --- | --- | --- | --- | --- | --- | 
| 0 | một | 1 | 0 | 0 | 1 | 
| 1 | c | 1 | 0 | 0 | 1 | 
| 2 | ? | 4 | 1 | 0 | 3 | 
| 3 | b | 4 | 5 | 0 | 3 | 
| 4 | ? | 15 | 19 | 5 | 9 | 
| 5 | c | 15 | 19 | 24 | 9 | 

Dấu vết này cho thấy mỗi dấu "?" nhân các số đếm trước đó và đóng góp vào các chuỗi con mới, khớp với tổng số dự kiến là 24. 

Đầu vào tùy chỉnh 2:`"???"`| Chỉ mục | Char | a_count | ab_count | abc_count | sức mạnh của ba | 
| --- | --- | --- | --- | --- | --- | 
| 0 | ? | 1 | 0 | 0 | 3 | 
| 1 | ? | 4 | 1 | 0 | 9 | 
| 2 | ? | 13 | 5 | 1 | 27 | 

Câu trả lời là 1, như mong đợi, xác nhận rằng thuật toán xử lý chính xác nhiều dấu chấm hỏi liên tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần với sự cập nhật liên tục về thời gian của bộ đếm. | 
| Không gian | O(1) | Chỉ có một số bộ đếm số nguyên cố định được duy trì. | 

Được cho$n \le 2 \cdot 10^5$, giải pháp dễ dàng chạy trong vòng 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    s = input().strip()

    MOD = 10**9 + 7
    a_count = ab_count = abc_count = 0
    power_of_three = 1

    for ch in s:
        if ch == 'a':
            a_count = (a_count + power_of_three) % MOD
        elif ch == 'b':
            ab_count = (ab_count + a_count) % MOD
        elif ch == 'c':
            abc_count = (abc_count + ab_count) % MOD
        elif ch == '?':
            new_abc = (3 * abc_count + ab_count) % MOD
            new_ab = (3 * ab_count + a_count) % MOD
            new_a = (3 * a_count + power_of_three) % MOD
            a_count, ab_count, abc_count = new_a, new_ab, new_abc
            power_of_three = (power_of_three *
```
