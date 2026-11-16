---
title: "CF 1926A - Vlad Và Năm Người Tốt Nhất"
description: "Nhiệm vụ rất đơn giản: với mỗi chuỗi có độ dài 5 chỉ gồm các chữ cái A và B, hãy xác định chữ cái nào xuất hiện thường xuyên hơn. Mỗi trường hợp kiểm thử cung cấp một chuỗi như vậy và chúng tôi có nhiều trường hợp kiểm thử cần xử lý."
date: "2026-06-08T18:58:51+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1926
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 928 (Div. 4)"
rating: 800
weight: 1926
solve_time_s: 89
verified: true
draft: false
---

[CF 1926A - Vlad và bộ năm người giỏi nhất](https://codeforces.com/problemset/problem/1926/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ rất đơn giản: với mỗi chuỗi có độ dài năm chỉ bao gồm các chữ cái`A`Và`B`, xác định chữ cái nào xuất hiện thường xuyên hơn. Mỗi trường hợp kiểm thử cung cấp một chuỗi như vậy và chúng tôi có nhiều trường hợp kiểm thử cần xử lý. Đầu ra chỉ là ký tự`A`hoặc`B`cho mỗi chuỗi, đại diện cho chữ cái thường xuyên nhất. 

Những ràng buộc là cực kỳ nhẹ nhàng. Mỗi chuỗi có độ dài chính xác là 5 và số trường hợp kiểm tra nhiều nhất là 32. Điều này có nghĩa là ngay cả thuật toán brute-force kiểm tra mọi ký tự của mỗi chuỗi cũng nhanh một cách tầm thường, vì tổng số lần kiểm tra ký tự trong trường hợp xấu nhất là 32 lần 5 hoặc 160 thao tác. Không cần tối ưu hóa hoặc cấu trúc dữ liệu phức tạp. Việc sử dụng bộ nhớ cũng không đáng kể. 

Trường hợp cạnh không rõ ràng xảy ra khi`A`Và`B`xuất hiện cùng số lần. Trong một chuỗi có độ dài bằng 5, điều này không thể xảy ra chính xác vì 5 là số lẻ. Vì vậy, một chữ cái luôn thống trị nghiêm ngặt. Các chuỗi như`AAAAA`hoặc`BBBBB`đại diện cho các trường hợp cực đoan, trong đó cùng một ký tự lấp đầy toàn bộ chuỗi. Một trường hợp tinh tế khác là khi chỉ có một lần xuất hiện của chữ cái chiếm ưu thế, chẳng hạn`ABBBB`, trong đó logic đếm không được tính sai. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là lặp qua từng chuỗi và đếm số lần xuất hiện của`A`Và`B`bằng cách sử dụng một vòng lặp đơn giản. Khi đã biết cả hai số đếm, hãy so sánh chúng và xuất ra chữ cái có số lượng cao hơn. Điều này hiệu quả vì độ dài chuỗi không đổi và rất nhỏ, do đó chi phí của hai bộ đếm riêng biệt trên mỗi chuỗi là không đáng kể. 

Cái nhìn sâu sắc quan trọng cho một cách tiếp cận tinh tế hơn một chút là các chuỗi Python hỗ trợ một cơ chế tích hợp sẵn`count`phương thức, vì vậy chúng ta có thể gọi trực tiếp`s.count('A')`Và`s.count('B')`cho mỗi chuỗi. Độ dài tối đa là năm, vì vậy`count`lặp lại tối đa năm ký tự bên trong. Điều này cho kết quả tương tự với mã sạch hơn. Không cần phải tối ưu hóa thêm vì thuật toán đã chạy trong thời gian không đổi cho từng trường hợp thử nghiệm. 

Cả phương pháp brute-force và phương pháp đếm tích hợp đều chạy theo thời gian tuyến tính tương ứng với độ dài chuỗi, nhưng số lượng thao tác thực tế quá nhỏ nên về cơ bản chúng tương đương nhau trong thực tế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Vòng đếm thủ công | O(5 * t) = O(t) | O(1) | Đã chấp nhận | 
| Python`count`phương pháp | O(5 * t) = O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Điều này cho chúng ta biết có bao nhiêu chuỗi cần xử lý. 
2. Lặp lại từng test case, đọc chuỗi`s`có chiều dài năm. 
3. Đếm số lượng`A`các ký tự trong chuỗi bằng cách sử dụng`s.count('A')`. Lưu trữ nó trong một biến`a_count`. 
4. Đếm số lượng`B`ký tự sử dụng`s.count('B')`, hoặc tính toán nó một cách tương đương như`5 - a_count`vì độ dài chuỗi là cố định. 
5. So sánh`a_count`Và`b_count`. Nếu như`a_count`lớn hơn, sản lượng`A`; nếu không thì xuất ra`B`. 
6. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: tổng của`A`Và`B`số đếm luôn bằng năm, vì vậy một số đếm là đủ để suy ra số còn lại. Mỗi chuỗi có một chữ cái đa số duy nhất vì độ dài là số lẻ nên việc so sánh ở bước năm luôn cho ra câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = input().strip()
    a_count = s.count('A')
    b_count = 5 - a_count
    if a_count > b_count:
        print('A')
    else:
        print('B')
```Giải pháp đọc tất cả các dòng đầu vào bằng I/O nhanh. các`.strip()`cuộc gọi sẽ xóa mọi ký tự dòng mới ở cuối. Đếm`A`đầu tiên và trừ đi năm để có được`B`tránh cuộc gọi thứ hai không cần thiết tới`count`. Việc so sánh rất đơn giản vì chữ cái đa số được đảm bảo bởi độ dài chuỗi. Không có rủi ro riêng lẻ nào vì chúng tôi đang làm việc với độ dài cố định là 5. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**`ABABB`| Bước |`s`|`a_count`|`b_count`| Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | ABABB | 2 | 3 | B | 

**Đầu vào mẫu 2:**`AAAAA`| Bước |`s`|`a_count`|`b_count`| Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | AAAAA | 5 | 0 | A | 

Những dấu vết này cho thấy thuật toán xác định chính xác ký tự đa số trong chuỗi trong đó`B`chiếm ưu thế và ở đâu`A`thống trị. Logic đếm rất đơn giản và độ dài cố định ngăn ngừa các trường hợp mơ hồ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi chuỗi có độ dài 5, do đó các phép tính đếm không đổi trên mỗi chuỗi. Với tối đa 32 chuỗi, tổng thời gian là tối thiểu. | 
| Không gian | O(1) | Chúng tôi chỉ lưu trữ hai bộ đếm trên mỗi chuỗi và không có cấu trúc dữ liệu bổ sung. | 

Với những hạn chế, giải pháp này hoàn thành trong thời gian không đáng kể và sử dụng bộ nhớ tầm thường, nằm trong giới hạn được cung cấp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        s = input().strip()
        a_count = s.count('A')
        b_count = 5 - a_count
        if a_count > b_count:
            output.append('A')
        else:
            output.append('B')
    return "\n".join(output)

# provided sample
assert run("8\nABABB\nABABA\nBBBAB\nAAAAA\nBBBBB\nBABAA\nAAAAB\nBAAAA\n") == "B\nA\nB\nA\nB\nA\nA\nA", "sample 1"

# custom cases
assert run("1\nBBBBB\n") == "B", "all B"
assert run("1\nAAAAA\n") == "A", "all A"
assert run("1\nABBBB\n") == "B", "one A, four B"
assert run("1\nAAABB\n") == "A", "three A, two B"
assert run("1\nBAAAA\n") == "A", "one B, four A"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| BBBBB | B | tất cả các ký tự giống nhau, tất cả B | 
| AAAAA | A | tất cả các ký tự giống nhau, tất cả A | 
| ABBBB | B | đa số nhỏ của B trên A | 
| AAABB | A | đa số nhỏ của A trên B | 
| BAAAA | A | một B lúc đầu, đa số A | 

## Vỏ cạnh 

Đối với các chuỗi có tất cả các ký tự giống nhau, như`AAAAA`hoặc`BBBBB`, thuật toán đếm năm cho một chữ cái và 0 cho chữ cái còn lại. Việc so sánh vẫn hoạt động và trả về đúng chữ cái. Đối với các chuỗi có một ký tự thiểu số, chẳng hạn như`ABBBB`hoặc`BAAAA`, việc đếm vẫn tạo ra đa số chính xác vì độ dài chuỗi cố định đảm bảo một ký tự trội duy nhất. Không cần xử lý bổ sung.
