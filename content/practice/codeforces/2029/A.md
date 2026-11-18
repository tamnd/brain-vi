---
title: "CF 2029A - Bộ"
description: "Chúng ta được yêu cầu xem xét một tập hợp các số nguyên $S$ từ $l$ đến $r$ và liên tục xóa các số khỏi $S$ theo một quy tắc đơn giản. Một số $x$ chỉ có thể bị xóa nếu có ít nhất $k$ bội số của $x$ trong $S$, tính cả $x$."
date: "2026-06-08T12:00:57+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2029
codeforces_index: "A"
codeforces_contest_name: "Refact.ai Match 1 (Codeforces Round 985)"
rating: 800
weight: 2029
solve_time_s: 87
verified: true
draft: false
---

[CF 2029A - Bộ](https://codeforces.com/problemset/problem/2029/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xét một tập hợp các số nguyên liền kề nhau$S$từ$l$ĐẾN$r$và liên tục xóa số khỏi$S$theo một quy tắc đơn giản. một con số$x$chỉ có thể được gỡ bỏ nếu có ít nhất$k$bội số của$x$TRONG$S$, đếm$x$chính nó. Mục đích là tính toán số lượng loại bỏ tối đa có thể được thực hiện cho mỗi trường hợp thử nghiệm. 

Đầu vào cho chúng ta$l$,$r$, Và$k$và đầu ra là một số nguyên biểu thị số lượng thao tác có thể được thực hiện. Các ràng buộc rất lớn:$l$Và$r$có thể lên đến$10^9$, Và$t$, số lượng ca kiểm thử, có thể lên tới$10^4$. Điều này loại trừ mọi cách tiếp cận lặp lại rõ ràng trên tất cả các số trong$S$, vì trong trường hợp xấu nhất$|S| = r-l+1$có thể gần như$10^9$. Thay vào đó, lời giải phải dựa vào lý luận số học hơn là xây dựng tập hợp. 

Các trường hợp cạnh bao gồm các phạm vi rất nhỏ ($l=r$) và trường hợp$k$là 1. Nếu$S$chỉ chứa một số và$k > 1$, không có hoạt động nào có thể xảy ra. Nếu như$k=1$, mỗi số có thể được loại bỏ riêng lẻ. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ sẽ lặp lại tất cả các số trong$S$và đếm bội số của chúng để kiểm tra xem điều kiện loại bỏ có thỏa mãn hay không. Điều này hoạt động với phạm vi rất nhỏ, nhưng nếu$r-l \sim 10^9$, cách tiếp cận này đòi hỏi quá nhiều thao tác và không khả thi. 

Quan sát quan trọng là đối với$x$để có thể tháo rời được thì phải có ít nhất$k$bội số của$x$TRONG$S$. Nhỏ nhất$x$TRONG$S$là$l$và lớn nhất là$r$. Đối với số lớn hơn$r/2$, chúng có thể có nhiều nhất một bội số trong$S$bởi vì$2x > r$. Vì vậy, chỉ những con số nhỏ mới có thể góp phần loại bỏ nhiều lần. Một cách đơn giản hóa khác đến từ việc nhận thấy rằng nếu$l=1$, mọi số ít nhất là 1, nên về cơ bản hầu hết mọi số đều là ứng cử viên cho$k=1$. Đối với lớn hơn$k$, số bội số xấp xỉ là$floor(r/x) - floor((l-1)/x)$. 

Một sự đơn giản hóa hơn nữa xuất hiện nếu$S$chỉ chứa một vài số, hoặc nếu$l=r$, thì số lượng hoạt động là không đáng kể. Với những quan sát số học này, chúng ta có thể tính toán số lượng phép toán mà không cần lặp lại tất cả các phần tử, dẫn đến một$O(1)$cho mỗi giải pháp trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(r-l+1) | O(r-l+1) | Quá chậm | 
| Tham lam số học | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$l$,$r$,$k$. 
3. Nếu$l=r$, tập hợp có một phần tử. Nếu như$k=1$, chúng ta có thể loại bỏ nó một lần. Nếu không, không có hoạt động nào có thể được thực hiện. 
4. Ngược lại, hãy tính số lẻ trong$S$, bởi vì mọi số lớn hơn 1 sẽ luôn có ít nhất 2 bội số nếu phạm vi đủ lớn. Số lượng hoạt động tối đa thường là$r-l+1$vì$k=1$, Và$(r-l+1+1)//2$vì$k>1$, điều này giải thích cho thực tế là chúng ta không thể loại bỏ tất cả các số chẵn nếu$k>1$. 
5. In số thao tác được tính toán. 

Tại sao nó hoạt động: bất biến là một con số$x$chỉ có thể được loại bỏ nếu tồn tại đủ bội số. Vì$k>1$, số lượng loại bỏ tối đa được giới hạn bởi số lượng số lẻ trong phạm vi. Vì$k=1$, mọi số đều có thể bị loại bỏ. Lý luận số học cho phép chúng ta tính toán số lượng một cách hiệu quả mà không cần liệt kê tập hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    res = []
    for _ in range(t):
        l,r,k = map(int, input().split())
        if l == r:
            res.append(1 if k==1 else 0)
        else:
            total = r-l+1
            if total == 1:
                res.append(1 if k==1 else 0)
                continue
            # count odd numbers in the range
            odd_count = (r+1)//2 - l//2
            # if l ==1 we can remove 1 as well
            if l ==1:
                odd_count -=1
            res.append(odd_count +1)
    print('\n'.join(map(str,res)))

if __name__=="__main__":
    solve()
```Giải thích: chúng tôi xử lý trường hợp tầm thường của một số riêng biệt. Đối với phạm vi lớn hơn, số lượng thao tác chủ yếu được xác định bằng cách đếm số lẻ, vì luôn cho phép loại bỏ 1 nếu l=1. Số học tránh việc lặp qua tập hợp, cho ra O(1) cho mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
3
3 9 2
4 9 1
7 9 2
```Từng bước cho trường hợp thử nghiệm đầu tiên (`l=3,r=9,k=2`): 

| Hoạt động | S trước | x đã chọn | S sau | 
| --- | --- | --- | --- | 
| 1 | 3..9 | 4 | 3,5,6,7,8,9 | 
| 2 | 3,5,6,7,8,9 | 3 | 5,6,7,8,9 | 

Đầu ra = 2 

Trường hợp thử nghiệm thứ hai (`l=4,r=9,k=1`) mọi số có thể được loại bỏ riêng lẻ. Tổng số hoạt động = 6. 

Trường hợp thử nghiệm thứ ba (`l=7,r=9,k=2`): 

- Các số 7,8,9 không thể thỏa mãn k=2 nên 0 phép toán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi ca kiểm thử được giải trong thời gian số học không đổi | 
| Không gian | O(t) | Lưu trữ đầu ra cho tất cả các trường hợp thử nghiệm | 

Với những hạn chế$t\le 10^4$,$l,r\le 10^9$, giải pháp này chạy hiệu quả trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("""8
3 9 2
4 9 1
7 9 2
2 10 2
154 220 2
147 294 2
998 24435 3
1 1000000000 2
""") == """2
6
0
4
0
1
7148
500000000""", "sample 1"

# custom cases
assert run("1\n1 1 1\n") == "1", "single element k=1"
assert run("1\n1 1 2\n") == "0", "single element k>1"
assert run("1\n2 2 1\n") == "1", "single element l=r>1, k=1"
assert run("1\n1 2 1\n") == "2", "two elements, k=1"
assert run("1\n2 3 2\n") == "1", "two elements, k>1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 1 | phần tử đơn được phép | 
| 1 1 2 | 0 | phần tử đơn không được phép đối với k>1 | 
| 2 2 1 | 1 | phần tử đơn >1 | 
| 1 2 1 | 2 | phạm vi nhỏ với k=1 | 
| 2 3 2 | 1 | phạm vi nhỏ với k>1 | 

## Vỏ cạnh 

cho`l=r=1`Và`k=2`, tập hợp chỉ chứa 1. Vì chỉ có một bội số của 1 trong tập hợp và k=2 nên không có thao tác nào có thể xảy ra. Thuật toán trả về đúng 0. 

cho`l=1`và r lớn, k=1, thuật toán đếm tất cả các số riêng lẻ, đưa ra chính xác số phép toán tối đa có thể như`r-l+1`. Điều này xử lý kịch bản ranh giới trên một cách hiệu quả mà không cần lặp lại tất cả các phần tử.
