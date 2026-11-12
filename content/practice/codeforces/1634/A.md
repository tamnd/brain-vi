---
title: "CF 1634A - Đảo ngược và nối"
description: "Chúng ta được cho một chuỗi s có độ dài n và một số k. Chúng ta có thể thực hiện chính xác k thao tác trên s, trong đó mỗi thao tác hoặc nối chuỗi đảo ngược vào chính nó (s + rev(s)) hoặc thêm chuỗi đảo ngược (rev(s) + s)."
date: "2026-06-10T04:44:16+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1634
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 770 (Div. 2)"
rating: 800
weight: 1634
solve_time_s: 85
verified: false
draft: false
---

[CF 1634A - Đảo ngược và nối](https://codeforces.com/problemset/problem/1634/A) 

**Đánh giá:** 800 
**Tags:** tham lam, dây 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chuỗi`s`chiều dài`n`và một số`k`. Chúng tôi có thể thực hiện chính xác`k`hoạt động trên`s`, trong đó mỗi thao tác sẽ thêm chuỗi đảo ngược của chuỗi vào chính nó (`s + rev(s)`) hoặc chuẩn bị ngược lại (`rev(s) + s`). Nhiệm vụ là đếm xem có thể tạo ra bao nhiêu chuỗi riêng biệt sau khi thực hiện tất cả`k`hoạt động. Đầu ra cho mỗi trường hợp thử nghiệm là một số nguyên duy nhất biểu thị số lượng này. 

Các hạn chế là nhỏ:`n`lên tới 100,`k`lên tới 1000 và có tối đa 100 trường hợp thử nghiệm. Điều này loại trừ việc mô phỏng tất cả`2^k`trình tự các hoạt động một cách rõ ràng, bởi vì ngay cả đối với`k = 1000`, số lượng các chuỗi có thể là vô cùng lớn. Chúng ta cần một cách tiếp cận giải thích về cấu trúc của các chuỗi thay vì xây dựng từng chuỗi. 

Một trường hợp tinh tế xảy ra khi chuỗi gốc là một bảng màu. Ví dụ, nếu`s = "aba"`, sau đó`rev(s) = "aba"`cũng vậy. Sau bất kỳ số thao tác nào, thêm vào trước hoặc thêm vào`rev(s)`tạo ra cùng một chuỗi. Điều này thu gọn nhiều chuỗi hoạt động thành một chuỗi duy nhất. Một trường hợp cạnh khác là khi`k = 0`, trong trường hợp đó chúng ta chỉ trả về 1 vì không có thao tác nào thay đổi chuỗi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng mọi chuỗi thao tác: đối với mỗi thao tác, thêm vào trước hoặc nối thêm ngược lại và giữ một tập hợp tất cả các kết quả riêng biệt. Điều này làm việc cho nhỏ`k`, nếu không có`k = 1000`nó sẽ yêu cầu lưu trữ và tạo ra các chuỗi có độ dài lên tới`n * 2^k`, điều đó hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là số lượng chuỗi riêng biệt sau`k`các phép toán chỉ phụ thuộc vào việc chuỗi gốc có phải là chuỗi palindrome hay không. Nếu chuỗi đó là một chuỗi palindrome, việc thêm vào trước hoặc nối thêm chuỗi đảo ngược của nó sẽ tạo ra kết quả tương tự, do đó không có chuỗi riêng biệt mới nào xuất hiện. Nếu chuỗi không phải là một chuỗi palindrome, thì phép toán đầu tiên tạo ra hai kết quả riêng biệt và về nguyên tắc, mỗi phép toán tiếp theo sẽ tiếp tục nhân đôi. Tuy nhiên, do tính đối xứng, tất cả các thao tác tiếp theo chỉ tạo ra hai chuỗi duy nhất: một chuỗi bắt đầu bằng chuỗi gốc và một chuỗi bắt đầu bằng chuỗi đảo ngược. Do đó, đối với các máy không palindrome có`k > 0`, câu trả lời luôn là 2. Vì`k = 0`, câu trả lời là 1. 

Cái nhìn sâu sắc này làm giảm vấn đề thành một phép kiểm tra đơn giản đối với các palindromes và giá trị của`k`. Cách tiếp cận bạo lực không thành công vì nó cố gắng xây dựng các chuỗi có độ dài theo cấp số nhân, trong khi cách tiếp cận tối ưu tận dụng tính đối xứng trong các phép đảo ngược. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k * n * k) | O(2^k * n * k) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, hãy đọc`n`,`k`, và chuỗi`s`. 
3. Nếu`k = 0`, ngay lập tức trả về 1 vì không có thao tác nào được thực hiện. 
4. Kiểm tra xem`s`là một palindrome bằng cách so sánh nó với đảo ngược của nó. 
5. Nếu`s`là một palindrome, trả về 1 vì mọi chuỗi thao tác đều tạo ra cùng một chuỗi. 
6. Nếu`s`không phải là một bảng màu, trả về 2 vì thao tác đầu tiên tạo ra hai chuỗi riêng biệt và mọi thao tác tiếp theo đều giữ nguyên hai dạng riêng biệt này. 
7. In kết quả cho từng test. 

Tại sao nó hoạt động: Điều bất biến là khi thao tác đầu tiên được áp dụng cho một chuỗi không phải palindrome, nó sẽ tạo ra chính xác hai chuỗi riêng biệt: một chuỗi bắt đầu bằng chuỗi gốc và một chuỗi bắt đầu bằng chuỗi đảo ngược. Các hoạt động tiếp theo không thể tạo các chuỗi riêng biệt mới vì việc thêm vào hoặc thêm các đảo ngược chỉ hoán đổi giữa hai biểu mẫu này. Palindromes vẫn bất biến khi đảo ngược, do đó chỉ tồn tại một chuỗi duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        s = input().strip()
        if k == 0:
            print(1)
        elif s == s[::-1]:
            print(1)
        else:
            print(2)

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng cách đọc số lượng trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đọc độ dài, số lượng thao tác và chuỗi. Trường hợp đặc biệt`k = 0`được xử lý đầu tiên. Kiểm tra palindrome sử dụng kỹ thuật cắt Python, là O(n), đủ cho`n <= 100`. Logic cho các chuỗi không phải palindrome trực tiếp xuất ra 2, dựa trên thao tác đầu tiên tạo ra hai chuỗi riêng biệt. 

## Ví dụ đã hoạt động 

Mẫu 1:`s = "aab"`,`k = 2`. 

| Hoạt động | (Các) chuỗi | 
| --- | --- | 
| Ban đầu | aab | 
| Đầu tiên | aabbaa, baaaab | 
| Thứ hai | aabbaaaabbaa, baaaabbaaab | 

Sau 2 thao tác, tồn tại hai chuỗi riêng biệt. Thuật toán của chúng tôi đưa ra 2, khớp với dấu vết. 

Mẫu 2:`s = "abacaba"`,`k = 1`. 

| Hoạt động | (Các) chuỗi | 
| --- | --- | 
| Ban đầu | bàn tính | 
| Đầu tiên | abacabaabacaba | 

Từ`s`là một palindrome, phép toán đầu tiên chỉ mang lại một chuỗi. Đầu ra của thuật toán 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * t) | Mỗi chuỗi có độ dài n được kiểm tra một lần đối với palindrome, lặp lại đối với t trường hợp kiểm tra | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung ngoài việc đọc đầu vào và các biến tạm thời | 

Điều này nằm trong giới hạn, vì`n * t <= 100 * 100 = 10^4`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# Provided samples
assert run("4\n3 2\naab\n3 3\naab\n7 1\nabacaba\n2 0\nab") == "2\n2\n1\n1", "Sample tests"

# Custom tests
assert run("1\n1 5\na") == "1", "single character palindrome"
assert run("1\n2 0\nab") == "1", "k=0 case"
assert run("1\n3 1\nabc") == "2", "non-palindrome short string"
assert run("1\n5 100\nabcba") == "1", "palindrome with large k"
assert run("1\n4 10\nabca") == "2", "non-palindrome with large k"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "1\n1 5\na" | 1 | Bảng màu ký tự đơn | 
| "1\n2 0\nab" | 1 | k = 0 tạo ra một chuỗi | 
| "1\n3 1\nabc" | 2 | Hoạt động đầu tiên của chuỗi ngắn không phải palindrome | 
| "1\n5 100\nabcba" | 1 | Palindrome với k lớn vẫn còn một chuỗi | 
| "1\n4 10\nabca" | 2 | Non-palindrome với nhiều phép toán | 

## Vỏ cạnh 

Khi nào`k = 0`, thuật toán sẽ đưa ra đúng 1. Đối với`s = "aba"`, một bảng màu và`k > 0`, thuật toán cho kết quả là 1, vì mọi chuỗi thao tác đều tạo ra cùng một chuỗi`aba...aba`. Vì`s = "abc"`, một không phải palindrome, và`k = 1`, đầu ra là 2 vì hai chuỗi có thể có sau một thao tác là`"abccba"`Và`"cbaabc"`. Tất cả các tình huống này đều được xử lý chính xác bằng cách kiểm tra palindrome và`k == 0`tình trạng.
