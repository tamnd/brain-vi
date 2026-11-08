---
title: "CF 1426A - Số tầng"
description: "Bài toán yêu cầu chúng ta tìm số tầng của một căn hộ nhất định trong một tòa nhà có cách bố trí hơi khác thường. Tầng một luôn có đúng hai căn hộ và mỗi tầng tiếp theo có cùng số căn hộ, được tính bằng $x$."
date: "2026-06-11T05:49:15+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1426
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 674 (Div. 3)"
rating: 800
weight: 1426
solve_time_s: 328
verified: false
draft: false
---

[CF 1426A - Số tầng](https://codeforces.com/problemset/problem/1426/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 5 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta tìm số tầng của một căn hộ nhất định trong một tòa nhà có cách bố trí hơi khác thường. Tầng một luôn có đúng hai căn hộ, mỗi tầng tiếp theo có cùng số căn hộ, được tính bởi$x$. Số căn hộ bắt đầu từ$1$ở tầng một và tăng dần khi bạn di chuyển lên tòa nhà. Cho số căn hộ$n$và số lượng căn hộ trên mỗi tầng$x$sau lần đầu tiên, nhiệm vụ là tính xem tầng nào có căn hộ$n$. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm và với mỗi trường hợp thử nghiệm, chúng ta được cung cấp$n$Và$x$. Những hạn chế là nhỏ -$n$Và$x$nhiều nhất là$1000$, và có thể có tới$1000$trường hợp thử nghiệm. Điều này chỉ ra rằng bất kỳ giải pháp nào chạy trong$O(t)$hoặc$O(t \cdot 1)$mỗi trường hợp thử nghiệm là hoàn toàn có thể chấp nhận được. Vì các hoạt động trên mỗi trường hợp thử nghiệm là các phép tính số học đơn giản nên không có nguy cơ vượt quá giới hạn thời gian. 

Các trường hợp khó khăn cần lưu ý là các căn hộ ở tầng một, cụ thể là$n = 1$hoặc$n = 2$. Việc thực hiện bất cẩn luôn áp dụng công thức cho các tầng tiếp theo có thể tính toán sai những trường hợp này. Một cạm bẫy tiềm ẩn khác là khi$n$tiếp đất chính xác ở ranh giới giữa hai tầng. Ví dụ, nếu$x = 3$, căn hộ$3$,$4$, Và$5$chiếm tầng hai, nên một căn hộ$n = 5$phải được xác định chính xác là ở tầng hai chứ không phải tầng ba. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là mô phỏng từng tầng một, đếm số căn hộ cho đến khi đạt tới$n$. Bắt đầu với hai căn hộ ở tầng một và sau đó thêm vào$x$căn hộ cho mỗi tầng tiếp theo, tăng số tầng cho đến khi tầng hiện tại chứa căn hộ$n$. Mặc dù điều này đúng nhưng nó hơi dài dòng đối với một bài toán có số học đơn giản và không cần thiết đối với các ràng buộc. 

Cách tiếp cận tối ưu sử dụng một công thức đơn giản. Nếu như$n \le 2$, căn hộ nằm ở tầng một. Ngược lại, trừ đi số căn hộ ở tầng một và tính xem cần bao nhiêu tầng tiếp theo. Điều này có thể được thể hiện như$(n - 3) // x + 2$, sử dụng phép chia số nguyên. Lý do là thế$n - 2$căn hộ còn lại sau tầng một. Chia cho$x$cung cấp số tầng bổ sung phía trên tầng đầu tiên và chúng tôi thêm$1$để tính đến tầng một. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đúng nhưng không cần thiết | 
| Công thức | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Tối ưu và được chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$Và$x$. 
3. Kiểm tra xem$n$là$1$hoặc$2$. Nếu vậy thì sàn xuất$1$trực tiếp. 
4. Ngược lại thì trừ$2$từ$n$để tính đến tầng một. 
5. Tính số tầng bổ sung là$(n - 3) // x + 2$. Phép trừ bằng$1$bên trong bộ phận đảm bảo làm tròn sàn chính xác cho bội số chính xác của$x$. 
6. Xuất số tầng được tính toán. 

Tại sao nó hoạt động: Công thức phân chia các căn hộ thành tầng đầu tiên gồm hai tầng, sau đó là các tầng liên tiếp có diện tích$x$. Phép chia số nguyên đảm bảo rằng bất kỳ căn hộ nào nằm trong tầng cuối cùng được lấp đầy một phần sẽ làm tròn chính xác đến số tầng đó. Trường hợp cạnh$n \le 2$được xử lý riêng để tránh sự phân chia tiêu cực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())
        if n <= 2:
            print(1)
        else:
            # subtract first floor, then compute which floor
            floor = (n - 3) // x + 2
            print(floor)

if __name__ == "__main__":
    solve()
```Điều kiện đầu tiên xử lý trường hợp cạnh tầng một. Đối với tất cả các căn hộ khác, trừ đi$3$và thêm$2$trong công thức căn chỉnh chính xác số căn hộ với số tầng. Phép chia số nguyên đảm bảo rằng các căn hộ ở cuối tầng được tính vào đúng tầng. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
n = 7, x = 3
```Trừ tầng một:```
n_remaining = 7 - 2 = 5
```Tính tầng bổ sung:```
(7 - 3) // 3 + 2 = 4 // 3 + 2 = 1 + 2 = 3
```Kết quả phù hợp với sản lượng dự kiến. Một ví dụ khác:```
n = 1, x = 5
```Vì n <= 2 nên tầng 1 được trả về. 

| n | x | tính toán sàn | đầu ra | 
| --- | --- | --- | --- | 
| 7 | 3 | (7-3)//3+2 = 3 | 3 | 
| 1 | 5 | tầng 1 = 1 | 1 | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi test case được xử lý trong thời gian không đổi | 
| Không gian | O(1) | Chỉ một vài số nguyên được sử dụng cho mỗi trường hợp thử nghiệm | 

Với$t \le 1000$Và$n, x \le 1000$, giải pháp này thực thi thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("4\n7 3\n1 5\n22 5\n987 13\n") == "3\n1\n5\n77", "sample 1"

# Custom cases
assert run("3\n2 10\n3 1\n5 2\n") == "1\n2\n2", "edge cases first and small x"
assert run("2\n1000 50\n51 50\n") == "21\n2", "large n and boundary floor"
assert run("1\n4 1\n") == "3", "small x, multiple floors"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 10, 3 1, 5 2 | 1,2,2 | Vỏ cạnh có tầng một và x nhỏ | 
| 1000 50, 51 50 | 21,2 | N lớn, nhiều tầng, ranh giới | 
| 4 1 | 3 | Tối thiểu x, nhiều tầng | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi$n$rơi xuống tầng một. Giải pháp nhận diện chính xác căn hộ$1$Và$2$như đang ở trên sàn$1$. Một trường hợp tế nhị khác xảy ra khi$n$hạ cánh chính xác tại căn hộ cuối cùng của tầng sau tầng đầu tiên; công thức chia số nguyên tự động đặt nó vào đúng tầng. Ví dụ,$n = 5$,$x = 3$: trừ tầng 1 (2 căn hộ) → 3 căn hộ còn lại. Sau đó$(5-3)//3 + 2 = 2$xác định chính xác tầng 2.
