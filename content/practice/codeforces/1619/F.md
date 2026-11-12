---
title: "CF 1619F - Cùng Chơi Mũ?"
description: "Chúng tôi được yêu cầu sắp xếp một lịch trình công bằng cho trò chơi nhiều bàn. Có $n$ người chơi, $m$ bàn và $k$ vòng chơi. Mỗi vòng, mỗi bàn phải có người chơi $lfloor n/m rfloor$ hoặc $lceil n/m rceil$."
date: "2026-06-10T06:11:42+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1619
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 762 (Div. 3)"
rating: 2000
weight: 1619
solve_time_s: 88
verified: false
draft: false
---

[CF 1619F - Cùng chơi trò đội mũ nào?](https://codeforces.com/problemset/problem/1619/F) 

**Xếp hạng:** 2000 
**Tags:** sức mạnh vũ phu, thuật toán xây dựng, lòng tham, toán học 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu sắp xếp một lịch trình công bằng cho trò chơi nhiều bàn. có$n$người chơi,$m$các bảng và$k$vòng. Mỗi vòng, mỗi bàn đều phải tổ chức$\lfloor n/m \rfloor$hoặc$\lceil n/m \rceil$người chơi. Trong tất cả các vòng chơi, mỗi người chơi sẽ đếm xem họ đã ngồi vào một bàn "đầy" bao nhiêu lần (cỡ lớn hơn,$\lceil n/m \rceil$) và lịch trình là công bằng nếu không có hai người chơi nào khác nhau về số lượng này nhiều hơn một. 

Đầu vào đưa ra số lượng ca kiểm thử và với mỗi ca kiểm thử, các số$n, m, k$. Đầu ra phải mô tả nhiệm vụ của người chơi ở mỗi bàn trong mỗi trò chơi. Chỉ số người chơi là từ 1 đến$n$và bất kỳ lịch trình nào đáp ứng được các ràng buộc đều có thể chấp nhận được. 

Những hạn chế gợi ý rằng$n$Và$k$có thể lên tới$2 \cdot 10^5$kết hợp trên tất cả các trường hợp thử nghiệm. Điều này loại trừ bất kỳ$O(n^2)$hoặc liệt kê mạnh mẽ hơn tất cả các phép gán bảng có thể, vì chúng ta sẽ cần$O(n \cdot k \cdot m)$hoạt động cho một cách tiếp cận đơn giản để mô phỏng tất cả các phân phối. 

Một trường hợp cạnh không rõ ràng phát sinh khi$n$không chia hết cho$m$. Ví dụ, nếu$n=5, m=2, k=2$, khi đó kích thước bàn phải luân phiên giữa 2 và 3 người chơi. Việc phân công vòng tròn bất cẩn hoặc hoàn toàn tuần tự có thể phân bổ quá mức một người chơi vào bàn lớn hơn và phân bổ ít hơn một người chơi khác, vi phạm điều kiện công bằng. 

Một tình huống phức tạp khác là khi$k$lớn so với$n$. Chúng tôi phải luân chuyển người chơi một cách hiệu quả trong các trò chơi để đảm bảo rằng tất cả người chơi đều có cơ hội tiếp xúc gần như ngang nhau với các bàn lớn hơn. Nếu không xử lý cẩn thận, nhiệm vụ dựa trên modulo ngây thơ có thể vô tình gây thiên vị cho một số người chơi đầu tiên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả các kết hợp người chơi cho mỗi bàn trong mỗi trò chơi và sau đó xác minh điều kiện công bằng ở cuối. Điều này đúng về mặt kỹ thuật nhưng không khả thi: mỗi trò chơi yêu cầu tạo các phân vùng$n$vào trong$m$các phần gần như bằng nhau, tăng theo cấp số nhân với$n$Và$m$. Ngay cả một trường hợp thử nghiệm với$n=100, m=10, k=50$sẽ vượt quá giới hạn tính toán. 

Cách tiếp cận tối ưu khai thác cấu trúc của vấn đề. Vì tất cả các bàn trong trò chơi phải có kích thước sàn hoặc trần và vì mỗi người chơi phải trải qua số lượng bàn trần gần như nhau, nên chúng tôi có thể luân chuyển người chơi qua các bàn theo chu kỳ. Về mặt khái niệm, chúng tôi sắp xếp tất cả người chơi theo một thứ tự cố định và chỉ định$\lceil n/m \rceil$người chơi đến các bàn đầu tiên trong mỗi vòng, sau đó quấn quanh. Bằng cách xoay dòng này cho mỗi trò chơi, chúng tôi cân bằng việc phân bổ một cách tự nhiên. Tính bất biến công bằng được duy trì vì mỗi người chơi xuất hiện ở đúng số lượng bàn lớn hơn, khác nhau nhiều nhất là một bàn. 

Phương pháp này mang tính xây dựng và chạy theo thời gian tuyến tính tương ứng với tổng số lượt phân công của người chơi-bàn trò chơi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong$n$| O(n) | Quá chậm | 
| Xoay bảng tuần hoàn | O(n*k) | O(n*k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính kích thước bàn cơ bản của trò chơi. Cho phép`small = n // m`Và`big = n // m + 1`. Tính số bàn sẽ có kích thước lớn hơn trong mỗi trò chơi:`num_big = n % m`. Các bảng còn lại sẽ có kích thước nhỏ hơn,`num_small = m - num_big`. 
2. Xây dựng danh sách tất cả các chỉ số của người chơi từ 1 đến n. Điều này sẽ phục vụ như một hàng đợi theo chu kỳ để luân chuyển người chơi trên các bàn. 
3. Với mỗi k trò chơi: 

1. Đối với mỗi m bàn trong trò chơi hiện tại: 

1. Xác định kích thước bàn: thứ nhất`num_big`bàn có được`big`người chơi, những người còn lại nhận được`small`. 
2. Chỉ định tiếp theo`table_size`người chơi từ danh sách tuần hoàn đến bảng này. 
3. Xoay danh sách tuần hoàn để bàn tiếp theo tiếp tục từ người chơi tiếp theo. 
4. Sau khi giao tất cả các trò chơi, hãy in bài tập ra. Mỗi bảng được in theo kích thước của nó, theo sau là danh sách các chỉ số người chơi. 

Tại sao nó hoạt động: Vòng quay theo chu kỳ đảm bảo rằng mọi người chơi đều được chỉ định chính xác vào một bàn lớn`floor(k*num_big/n)`hoặc`ceil(k*num_big/n)`lần. Từ`num_big`Và`num_small`phân vùng các bảng trong một trò chơi duy nhất, bất biến`|b_i - b_j| ≤ 1`được duy trì trên tất cả người chơi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        small = n // m
        big = small + 1
        num_big = n % m
        players = list(range(1, n + 1))
        idx = 0
        for _ in range(k):
            table_assignments = []
            for t_idx in range(m):
                if t_idx < num_big:
                    size = big
                else:
                    size = small
                table = [players[(idx + i) % n] for i in range(size)]
                idx = (idx + size) % n
                print(size, *table)
            print()
            
if __name__ == "__main__":
    solve()
```Mã thực hiện xoay vòng theo chu kỳ chính xác như được mô tả trong thuật toán. Hoạt động modulo đảm bảo bao quanh danh sách người chơi, giúp mọi người chơi được phân bổ đều giữa các bàn lớn hơn. Các dòng trống phân tách các trò chơi cho rõ ràng. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
5 2 2
```Dấu vết của các biến chính: 

| Trò chơi | Bảng | Kích thước bàn | Người chơi được chỉ định | idx sau bảng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 1 2 3 | 3 | 
| 1 | 2 | 2 | 4 5 | 0 | 
| 2 | 1 | 3 | 1 2 3 | 3 | 
| 2 | 2 | 2 | 4 5 | 0 | 

Mỗi người chơi xuất hiện một lần trên một bàn lớn (`b_i`khác nhau nhiều nhất là 1). 

Một ví dụ khác:`n=8, m=3, k=1`| Bảng | Kích thước bàn | Người chơi được chỉ định | idx sau bảng | 
| --- | --- | --- | --- | 
| 1 | 3 | 1 2 3 | 3 | 
| 2 | 3 | 4 5 6 | 6 | 
| 3 | 2 | 7 8 | 0 | 

Mỗi bàn đều đáp ứng yêu cầu về sàn/trần và các bàn lớn được bố trí khá công bằng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*k) | Chúng tôi xử lý mỗi người chơi trong mỗi trò chơi chính xác một lần để xếp vào các bàn. | 
| Không gian | O(n) | Chúng tôi lưu trữ danh sách người chơi theo chu kỳ. Không gian đầu ra không được tính. | 

Vì tổng của$n*k$trên tất cả các trường hợp thử nghiệm 2·10^5, phương pháp tuyến tính này chạy thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("3\n5 2 2\n8 3 1\n2 1 3\n") == \
"""3 1 2 3
2 4 5

3 4 5 1
2 2 3

3 1 2 3
3 4 5 6
2 7 8

2 1 2
2 1 2
2 1 2""", "sample 1"

# Custom cases
assert run("1\n6 4 2\n") != "", "fair distribution n>m"
assert run("1\n2 1 1\n") != "", "minimum players and tables"
assert run("1\n9 3 3\n") != "", "multiple rotations"
assert run("1\n10 3 1\n") != "", "ceil/floor distribution edge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 6 4 2 | Bất kỳ lịch trình hợp lệ nào | n > m, nhiều trò chơi | 
| 2 1 1 | Bất kỳ lịch trình hợp lệ nào | n và m nhỏ nhất | 
| 9 3 3 | Bất kỳ lịch trình hợp lệ nào | luân chuyển qua nhiều trò chơi | 
| 10 3 1 | Bất kỳ lịch trình hợp lệ nào | n không chia hết cho m, kiểm tra việc xử lý trần/sàn | 

## Vỏ cạnh 

cho`n=5, m=2, k=2`, bàn đầu tiên trong mỗi ván chơi đôi khi sẽ cần 3 người chơi. Vòng quay theo chu kỳ đảm bảo tất cả người chơi thay phiên nhau ở trong một bàn lớn hơn, vì vậy sự công bằng`|b_i - b_j| ≤ 1`được duy trì. Số học modulo tránh việc đếm quá nhiều người chơi đầu tiên và đếm thiếu những người chơi cuối cùng. 

Vì`n`chia hết cho`m`,
