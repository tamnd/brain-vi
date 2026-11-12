---
title: "CF 1623A - Robot dọn dẹp"
description: "Chúng ta có một robot di chuyển bên trong một căn phòng hình chữ nhật có $n$ hàng và $m$ cột. Robot bắt đầu tại ô $(rb, cb)$ và di chuyển theo đường chéo: xuống một bước và sang phải một bước tại một thời điểm."
date: "2026-06-10T05:40:48+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1623
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 763 (Div. 2)"
rating: 800
weight: 1623
solve_time_s: 84
verified: true
draft: false
---

[CF 1623A - Robot dọn dẹp](https://codeforces.com/problemset/problem/1623/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một robot đang di chuyển bên trong một căn phòng hình chữ nhật có$n$hàng và$m$cột. Robot bắt đầu ở một ô nào đó$(r_b, c_b)$và di chuyển theo đường chéo: xuống từng bước một và sang phải một bước. Căn phòng có tường nên bất cứ khi nào robot di chuyển qua một ranh giới, nó sẽ "nảy" ra khỏi ranh giới đó và đảo ngược hướng dọc theo trục đó. Ngoài việc di chuyển, robot còn dọn dẹp tất cả các ô trong hàng và cột hiện tại mỗi giây. Mục tiêu là xác định xem robot sẽ mất bao lâu để làm sạch một ô bẩn duy nhất tại một thời điểm.$(r_d, c_d)$. 

Đầu vào cung cấp nhiều trường hợp kiểm thử, mỗi trường hợp chỉ định kích thước phòng, vị trí bắt đầu của robot và vị trí ô bẩn. Đầu ra là thời gian, tính bằng giây, khi robot làm sạch ô bẩn lần đầu tiên. 

Các hạn chế là nhỏ:$n, m \le 100$và lên đến$10^4$trường hợp thử nghiệm. Điều này có nghĩa là chúng ta có thể mô phỏng chuyển động của robot từng bước một, vì ngay cả trong trường hợp xấu nhất, mỗi trường hợp thử nghiệm sẽ bao gồm tối đa khoảng 200 bước trước khi robot quay lại vị trí và hướng trước đó. Tối ưu hóa quy mô lớn là không cần thiết; sự đúng đắn và xử lý cẩn thận các ranh giới là quan trọng hơn. 

Các trường hợp cạnh cần chú ý bao gồm các tình huống trong đó robot bắt đầu ở cùng hàng hoặc cột với ô bẩn, điều này sẽ mang lại thời gian bằng 0. Một trường hợp khó phát hiện khác là khi ô bẩn nằm ở một góc, điều này có thể trùng khớp với nhiều hướng đảo ngược trong một bước mô phỏng nếu các ranh giới được xử lý bất cẩn. 

## Phương pháp tiếp cận 

Phương pháp brute-force là mô phỏng từng giây chuyển động của robot, cập nhật hàng và cột theo hướng hiện tại, phản chiếu khỏi các bức tường khi cần thiết và kiểm tra xem hàng hoặc cột hiện tại có khớp với ô bẩn hay không. Điều này hoạt động vì$n$Và$m$nhỏ. Đối với mỗi trường hợp thử nghiệm, robot sẽ không bao giờ cần nhiều hơn$2(n-1) + 2(m-1)$các bước để tiếp cận ô bẩn dọc theo đường chéo của nó. Do đó, cách tiếp cận vũ phu có thể được chấp nhận ở đây. 

Một thông tin chi tiết quan trọng là chúng ta không cần theo dõi toàn bộ đường đi của robot, chỉ cần theo dõi vị trí và hướng chuyển động của nó. Mỗi bước đều mang tính xác định và robot sẽ dọn sạch toàn bộ hàng và cột ở vị trí hiện tại, vì vậy chúng ta chỉ cần kiểm tra xem hàng hoặc cột hiện tại có khớp với ô bẩn hay không. Logic phản ánh rất đơn giản: nếu hàng tiếp theo vượt quá giới hạn phòng, chúng ta sẽ lật theo hướng dọc; nếu cột tiếp theo vượt quá giới hạn phòng thì chúng ta lật theo hướng ngang. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n + m) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 
| Tối ưu (mô phỏng có phản chiếu) | O(n + m) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Lặp lại từng trường hợp thử nghiệm. 
2. Đối với mỗi trường hợp thử nghiệm, hãy đọc kích thước phòng$n$Và$m$, vị trí ban đầu của robot$(r_b, c_b)$, và ô bẩn$(r_d, c_d)$. 
3. Khởi tạo hướng chuyển động của robot thành$(dr, dc) = (1, 1)$. 
4. Khởi tạo biến hẹn giờ`time = 0`. 
5. Lặp lại khi robot chưa làm sạch ô bẩn: 

1. Kiểm tra xem hàng hiện tại của robot có`r_b`bằng`r_d`hoặc cột hiện tại của nó`c_b`bằng`c_d`. Nếu vậy thì hãy ngắt vòng lặp vì ô bẩn đã được làm sạch. 
2. Tính hàng tiếp theo`next_r = r_b + dr`và cột tiếp theo`next_c = c_b + dc`. 
3. Nếu`next_r`sẽ vượt quá ranh giới phòng (nhỏ hơn 1 hoặc lớn hơn$n$), đảo ngược`dr`và tính toán lại`next_r`. 
4. Nếu`next_c`sẽ vượt quá ranh giới phòng (nhỏ hơn 1 hoặc lớn hơn$m$), đảo ngược`dc`và tính toán lại`next_c`. 
5. Cập nhật vị trí của robot thành`(r_b, c_b) = (next_r, next_c)`. 
6. Tăng dần`time`bởi một. 
6. Xuất ra`time`cho trường hợp thử nghiệm hiện tại. 

Tại sao nó hoạt động: ở mỗi bước, robot sẽ di chuyển theo đường chéo hoặc phản chiếu vào tường, điều này đảm bảo rằng cuối cùng nó sẽ đến được một hàng hoặc cột giao với ô bẩn. Vì căn phòng là hữu hạn và chuyển động là tuần hoàn nên robot không thể đi vào một vòng lặp vô hạn nếu không làm sạch ô bẩn. Mỗi giây tương ứng chính xác với một bước chuyển động nên bộ đếm thời gian sẽ theo dõi số giây cho đến khi làm sạch. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def robot_cleaner():
    t = int(input())
    for _ in range(t):
        n, m, r_b, c_b, r_d, c_d = map(int, input().split())
        dr, dc = 1, 1
        time = 0
        while r_b != r_d and c_b != c_d:
            if r_b + dr < 1 or r_b + dr > n:
                dr = -dr
            if c_b + dc < 1 or c_b + dc > m:
                dc = -dc
            r_b += dr
            c_b += dc
            time += 1
        print(time)

robot_cleaner()
```Trong giải pháp,`dr`Và`dc`theo dõi các hướng dọc và ngang. Thứ tự thực hiện đảm bảo rằng hướng sẽ được đảo ngược trước khi di chuyển nếu robot băng qua tường. Vòng lặp while kiểm tra xem robot đã dọn sạch ô bẩn trước khi di chuyển hay chưa. Điều này tránh được từng lỗi một khi đếm giây. 

## Ví dụ đã hoạt động 

Dấu vết mẫu 1:`10 10 6 1 2 8`| thời gian | r_b | c_b | tiến sĩ | dc | được làm sạch? | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 6 | 1 | 1 | 1 | Không | 
| 1 | 7 | 2 | 1 | 1 | Không | 
| 2 | 8 | 3 | 1 | 1 | Không | 
| 3 | 9 | 4 | 1 | 1 | Không | 
| 4 | 10 | 5 | 1 | 1 | Không | 
| 5 | 9 | 6 | -1 | 1 | Không | 
| 6 | 8 | 7 | -1 | 1 | Không | 
| 7 | 7 | 8 | -1 | 1 | Có | 

Ô bẩn được làm sạch tại thời điểm thứ 7, phù hợp với kết quả đầu ra dự kiến. 

Dấu vết mẫu 2:`2 2 1 1 2 1`| thời gian | r_b | c_b | tiến sĩ | dc | được làm sạch? | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 1 | Có | 

Robot khởi động cùng cột với ô bẩn nên ô bẩn được làm sạch ngay tại thời điểm 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) cho mỗi trường hợp thử nghiệm | Mỗi bước di chuyển robot theo đường chéo và robot sẽ đánh vào mỗi hàng và cột nhiều nhất một lần trước khi dọn sạch ô bẩn. | 
| Không gian | O(1) | Chỉ một số số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm; không có bộ nhớ bổ sung tỷ lệ thuận với$n$hoặc$m$là cần thiết. | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm là có thể chấp nhận được vì$t \le 10^4$Và$n, m \le 100$, dẫn đến tối đa 2 triệu lần lặp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    robot_cleaner()
    return output.getvalue().strip()

# provided samples
assert run("5\n10 10 6 1 2 8\n10 10 9 9 1 1\n9 8 5 6 2 1\n6 9 2 2 5 8\n2 2 1 1 2 1\n") == "7\n10\n9\n3\n0", "sample 1"

# custom cases
assert run("1\n1 1 1 1 1 1\n") == "0", "single cell room"
assert run("1\n5 5 3 3 3 5\n") == "0", "same row as dirty cell"
assert run("1\n5 5 3 3 5 3\n") == "0", "same column as dirty cell"
assert run("1\n5 5 1 1 5 5\n") == "4", "opposite corner"
assert run("1\n5 5 5 5 1 1\n") == "4", "other corner diagonal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 1 1 | 0 | Phòng có kích thước tối thiểu | 
| 5 5 3 3 3 5 | 0 | Robot xuất phát cùng hàng với ô bẩn | 
| 5 5 | | |
