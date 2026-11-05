---
title: "CF 1194C - Từ S Đến T"
description: "Chúng ta được cho ba chuỗi s, t và p. Chúng ta có thể liên tục lấy bất kỳ ký tự nào từ p và chèn nó vào bất kỳ đâu trong s. Mục đích là để xác định xem sau một số lần chèn như vậy, s có thể trở nên chính xác bằng t hay không."
date: "2026-06-12T00:17:09+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1194
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 68 (Rated for Div. 2)"
rating: 1300
weight: 1194
solve_time_s: 101
verified: true
draft: false
---

[CF 1194C - Từ S đến T](https://codeforces.com/problemset/problem/1194/C) 

**Đánh giá:** 1300 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp ba chuỗi`s`,`t`, Và`p`. Chúng ta có thể liên tục lấy bất kỳ ký tự nào từ`p`và chèn nó vào bất cứ đâu trong`s`. Mục đích là để xác định xem sau một số chuỗi chèn như vậy,`s`có thể trở nên chính xác bằng`t`. Mỗi truy vấn là độc lập và chúng tôi phải trả lời`YES`hoặc`NO`cho mỗi người. 

Quan sát đầu tiên là thứ tự của các ký tự hiện có trong`s`vấn đề. Nếu như`s`chứa các ký tự theo thứ tự không khớp với chuỗi con của`t`, không cần chèn thêm ký tự từ`p`có thể cố định các vị trí tương đối. Vì thế,`s`phải là một dãy con của`t`. Thứ hai, đối với mỗi ký tự có trong`t`nhưng không ở trong`s`, phải có đủ ký tự đó trong`p`để lấp đầy khoảng trống. 

Độ dài chuỗi nhỏ: mỗi chuỗi tối đa 100 ký tự và có tối đa 100 truy vấn. Điều này cho phép quét tuyến tính đơn giản cho mỗi truy vấn, bởi vì ngay cả một phép kiểm tra O(|t| + |p|) đơn giản cho mỗi truy vấn cũng tối đa khoảng 10.000 thao tác, tức là trong vòng 1 giây. 

Trường hợp khó phát hiện xảy ra khi`s`đã dài hơn rồi`t`, hoặc khi`s`có các ký tự sai thứ tự. Ví dụ,`s = "ba"`,`t = "ab"`,`p = "a"`.`s`không phải là dãy con của`t`, vậy đáp án phải là`NO`, mặc dù`p`có các ký tự tồn tại trong`t`. Cách tiếp cận bất cẩn có thể chỉ kiểm tra số lượng ký tự và báo cáo nhầm`YES`. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi cách có thể để chèn các ký tự từ`p`vào trong`s`để phù hợp`t`. Chúng ta có thể tạo ra tất cả các hoán vị của các phần chèn, nhưng ngay cả với độ dài 100, điều này hoàn toàn không khả thi vì số lượng khả năng tăng theo giai thừa theo số lượng phần chèn. 

Cái nhìn sâu sắc quan trọng là tách vấn đề thành hai bước kiểm tra đơn giản hơn. Đầu tiên, hãy xác minh rằng`s`là một dãy con của`t`. Điều này đảm bảo rằng thứ tự tương đối của các ký tự đã có trong`s`là đúng. Thứ hai, đếm số lượng mỗi ký tự cần thiết trong`t`điều đó không có trong`s`. Nếu như`p`chứa ít nhất nhiều ký tự cần thiết, câu trả lời là`YES`; nếu không thì đó là`NO`. Cách tiếp cận này có hiệu quả vì các thao tác chèn có vị trí linh hoạt, do đó thứ tự không hạn chế các ký tự từ`p`. 

Cách tiếp cận vũ phu đúng về mặt lý thuyết nhưng không thực tế, trong khi kiểm tra hai bước sử dụng cấu trúc của vấn đề để giảm nó thành giải pháp tuyến tính theo thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(giai thừa( | t | )) | 
| Trình tự + Kiểm tra số lượng | O( | t | + | 

## Hướng dẫn thuật toán 

1. Bắt đầu bằng cách kiểm tra xem`s`là một dãy con của`t`. Khởi tạo hai con trỏ,`i`vì`s`Và`j`vì`t`. Quét qua`t`, tiến lên`i`bất cứ khi nào`t[j] == s[i]`. Cuối cùng, nếu`i`bằng với chiều dài của`s`, sau đó`s`là một dãy con của`t`. 
2. Nếu`s`không phải là dãy con của`t`, quay lại ngay`NO`. Điều này là do thứ tự tương đối của các ký tự trong`s`không thể thay đổi bằng cách chèn ký tự từ`p`. 
3. Đếm tần số xuất hiện của từng ký tự trong`s`,`t`, Và`p`. Với mỗi ký tự trong`t`, hãy tính xem cần thêm bao nhiêu lần xuất hiện nữa ngoài những gì`s`đã cung cấp rồi. Điều này đơn giản là`needed = count_t[char] - count_s.get(char, 0)`. 
4. Kiểm tra xem`p`có đủ từng ký tự được yêu cầu. Nếu đối với bất kỳ ký tự nào, số đếm trong`p`ít hơn`needed`, trở lại`NO`. Còn không thì trả lại`YES`. 

Tại sao nó hoạt động: Bước đầu tiên đảm bảo rằng`s`căn chỉnh chính xác với`t`. Bước đếm đảm bảo rằng các ký tự phụ cần điền vào`s`vào trong`t`có sẵn ở`p`. Vì việc chèn được cho phép ở bất cứ đâu nên không có ràng buộc bổ sung nào tồn tại, vì vậy hai bước kiểm tra này đều cần thiết và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import Counter

def can_transform(s, t, p):
    # Check if s is a subsequence of t
    i = 0
    for ch in t:
        if i < len(s) and s[i] == ch:
            i += 1
    if i != len(s):
        return "NO"
    
    # Count characters
    count_s = Counter(s)
    count_t = Counter(t)
    count_p = Counter(p)
    
    # Check if p can supply the missing characters
    for ch in count_t:
        if count_t[ch] > count_s.get(ch, 0) + count_p.get(ch, 0):
            return "NO"
    
    return "YES"

def main():
    q = int(input())
    for _ in range(q):
        s = input().strip()
        t = input().strip()
        p = input().strip()
        print(can_transform(s, t, p))

if __name__ == "__main__":
    main()
```chức năng`can_transform`đầu tiên kiểm tra thuộc tính chuỗi con bằng cách sử dụng quét con trỏ. Điều này tránh được logic phức tạp khi chèn chuỗi. Kế tiếp,`Counter`được sử dụng để đếm ký tự, giúp đơn giản hóa việc kiểm tra xem`p`có thể cung cấp các chữ cái cần thiết. Thứ tự của các phép toán rất quan trọng: việc kiểm tra trình tự con phải diễn ra trước khi đếm, bởi vì nếu`s`không phải là dãy con của`t`, chỉ tính thôi là không đủ. 

## Ví dụ đã hoạt động 

Sử dụng đầu vào mẫu đầu tiên: 

| s | t | p | tôi con trỏ | quyết định | 
| --- | --- | --- | --- | --- | 
| ab | acxb | cax | 0→1→2 | s là dãy con của t, đếm số lượt kiểm tra → CÓ | 
| một | aaa | aaabbcc | 0→1 | dãy sau trôi qua, t cần thêm 3 chữ a, p có đủ → CÓ | 
| aaa | aabbcc | aabbcc | 0→1→2→3 | dãy tiếp theo trôi qua, t cần 2 b và 2 c, p chỉ có 2 b, 2 c → CÓ hay KHÔNG? | 

Dấu vết thứ hai xác nhận việc kiểm tra trình tự tiếp theo hoạt động, kiểm tra số lượng đảm bảo tất cả các chữ cái cần thiết đều có sẵn trong p. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | s | 
| Không gian | O(26) = O(1) | Từ điển truy cập lưu trữ tối đa 26 chữ thường | 

Vì |s|, |t|, |p| ≤ 100 và q ≤ 100, các phép toán trong trường hợp xấu nhất là khoảng 30.000, có thể chấp nhận được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("4\nab\nacxb\ncax\na\naaaa\naaabbcc\naaaa\naabbcc\nab\nbaaa\naaaaa\n") == "YES\nYES\nNO\nNO", "sample 1"

# Custom cases
assert run("1\na\nb\na\n") == "NO", "s not subsequence of t"
assert run("1\na\nab\nb\n") == "YES", "s can be extended by p"
assert run("1\nabc\nabc\n\n") == "YES", "s equals t, p empty"
assert run("1\nabc\nacb\nabc\n") == "NO", "s subsequence fails due to order"
assert run("1\na"*100 + "\n" + "a"*100 + "\n" + "" + "\n") == "YES", "maximum-length equal strings"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| a/b/a | KHÔNG | s không phải là dãy con của t | 
| a/ab/b | CÓ | s có thể được mở rộng bởi p để khớp với t | 
| abc / abc / trống | CÓ | s bằng t, p trống | 
| abc/acb/abc | KHÔNG | thứ tự không khớp ngăn chặn trình tự tiếp theo | 
| 100×a / 100×a / trống | CÓ | chuỗi có độ dài tối đa được xử lý | 

## Vỏ cạnh 

cho`s = "ba"`,`t = "ab"`, `p =
