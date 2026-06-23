---
title: "CF 105319F - Chúng tôi muốn một bài học"
description: "Chúng ta được cung cấp một chuỗi tin nhắn văn bản ngắn và đối với mỗi tin nhắn, chúng ta phải quyết định cách trả lời dựa trên một cụm từ đặc biệt."
date: "2026-06-22T12:01:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105319
codeforces_index: "F"
codeforces_contest_name: "Tishreen Collegiate Programming Contest 2024"
rating: 0
weight: 105319
solve_time_s: 48
verified: true
draft: false
---

[CF 105319F - Chúng tôi muốn có một bài học](https://codeforces.com/problemset/problem/105319/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi tin nhắn văn bản ngắn và đối với mỗi tin nhắn, chúng ta phải quyết định cách trả lời dựa trên một cụm từ đặc biệt. Mỗi chuỗi đến chỉ là một dòng chữ cái và nhiệm vụ là kiểm tra xem nó có khớp chính xác với một từ mục tiêu cố định hay không, bao gồm cả chữ hoa và ký tự. 

Nếu tin nhắn chính xác bằng chuỗi`BdnaDars`, câu trả lời phải là`Enough!`. Đối với mọi chuỗi có thể khác, phản hồi là`OK`. 

Kích thước đầu vào nhỏ, tối đa 1000 chuỗi và mỗi chuỗi có độ dài tối đa 100 ký tự. Điều này có nghĩa là chỉ cần so sánh trực tiếp từng chuỗi là quá đủ. Ngay cả một lần quét từng ký tự đơn giản cũng bị giới hạn bởi khoảng 100.000 lần kiểm tra ký tự, điều này không đáng kể trong một giới hạn thời gian thông thường. 

Chế độ lỗi tinh tế duy nhất xuất phát từ logic so sánh chuỗi không chính xác. Một vài ví dụ minh họa những gì có thể sai. Nếu một giải pháp chuẩn hóa chữ hoa hoặc cắt bớt ký tự thì`bdnadars`hoặc`BdnaDars `sẽ bị coi là trùng khớp một cách không chính xác mặc dù lẽ ra chúng không nên như vậy. Nếu một giải pháp chỉ so sánh các tiền tố thì`BdnaDarsX`có thể được chấp nhận một cách nhầm lẫn. Hành vi đúng đòi hỏi sự bình đẳng chính xác đầy đủ. 

## Phương pháp tiếp cận 

Phương pháp brute-force cũng là phương pháp tối ưu trong bài toán này. Đối với mỗi chuỗi đến, chúng tôi so sánh từng ký tự với chuỗi đích`BdnaDars`. Nếu tìm thấy bất kỳ sự không phù hợp nào, chúng tôi sẽ ngay lập tức phân loại nó là`OK`. Nếu chúng tôi hoàn thành việc so sánh mà không tìm thấy điểm không khớp và độ dài giống hệt nhau, chúng tôi sẽ xuất ra`Enough!`. 

Lý do điều này là đủ vì vấn đề xác định một mẫu chính xác duy nhất để phát hiện. Không có cấu trúc nào để khai thác ngoài việc kiểm tra tính bằng nhau và không cần xử lý trước hoặc băm. Công việc trong trường hợp xấu nhất là so sánh từng ký tự của mỗi chuỗi một lần, đưa ra tối đa 1000 chuỗi nhân với 100 ký tự, tức là chỉ có 100.000 so sánh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (so sánh trực tiếp) | O(n · L) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng chuỗi đầu vào một cách độc lập và quyết định đầu ra của nó dựa trên việc kiểm tra tính bằng nhau trực tiếp với cụm từ mục tiêu. 

1. Đọc số chuỗi n. Điều này quyết định chúng ta sẽ đưa ra bao nhiêu quyết định độc lập. 
2. Sửa chuỗi mục tiêu thành`BdnaDars`, vì mọi so sánh đều chống lại tham chiếu không đổi này. 
3. Đối với mỗi chuỗi đầu vào, hãy so sánh trực tiếp nó với chuỗi đích. Việc so sánh phải xem xét cả độ dài và sự bình đẳng giữa các ký tự. 
4. Nếu s chính xác bằng chuỗi đích, xuất ra`Enough!`, vì nó phù hợp với cụm từ bị cấm. 
5. Ngược lại, xuất ra`OK`, vì tất cả các chuỗi không khớp đều được xử lý thống nhất. 

Lựa chọn thiết kế quan trọng là coi sự bình đẳng như một điều kiện nguyên tử duy nhất. Điều này tránh logic khớp một phần có thể vô tình chấp nhận tiền tố hoặc biến thể chữ hoa chữ thường. 

### Tại sao nó hoạt động 

Thuật toán đúng vì đầu ra chỉ phụ thuộc vào việc chuỗi đầu vào có giống với một chuỗi tham chiếu cố định hay không. Sự bình đẳng của các chuỗi được xác định đầy đủ bằng cách khớp độ dài và ký tự khớp ở mọi chỉ mục. Vì thuật toán kiểm tra chính xác điều kiện đó nên mọi chuỗi đều được phân loại chính xác và không có hai chuỗi riêng biệt nào có thể tạo ra kết quả khớp không chính xác giống nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

TARGET = "BdnaDars"

def solve():
    n = int(input().strip())
    for _ in range(n):
        s = input().strip()
        if s == TARGET:
            print("Enough!")
        else:
            print("OK")

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả các chuỗi bằng cách sử dụng đầu vào nhanh và so sánh từng chuỗi với hằng số cố định. các`.strip()`rất quan trọng vì nó loại bỏ ký tự dòng mới ở cuối, điều này sẽ cản trở việc kiểm tra tính bằng nhau. 

Sự so sánh`s == TARGET`được triển khai hiệu quả trong Python và đoản mạch bên trong ở lần không khớp đầu tiên, làm cho nó trở nên tối ưu cho quy mô vấn đề này. Không cần vòng lặp ký tự thủ công hoặc băm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
Hi
BdnaDars
Bye
```Chúng tôi xử lý từng chuỗi theo thứ tự. 

| Bước | Chuỗi đầu vào | Kết quả so sánh | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | Xin chào | không bằng | được | 
| 2 | BdnaDars | bằng | Đủ! | 
| 3 | Tạm biệt | không bằng | được | 

Điều này cho thấy rằng chỉ có kết quả khớp chính xác mới kích hoạt phản hồi đặc biệt, trong khi tất cả các chuỗi khác được xử lý thống nhất bất kể sự giống nhau. 

### Ví dụ 2 

đầu vào:```
2
bdnadars
BdnaDarsX
```| Bước | Chuỗi đầu vào | Kết quả so sánh | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | bdnadars | trường hợp không khớp | được | 
| 2 | BdnaDarsX | nhân vật phụ | được | 

Dấu vết này nhấn mạnh rằng cả sự khác biệt về kiểu chữ và độ dài đều ngay lập tức phá vỡ sự bình đẳng, điều này là cần thiết để tránh các kết quả khớp không chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · L) | Mỗi chuỗi trong số n chuỗi được so sánh từng ký tự với độ dài L | 
| Không gian | O(1) | Chỉ sử dụng một chuỗi mục tiêu cố định và một bộ đệm đầu vào duy nhất | 

Các ràng buộc cho phép tối đa 1000 chuỗi có độ dài 100, do đó tổng số thao tác ký tự bị giới hạn bởi 100.000. Điều này nằm trong giới hạn thoải mái đối với Python, ngay cả khi so sánh chuỗi đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    TARGET = "BdnaDars"
    n = int(input().strip())
    for _ in range(n):
        s = input().strip()
        if s == TARGET:
            print("Enough!")
        else:
            print("OK")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# provided sample
assert run("2\nHi\nBdnaDars\n") == "OK\nEnough!", "sample 1"

# custom cases

# minimum size
assert run("1\nBdnaDars\n") == "Enough!", "exact match single case"

# all different
assert run("3\nA\nB\nC\n") == "OK\nOK\nOK", "no matches"

# case sensitivity check
assert run("2\nbdnaDars\nBdnaDars\n") == "OK\nEnough!", "case sensitivity"

# near match with extra character
assert run("1\nBdnaDarsX\n") == "OK", "extra suffix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\nBdnaDars | Đủ! | phần tử đơn khớp chính xác | 
| 3\nA\nB\nC | được rồi được được | từ chối thống nhất | 
| bdnaDars / BdnaDars | Được rồi/ Đủ rồi! | xử lý phân biệt chữ hoa chữ thường | 
| BdnaDarsX | được | ngăn chặn việc chấp nhận tiền tố | 

## Vỏ cạnh 

Một trường hợp quan trọng là phân biệt chữ hoa chữ thường. Chuỗi`bdnadars`không bao giờ được chấp nhận, mặc dù nó trông giống mục tiêu. Thuật toán từ chối nó một cách chính xác vì tính bằng nhau của chuỗi Python yêu cầu các ký tự khớp chính xác, bao gồm cả sự khác biệt giữa chữ hoa và chữ thường. 

Một trường hợp cạnh khác là các ký tự cuối bổ sung. Ví dụ,`BdnaDarsX`chia sẻ tiền tố với mục tiêu nhưng có độ dài dài hơn. Kiểm tra đẳng thức không thành công vì độ dài chuỗi khác nhau, do đó nó được phân loại an toàn là`OK`. 

Trường hợp cạnh thứ ba là kết quả khớp ranh giới chính xác trong đó đầu vào giống hệt với mục tiêu. Trong trường hợp đó, việc so sánh chỉ thành công sau khi xác minh tất cả các ký tự và đầu ra trở thành`Enough!`, đây là trường hợp đặc biệt duy nhất mà bài toán xác định.
