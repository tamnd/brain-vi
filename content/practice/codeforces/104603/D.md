---
title: "CF 104603D - Bài toán phân công"
description: "Chúng ta được cung cấp một cấu trúc cố định cho một cuộc thi: mỗi cuộc thi có mức độ khó $K$, và cấp độ $i$ yêu cầu chính xác các vấn đề về $Ci$. Các bài toán được phân loại theo mức sử dụng tối thiểu: một bài toán cấp $d$ chỉ có thể được gán cho các cấp $d, d+1, dots, K$."
date: "2026-06-30T02:53:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104603
codeforces_index: "D"
codeforces_contest_name: "2023 Argentinian Programming Tournament (TAP)"
rating: 0
weight: 104603
solve_time_s: 35
verified: true
draft: false
---

[CF 104603D - Vấn đề về gán](https://codeforces.com/problemset/problem/104603/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một cấu trúc cố định cho một cuộc thi: mỗi cuộc thi có$K$mức độ khó khăn và mức độ$i$yêu cầu chính xác$C_i$vấn đề. Các vấn đề được phân loại theo mức độ sử dụng tối thiểu: vấn đề về cấp độ$d$chỉ có thể được chỉ định cho các cấp độ$d, d+1, \dots, K$. Mỗi vấn đề chỉ có thể được sử dụng tối đa một lần và trong một cuộc thi, nó chỉ có thể chiếm một cấp độ. 

Chúng tôi cũng có nguồn cung cấp$P_i$các vấn đề về lớp$i$. Nhiệm vụ là tối đa hóa số lượng cuộc thi hoàn chỉnh có thể được hình thành, trong đó mỗi cuộc thi phải được điền đầy đủ theo yêu cầu.$C_i$yêu cầu, sử dụng các bài toán có sẵn mà không cần sử dụng lại. 

Khó khăn cốt lõi là các bài toán cấp cao hơn rất linh hoạt và có thể phục vụ cấp độ cao hơn, trong khi các bài toán cấp thấp bị hạn chế. Điều này tạo ra sự phụ thuộc giống như dòng chảy giữa các cấp độ, trong đó các quyết định sớm ảnh hưởng đến tính khả thi sau này. 

Các ràng buộc đạt tới$K \le 10^5$và giá trị lên đến$10^9$, điều này ngay lập tức loại trừ mọi mô phỏng cho mỗi cuộc thi. Ngay cả việc cố gắng xây dựng từng cuộc thi một cách tham lam cũng sẽ quá chậm vì số lượng cuộc thi có thể xảy ra cũng có thể rất lớn. 

Một trường hợp thất bại tinh vi đối với bài tập tham lam ngây thơ xuất hiện khi chúng ta giao bài tập cấp thấp một cách quá háo hức. 

Ví dụ:```
K = 2
C = [1, 1]
P = [1, 1]
```Một chiến lược bất cẩn có thể gán bài toán cấp 1 lên cấp 1 và bài toán cấp 2 lên cấp 2, tạo ra 1 cuộc thi, điều này đúng. Nhưng nếu chúng ta mở rộng quy mô này:```
K = 2
C = [1, 1]
P = [100, 1]
```Nếu chúng ta tham lam sử dụng các bài toán cấp 1 ở cấp độ 1 và bỏ qua tính linh hoạt của chúng, chúng ta vẫn có thể nhận được câu trả lời đúng, nhưng trong các phân phối phức tạp hơn, điều này dẫn đến tình trạng đói khát ở các cấp độ cao hơn vì các vấn đề cấp độ thấp phải được bảo toàn cho các cấp độ hạn chế nhất có thể sử dụng chúng. 

Khó khăn chính là đảm bảo rằng các bài toán cấp cao linh hoạt luôn sẵn sàng để lấp đầy những khoảng trống do sự thiếu hụt ở các cấp thấp hơn tạo ra. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là cố gắng xây dựng từng cuộc thi một. Đối với mỗi cuộc thi, chúng tôi cố gắng hoàn thành cấp độ 1 đến cấp độ$K$, luôn chọn một bài toán khả thi có sẵn với cấp độ nhỏ nhất mà vẫn cho phép phân công. Điều này đúng vì nó tôn trọng các ràng buộc một cách trực tiếp, nhưng nó không khả thi về mặt tính toán. 

Mỗi cuộc thi yêu cầu quét nhiều cấp độ và mỗi cấp độ có thể yêu cầu tìm kiếm các cấp độ vấn đề có thể sử dụng được. Trong trường hợp xấu nhất, có tới$10^5$mức độ và khả năng$10^9$các cuộc thi, cách tiếp cận này trở nên không thể. 

Cái nhìn sâu sắc về cấu trúc nhằm lật ngược quan điểm: thay vì xây dựng các cuộc thi, chúng tôi kiểm tra tính khả thi của một con số nhất định$x$. Nếu chúng ta muốn xây dựng$x$cuộc thi, sau đó cấp$i$yêu cầu$x \cdot C_i$vấn đề được giao cho cấp độ$i$, và những điều này phải đến từ tất cả các cấp độ bài toán$d \le i$. 

Điều này biến vấn đề thành một cuộc kiểm tra tính khả thi dưới các ràng buộc tích lũy. Chúng tôi đang phân phối nguồn cung một cách hiệu quả với giới hạn thấp hơn cho các vị trí có thể sử dụng, điều này gợi ý một cách tự nhiên mức độ xử lý theo thứ tự tăng dần và duy trì mức dư thừa công suất linh hoạt cấp cao hơn. 

Quan sát quan trọng là các vấn đề ở cấp độ cao hơn luôn có thể bị “trì hoãn” đi xuống, nhưng những vấn đề ở cấp độ thấp hơn thì không thể di chuyển lên trên. Vì vậy, chúng tôi xử lý từ thấp đến cao, theo dõi xem còn lại bao nhiêu vấn đề có thể sử dụng được sau khi đáp ứng từng cấp độ. 

Sau đó chúng tôi tìm kiếm nhị phân khả thi tối đa$x$, vì tính khả thi là đơn điệu: nếu chúng ta có thể xây dựng$x$các cuộc thi, chúng tôi cũng có thể xây dựng bất kỳ$x' < x$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tham lam mỗi cuộc thi mô phỏng |$O(xK)$|$O(K)$| Quá chậm | 
| Tìm kiếm nhị phân + tính khả thi tham lam |$O(K \log \max P)$|$O(K)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Kiểm tra tính khả thi của một số cuộc thi cố định$x$1. Yêu cầu về thang đo sao cho mỗi cấp độ$i$nhu cầu$need_i = x \cdot C_i$vấn đề. Điều này thể hiện tổng nhu cầu mà chúng ta phải đáp ứng. 
2. Đi qua các cấp độ từ$1$ĐẾN$K$, duy trì một nhóm các vấn đề sẵn có có thể được sử dụng cho cấp độ hiện tại hoặc cao hơn. 
3. Ở cấp độ$i$, cộng tất cả các bài toán về điểm$i$vào nhóm, vì hiện tại chúng đã có sẵn cho cấp độ$i$và ở trên. 
4. Cố gắng thỏa mãn$need_i$sử dụng hồ bơi hiện tại. Nếu nhóm không đủ, hãy trả về sai ngay lập tức vì các cấp độ sau không thể bù đắp cho sự thiếu hụt ở đây. 
5. Sau khi hoàn thành cấp độ$i$, chuyển tiếp bất kỳ nhóm chưa sử dụng nào sang cấp độ tiếp theo. 

Điều này có hiệu quả vì các cấp độ thấp hơn có yêu cầu khắt khe hơn: nếu chúng tôi không thể đáp ứng được cấp độ$i$, không có sự phân phối lại từ cấp cao hơn có thể khắc phục được. 

### Tìm kiếm nhị phân qua câu trả lời 

1. Xác định hàm`ok(x)`kiểm tra tính khả thi bằng cách sử dụng quy trình trên. 
2. Tìm kiếm nhị phân$x$từ 0 đến giới hạn trên lớn. Giới hạn tự nhiên là$\sum P_i / \min C_i$, nhưng trong thực tế$10^9$giới hạn tỷ lệ là an toàn với số học dài. 
3. Di chuyển phạm vi tìm kiếm tùy theo tính khả thi. 

### Tại sao nó hoạt động 

Kiểm tra tính khả thi thực thi điều kiện nhất quán tiền tố: ở mọi cấp độ$i$, tổng nguồn cung có thể sử dụng tính đến cấp$i$phải đủ cho tổng nhu cầu lên đến mức$i$. Vì các bài toán cấp cao hơn chỉ có thể di chuyển xuống dưới nên mọi phép gán hợp lệ đều phải đáp ứng các ràng buộc tiền tố này. 

Điều này làm cho việc tích lũy tham lam từ thấp đến cao vừa cần thiết vừa đủ: nó phù hợp với hướng duy nhất tồn tại tính linh hoạt, do đó, không có sự phân công lại nào trong tương lai có thể sửa chữa tiền tố bị vi phạm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(x, K, C, P):
    # current available pool of usable problems
    pool = 0
    
    for i in range(K):
        pool += P[i]
        need = x * C[i]
        
        if pool < need:
            return False
        
        pool -= need
    
    return True

def main():
    K = int(input())
    C = list(map(int, input().split()))
    P = list(map(int, input().split()))
    
    lo, hi = 0, 10**18
    ans = 0
    
    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid, K, C, P):
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1
    
    print(ans)

if __name__ == "__main__":
    main()
```Giải pháp được xây dựng xung quanh một hàm khả thi duy nhất có thể xử lý tích lũy tất cả các cấp độ vấn đề. Chi tiết triển khai chính là việc chạy`pool`, đại diện cho tất cả các vấn đề có sẵn cho đến ranh giới lớp hiện tại. 

Bước trừ`pool -= need`là rất quan trọng: nó buộc mỗi vấn đề được sử dụng chính xác một lần. Nếu bước này bị bỏ qua, chúng tôi sẽ sử dụng lại dung lượng ở các cấp một cách không chính xác. 

Tìm kiếm nhị phân được áp dụng vì tính khả thi là đơn điệu trong$x$. Khi một số lượng cuộc thi nhất định không thể được hình thành, bất kỳ số lượng lớn hơn nào cũng sẽ thất bại do nhu cầu mở rộng tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
K = 2
C = [1, 1]
P = [2, 2]
```| tôi | hồ bơi trước | thêm P[i] | cần | bơi sau | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 2 | 1 | 1 | 
| 2 | 1 | 4 | 1 | 3 | 

Vì$x = 2$, mỗi cấp cần 2 đơn vị và quá trình vẫn thành công với dung lượng còn sót lại. Đang cố gắng$x = 3$thất bại vì cầu vượt quá cung tiền tố. 

Điều này chứng tỏ cách tích lũy tiền tố nắm bắt tính khả thi một cách chính xác. 

### Ví dụ 2```
K = 3
C = [2, 1, 1]
P = [2, 1, 3]
```Kiểm tra$x = 1$: 

| tôi | hồ bơi trước | quảng cáo
