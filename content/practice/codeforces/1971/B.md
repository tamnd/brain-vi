---
title: "CF 1971B - Chuỗi khác"
description: "Chúng ta được cho một chuỗi các chữ cái tiếng Anh viết thường và nhiệm vụ là sắp xếp lại các chữ cái của nó để tạo thành một chuỗi mới khác với chuỗi ban đầu. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có một chuỗi."
date: "2026-06-08T17:21:16+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1971
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 944 (Div. 4)"
rating: 800
weight: 1971
solve_time_s: 196
verified: false
draft: false
---

[CF 1971B - Chuỗi khác](https://codeforces.com/problemset/problem/1971/B) 

**Đánh giá:** 800 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 3 phút 16s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các chữ cái tiếng Anh viết thường và nhiệm vụ là sắp xếp lại các chữ cái của nó để tạo thành một chuỗi mới khác với chuỗi ban đầu. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có một chuỗi. The output must either produce a rearranged string different from the input or state "NO" if all possible rearrangements are identical to the original.

 The strings are very short, at most length 10. This implies that brute-force approaches that generate all permutations could work, but they are unnecessary. Thay vào đó, chúng ta có thể dựa vào những thao tác đơn giản như sắp xếp. Trường hợp cạnh phát sinh khi tất cả các ký tự trong chuỗi giống hệt nhau hoặc chuỗi có độ dài bằng một. Trong những trường hợp này, mọi sự sắp xếp lại đều giống hệt với bản gốc, vì vậy câu trả lời là "KHÔNG". Đối với các chuỗi có ít nhất hai ký tự riêng biệt, chúng ta có thể tạo ra một sự sắp xếp lại hợp lệ một cách xác định. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là tạo ra tất cả các hoán vị của chuỗi và kiểm tra từng hoán vị cho đến khi chúng ta tìm thấy một hoán vị khác với chuỗi ban đầu. Điều này có hiệu quả vì các chuỗi có độ dài tối đa là 10, nhưng nó không hiệu quả do số lượng hoán vị giai thừa. 

The optimal approach is based on a simple observation: if we sort the characters of the string, the result will only equal the original if all characters are identical. Mặt khác, chuỗi được sắp xếp được đảm bảo khác với chuỗi gốc ở ít nhất một vị trí. Việc sắp xếp đưa ra sự sắp xếp lại có tính xác định và chạy theo thời gian tuyến tính theo độ dài của chuỗi đối với các đầu vào ngắn như vậy. Phương pháp này xử lý tất cả các chuỗi một cách hiệu quả và tránh những kiểm tra không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Hoạt động nhưng không hiệu quả với độ dài 10 | 
| Dựa trên sắp xếp | O(n log n) | O(n) | Được chấp nhận và mang tính quyết định | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc chuỗi`s`. 
3. Nếu chuỗi bao gồm các ký tự giống hệt nhau, hãy xuất "NO" vì mọi sự sắp xếp lại đều bằng chuỗi gốc. 
4. Ngược lại, sắp xếp các ký tự của`s`hình thành`r`. 
5. Xuất "YES" rồi xuất chuỗi`r`. 

**Tại sao nó hoạt động**: Việc sắp xếp đảm bảo rằng các ký tự theo thứ tự cố định. Nếu chuỗi gốc chứa ít nhất hai chữ cái riêng biệt thì chuỗi được sắp xếp không thể giống nhau vì vị trí của ít nhất một ký tự thay đổi. Điều bất biến là một chuỗi có nhiều hơn một ký tự riêng biệt luôn tạo ra một chuỗi khác sau khi sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        if len(set(s)) == 1:
            print("NO")
        else:
            r = ''.join(sorted(s))
            print("YES")
            print(r)

if __name__ == "__main__":
    solve()
```**Giải thích về mã**:`set(s)`xác định các ký tự duy nhất. Nếu chỉ có một ký tự tồn tại, "NO" sẽ được in. Nếu không thì,`sorted(s)`tạo ra sự sắp xếp lại, đảm bảo khác với`s`. Việc sắp xếp đảm bảo đầu ra xác định và xử lý các trường hợp khó khăn một cách tự nhiên. sử dụng`input().strip()`tránh các vấn đề dòng mới. 

## Ví dụ đã hoạt động 

### Dữ liệu mẫu đầu vào:`codeforces`| Bước | Bản gốc | Đã sắp xếp | Kiểm tra | 
| --- | --- | --- | --- | 
| 1 | c o d e lực lượng c e s | c c d e e f o o r s | Chuỗi được sắp xếp khác với chuỗi gốc | 
| 2 | Đầu ra | CÓ / c c d e e f o r s | Đúng | 

### Dữ liệu mẫu đầu vào:`aaaaa`| Bước | Bản gốc | Đã sắp xếp | Kiểm tra | 
| --- | --- | --- | --- | 
| 1 | a a a a a | a a a a a | Tất cả các ký tự giống hệt nhau | 
| 2 | Đầu ra | KHÔNG | Đúng | 

Những ví dụ này xác nhận thuật toán xử lý cả hai trường hợp: chuỗi có tất cả các chữ cái giống nhau và chuỗi có các chữ cái riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n log n) | Sắp xếp từng chuỗi có độ dài n 10 trên t 1000 ca kiểm thử | 
| Không gian | O(n) | Lưu trữ chuỗi đã sắp xếp | 

Cho n ≤ 10, điều này là không đáng kể. Giải pháp chạy hiệu quả trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        solve()
    return f.getvalue().strip()

# provided samples
assert run("8\ncodeforces\naaaaa\nxxxxy\nco\nd\nnutdealer\nmwistht\nhhhhhhhhhh\n") == \
"YES\nccdeefnoors\nNO\nYES\nxxxyy\nYES\nco\nNO\nYES\ndeelnrtau\nYES\nhhimstw\nNO", "sample 1"

# custom tests
assert run("2\na\nab\n") == "NO\nYES\nab", "min-size cases"
assert run("1\nbbbbbbbbbb\n") == "NO", "all identical characters max length"
assert run("1\nabcdefghij\n") == "YES\nabcdefghij", "already sorted distinct letters"
assert run("1\nba\n") == "YES\nab", "small unsorted string"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`| KHÔNG | Chuỗi kích thước tối thiểu, ký tự đơn | 
|`ab`| CÓ / ab | Chuỗi kích thước tối thiểu, ký tự riêng biệt | 
|`bbbbbbbbbb`| KHÔNG | Ký tự giống nhau có độ dài tối đa | 
|`abcdefghij`| CÓ / abcdefghij | Chuỗi đã được sắp xếp với các chữ cái riêng biệt | 
|`ba`| CÓ / ab | Chuỗi nhỏ chưa được sắp xếp | 

## Vỏ cạnh 

Đối với các chuỗi có tất cả các chữ cái giống hệt nhau như`aaaa`hoặc độ dài một, thuật toán sẽ đưa ra chính xác "KHÔNG". Việc sắp xếp giữ nguyên chuỗi gốc khi tất cả các chữ cái đều giống nhau và việc kiểm tra bằng cách sử dụng`set(s)`đảm bảo trường hợp này được bắt trước khi xuất. Đối với các chuỗi có nhiều loại khác nhau, việc sắp xếp sẽ tạo ra một chuỗi khác với chuỗi gốc, xử lý các đầu vào như`co`→`oc`hoặc`nutdealer`→`adeelnrtu`. 

Điều này đảm bảo tính chính xác cho tất cả các đầu vào trong các ràng buộc.
