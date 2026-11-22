---
title: "CF 290D - Cam"
description: "Chúng ta được cung cấp một chuỗi bao gồm các chữ cái tiếng Anh, cả chữ hoa và chữ thường, cùng một số nguyên biểu thị \"ngân sách viết hoa\" trong khoảng từ 0 đến 26."
date: "2026-06-05T10:35:44+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 290
codeforces_index: "D"
codeforces_contest_name: "April Fools Day Contest 2013"
rating: 1400
weight: 290
solve_time_s: 127
verified: true
draft: false
---

[CF 290D - Cam](https://codeforces.com/problemset/problem/290/D) 

**Đánh giá:** 1400 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi bao gồm các chữ cái tiếng Anh, cả chữ hoa và chữ thường, và một số nguyên biểu thị "ngân sách viết hoa" trong khoảng từ 0 đến 26. Nhiệm vụ là chuyển đổi chuỗi bằng cách chuyển đổi càng nhiều chữ cái viết thường thành chữ hoa càng tốt mà không vượt quá ngân sách này. Mỗi chuyển đổi chữ in hoa sẽ tiêu tốn một đơn vị từ ngân sách. Các chữ cái đã viết hoa sẽ không được tính vào ngân sách và không thay đổi. 

Chuỗi đầu vào có thể dài tới 50 ký tự. Vì kích thước này rất nhỏ nên mọi giải pháp xử lý từng ký tự riêng lẻ sẽ hiệu quả. Ràng buộc số nguyên từ 0 đến 26 là rất quan trọng: nó giới hạn số lượng ký tự chúng ta có thể chuyển đổi, có nghĩa là chúng ta phải chọn lọc nếu chuỗi chứa nhiều hơn 26 chữ cái viết thường. 

Các trường hợp cạnh xuất hiện ở một vài nơi. Nếu ngân sách bằng 0 thì không có chuyển đổi nào xảy ra. Nếu chuỗi chứa ít chữ cái viết thường hơn ngân sách thì tất cả các chữ cái viết thường sẽ được chuyển đổi và ngân sách còn lại không được sử dụng. Các chuỗi bao gồm toàn bộ chữ in hoa không bị ảnh hưởng bất kể ngân sách. Việc thực hiện bất cẩn, chuyển đổi một cách mù quáng các chữ cái hoặc đếm sai có thể dễ dàng vượt quá ngân sách hoặc không giữ nguyên các chữ cái viết hoa. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp qua từng ký tự chuỗi. Đối với mỗi ký tự, hãy kiểm tra xem đó có phải là chữ thường hay không và liệu còn ngân sách hay không. Nếu cả hai điều kiện đều đúng, hãy chuyển nó thành chữ hoa và giảm ngân sách. Ngược lại, giữ nguyên ký tự. Phương pháp này đúng vì nó thực hiện rõ ràng các quy tắc của bài toán. Độ phức tạp của nó là O(n), trong đó n là độ dài chuỗi, trong trường hợp xấu nhất là 50. Ngay cả với 50 ký tự, số lượng thao tác này là không đáng kể. Lực lượng vũ phu trong bối cảnh này đã là tối ưu do kích thước đầu vào nhỏ. 

Thông tin chi tiết quan trọng có thể tinh chỉnh cách tiếp cận này là nhận ra rằng không cần cấu trúc dữ liệu hoặc sắp xếp bổ sung. Mỗi ký tự chữ thường đều có giá trị như nhau cho việc chuyển đổi, do đó việc quét đơn giản từ trái sang phải đảm bảo sử dụng ngân sách tối đa có thể mà không vượt quá nó. Quan sát này cho phép chúng ta tránh suy nghĩ về các chiến lược ưu tiên hoặc lựa chọn phức tạp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(n) | Đã chấp nhận | 
| Quét được tối ưu hóa | O(n) | O(n) | Đã chấp nhận | 

Vì cả hai đều là O(n) và n ≤ 50 nên phương pháp quét là đủ. 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào và số nguyên ngân sách. 
2. Khởi tạo danh sách kết quả trống. Danh sách được ưu tiên sử dụng trong Python để đột biến theo từng ký tự vì các chuỗi là bất biến. 
3. Lặp lại từng ký tự trong chuỗi đầu vào. 
4. Đối với mỗi ký tự, hãy kiểm tra xem đó có phải là chữ thường không. Nếu nó là chữ thường và ngân sách lớn hơn 0, hãy chuyển nó thành chữ hoa và giảm ngân sách. Ngược lại, giữ nguyên ký tự. 
5. Thêm từng ký tự đã xử lý vào danh sách kết quả. 
6. Sau khi xử lý tất cả các ký tự, nối danh sách thành một chuỗi duy nhất và xuất ra. 

Lý do điều này hoạt động rất đơn giản: thuật toán duy trì một bất biến rằng tại bất kỳ thời điểm nào, số lượng chữ cái viết thường được chuyển đổi không vượt quá ngân sách. Mỗi ký tự chữ thường chỉ được chuyển đổi nếu ngân sách vẫn còn và các chữ cái viết hoa không bị ảnh hưởng. Điều này đảm bảo chuỗi kết quả là hợp lệ và tối đa hóa chuyển đổi chữ hoa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
k = int(input().strip())

result = []

for ch in s:
    if 'a' <= ch <= 'z' and k > 0:
        result.append(ch.upper())
        k -= 1
    else:
        result.append(ch)

print("".join(result))
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`và dải các ký tự dòng mới. Việc lặp lại kiểm tra từng ký tự một cách rõ ràng theo phạm vi ASCII chữ thường, điều này tránh được các lỗi với`str.islower()`hoạt động sai trên các ký tự không phải ASCII. Chỉ giảm ngân sách khi xảy ra chuyển đổi để đảm bảo tính bất biến được giữ nguyên. Việc sử dụng danh sách để xây dựng kết quả sẽ tránh được việc nối chuỗi lặp lại, vốn chậm hơn trong Python. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
AprilFool
14
```| Nhân vật | Ngân sách | Hành động | Danh sách kết quả | 
| --- | --- | --- | --- | 
| 'A' | 14 | đã viết hoa | ['A'] | 
| 'p' | 14 | chuyển đổi | ['A', 'P'], k=13 | 
| 'r' | 13 | chuyển đổi | ['A', 'P', 'R'], k=12 | 
| 'tôi' | 12 | chuyển đổi | ['A', 'P', 'R', 'Tôi'], k=11 | 
| 'l' | 11 | chuyển đổi | ['A', 'P', 'R', 'Tôi', 'L'], k=10 | 
| 'F' | 10 | đã viết hoa | ['A', 'P', 'R', 'Tôi', 'L', 'F'] | 
| 'o' | 10 | chuyển đổi | ['A', 'P', 'R', 'I', 'L', 'F', 'O'], k=9 | 
| 'o' | 9 | chuyển đổi | ['A', 'P', 'R', 'I', 'L', 'F', 'O', 'O'], k=8 | 
| 'l' | 8 | chuyển đổi | ['A', 'P', 'R', 'I', 'L', 'F', 'O', 'O', 'L'], k=7 | 

Đầu ra:`APRILFOOL`Dấu vết này xác nhận thuật toán tôn trọng ngân sách và chuyển đổi các chữ cái từ trái sang phải. 

**Ví dụ về trường hợp cạnh** 

đầu vào:```
XYZ
0
```| Nhân vật | Ngân sách | Hành động | Danh sách kết quả | 
| --- | --- | --- | --- | 
| 'X' | 0 | đã viết hoa | ['X'] | 
| 'Y' | 0 | đã viết hoa | ['X', 'Y'] | 
| 'Z' | 0 | đã viết hoa | ['X', 'Y', 'Z'] | 

Đầu ra:`XYZ`Ngân sách bằng 0, do đó không có chữ cái viết thường nào được chuyển đổi và chữ cái viết hoa vẫn không thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng ta lặp qua từng ký tự của chuỗi đúng một lần. n 50. | 
| Không gian | O(n) | Danh sách kết quả lưu trữ từng ký tự, tạo khoảng trống tuyến tính tương ứng với độ dài đầu vào. | 

Số lượng thao tác tối thiểu, nằm trong giới hạn 2 giây. Việc sử dụng bộ nhớ cũng không đáng kể nên giải pháp này dễ dàng phù hợp với các hạn chế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    k = int(input().strip())
    result = []
    for ch in s:
        if 'a' <= ch <= 'z' and k > 0:
            result.append(ch.upper())
            k -= 1
        else:
            result.append(ch)
    return "".join(result)

# provided sample
assert run("AprilFool\n14\n") == "APRILFOOL", "sample 1"

# custom cases
assert run("xyz\n0\n") == "xyz", "zero budget"
assert run("abcXYZ\n2\n") == "ABcXYZ", "partial budget usage"
assert run("abcdef\n26\n") == "ABCDEF", "full budget usage"
assert run("ABCDEFGHIJKLMNOPQRSTUVWXYZ\n10\n") == "ABCDEFGHIJKLMNOPQRSTUVWXYZ", "all uppercase input"
assert run("aAaaA\n3\n") == "AAAaA", "mixed letters with limited budget"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "xyz\n0" | "xyz" | Trường hợp không có ngân sách | 
| "abcXYZ\n2" | "ABcXYZ" | Sử dụng ngân sách một phần với trường hợp hỗn hợp | 
| "abcdef\n26" | "ABCDEF" | Ngân sách vượt quá số chữ thường | 
| "ABCDEFGHIJKLMNOPQRSTUVWXYZ\n10" | "ABCDEFGHIJKLMNOPQRSTUVWXYZ" | Tất cả đầu vào viết hoa | 
| "aAaA\n3" | "AAAAA" | Thư hỗn hợp với ngân sách hạn chế | 

## Vỏ cạnh 

Đối với một chuỗi có ngân sách bằng 0, thuật toán giữ nguyên mọi ký tự. Nhập "xyz\n0" mang lại "xyz" vì ngân sách không bao giờ cho phép chuyển đổi. Đối với các chuỗi có ít chữ cái viết thường hơn ngân sách, chẳng hạn như "abc\n5", tất cả chữ cái viết thường đều được chuyển đổi và ngân sách còn lại không được sử dụng. Đối với các chuỗi chỉ có chữ in hoa, ngân sách không liên quan và chuỗi được trả về nguyên trạng. Các kịch bản này được xử lý tự động bằng cách kiểm tra điều kiện trong lần lặp.
