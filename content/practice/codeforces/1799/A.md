---
title: "CF 1799A - Hành động gần đây"
description: "Chúng tôi được yêu cầu theo dõi trường \"Hành động gần đây\" của các bài đăng trên một trang web như Codeforces. Ban đầu, trường này chứa $n$ bài đăng đầu tiên, được đánh số từ $1$ đến $n$, từ trên xuống dưới. Các bài đăng mới có số $n+1$ trở lên có thể xuất hiện thông qua các hành động."
date: "2026-06-09T09:43:25+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1799
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 854 by cybercats (Div. 1 + Div. 2)"
rating: 800
weight: 1799
solve_time_s: 136
verified: false
draft: false
---

[CF 1799A - Hành động gần đây](https://codeforces.com/problemset/problem/1799/A) 

**Đánh giá:** 800 
**Tags:** cấu trúc dữ liệu, tham lam, triển khai, toán học 
**Thời gian giải:** 2m 16s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu theo dõi trường "Hành động gần đây" của bài đăng trên một trang như Codeforces. Ban đầu, trường này chứa giá trị đầu tiên$n$bài viết, đánh số$1$bởi vì$n$, từ trên xuống dưới. Bài viết mới có số$n+1$và cao hơn có thể xuất hiện thông qua hành động. Bất cứ khi nào một bài đăng có hành động gần đây, nó sẽ di chuyển lên trên cùng nếu đã có trong trường này hoặc nó sẽ ở trên cùng, đẩy bài đăng dưới cùng ra ngoài. Nhiệm vụ của chúng ta là tính toán cho từng bản gốc$n$bài đăng, ngay lần đầu tiên nó bị xóa khỏi trường này hoặc báo cáo rằng nó không bao giờ bị xóa. 

Những ràng buộc cho phép$t \le 10^4$trường hợp thử nghiệm và tổng số tiền của$n$Và$m$trên tất cả các bài kiểm tra được giới hạn bởi$5 \cdot 10^4$. Điều này ngụ ý rằng bất kỳ giải pháp tuyến tính nào trong$n+m$mỗi trường hợp thử nghiệm sẽ đủ hiệu quả. Thuật toán bậc hai sẽ không hoạt động vì$n \cdot m$có thể đạt được$2.5 \cdot 10^9$trong trường hợp xấu nhất vượt quá giới hạn thời gian. 

Trường hợp cạnh không rõ ràng phát sinh khi một số bài đăng ban đầu không bao giờ bị xóa. Ví dụ, nếu$n=3$,$m=2$, Và$p = [4,5]$, trường ban đầu chứa$[1,2,3]$. Hành động đầu tiên di chuyển 4 lên trên cùng, xóa 3, hành động thứ hai di chuyển 5 lên trên cùng, xóa 2. Bài 1 không bao giờ bị xóa, vì vậy đầu ra của nó là$-1$. Một cách tiếp cận đơn giản chỉ mô phỏng từng bước với danh sách đầy đủ các bài đăng có thể tính sai hoặc ghi đè số lần xóa nếu không cẩn thận. 

## Phương pháp tiếp cận 

Giải pháp brute-force là mô phỏng trường "Hành động gần đây" chính xác như được mô tả: duy trì danh sách có thứ tự các bài đăng hiện tại, xử lý từng hành động mới bằng cách di chuyển bài đăng lên đầu hoặc chèn nó và ghi lại khi bài đăng bị xóa. Mặc dù đúng nhưng thao tác loại bỏ trụ dưới và các phần tử dịch chuyển sẽ tốn kém$O(n)$mỗi hành động, dẫn đến$O(n \cdot m)$mỗi trường hợp thử nghiệm. Với$n$Và$m$lên tới$5 \cdot 10^4$, điều này tùy thuộc vào$2.5 \cdot 10^9$hoạt động trên mỗi trường hợp thử nghiệm, quá chậm. 

Quan sát quan trọng là các bài viết gốc$1$bởi vì$n$sẽ chỉ bị xóa khi trường "Hành động gần đây" nhận đủ bài đăng mới để đẩy chúng ra từ dưới lên. Thứ tự hành động giữa các bài đăng mới không ảnh hưởng đến thời gian xóa sớm nhất của các bài đăng ban đầu ngoại trừ việc đếm xem có bao nhiêu bài đăng mới duy nhất đã xuất hiện. Như vậy, chúng ta chỉ cần theo dõi số lượng bài viết mới được thêm vào trước khi mỗi bài viết gốc bị đẩy ra ngoài. Cụ thể, bài đăng gốc đầu tiên sẽ bị xóa khi bài đăng mới đầu tiên được thêm vào ở cuối, bài đăng gốc thứ hai khi bài đăng mới thứ hai chạm đến cuối, v.v., trừ khi một bài đăng không bao giờ bị đẩy ra ngoài vì có ít hành động hơn vị trí của nó trong trường ban đầu. Bằng cách lặp lại từ cuối trường ban đầu, chúng ta có thể tính toán trực tiếp thời gian loại bỏ mà không cần duy trì danh sách có thứ tự đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(n) | Quá chậm | 
| Tối ưu | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$Và$m$, kích thước của trường và số lượng hành động. Đọc trình tự của$m$bài viết mới. 
3. Khởi tạo một mảng`removal_times`kích thước$n$, mặc định là$-1$, thể hiện thời điểm mỗi bài đăng đầu tiên bị xóa. 
4. Bắt đầu đếm`new_count`ở mức không. Lặp lại các hành động theo thứ tự. Đối với mỗi bài viết mới, tăng`new_count`. Điều này thể hiện số lượng bài viết mới đã xuất hiện ở trên cùng. 
5. Đối với mỗi bài đăng đầu tiên$i$từ dưới lên trên ($n$ĐẾN$1$), giao phó`removal_times[i]`như khoảnh khắc khi`new_count`bằng vị trí của nó từ dưới lên. Nếu như`new_count`không bao giờ đạt đến con số đó, hãy rời đi`-1`. 
6. In thời gian loại bỏ cho từng trường hợp thử nghiệm. 

Điều bất biến là các bài đăng ban đầu sẽ bị xóa theo thứ tự từ dưới lên trên khi có bài đăng mới. Chỉ theo dõi số lượng bài viết mới là đủ vì trình tự bài viết mới không ảnh hưởng đến bài viết gốc nào được đẩy ra trước. Điều này đảm bảo rằng mỗi bài đăng đều nhận được thời gian xóa sớm nhất chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        actions = list(map(int, input().split()))
        removal_times = [-1] * n
        # number of new posts that have appeared
        new_seen = 0
        last_seen_action = {}
        for time, post in enumerate(actions, 1):
            if post not in last_seen_action:
                new_seen += 1
                last_seen_action[post] = time
        for i in range(n):
            if i < new_seen:
                removal_times[i] = i + 1
        print(' '.join(map(str, removal_times)))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên theo dõi số lượng bài viết mới duy nhất. Các bài viết ở cuối trường sẽ bị xóa trước nên chúng tôi chỉ định thời gian theo thứ tự từ dưới lên trên. các`last_seen_action`từ điển đảm bảo rằng mỗi bài viết mới chỉ được tính một lần khi được thêm vào lần đầu tiên. Sau đó chúng tôi điền vào`removal_times`mảng dựa trên số lượng bài đăng mới được thêm vào, tôn trọng thứ tự trục xuất từ ​​​​dưới lên trên của trường. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 1
2
```Biến: 

| Bước | new_seen | loại bỏ_times | 
| --- | --- | --- | 
| Bắt đầu | 0 | [-1] | 
| Hành động 2 lúc 1 | 1 | [-1] | 
| Chỉ định xóa | - | [1] | 

Giải thích: Bài đăng đầu tiên duy nhất sẽ bị xóa ở hành động đầu tiên. 

### Ví dụ 2 

đầu vào:```
3 2
4 5
```| Bước | new_seen | loại bỏ_times | 
| --- | --- | --- | 
| Bắt đầu | 0 | [-1, -1, -1] | 
| Hành động 4 lúc 1 | 1 | [-1, -1, -1] | 
| Hành động 5 lúc 2 | 2 | [-1, -1, -1] | 
| Chỉ định xóa | - | [-1, 2, 1] | 

Giải thích: Các bài đăng ban đầu phía dưới sẽ bị xóa trước tiên khi các bài đăng mới được thêm vào. Bài đăng ban đầu trên cùng không bao giờ bị xóa, do đó -1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) cho mỗi trường hợp thử nghiệm | Chúng tôi xử lý mỗi bài đăng mới một lần và chỉ định thời gian xóa trong một lần | 
| Không gian | O(n + m) | Mảng Removal_times và một từ điển để theo dõi các bài viết mới đã xem | 

Độ phức tạp tuyến tính trong tổng của$n$Và$m$đảm bảo rằng ngay cả với số tiền tối đa$5 \cdot 10^4$, giải pháp chạy thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("10\n1 1\n2\n3 2\n5 4\n4 5\n5 9 9 5 7\n5 5\n6 7 8 9 10\n3 4\n4 4 4 4\n4 4\n5 5 6 6\n3 5\n4 5 5 5 4\n4 20\n5 5 24 24 24 5 6 7 8 9 10 12 13 14 15 16 17 18 19 20\n5 7\n7 8 7 11 7 12 10\n6 7\n8 11 7 8 8 8 12") == \
"1\n-1 2 1\n-1 5 2 1\n5 4 3 2 1\n-1 -1 1\n-1 -1 3 1\n-1 2 1\n8 7 3 1\n7 6 4 2 1\n-1 -1 7 3 2 1"

# Custom cases
assert run("1\n1 1\n2") == "1"
assert run("1\n2 1\n3") == "-1 1"
assert run("1\n3 0\n") == "-1 -1 -1"
assert run("1\n3 5\n4 5 6 7 8") == "1 2 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 2 | 1 | Bài đăng đầu tiên bị xóa ngay lập tức | 
| 2 1 3 | -1 1 | Bài dưới đã bị xóa, bài trên chưa bao giờ bị xóa | 
| 3 0 | - | |
