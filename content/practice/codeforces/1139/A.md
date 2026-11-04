---
title: "CF 1139A - Chuỗi con chẵn"
description: "Chúng ta được cấp một chuỗi các chữ số từ 1 đến 9. Nhiệm vụ của chúng ta là đếm xem có bao nhiêu chuỗi con, được xác định bởi bất kỳ phạm vi chỉ số liền kề nào, đại diện cho các số chẵn. Một chuỗi con là số chẵn nếu chữ số cuối cùng của nó là số chẵn, vì tính chẵn lẻ của số đó được xác định hoàn toàn bởi chữ số cuối cùng."
date: "2026-06-12T03:50:18+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1139
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 548 (Div. 2)"
rating: 800
weight: 1139
solve_time_s: 68
verified: true
draft: false
---

[CF 1139A - Chuỗi con chẵn](https://codeforces.com/problemset/problem/1139/A) 

**Đánh giá:** 800 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các chữ số từ 1 đến 9. Nhiệm vụ của chúng ta là đếm xem có bao nhiêu chuỗi con, được xác định bởi bất kỳ phạm vi chỉ số liền kề nào, đại diện cho các số chẵn. Một chuỗi con là số chẵn nếu chữ số cuối cùng của nó là số chẵn, vì tính chẵn lẻ của số đó được xác định hoàn toàn bởi chữ số cuối cùng. 

Kích thước đầu vào có thể lên tới 65.000 chữ số. Điều này loại trừ bất kỳ giải pháp nào xây dựng rõ ràng tất cả các chuỗi con vì tổng số chuỗi con là khoảng$n(n+1)/2$, tức là hơn 2 tỷ cho$n = 65,000$. Bất kỳ thuật toán nào cố gắng lặp qua tất cả các chuỗi con và chuyển đổi chúng thành số nguyên sẽ quá chậm. 

Trường hợp cạnh tinh tế là một chuỗi bao gồm toàn bộ các chữ số lẻ. Ví dụ: "1357" sẽ tạo ra kết quả là 0, vì không có chuỗi con nào kết thúc bằng chữ số chẵn. Một trường hợp đặc biệt khác là khi mọi chữ số đều là số chẵn, chẳng hạn như "246", trong đó mọi chuỗi con sẽ được tính. Việc xử lý chính xác những điều này phụ thuộc vào việc đếm các chuỗi con dựa trên chữ số cuối cùng thay vì thử tính toán trên các chuỗi con đầy đủ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại tất cả các chỉ số bắt đầu và kết thúc có thể có của chuỗi con, chuyển đổi chuỗi con thành số và kiểm tra xem nó có chia hết cho 2 hay không. Điều này đúng nhưng quá chậm vì nó yêu cầu$O(n^2)$trích xuất chuỗi con và chuyển đổi số nguyên. Với$n = 65,000$, đây là đại khái$2 \times 10^9$hoạt động vượt quá giới hạn hợp lý cho giới hạn thời gian 0 giây. 

Điểm mấu chốt là tính chẵn lẻ của một số chỉ phụ thuộc vào chữ số cuối cùng của nó. Một chuỗi con kết thúc ở vị trí$i$thậm chí là nếu$s[i]$là một trong 2, 4, 6 hoặc 8. Mọi chuỗi con kết thúc ở vị trí này đều có thể được đếm trực tiếp: có chính xác$i$chuỗi con kết thúc tại chỉ mục$i$. Do đó, thay vì tạo ra các chuỗi con, chúng tôi chỉ cần lặp qua chuỗi và bất cứ khi nào chúng tôi thấy một chữ số chẵn, chúng tôi sẽ thêm vị trí của nó (được lập chỉ mục 1) vào tổng số đang chạy của chúng tôi. Điều này làm giảm độ phức tạp về thời gian để$O(n)$và tránh các thao tác chuỗi hoặc số nguyên không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào và độ dài của nó. Chúng ta sẽ lặp qua chuỗi từ trái sang phải bằng chỉ mục dựa trên 1. 
2. Khởi tạo bộ đếm về 0. Bộ đếm này sẽ tích lũy tổng số chuỗi con chẵn. 
3. Đối với mỗi ký tự tại chỉ mục$i$, chuyển đổi nó thành số nguyên để kiểm tra tính chẵn lẻ của nó. 
4. Nếu chữ số chẵn (2, 4, 6, 8), hãy tăng bộ đếm lên$i$. Điều này có tác dụng vì mọi chuỗi con kết thúc ở vị trí này được xác định duy nhất bởi chỉ số bắt đầu của nó từ 1 đến$i$. 
5. Tiếp tục cho đến hết chuỗi rồi in bộ đếm. 

Tại sao nó hoạt động: bất biến là cho mỗi chỉ mục$i$, tổng số chuỗi con kết thúc tại$i$chính xác là$i$và mỗi chuỗi con kết thúc bằng chữ số chẵn sẽ góp phần tạo ra câu trả lời. Vì chúng tôi truy cập mọi chỉ mục chính xác một lần và đếm tất cả các chuỗi con kết thúc bằng các chữ số chẵn nên không có chuỗi con hợp lệ nào bị bỏ sót hoặc được tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
s = input().strip()

count = 0
for i, c in enumerate(s, start=1):
    if int(c) % 2 == 0:
        count += i

print(count)
```Giải pháp đọc chuỗi đầu vào và liệt kê nó bằng chỉ mục dựa trên 1. Đối với mỗi ký tự, chúng tôi kiểm tra xem nó có chẵn không và nếu có, hãy thêm chỉ mục của nó vào tổng số. Chúng ta không cần lưu trữ chuỗi con hoặc chuyển đổi nhiều ký tự thành số, điều này giúp duy trì cả bộ nhớ và khả năng tính toán ở mức thấp. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

Đầu vào: "1234" 

| tôi | s[i] | có bằng không? | cập nhật số lượng | tổng số | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | không | - | 0 | 
| 2 | 2 | vâng | +2 | 2 | 
| 3 | 3 | không | - | 2 | 
| 4 | 4 | vâng | +4 | 6 | 

Điều này xác nhận rằng các chuỗi con kết thúc ở vị trí 2 và 4 được tính chính xác. 

**Mẫu 2:** 

Đầu vào: "246" 

| tôi | s[i] | có bằng không? | cập nhật số lượng | tổng số | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | vâng | +1 | 1 | 
| 2 | 4 | vâng | +2 | 3 | 
| 3 | 6 | vâng | +3 | 6 | 

Tất cả các chuỗi con đều được tính, thể hiện việc xử lý chính xác các chữ số chẵn liên tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được truy cập một lần và kiểm tra tính chẵn lẻ là O(1). | 
| Không gian | O(1) | Chỉ có một bộ đếm được sử dụng; không cần lưu trữ bổ sung. | 

Được cho$n \le 65,000$, thuật toán thực hiện tối đa 65.000 thao tác, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    s = input().strip()
    count = 0
    for i, c in enumerate(s, start=1):
        if int(c) % 2 == 0:
            count += i
    return str(count)

# Provided samples
assert run("4\n1234\n") == "6", "sample 1"
assert run("3\n246\n") == "6", "sample 2"

# Custom cases
assert run("1\n1\n") == "0", "single odd digit"
assert run("1\n2\n") == "1", "single even digit"
assert run("5\n11111\n") == "0", "all odd digits"
assert run("5\n22222\n") == "15", "all even digits"
assert run("6\n135792\n") == "6", "mixed digits, only last even matters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | một chữ số lẻ, số đếm bằng 0 | 
| 2 | 1 | một chữ số chẵn, số lượng tối thiểu khác 0 | 
| 11111 | 0 | tất cả các chữ số lẻ, không tính chuỗi con | 
| 22222 | 15 | tất cả các chữ số chẵn, tổng các chỉ số 1+2+3+4+5 | 
| 135792 | 6 | các chữ số hỗn hợp, chữ số chẵn cuối cùng được đếm chính xác | 

## Vỏ cạnh 

Đối với chuỗi một chữ số "1", thuật toán xuất ra đúng 0 vì chuỗi con duy nhất kết thúc bằng một chữ số lẻ. Đối với một chuỗi có tất cả các chữ số chẵn như "22222", mỗi chỉ số đóng góp vị trí dựa trên 1 của nó vào tổng, mang lại 1+2+3+4+5 = 15. Đối với tính chẵn lẻ xen kẽ như "135792", các chữ số chẵn duy nhất nằm ở vị trí 2 và 6, đóng góp 2+6=8 vào tổng, xác nhận rằng thuật toán không bỏ sót hoặc đếm kép các chuỗi con.
