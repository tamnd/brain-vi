---
title: "CF 1731D - Bản đồ mới của Valiant"
description: "Chúng ta có một số lưới độc lập, mỗi lưới biểu thị một bản đồ thành phố trong đó mỗi ô chứa chiều cao tòa nhà. Từ mỗi lưới, chúng ta phải chọn một lưới con hình vuông có kích thước $l nhân l$ sao cho mọi ô bên trong hình vuông đó có chiều cao ít nhất là $l$."
date: "2026-06-15T02:56:24+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "dp", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1731
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 841 (Div. 2) and Divide by Zero 2022"
rating: 1700
weight: 1731
solve_time_s: 357
verified: true
draft: false
---

[CF 1731D - Bản đồ mới của Valiant](https://codeforces.com/problemset/problem/1731/D) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, cấu trúc dữ liệu, dp, hai con trỏ 
**Thời gian giải:** 5 phút 57 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số lưới độc lập, mỗi lưới biểu thị một bản đồ thành phố trong đó mỗi ô chứa chiều cao tòa nhà. Từ mỗi lưới, chúng ta phải chọn một lưới con hình vuông có kích thước$l \times l$sao cho mọi ô bên trong hình vuông đó có chiều cao ít nhất$l$. Nhiệm vụ là tìm giá trị lớn nhất có thể của$l$cho từng trường hợp thử nghiệm. 

Khó khăn chính là ràng buộc kết hợp hình học với các giá trị: việc mở rộng hình vuông khiến yêu cầu chặt chẽ hơn vì cả chiều dài cạnh và chiều cao tối thiểu cho phép đều tăng cùng nhau. Điều này làm cho bài toán về cơ bản khác với bài toán loại “bình phương lớn nhất của một” tiêu chuẩn trong đó ngưỡng được cố định. 

Các ràng buộc hàm ý tổng cộng nhiều nhất là$10^6$các ô trên tất cả các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi cách tiếp cận siêu tuyến tính cho mỗi trường hợp thử nghiệm, chẳng hạn như tính toán lại các câu trả lời cho mọi ô vuông ứng viên một cách ngây thơ với$O(n^3)$hoặc thậm chí$O(n^2 m)$hành vi. Một giải pháp thực hiện gần như$O(nm \log \min(n,m))$hoặc$O(nm)$mỗi trường hợp thử nghiệm tổng hợp là có thể chấp nhận được. 

Một vài hành vi cạnh rất dễ bị bỏ lỡ. Nếu tất cả các giá trị đều nhỏ, ví dụ như một lưới chứa đầy số 1, thì bất kỳ hình vuông nào lớn hơn$1 \times 1$ngay lập tức không thành công vì yêu cầu đã buộc các giá trị phải có độ dài tối thiểu bằng cạnh. Ngược lại, nếu các giá trị lớn nhưng nằm rải rác thì một giá trị lớn tách biệt sẽ không có tác dụng trừ khi nó tạo thành một vùng dày đặc. 

Một lỗi phổ biến là coi điều kiện đó như thể nó chỉ phụ thuộc vào kích thước hình vuông hoặc chỉ phụ thuộc vào các giá trị. Ví dụ: giả sử “hình vuông lớn nhất trong đó tất cả các giá trị ít nhất bằng 1” luôn là tối ưu bỏ qua việc tăng dần$l$tự tăng ngưỡng đó. 

## Phương pháp tiếp cận 

Giải pháp brute-force thử mọi góc trên bên trái có thể và mọi kích thước hình vuông có thể. Đối với mỗi ô vuông ứng cử viên, nó sẽ kiểm tra xem tất cả các ô có thỏa mãn điều kiện về chiều cao hay không. Việc này bao gồm tới$O(nm)$kiểm tra trên mỗi ô vuông, và có$O(nm)$hình vuông, dẫn đến sự phức tạp trong trường hợp xấu nhất xung quanh$O(n^2 m^2)$, vượt xa giới hạn khả thi đối với$10^6$tổng số tế bào. 

Quan sát quan trọng là điều kiện cho một cố định$l$trở thành bài toán lưới nhị phân tiêu chuẩn. Nếu chúng ta quyết định độ dài cạnh ứng cử viên$l$, chúng tôi chỉ quan tâm liệu có tồn tại một$l \times l$ma trận con trong đó tất cả các giá trị ít nhất$l$. Điều này biến lưới thành ma trận nhị phân trong đó các ô hợp lệ hoặc không hợp lệ tùy thuộc vào việc chúng có đáp ứng ngưỡng hay không. 

Đối với một ngưỡng cố định, chúng ta có thể tính toán hình vuông lớn nhất bằng cách sử dụng phép lặp lập trình động để theo dõi kết thúc hình vuông lớn nhất ở mỗi ô. Nếu bất kỳ ô nào đạt được giá trị ít nhất$l$, thì tồn tại một hình vuông hợp lệ. 

Vì tính khả thi là đơn điệu trong$l$(nếu một hình vuông phù hợp với$l$, nó cũng hoạt động với các giá trị nhỏ hơn), chúng ta có thể tìm kiếm nhị phân câu trả lời$l$. Mỗi lần kiểm tra chạy theo thời gian tuyến tính trên lưới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3 m^3)$|$O(1)$| Quá chậm | 
| Tìm kiếm nhị phân + DP |$O(nm \log \min(n,m))$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta rút gọn vấn đề thành việc kiểm tra xem độ dài một cạnh cho trước có$l$là khả thi. 

1. Đối với ứng viên cố định$l$, chuyển đổi lưới thành lưới nhị phân trong đó một ô là 1 nếu chiều cao của nó ít nhất$l$, nếu không thì bằng 0. Bước này mã hóa ràng buộc trực tiếp vào cấu trúc, vì vậy hình học là tất cả những gì còn lại. 
2. Xây dựng bảng lập trình động trong đó$dp[i][j]$biểu thị kích thước của hình vuông lớn nhất kết thúc tại ô$(i,j)$. Nếu ô hiện tại hợp lệ, chúng ta sẽ mở rộng ô nhỏ nhất trong ba ô lân cận. Nếu không thì nó bằng không. Điều này có hiệu quả vì bất kỳ hình vuông nào kết thúc tại$(i,j)$phải mở rộng các ô vuông hợp lệ từ phía trên, bên trái và theo đường chéo lên bên trái. 
3. Trong khi điền vào bảng DP, nếu có$dp[i][j]$đạt ít nhất$l$, chúng ta biết ngay một giá trị hợp lệ$l \times l$hình vuông tồn tại và có thể dừng sớm. 
4. Gói phần kiểm tra tính khả thi này vào bên trong tìm kiếm nhị phân$l$từ 1 đến$\min(n,m)$. Nếu một giá trị$l$khả thi, chúng tôi thử các giá trị lớn hơn; nếu không chúng tôi sẽ giảm phạm vi. 
5. Đáp án cuối cùng lớn nhất khả thi$l$. 

Sự tái phát DP là:$dp[i][j] = \begin{cases} 1 + \min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) & \text{if } a_{i,j} \ge l \\ 0 & \text{otherwise} \end{cases}$### Tại sao nó hoạt động 

Mỗi hình vuông hợp lệ kết thúc tại một ô phải chứa một hình vuông kết thúc nhỏ hơn ở mỗi hướng trong số ba hướng trước đó. Nếu bất kỳ hướng nào trong số đó phá vỡ điều kiện thì hình vuông không thể được mở rộng. Phép truy toán ghi lại chính xác hình vuông lớn nhất có thể được neo ở mỗi vị trí và bất kỳ giá trị hợp lệ nào$l \times l$hình vuông phải xuất hiện dưới dạng giá trị DP đạt ít nhất$l$. Điều này làm cho việc kiểm tra vừa cần thiết vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(l, n, m, a):
    dp = [0] * (m + 1)
    prev = [0] * (m + 1)
    
    for i in range(1, n + 1):
        for j in range(1, m + 1):
            if a[i-1][j-1] >= l:
                dp[j] = min(prev[j], dp[j-1], prev[j-1]) + 1
                if dp[j] >= l:
                    return True
            else:
                dp[j] = 0
        prev, dp = dp, prev
    return False

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = [list(map(int, input().split())) for _ in range(n)]
        
        lo, hi = 1, min(n, m)
        ans = 1
        
        while lo <= hi:
            mid = (lo + hi) // 2
            if can(mid, n, m, a):
                ans = mid
                lo = mid + 1
            else:
                hi = mid - 1
        
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp tách biệt việc kiểm tra tính khả thi khỏi việc tối ưu hóa. các`can`Hàm mã hóa logic DP trong khi quét từng hàng, chỉ sử dụng hai mảng cuộn để giữ cho bộ nhớ tuyến tính theo số cột. 

Tìm kiếm nhị phân đưa ra câu trả lời cuối cùng vì tính khả thi là đơn điệu trong$l$: một lần có kích thước một hình vuông$l$tồn tại, tất cả các kích thước nhỏ hơn đều có thể tự động được thực hiện vì cả yêu cầu về kích thước và ngưỡng chiều cao đều giảm cùng nhau. 

Một chi tiết triển khai tinh tế là thoát sớm bên trong vòng lặp DP. Khi bất kỳ ô nào đạt được kích thước tối thiểu là một hình vuông$l$, việc tính toán thêm là không cần thiết cho thử nghiệm đó, điều này giúp cải thiện đáng kể hiệu suất trong các trường hợp dày đặc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một lưới nhỏ:```
3 3
5 5 5
5 5 5
5 5 5
```Chúng tôi kiểm tra các giá trị ứng viên của$l$. 

| tôi | Kết quả DP (ý tưởng chính) | khả thi | 
| --- | --- | --- | 
| 1 | tất cả các ô hợp lệ | vâng | 
| 2 | tồn tại hình vuông 2x2 | vâng | 
| 3 | hình vuông 3x3 tồn tại | vâng | 

Tìm kiếm nhị phân sẽ hội tụ về 3, phù hợp với kích thước lưới đầy đủ. 

Điều này chứng tỏ trường hợp cấu trúc đồng nhất và câu trả lời chỉ bị giới hạn bởi hình học. 

### Ví dụ 2```
2 3
1 2 3
2 3 4
```Vì$l=2$, chúng tôi yêu cầu tất cả các ô trong một hình vuông 2x2 phải có ít nhất 2. Chỉ một vùng như vậy thỏa mãn một phần điều này, nhưng một ô không đạt giới hạn ngưỡng, do đó không có hình vuông 2x2 nào hoạt động. 

| tôi | khả thi | 
| --- | --- | 
| 1 | vâng | 
| 2 | không | 

Câu trả lời là 1, thể hiện sự tương tác giữa việc tăng kích thước và tăng ngưỡng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm \log \min(n,m))$| mỗi bước tìm kiếm nhị phân sẽ quét toàn bộ DP | 
| Không gian |$O(m)$| mảng cuộn chỉ lưu trữ các trạng thái hàng trước đó | 

Tổng số ô trên tất cả các trường hợp thử nghiệm được giới hạn bởi$10^6$, do đó, ngay cả với khoảng 20 lần lặp lại tìm kiếm nhị phân, tổng số phép toán vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def can(l, n, m, a):
        dp = [0] * (m + 1)
        prev = [0] * (m + 1)
        for i in range(1, n + 1):
            for j in range(1, m + 1):
                if a[i-1][j-1] >= l:
                    dp[j] = min(prev[j], dp[j-1], prev[j-1]) + 1
                    if dp[j] >= l:
                        return True
                else:
                    dp[j] = 0
            prev, dp = dp, prev
        return False

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        a = [list(map(int, input().split())) for _ in range(n)]
        lo, hi = 1, min(n, m)
        ans = 1
        while lo <= hi:
            mid = (lo + hi) // 2
            if can(mid, n, m, a):
                ans = mid
                lo = mid + 1
            else:
                hi = mid - 1
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("""4
2 2
2 3
4 5
1 3
1 2 3
2 3
4 4 3
2 1 4
5 6
1 9 4 6 5 8
10 9 5 8 11 6
24 42 32 8 11 1
23 1 9 69 13 3
13 22 60 12 14 17
""") == """2
1
1
3"""

# custom cases
assert run("""1
1 1
5
""") == "1", "min case"

assert run("""1
2 2
1 1
1 1
""") == "1", "all small values"

assert run("""1
3 3
9 9 9
9 9 9
9 9 9
""") == "3", "all large values"

assert run("""1
3 3
1 2 3
4 5 6
7 8 9
""") == "2", "mixed grid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 ô đơn | 1 | ranh giới tối thiểu | 
| tất cả những cái 2x2 | 1 | hạn chế ngưỡng | 
| tất cả cao 3x3 | 3 | đầy đủ tính khả thi | 
| tăng lưới | 2 | cấu trúc không đồng nhất | 

## Vỏ cạnh 

Lưới một ô hoạt động bình thường vì hình vuông duy nhất có thể có kích thước 1 và yêu cầu về ngưỡng luôn được đáp ứng khi giá trị ít nhất là 1. 

Một lưới thấp đồng nhất chẳng hạn như tất cả các lưới đảm bảo rằng bất kỳ nỗ lực nào để tạo thành các hình vuông lớn hơn 1 đều thất bại ngay lập tức do giới hạn chiều cao tỷ lệ với kích thước hình vuông, buộc phải từ chối bất kể hình học. 

Một lưới cao đồng nhất chẳng hạn như tất cả các số 9 làm cho câu trả lời hoàn toàn là hình học và DP sẽ truyền các kích thước hình vuông tối đa cho đến khi bị giới hạn bởi ranh giới lưới thay vì các giá trị.
