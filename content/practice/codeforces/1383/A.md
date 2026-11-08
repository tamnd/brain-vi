---
title: "CF 1383A - Chuyển đổi chuỗi 1"
description: "Chúng ta có hai chuỗi A và B có cùng độ dài, chỉ sử dụng 20 chữ cái viết thường đầu tiên từ a đến t. Chúng ta được phép chọn nhiều lần một nhóm vị trí trong A mà tất cả đều chứa cùng một chữ cái x và thay thế chúng bằng một chữ cái y lớn hơn."
date: "2026-06-11T10:48:08+07:00"
tags: ["codeforces", "competitive-programming", "dsu", "graphs", "greedy", "sortings", "strings", "trees", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1383
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 659 (Div. 1)"
rating: 1700
weight: 1383
solve_time_s: 97
verified: true
draft: false
---

[CF 1383A - Chuyển đổi chuỗi 1](https://codeforces.com/problemset/problem/1383/A) 

**Đánh giá:** 1700 
**Tags:** dsu, đồ thị, tham lam, sắp xếp, chuỗi, cây, hai con trỏ 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai chuỗi`A`Và`B`có cùng độ dài, chỉ sử dụng 20 chữ cái viết thường đầu tiên từ`a`ĐẾN`t`. Chúng ta được phép chọn nhiều lần một nhóm vị trí trong`A`tất cả đều chứa cùng một chữ cái`x`và thay thế chúng bằng một chữ cái lớn hơn`y`. Nhiệm vụ là xác định số lần di chuyển tối thiểu cần thiết để biến đổi`A`vào trong`B`hoặc báo cáo rằng điều đó là không thể. 

Hạn chế cốt lõi là một chữ cái chỉ có thể di chuyển về phía trước trong bảng chữ cái. Điều này ngay lập tức ngụ ý rằng nếu đối với bất kỳ vị trí nào`i`,`A[i] > B[i]`, thì không thể đạt được`B`bởi vì chúng ta không thể giảm chữ cái. Ví dụ, nếu`A = "c"`Và`B = "a"`, không có chuỗi chuyển động nào có thể biến đổi`c`vào trong`a`, vậy câu trả lời là`-1`. 

Độ dài chuỗi có thể lên tới`10^5`và tổng độ dài của tất cả các ca kiểm thử cũng nhiều nhất là`10^5`. Điều này loại trừ các thuật toán hoạt động trong`O(n^2)`thời gian, bởi vì ngay cả một vòng lặp lồng nhau trên tất cả các vị trí cũng sẽ hoạt động tới`10^10`hoạt động. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống có nhiều chữ cái trong`A`phải di chuyển qua các chữ cái trung gian để đạt được mục tiêu trong`B`. Ví dụ, nếu`A = "abb"`Và`B = "ccc"`, chúng ta không thể biến đổi`b`trực tiếp đến`c`nếu có một bước trước đó yêu cầu thay đổi tất cả`a`s để`b`S. Việc triển khai tham lam bất cẩn thay đổi các chữ cái theo thứ tự tùy ý có thể không tìm được chuỗi tối thiểu hoặc thậm chí là chuỗi hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là quét liên tục`A`và cho mọi vị trí nơi`A[i] != B[i]`, cố gắng tìm một nước đi hợp lý để mang lại`A[i]`gần hơn với`B[i]`. Người ta có thể thử chọn từng vị trí không khớp và thay đổi nó một cách riêng lẻ hoặc theo nhóm tùy ý. Mặc dù về nguyên tắc điều này có hiệu quả nhưng nó không hiệu quả vì mỗi lần quét có thể mất`O(n)`và chúng tôi có thể biểu diễn tới`O(n)`di chuyển, cho`O(n^2)`thời gian, quá chậm. 

Cái nhìn sâu sắc quan trọng là chúng ta chỉ cần tập trung vào chữ cái nhỏ nhất trong`A`nhỏ hơn mục tiêu tương ứng của nó trong`B`. Đối với mỗi chữ cái`x`, chúng ta có thể tìm được giá trị nhỏ nhất`y > x`xuất hiện trong`B`ở một vị trí nào đó`A`có`x`. Sau đó, tất cả các vị trí trong`A`chứa`x`và cần đạt được`y`hoặc cao hơn có thể nâng cấp lên`y`chỉ trong một lần di chuyển. Sau khi thực hiện động tác này, chúng ta xử lý đệ quy`y`như mới`x`trong lần lặp tiếp theo. Điều này đảm bảo mỗi bước di chuyển sẽ nâng cao tối đa các chữ cái mà không vi phạm ràng buộc về thứ tự và đảm bảo rằng chúng ta không bao giờ "vượt quá" mục tiêu hoặc tạo ra tình huống bất khả thi. 

Điều này làm giảm vấn đề phải quét từng chữ cái theo thứ tự từ`a`ĐẾN`t`và đối với mỗi chữ cái, tính toán tập hợp nâng cấp tối thiểu cần thiết để tiếp cận các chữ cái trong`B`. Vì chỉ có 20 chữ cái và với mỗi chữ cái chúng tôi xử lý mỗi vị trí nhiều nhất một lần nên thuật toán sẽ chạy trong`O(n)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n * 20) ≈ O(n) | O(n + 20^2) ≈ O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi test case, trước tiên hãy quét tất cả các vị trí`i`để kiểm tra xem`A[i] > B[i]`. Nếu điều này xảy ra, hãy quay lại ngay lập tức`-1`bởi vì không thể biến đổi`A`vào trong`B`. 
2. Đối với mỗi chữ cái`x`từ`a`ĐẾN`t`, xây dựng một tập hợp tất cả các chữ cái mục tiêu`y`TRONG`B`như vậy`y > x`Và`A[i] = x`. Điều này xác định các chữ cái`x`phải tiến tới. 
3. Nếu bộ này trống, hãy chuyển sang chữ cái tiếp theo. Không cần thực hiện hành động nào đối với`x`. 
4. Ngược lại, hãy tìm giá trị nhỏ nhất`y`trong bộ này. Tất cả các vị trí ở đó`A[i] = x`Và`B[i] >= y`có thể được nâng cấp lên`y`chỉ trong một lần di chuyển. Ghi lại động thái này. 
5. Lặp lại bước 2-4 cho tất cả các chữ cái theo thứ tự tăng dần. Mỗi lần di chuyển sẽ tăng các chữ cái một cách nghiêm ngặt, giữ nguyên ràng buộc`y > x`. 
6. Đếm tổng số lượt di chuyển đã thực hiện. Điều này mang lại số lần di chuyển tối thiểu cần thiết để biến đổi`A`vào trong`B`. 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi chỉ tăng các chữ cái từ`x`đến mức tối thiểu cần thiết`y`. Bất kỳ mục tiêu lớn hơn nào sẽ được xử lý trong lần lặp tiếp theo vì chúng tôi xử lý các chữ cái theo thứ tự. Không có chữ cái nào bị giảm đi và mỗi lần di chuyển sẽ tối đa hóa tiến trình cho nhiều vị trí cùng một lúc, đảm bảo số lần di chuyển tối thiểu. Điều bất biến là sau khi xử lý tất cả các chữ cái nhỏ hơn`z`, tất cả các chữ cái`<= z`hoặc đã đúng hoặc được đặt thành một chữ cái có thể đạt được mục tiêu trong các bước đi trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_moves_to_transform(n, A, B):
    A = list(A)
    moves = 0
    for x_ord in range(20):  # letters 'a' to 't'
        x = chr(ord('a') + x_ord)
        targets = set()
        for i in range(n):
            if A[i] == x and B[i] != x:
                if B[i] < x:
                    return -1
                targets.add(B[i])
        if not targets:
            continue
        y = min(targets)
        for i in range(n):
            if A[i] == x and B[i] >= y:
                A[i] = y
        moves += 1
    return moves

t = int(input())
for _ in range(t):
    n = int(input())
    A = input().strip()
    B = input().strip()
    print(min_moves_to_transform(n, A, B))
```Đầu tiên, chức năng này sẽ kiểm tra tính không thể thực hiện được bằng cách phát hiện bất kỳ`A[i] > B[i]`. Đối với mỗi chữ cái`x`, chúng tôi tính toán chữ cái nâng cấp tối thiểu`y`cần thiết. Đang cập nhật`A`tại chỗ đảm bảo chúng tôi xử lý chính xác các phép biến đổi xếp tầng, vì các chữ cái được nâng cấp trong một lần di chuyển có thể đóng vai trò là`x`trong lần lặp sau. sử dụng`min(targets)`đảm bảo rằng mỗi nước đi sẽ tiến tới tất cả các vị trí liên quan ít nhất cần thiết, giảm thiểu tổng số nước đi. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
A = "aab"
B = "bcc"
```| Bước | Thư x | Mục tiêu đặt ra | Được chọn y | Đã cập nhật A | 
| --- | --- | --- | --- | --- | 
| 1 | một | {b,c} | b | "bbb" | 
| 2 | b | {c} | c | "bcc" | 
| 3 | c | {} | - | "bcc" | 

Đầu ra: 2 bước. Dấu vết xác nhận rằng thuật toán nâng cấp các nhóm chữ cái một cách hiệu quả mà không vi phạm ràng buộc thứ tự. 

Đầu vào mẫu 2:```
A = "cabc"
B = "abcb"
```Ở đây, tại vị trí 1,`A[0] = 'c' > B[0] = 'a'`, do đó hàm trả về ngay`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * 20) ≈ O(n) | Đối với mỗi chữ cái (tổng cộng 20), chúng tôi quét tất cả n vị trí một lần để thu thập mục tiêu và áp dụng các bản cập nhật | 
| Không gian | O(n + 20) ≈ O(n) | Chúng tôi lưu trữ chuỗi`A`và một tập hợp nhỏ các chữ cái mục tiêu cho mỗi lần lặp | 

Được cho`n ≤ 10^5`Và`t ≤ 10`, thuật toán này thực hiện tối đa 2 triệu thao tác cho mỗi trường hợp thử nghiệm trong thực tế, trong giới hạn thời gian 1 giây. Việc sử dụng bộ nhớ bị chi phối bởi việc lưu trữ các chuỗi, vừa vặn thoải mái trong phạm vi 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# Provided samples
assert run("5\n3\naab\nbcc\n4\ncabc\nabcb\n3\nabc\nts r\n4\naabd\ncccd\n5\nabcbd\nbcdda\n") == "2\n-1\n3\n2\n-1"

# Custom cases
assert run("1\n1\na\na\n") == "0", "single letter equal"
assert run("1\n1\na\nb\n") == "1", "single letter increase"
assert run("1\n3\naaa\nttt\n") == "1", "all letters same, one move suffices"
assert run("1\n5\naabcd\naabcd\n") == "0", "already equal strings"
assert run("1\n3\nabc\nabc\n") == "0", "no moves needed"
assert run("1\n3\nabc\nacb\n") == "-1", "impossible due to decrease"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | Đã có chuỗi bằng nhau, không di chuyển | 
| 2 | | |
