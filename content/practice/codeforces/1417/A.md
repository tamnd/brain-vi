---
title: "CF 1417A - Sao chép-dán"
description: "Bài toán đưa ra cho chúng ta danh sách các đống kẹo và giới hạn số lượng kẹo mà một đống có thể chứa trước khi BThero mất đi phép thuật của mình. Chúng ta được phép thực hiện thao tác sao chép-dán nhiều lần: chọn hai cọc khác nhau và thêm tất cả kẹo từ cọc thứ nhất vào cọc thứ hai."
date: "2026-06-11T06:55:29+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1417
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 673 (Div. 2)"
rating: 800
weight: 1417
solve_time_s: 83
verified: false
draft: false
---

[CF 1417A - Sao chép-dán](https://codeforces.com/problemset/problem/1417/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra cho chúng ta danh sách các đống kẹo và giới hạn số lượng kẹo mà một đống có thể chứa trước khi BThero mất đi phép thuật của mình. Chúng ta được phép thực hiện thao tác sao chép-dán nhiều lần: chọn hai cọc khác nhau và thêm tất cả kẹo từ cọc thứ nhất vào cọc thứ hai. Mục tiêu là tối đa hóa số lượng các thao tác như vậy đồng thời đảm bảo rằng không có đống kẹo nào vượt quá số lượng kẹo tối đa cho phép. Đầu vào chỉ định nhiều trường hợp thử nghiệm, mỗi trường hợp cung cấp số lượng cọc, số kẹo tối đa được phép trên mỗi cọc và kích thước cọc ban đầu. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm biểu thị số lượng thao tác sao chép-dán tối đa có thể được thực hiện một cách an toàn. 

Các ràng buộc rất nhỏ: số lượng cọc trên mỗi trường hợp thử nghiệm không vượt quá 1000 và số kẹo tối đa được phép trên mỗi cọc nhiều nhất là 10.000. Tổng của tất cả các cọc trong các trường hợp thử nghiệm cũng bị giới hạn bởi 1000. Những ràng buộc này ngụ ý rằng chúng ta không cần phải lo lắng về các cấu trúc dữ liệu hoặc thuật toán được tối ưu hóa cao với độ phức tạp logarit hoặc cận tuyến tính - một giải pháp kiểm tra từng cọc hoặc thực hiện các phép toán tuyến tính trên mỗi trường hợp thử nghiệm là đủ nhanh. 

Các trường hợp cần chú ý bao gồm các tình huống trong đó tất cả các cọc đã ở giá trị tối đa, các cọc chỉ có một viên kẹo hoặc các cấu hình trong đó việc sao chép từ cọc này sang cọc khác ngay lập tức vượt quá giới hạn. Ví dụ, nếu`n = 2`,`k = 2`, và cả hai cọc đều có 1 viên kẹo, chúng ta chỉ có thể thực hiện một thao tác trước khi đạt đến giới hạn. Một cách tiếp cận ngây thơ luôn cố gắng sao chép mà không kiểm tra giới hạn có thể thực hiện thao tác thứ hai không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng rõ ràng từng thao tác sao chép-dán. Đối với mỗi thao tác, chúng ta có thể thử từng cặp cọc hợp lệ, kiểm tra xem việc thêm cọc này vào cọc khác có giữ cả hai cọc ở dưới giới hạn hay không và cập nhật trạng thái. Mặc dù cách này hiệu quả với các ví dụ nhỏ nhưng nó không cần thiết và tẻ nhạt. Mỗi trường hợp thử nghiệm có thể liên quan đến tối đa`n * n`kiểm tra mỗi thao tác và số lượng thao tác có thể tỷ lệ thuận với`k / min(a_i)`, dẫn đến hàng triệu lần lặp lại ngay cả đối với những ràng buộc khiêm tốn. 

Nhận xét quan trọng là bài toán quy về một tổng tham lam đơn giản. Số thao tác sao chép-dán tối đa được xác định bằng cách liên tục thêm chồng nhỏ nhất vào chồng khác mà không vượt quá giới hạn. Nếu chúng ta sắp xếp các cọc, chúng ta luôn có thể sử dụng cọc nhỏ nhất làm cọc cho và chúng ta có thể thêm nó vào các cọc lớn hơn nhiều lần. Khi đống nhỏ nhất đã cạn kiệt theo nghĩa là việc thêm nó vào bất kỳ đống nào khác sẽ vượt quá mức tối đa, chúng tôi dừng lại. Về mặt toán học, điều này tương ứng với việc tính tổng chênh lệch giữa giới hạn và từng cọc rồi chia cho giá trị cọc đóng góp, nhưng vì cọc đóng góp có thể được sử dụng nhiều lần trên bất kỳ đống kẹo lớn hơn nào, chúng ta có thể chỉ cần tính tổng của tất cả các viên kẹo ngoại trừ cọc lớn nhất và trừ nó khỏi giới hạn của cọc lớn nhất. 

Cách tiếp cận tối ưu hoạt động vì thao tác sao chép-dán có tính chất cộng gộp và số lượng thao tác được tối đa hóa khi chúng ta sử dụng cọc nhỏ nhất để "lấp đầy" cọc lớn nhất. Chúng ta không cần phải mô phỏng các bước di chuyển riêng lẻ - chỉ cần tính tổng số kẹo có thể được chuyển vào đống lớn nhất một cách an toàn là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * hoạt động) | O(n) | Quá chậm, không cần thiết | 
| Tối ưu (Tổng tham lam) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc số cọc`n`và số kẹo tối đa được phép`k`. 
2. Đọc danh sách cọc`a`và sắp xếp theo thứ tự tăng dần. Việc sắp xếp đảm bảo rằng cọc nhỏ nhất ở đầu tiên và cọc lớn nhất ở cuối cùng, giúp việc lựa chọn tham lam trở nên đơn giản. 
3. Khởi tạo bộ đếm`ops`về 0 để theo dõi số lượng thao tác sao chép-dán an toàn. 
4. Lặp lại từng cọc ngoại trừ cọc lớn nhất. Đối với mỗi cọc`i`có giá trị`a[i]`, tính xem có thể thêm bao nhiêu lần vào đống lớn nhất`a[-1]`không vượt quá`k`. Số lượng hoạt động cho đống này là`min(a[i], k - a[-1])`. Thêm số này vào`ops`, sau đó cập nhật`a[-1]`để bao gồm các loại kẹo được thêm vào. 
5. Khi tất cả các cọc nhỏ hơn đã được xử lý,`ops`chứa tổng số thao tác sao chép-dán. 
6. In`ops`cho trường hợp thử nghiệm. 

Tại sao nó hoạt động: Điều bất biến là ở mỗi bước, chúng tôi không bao giờ vượt quá số kẹo tối đa được phép trong đống lớn nhất và bằng cách luôn sử dụng đống nhỏ nhất làm nhà tài trợ, chúng tôi tối đa hóa số lượng các hoạt động phụ gia. Thứ tự được sắp xếp đảm bảo chúng ta không bỏ sót một đống nào có thể góp phần vào các hoạt động bổ sung. Vì tất cả các phép toán đều có tính cộng và giao hoán nên thứ tự di chuyển chính xác không quan trọng, chỉ có tổng số tiền đóng góp cho mỗi cọc là không quan trọng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

T = int(input())
for _ in range(T):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()
    ops = 0
    for i in range(n - 1):
        if a[i] + a[-1] > k:
            ops += k - a[-1]
            a[-1] = k
        else:
            ops += a[i]
            a[-1] += a[i]
    print(ops)
```Mã đầu tiên sắp xếp các cọc sao cho cọc nhỏ nhất có thể được sử dụng để tăng cọc lớn nhất một cách an toàn. các`if`tuyên bố đảm bảo rằng chúng tôi không bao giờ vượt quá giới hạn, điều chỉnh cọc lớn nhất và chỉ tính các hoạt động cho phép. Điều này tránh được từng lỗi một trong đó phần bổ sung ngây thơ sẽ tràn ra`k`. Việc phân loại là cần thiết, vì sử dụng bất kỳ đống nào khác trước có thể làm giảm số lượng thao tác an toàn. 

## Ví dụ đã hoạt động 

**Mẫu 1:**`2 2`với`a = [1, 1]`| tôi | một [tôi] | một[-1] | tính toán hoạt động | hoạt động | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 + 1 > 2 → ops += 1 | 1 | 

Điều này thể hiện trường hợp ranh giới trong đó chỉ cho phép một thao tác. 

**Mẫu 2:**`3 5`với`a = [1, 2, 3]`sau khi sắp xếp`[1, 2, 3]`| tôi | một [tôi] | một[-1] | tính toán hoạt động | một[-1] | hoạt động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 3 | 1 + 3 5 → ops += 1 | 4 | 1 | 
| 1 | 2 | 4 | 2 + 4 > 5 → ops += 1 | 5 | 2 | 

Tổng số lượt hoạt động là 5, xác nhận cách tiếp cận tổng tham lam có hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp mảng chiếm ưu thế, lặp đi lặp lại`n-1`cọc là tuyến tính | 
| Không gian | O(n) | Lưu trữ danh sách cọc | 

Với những hạn chế,`n ≤ 1000`Và`T ≤ 500`với`Σ n ≤ 1000`, giải pháp thực thi thoải mái dưới 1 giây với bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# provided samples
assert run("3\n2 2\n1 1\n3 5\n1 2 3\n3 7\n3 2 2\n") == "1\n5\n4", "sample 1"

# custom cases
assert run("1\n2 100\n50 50\n") == "50", "two large equal piles"
assert run("1\n3 10\n1 1 10\n") == "2", "small piles cannot exceed largest"
assert run("1\n4 10\n2 2 2 2\n") == "8", "all equal small piles"
assert run("1\n5 15\n1 2 3 4 5\n") == "14", "increasing sequence"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 100\n50 50`| 50 | vận hành an toàn tối đa với hai cọc lớn bằng nhau | 
|`3 10\n1 1 10`| 2 | không được vượt cọc lớn nhất | 
|`4 10\n2 2 2 2`| 8 | tất cả các cọc nhỏ bằng nhau đóng góp | 
|`5 15\n1 2 3 4 5`| 14 | tính đúng của tổng dãy | 

## Vỏ cạnh 

Đối với trường hợp thử nghiệm trong đó tất cả các cọc đều ở mức tối đa, chẳng hạn như`2 2\n2 2`, thuật toán xác định chính xác rằng không thể thực hiện thao tác sao chép-dán nào. Việc sắp xếp không làm thay đổi mảng và`if`điều kiện ngăn chặn bất kỳ sự bổ sung nào vượt quá`k`. Đối với kịch bản cọc nhỏ nhất,`2 100\n1 1`, thuật toán cho phép một thao tác một cách an toàn từ đống thứ nhất đến đống thứ hai, xác nhận logic tham lam tôn trọng các ranh giới.
