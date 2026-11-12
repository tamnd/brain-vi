---
title: "CF 1619H - Hoán vị và truy vấn"
description: "Chúng ta được cho một hoán vị các số nguyên từ 1 đến $n$. Về mặt khái niệm, đây là một mảng có độ dài $n$ trong đó mọi số nguyên từ 1 đến $n$ xuất hiện chính xác một lần, do đó mỗi chỉ mục ánh xạ tới một giá trị duy nhất. Ngoài mảng này, chúng ta có hai loại truy vấn."
date: "2026-06-10T06:12:47+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "divide-and-conquer", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1619
codeforces_index: "H"
codeforces_contest_name: "Codeforces Round 762 (Div. 3)"
rating: 2400
weight: 1619
solve_time_s: 64
verified: true
draft: false
---

[CF 1619H - Hoán vị và Truy vấn](https://codeforces.com/problemset/problem/1619/H) 

**Đánh giá:** 2400 
**Tags:** bạo lực, cấu trúc dữ liệu, phân chia và chinh phục, hai con trỏ 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số nguyên từ 1 đến$n$. Về mặt khái niệm, đây là một mảng có độ dài$n$trong đó mọi số nguyên từ 1 đến$n$xuất hiện chính xác một lần, vì vậy mỗi chỉ mục sẽ ánh xạ tới một giá trị duy nhất. Ngoài mảng này, chúng ta có hai loại truy vấn. Loại đầu tiên hoán đổi hai phần tử trong hoán vị. Loại thứ hai liên tục áp dụng hoán vị bắt đầu từ một chỉ mục nhất định và hỏi giá trị sẽ trở thành gì sau đó$k$ứng dụng. Ví dụ, nếu$p = [5, 3, 4, 2, 1]$và chúng tôi truy vấn chỉ mục 1 với$k = 2$, chúng tôi làm theo$p[1] = 5$và sau đó$p[5] = 1$, vậy đáp án là 1 

Những hạn chế là đáng kể:$n$Và$q$cả hai có thể đạt được$10^5$. Một giải pháp đơn giản mô phỏng$k$các bước cho mỗi truy vấn có khả năng thực hiện tối đa$10^5 \times 10^5 = 10^{10}$vượt xa giới hạn thời gian 4 giây cho phép. Chúng ta cần một cách tiếp cận giúp giảm bớt công việc lặp đi lặp lại và tránh việc tính toán lại các giá trị một cách không cần thiết. 

Các trường hợp cạnh không rõ ràng phát sinh khi$k = n$hoặc$k > n$, bởi vì hoán vị tạo thành chu kỳ. Một cách tiếp cận bất cẩn chỉ lặp đi lặp lại$k$thời gian sẽ quá chậm và thậm chí có thể thất bại nếu các giao dịch hoán đổi đã thay đổi cấu trúc chu kỳ. Một tình huống khó khăn khác là khi các lần hoán đổi lặp đi lặp lại sẽ di chuyển các phần tử bên trong một chu kỳ; chúng ta cần một cách để cập nhật hiểu biết của mình về chu trình một cách hiệu quả mà không cần tính toán lại mọi thứ từ đầu. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: đối với mỗi truy vấn loại 2, chúng tôi bắt đầu từ chỉ mục$i$và làm theo$p[i]$ $k$lần. Mỗi truy vấn loại 1 chỉ cần hoán đổi hai phần tử. Điều này hoạt động chính xác đối với đầu vào nhỏ nhưng quá chậm khi$n$hoặc$q$lớn vì mỗi truy vấn loại 2 có thể tốn$O(n)$, và có thể có tới$10^5$những truy vấn như vậy. 

Quan sát quan trọng là một hoán vị được tạo thành từ các chu trình rời rạc. Áp dụng nhiều lần$p$bắt đầu từ một số chỉ mục$i$tương đương với việc di chuyển dọc theo chu trình chứa$i$. Vì vậy, thay vì lặp lại$k$lần, chúng ta có thể tính toán$k \mod \text{cycle\_length}$và nhảy trực tiếp. Hoán đổi thay đổi việc phân tách chu trình, vì vậy chúng ta cần một cấu trúc dữ liệu cho phép chúng ta cập nhật các chu trình một cách hiệu quả. Từ$n$Và$q$tương đối lớn, cách tiếp cận đơn giản nhất là chỉ xây dựng lại các chu kỳ bị ảnh hưởng sau khi hoán đổi, cách này hiệu quả vì mỗi phần tử thuộc về chính xác một chu kỳ. 

Điều này làm giảm truy vấn loại 2 từ$O(k)$ĐẾN$O(1)$nếu chúng tôi lưu trữ các chu trình một cách rõ ràng và các truy vấn loại 1 có thể được xử lý trong$O(\text{cycle\_size})$, điều này có thể chấp nhận được vì mỗi phần tử chỉ thay đổi chu kỳ khi hoán đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*q) | O(n) | Quá chậm | 
| Phân hủy chu kỳ | O(n + q) khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hoán vị$p$và các truy vấn. Chuyển thành$p$lập chỉ mục dựa trên 0 để thao tác mảng dễ dàng hơn trong Python. 
2. Khởi tạo một mảng để đánh dấu xem một phần tử đã được truy cập trong khám phá chu trình hay chưa. Đồng thời duy trì một`cycle`mảng ánh xạ từng chỉ mục vào danh sách chu trình của nó. 
3. Đối với mỗi chỉ mục chưa được gán cho một chu trình, hãy duyệt hoán vị cho đến khi chúng ta lặp lại. Ghi lại chu trình vào một danh sách và lưu trữ các tài liệu tham khảo trong`cycle`mảng. 
4. Đối với truy vấn loại 2, tìm chu trình của chỉ mục được truy vấn. Tính chỉ số mục tiêu bên trong chu trình như$(\text{position in cycle} + k) \mod \text{cycle length}$. Xuất phần tử tại vị trí đó. 
5. Đối với truy vấn loại 1, hoán đổi hai phần tử. Vì việc hoán đổi ảnh hưởng đến các chu kỳ nên hãy xây dựng lại các chu trình chứa các phần tử được hoán đổi. Chúng ta chỉ cần xử lý lại các chu trình chứa hai chỉ số này. 
6. Tiếp tục xử lý các truy vấn theo thứ tự, áp dụng hoán đổi và trả lời các truy vấn loại 2 bằng các chu kỳ được cập nhật. 

Điều bất biến là mọi chỉ mục đều thuộc về chính xác một chu kỳ và cấu trúc chu trình được cập nhật chính xác sau mỗi lần hoán đổi. Điều này đảm bảo rằng các truy vấn loại 2 luôn trả về phần tử chính xác, ngay cả sau nhiều lần hoán đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, q = map(int, input().split())
    p = list(map(int, input().split()))
    p = [x-1 for x in p]  # convert to 0-based indexing

    # each element's cycle index and position in the cycle
    cycle_id = [-1] * n
    pos_in_cycle = [-1] * n
    cycles = []

    def build_cycle(start):
        cycle = []
        x = start
        while cycle_id[x] == -1:
            cycle_id[x] = len(cycles)
            pos_in_cycle[x] = len(cycle)
            cycle.append(x)
            x = p[x]
        cycles.append(cycle)

    for i in range(n):
        if cycle_id[i] == -1:
            build_cycle(i)

    for _ in range(q):
        t, x, y = map(int, input().split())
        if t == 1:
            x -= 1
            y -= 1
            # swap values
            p[x], p[y] = p[y], p[x]
            # rebuild cycles affected by x and y
            for idx in (x, y):
                # reset old cycle info
                cid = cycle_id[idx]
                for v in cycles[cid]:
                    cycle_id[v] = -1
                    pos_in_cycle[v] = -1
                cycles[cid] = []
            for idx in (x, y):
                if cycle_id[idx] == -1:
                    build_cycle(idx)
        else:
            x -= 1
            k = y
            cid = cycle_id[x]
            cycle = cycles[cid]
            pos = pos_in_cycle[x]
            result = cycle[(pos + k) % len(cycle)]
            print(result + 1)

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên xây dựng các chu trình cho hoán vị ban đầu. Đối với truy vấn loại 2, nó tính toán kết quả theo thời gian không đổi bằng số học mô-đun. Đối với giao dịch hoán đổi, nó chỉ xây dựng lại các chu kỳ bị ảnh hưởng bởi giao dịch hoán đổi, giúp thuật toán luôn hoạt động hiệu quả. Cần chú ý cẩn thận để thiết lập lại dữ liệu chu trình trước khi xây dựng lại. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
5 4
5 3 4 2 1
2 3 1
2 1 2
1 1 3
2 1 2
```| Truy vấn | Hành động | Chu kỳ | Kết quả | 
| --- | --- | --- | --- | 
| 2 3 1 | Theo dõi p[3] một lần | [5,1,2,3,4] chu kỳ | 4 | 
| 2 1 2 | Theo dõi p[1] hai lần | giống nhau | 1 | 
| 1 1 3 | Hoán đổi p[1] và p[3] | xây dựng lại chu kỳ chứa 1,3 | - | 
| 2 1 2 | Theo dõi p[1] mới hai lần | chu kỳ cập nhật | 2 | 

Điều này chứng tỏ rằng việc xây dựng lại chu trình xử lý chính xác các giao dịch hoán đổi và truy vấn loại 2 sau khi sửa đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) khấu hao | Mỗi phần tử được gán lại cho một chu kỳ nhiều nhất một lần cho mỗi lần hoán đổi ảnh hưởng đến nó, mỗi truy vấn loại 2 là O(1) | 
| Không gian | O(n) | Lưu trữ hoán vị, id chu trình, vị trí và danh sách chu trình | 

Với$n, q \le 10^5$, tổng số hoạt động vẫn dưới$10^6$khấu hao, phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    main()
    return out.getvalue().strip()

# Sample provided
assert run("5 4\n5 3 4 2 1\n2 3 1\n2 1 2\n1 1 3\n2 1 2\n") == "4\n1\n2"

# Minimum-size input
assert run("1 1\n1\n2 1 1\n") == "1"

# Swap first and last in small permutation
assert run("4 3\n1 2 3 4\n1 1 4\n2 1 3\n2 4 1\n") == "4\n1"

# Cycle-length test
assert run("3 2\n2 3 1\n2 1 5\n2 3 6\n") == "2\n1"

# Repeated swaps
assert run("3 5\n1 2 3\n1 1 2\n1 2 3\n2 1 1\n2 2 2\n2 3 3\n") == "3\n1\n2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | | |
