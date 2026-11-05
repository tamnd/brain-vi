---
title: "CF 1200E - Nén Từ"
description: "Chúng ta được cung cấp một chuỗi các từ tạo thành một câu và nhiệm vụ là nén chúng thành một chuỗi bằng cách hợp nhất chúng từ trái sang phải."
date: "2026-06-11T23:54:24+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "hashing", "implementation", "string-suffix-structures", "strings"]
categories: ["algorithms"]
codeforces_contest: 1200
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 578 (Div. 2)"
rating: 2000
weight: 1200
solve_time_s: 77
verified: true
draft: false
---

[CF 1200E - Nén từ](https://codeforces.com/problemset/problem/1200/E) 

**Xếp hạng:** 2000 
**Tags:** brute Force, băm, triển khai, cấu trúc hậu tố chuỗi, chuỗi 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các từ tạo thành một câu và nhiệm vụ là nén chúng thành một chuỗi bằng cách hợp nhất chúng từ trái sang phải. Quy tắc hợp nhất là khi chúng ta thêm một từ vào chuỗi hiện tại, mọi tiền tố của từ mới khớp với hậu tố của chuỗi hiện tại sẽ bị xóa. Mục đích là tạo ra chuỗi nén cuối cùng sau khi xử lý tất cả các từ. 

Đầu vào bao gồm một số nguyên`n`cho số lượng từ, theo sau là các từ. Tổng độ dài của tất cả các từ cộng lại có thể lên tới 1 triệu ký tự. Điều này ngụ ý rằng một thuật toán hoạt động tỷ lệ thuận với tổng chiều dài của các ký tự, chẳng hạn như O(total_length), là khả thi, nhưng bất kỳ thuật toán nào có độ dài bậc hai (O(total_length²)) thì quá chậm. Điều này loại trừ các cách tiếp cận ngây thơ liên tục so sánh các hậu tố và tiền tố theo từng ký tự trên các chuỗi dài. 

Các trường hợp cạnh bao gồm các tình huống trong đó một từ hoàn toàn là tiền tố của từ tiếp theo hoặc hai từ chia sẻ các phần chồng chéo. Ví dụ như việc sáp nhập`"abc"`Và`"bcd"`nên sản xuất`"abcd"`. Việc triển khai ngây thơ luôn bổ sung mà không tính toán tiền tố hậu tố chồng chéo dài nhất sẽ tạo ra`"abcbcd"`, điều đó không đúng. Một trường hợp tinh tế khác là khi một từ giống hệt với chuỗi trước đó, trong đó phần trùng lặp bằng toàn bộ từ thứ hai và không nên thêm gì vào. 

## Phương pháp tiếp cận 

Một giải pháp brute-force lặp lại qua từng từ và đối với mỗi từ mới, hãy kiểm tra mọi hậu tố có thể có của chuỗi hiện tại để xem liệu nó có khớp với tiền tố của từ mới hay không. Điều này yêu cầu so sánh lên tới O(L²), trong đó L là độ dài của chuỗi tích lũy cho đến nay. Với tối đa 10^6 ký tự, điều này dẫn đến khoảng 10^12 thao tác trong trường hợp xấu nhất, rõ ràng là quá chậm. 

Cái nhìn sâu sắc quan trọng để tăng tốc độ này là nhận ra đây là một vấn đề chồng chéo chuỗi cổ điển, có thể giải quyết được bằng thuật toán Knuth-Morris-Pratt (KMP). Thay vì so sánh hậu tố và tiền tố theo cách thủ công, chúng ta có thể xây dựng một chuỗi tạm thời bằng cách ghép từ mới, dấu phân cách và phần cuối cùng của chuỗi tích lũy, sau đó tính hàm tiền tố (mảng lỗi KMP). Giá trị cuối cùng của hàm tiền tố cho chúng ta biết độ dài của phần chồng lấp tối đa. Bằng cách chỉ thêm hậu tố không chồng chéo của từ mới, chúng ta có thể ghép các từ một cách hiệu quả. Điều này làm giảm tổng công việc xuống O(total_length) vì mỗi ký tự được xử lý một số lần không đổi khi tính toán hàm tiền tố. 

Câu chuyện là brute-force hoạt động vì chúng ta luôn có thể kiểm tra tất cả các kết hợp tiền tố hậu tố, nhưng nó không thành công do thời gian bậc hai. Quan sát thấy rằng sự chồng chéo có thể được tính toán một cách hiệu quả bằng KMP sẽ chuyển vấn đề thành một nhiệm vụ thao tác chuỗi thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng_length²) | O(tổng_chiều dài) | Quá chậm | 
| Tối ưu dựa trên KMP | O(tổng_chiều dài) | O(tổng_chiều dài) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`compressed`làm từ đầu tiên, trở thành chuỗi bắt đầu của chúng tôi. 
2. Lặp lại từng từ tiếp theo trong câu. 
3. Đối với từ hiện tại, hãy tính mức trùng lặp tối đa với từ hiện tại`compressed`sợi dây. Nối từ hiện tại, một dấu phân cách duy nhất (ví dụ:`#`) và cuối cùng`len(word)`nhân vật của`compressed`. 
4. Xây dựng hàm tiền tố cho chuỗi nối này. Giá trị cuối cùng của mảng hàm tiền tố cho biết độ dài tiền tố dài nhất của từ hiện tại khớp với hậu tố của`compressed`. 
5. Nối vào`compressed`chỉ phần của từ hiện tại không trùng nhau. 
6. Tiếp tục cho đến khi tất cả các từ được xử lý. 
7. Xuất kết quả cuối cùng`compressed`sợi dây. 

Thuật toán hoạt động vì ở mỗi bước chúng tôi tính toán rõ ràng mức độ chồng lấp tối đa giữa chuỗi hiện tại và từ tiếp theo. Hàm tiền tố KMP đảm bảo chúng tôi không bao giờ bỏ lỡ phần chồng chéo dài hơn, do đó, bất biến "được nén luôn chứa tất cả các từ được hợp nhất chính xác với độ chồng chéo tối đa" được giữ xuyên suốt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def compute_prefix_function(s):
    n = len(s)
    pi = [0] * n
    for i in range(1, n):
        j = pi[i - 1]
        while j > 0 and s[i] != s[j]:
            j = pi[j - 1]
        if s[i] == s[j]:
            j += 1
        pi[i] = j
    return pi

def main():
    n = int(input())
    words = input().strip().split()
    
    compressed = words[0]
    
    for word in words[1:]:
        # only need last len(word) chars for overlap check
        overlap_check = word + "#" + compressed[-len(word):] if len(compressed) >= len(word) else word + "#" + compressed
        pi = compute_prefix_function(overlap_check)
        overlap_len = pi[-1]
        compressed += word[overlap_len:]
    
    print(compressed)

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng cách đọc dữ liệu đầu vào và khởi tạo chuỗi nén bằng từ đầu tiên. Đối với mỗi từ tiếp theo, nó xây dựng chuỗi được sử dụng để tính toán hàm tiền tố. Giá trị cuối cùng của mảng tiền tố xác định độ dài tiền tố của từ mới khớp với hậu tố của chuỗi nén hiện có. Chỉ thêm phần không chồng chéo sẽ đảm bảo việc hợp nhất chính xác. Chỉ sử dụng cái cuối cùng`len(word)`nhân vật của`compressed`tránh những công việc không cần thiết, giữ cho thuật toán tuyến tính. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:`"I want to order pizza"`| Bước | Đã nén | Từ hiện tại | Chồng chéo | Nối thêm | 
| --- | --- | --- | --- | --- | 
| 1 | "Tôi" | "muốn" | 0 | "muốn" | 
| 2 | "Tôi muốn" | "đến" | 0 | "đến" | 
| 3 | "Tôi muốn" | "đặt hàng" | 0 | "đặt hàng" | 
| 4 | "Tôi muốn đặt hàng" | "pizza" | 0 | "pizza" | 

Đầu ra cuối cùng:`"Iwanttoorderpizza"`**Mẫu 2** 

đầu vào:`"abc bc c"`| Bước | Đã nén | Từ hiện tại | Chồng chéo | Nối thêm | 
| --- | --- | --- | --- | --- | 
| 1 | "abc" | "bc" | 2 | "" | 
| 2 | "abc" | "c" | 1 | "" | 

Đầu ra cuối cùng:`"abc"`Dấu vết này xác nhận thuật toán phát hiện chính xác các phần trùng lặp tối đa và chỉ nối thêm các ký tự cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng_chiều dài) | Mỗi ký tự được xử lý nhiều nhất hai lần: một lần trong tính toán tiền tố, một lần trong phần bổ sung | 
| Không gian | O(tổng_chiều dài) | Mảng hàm tiền tố cho mỗi cách nối từ sử dụng khoảng trắng tỷ lệ với độ dài từ | 

Với tổng độ dài của các từ ≤ 10^6, điều này phù hợp thoải mái trong vòng 1 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("5\nI want to order pizza\n") == "Iwanttoorderpizza", "sample 1"

# minimum size input
assert run("1\na\n") == "a", "minimum size"

# complete overlap
assert run("3\nabc bc c\n") == "abc", "overlap"

# partial overlaps
assert run("3\nabc bcd cde\n") == "abcde", "partial overlaps"

# no overlaps
assert run("3\nhello world test\n") == "helloworldtest", "no overlaps"

# long overlap with same word
assert run("2\naaaa aaa\n") == "aaaa", "long overlap same letter"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "5\nTôi muốn đặt pizza" | "Tôi muốn đặt hàng pizza" | Sáp nhập thông thường | 
| "1\na" | "một" | Đầu vào tối thiểu | 
| "3\nabc bc c" | "abc" | Hoàn thành chồng chéo | 
| "3\nabc bcd cde" | "abcde" | Chồng chéo một phần | 
| "3\nhello world test" | "helloworldtest" | Không chồng chéo | 
| "2\naaaa aaa" | "aaa" | Trùng lặp với các ký tự lặp lại | 

## Vỏ cạnh 

Đối với đầu vào`"abc bc c"`, thuật toán hợp nhất đầu tiên`"abc"`với`"bc"`. Hai ký tự cuối cùng`"bc"`chồng chéo với`"bc"`, vì vậy không có gì được thêm vào. Kế tiếp,`"c"`trùng lặp hoàn toàn với ký tự cuối cùng của`"abc"`, vì vậy một lần nữa không có gì được thêm vào. Chuỗi cuối cùng vẫn còn`"abc"`, điều đó đúng. Hàm tiền tố xác định chính xác các phần trùng lặp tối đa ngay cả khi chúng bao trùm toàn bộ từ mới. Đối với đầu vào`"aaaa aaa"`, sự trùng lặp là ba ký tự, để lại chuỗi nén là`"aaaa"`. Các trường hợp đặc biệt này chứng tỏ rằng thuật toán xử lý chính xác các phần chồng chéo hoàn chỉnh và một phần mà không cần thêm các ký tự dư thừa.
