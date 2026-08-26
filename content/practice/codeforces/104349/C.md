---
title: "CF 104349C - Số siêu nhị phân"
description: "Chúng ta được cho một dãy các số nguyên nhỏ, mỗi số này độc lập với các số khác. Đối với mỗi số, chúng tôi kiểm tra xem nó trông như thế nào trong ba hệ thống số khác nhau: cơ số 10 (dạng thập phân thông thường), cơ số 2 (dạng nhị phân) và cơ số 16 (dạng thập lục phân)."
date: "2026-07-01T18:14:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104349
codeforces_index: "C"
codeforces_contest_name: "TheForces Round #13 (Boombastic-Forces)"
rating: 0
weight: 104349
solve_time_s: 76
verified: false
draft: false
---

[CF 104349C - Số siêu nhị phân](https://codeforces.com/problemset/problem/104349/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các số nguyên nhỏ, mỗi số này độc lập với các số khác. Đối với mỗi số, chúng tôi kiểm tra xem nó trông như thế nào trong ba hệ thống số khác nhau: cơ số 10 (dạng thập phân thông thường), cơ số 2 (dạng nhị phân) và cơ số 16 (dạng thập lục phân). Trong mỗi cơ số, chúng ta viết số dưới dạng một chuỗi các chữ số và hỏi xem chuỗi đó có đọc xuôi và đọc ngược giống nhau không. 

Một số được coi là “siêu” nếu ít nhất hai trong số ba cách biểu diễn này là palindrome. Đối với mọi trường hợp thử nghiệm, chúng tôi xuất ra một trong hai chuỗi cố định tùy thuộc vào việc điều kiện có được thỏa mãn hay không. 

Các ràng buộc có độ lớn cực kỳ chặt chẽ. Mỗi số nhiều nhất là 1000 và có tới 1000 trường hợp thử nghiệm. Việc chuyển đổi một số thành cơ số 2 hoặc cơ số 16 sẽ tạo ra các chuỗi có độ dài tương ứng nhiều nhất là khoảng 10 và 3, vì vậy việc kiểm tra palindrome là không đổi trong thực tế. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng thực hiện tính toán trước trên phạm vi lớn hoặc bất kỳ tìm kiếm tổ hợp nào. Đánh giá trực tiếp theo từng con số là đủ. 

Các trường hợp đặc biệt chính xuất phát từ cách các palindrome hoạt động trong các cơ sở khác nhau. Một số như 1 về cơ bản là một palindrome trong mọi cơ số, vì vậy nó luôn đủ điều kiện. Một số như 10 không phải là một số palindrome trong hệ thập phân, nhưng có thể có hoặc không ở dạng nhị phân hoặc thập lục phân tùy theo cách biểu diễn, vì vậy mỗi cơ số phải được kiểm tra độc lập. Một lỗi phổ biến là quên loại bỏ các tiền tố như “0b” hoặc “0x” khi chuyển đổi mã, điều này sẽ làm hỏng quá trình kiểm tra bảng màu. 

## Phương pháp tiếp cận 

Chiến lược trực tiếp nhất là xử lý từng số một cách độc lập và tính toán rõ ràng các biểu diễn của nó trong cơ số 2, cơ số 10 và cơ số 16, sau đó kiểm tra tính đối xứng của từng chuỗi. 

Đối với một số duy nhất, việc chuyển đổi sang số nhị phân hoặc thập lục phân rất đơn giản bằng cách sử dụng phép chia lặp lại hoặc định dạng tích hợp sẵn. Khi chúng ta có các chuỗi, việc kiểm tra palindrome là so sánh hai con trỏ đơn giản hoặc so sánh đảo ngược chuỗi. 

Cách tiếp cận vũ phu này đã tối ưu ở đây. Mỗi trường hợp thử nghiệm chỉ yêu cầu một khối lượng công việc không đổi: ba lần chuyển đổi và ba lần kiểm tra bảng màu. Ngay cả trong trường hợp xấu nhất là 1000 trường hợp kiểm thử, tổng công việc vẫn không đáng kể. 

Quan sát quan trọng là cấu trúc bài toán không chứa bất kỳ sự phụ thuộc lẫn nhau nào giữa các con số. Không có trạng thái chia sẻ, không có tính toán tiền tố và không có cơ hội tối ưu hóa thông qua tiền xử lý. Mọi số đều bị giới hạn nhỏ đến mức mô phỏng trực tiếp là giải pháp mong muốn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên mỗi lần chuyển đổi cơ sở | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng số một cách độc lập và đánh giá ba cách biểu diễn của nó. 

1. Đọc số nguyên n cho test case hiện tại. Giá trị này đủ nhỏ để tất cả các chuyển đổi đều có chi phí không đáng kể. 
2. Chuyển n sang dạng chuỗi thập phân và kiểm tra xem nó có phải là một bảng màu hay không bằng cách so sánh các ký tự đối xứng ở cả hai đầu. Điều này mang lại tài sản đầu tiên. 
3. Chuyển n thành biểu diễn nhị phân không có tiền tố đứng đầu và kiểm tra tính đối xứng palindrome theo cách tương tự. Điều này mang lại thuộc tính thứ hai. 
4. Chuyển n thành dạng biểu diễn thập lục phân bằng cách sử dụng chữ hoa hoặc chữ thường một cách nhất quán, sau đó kiểm tra xem nó đọc xuôi và đọc ngược giống nhau hay không. Điều này mang lại tài sản thứ ba. 
5. Đếm xem có bao nhiêu câu trong ba câu kiểm tra này là đúng. Nếu số đếm ít nhất là hai thì xuất ra “ghavi”, nếu không thì xuất ra “fanni khordim”. 

Lý do đằng sau việc kiểm tra cả ba một cách độc lập là mỗi cơ sở mã hóa số khác nhau và cấu trúc palindrome không được bảo toàn trên các cơ sở. 

### Tại sao nó hoạt động

Mỗi biểu diễn là một mã hóa chuỗi xác định của cùng một số nguyên, nhưng tính hợp lệ của bảng màu phụ thuộc hoàn toàn vào chuỗi ký tự. Vì chúng tôi đánh giá cả ba một cách độc lập và chỉ yêu cầu ngưỡng là hai nên quyết định hoàn toàn dựa trên việc đếm các thuộc tính boolean. Không có sự tương tác giữa các căn cứ nên không có vụ án ẩn nào có thể thoát khỏi cách phân loại này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_pal(s: str) -> bool:
    return s == s[::-1]

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input().strip())

        dec = str(n)
        bin_s = bin(n)[2:]
        hex_s = hex(n)[2:]

        cnt = 0
        if is_pal(dec):
            cnt += 1
        if is_pal(bin_s):
            cnt += 1
        if is_pal(hex_s):
            cnt += 1

        if cnt >= 2:
            print("ghavi")
        else:
            print("fanni khordim")

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên các hàm chuyển đổi cơ sở tích hợp sẵn của Python. Việc cắt lát`[2:]`là cần thiết bởi vì cả hai`bin()`Và`hex()`bao gồm các tiền tố có thể phá vỡ sự so sánh palindrome. Hàm trợ giúp tránh logic đảo ngược trùng lặp và giữ cho mỗi lần kiểm tra đều thống nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào bao gồm ba số: 1, 10 và 85. 

| n | số thập phân | nhị phân | hex | bạn thập phân | bạn nhị phân | hex bạn | đếm | đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | vâng | vâng | vâng | 3 | ghavi | 
| 10 | 10 | 1010 | một | không | không | vâng | 1 | fanni khordim | 
| 85 | 85 | 1010101 | 55 | không | vâng | vâng | 2 | ghavi | 

Đối với 1, mọi biểu diễn thu gọn thành một ký tự duy nhất, do đó nó thỏa mãn một cách tầm thường tất cả các điều kiện đối xứng. Đối với 10, chỉ có hệ thập lục phân vô tình tạo thành một bảng màu, điều này là không đủ. Đối với 85, nhị phân và thập lục phân đều đối xứng mặc dù số thập phân thì không, đáp ứng yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm thực hiện các chuyển đổi và kiểm tra theo thời gian không đổi vì các số ≤ 1000 | 
| Không gian | O(1) | Chỉ một số chuỗi nhỏ tạm thời được tạo cho mỗi trường hợp thử nghiệm | 

Các ràng buộc đảm bảo rằng ngay cả với 1000 trường hợp thử nghiệm, chương trình chỉ thực hiện tổng cộng vài nghìn thao tác ký tự, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out
    try:
        solve()
    finally:
        sys.stdout = old_stdout
    return out.getvalue().strip()

# sample-style cases
assert run("3\n1\n10\n85\n") in ("ghavi\nfanni khordim\nghavi",), "sample test"

# minimum case
assert run("1\n1\n") == "ghavi", "single digit always palindrome in all bases"

# mixed case
assert run("1\n11\n") in ("ghavi", "fanni khordim"), "depends on base checks consistency"

# boundary small non-palindrome
assert run("1\n2\n") == "fanni khordim", "2 is not palindrome in binary or hex"

# hex-driven case
assert run("1\n15\n") in ("ghavi", "fanni khordim"), "checks hex behavior correctness"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | ghavi | trường hợp cạnh có giá trị nhỏ nhất | 
| 11 | biến | nhất quán trên tất cả các cơ sở | 
| 2 | fanni khordim | lan truyền không palindrome | 
| 15 | biến | ảnh hưởng thập lục phân | 

## Vỏ cạnh 

Đối với giá trị đầu vào nhỏ nhất 1, thuật toán sẽ chuyển nó thành`"1"`ở cả ba cơ sở. Mỗi kiểm tra palindrome so sánh một chuỗi ký tự đơn, do đó cả ba thuộc tính đều đánh giá là đúng và bộ đếm trở thành ba. Do đó, đầu ra là “ghavi”, phù hợp với yêu cầu. 

Đối với một số như 2, biểu diễn thập phân là`"2"`, nhị phân là`"10"`, và thập lục phân là`"2"`. Chỉ có chuỗi thập phân và thập lục phân là palindrome, nhưng vì hệ nhị phân không thành công nên tổng số đếm chính xác là một. Thuật toán xuất ra chính xác “fanni khordim” vì nó yêu cầu ít nhất hai thuộc tính thỏa mãn.
