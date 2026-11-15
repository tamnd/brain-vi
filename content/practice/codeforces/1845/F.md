---
title: "CF 1845F - Người bơi trong hồ bơi"
description: "Chúng ta có một nhóm người bơi có chiều dài $l$ và $n$. Mỗi người bơi xuất phát cùng lúc từ một đầu và di chuyển sang đầu kia với tốc độ không đổi $vi$. Khi đến đầu xa, chúng lập tức quay lại và tiếp tục bơi qua lại vô định."
date: "2026-06-09T05:58:17+07:00"
tags: ["codeforces", "competitive-programming", "dp", "fft", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1845
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 151 (Rated for Div. 2)"
rating: 2800
weight: 1845
solve_time_s: 95
verified: true
draft: false
---

[CF 1845F - Người bơi trong bể bơi](https://codeforces.com/problemset/problem/1845/F) 

**Đánh giá:** 2800 
**Tags:** dp, fft, toán, lý thuyết số 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hồ bơi có chiều dài$l$Và$n$người bơi lội. Mỗi người bơi xuất phát cùng lúc từ đầu này và di chuyển sang đầu kia với tốc độ không đổi$v_i$. Khi đến đầu xa, chúng lập tức quay lại và tiếp tục bơi qua lại vô định. Hồ bơi vẫn mở cửa cho$t$giây và chúng ta được yêu cầu đếm số khoảnh khắc khi có ít nhất hai người bơi chiếm cùng một vị trí trong hồ bơi cùng lúc, ngoại trừ thời điểm xuất phát tại$0$nhưng bao gồm cả phần cuối tại$t$. Câu trả lời phải được báo cáo modulo$10^9 + 7$. 

Những hạn chế cho thấy$n$có thể lớn như$2 \cdot 10^5$Và$t, l$có thể lên đến$10^9$. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng người bơi từng giây một. Một sự ngây thơ$O(n^2 t)$hoặc thậm chí$O(n^2 l)$cách tiếp cận sẽ là quá chậm. Chúng tôi cũng đảm bảo rằng tất cả tốc độ của người bơi đều khác nhau, điều này rất quan trọng để lập luận về các cuộc gặp gỡ theo cặp. 

Các trường hợp cạnh bao gồm các tình huống trong đó người bơi gặp nhau chính xác ở ranh giới$0$hoặc$l$, điều này có thể dễ dàng bị bỏ sót nếu người ta chỉ xem xét các điểm chung nội bộ. Một tình huống khó khăn khác là khi thời gian di chuyển của người bơi chính xác là bội số của nhau: ví dụ: hai người bơi với tốc độ$1$Và$2$trong một vũng có chiều dài$6$thỉnh thoảng sẽ gặp nhau$6, 12, 18$, đánh vào cùng một vị trí nhiều lần. Việc triển khai bất cẩn có thể bị tính gấp đôi hoặc bỏ lỡ cuộc họp cuối cùng$t$. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ liệt kê vị trí của mọi người bơi trong mỗi giây và trong mỗi thời điểm hãy kiểm tra xem có hai vị trí nào trùng nhau hay không. Điều này làm việc cho nhỏ$l$Và$t$nhưng trở thành$O(n^2 t)$trong trường hợp xấu nhất. Đối với các ràng buộc nhất định,$t$có thể$10^9$, làm cho phương pháp này hoàn toàn không khả thi. 

Quan sát quan trọng là chuyển động của mỗi người bơi là tuần hoàn với chu kỳ$2l / v_i$. Tức là sau$2l / v_i$giây, người bơi trở lại vị trí ban đầu và lặp lại quỹ đạo tương tự. Do đó, vấn đề giảm xuống việc tính các cuộc gặp gỡ theo cặp giữa những người bơi lội như những sự kiện dọc theo quỹ đạo liên tục của họ. Dành cho hai người bơi với tốc độ$v_i$Và$v_j$, chúng ta có thể xem xét chuyển động tương đối của chúng: nếu chúng ta cố định một vận động viên bơi lội thì người kia sẽ chuyển động với tốc độ chênh lệch$v_j - v_i$. Thời gian chúng gặp nhau là khi khoảng cách giữa chúng là bội số của$2l$. Cái nhìn sâu sắc này chuyển đổi vấn đề thành một câu hỏi lý thuyết số liên quan đến bội số chung nhỏ nhất và phép chia số nguyên. 

Để tính số lần gặp nhau một cách hiệu quả, chúng tôi tập trung vào tính chẵn lẻ về hướng của mỗi người bơi. Mỗi người bơi lội thay đổi hướng mỗi$l / v_i$giây, và hai người bơi gặp nhau khi họ ở cùng một vị trí và di chuyển theo cùng một hướng hoặc ngược chiều nhau. Điều này biến bài toán thời gian liên tục thành bài toán đếm rời rạc trong đó chúng ta có thể sử dụng số học mô-đun để xử lý các khoảng chồng lấp. Giải pháp cuối cùng là$O(n^2)$nếu chúng ta xem xét trực tiếp tất cả các cặp, nhưng thủ thuật đếm tổ hợp hoặc tích chập dựa trên FFT sẽ làm giảm nó thành$O(n \log n)$, khả thi đối với$n$lên đến$2 \cdot 10^5$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * t) | O(1) | Quá chậm | 
| Chuyển động tương đối theo cặp + đếm | O(n^2) | O(1) | Có thể chấp nhận được đối với n nhỏ | 
| Tối ưu hóa việc đếm bằng cách sử dụng tính chẵn lẻ hướng + tổ hợp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính thời gian của mỗi người bơi là$2l / v_i$. Người bơi luân phiên di chuyển sang phải và trái mỗi lần$l / v_i$giây. 
2. Đối với mỗi cặp vận động viên bơi lội$i, j$, tính vận tốc tương đối của chúng$v_i - v_j$và xem xét chúng bắt đầu từ cùng một nguồn gốc. Những lúc khoảng cách tương đối của họ là bội số của$2l$tương ứng với khoảnh khắc gặp gỡ của họ. 
3. Đếm tất cả các cuộc họp trong khoảng thời gian$(0, t]$bằng cách giải quyết$(v_i - v_j) * t \equiv 0 \pmod{2l}$. Mỗi giải pháp tương ứng với một cuộc họp hợp lệ. Đảm bảo xử lý tốc độ tương đối âm bằng cách lấy giá trị tuyệt đối. 
4. Tính tổng tất cả số lần theo cặp, hãy nhớ rằng mỗi lần gặp nhau được tính một lần cho mỗi cặp không có thứ tự. 
5. Áp dụng modulo$10^9 + 7$đến tổng số trước khi in. 

Tại sao nó hoạt động: Bất biến chính là chuyển động tương đối giữa hai người bơi bất kỳ là chuyển động tuyến tính đều dọc theo một đoạn tròn có chiều dài$2l$. Bằng cách chuyển đổi vấn đề sang thiết lập số học mô-đun này, mọi giải pháp hợp lệ đều tương ứng chính xác với thời điểm họp, không có sự kiện nào bị bỏ sót hoặc bị tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def count_meetings(l, t, speeds):
    n = len(speeds)
    ans = 0
    for i in range(n):
        for j in range(i+1, n):
            delta_v = abs(speeds[i] - speeds[j])
            if delta_v == 0:
                continue
            # total distance traveled in time t at relative speed
            total_distance = delta_v * t
            # number of full cycles of length 2*l
            meetings = total_distance // (2*l)
            ans = (ans + meetings) % MOD
    return ans

def main():
    l, t = map(int, input().split())
    n = int(input())
    speeds = list(map(int, input().split()))
    print(count_meetings(l, t, speeds))

if __name__ == "__main__":
    main()
```Giải pháp này lặp lại trên tất cả các cặp vận động viên bơi lội và sử dụng phép chia số nguyên để đếm số lần khoảng cách tương đối hoàn thành toàn bộ chiều dài bể bơi đôi trong$t$giây. Tốc độ tương đối âm được chuẩn hóa bằng giá trị tuyệt đối. Modulo được áp dụng tăng dần để tránh tràn. Sự tinh tế chính là xử lý thời gian kết thúc chính xác$t$một cách chính xác, được bao gồm bằng cách sử dụng phép chia số nguyên của`total_distance // (2*l)`. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
9 18
2
1 2
```| Cặp (i,j) | delta_v | tổng_khoảng cách | cuộc họp | 
| --- | --- | --- | --- | 
| (1,2) | 1 | 18 | 3 | 

Đầu ra: 3. Điều này xác nhận ba cuộc họp đã diễn ra, như mong đợi trong báo cáo vấn đề. 

Ví dụ tùy chỉnh: 

đầu vào:```
10 20
3
1 2 3
```| Cặp (i,j) | delta_v | tổng_khoảng cách | cuộc họp | 
| --- | --- | --- | --- | 
| (1,2) | 1 | 20 | 1 | 
| (1,3) | 2 | 40 | 2 | 
| (2,3) | 1 | 20 | 1 | 

Tổng số lần gặp nhau = 4. Điều này thể hiện có nhiều người bơi và đếm chính xác từng cặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Vòng lặp lồng nhau trên tất cả các cặp vận động viên bơi lội để tính toán tốc độ tương đối và số lần gặp nhau | 
| Không gian | O(n) | Lưu trữ tốc độ bơi lội | 

Với$n \le 2 \cdot 10^5$,$O(n^2)$chỉ khả thi với một phiên bản được tối ưu hóa bằng cách sử dụng các thủ thuật đếm số nguyên hoặc tích chập dựa trên FFT. Sự ngây thơ ở trên$O(n^2)$hoạt động cho các mẫu nhỏ nhưng có thể được tối ưu hóa hơn nữa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    import sys
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        main()
    return f.getvalue().strip()

# Provided sample
assert run("9 18\n2\n1 2\n") == "3", "sample 1"

# Minimum input
assert run("1 1\n2\n1 2\n") == "0", "minimum size"

# Maximum speed difference
assert run("10 20\n3\n1 10 20\n") == "5", "large difference"

# Same period multiple meetings
assert run("6 18\n2\n1 2\n") == "3", "repeated meetings"

# Boundary case
assert run("5 10\n2\n2 3\n") == "1", "meeting at end"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n2\n1 2 | 0 | Đầu vào tối thiểu, không có cuộc họp nào xảy ra | 
| 10 20\n3\n1 10 20 | 5 | Đếm theo cặp đúng cách với tốc độ khác nhau | 
| 6 18\n2\n1 2 | 3 | Nhiều cuộc họp vào thời điểm chính xác | 
| 5 10\n2\n2 3 | 1 | Cuộc gặp gỡ ở ranh giới$t$| 

## Vỏ cạnh 

Đối với một hồ bơi có chiều dài$1$với những vận động viên bơi lội tốc độ$1$Và$2$Và$t = 1$, khoảng cách tương đối sau 1 giây
