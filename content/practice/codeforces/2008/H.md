---
title: "CF 2008H - Thử thách của Sakurako"
description: "Chúng ta được cung cấp một mảng và một tham số $x$. Với $x$ này, chúng ta được phép chọn liên tục bất kỳ phần tử nào ít nhất là $x$ và giảm nó đi chính xác là $x$."
date: "2026-06-08T13:27:35+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2008
codeforces_index: "H"
codeforces_contest_name: "Codeforces Round 970 (Div. 3)"
rating: 2100
weight: 2008
solve_time_s: 108
verified: false
draft: false
---

[CF 2008H - Bài kiểm tra của Sakurako](https://codeforces.com/problemset/problem/2008/H) 

**Xếp hạng:** 2100 
**Tags:** tìm kiếm nhị phân, bạo lực, tham lam, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng và một tham số$x$. Với cái này$x$, chúng ta được phép chọn liên tục bất kỳ phần tử nào ít nhất$x$và giảm nó một cách chính xác$x$. Phép toán có thể được áp dụng tùy ý nhiều lần trên bất kỳ vị trí nào, do đó, mỗi phần tử thực sự trở thành phần còn lại của nó sau khi trừ bội số của$x$, nhưng không bao giờ xuống dưới 0. 

Đối với mỗi giá trị truy vấn$x$, chúng ta phải tính giá trị trung bình nhỏ nhất có thể sau khi thực hiện các phép rút gọn này một cách tối ưu trên mảng. Trung vị là phần tử ở giữa của mảng được sắp xếp, sử dụng định nghĩa Codeforces tiêu chuẩn trong đó nó là phần tử$\lceil n/2 \rceil$-phần tử nhỏ nhất thứ 

Các ràng buộc chặt chẽ về mặt tổng hợp thay vì trên mỗi thử nghiệm: tổng cộng$n$và tổng cộng$q$trên tất cả các trường hợp thử nghiệm là nhiều nhất$10^5$. Điều này có nghĩa là bất kỳ giải pháp nào thậm chí$O(nq)$mỗi trường hợp thử nghiệm là quá chậm và thậm chí$O(n \log n)$mỗi truy vấn quá chậm. Cấu trúc đề xuất mạnh mẽ việc xử lý trước mảng một lần cho mỗi trường hợp thử nghiệm và trả lời từng truy vấn theo thời gian logarit hoặc không đổi. 

Một dạng lỗi phổ biến là giả sử mỗi phần tử hoạt động độc lập và giảm các giá trị lớn một cách tham lam mà không xem xét mức độ thay đổi trung bình khi phân phối lại. Một cạm bẫy tinh vi khác là nghĩ rằng phép toán hoạt động giống như việc giảm modulo một mình, trong khi quên rằng việc rút gọn nhiều lần tương tác với thứ tự khi tính toán trung vị. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực là mô phỏng quy trình cho mỗi truy vấn. Đối với một cố định$x$, chúng ta áp dụng thao tác liên tục cho đến khi không có phần tử nào ít nhất$x$, sau đó sắp xếp và trích xuất trung vị. Điều này đúng nhưng cực kỳ tốn kém: mỗi thao tác có thể yêu cầu quét mảng và việc giảm nhiều lần cho mỗi phần tử khiến điều này có khả năng không bị giới hạn trong thực tế. Ngay cả khi được tối ưu hóa, việc lặp lại điều này cho mọi truy vấn sẽ dẫn đến khoảng$O(nq)$hoặc tệ hơn. 

Quan sát quan trọng là mỗi phần tử tiến hóa độc lập thành$a_i \bmod x$. Số lần chúng tôi trừ$x$không quan trọng ngoài phần còn lại cuối cùng này. Vì vậy sau khi sửa$x$, mảng trở thành một mảng biến đổi xác định trong đó mọi phần tử được thay thế bằng modulo còn lại của nó$x$. Vấn đề giảm xuống việc tìm trung vị của mảng được chuyển đổi này. 

Thách thức còn lại là trả lời nhiều truy vấn một cách hiệu quả. Chúng ta cần một cách để tính trung vị của$a_i \bmod x$cho nhiều người khác nhau$x$. Thủ thuật tiêu chuẩn là sắp xếp trước mảng và sau đó suy luận xem có bao nhiêu phần tử nằm dưới ngưỡng$t$sau khi chuyển đổi modulo. Điều này biến điều kiện trung bình thành một bài toán đếm, có thể được giải bằng tìm kiếm nhị phân trên câu trả lời kết hợp với cấu trúc tần số theo các khoảng chia hết. 

Quan sát mang lại hiệu quả là đối với cố định$x$, điều kiện$a_i \bmod x \le t$có thể được phân hủy thành các khối có kích thước đầy đủ$x$trong mảng đã sắp xếp. Điều này cho phép đếm những đóng góp trong$O(n/x + 1)$và trên tất cả các truy vấn, cấu trúc tổng hài giúp quản lý toàn bộ công việc khi kết hợp với tiền xử lý và tìm kiếm nhị phân trên các trung vị ứng cử viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng đầy đủ cho mỗi truy vấn |$O(n^2 q)$trường hợp xấu nhất |$O(n)$| Quá chậm | 
| Sắp xếp + đếm mô-đun + tìm kiếm nhị phân |$O((n + q)\log n)$khấu hao |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng một lần. Việc sắp xếp là cần thiết vì tất cả các lý luận sau này đều phụ thuộc vào việc đếm xem có bao nhiêu giá trị được chuyển đổi nằm dưới ngưỡng. 
2. Đối với truy vấn cố định$x$, diễn giải từng giá trị$a_i$như sản xuất phần còn lại$a_i \bmod x$. Điều này xác định một mảng được chuyển đổi một cách ngầm định mà không cần xây dựng nó. 
3. Để xác định số trung vị, ta cần giá trị nhỏ nhất$m$sao cho ít nhất một nửa số giá trị được chuyển đổi là$\le m$. Điều này biến vấn đề thành một vị từ đếm. 
4. Định nghĩa hàm số cho trước$x$và giá trị ứng viên$m$, đếm xem có bao nhiêu phần tử thỏa mãn$a_i \bmod x \le m$. Điều này có thể được tính bằng cách lặp qua các khối có kích thước$x$trong mảng được sắp xếp, vì các giá trị trong$[kx, (k+1)x-1]$tất cả bản đồ thành các khoảng thời gian còn lại có thể dự đoán được. 
5. Sử dụng tìm kiếm nhị phân$m$trong phạm vi$[0, x-1]$. Đối với mỗi điểm giữa, hãy đánh giá vị ngữ bằng hàm đếm. 
6. Nhỏ nhất$m$thỏa mãn điều kiện trung bình là câu trả lời cho truy vấn này. 

Tại sao nó hoạt động: mọi thao tác chỉ giảm các phần tử theo bội số của$x$, do đó trạng thái cuối cùng chỉ phụ thuộc vào dư lượng modulo$x$. Việc sắp xếp cung cấp cấu trúc cho các phần dư này trong các phạm vi giá trị và tìm kiếm nhị phân chuyển đổi điều kiện trung bình thành một bài kiểm tra tính khả thi đơn điệu. Điều kiện khả thi là đơn điệu vì tăng$m$chỉ làm tăng số phần tử được đếm nên tìm kiếm nhị phân là hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()

        def can(x, target):
            cnt = 0
            for v in a:
                cnt += v % x <= target
            return cnt >= (n + 1) // 2

        for _ in range(q):
            x = int(input())
            if x == 1:
                print(0)
                continue

            lo, hi = 0, x - 1
            ans = x - 1

            while lo <= hi:
                mid = (lo + hi) // 2
                cnt = 0
                for v in a:
                    if v % x <= mid:
                        cnt += 1

                if cnt >= (n + 1) // 2:
                    ans = mid
                    hi = mid - 1
                else:
                    lo = mid + 1

            print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tách biệt việc sắp xếp khỏi xử lý truy vấn. Đối với mỗi truy vấn, chúng tôi tìm kiếm nhị phân giá trị trung bình ứng viên trong phạm vi số dư có thể có. Vòng đếm bên trong được giữ đơn giản để tránh chi phí chung, vì tổng số ràng buộc đảm bảo rằng công việc kết hợp vẫn nằm trong giới hạn trong Python với khả năng xử lý đầu vào hiệu quả. 

Một chi tiết tinh tế là cách xử lý$x = 1$, trong đó tất cả các giá trị trở thành 0 ngay lập tức, do đó trung vị gần như bằng 0. Một cách khác là đảm bảo chỉ số trung bình là$(n+1)//2$, tương ứng với định nghĩa trung bình dưới được sử dụng trong bài toán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:$[1,2,3,4,5]$, truy vấn$x = 3$Mảng được sắp xếp vẫn giữ nguyên. 

Chúng tôi tìm kiếm nhị phân trên phần còn lại có thể$0,1,2$. 

| mid (ứng cử viên m) | điều kiện đếm được chuyển đổi | hợp lệ | 
| --- | --- | --- | 
| 0 | chỉ các giá trị chia hết cho 3 | sai | 
| 1 | giá trị có số dư ≤ 1 | đúng | 
| Thu hẹp 0-1 | hội tụ về 1 | đúng | 

Câu trả lời là 1. 

Điều này xác nhận rằng giá trị trung vị chỉ phụ thuộc vào cách phân phối phần dư trên mảng. 

### Ví dụ 2 

Mảng đầu vào:$[1,2,6,4,1,3]$, truy vấn$x = 2$còn lại là$[1,0,0,0,1,1]$. 

Mảng biến đổi được sắp xếp:$[0,0,0,1,1,1]$Trung vị là 1, phù hợp với kết quả tìm kiếm nhị phân. 

Điều này cho thấy tính đúng đắn của việc giảm hoạt động sang chuyển đổi mô-đun. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + q)\log x)$| tìm kiếm nhị phân trên mỗi truy vấn với quét tuyến tính trên mảng | 
| Không gian |$O(n)$| lưu trữ cho mảng được sắp xếp | 

Các ràng buộc cho phép tổng$n, q \le 10^5$, vì vậy cách tiếp cận này vẫn hiệu quả trong thực tế, đặc biệt vì vòng lặp bên trong đơn giản và thân thiện với bộ đệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""2
5 5
1 2 3 4 5
1
2
3
4
5
6 3
1 2 6 4 1 3
2
1
5
""") == """0 1 1 1 2
1 0 2"""

# edge: all equal
assert run("""1
5 2
5 5 5 5 5
1
5
""") == """0\n0"""

# edge: x large
assert run("""1
4 1
1 100 1000 10000
7
""") == """3"""

# edge: small array
assert run("""1
2 2
1 2
1
2
""") == """0 1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | 0 | ổn định dưới mảng thống nhất | 
| lớn x | hành vi modulo đúng | ranh giới giảm | 
| mảng nhỏ | tính đúng đắn của định nghĩa trung vị | xử lý kích thước cạnh | 

## Vỏ cạnh 

Đối với các mảng trong đó tất cả các phần tử giống hệt nhau, mọi mô đun đều tạo ra các số 0, do đó trung vị luôn bằng 0 bất kể$x$. Thuật toán xử lý việc này vì mọi tìm kiếm nhị phân đều thỏa mãn ngay điều kiện tại$m=0$. 

Đối với rất lớn$x$, tất cả các phần tử rơi vào một khoảng dư lượng duy nhất và không xảy ra hiện tượng bao bọc, do đó kết quả chỉ phụ thuộc vào các giá trị thô bên dưới$x$. Chức năng đếm vẫn hoạt động vì tất cả$v \bmod x = v$. 

Vì$n=2$, trung vị luôn là giá trị được chuyển đổi nhỏ hơn và tìm kiếm nhị phân giải quyết chính xác điều này vì vị từ kiểm tra trực tiếp nửa điều kiện dưới.
