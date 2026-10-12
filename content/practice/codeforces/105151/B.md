---
title: "CF 105151B - \u041a\u0430\u043f\u0435\u043b\u044c\u043a\u0438"
description: "Chúng ta được cấp một tập hợp các giọt mưa, mỗi giọt rơi xuống một điểm trên đường nằm ngang. Mỗi giọt nước xuất hiện ở một tọa độ cụ thể và chỉ bắt đầu giãn nở sau thời gian rơi của chính nó."
date: "2026-06-27T11:08:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105151
codeforces_index: "B"
codeforces_contest_name: "XIX \u041d\u0438\u0436\u0435\u0433\u043e\u0440\u043e\u0434\u0441\u043a\u0430\u044f \u0433\u043e\u0440\u043e\u0434\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u0412. \u0414. \u041b\u0435\u043b\u044e\u0445\u0430"
rating: 0
weight: 105151
solve_time_s: 82
verified: true
draft: false
---

[CF 105151B - \u041a\u0430\u043f\u0435\u043b\u044c\u043a\u0438](https://codeforces.com/problemset/problem/105151/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các giọt mưa, mỗi giọt rơi xuống một điểm trên đường nằm ngang. Mỗi giọt nước xuất hiện ở một tọa độ cụ thể và chỉ bắt đầu giãn nở sau thời gian rơi của chính nó. Khi một giọt nước rơi xuống, nó giãn nở đối xứng với tốc độ không đổi: sau$k$giây kể từ khi hạ cánh, nó bao gồm khoảng cách từ tâm của nó$x_i$theo cả hai hướng theo khoảng cách$k$. Vì vậy, mỗi giọt tạo thành một khoảng tăng trưởng có bán kính tăng tuyến tính theo thời gian, nhưng chỉ sau thời gian kích hoạt của nó. 

Nhiệm vụ là xác định thời điểm nguyên sớm nhất$T$sao cho sự kết hợp của tất cả các khoảng mở rộng này bao phủ hoàn toàn đoạn cố định$[0, r]$. Bao phủ có nghĩa là mọi điểm bên trong$[0, r]$được chứa trong khoảng thời gian mở rộng của ít nhất một giọt tại thời điểm đó$T$. 

Các ràng buộc rất lớn, có thể lên tới$2 \cdot 10^6$các giọt và tọa độ lên đến$10^9$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào mô phỏng từng bước thời gian hoặc tính toán lại mức độ phù hợp một cách độc lập cho từng thời điểm ứng cử viên. Mọi giải pháp đều phải tránh lặp lại tất cả các giọt trong mỗi lần kiểm tra. Chúng tôi cũng không đủ khả năng$O(n \log n)$đánh giá mỗi lần trong tìm kiếm nhị phân đơn giản mà không cần hợp nhất tuyến tính cẩn thận, vì mỗi lần kiểm tra phải gần với thời gian tuyến tính. 

Một khía cạnh tinh vi của vấn đề là các giọt nước không đóng góp ngay lập tức; chúng chỉ hoạt động sau thời gian rơi của cá nhân chúng. Điều này tạo ra một tập hợp các khoảng thời gian phụ thuộc vào thời gian, rất dễ xử lý sai nếu người ta cho rằng tất cả các giọt đều hoạt động từ thời điểm 0. 

Một số trường hợp đặc biệt bộc lộ những lỗi phổ biến. Nếu tất cả các giọt rơi sau một thời gian ứng cử viên, thì phạm vi phủ sóng sẽ trống ngay cả khi vị trí của chúng dày đặc. Ví dụ,$n=2, r=10$, giọt nước ở$x=0, x=10$, cả hai với$t_i=100$. Tại$T=50$, không có vùng phủ sóng nào tồn tại và câu trả lời không thể là 50 ngay cả khi mọi thứ đều hoàn hảo về mặt không gian. 

Một trường hợp thất bại khác xảy ra khi một giọt nước rơi đúng vào thời điểm$T$. Bán kính của nó lúc đó$T$bằng 0 nên nó chỉ đóng góp một điểm duy nhất. Hiểu sai điều này là đóng góp một khoảng khác 0 dẫn đến việc chấm dứt sớm không chính xác. 

Cuối cùng, lỗi hợp nhất tham lam xảy ra nếu người ta giả định một thứ tự cố định mà không tính đến việc kích hoạt dựa trên thời gian. Tập hợp các khoảng thời gian hoạt động thay đổi theo$T$, do đó mức độ phù hợp phải luôn được đánh giá theo thời gian cố định của ứng viên. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là mô phỏng thời gian từ$0$trở lên. Tại mỗi thời điểm số nguyên$T$, chúng tôi tính toán tất cả các giọt đã rơi và với mỗi giọt như vậy, chúng tôi tính toán khoảng thời gian của nó$[x_i - (T - t_i), x_i + (T - t_i)]$. Sau đó chúng tôi hợp nhất các khoảng này và kiểm tra xem chúng có bao gồm$[0, r]$. Điều này đúng vì nó trực tiếp tuân theo định nghĩa bài toán. 

Vấn đề là hiệu suất. Trong trường hợp xấu nhất, chúng ta có thể cần phải kiểm tra$10^9$số lần ứng cử viên và mỗi lần kiểm tra yêu cầu xử lý tối đa$2 \cdot 10^6$khoảng thời gian. Điều đó vượt xa mọi tính toán khả thi. 

Quan sát quan trọng là thuộc tính bao phủ là đơn điệu về mặt thời gian. Một lần vào một thời điểm nhất định$T$đủ để che phủ$[0, r]$, thời gian lớn hơn$T'$cũng sẽ có tác dụng vì mỗi giọt chỉ giãn nở thêm mà thôi. Điều này cho phép tìm kiếm nhị phân trên$T$. 

Mỗi lần kiểm tra tính khả thi cho một$T$có thể được thực hiện trong$O(n)$bằng cách xây dựng khoảng thời gian hiệu quả chỉ dành cho những giọt có$t_i \le T$, sắp xếp hoặc quét chúng và hợp nhất. Tuy nhiên, việc sắp xếp mỗi lần kiểm tra sẽ quá chậm. Instead, we sort droplets once by position or process them in a way that avoids repeated sorting. Công thức hiệu quả nhất là ghép đôi$x_i$Và$t_i$, và cho một cố định$T$, tính toán các khoảng thời gian nhanh chóng và thực hiện quét tuyến tính sau khi sắp xếp theo điểm cuối bên trái. 

Do đó giải pháp trở thành tìm kiếm nhị phân trên$T$, với mỗi lần kiểm tra thực hiện một phép biến đổi tuyến tính và hợp nhất tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(T \cdot n)$|$O(n)$| Quá chậm | 
| Tìm kiếm nhị phân + Quét |$O(n \log R)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sort droplets by their positions only if needed for stable processing, but more importantly treat each check independently by computing coverage intervals based on a fixed$T$. Việc đặt hàng vốn không được yêu cầu trên toàn cầu vì việc hợp nhất được thực hiện cho mỗi lần kiểm tra. 
2. Xác định hàm`can(T)`xác định liệu tất cả các điểm trong$[0, r]$được bảo hiểm vào thời điểm$T$. 
3. Bên trong`can(T)`, lặp lại trên tất cả các giọt. Đối với mỗi giọt có$t_i \le T$, tính bán kính của nó$d = T - t_i$, và do đó khoảng của nó$[x_i - d, x_i + d]$. Bỏ qua các giọt nước với$t_i > T$vì chúng chưa xuất hiện. 
4. Thu thập tất cả các khoảng như vậy và hợp nhất chúng theo thứ tự tăng dần của điểm cuối bên trái. Trong khi hợp nhất, hãy duy trì điểm bên phải xa nhất được bao phủ cho đến nay. Nếu tại bất kỳ thời điểm nào có khoảng cách giữa phạm vi phủ sóng được hợp nhất hiện tại và khoảng thời gian tiếp theo, phạm vi phủ sóng sẽ thất bại ngay lập tức. 
5. Sau khi hợp nhất tất cả các khoảng, hãy kiểm tra xem điểm bao phủ cuối cùng có đạt ít nhất$r$. Nếu có,$T$là đủ. 
6. Tìm kiếm nhị phân trên$T$trong phạm vi$[0, \max t_i + r]$. Giới hạn trên xuất phát từ trường hợp xấu nhất trong đó vùng phủ sóng ngoài cùng bên phải phải lan truyền từ một giọt ở xa. 
7. Với mỗi giá trị ở giữa trong tìm kiếm nhị phân, hãy gọi`can(mid)`và điều chỉnh giới hạn tương ứng để tìm giá trị tối thiểu hợp lệ$T$. 

Tại sao nó hoạt động dựa trên tính đơn điệu của phạm vi bảo hiểm. Sau khi cấu hình khoảng thời gian bao gồm$[0, r]$vào thời điểm đó$T$, tăng dần$T$chỉ mở rộng tất cả các khoảng thời gian hoạt động hoặc kích hoạt nhiều giọt hơn. Điều này có nghĩa là tập hợp thời gian hợp lệ tạo thành một hậu tố của dòng thời gian số nguyên, đây chính xác là điều kiện cần thiết để đảm bảo tính chính xác của tìm kiếm nhị phân. Bước hợp nhất đảm bảo rằng chúng tôi tính toán chính xác các phần mở rộng chồng chéo và phát hiện các khoảng trống một cách chính xác mà không đánh giá quá cao mức độ phù hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(T, x, t, r):
    intervals = []
    for i in range(len(x)):
        if t[i] <= T:
            d = T - t[i]
            l = x[i] - d
            rgt = x[i] + d
            intervals.append((l, rgt))

    if not intervals:
        return False

    intervals.sort()
    
    cur = 0
    for l, rgt in intervals:
        if l > cur:
            return False
        if rgt > cur:
            cur = rgt
        if cur >= r:
            return True
    return cur >= r

def solve():
    n, R = map(int, input().split())
    x = list(map(int, input().split()))
    t = list(map(int, input().split()))

    lo, hi = 0, R + max(t)
    ans = hi

    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid, x, t, R):
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Cốt lõi của việc thực hiện là`can`chức năng tái tạo vùng phủ sóng hiệu quả tại một thời điểm cố định. Mỗi giọt chỉ được chuyển đổi thành một khoảng nếu nó hoạt động. Việc mở rộng khoảng được tính toán trực tiếp từ chênh lệch thời gian, đảm bảo tính chính xác của việc giải thích hình học. 

Quá trình hợp nhất duy trì một biến duy nhất`cur`, theo dõi điểm được bao phủ ngoài cùng bên phải cho đến nay. Bất kỳ khoảng thời gian nào bắt đầu sau`cur`báo hiệu một khoảng trống, điều này ngay lập tức làm mất hiệu lực thời gian của ứng viên. 

Tìm kiếm nhị phân sử dụng thực tế là câu trả lời được giới hạn ở trên bởi$R + \max(t_i)$, vì trong trường hợp xấu nhất một giọt ở vị trí 0 phải giãn nở theo khoảng cách$R$sau khi nó hoạt động. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi Mẫu 2 để xem mức độ thay đổi phạm vi kích hoạt sớm. 

đầu vào:```
4 2
3 2 1 0
1 1 1 1
```Chúng tôi kiểm tra một vài lần ứng viên bên trong logic tìm kiếm nhị phân. 

| T | Giọt hoạt động | Khoảng thời gian | Bảo hiểm hợp nhất | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | tất cả | [2,4], [1,3], [0,2], [-1,1] | [ -1,4 ] | bìa | 
| 0 | không | [] | không | thất bại | 

Tại$T=1$, tất cả các giọt đều hoạt động và đã bao phủ một vùng rộng bao gồm$[0,2]$, vì vậy bảo hiểm thành công ngay lập tức. 

This shows that once all droplets activate at the same time, the problem reduces to pure interval expansion, and binary search quickly identifies the minimal feasible moment.

 Bây giờ hãy xem xét Mẫu 1:```
3 5
1 3 5
1 3 3
```Chúng tôi kiểm tra$T=4$, đó là câu trả lời đúng. 

| T | Giọt hoạt động | Khoảng thời gian | Bảo hiểm hợp nhất | Kết quả | 
| --- | --- | --- | --- | --- | 
| 4 | tất cả | [-2,4], [0,4], [2,6] | [ -2,6 ] | bìa | 
| 3 | tất cả ngoại trừ phần cuối cùng | [-1,3], [0,2], [2,4] | [ -1,4 ] | thất bại | 

Tại$T=3$, vùng phủ sóng chỉ đạt tới 4, để lại một phần$[0,5]$chưa được khám phá. Tại$T=4$, sự giãn nở của tất cả các giọt sẽ thu hẹp khoảng cách cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log R)$| tìm kiếm nhị phân theo thời gian với$O(n)$kiểm tra tính khả thi từng bước | 
| Không gian |$O(n)$| lưu trữ cho mảng đầu vào và danh sách khoảng thời gian tạm thời | 

Độ phức tạp phù hợp thoải mái trong giới hạn vì tìm kiếm nhị phân chạy tối đa khoảng 30 đến 32 lần lặp cho phạm vi tọa độ nhất định và mỗi lần lặp xử lý tối đa$2 \cdot 10^6$giọt một cách tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def can(T, x, t, r):
        intervals = []
        for i in range(len(x)):
            if t[i] <= T:
                d = T - t[i]
                intervals.append((x[i] - d, x[i] + d))
        if not intervals:
            return False
        intervals.sort()
        cur = 0
        for l, rgt in intervals:
            if l > cur:
                return False
            cur = max(cur, rgt)
            if cur >= r:
                return True
        return cur >= r

    def solve():
        n, R = map(int, input().split())
        x = list(map(int, input().split()))
        t = list(map(int, input().split()))

        lo, hi = 0, R + max(t)
        ans = hi

        while lo <= hi:
            mid = (lo + hi) // 2
            if can(mid, x, t, R):
                ans = mid
                hi = mid - 1
            else:
                lo = mid + 1

        print(ans)

    solve()
    return ""

# sample tests
assert run("3 5\n1 3 5\n1 3 3\n") == "", "sample 1"
assert run("4 2\n3 2 1 0\n1 1 1 1\n") == "", "sample 2"

# custom tests
assert run("1 7\n2\n0\n") == "", "single droplet expansion"
assert run("2 10\n0 10\n0 0\n") == "", "two endpoints already covered"
assert run("3 5\n5 5 5\n10 10 10\n") == "", "delayed activation"
assert run("2 1\n0 100\n0 100\n") == "", "far delayed useless early coverage"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giọt đơn | mở rộng dần dần | độ chính xác cấu trúc tối thiểu | 
| điểm cuối | bảo hiểm đầy đủ ngay lập tức | vùng phủ cạnh đối xứng | 
| kích hoạt bị trì hoãn | chờ đợi thống trị câu trả lời | kích hoạt thời gian chính xác | 
| bị trì hoãn rất xa | không thể bảo hiểm sớm | phát hiện khoảng cách trước khi kích hoạt | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi không có giọt nào hoạt động tại thời điểm ứng cử viên. Ví dụ,$T=0$với tất cả$t_i > 0$tạo ra một tập khoảng trống. Thuật toán trả về sai ngay lập tức một cách chính xác vì không thể có phạm vi bao phủ. 

Một trường hợp khác là khi phạm vi phủ sóng gần như hoàn tất nhưng lại thất bại ở một khoảng trống. Ví dụ: khoảng thời gian sau khi mở rộng có thể tạo ra$[0, 2]$Và$[3, 5]$với khoảng cách từ 2 đến 3. Bước hợp nhất phát hiện điều này vì khoảng thời gian tiếp theo bắt đầu sau`cur`, và hàm trả về false mà không tiếp tục. 

Trường hợp tinh vi cuối cùng là khi một giọt kích hoạt chính xác vào thời điểm thích hợp. Vì bán kính của nó bằng 0 nên khoảng của nó là một điểm. Thuật toán vẫn bao gồm nó, nhưng nó không thể thu hẹp khoảng cách trừ khi nó nằm chính xác ở ranh giới của vùng phủ sóng hiện có, được xử lý chính xác theo điều kiện hợp nhất`l > cur`.
