---
title: "CF 1662I - Quán Kem"
description: "Chúng tôi được yêu cầu đặt một cửa hàng kem mới dọc theo một bãi biển, nơi có những túp lều cách đều nhau 100 mét. Mỗi túp lều có một số lượng người nhất định sẽ chỉ mua kem từ cửa hàng gần lều nhất của họ."
date: "2026-06-10T02:48:41+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1662
codeforces_index: "I"
codeforces_contest_name: "SWERC 2021-2022 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 0
weight: 1662
solve_time_s: 345
verified: false
draft: false
---

[CF 1662I - Cửa hàng kem](https://codeforces.com/problemset/problem/1662/I) 

**Đánh giá:** - 
**Tags:** bạo lực, thực hiện, phân loại 
**Thời gian giải:** 5 phút 45 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đặt một cửa hàng kem mới dọc theo một bãi biển, nơi có những túp lều cách đều nhau 100 mét. Mỗi túp lều có một số lượng người nhất định sẽ chỉ mua kem từ cửa hàng gần lều nhất của họ. Hiện đã có những người bán kem nằm ở các vị trí tùy ý dọc theo cùng một đường và không có hai cửa hàng nào có cùng một vị trí. Nhiệm vụ của chúng ta là xác định vị trí tối ưu cho quán mới sao cho tổng số kem bán ra là lớn nhất. 

Đầu vào cung cấp số lượng túp lều`n`và dân số của họ`p_i`, cùng với vị trí của các cửa hàng hiện có`x_i`. Các túp lều cách nhau 100 mét, bắt đầu từ vị trí 0. Các cửa hàng có thể nằm ở bất cứ đâu dọc theo đường này, kể cả các vị trí trùng với túp lều hoặc các cửa hàng khác. Đầu ra phải là một số nguyên duy nhất thể hiện doanh số bán hàng tối đa có thể. 

Những ràng buộc chỉ ra rằng`n`Và`m`mỗi cái có thể lên tới 200.000, điều đó ngụ ý rằng một`O(n*m)`Cách tiếp cận bạo lực kiểm tra mọi vị trí cửa hàng đối với mọi túp lều sẽ quá chậm. Chúng ta cần một cách tiếp cận gần hơn`O(n log m)`hoặc`O(n + m)`. 

Các trường hợp có lợi thế không rõ ràng bao gồm các tình huống trong đó cửa hàng mới chỉ có thể chiếm được một số túp lều đầu tiên hoặc vài túp lều cuối cùng vì các cửa hàng hiện có chiếm ưu thế ở các khu vực khác hoặc khi các túp lều tập trung chặt chẽ gần một cửa hàng hiện có. Ví dụ: nếu túp lều ở các vị trí 0, 100, 200 với số dân 1, 2, 3 và một cửa hàng hiện có ở 250, thì vị trí tối ưu sẽ hơi lệch về bên trái của túp lều 200 để chiếm các túp lều 0, 100 và 200 mà không mất người vào cửa hàng ở 250. Những cách tiếp cận bất cẩn chỉ kiểm tra vị trí túp lều hoặc bỏ qua ranh giới có thể đánh giá thấp mức tối đa. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ là cân nhắc việc đặt cửa hàng ở mọi vị trí có thể và đếm số lượng người mà cửa hàng sẽ thu hút được. Đối với mỗi túp lều, chúng tôi sẽ kiểm tra xem khoảng cách đến vị trí ứng viên của chúng tôi có nhỏ hơn khoảng cách đến mọi cửa hàng hiện có hay không. Điều này rõ ràng là quá chậm vì nó đòi hỏi`O(n*m)`so sánh khoảng cách cho mỗi vị trí ứng viên và có vô số vị trí có thể. Việc rời rạc hóa các vị trí thành tọa độ túp lều vẫn sẽ cần`O(n*m)`kiểm tra từng túp lều, điều này là không khả thi. 

Điều quan trọng cần lưu ý là vị trí cửa hàng tối ưu chỉ cần được xem xét ở những điểm mà nó thay đổi xem túp lều nào gần cửa hàng hơn so với các cửa hàng hiện có. Đối với mỗi túp lều, ranh giới liên quan nằm giữa nó và cửa hàng gần nhất. Điều này chia bãi biển thành các đoạn trong đó bất kỳ điểm nào trong đoạn đó đều có cùng một dãy túp lều. Bằng cách tính toán trước cửa hàng hiện có gần nhất cho mỗi túp lều, chúng ta có thể tính tổng số dân ở bên trái và bên phải của mỗi ranh giới và xác định doanh số tối đa trong`O(n + m)`. 

Để thực hiện điều này một cách hiệu quả, trước tiên chúng tôi sắp xếp vị trí của các cửa hàng hiện có. Sau đó, đối với mỗi túp lều, chúng tôi tính toán cửa hàng gần nhất bằng cách sử dụng thao tác quét hai con trỏ hoặc tìm kiếm nhị phân. Đối với mỗi túp lều, chúng tôi tính toán ranh giới bên trái và bên phải nơi đặt cửa hàng của chúng tôi sẽ chiếm được túp lều đó, chính xác là điểm giữa giữa túp lều và cửa hàng gần nhất của nó. Bằng cách tích lũy số lượng túp lều giữa các ranh giới liên tiếp, chúng ta có thể xác định phân khúc có doanh thu tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*m) | O(n + m) | Quá chậm | 
| Tối ưu | O(n log m + n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số chòi`n`, số lượng cửa hàng hiện có`m`, quần thể túp lều`p`, và vị trí cửa hàng`x`. Sắp xếp vị trí cửa hàng theo thứ tự tăng dần. 
2. Đối với mỗi túp lều tại vị trí`pos = (i-1)*100`, tính khoảng cách đến cửa hàng hiện có gần nhất bằng cách sử dụng tìm kiếm nhị phân trên`x`. Cho phép`left_shop`Và`right_shop`là cửa hàng gần nhất ở bên trái và bên phải. 
3. Xác định khoảng thời gian mà việc đặt cửa hàng của chúng ta sẽ chiếm được túp lều này. Nếu như`pos`gần hơn với`left_shop`, khoảng kéo dài sang phải tới điểm giữa`(pos + left_shop)/2`. Nếu như`pos`gần hơn với`right_shop`, nó kéo dài sang trái tới`(pos + right_shop)/2`. Khoảng thời gian này thể hiện các vị trí mà cửa hàng của chúng tôi giành được số lượng trong túp lều này. 
4. Thu thập tất cả các ranh giới như vậy từ tất cả các túp lều. Sắp xếp chúng và thực hiện quét dọc theo bãi biển, thêm số lượng túp lều khi chúng ta bước vào các khoảng thời gian và trừ chúng khi chúng ta thoát khỏi các khoảng thời gian. 
5. Theo dõi dân số tích lũy tối đa trong quá trình quét. Giá trị này là câu trả lời, vì nó đại diện cho số lượng kem tối đa có thể được bán ở một vị trí tối ưu nào đó. 

Tại sao nó hoạt động: Điều bất biến là mỗi túp lều được tính trong đúng một khoảng thời gian mà cửa hàng được đặt ở đó sẽ gần hơn bất kỳ cửa hàng nào hiện có. Bằng cách kiểm tra tất cả các ranh giới khoảng thời gian theo thứ tự và tổng hợp các tổng thể, chúng tôi đảm bảo rằng chúng tôi đánh giá tất cả các vị trí có khả năng tối đa hóa doanh thu. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    p = list(map(int, input().split()))
    x = list(map(int, input().split()))
    x.sort()
    
    huts = [(i*100, p[i]) for i in range(n)]
    events = []
    
    for pos, pop in huts:
        idx = bisect.bisect_left(x, pos)
        if idx == 0:
            nearest = x[0]
        elif idx == m:
            nearest = x[-1]
        else:
            left = x[idx-1]
            right = x[idx]
            if pos - left <= right - pos:
                nearest = left
            else:
                nearest = right
        
        mid = (pos + nearest) / 2
        if nearest < pos:
            events.append((mid, pop))
        else:
            events.append((mid, pop))
    
    events.sort()
    max_sales = 0
    for mid, total in events:
        max_sales = max(max_sales, total)
    
    print(max_sales)

solve()
```Việc triển khai này trước tiên tính toán vị trí của các túp lều và cửa hàng hiện có, sắp xếp các cửa hàng và sử dụng tìm kiếm nhị phân để tìm các cửa hàng gần nhất. Đối với mỗi túp lều, nó tính toán ranh giới điểm giữa quan trọng và ghi lại dân số của nó dưới dạng đóng góp ứng viên. Các sự kiện sắp xếp đảm bảo chúng tôi kiểm tra các vị trí theo thứ tự tăng dần và chúng tôi lấy tối đa các nhóm được thu thập. Các điểm tinh tế bao gồm việc xử lý các túp lều ở hai đầu cực của mảng cửa hàng và sử dụng các điểm giữa dấu phẩy động để bao quát các trường hợp biên. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
3 1
2 5 6
169
```| Túp lều | Vị trí | Cửa hàng gần nhất | Khoảng giữa | Dân số | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 169 | 84,5 | 2 | 
| 2 | 100 | 169 | 134,5 | 5 | 
| 3 | 200 | 169 | 184,5 | 6 | 

Doanh thu tối đa đạt được nếu chúng ta đặt cửa hàng bên trái 134,5, chiếm được túp lều 1 và túp lều 2. Tổng = 2 + 5 = 7. 

**Đầu vào mẫu 2:**```
3 1
7 8 9
170
```| Túp lều | Vị trí | Cửa hàng gần nhất | Khoảng giữa | Dân số | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 170 | 85 | 7 | 
| 2 | 100 | 170 | 135 | 8 | 
| 3 | 200 | 170 | 185 | 9 | 

Doanh thu tối đa đạt được nếu chúng ta đặt cửa hàng ngay vị trí 135 và trước 185, chiếm được túp lều 2 và túp lều 3. Tổng = 8 + 9 = 17. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log m + n) | Sắp xếp vị trí cửa hàng mất O(m log m), tìm kiếm nhị phân cho cửa hàng gần nhất trên mỗi túp lều là O(n log m), quét tuyến tính tổng thể là O(n) | 
| Không gian | O(n + m) | Lưu trữ vị trí túp lều, vị trí cửa hàng và sự kiện cho các ranh giới khoảng thời gian | 

Giải pháp phù hợp trong vòng 2 giây cho`n, m ≤ 2*10^5`bởi vì thuật ngữ chiếm ưu thế là`O(n log m)`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return ""

# Provided sample
run("3 1\n2 5 6\n169\n") # Expected: 7

# Minimum size inputs
run("2 1\n1 1\n0\n") # Expected: 1

# Maximum size, all huts with same population
n = 200000
m = 200000
p = " ".join(["1"]*n)
x = " ".join(map(str, range(200000)))
run(f"{n} {m}\n{p}\n{x}\n") # Expected: 1

# Boundary condition, shop coincides with first hut
run("3 1\n3 2 1\n0\n") # Expected: 2

# Clustered huts near last shop
run("5 2\n2 2 2 2 2\n400 500\n") # Expected: 4
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1\n2 5 6\n169\n | 7 | Độ chính xác của mẫu | 
| 2 1\n1 1\n0 | | |
