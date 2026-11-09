---
title: "CF 1487C - Dây buộc tối thiểu"
description: "Chúng ta được yêu cầu thiết kế kết quả của một giải đấu bóng đá vòng tròn sao cho mỗi đội đều có cùng số điểm. Có $n$ đội và mỗi cặp chơi đúng một lần."
date: "2026-06-10T23:02:33+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "dfs-and-similar", "graphs", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1487
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 104 (Rated for Div. 2)"
rating: 1500
weight: 1487
solve_time_s: 365
verified: false
draft: false
---

[CF 1487C - Mối ràng buộc tối thiểu](https://codeforces.com/problemset/problem/1487/C) 

**Đánh giá:** 1500 
**Tags:** sức mạnh vũ phu, thuật toán xây dựng, dfs và tương tự, đồ thị, tham lam, triển khai, toán học 
**Thời gian giải:** 6m 5s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu thiết kế kết quả của một giải đấu bóng đá vòng tròn sao cho mỗi đội đều có cùng số điểm. có$n$các đội và mỗi cặp chơi đúng một lần. Mỗi trận đấu có thể kết thúc với tỷ số hòa (cả hai đội được 1 điểm) hoặc kết quả quyết định (một đội được 3 điểm, đội còn lại 0 điểm). Mục tiêu của chúng tôi là ấn định kết quả cho tất cả các trận đấu sao cho tổng điểm của mỗi đội là như nhau và trong số tất cả các cấu hình như vậy, số lượng hòa được giảm thiểu. Định dạng đầu ra là một danh sách kết quả được làm phẳng theo thứ tự các cặp trận đấu: đội đầu tiên 1 đấu với tất cả các đội khác, sau đó đội 2 đấu với tất cả các đội sau đó, v.v. 

Các hạn chế ở mức vừa phải:$2 \le n \le 100$và lên tới 100 trường hợp thử nghiệm. Tính toán trực tiếp tất cả các kết quả theo cặp là khả thi, vì tổng số trận đấu là$n(n-1)/2$, nhiều nhất là 4950 khi$n = 100$. Do đó một$O(n^2)$thuật toán có thể chấp nhận được. 

Một điểm tinh tế là thậm chí$n$, có thể tránh hoàn toàn các ràng buộc, trong khi đối với các trường hợp lẻ$n$, ít nhất một số ràng buộc là cần thiết. Một cách tiếp cận ngây thơ có thể cố gắng ấn định chiến thắng một cách tùy tiện, điều này có thể dễ dàng dẫn đến điểm số không đồng đều. Ví dụ, với$n = 3$, nếu tất cả các trận đấu đều có người thắng tùy ý, một đội có thể thắng hai lần trong khi đội kia thua hai lần, dẫn đến tổng điểm không bằng nhau. Cần phải thiết kế cẩn thận để cân bằng giữa thắng và thua một cách đối xứng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả$3^{n(n-1)/2}$có thể kết hợp các kết quả trận đấu và kiểm tra điểm số bằng nhau. Điều này rõ ràng là không thể thực hiện được đối với$n = 100$bởi vì số lượng cấu hình phù hợp tăng theo cấp số nhân. Ngay cả việc mô phỏng trực tiếp tất cả các hoán vị của chiến thắng và hòa cũng quá chậm. 

Quan sát quan trọng là chúng ta có thể mô hình hóa các kết quả dưới dạng một vòng quay thắng và thua. Nếu như$n$Thật kỳ quặc, mỗi đội có thể được sắp xếp theo chu kỳ “thắng-hòa-thua” với các đội khác: đối với bất kỳ trận đấu nào giữa các đội$i$và đội$j$, chúng ta có thể tính toán phần bù$(j - i)$modulo$n$và ấn định kết quả dựa trên việc chênh lệch này nhỏ hơn, bằng hay lớn hơn$n/2$. Nếu như$n$chẵn, ý tưởng xoay vòng tương tự cũng có hiệu quả, nhưng đối với một nửa số trận đấu của mỗi đội, chúng tôi ấn định một đội thắng và nửa còn lại là thua, tránh hoàn toàn hòa. Sự hòa chỉ xảy ra khi sự khác biệt nằm chính xác ở giữa đối với số lẻ$n$. 

Quan sát này làm giảm vấn đề xuống một cách đơn giản$O(n^2)$lặp lại tất cả các trận đấu, chỉ định 1, -1 hoặc 0 một cách xác định dựa trên chỉ số tương đối của các đội. Đó là một giải pháp mang tính xây dựng với một khuôn mẫu rõ ràng nhằm đảm bảo tổng số điểm bằng nhau cho mỗi đội. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(3^{n(n-1)/2})$|$O(n^2)$| Quá chậm | 
| Xoay vòng mang tính xây dựng |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Đối với mỗi trường hợp thử nghiệm, hãy đọc số lượng đội$n$. 
2. Khởi tạo một danh sách trống để giữ kết quả cho tất cả các kết quả khớp. 
3. Lặp lại tất cả các cặp đội$(i, j)$với$i < j$để điền kết quả trận đấu. 
4. Tính khoảng cách$d = j - i$. 
5. Nếu$n$là số lẻ, hãy gán: 

- 1 nếu$d \le n//2$, nghĩa là đội$i$thắng. 
- -1 nếu$d > n//2$, nghĩa là đội$j$thắng. 
6. Nếu$n$là số chẵn, gán: 

- 1 nếu$d \le n//2 - 1$, nghĩa là đội$i$thắng. 
- -1 nếu$d \ge n//2 + 1$, nghĩa là đội$j$thắng. 
- 0 nếu$d = n//2$, nghĩa là cần phải có sự hòa để giữ thăng bằng. 
7. Thêm kết quả tính toán vào danh sách kết quả. 
8. Sau khi xử lý tất cả các cặp, in kết quả theo thứ tự làm phẳng cần thiết. 

Tại sao nó hoạt động: phân công vòng tròn đảm bảo rằng mỗi đội đều thắng chính xác$\lfloor (n-1)/2 \rfloor$trận đấu, thua chính xác$\lfloor (n-1)/2 \rfloor$khớp nếu$n$là số lẻ và hòa đúng một trận nếu$n$thật kỳ quặc. Thậm chí$n$, mỗi đội thắng và thua$n/2 - 1$trận đấu và hòa chính xác một, cho tổng số điểm bằng nhau. Mô hình này đảm bảo điểm số bằng nhau với các mối quan hệ tối thiểu vì bất kỳ nỗ lực nào nhằm giảm bớt mối quan hệ hơn nữa sẽ phá vỡ tính đối xứng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    res = []
    for i in range(1, n+1):
        for j in range(i+1, n+1):
            d = j - i
            if n % 2 == 1:
                if d <= n // 2:
                    res.append(1)
                else:
                    res.append(-1)
            else:
                if d < n // 2:
                    res.append(1)
                elif d == n // 2:
                    res.append(0)
                else:
                    res.append(-1)
    print(' '.join(map(str, res)))
```Giải pháp đầu tiên là đọc$t$các trường hợp thử nghiệm, sau đó lặp lại qua từng cặp đội. Khoảng cách$d$giữa các chỉ số của nhóm xác định kết quả bằng cách sử dụng logic mô-đun. Thậm chí$n$, trận giữa phải hòa để cân bằng tỷ số, còn lẻ$n$, mọi trận đấu đều có thể mang tính chất quyết định. Đầu ra được làm phẳng theo thứ tự yêu cầu. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
2
2
3
```| Trận đấu | tôi | j | n | d=j-i | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 2 | 1 | 0 | 
| 2 | 1 | 2 | 3 | 1 | 1 | 
| 3 | 1 | 3 | 3 | 2 | -1 | 
| 4 | 2 | 3 | 3 | 1 | 1 | 

Giải thích: Với n=2, trận đấu duy nhất phải hòa. Với n=3, đội 1 thắng 2, thua 3, đội 2 thắng 3. Tất cả các đội đều được 3 điểm. Mối quan hệ tối thiểu xảy ra, bằng không trong trường hợp này. 

### Đầu vào mẫu 2```
1
4
```| Trận đấu | tôi | j | n | d | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 4 | 1 | 1 | 
| 2 | 1 | 3 | 4 | 2 | 0 | 
| 3 | 1 | 4 | 4 | 3 | -1 | 
| 4 | 2 | 3 | 4 | 1 | 1 | 
| 5 | 2 | 4 | 4 | 2 | 0 | 
| 6 | 3 | 4 | 4 | 1 | 1 | 

Giải thích: Thậm chí n yêu cầu trận giữa hòa để cân bằng điểm số. Mỗi đội kết thúc với 4 điểm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Chúng tôi lặp lại tất cả$\frac{n(n-1)}{2}$khớp một lần cho mỗi trường hợp thử nghiệm | 
| Không gian | O(n^2) | Chúng tôi lưu trữ tất cả kết quả trận đấu trong một danh sách trước khi in | 

Với$n \le 100$Và$t \le 100$, tổng số thao tác nhiều nhất là$100 * 4950 = 495,000$, trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        res = []
        for i in range(1, n+1):
            for j in range(i+1, n+1):
                d = j - i
                if n % 2 == 1:
                    if d <= n // 2:
                        res.append(1)
                    else:
                        res.append(-1)
                else:
                    if d < n // 2:
                        res.append(1)
                    elif d == n // 2:
                        res.append(0)
                    else:
                        res.append(-1)
        print(' '.join(map(str, res)))
    return output.getvalue().strip()

assert run("2\n2\n3\n") == "0\n1 -1 1", "sample 1"
assert run("1\n4\n") == "1 0 -1 1 0 1", "even n case"
assert run("1\n5\n") == "1 1 -1 -1 1 1 -1 -1 1 1", "odd n case"
assert run("1\n6\n") == "1 1 0 -1 -1 1 1 0 -1 -1 1 1 0 -1 -1", "even n larger"
assert run("1\n2\n") == "0", "
```
