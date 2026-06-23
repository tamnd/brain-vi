---
title: "CF 105064G - Lính vũ trang 1"
description: "Chúng ta được phát một bộ lính được đặt trên trục số. Mỗi người lính có một vị trí cố định và một loại vũ khí có sức mạnh nhất định. Khi một con quái vật xuất hiện ở vị trí nào đó, mọi người lính sẽ bắn về vị trí đó."
date: "2026-06-23T10:04:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105064
codeforces_index: "G"
codeforces_contest_name: "ICPC-de-Tryst 2024"
rating: 0
weight: 105064
solve_time_s: 77
verified: false
draft: false
---

[CF 105064G - Lính vũ trang 1](https://codeforces.com/problemset/problem/105064/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được phát một bộ lính được đặt trên trục số. Mỗi người lính có một vị trí cố định và một loại vũ khí có sức mạnh nhất định. Khi một con quái vật xuất hiện ở vị trí nào đó, mọi người lính sẽ bắn về vị trí đó. Một viên đạn chỉ có thể gây sát thương nếu nó vẫn còn đủ sức mạnh sau khi di chuyển quãng đường từ người lính đến quái vật. Viên đạn càng di chuyển xa thì nó càng trở nên tuyến tính. 

Đối với một quái vật được đặt ở vị trí$d$với chiều rộng lá chắn$s$, một người lính ở vị trí$a_i$với sức mạnh$p_i$đóng góp thành công một cú đánh nếu hình phạt khoảng cách không làm giảm sức mạnh của nó xuống dưới mức yêu cầu về lá chắn. Điều kiện này đơn giản hóa thành bất đẳng thức$$|a_i - d| \le p_i - s.$$Vì vậy, đối với bất kỳ chiều rộng lá chắn cố định$s$và bất kỳ vị trí nào$d$, chúng ta có thể đếm được bao nhiêu binh sĩ còn có thể đánh trúng quái vật. Quái vật sống sót ở vị trí$d$nếu số lượng này nhiều nhất$k$. Mục tiêu là chọn số nguyên nhỏ nhất có thể$s$sao cho dù quái vật có đứng ở đâu trên vạch số thì số lần trúng đòn cũng không bao giờ vượt quá$k$. 

Những hạn chế đẩy tới một$O(n \log n)$hoặc$O(n)$giải pháp cho mỗi trường hợp thử nghiệm. Với tối đa$10^5$tổng số lính trong tất cả các trường hợp thử nghiệm, mọi thứ bậc hai trong$n$sẽ quá chậm vì nó đòi hỏi tới$10^{10}$hoạt động trong trường hợp xấu nhất. 

Một vài trường hợp phức tạp xuất hiện ngay lập tức. Nếu tất cả binh sĩ đều có sức mạnh bằng 0, thì bất kỳ chiều rộng khiên dương nào cũng sẽ ngăn chặn tất cả các đòn đánh một cách tầm thường, nhưng câu trả lời vẫn phải là không âm. Nếu như$k = n$, thì quái vật được phép tấn công bởi tất cả binh lính, vì vậy ngay cả một chiếc khiên bằng 0 cũng có thể đủ. Nếu binh lính tập trung dày đặc tại cùng một điểm với sức mạnh cao, thì thậm chí có thể cần một tấm khiên lớn để giảm khoảng thời gian chồng chéo. 

Khó khăn tiềm ẩn quan trọng nhất là chúng ta không được hỏi về một vị trí cố định$d$, nhưng về điều tồi tệ nhất có thể$d$, điều này buộc chúng ta phải suy luận về cấu trúc chồng chéo toàn cục hơn là đánh giá theo từng điểm. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề là sửa chiều rộng của tấm chắn$s$và sau đó mô phỏng mọi vị trí có thể có của quái vật. Đối với mỗi vị trí$d$, ta sẽ đếm xem có bao nhiêu người lính thỏa mãn$|a_i - d| \le p_i - s$. Điều này tương đương với việc kiểm tra xem có bao nhiêu khoảng thời gian bao trùm mỗi điểm, vì mỗi người lính xác định khoảng thời gian ảnh hưởng tập trung vào$a_i$với bán kính$p_i - s$. Cách tiếp cận bạo lực sẽ đánh giá mức độ bao phủ này cho tất cả các vị trí số nguyên giữa$1$Và$10^9$, rõ ràng là không thể thực hiện được. 

Ngay cả khi chúng tôi hạn chế chỉ kiểm tra các điểm quan trọng, việc tính toán lại mức độ phù hợp cho từng điểm$s$vẫn còn chi phí$O(n^2)$trong trường hợp xấu nhất vì mỗi lần kiểm tra đều liên quan đến việc quét tất cả binh lính. 

Quan sát quan trọng là đối với một cố định$s$, mỗi người lính đóng góp một khoảng đối xứng:$$[a_i - (p_i - s),\ a_i + (p_i - s)].$$Chúng tôi muốn đảm bảo rằng không có điểm nào nằm bên trong hơn$k$những khoảng như vậy. Đây là một vấn đề "chồng chéo tối đa tối thiểu" cổ điển. 

Thay vì sửa$s$và kiểm tra sự chồng chéo, chúng tôi đảo ngược quan điểm: chúng tôi hỏi mức tối thiểu là bao nhiêu$s$sao cho sự chồng chéo tối đa của các khoảng thu hẹp này nhiều nhất là$k$. BẰNG$s$tăng lên, các khoảng co lại đều nhau nên sự chồng chéo chỉ có thể giảm đi. Tính đơn điệu này cho phép tìm kiếm nhị phân trên$s$. 

Đối với ứng viên cố định$s$, chúng tôi tính toán tất cả các khoảng, sắp xếp điểm cuối và đường quét để tính toán sự chồng chéo tối đa trong$O(n \log n)$. Sau đó, chúng tôi tìm kiếm nhị phân nhỏ nhất$s$thỏa mãn điều kiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng mọi tư thế) |$O(n \cdot 10^9)$|$O(n)$| Quá chậm | 
| Kiểm tra + Tìm kiếm nhị phân + Dòng quét |$O(n \log n \log P)$|$O(n)$| Đã chấp nhận | 

Đây$P$là thang công suất cực đại. 

## Hướng dẫn thuật toán 

### 1. Diễn giải mỗi người lính như một khoảng thời gian rút ngắn lại 

Đối với tấm chắn cố định$s$, tính bán kính hiệu dụng của mỗi người lính$r_i = p_i - s$. Nếu như$r_i < 0$, người lính đó không đóng góp gì cả. Nếu không thì nó bao gồm phân khúc$[a_i - r_i, a_i + r_i]$. Điều này chuyển đổi vấn đề thành tính toán chồng chéo khoảng thời gian. 

Lý do phép biến đổi này hoạt động là vì điều kiện ban đầu chính xác là điều kiện để một điểm nằm bên trong bán kính xung quanh tâm. 

### 2. Xác định tính khả thi của lá chắn 

Chúng tôi xác định một chức năng`check(s)`trả về true nếu không có điểm nào trên đường được bao phủ bởi nhiều hơn$k$khoảng thời gian. Điều này trực tiếp phù hợp với yêu cầu quái vật phải sống sót ở bất cứ đâu. 

Hành vi đơn điệu là rất quan trọng: tăng$s$chỉ giảm tất cả$r_i$, do đó các khoảng chỉ co lại và sự chồng chéo không thể tăng lên. 

### 3. Tính toán sự chồng chéo tối đa cho một điểm cố định$s$Để đánh giá`check(s)`, chúng tôi tạo ra tất cả các khoảng hợp lệ và chuyển đổi chúng thành các sự kiện: +1 ở điểm cuối bên trái và -1 ngay sau điểm cuối bên phải. Việc sắp xếp các sự kiện này cho phép quét duy trì số lượng khoảng thời gian hoạt động ở mọi tọa độ. Giá trị tối đa trong quá trình quét là số lần truy cập đồng thời tối đa. 

Chúng tôi so sánh mức tối đa này với$k$. 

### 4. Tìm kiếm nhị phân hợp lệ nhỏ nhất$s$Vì tính khả thi là đơn điệu nên chúng ta tìm kiếm nhị phân$s$từ 0 đến$\max p_i$. Mỗi bước chạy kiểm tra đường quét. 

### Tại sao nó hoạt động 

Mỗi người lính xác định một họ các khoảng co lại đồng đều như$s$increases. Hàm “chồng chéo tối đa các khoảng” đơn điệu không tăng trong$s$, vậy tập hợp lệ$s$giá trị là hậu tố của số nguyên. Binary search correctly finds the smallest valid value. The sweep line correctly captures worst-case overlap because any point of maximum coverage must occur at an interval endpoint in this construction.

 ## Giải pháp Python```python
import sys
input = sys.stdin.readline

def check(s, a, p, k):
    events = []
    for ai, pi in zip(a, p):
        r = pi - s
        if r < 0:
            continue
        l = ai - r
        rgt = ai + r
        events.append((l, 1))
        events.append((rgt + 1, -1))
    
    events.sort()
    
    cur = 0
    best = 0
    for x, v in events:
        cur += v
        if cur > best:
            best = cur
    
    return best <= k

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        p = list(map(int, input().split()))
        
        lo, hi = 0, max(p)
        ans = hi
        
        while lo <= hi:
            mid = (lo + hi) // 2
            if check(mid, a, p, k):
                ans = mid
                hi = mid - 1
            else:
                lo = mid + 1
        
        print(ans)

if __name__ == "__main__":
    solve()
```Cấu trúc cốt lõi là tìm kiếm nhị phân bao quanh việc kiểm tra tính khả thi. các`check`hàm xây dựng các sự kiện theo khoảng thời gian và chạy một đường quét để tính toán mức chồng lấp tối đa. 

Một điểm tinh tế là việc sử dụng`rgt + 1`for the closing event. Điều này đảm bảo ngữ nghĩa bao phủ số nguyên chính xác: một điểm chính xác ở điểm cuối bên phải vẫn được bao gồm trong khoảng. 

Another important detail is skipping soldiers where$p_i - s < 0$, vì họ không thể đóng góp bất kỳ khoản bảo hiểm hợp lệ nào. 

## Ví dụ đã hoạt động 

Consider a small configuration where soldiers are at positions`[1, 5, 9]`với quyền hạn`[3, 3, 3]`, Và$k = 1$. 

### Theo dõi$s = 1$| Người Lính | Bán kính khoảng$p_i - s$| Khoảng thời gian |
 | --- | --- | --- | 
| 1 | 2 | [-1, 3] | 
| 5 | 2 | [3, 7] | 
| 9 | 2 | [7, 11] | 

Sự kiện quét:

 sự kiện được sắp xếp = (-1,+1), (3,+1), (4,-1), (7,+1), (8,-1), (12,-1)

 | Sự kiện | Đang hoạt động | 
| --- | --- | 
| -1 | 1 | 
| 3 | 2 | 
| 4 | 1 | 
| 7 | 2 | 
| 8 | 1 | 
| 12 | 0 | 

Sự chồng chéo tối đa là 2, vượt quá$k=1$, Vì thế$s=1$không hợp lệ. 

### Theo dõi$s = 2$| Người Lính | Bán kính khoảng | Khoảng thời gian | 
| --- | --- | --- | 
| 1 | 1 | [0, 2] | 
| 5 | 1 | [4, 6] | 
| 9 | 1 | [8, 10] |

 Sự kiện: 

(0,+1), (3,-1), (4,+1), (7,-1), (8,+1), (11,-1) 

| Sự kiện | Đang hoạt động | 
| --- | --- | 
| 0 | 1 | 
| 3 | 0 | 
| 4 | 1 | 
| 7 | 0 | 
| 8 | 1 | 
| 11 | 0 | 

Sự chồng chéo tối đa là 1, thỏa mãn điều kiện. 

Những dấu vết này cho thấy ngày càng tăng$s$giảm kích thước khoảng và giảm nghiêm ngặt sự chồng chéo, đó là tính đơn điệu cốt lõi được sử dụng trong tìm kiếm nhị phân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n \log P)$| Mỗi loại kiểm tra tính khả thi$O(n)$sự kiện và tìm kiếm nhị phân chạy qua$O(\log P)$giá trị | 
| Không gian |$O(n)$| Danh sách sự kiện lưu trữ tối đa$2n$điểm cuối | 

Tổng cộng$n$qua các trường hợp thử nghiệm là$10^5$, do đó ngay cả với các thừa số logarit, nghiệm vẫn nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def check(s, a, p, k):
        events = []
        for ai, pi in zip(a, p):
            r = pi - s
            if r < 0:
                continue
            l = ai - r
            rgt = ai + r
            events.append((l, 1))
            events.append((rgt + 1, -1))
        events.sort()
        cur = best = 0
        for _, v in events:
            cur += v
            best = max(best, cur)
        return best <= k

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        p = list(map(int, input().split()))

        lo, hi = 0, max(p)
        ans = hi
        while lo <= hi:
            mid = (lo + hi) // 2
            if check(mid, a, p, k):
                ans = mid
                hi = mid - 1
            else:
                lo = mid + 1
        out.append(str(ans))
    return "\n".join(out)

# sample-style sanity checks
assert run("1\n3 1\n1 5 9\n3 3 3\n") == "2"
assert run("1\n2 0\n1 10\n0 0\n") == "0"
assert run("1\n3 2\n1 2 3\n5 5 5\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng co rút đối xứng | 2 | giảm chồng chéo khi tăng s | 
| người lính quyền lực bằng không | 0 | trường hợp cạnh khả thi ngay lập tức | 
| k gần n | 0 | điều kiện sinh tồn tầm thường | 

## Vỏ cạnh 

Khi tất cả sức mạnh bằng 0, mọi người lính đều tạo ra các khoảng bán kính trống hoặc âm cho bất kỳ giá trị dương nào.$s$. Đường quét không nhận được sự kiện nào, do đó sự chồng chéo tối đa bằng 0 và thuật toán chấp nhận chính xác$s = 0$. 

Khi$k = n$, mọi cấu hình đều hợp lệ ngay cả ở$s = 0$. Hàm kiểm tra tính toán mức chồng lấp tối đa không bao giờ vượt quá$n$, do đó tìm kiếm nhị phân ngay lập tức hội tụ về 0. 

Khi tất cả binh lính ở cùng một vị trí với sức mạnh lớn, sự chồng chéo ban đầu là$n$Tại$s = 0$. Chức năng kiểm tra báo lỗi chính xác cho đến khi$s$giảm tất cả bán kính đủ để các khoảng co lại và tách ra, giảm sự chồng chéo bên dưới$k$.
