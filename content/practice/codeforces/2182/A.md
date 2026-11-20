---
title: "CF 2182A - Dây Tết"
description: "Chúng ta được cấp một chuỗi gồm các ký tự 0, 2, 5 và 6. Một chuỗi được coi là chuỗi Năm Mới nếu nó chứa chuỗi con 2026 ở đâu đó hoặc hoàn toàn không chứa chuỗi con 2025."
date: "2026-06-07T21:51:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 2182
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 186 (Rated for Div. 2)"
rating: 800
weight: 2182
solve_time_s: 117
verified: true
draft: false
---

[CF 2182A - Chuỗi năm mới](https://codeforces.com/problemset/problem/2182/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam, triển khai, chuỗi 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi gồm các ký tự 0, 2, 5 và 6. Một chuỗi được coi là chuỗi _New Year_ nếu nó chứa chuỗi con`2026`ở đâu đó hoặc không chứa chuỗi con`2025`không hề. Đối với mỗi chuỗi, chúng ta được phép thay đổi bất kỳ ký tự nào thành một trong bốn ký tự được phép với số lần bất kỳ. Nhiệm vụ của chúng ta là tính toán số lần thay thế ký tự tối thiểu cần thiết để biến chuỗi thành chuỗi Năm mới. 

Mỗi trường hợp thử nghiệm cung cấp độ dài chuỗi`n`và chuỗi`s`. Những hạn chế`4 ≤ n ≤ 20`và lên đến`10^4`các trường hợp thử nghiệm ngụ ý rằng các chuỗi đủ ngắn để kiểm tra tổ hợp hoặc lực lượng đơn giản, nhưng số lượng trường hợp thử nghiệm cao, vì vậy chúng tôi cần một giải pháp hiệu quả cho từng chuỗi riêng lẻ. 

Các trường hợp cạnh không rõ ràng phát sinh khi chuỗi đã chứa một phần`2025`hoặc`2026`. Ví dụ, chuỗi`2025`không phải là chuỗi Năm Mới và cần ít nhất một thay đổi. Thay thế cái cuối cùng`5`với`6`làm cho nó`2026`, thỏa mãn điều kiện đầu tiên. Một trường hợp phức tạp khác là`20252025`, trong đó có chứa`2025`hai lần. Chỉ thay đổi ký tự cuối cùng là không đủ; chúng ta cần sự thay đổi tối thiểu mà nó mang lại`2026`hoặc loại bỏ`2025`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực ngây thơ là thử tất cả các kết hợp thay thế ký tự có thể có. Đối với mỗi ký tự, chúng ta có thể thử tất cả bốn cách thay thế có thể, tạo ra`4^n`các chuỗi có thể. Vì`n = 20`, điều này tạo ra hơn một nghìn tỷ khả năng, điều này rõ ràng là không khả thi. 

Quan sát chính là vấn đề bị hạn chế ở các chuỗi ngắn. Chuỗi con duy nhất trực tiếp kích hoạt một yêu cầu là`2026`hoặc`2025`. Vì vậy, chúng ta không cần tạo ra tất cả các chuỗi; chúng ta chỉ cần kiểm tra mọi chuỗi con có độ dài 4 trong đầu vào. Đối với mỗi cửa sổ có 4 chiều dài, chúng tôi tính toán cần bao nhiêu thay đổi ký tự để tạo chính xác`2026`. Giá trị tối thiểu của các giá trị này trên tất cả các cửa sổ là số lượng thay đổi tối thiểu cần thiết để đáp ứng điều kiện đầu tiên. 

Nếu chuỗi chứa`2025`, chúng ta có thể sửa đổi một số ký tự để ngắt`2025`hoặc trực tiếp chuyển đổi một lần xuất hiện thành`2026`(đồng thời phá vỡ`2025`và thỏa mãn điều kiện đầu tiên). Điều này làm cho giải pháp trở nên đơn giản hơn nhiều: lặp lại tất cả các chuỗi con có độ dài 4, tính toán các điểm không khớp với`2026`, và chọn mức tối thiểu. Bởi vì`n ≤ 20`, việc kiểm tra này yêu cầu nhiều nhất`17 * 4 = 68`so sánh trên mỗi chuỗi, đủ nhanh để`10^4`trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các sản phẩm thay thế) | O(4^n) | O(n) | Quá chậm | 
| Tối ưu (kiểm tra chuỗi con có độ dài 4) | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc chuỗi`s`và chiều dài của nó`n`. 
3. Khởi tạo một biến`min_changes`với giá trị lớn. Điều này sẽ lưu trữ các hoạt động tối thiểu cần thiết. 
4. Lặp lại tất cả các chuỗi con có độ dài 4 inch`s`. Với mỗi chuỗi con, hãy đếm xem có bao nhiêu ký tự khác với chuỗi đích`2026`. 
5. Cập nhật`min_changes`với sự không khớp tối thiểu được tìm thấy giữa tất cả các chuỗi con. 
6. Nếu chuỗi không chứa`2025`thì không cần thay đổi gì theo điều kiện chuỗi Năm Mới thứ hai. 
7. Xuất ra số lượng thao tác tối thiểu cho từng trường hợp thử nghiệm. 

Tại sao nó hoạt động: bằng cách kiểm tra tất cả các chuỗi con có độ dài 4`2026`, chúng tôi đảm bảo rằng nếu có thể tạo được`2026`trong chuỗi có ít thay đổi nhất, chúng ta sẽ tìm được số tối ưu. Đối với chuỗi không chứa`2025`, thuật toán ngầm trả về 0 vì không cần thay đổi để thỏa mãn điều kiện thứ hai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()
    
    if "2025" not in s:
        print(0)
        continue
    
    min_changes = float('inf')
    for i in range(n - 3):
        substring = s[i:i+4]
        changes = sum(substring[j] != target for j, target in enumerate("2026"))
        if changes < min_changes:
            min_changes = changes
    print(min_changes)
```Mã này trước tiên sẽ kiểm tra xem`2025`tồn tại. Nếu không, không cần thay đổi. Mặt khác, nó lặp qua từng chuỗi con 4 ký tự và đếm các chuỗi không khớp với`2026`.`min_changes`đảm bảo chúng tôi tìm thấy số lượng hoạt động tối thiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
2025
```| tôi | chuỗi con | không khớp với 2026 | phút_thay đổi | 
| --- | --- | --- | --- | 
| 0 | 2025 | 1 | 1 | 

Đầu ra: 1 

Chuỗi con`2025`chỉ yêu cầu ký tự cuối cùng thay đổi thành`6`trở thành`2026`. 

### Ví dụ 2 

đầu vào:```
8
20252025
```| tôi | chuỗi con | không khớp với 2026 | phút_thay đổi | 
| --- | --- | --- | --- | 
| 0 | 2025 | 1 | 1 | 
| 1 | 0252 | 3 | 1 | 
| 2 | 2520 | 4 | 1 | 
| 3 | 5202 | 4 | 1 | 
| 4 | 2025 | 1 | 1 | 

Đầu ra: 1 

Mặc dù có hai trường hợp xảy ra`2025`, việc thay đổi một ký tự trong lần xuất hiện đầu tiên là đủ để thỏa mãn điều kiện đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n) | Đối với mỗi`t`các trường hợp thử nghiệm, chúng tôi lặp lại tối đa`n-3`chuỗi con có độ dài 4. | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng; chúng tôi không lưu trữ mảng trung gian. | 

Được cho`t ≤ 10^4`Và`n ≤ 20`, tổng số thao tác nhiều nhất là`2 * 10^5`, vừa vặn thoải mái trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call the solution
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        
        if "2025" not in s:
            print(0)
            continue
        
        min_changes = float('inf')
        for i in range(n - 3):
            substring = s[i:i+4]
            changes = sum(substring[j] != target for j, target in enumerate("2026"))
            if changes < min_changes:
                min_changes = changes
        print(min_changes)
    return output.getvalue().strip()

# Provided samples
assert run("7\n4\n0000\n4\n2025\n4\n2026\n8\n20252026\n8\n20252025\n9\n202520256\n9\n202520265\n") == "0\n1\n0\n0\n1\n1\n0"

# Custom cases
assert run("2\n4\n2025\n4\n0002\n") == "1\n0", "handles single 2025 and no 2025"
assert run("1\n20\n20252025202520252025\n") == "1", "long repeated 2025"
assert run("1\n4\n2026\n") == "0", "already New Year string"
assert run("1\n5\n22222\n") == "0", "no 2025, no change needed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2025`| 1 | Chuyển đổi chính xác một lần xuất hiện của năm 2025 | 
|`0002`| 0 | Không chứa 2025 nên không thay đổi | 
|`20252025202520252025`| 1 | Nhiều năm 2025 liên tiếp, áp dụng thay đổi tối thiểu | 
|`2026`| 0 | Đã chứa 2026 | 
|`22222`| 0 | Không có chuỗi con bị cấm, không có gì để thay đổi | 

## Vỏ cạnh 

Đối với một chuỗi đã hợp lệ theo điều kiện thứ hai, chẳng hạn như`0000`, thuật toán kiểm tra`2025`. Vì nó không tồn tại nên nó sẽ xuất ra số 0 ngay lập tức. Đối với các chuỗi có nhiều chồng chéo`2025`, thuật toán kiểm tra tất cả các chuỗi con có độ dài 4 và tìm ra số lượng ký tự thay đổi tối thiểu để tạo`2026`, đồng thời loại bỏ tất cả các trường hợp của`2025`trong chuỗi con đó. Đối với các chuỗi đã chứa`2026`, không khớp với`2026`sẽ bằng 0, do đó đầu ra bằng 0, xác nhận tính đúng đắn.
