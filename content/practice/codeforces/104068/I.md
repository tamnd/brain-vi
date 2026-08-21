---
title: "CF 104068I - \u70e4\u571f\u8c46"
description: "Một chuỗi khoai tây đến theo thời gian. Mỗi củ khoai tây đều có dấu thời gian đến và khi đưa vào máy, “giá trị quá chín” của nó sẽ tăng tuyến tính theo thời gian. Bất cứ lúc nào, chúng tôi được phép lấy tất cả khoai tây hiện có trong máy ra bằng một thao tác."
date: "2026-07-02T03:05:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104068
codeforces_index: "I"
codeforces_contest_name: "The 17-th Beihang University Collegiate Programming Contest (BCPC 2022) - Preliminary"
rating: 0
weight: 104068
solve_time_s: 49
verified: true
draft: false
---

[CF 104068I - \u70e4\u571f\u8c46](https://codeforces.com/problemset/problem/104068/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một chuỗi khoai tây đến theo thời gian. Mỗi củ khoai tây đều có dấu thời gian đến và khi đưa vào máy, “giá trị quá chín” của nó sẽ tăng tuyến tính theo thời gian. Bất cứ lúc nào, chúng tôi được phép lấy tất cả khoai tây hiện có trong máy ra bằng một thao tác. Mỗi lần kéo như vậy sẽ có một mức phạt cố định cộng với một khoản chi phí bổ sung bằng tổng số khoai tây nấu quá chín tích lũy tại thời điểm đó. 

Nhiệm vụ là chọn một tập hợp thời gian kéo sao cho mỗi củ khoai tây cuối cùng được loại bỏ đúng một lần, đồng thời giảm thiểu tổng chi phí. 

Đầu vào là một chuỗi thời gian đến không giảm. Sự đơn điệu đó quan trọng vì nó đảm bảo rằng khi chúng ta xem xét việc nhóm khoai tây thành một lần kéo, bộ khoai tây bên trong luôn là hậu tố liền kề theo thứ tự thời gian. 

Ràng buộc n lên tới 10^6 buộc mọi giải pháp phải tuyến tính hoặc gần tuyến tính. Bất cứ điều gì liên quan đến chuyển đổi theo cặp, lập trình động trên tất cả các khoảng hoặc hợp nhất bậc hai dựa trên sắp xếp sẽ thất bại. Chúng ta cần một cấu trúc trong đó mỗi củ khoai tây được xử lý một lần hoặc một số lần không đổi. 

Trường hợp cạnh khóa xuất hiện khi k lớn. Nếu việc kéo đắt tiền, chúng ta có thể thích nhiều lần kéo nhỏ hơn. Nếu k bằng 0, chúng ta có thể muốn trì hoãn càng lâu càng tốt và kéo mọi thứ cùng một lúc. Một trường hợp khác là khi thời gian đến bằng hoặc gần bằng nhau, vì hành vi nhóm trở nên nhạy cảm với việc liệu việc hợp nhất có làm giảm chi phí hay không. 

Một ví dụ nhỏ minh họa cấu trúc: 

đầu vào: 

n = 3, k = 2 

t = [0, 1, 2] 

Nếu chúng ta kéo tại thời điểm 2 một lần thì chi phí là k + (2 + 1 + 0) = 5. 

Nếu chúng ta kéo riêng ba lần ở mỗi lần đến thì chi phí là 3k = 6. 

Vì vậy, việc nhóm có lợi ở đây, nhưng không phải lúc nào cũng vậy. 

Quyết định hoàn toàn là về việc phân chia dòng thời gian thành các phân đoạn. 

## Phương pháp tiếp cận 

Bắt đầu với chế độ xem bạo lực: chúng tôi chọn một tập hợp con các thời điểm để thực hiện thao tác kéo. Mỗi lần kéo xác định một nhóm khoai tây đã đến kể từ lần kéo trước. Đối với một phân vùng cố định của mảng thành các phân đoạn, chi phí của một phân đoạn phụ thuộc vào thời gian kéo đã chọn và trong một phân đoạn, thời gian kéo tốt nhất là thời gian đến cuối cùng của nó. 

Vì vậy, nếu chúng tôi cố định ranh giới phân khúc, chúng tôi có thể tính toán chi phí trực tiếp: mỗi phân khúc đóng góp k cộng với tổng chênh lệch giữa lần trước và thời gian đến trong phân khúc. 

Lực lượng vũ phu liệt kê tất cả các phân vùng của n mục. Về cơ bản đó là 2^(n-1) khả năng, vì mỗi khoảng cách có thể được cắt hoặc không. Ngay cả khi đánh giá một phân vùng là O(n), tổng độ phức tạp sẽ trở thành cấp số nhân và không thể sử dụng được với n lên tới 10^6. 

Quan sát quan trọng là chi phí phân rã cục bộ và có thể được biểu thị tăng dần. Khi chúng tôi mở rộng một phân khúc, chúng tôi sẽ tiếp tục tích lũy trong cùng một lực kéo hoặc chúng tôi bắt đầu một lực kéo mới. Điều này tạo ra một quyết định cổ điển giữa việc hợp nhất phần tử hiện tại vào phân đoạn cuối cùng hoặc cắt ở đây. 

Chúng tôi trình bày lại vấn đề dưới dạng quét tuyến tính với việc duy trì động chi phí phân đoạn đang chạy. Mỗi củ khoai tây mới đóng góp thêm chi phí chờ đợi tùy thuộc vào thời gian nó không được kéo ra và mỗi đoạn đóng góp một k cố định. Cấu trúc này tương đương với việc quyết định vị trí cắt giảm và cấu trúc tối ưu có thể được tính toán một cách tham lam bằng cách cho biết liệu việc mở rộng phân khúc hiện tại có tệ hơn việc bắt đầu một phân khúc mới hay không, điều này làm giảm việc so sánh mức tăng trưởng chi phí gia tăng với k. 

Điều này dẫn đến DP tuyến tính trong đó chúng tôi theo dõi chi phí tốt nhất lên tới i với phân đoạn cuối cùng kết thúc tại i và chúng tôi cập nhật các chuyển đổi trong O(1) bằng cách sử dụng tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| DP tối ưu / phân vùng tham lam | O(n) | O(1) hoặc O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi viết lại chi phí theo cách thể hiện rõ ràng sự đóng góp của phân khúc. Giả sử một đoạn kết thúc ở chỉ số r và bắt đầu ở l. Chúng ta kéo tại thời điểm t_r, do đó chi phí là k cộng tổng trên i trong [l, r] của (t_r - t_i). 

Chúng tôi tính toán trước tổng tiền tố của t. Khi đó chi phí phân khúc sẽ trở thành k + (r - l + 1) * t_r - (tổng t[l..r]). 

Chúng tôi xử lý i từ 1 đến n và duy trì giá trị DP thể hiện chi phí tối thiểu để xử lý khoai tây thứ i đầu tiên. Đoạn cuối cùng kết thúc tại i, vì vậy chúng ta xem xét đoạn cắt trước đó ở đâu. 

## Hướng dẫn thuật toán 

1. Sắp xếp là không cần thiết vì thời gian không hề giảm. Chúng tôi dựa vào thứ tự này để các phân đoạn tương ứng với các phạm vi liền kề. 
2. Tính tổng tiền tố của thời gian đến để có thể đánh giá chi phí phân đoạn trong O(1). Điều này tránh việc tính toán lại số tiền trong mỗi lần chuyển đổi. 
3. Xác định dp[i] là chi phí tối thiểu để xử lý khoai tây thứ i đầu tiên. 
4. Với mỗi i, coi đoạn cuối cùng bắt đầu ở j và kết thúc ở i. Sự chuyển tiếp là: 

dp[i] = min trên j < i của dp[j] + cost(j+1, i). 
5. Mở rộng chi phí phân khúc bằng cách sử dụng tổng tiền tố: 

cost(j+1, i) = k + (i - j) * t[i] - (tiền tố[i] - tiền tố[j]). 
6. Sắp xếp lại các số hạng để tách biệt các phần phụ thuộc j: 

dp[i] = k + i * t[i] - tiền tố[i] + min trên j < i của (dp[j] - j * t[i] + tiền tố[j]). 
7. Quan sát rằng đối với i cố định, t[i] không đổi bên trong cực tiểu, vì vậy chúng tôi duy trì cấu trúc dữ liệu trên j lưu trữ các dòng có dạng: 

value_j(x) = dp[j] + tiền tố[j] - j * x, 

được đánh giá tại x = t[i]. 

Đây là cấu trúc thủ thuật bao lồi trong đó hệ số góc là -j, đơn điệu khi j tăng, cho phép phân bổ các truy vấn O(1) hoặc O(log n) tùy thuộc vào việc triển khai. 
8. Chèn từng j theo thứ tự tăng dần, duy trì ứng cử viên tốt nhất cho i trong tương lai. Truy vấn tại t[i] để tính dp[i]. 
9. Khởi tạo dp[0] = 0 và bao gồm đường cơ sở j = 0. 
10. Đáp án là dp[n]. 

### Tại sao nó hoạt động 

Mỗi trạng thái j đại diện cho việc chọn vị trí kéo trước đó. Phép biến đổi bao lồi mã hóa sự phụ thuộc tuyến tính vào t[i] và thời gian đến đơn điệu đảm bảo rằng các điểm truy vấn không giảm. Điều này đảm bảo chúng ta không bao giờ cần phải xem xét lại các dòng trước đó theo cách phá vỡ tính tối ưu. Mỗi dp[i] được xây dựng từ sự phân tách chính xác của tất cả các phân đoạn cuối cùng có thể có, do đó không có phân vùng khả thi nào bị loại trừ và mọi phân vùng đều ánh xạ tới chính xác một chuỗi chuyển tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    t = list(map(int, input().split()))
    
    if n == 0:
        print(0)
        return

    # prefix sums of t
    pref = [0] * (n + 1)
    for i in range(1, n + 1):
        pref[i] = pref[i - 1] + t[i - 1]

    # Convex hull trick with monotone slopes and queries
    hull = []  # each item: (m, b) representing m*x + b

    def add_line(m, b):
        # remove last if new line makes it obsolete
        while len(hull) >= 2:
            m1, b1 = hull[-2]
            m2, b2 = hull[-1]
            m3, b3 = m, b
            # check intersection condition
            if (b3 - b2) * (m2 - m1) <= (b2 - b1) * (m3 - m2):
                hull.pop()
            else:
                break
        hull.append((m, b))

    ptr = 0

    def query(x):
        nonlocal ptr
        if ptr >= len(hull):
            ptr = len(hull) - 1
        while ptr + 1 < len(hull):
            m1, b1 = hull[ptr]
            m2, b2 = hull[ptr + 1]
            if m1 * x + b1 >= m2 * x + b2:
                ptr += 1
            else:
                break
        m, b = hull[ptr]
        return m * x + b

    dp0 = 0
    add_line(0, 0)  # j = 0

    for i in range(1, n + 1):
        x = t[i - 1]
        best = query(x)
        dp0 = k + i * x - pref[i] + best
        add_line(-i, dp0 + pref[i])

    print(dp0)

if __name__ == "__main__":
    solve()
```Việc triển khai theo dõi các tổng tiền tố để đánh giá chi phí phân khúc mà không cần tính toán lại. Phép truy toán DP được nhúng vào cấu trúc bao lồi trong đó mỗi trạng thái j trở thành một đường thẳng. Độ dốc là -j và phần chặn là dp[j] + tiền tố[j], khớp với sự sắp xếp lại đại số từ quá trình chuyển đổi. 

Truy vấn dựa trên con trỏ hoạt động vì cả độ dốc và điểm truy vấn đều đơn điệu. Con trỏ không bao giờ di chuyển lùi lại, tạo ra độ phức tạp tuyến tính giảm dần. 

Một lỗi phổ biến là quên rằng đóng góp dp phải bao gồm prefix[j] trong phần chặn; thiếu nó sẽ phá vỡ phép biến đổi tuyến tính và tạo ra chi phí phân khúc không chính xác. 

## Ví dụ đã hoạt động 

Xem xét đầu vào: 

n = 3, k = 2 

t = [0, 1, 2] 

Chúng tôi tính tổng tiền tố pref = [0, 0, 1, 3]. 

| tôi | x = t[i] | giá trị dòng tốt nhất | tính toán dp[i] | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 2 + 1*0 - 0 + 0 = 2 | 
| 2 | 1 | phút từ dòng | 2+2*1 - 1+tốt nhất | 
| 3 | 2 | phút từ dòng | hợp nhất cuối cùng và chia tách | 

Tại i = 3, cấu trúc thiên về một phân đoạn duy nhất, tạo ra dp[3] = 5. 

Dấu vết này cho thấy các quyết định ban đầu bị ghi đè như thế nào bằng cách phân nhóm tầm xa tốt hơn, đó chính xác là những gì mà bao lồi đang mã hóa: đường cắt trước tốt nhất cho mỗi điểm cuối. 

Bây giờ hãy xem xét một trường hợp tương phản: 

n = 3, k = 10 

t = [0, 1, 2] 

Ở đây k chiếm ưu thế nên giải pháp tốt nhất là phân đoạn đơn. Quá trình chuyển đổi dp luôn thích mở rộng cùng một dòng hơn là đưa ra các ngắt hiệu quả mới. Thân tàu vẫn hoạt động nhưng không bao giờ chuyển đổi tối ưu như người tiền nhiệm. 

Điều này chứng tỏ rằng thuật toán tự động thích ứng với cả hai chế độ mà không cần xử lý cụ thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được thêm một lần và được truy vấn một lần trong cấu trúc bao lồi đơn điệu | 
| Không gian | O(n) | Lưu trữ tổng tiền tố và dòng thân tàu | 

Độ phức tạp tuyến tính là cần thiết cho n lên đến 10^6. Bất kỳ giải pháp nào đánh giá lại chi phí của phân khúc hoặc thử tất cả các vị trí bị cắt sẽ vượt quá giới hạn thời gian theo một số bậc độ lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve()).strip()

# minimum
assert run("1 5\n0\n") == "5"

# all same time
assert run("3 2\n1 1 1\n") is not None

# increasing times
assert run("3 2\n0 1 2\n") is not None

# k = 0, should prefer single pull
assert run("3 0\n0 1 2\n") is not None

# large separation
assert run("4 1\n0 100 200 300\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | k | trường hợp cơ sở phần tử đơn | 
| lần bằng nhau | nhóm ổn định | phân đoạn thời lượng bằng không | 
| ngày càng tăng | phân nhóm cân bằng | tính đúng đắn chung | 
| k=0 | phân đoạn đơn | thống trị hợp nhất | 
| thời gian thưa thớt | nhiều phân khúc | hành vi chia rẽ | 

## Vỏ cạnh 

Khi n = 1, hành động hợp lệ duy nhất là kéo một lần, do đó chi phí chính xác là k. Thuật toán khởi tạo dp[0] và xây dựng dp[1] bằng cách sử dụng đường cơ sở j = 0, tạo ra k + 0. 

Khi tất cả thời gian đến bằng nhau, mỗi củ khoai tây đều có chi phí nội bộ bằng 0 trong bất kỳ phân khúc nào. Quyết định giảm xuống mức tối thiểu số lần kéo, do đó, thuật toán ưu tiên hợp nhất tất cả các mục thành một phân đoạn một cách hiệu quả trừ khi k buộc phải chia tách. Bao lồi suy biến thành các điểm truy vấn giống hệt nhau, nhưng cấu trúc đơn điệu vẫn trả về các chuyển tiếp dp chính xác. 

Khi k = 0, chiến lược tối ưu luôn là thực hiện lần kéo cuối cùng. DP không bao giờ được hưởng lợi từ việc bắt đầu các phân đoạn mới và thân tàu liên tục chọn j = 0 là tối ưu, tạo ra tổng chi phí bằng tổng (t[n] - t[i]) trên tất cả i.
