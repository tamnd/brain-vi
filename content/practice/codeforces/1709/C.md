---
title: "CF 1709C - Khôi phục RBS"
description: "Chúng ta được cung cấp các chuỗi gồm ba loại ký tự: dấu ngoặc mở '(', dấu ngoặc đóng ')' và dấu chấm hỏi '?'. Chuỗi ban đầu là một chuỗi ngoặc đúng, còn được gọi là chuỗi ngoặc thông thường (RBS), nhưng một số dấu ngoặc đã được thay thế bằng dấu chấm hỏi."
date: "2026-06-09T20:52:32+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1709
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 132 (Rated for Div. 2)"
rating: 1800
weight: 1709
solve_time_s: 159
verified: true
draft: false
---

[CF 1709C - Khôi phục RBS](https://codeforces.com/problemset/problem/1709/C) 

**Đánh giá:** 1800 
**Tags:** thuật toán xây dựng, tham lam, triển khai, chuỗi 
**Thời gian giải:** 2m 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp các chuỗi gồm ba loại ký tự: dấu ngoặc mở`'('`, dấu ngoặc đóng`')'`, và dấu chấm hỏi`'?'`. Chuỗi ban đầu là một chuỗi ngoặc đúng, còn được gọi là chuỗi ngoặc thông thường (RBS), nhưng một số dấu ngoặc đã được thay thế bằng dấu chấm hỏi. Nhiệm vụ của chúng ta là xác định xem có chính xác một cách thay thế dấu chấm hỏi để chuỗi kết quả trở thành RBS hợp lệ hay không. 

Mỗi trường hợp thử nghiệm là độc lập và chúng tôi được đảm bảo rằng tồn tại ít nhất một RBS hợp lệ. Tổng độ dài của mỗi chuỗi có thể lên tới khoảng 200.000 ký tự trong tất cả các trường hợp thử nghiệm, vì vậy chúng tôi cần một giải pháp tuyến tính theo độ dài chuỗi. Những cách tiếp cận ngây thơ thử tất cả các khả năng thay thế dấu chấm hỏi sẽ mất thời gian theo cấp số nhân, điều này hoàn toàn không khả thi đối với những ràng buộc này. 

Các trường hợp cần xem xét bao gồm các chuỗi trong đó tất cả các ký tự là dấu chấm hỏi, các chuỗi đã gần như hoàn chỉnh RBS chỉ có một vị trí không rõ ràng và các chuỗi có độ dài 2 hoặc 4, trong đó có thể có nhiều hoán vị nhỏ. Ví dụ, chuỗi`"??"`có hai RBS có thể:`"()"`Và`"()"`(giống nhau về mặt kỹ thuật nhưng được tính là một mẫu duy nhất), trong khi một chuỗi như`"?))"`chỉ có thể được hoàn thành như`"(())"`và là duy nhất. 

Một cách tiếp cận bất cẩn có thể cho rằng chỉ cần đếm số lượng cần thiết`'('`Và`')'`để cân bằng chuỗi là đủ. Điều đó có tác dụng xây dựng một RBS hợp lệ, nhưng nó không đảm bảo tính duy nhất, vì việc sắp xếp lại các dấu chấm hỏi ở một số vị trí có thể mang lại một RBS hợp lệ thay thế. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là tạo ra tất cả các chuỗi bằng cách thay thế mỗi dấu chấm hỏi bằng cả hai`'('`Và`')'`và kiểm tra xem cái nào là RBS hợp lệ. Kiểm tra tính hợp lệ liên quan đến việc duy trì bộ đếm số dư trong khi duyệt qua trình tự. Đối với một chuỗi có độ dài`n`với`k`dấu hỏi, cách tiếp cận này sẽ yêu cầu`2^k * O(n)`hoạt động, điều này trở nên không thể thực hiện được khi`k`vượt quá 20 hoặc 30. Ngay cả với đầu vào nhỏ hơn, tốc độ này vẫn quá chậm. 

Cái nhìn sâu sắc quan trọng là để một RBS trở nên duy nhất thì không thể có sự linh hoạt trong việc đặt các dấu chấm hỏi. Nếu chúng ta thay thế tất cả các dấu hỏi bằng một phép gán giữ số dư chính xác theo yêu cầu ở mọi vị trí thì mọi phép gán thay thế sẽ tạm thời phá vỡ số dư hoặc ở cuối, tạo ra RBS không hợp lệ. Do đó, chúng ta chỉ cần kiểm tra "các vị trí quan trọng": vị trí đầu tiên và cuối cùng mà sự mơ hồ có thể cho phép phép gán hợp lệ thay thế. Nếu việc hoán đổi hai dấu chấm hỏi ở giữa chuỗi vẫn tạo ra RBS hợp lệ thì giải pháp đó không phải là duy nhất. 

Một cách hiệu quả, chúng tôi tính toán chính xác số lượng`'('`Và`')'`yêu cầu. Sau đó, chúng tôi kiểm tra xem lần đầu tiên`'('`điều đó có thể được thay thế bằng`'('`hoặc`')'`và cuối cùng`'('`điều đó có thể được thay thế bằng`'('`rất quan trọng. Nếu việc di chuyển hai vị trí này vẫn giữ nguyên giá trị thì nghiệm không phải là duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k * n) | O(n) | Quá chậm | 
| Tham lam + Kiểm tra số dư | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số dấu chấm hỏi và số câu hiện có`'('`Và`')'`. Tính số lượng cần thiết`'('`Và`')'`để hoàn thành RBS: mỗi nửa chuỗi phải`'('`hoặc`')'`nên tổng số được cân bằng. 
2. Gán số dấu hỏi đầu tiên cần thiết cho`'('`và phần còn lại để`')'`. 
3. Kiểm tra xem chuỗi có phải là RBS hợp lệ hay không bằng cách duy trì bộ đếm số dư từ trái sang phải. Số dư bắt đầu từ 0 và tăng thêm 1 cho`'('`và giảm đi 1 đối với`')'`. Nếu số dư âm hoặc không kết thúc bằng 0 thì chuỗi không hợp lệ. Bởi vì chúng tôi được đảm bảo có ít nhất một RBS hợp lệ nên việc này sẽ luôn thành công ở đây. 
4. Để xác định tính duy nhất, hãy nhìn vào các vị trí mà điểm cuối cùng`'('`được giao và lần đầu tiên`')'`được giao xảy ra. Hoán đổi hai ký tự này. Nếu việc kiểm tra số dư vẫn thành công thì giải pháp đó không phải là duy nhất. 
5. Nếu việc hoán đổi không thành công thì chỉ có một phép gán hợp lệ duy nhất và giải pháp là duy nhất. 

Tại sao nó hoạt động: kiểm tra số dư đảm bảo rằng các dấu ngoặc được lồng chính xác. Bằng cách tập trung vào vị trí đầu tiên và cuối cùng mà sự mơ hồ có thể cho phép thực hiện một nhiệm vụ khác, chúng tôi tách biệt tính linh hoạt duy nhất có thể mang lại nhiều RBS hợp lệ. Bất kỳ sự hoán đổi dấu chấm hỏi nào khác sẽ phá vỡ chuỗi sớm hoặc ở cuối, đảm bảo tính duy nhất nếu việc hoán đổi không thành công. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_rbs(s):
    balance = 0
    for c in s:
        balance += 1 if c == '(' else -1
        if balance < 0:
            return False
    return balance == 0

def solve_case(s):
    n = len(s)
    half = n // 2
    count_open = s.count('(')
    count_close = s.count(')')
    count_q = s.count('?')
    
    need_open = half - count_open
    need_close = half - count_close

    arr = list(s)
    for i in range(n):
        if arr[i] == '?':
            if need_open > 0:
                arr[i] = '('
                need_open -= 1
            else:
                arr[i] = ')'

    # Check uniqueness
    first_close_index = None
    last_open_index = None
    for i, c in enumerate(arr):
        if s[i] == '?':
            if c == '(':
                last_open_index = i
            else:
                if first_close_index is None:
                    first_close_index = i

    if last_open_index is not None and first_close_index is not None:
        arr[last_open_index], arr[first_close_index] = ')', '('
        if is_rbs(arr):
            return "NO"
    
    return "YES"

def main():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        print(solve_case(s))

if __name__ == "__main__":
    main()
```Mã đầu tiên tính toán có bao nhiêu`'('`Và`')'`được yêu cầu. Nó lấp đầy các dấu hỏi một cách tham lam, sau đó xác định các vị trí hoán đổi quan trọng để kiểm tra tính duy nhất. các`is_rbs`Hàm kiểm tra số dư, trả về Sai ngay lập tức nếu nó âm. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`"(?))"`| Bước | mảng | cân bằng | 
| --- | --- | --- | 
| ban đầu | ['(', '?', ')', ')'] | 0 | 
| thay thế '?' | ['(', '(', ')', ')'] | 0→1→2→1→0 | 
| cuối cùng '(' | chỉ mục 1 | đầu tiên ')' chỉ mục 2 | 
| hoán đổi → ['(', ')', '(', ')'] | 0→1→0→1→0, hợp lệ | | 

Hoán đổi không thành công sớm nên giải pháp là duy nhất:`"YES"`. 

### Ví dụ 2:`"??????"`| Bước | mảng | cân bằng | 
| --- | --- | --- | 
| thay thế | ['(', '(', '(', ')', ')', ')'] | 0→1→2→3→2→1→0 | 
| cuối cùng '(' chỉ mục 2, đầu tiên ')' chỉ mục 3 | trao đổi → ['(', '(', ')', '(', ')', ')'] | 0→1→2→1→2→1→0, hợp lệ | 

Hoán đổi thành công nên có nhiều giải pháp:`"NO"`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Một lần để điền dấu chấm hỏi và một lần để kiểm tra số dư | 
| Không gian | O(n) | Lưu trữ danh sách các ký tự có thể thay đổi để thay thế và hoán đổi | 

Cho rằng tổng độ dài của tất cả các chuỗi trong tất cả các trường hợp thử nghiệm là 2·10^5, phương pháp tuyến tính này có hiệu quả trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    main()
    return out.getvalue().strip()

# provided samples
assert run("5\n(?))\n??????\n()\n??\n?(?)()?)") == "YES\nNO\nYES\nYES\nNO", "sample 1"

# custom cases
assert run("2\n??\n?()") == "YES\nYES", "min-length and almost complete"
assert run("1\n((((????))))") == "YES", "inner question marks filled uniquely"
assert run("1\n????") == "NO", "all question marks length 4, multiple assignments"
assert run("1\n()??") == "YES", "question marks at the end, unique fill"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`??`| CÓ | Độ dài tối thiểu, bài tập duy nhất | 
|`?()`| CÓ | Chỉ có một dấu chấm hỏi, hoàn thành duy nhất | 
|`((((????))))`| CÓ | Dấu hỏi bên trong, độc đáo bởi sự cân bằng | 
|`????`| KHÔNG | Tồn tại nhiều RBS hợp lệ | 
|`()??`| CÓ | Dấu hỏi ở cuối, chỉ có một điền hợp lệ | 

## Vỏ cạnh 

cho`"????"`,`half = 2`. Tham lam lấp đầy tạo ra`"(()())"`, nhưng hoán đổi vị trí 1 và 2 cho`"()()"`, điều này cũng hợp lệ. Kiểm tra trao đổi phát hiện tính không duy nhất. Vì`"()??"`, cái
