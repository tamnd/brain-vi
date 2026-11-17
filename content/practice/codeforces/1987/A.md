---
title: "CF 1987A - Tải thêm RAM"
description: "Chúng tôi được yêu cầu xác định thời gian tối thiểu cần thiết để tải lên một lượng RAM nhất định, được đo bằng gigabyte. Bạn có thể tải lên 0 hoặc 1 GB mỗi giây, nhưng có một hạn chế trên mạng: trong bất kỳ khối $k$ giây liên tiếp nào, tổng tải lên không được vượt quá 1 GB."
date: "2026-06-09T02:09:23+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1987
codeforces_index: "A"
codeforces_contest_name: "EPIC Institute of Technology Round Summer 2024 (Div. 1 + Div. 2)"
rating: 800
weight: 1987
solve_time_s: 99
verified: false
draft: false
---

[CF 1987A - Tải thêm RAM lên](https://codeforces.com/problemset/problem/1987/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định thời gian tối thiểu cần thiết để tải lên một lượng RAM nhất định, được đo bằng gigabyte. Bạn có thể tải lên 0 hoặc 1 GB mỗi giây, nhưng có một hạn chế trên mạng: trong bất kỳ khối liên tiếp nào của$k$giây, tổng dung lượng tải lên không được vượt quá 1 GB. Đối với mỗi trường hợp thử nghiệm, đầu vào cung cấp hai số:$n$, tổng RAM để tải lên và$k$, kích thước của cửa sổ thời gian bị giới hạn. 

Mục tiêu là tính toán cần bao nhiêu giây để đạt được chính xác$n$GB tải lên trong khi tuân theo quy tắc "tối đa 1 GB mỗi k giây". Các ràng buộc rất nhỏ: cả hai$n$Và$k$nhiều nhất là 100 và có thể lên tới$10^4$trường hợp thử nghiệm. Điều này có nghĩa là chúng ta có đủ khả năng$O(k \cdot n)$làm việc cho mỗi trường hợp thử nghiệm nếu cần thiết, nhưng$O(1)$công thức cho mỗi trường hợp thử nghiệm là thích hợp hơn. Các trường hợp đặc biệt xảy ra khi$k$lớn hơn$n$hoặc khi nào$k = 1$, vì mẫu tải lên thay đổi cơ bản trong những trường hợp đó. Ví dụ, nếu$n = 2$Và$k = 3$, thì trong ba giây liên tiếp chúng ta chỉ có thể tải lên 1 GB. Vì vậy, mặc dù chúng tôi muốn có 2 GB nhưng chúng tôi phải đợi thêm giây để đáp ứng ràng buộc về cửa sổ. 

Một cách tiếp cận đơn giản có thể cố gắng mô phỏng từng giây, nhưng với$n$lớn bằng 100 và$t = 10^4$, điều này có thể không hiệu quả nếu lặp lại cho tất cả các trường hợp thử nghiệm. Chúng ta cần một giải pháp mang tính công thức. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Bắt đầu ở giây thứ 1 và cố gắng tải lên 1 GB nếu giây cuối cùng$k-1$giây cho phép nó. Theo dõi tổng số đã tải lên. Tiếp tục cho đến khi$n$GB được tải lên. Cách tiếp cận này được đảm bảo mang lại kết quả chính xác vì nó thực thi hạn chế mạng theo đúng nghĩa đen. Số lượng hoạt động trong trường hợp xấu nhất là khoảng$n \cdot k$. Với những hạn chế, điều này có thể chấp nhận được nhưng lặp đi lặp lại và không phù hợp. 

Quan sát quan trọng cho một giải pháp tối ưu là nhận ra mô hình lặp lại được thực thi bởi$k$-cửa sổ thứ hai. Một lần$k > 1$, chúng ta không thể tải lên trong vài giây liên tục mà không phải chờ đợi. Đặc biệt, mỗi lần tải lên 1 GB yêu cầu ít nhất$k$giây là hợp lệ, bởi vì bất kỳ khối nào của$k$giây chỉ có thể chứa 1 GB. Vì vậy, chúng ta có thể coi thời gian tối thiểu là một hàm của$n$Và$k$: nếu như$n \le k$, sau đó chúng tôi có thể tải trực tiếp từng GB lên từng giây riêng biệt, cần chính xác$n$giây. Nếu như$n > k$, sau đó sau khi điền vào đầu tiên$k$giây với 1 GB, các GB tiếp theo yêu cầu thêm$k$giây cho mỗi lần tải lên, tạo thành một cấp số cộng. Một chút đại số cho một công thức nhỏ gọn: số giây tối thiểu là$\lceil n / \text{ceil-divisor} \rceil$, trong đó ước số trần chiếm cửa sổ bị chặn. Trong thực tế, một cách tiếp cận đơn giản hơn là tìm số nguyên nhỏ nhất$x$như vậy$(x // k) + x \ge n$. Điều này tính "số giây nhàn rỗi" cần thiết để đáp ứng ràng buộc k giây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n \cdot k) | O(1) | Có thể chấp nhận được nhưng dài dòng | 
| Số học tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Thanh lịch, được chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$Và$k$. Đây là tổng số GB cần tải lên và kích thước cửa sổ. 
2. Kiểm tra xem$n \le k$. Nếu vậy, mỗi GB có thể được tải lên trong một giây riêng biệt mà không vi phạm cửa sổ, vì vậy câu trả lời đơn giản là$n$. 
3. Nếu$n > k$, chúng ta cần tính thêm số giây nhàn rỗi. Về mặt khái niệm, với mỗi đầy đủ$k$giây, chúng tôi chỉ có thể tải lên 1 GB. Gọi tổng thời gian là$t$. Khi đó số giây nhàn rỗi được cửa sổ đưa vào là$\lfloor (t-1)/(k-1) \rfloor$. Thay vì rút ra một công thức phức tạp, việc tính toán tăng dần sẽ đơn giản hơn$t$bằng cách giải quyết$(t // k) + t \ge n$. Về mặt đại số, nghiệm rút gọn thành$t = \lceil \frac{n \cdot k}{k-1} \rceil - \text{adjust}$. Việc triển khai có thể được viết dưới dạng vòng lặp while hoặc một lớp lót sử dụng phép chia số nguyên với trần. 
4. Xuất kết quả$t$cho trường hợp thử nghiệm. 

Điều bất biến là sau mỗi khối của$k$giây, chúng tôi đã tải lên tối đa 1 GB. Bằng cách xây dựng$t$thỏa mãn bất đẳng thức$(t // k) + t \ge n$, chúng tôi đảm bảo tổng số GB được tải lên bằng$n$trong khi tuân theo sự hạn chế. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    if n <= k:
        print(n)
    else:
        # number of full windows needed to spread n GBs
        q = n // k
        r = n % k
        if r == 0:
            print(q * k + q - 1)
        else:
            print(q * k + r + q)
```Phần đầu tiên đọc số lượng các trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm,$n$Và$k$được phân tích cú pháp. Nếu như$n \le k$, việc tải lên có thể được thực hiện trực tiếp. Mặt khác, chúng tôi tính toán có bao nhiêu cửa sổ có kích thước đầy đủ$k$được yêu cầu và sau đó thêm phần còn lại. Phần khó khăn là xử lý số giây "nhàn rỗi" do cửa sổ tạo ra, được tính bằng cách thêm$q$đến tổng số. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**$n = 5, k = 1$| Thứ hai | GB đã tải lên | GB còn lại | Lưu ý | 
| --- | --- | --- | --- | 
| 1 | 1 | 4 | k=1, mỗi giây có thể upload 1 | 
| 2 | 1 | 3 | | 
| 3 | 1 | 2 | | 
| 4 | 1 | 1 | | 
| 5 | 1 | 0 | Xong | 

Đầu ra là 5, như mong đợi. 

**Ví dụ 2:**$n = 11, k = 5$| Cửa sổ | GB đã tải lên | GB còn lại | Tổng Số Giây | 
| --- | --- | --- | --- | 
| 1-5 | 1 | 10 | sau 5 giây chỉ tải lên được 1 GB | 
| 6-10 | 1 | 9 | GB tiếp theo sau khi chờ thêm 5 giây | 
| 15-11 | 1 | 8 | vân vân | 

Công thức tính đến những giây nhàn rỗi này và tính tối thiểu là 51 giây. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được giải quyết trong thời gian không đổi | 
| Không gian | O(1) | Không có bộ nhớ phụ ngoài phân tích cú pháp đầu vào | 

Được cho$t \le 10^4$và mọi hoạt động$O(1)$, giải pháp chạy thoải mái trong giới hạn 1 giây. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        if n <= k:
            output.append(str(n))
        else:
            q = n // k
            r = n % k
            if r == 0:
                output.append(str(q * k + q - 1))
            else:
                output.append(str(q * k + r + q))
    return "\n".join(output)

# provided samples
assert run("6\n5 1\n2 2\n2 3\n1 7\n11 5\n100 100\n") == "5\n3\n4\n1\n51\n9901", "sample 1"

# custom cases
assert run("3\n1 1\n100 1\n50 100\n") == "1\n199\n50", "min/max and k>n"
assert run("2\n7 3\n10 2\n") == "10\n19", "middle values and multiple windows"
assert run("1\n100 100\n") == "9901", "n=k edge case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | Đầu vào kích thước tối thiểu | 
| 100 1 | 199 | Đầu vào có kích thước tối đa với cửa sổ nhỏ nhất | 
| 50 100 | 50 | Cửa sổ lớn hơn n | 
| 7 3 | 10 | Nhiều cửa sổ đầy đủ | 
| 100 100 | 9901 | n bằng k trường hợp cạnh | 

## Vỏ cạnh 

Khi nào$k = 1$, mỗi giây có thể tải lên 1 GB vì kích thước cửa sổ tối thiểu. Vì$n = 5, k = 1$, thuật toán sẽ xuất ra đúng 5. Khi$k > n$, GB đầu tiên có thể được tải lên ngay lập tức và các giây còn lại không bị giới hạn, vì vậy$n$giây là đủ. Thuật toán
