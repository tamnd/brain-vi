---
title: "CF 1475D - Vệ sinh điện thoại"
description: "Chúng tôi được cấp một chiếc điện thoại có danh sách các ứng dụng đã cài đặt. Mỗi ứng dụng tiêu tốn một lượng bộ nhớ nhất định và cũng có một “chi phí” được đo bằng sự mất đi sự thuận tiện nếu chúng ta xóa nó. Mỗi ứng dụng đóng góp 1 hoặc 2 điểm tiện lợi."
date: "2026-06-11T00:08:30+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1475
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 697 (Div. 3)"
rating: 1800
weight: 1475
solve_time_s: 102
verified: true
draft: false
---

[CF 1475D - Vệ sinh điện thoại](https://codeforces.com/problemset/problem/1475/D) 

**Đánh giá:** 1800 
**Thẻ:** tìm kiếm nhị phân, dp, sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chiếc điện thoại có danh sách các ứng dụng đã cài đặt. Mỗi ứng dụng tiêu tốn một lượng bộ nhớ nhất định và cũng có một “chi phí” được đo bằng sự mất đi sự thuận tiện nếu chúng ta xóa nó. Mỗi ứng dụng đóng góp 1 hoặc 2 điểm tiện lợi. 

Nhiệm vụ là xóa một tập hợp con ứng dụng để tổng bộ nhớ được giải phóng ít nhất đạt đến ngưỡng bắt buộc$m$, đồng thời giảm thiểu tổng số tiện ích bị mất. Nếu không thể giải phóng đủ bộ nhớ, chúng tôi phải báo cáo thực tế đó. 

Vì vậy, đầu vào cung cấp cho chúng ta hai mảng song song: một mảng mô tả kích thước bộ nhớ, một mảng mô tả mức độ “quan trọng” của mỗi ứng dụng. Chúng tôi đang chọn một tập hợp con thỏa mãn ràng buộc giống như chiếc ba lô về tổng kích thước, nhưng chi phí lại đơn giản một cách bất thường vì nó chỉ lấy hai giá trị có thể. 

Hạn chế chính trong việc định hình giải pháp là tổng số ứng dụng trên tất cả các trường hợp thử nghiệm, nhiều nhất là$2 \cdot 10^5$. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các tập hợp con hoặc sử dụng lập trình động bậc hai trên các giá trị bộ nhớ. Một chiếc ba lô đầy ắp$m$cũng là không thể vì$m$có thể đạt được$10^9$. 

Một kiểu thất bại phổ biến xuất phát từ việc coi vấn đề này giống như một chiếc ba lô tiêu chuẩn hoặc vấn đề tham lam theo tỷ lệ. Ví dụ: việc chọn ứng dụng có tỷ lệ bộ nhớ trên chi phí tốt nhất có thể không thành công: 

đầu vào:```
3 10
6 5 4
2 1 1
```Tỷ lệ tham lam có thể chọn (6,2) và (5,1) cho tổng bộ nhớ 11 chi phí 3. Nhưng tối ưu là (6,2)+(4,1)=10 chi phí 3, do đó, mối quan hệ có thể che giấu sai sót; những ví dụ tồi tệ hơn tồn tại khi tham lam thất bại vì chi phí chỉ bằng 1 hoặc 2, không liên tục. 

Một trường hợp khó phát hiện khác là khi giải pháp tốt nhất sử dụng nhiều ứng dụng giá rẻ (b = 1), nhưng cách tiếp cận ngây thơ sẽ làm cạn kiệt b = 2 ứng dụng trước và dừng quá sớm. 

Cơ cấu chi phí chỉ bằng 1 hoặc 2 là điều khiến vấn đề trở nên dễ giải quyết. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các tập hợp con của ứng dụng, tính toán tổng bộ nhớ và tổng chi phí, đồng thời lấy tập hợp con hợp lệ nhất. Điều này hoạt động về mặt khái niệm vì mọi tập hợp con đều được kiểm tra, nhưng nó yêu cầu$2^n$đánh giá, điều này vượt xa khả năng ngay cả đối với$n = 40$. 

Một cách mạnh mẽ có cấu trúc hơn là thử tất cả các tập hợp con bằng cách sử dụng đệ quy hoặc DP trên chỉ mục và tổng bộ nhớ, nhưng kích thước bộ nhớ lên tới$10^9$, khiến mọi vụ nổ không gian trạng thái DP xảy ra ngay lập tức. 

Quan sát quan trọng là chi phí có tính chất nhị phân: mọi ứng dụng đều thuộc một trong hai nhóm. Điều này gợi ý việc tách ứng dụng thành hai danh sách dựa trên chi phí của chúng. Nếu chúng ta sắp xếp các ứng dụng trong mỗi nhóm theo bộ nhớ giảm dần thì mọi giải pháp tối ưu sẽ lấy một số mục có bộ nhớ lớn nhất từ ​​nhóm chi phí 1 và nhóm chi phí 2. 

Tuy nhiên, chọn chúng một cách độc lập là chưa đủ vì sự đánh đổi mang tính toàn cầu: đôi khi sử dụng ứng dụng cost-2 nhỏ hơn một chút sẽ tốt hơn nếu nó giải phóng nhiều bộ nhớ hơn. 

Thông tin chi tiết quan trọng là sắp xếp tất cả các ứng dụng theo mức giảm dần bộ nhớ nhưng xử lý chúng theo cách cho phép lựa chọn tăng dần số lượng ứng dụng có chi phí 2 mà chúng tôi đưa vào. Chúng tôi tính toán trước tổng tiền tố của bộ nhớ cho cả hai nhóm sau khi sắp xếp. Sau đó, chúng tôi thử mọi cách có thể để đạt được điều tốt nhất$k$ứng dụng giá 2 và đáp ứng yêu cầu bộ nhớ còn lại bằng ứng dụng giá 1. 

Điều này làm giảm vấn đề khi kết hợp hai chuỗi đã sắp xếp với tổng tiền tố, thay vì tìm kiếm các tập hợp con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Tối ưu (sắp xếp + tham lam + tổng tiền tố) |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chia ứng dụng thành hai nhóm: nhóm có chi phí 1 và nhóm có chi phí 2. 

1. Tách tất cả các ứng dụng thành hai danh sách dựa trên giá của chúng. Điều này tách biệt hai “bậc định giá” mà chúng ta sẽ đánh đổi với nhau. 
2. Sắp xếp cả hai danh sách theo thứ tự kích thước bộ nhớ giảm dần. Điều này đảm bảo rằng bất cứ khi nào chúng tôi thực hiện$k$ứng dụng từ một nhóm, chúng tôi luôn chọn những ứng dụng hiệu quả nhất về mặt tăng bộ nhớ. 
3. Xây dựng mảng tổng tiền tố cho cả hai nhóm. Tổng tiền tố tại chỉ mục$i$cung cấp bộ nhớ tối đa có thể đạt được bằng cách tận dụng tốt nhất$i$ứng dụng thuộc loại đó. 
4. Tính tổng bộ nhớ khả dụng. Nếu việc lấy tất cả các ứng dụng vẫn chưa đủ, hãy xuất ngay -1. 
5. Với mỗi số có thể$i$trong số các ứng dụng có giá 2 được lấy (từ 0 đến len(b2)), tính toán bộ nhớ đạt được. Nếu nó đã đạt tới$m$, ghi lại chi phí$2i$. 
6. Nếu không đủ, hãy tính toán bộ nhớ còn lại cần thiết và xác định số lượng ứng dụng giá 1 được yêu cầu bằng cách sử dụng tìm kiếm nhị phân trên tổng tiền tố. 
7. Theo dõi tổng chi phí tối thiểu trên tất cả các phần chia tách hợp lệ. 

Lý do điều này hiệu quả là vì các ứng dụng giá 2 có chi phí thấp hơn nhưng có khả năng tốt hơn về bộ nhớ cho mỗi mục, vì vậy, quyết định có ý nghĩa duy nhất là bao nhiêu trong số chúng sẽ được đưa vào. Sau khi được sửa, phần còn lại sẽ giảm xuống chỉ còn một lần lấp đầy tham lam từ các ứng dụng giá 1. 

### Tại sao nó hoạt động 

Mọi giải pháp tối ưu đều có thể được sắp xếp lại sao cho trong mỗi nhóm chi phí, các ứng dụng được chọn là những ứng dụng có bộ nhớ lớn nhất. Nếu một giải pháp sử dụng ứng dụng có bộ nhớ nhỏ hơn trong khi ứng dụng lớn hơn không được sử dụng trong cùng một nhóm thì việc hoán đổi sẽ cải thiện hoặc duy trì tính khả thi mà không làm tăng chi phí. Đối số trao đổi này đảm bảo rằng việc chỉ xem xét các lựa chọn tiền tố bên trong mỗi nhóm sẽ không bao giờ bỏ lỡ cấu hình tối ưu. Khi cả hai nhóm được giảm xuống các lựa chọn tiền tố, mức độ tự do duy nhất còn lại là số lượng vật phẩm được lấy từ nhóm giá 2. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        ones = []
        twos = []
        
        total = 0
        
        for ai, bi in zip(a, b):
            total += ai
            if bi == 1:
                ones.append(ai)
            else:
                twos.append(ai)
        
        if total < m:
            print(-1)
            continue
        
        ones.sort(reverse=True)
        twos.sort(reverse=True)
        
        pre1 = [0]
        for x in ones:
            pre1.append(pre1[-1] + x)
        
        pre2 = [0]
        for x in twos:
            pre2.append(pre2[-1] + x)
        
        ans = float('inf')
        
        j = len(ones)
        
        for i in range(len(twos) + 1):
            mem2 = pre2[i]
            cost = 2 * i
            
            if mem2 >= m:
                ans = min(ans, cost)
                continue
            
            need = m - mem2
            
            # smallest j such that pre1[j] >= need
            l, r = 0, len(ones)
            while l <= r:
                mid = (l + r) // 2
                if pre1[mid] >= need:
                    j = mid
                    r = mid - 1
                else:
                    l = mid + 1
            
            if pre1[j] >= need:
                ans = min(ans, cost + j)
        
        print(ans if ans != float('inf') else -1)

if __name__ == "__main__":
    solve()
```Việc triển khai đầu tiên sẽ phân vùng các ứng dụng thành hai mảng và sắp xếp từng mảng. Tổng tiền tố cho phép đánh giá theo thời gian không đổi khi lấy một số cố định từ mỗi nhóm. Tìm kiếm nhị phân tìm thấy số lượng ứng dụng giá 1 tối thiểu cần thiết sau khi sửa một lựa chọn ứng dụng giá 2. Điều này tránh việc tính toán lại tổng nhiều lần và giữ lời giải trong khoảng thời gian logarit cho mỗi lần lặp trong nhóm thứ hai. 

Một điểm tinh tế là chúng ta phải tính toán lại tìm kiếm nhị phân cho mỗi lựa chọn về giá trị 2, vì yêu cầu còn lại thay đổi. Một chi tiết quan trọng khác là xử lý trường hợp chỉ riêng ứng dụng có giá 2 đã đáp ứng yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=5, m=7
a=[5,3,2,1,4]
b=[2,1,1,2,1]
```Chúng tôi chia: 

| Nhóm | Giá trị | 
| --- | --- | 
| giá 1 | [3,2,4] | 
| giá 2 | [5,1] | 

Đã sắp xếp: 

giá 1 → [4,3,2] 

giá 2 → [5,1] 

Tổng tiền tố: 

giá 1: [0,4,7,9] 

giá 2: [0,5,6] 

Chúng tôi thử lựa chọn: 

| tính chi phí2 | mem2 | còn lại | chi phí tốt nhất1 | tổng chi phí | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 7 | 2 (3+4) | 2 | 
| 1 | 5 | 2 | 1 | 3 | 
| 2 | 6 | 1 | 0 | 4 | 

Tối thiểu là 2. 

Điều này xác nhận rằng việc bỏ qua hoàn toàn các ứng dụng đắt tiền có chi phí 2 có thể là tối ưu. 

### Ví dụ 2 

đầu vào:```
n=4, m=10
a=[5,1,3,4]
b=[1,2,1,2]
```giá 1: [5,3] 

giá 2: [4,1] 

Tổng tiền tố: 

giá 1: [0,5,8] 

giá 2: [0,4,5] 

Hãy thử: 

| chi phí2 | mem2 | còn lại | chi phí1 cần thiết | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 10 | không thể | thông tin | 
| 1 | 4 | 6 | 2 | 4 | 
| 2 | 5 | 5 | 1 | 5 | 

Tốt nhất là 4. 

Điều này cho thấy sự cân bằng giữa việc mua nhiều vật phẩm giá 2 hơn so với việc dựa vào những sản phẩm nặng giá 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; mỗi thử nghiệm cũng thực hiện quét tuyến tính trên các mục giá 2 bằng tìm kiếm nhị phân qua tiền tố giá 1 | 
| Không gian |$O(n)$| Lưu trữ hai nhóm và tổng tiền tố | 

Tổng độ phức tạp là an toàn vì tổng của$n$qua các bài kiểm tra được giới hạn bởi$2 \cdot 10^5$và mỗi phép toán tối đa là logarit hoặc tuyến tính trong tổng số đó. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        ones, twos = [], []
        total = 0
        
        for ai, bi in zip(a, b):
            total += ai
            if bi == 1:
                ones.append(ai)
            else:
                twos.append(ai)
        
        if total < m:
            out.append("-1")
            continue
        
        ones.sort(reverse=True)
        twos.sort(reverse=True)
        
        pre1 = [0]
        for x in ones:
            pre1.append(pre1[-1] + x)
        
        pre2 = [0]
        for x in twos:
            pre2.append(pre2[-1] + x)
        
        ans = float('inf')
        
        for i in range(len(twos) + 1):
            mem2 = pre2[i]
            cost = 2 * i
            
            if mem2 >= m:
                ans = min(ans, cost)
                continue
            
            need = m - mem2
            l, r = 0, len(ones)
            j = len(ones)
            
            while l <= r:
                mid = (l + r) // 2
                if pre1[mid] >= need:
                    j = mid
                    r = mid - 1
                else:
                    l = mid + 1
            
            if pre1[j] >= need:
                ans = min(ans, cost + j)
        
        out.append(str(ans if ans != float('inf') else -1))
    
    return "\n".join(out)

# provided samples
assert run("""5
5 7
5 3 2 1 4
2 1 1 2 1
1 3
2
1
5 10
2 3 2 3 2
1 2 1 2 1
4 10
5 1 3 4
1 2 1 2
4 5
3 2 1 2
2 1 2 1
""") == """2
-1
6
4
3"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp không thể tối thiểu | -1 | tổng bộ nhớ không đủ | 
| tất cả ứng dụng tính phí 1 | sự đúng đắn của tiền tố tham lam | nhóm giá 2 trống | 
| hỗn hợp nặng chi phí-2 | sự đánh đổi đúng đắn | cân bằng giữa các nhóm | 
| cạnh cao m | hành vi ranh giới | xử lý yêu cầu lớn |
