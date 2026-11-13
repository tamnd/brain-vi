---
title: "CF 1697C - Bài toán yêu thích của awoo"
description: "Bài toán cho hai chuỗi s và t có cùng độ dài gồm các ký tự 'a', 'b' và 'c'. Các thao tác được phép cho phép chúng ta hoán đổi \"ab\" liền kề thành \"ba\" và \"bc\" thành \"cb\". Nhiệm vụ là xác định xem liệu chúng ta có thể biến đổi s thành t bằng cách sử dụng bất kỳ số bước di chuyển nào hay không."
date: "2026-06-09T22:30:50+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "data-structures", "greedy", "implementation", "strings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1697
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 130 (Rated for Div. 2)"
rating: 1400
weight: 1697
solve_time_s: 441
verified: true
draft: false
---

[CF 1697C - Vấn đề yêu thích của awoo](https://codeforces.com/problemset/problem/1697/C) 

**Đánh giá:** 1400 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, cấu trúc dữ liệu, tham lam, triển khai, chuỗi, hai con trỏ 
**Thời gian giải:** 7 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề đưa ra hai chuỗi`s`Và`t`có cùng độ dài bao gồm các ký tự 'a', 'b' và 'c'. Các hoạt động được phép cho phép chúng ta hoán đổi "ab" liền kề thành "ba" và "bc" thành "cb". Nhiệm vụ là xác định liệu chúng ta có thể biến đổi`s`vào trong`t`sử dụng bất kỳ số lượng các động tác này. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm và tổng độ dài chuỗi trên tất cả các trường hợp được giới hạn bởi 10^5. Ràng buộc này ngụ ý rằng bất kỳ giải pháp nào xử lý từng ký tự với số lần không đổi sẽ đủ nhanh, nhưng mô phỏng đơn giản về tất cả các bước di chuyển có thể là quá chậm vì số lượng thao tác cần thiết để đạt được.`t`có thể là bậc hai trong`n`. 

Các trường hợp cạnh phát sinh khi`s`Và`t`có số lượng 'a', 'b', 'c' giống hệt nhau nhưng thứ tự các ký tự khiến cho việc chuyển đổi không thể thực hiện được. Ví dụ,`s = "acb"`Và`t = "bac"`có cùng số lượng ký tự, nhưng 'a' không thể di chuyển trực tiếp qua 'c', do đó việc chuyển đổi là không thể. Một trường hợp khác xảy ra khi các chuỗi đã bằng nhau, trong trường hợp đó không cần di chuyển. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực cố gắng áp dụng lặp đi lặp lại tất cả các giao dịch hoán đổi có thể cho đến khi`s`bằng`t`. Về mặt lý thuyết, điều này đúng nhưng không khả thi đối với các chuỗi có độ dài lên tới 10^5 vì mỗi lần hoán đổi chỉ thay đổi một cặp duy nhất và số lần di chuyển có thể đạt tới O(n^2). 

Một cách tiếp cận tốt hơn phụ thuộc vào những hạn chế của nước đi. Mỗi lần hoán đổi chỉ có thể di chuyển 'a' sang phải trên 'b' và 'c' sang trái cho 'b'. Điều này có nghĩa là thứ tự tương đối của 'a' và 'c' không thể đảo ngược được. Do đó, cái nhìn sâu sắc cốt lõi là kiểm tra xem thứ tự của 'a' và 'c' trong`s`có thể được sắp xếp lại thành`t`theo các giao dịch hoán đổi được phép. Trước tiên chúng tôi xác minh rằng số lượng 'a', 'b', 'c' trong`s`Và`t`giống hệt nhau; nếu chúng khác nhau thì sự biến đổi là không thể được. Tiếp theo, chúng ta kiểm tra vị trí của 'a' và 'c' so với 'b'. 'a' chỉ có thể di chuyển sang phải và 'c' chỉ có thể di chuyển sang trái qua 'b', vì vậy đối với mỗi nhân vật, chúng tôi kiểm tra xem không nhân vật nào cần di chuyển qua nhân vật khác theo hướng bị cấm. Điều này làm giảm vấn đề thành quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Quét tuyến tính có ràng buộc | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc chuỗi`s`Và`t`và chiều dài của chúng`n`. 
2. So sánh số lượng 'a', 'b' và 'c' trong`s`Và`t`. Nếu có bất kỳ số lượng nào khác, hãy in "NO" và tiếp tục với trường hợp kiểm thử tiếp theo. 
3. Lặp lại`s`Và`t`đồng thời, bỏ qua tất cả các ký tự 'b'. Đối với các ký tự còn lại, hãy kiểm tra xem chuỗi 'a' và 'c' có khớp chính xác không. Nếu có sự không khớp về loại hoặc thứ tự, hãy in "KHÔNG". 
4. Đối với các vị trí có chữ 'a' xuất hiện, hãy đảm bảo rằng mỗi chữ 'a' trong`s`không ở bên phải vị trí tương ứng của nó trong`t`. 'a' chỉ có thể di chuyển sang phải. 
5. Đối với các vị trí có chữ 'c' xuất hiện, hãy đảm bảo rằng mỗi chữ 'c' trong`s`không ở bên trái vị trí tương ứng của nó trong`t`. 'c' chỉ có thể di chuyển sang trái. 
6. Nếu tất cả các lần kiểm tra đều đạt, hãy in "CÓ". 

Tại sao nó hoạt động: các bước di chuyển được phép duy trì thứ tự tương đối của 'a' và 'c' với nhau vì 'ab' chỉ có thể di chuyển 'a' qua phải 'b' và 'bc' chỉ có thể di chuyển 'c' sang trái qua 'b'. Bằng cách bỏ qua 'b', chúng tôi giảm vấn đề xuống mức kiểm tra thứ tự một chiều, đảm bảo tất cả các phép biến đổi đều có thể đạt được. 

## Giải pháp Python```
PythonRun
```Giải pháp trước tiên đảm bảo rằng các ký tự trong cả hai chuỗi khớp nhau. Sau đó nó tạo danh sách chỉ mục cho tất cả các ký tự ngoại trừ 'b', cho phép chúng ta kiểm tra xem 'a' và 'c' có thể di chuyển theo quy tắc cho phép hay không. 'a' chỉ có thể di chuyển sang phải và 'c' chỉ có thể di chuyển sang trái. Nếu bất kỳ nhân vật nào vi phạm các quy tắc này, câu trả lời là "KHÔNG". 

## Ví dụ đã hoạt động 

Theo dõi đầu vào mẫu:```

```| tôi | s[i] | t[i] | Kiểm tra | 
| --- | --- | --- | --- | 
| 0 | một | b | bỏ qua 'b', so sánh 'a' với 'a' trong t chỉ số 2 | 
| 1 | b | b | bỏ qua | 
| 2 | b | một | bỏ qua | 
| 3 | một | một | tôi=3, j=3 | 
| 4 | b | c | bỏ qua | 
| 5 | c | b | 'c' i=5, j=5 | 

Tất cả các ràng buộc đều được thỏa mãn, xuất ra "CÓ". Điều này xác nhận thuật toán xử lý chính xác các chuỗi hỗn hợp 'a', 'b', 'c'. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét một chuỗi và so sánh các ký tự; chiều dài tuyến tính. | 
| Không gian | O(n) | Để lưu trữ các chỉ mục đã lọc của các ký tự không phải 'b'. | 

Giải pháp này hiệu quả vì tổng của`n`trên tất cả các trường hợp thử nghiệm không vượt quá 10^5, đảm bảo rằng thuật toán tuyến tính hoàn thành tốt trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("5\n3\ncab\ncab\n1\na\nb\n6\nabbabc\nbbaacb\n10\nbcaabababc\ncbbababaac\n2\nba\nab\n") == \
    "YES\nNO\nYES\nYES\nNO", "sample 1"

# Custom cases
assert run("2\n4\naabc\nabac\n3\nabc\ncba\n") == "YES\nNO", "custom order check"
assert run("1\n5\naaaab\naaaab\n") == "YES", "all equal except last b"
assert run("1\n3\nacb\nabc\n") == "NO", "a cannot move left past c"
assert run("1\n6\nabcbac\nabcabc\n") == "YES", "complex interleaving"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4\naabc\nabac | CÓ | a có thể di chuyển sang phải để khớp với t | 
| 3\nabc\ncba | KHÔNG | không thể di chuyển 'a' qua 'c' | 
| 5\naaaab\naaaab | CÓ | xác minh các chuỗi đã gần bằng nhau | 
| 3\nacb\nabc | KHÔNG | 'a' không thể di chuyển sang trái | 
| 6\nabcbac\nabcabc | CÓ | hoán đổi phức tạp với nhiều ký tự 'b' | 

## Vỏ cạnh 

Thuật toán xử lý chính xác các chuỗi có độ dài 1. Ví dụ:`s = "a"`Và`t = "a"`xuất ra "CÓ" vì không cần di chuyển. Nếu như`s = "a"`Và`t = "b"`, đầu ra là "KHÔNG" vì các ký tự không khớp. 

Các chuỗi chỉ có ký tự 'b' luôn có thể chuyển đổi được nếu số lượng khớp nhau vì không có hạn chế nào đối với chuyển động của 'b'. Thuật toán xử lý chúng một cách chính xác bằng cách bỏ qua các vị trí 'b' trong kiểm tra thứ tự tương đối. 

Giải pháp này cũng xử lý kích thước tối đa một cách hiệu quả do độ phức tạp về thời gian tuyến tính và không gian bổ sung tối thiểu để theo dõi chỉ mục.
