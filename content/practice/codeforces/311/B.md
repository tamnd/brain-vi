---
title: "CF 311B - Vận Chuyển Mèo"
description: "Chúng tôi có một trang trại với một con đường thẳng và một số ngọn đồi được đánh số từ 1 đến n. Mỗi ngọn đồi cách nhau một khoảng cách nhất định. Có m con mèo, mỗi con kết thúc việc lang thang trên một ngọn đồi cụ thể vào một thời điểm nhất định và đợi ở đó để kiếm thức ăn."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 311
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 185 (Div. 1)"
rating: 2400
weight: 311
solve_time_s: 56
verified: true
draft: false
---

[CF 311B - Vận chuyển mèo](https://codeforces.com/problemset/problem/311/B) 

**Đánh giá:** 2400 
**Thẻ:** cấu trúc dữ liệu, dp 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một trang trại với một con đường thẳng và một số ngọn đồi được đánh số từ 1 đến _n_. Mỗi ngọn đồi cách nhau một khoảng cách nhất định. Có _m_ con mèo, mỗi con kết thúc việc lang thang trên một ngọn đồi cụ thể vào một thời điểm nhất định và đợi ở đó để kiếm thức ăn. Có những người cho ăn _p_, tất cả đều bắt đầu từ ngọn đồi 1, đi dọc đường, nhặt tất cả những con mèo đang chờ sẵn. Người cho ăn di chuyển với tốc độ 1 và có thể mang theo bất kỳ số lượng mèo nào. Thời gian chờ đợi của mỗi con mèo là sự chênh lệch giữa thời gian người cho ăn đến ngọn đồi của nó và thời gian về đích của con mèo, nếu người cho ăn đến muộn hơn khi con mèo kết thúc hoặc bằng 0 nếu người cho ăn đến sớm hơn. Chúng tôi muốn sắp xếp thời gian khởi hành của tất cả các nguồn cấp dữ liệu để tổng thời gian chờ đợi được giảm thiểu. 

Dữ liệu đầu vào cung cấp số ngọn đồi _n_, mèo _m_, người cho ăn _p_, khoảng cách giữa các ngọn đồi cũng như thời gian về đích và ngọn đồi của mỗi con mèo. Đầu ra là một số nguyên: tổng thời gian chờ tối thiểu. 

Với các ràng buộc, _n_ và _m_ có thể đạt tới 10^5. Điều đó loại trừ các thuật toán có tính bậc hai, vì 10^10 phép toán sẽ vượt quá 2 giây. Số lượng bộ nạp _p_ nhỏ, lên tới 100, điều này cho thấy rằng lập trình động hoặc tối ưu hóa lồi trên các con mèo có thể khả thi. Các trường hợp khó khăn bao gồm mèo về đích ở ngọn đồi đầu tiên (có thể không bao giờ chờ đợi) hoặc nhiều mèo về đích ở cùng một ngọn đồi cùng lúc, cũng như có nhiều người cho ăn hơn những ngọn đồi có mèo. 

Rõ ràng là không thể thực hiện một cách triển khai ngây thơ nhằm thử mọi lịch trình khởi hành trung chuyển có thể. Một trường hợp tinh vi khác là khi nhiều con mèo về đích ở cùng một ngọn đồi vào những thời điểm rất khác nhau. Chỉ cần gửi một máng ăn ngay khi con mèo đầu tiên ăn xong có thể khiến những con mèo sau phải chờ đợi quá lâu nếu một máng ăn khác không được lên lịch cẩn thận. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ xem xét mọi việc phân công mèo đến nơi cho ăn và thời gian khởi hành, tính toán thời gian chờ đợi cho mỗi lần và lấy mức tối thiểu. Với _m_ cats và _p_ Feeders, số lượng phân vùng có thể có là theo cấp số nhân tính theo _m_. Ngay cả việc chỉ lặp qua tất cả các tập hợp con mèo để tìm một máng ăn cũng không khả thi vì _m_ ≤ 10^5. 

Quan sát quan trọng là người cho ăn di chuyển nghiêm ngặt từ trái sang phải và không bao giờ quay lại. Điều đó cho phép chúng ta sắp xếp mèo theo khoảng cách được điều chỉnh theo thời gian mà chúng cần chờ, tức là thời gian về đích của mèo trừ đi khoảng cách từ đồi 1 đến đồi của nó. Sau khi sắp xếp xong, chúng ta có thể xử lý vấn đề bằng cách chia chuỗi thời gian đã điều chỉnh thành _p_ các nhóm liên tiếp, trong đó mỗi nhóm được xử lý bởi một bộ cấp dữ liệu duy nhất. Tổng thời gian chờ đợi của một nhóm sẽ được giảm thiểu nếu người cho ăn rời đi vào thời điểm đã điều chỉnh của con mèo đến sớm nhất trong nhóm đó. Điều này biến vấn đề thành bài toán phân chia chi phí lồi cổ điển, có thể giải được bằng lập trình động với tối ưu hóa chia để trị. 

Nếu không tối ưu hóa, DP trên _m_ mèo dành cho người cho ăn _p_ sẽ là O(m^2 * p), quá chậm đối với _m_ = 10^5. Bằng cách sử dụng thuộc tính lồi của hàm chi phí, DP chia để trị giảm giá trị này xuống O(m * p log m), có thể chấp nhận được với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m) | O(m) | Quá chậm | 
| DP với tối ưu hóa lồi | O(m * p log m) | O(m * p) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng tiền tố của khoảng cách giữa các ngọn đồi để có được vị trí chính xác của từng ngọn đồi so với ngọn đồi 1. Điều này cho phép chuyển đồi của mèo thành thời gian đến chính xác nếu người đưa đón rời đi vào thời điểm 0. 
2. Với mỗi con mèo, hãy tính thời gian hoàn thành đã điều chỉnh:`adjusted_time = t_i - prefix_distance[h_i]`. Đây là lần muộn nhất người cho ăn phải rời khỏi đồi 1 để đến đúng lúc mèo ăn xong. Việc sắp xếp mèo theo thời gian điều chỉnh này đảm bảo chúng tôi xử lý chúng theo thứ tự tăng dần về thời gian khởi hành của máng ăn bắt buộc. 
3. Sắp xếp mèo theo thời gian đã điều chỉnh. Giải pháp tối ưu không bao giờ yêu cầu gửi ngược khay nạp và việc nhóm mèo theo thứ tự đã sắp xếp đảm bảo thời gian chờ đợi tối thiểu trong mỗi nhóm nạp. 
4. Xác định mảng DP`dp[i][k]`là thời gian chờ đợi tối thiểu cho lần đầu tiên`i`mèo sử dụng`k`người cho ăn. Hàm chi phí của một nhóm mèo liên tiếp là tổng chênh lệch giữa thời gian điều chỉnh của mỗi con mèo và thời gian điều chỉnh tối thiểu trong nhóm đó. 
5. Tính toán trước tổng tiền tố của thời gian đã điều chỉnh để tính nhanh tổng thời gian chờ cho bất kỳ khoảng thời gian nào của mèo khi một máng ăn duy nhất phục vụ chúng. 
6. Áp dụng tối ưu hóa chia để trị trên DP: hàm chi phí là hàm lồi, nghĩa là điểm phân chia tối ưu cho`dp[i][k]`nằm bên trái của phép chia tối ưu cho`dp[i+1][k]`. Điều này làm giảm vòng lặp bên trong từ O(m) xuống O(log m) trên mỗi trạng thái DP. 
7. Câu trả lời là`dp[m][p]`, đại diện cho tổng thời gian chờ tối thiểu cho tất cả`m`mèo sử dụng tất cả`p`người cho ăn. 

Bất biến đảm bảo tính chính xác là mỗi người cho ăn phục vụ một nhóm mèo được sắp xếp liền kề và thời gian khởi hành của mỗi nhóm là thời gian được điều chỉnh tối thiểu trong nhóm đó. Bất kỳ nhiệm vụ nào khác đều làm tăng thời gian chờ đợi vì những con mèo ở đầu chuỗi sẽ đợi lâu hơn nếu được xử lý muộn hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m, p = map(int, input().split())
    d = list(map(int, input().split()))
    
    pos = [0] * n
    for i in range(1, n):
        pos[i] = pos[i-1] + d[i-1]
    
    cats = []
    for _ in range(m):
        h, t = map(int, input().split())
        cats.append(t - pos[h-1])
    
    cats.sort()
    
    prefix = [0] * (m+1)
    for i in range(1, m+1):
        prefix[i] = prefix[i-1] + cats[i-1]
    
    INF = 10**18
    dp = [INF] * (m+1)
    dp_prev = [0] + [INF]*m

    def cost(l, r):
        total = prefix[r] - prefix[l-1]
        count = r - l + 1
        return total - cats[l-1]*count

    for k in range(1, p+1):
        def compute(l, r, optl, optr):
            if l > r:
                return
            mid = (l + r) // 2
            best = (INF, -1)
            start = max(optl, 1)
            end = min(mid, optr)
            for j in range(start, end+1):
                val = dp_prev[j-1] + cost(j, mid)
                if val < best[0]:
                    best = (val, j)
            dp[mid] = best[0]
            opt = best[1]
            compute(l, mid-1, optl, opt)
            compute(mid+1, r, opt, optr)
        
        compute(1, m, 1, m)
        dp_prev, dp = dp, [INF]*(m+1)
    
    print(dp_prev[m])

if __name__ == "__main__":
    main()
```Phần đầu tiên tính toán vị trí của các ngọn đồi bằng cách sử dụng tổng tiền tố. Sau đó, thời gian hoàn thành đã điều chỉnh sẽ được tính cho mỗi con mèo. Việc sắp xếp đảm bảo chúng tôi xử lý mèo theo thứ tự không giảm của thời điểm khởi hành máng ăn tối thiểu. Tổng tiền tố cho phép tính toán O(1) thời gian chờ của nhóm. DP thực hiện tối ưu hóa phân chia để chinh phục, trong đó`compute`hàm đệ quy tìm ra sự phân chia tốt nhất cho từng trạng thái DP. 

Các điểm cẩn thận bao gồm việc sử dụng chỉ mục dựa trên 1 cho mèo để khớp với quy ước tổng tiền tố, xử lý các số nguyên lớn với`INF`và giữ các mảng DP riêng biệt ở mỗi lần lặp để tránh ghi đè dữ liệu cần thiết. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 6 2
1 3 5
1 0
2 1
4 9
1 10
2 10
3 12
```| Mèo | Đồi | Kết thúc t | Vị trí | Đã điều chỉnh t | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 0 | 
| 2 | 2 | 1 | 1 | 0 | 
| 3 | 4 | 9 | 9 | 0 | 
| 4 | 1 | 10 | 0 | 10 | 
| 5 | 2 | 10 | 1 | 9 | 
| 6 | 3 | 12 | 4 | 8 | 

Sắp xếp thời gian điều chỉnh: 0, 0, 0, 8, 9, 10 

DP chia hai nguồn cấp dữ liệu: 

- Người cho ăn đầu tiên bắt mèo 1-3 (đã điều chỉnh 0-0), tổng chờ = 0 
- Người cho ăn thứ hai lấy mèo 4-6 (đã điều chỉnh 8-10), tổng chờ = 3 

Tổng cộng = 3 

Điều này phù hợp với đầu ra mẫu. 

### Ví dụ tùy chỉnh```
3 3 2
1 2
1 2
2 5
3 6
```Khoảng cách tiền tố: 0, 1, 3 

Số lần điều chỉnh: 2-0=2, 5-1=4, 6-3=3 → sắp xếp: 2, 3, 4 

Phân chia tối ưu: mèo trung chuyển thứ nhất 1 → chờ 0, mèo trung chuyển thứ hai 2-3 → chờ (4-3)=1 

Tổng số lần chờ = 1 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 

|
