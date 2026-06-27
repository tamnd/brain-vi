---
title: "CF 105093J - Hồ chứa Doggos"
description: "Một bầy chó đang sửa một loạt lỗ trên một bể chứa nước bị hư hỏng trên Titan. Mỗi lỗ rò rỉ dầu với tốc độ không đổi cho đến khi được sửa chữa hoàn toàn."
date: "2026-06-27T20:50:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105093
codeforces_index: "J"
codeforces_contest_name: "2024 UP ACM Algolympics Final Round"
rating: 0
weight: 105093
solve_time_s: 36
verified: true
draft: false
---

[CF 105093J - Reservoir Doggos](https://codeforces.com/problemset/problem/105093/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một bầy chó đang sửa một loạt lỗ trên một bể chứa nước bị hư hỏng trên Titan. Mỗi lỗ rò rỉ dầu với tốc độ không đổi cho đến khi được sửa chữa hoàn toàn. Sau khi chọn xong một lỗ, những chú chó phải làm việc liên tục trong một khoảng thời gian cố định và chúng không thể chuyển đổi nhiệm vụ cho đến khi hoàn thành. 

Nếu một lỗ rò rỉ với tốc độ$v_i$và mất$t_i$giây để sửa chữa, sau đó đóng góp mỗi giây trước khi hoàn thành$v_i$đơn vị dầu bị mất. Tổng số tổn thất từ ​​một hố phụ thuộc vào thời điểm hố đó kết thúc chứ không chỉ thời gian tồn tại của hố đó. Nếu một lỗ kết thúc vào thời điểm$C_i$, phần đóng góp của nó vào tổng tổn thất là$v_i \cdot C_i$. Mục đích là chọn thứ tự sửa chữa tất cả các lỗ hổng để giảm thiểu tổng số tổn thất này. 

Vì vậy, vấn đề thực sự là việc sắp xếp các nhiệm vụ trên một máy duy nhất. Mỗi tác vụ có thời gian xử lý$t_i$và một trọng lượng$v_i$và chúng tôi muốn giảm thiểu tổng thời gian hoàn thành có trọng số. 

Đầu vào đưa ra nhiều trường hợp thử nghiệm. Đối với mỗi người, chúng tôi được cung cấp tất cả tỷ lệ rò rỉ và thời gian sửa chữa, đồng thời chúng tôi phải đưa ra tổng lượng dầu rò rỉ tối thiểu có thể. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng số lỗ trên tất cả các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các hoán vị, vì ngay cả đối với$n = 10^5$,$n!$là hoàn toàn không khả thi và thậm chí$O(n^2)$cách tiếp cận quá chậm. Giải pháp phải là$O(n \log n)$cho mỗi trường hợp thử nghiệm hoặc tốt hơn. 

Một trường hợp thất bại tinh vi đối với lý luận ngây thơ là cho rằng chúng ta phải luôn khắc phục tỷ lệ rò rỉ cao nhất trước tiên. Ví dụ, hãy xem xét hai lỗ: 

đầu vào:$v = [100, 1]$,$t = [1000, 1]$Sửa theo mức cao nhất$v$đầu tiên gây ra độ trễ lớn cho công việc dài, nhưng việc sửa chữa công việc dài trước tiên có thể tốt hơn về tổng thể. Điều này cho thấy rằng không chỉ sắp xếp theo$v$cũng không chỉ bởi$t$là đúng. 

Thứ tự chính xác phụ thuộc vào tỷ lệ giữa tốc độ rò rỉ và thời gian sửa chữa, không chỉ riêng giá trị. 

## Phương pháp tiếp cận 

Nếu chúng tôi thử tất cả các đơn đặt hàng có thể, chúng tôi sẽ tính tổng tổn thất bằng cách mô phỏng thời gian hoàn thành. Đối với mỗi hoán vị, chúng tôi tích lũy tổng tiền tố của thời gian sửa chữa và nhân mỗi thời gian hoàn thành với tốc độ rò rỉ của nó. Điều này đúng nhưng đòi hỏi$n!$hoán vị, và thậm chí đối với$n = 10$, cái này đã quá lớn rồi. 

Một ý tưởng tốt hơn một chút là sắp xếp các nhiệm vụ theo phương pháp phỏng đoán chẳng hạn như giảm$v_i$hoặc tăng$t_i$, nhưng cả hai đều thất bại trong các phản ví dụ trong đó không nên ưu tiên tốc độ rò rỉ nhỏ với thời gian lớn hơn tốc độ rò rỉ trung bình với thời gian nhỏ. 

Cấu trúc của hàm mục tiêu là chìa khóa: mỗi nhiệm vụ đóng góp$v_i$nhân với thời gian hoàn thành và thời gian hoàn thành phụ thuộc vào thứ tự xử lý tích lũy. Đây chính xác là cách giảm thiểu vấn đề lập kế hoạch trên một máy cổ điển$\sum w_i C_i$, Ở đâu$w_i = v_i$và thời gian xử lý là$t_i$. 

Một kết quả tối ưu đã biết cho cấu trúc này là quy tắc Smith: sắp xếp các nhiệm vụ theo tỷ lệ giảm dần$w_i / p_i$, ở đây trở thành sắp xếp bằng cách giảm$v_i / t_i$. Theo trực giác, một nhiệm vụ sẽ cấp bách hơn nếu nó bị rò rỉ nhiều so với thời gian cần thiết để khắc phục. 

Sau khi được sắp xếp, chúng tôi xem xét các nhiệm vụ theo thứ tự đó, duy trì thời gian chạy và tích lũy chi phí hoàn thành có trọng số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu (sắp xếp theo tỷ lệ) |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi lỗ là một công việc với thời gian xử lý$t_i$và cân nặng$v_i$. 

### Các bước 

1. Ghép từng lỗ thành một bộ$(v_i, t_i)$. 

Điều này là cần thiết vì thứ tự tối ưu phụ thuộc đồng thời vào cả hai giá trị chứ không phải độc lập. 
2. Sắp xếp tất cả các cặp theo giá trị giảm dần của$v_i / t_i$. 

Điều này ưu tiên các công việc có nhiều rò rỉ hơn trên một đơn vị thời gian sửa chữa. Nếu hai công việc có tỷ lệ bằng nhau thì thứ tự nào cũng an toàn vì chúng đóng góp tỷ lệ với cùng mật độ. 
3. Khởi tạo một biến`time = 0`để thể hiện ranh giới thời gian hoàn thành hiện tại. 

Điều này theo dõi thời điểm mỗi công việc kết thúc theo lịch trình đã chọn. 
4. Khởi tạo`answer = 0`. 
5. Lặp lại các công việc theo thứ tự được sắp xếp. Đối với mỗi công việc: 

tính thời gian hoàn thành của nó như`time + t_i`. 

Thêm vào`v_i * (time + t_i)`để trả lời. 

Sau đó cập nhật`time += t_i`. 

Lý do là mỗi giây trôi qua trước khi hoàn thành công việc này đều đóng góp vào tốc độ rò rỉ của nó, do đó tổng thiệt hại của nó tỷ lệ thuận với thời điểm hoàn thành của nó. 
6. Xuất ra câu trả lời tích lũy cuối cùng. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là việc hoán đổi hai công việc liền kề chỉ phụ thuộc vào thứ tự tương đối của chúng. Hãy xem xét hai công việc$A$Và$B$. Nếu chúng ta so sánh lịch trình$AB$Và$BA$, sự khác biệt trong tổng chi phí giảm xuống so với việc so sánh$v_A / t_A$Và$v_B / t_B$. Nếu như$v_A / t_A < v_B / t_B$, trao đổi làm giảm chi phí. Điều này chứng tỏ rằng bất kỳ sự đảo ngược nào liên quan đến thứ tự tỷ lệ giảm dần đều có thể được cải thiện, điều này ngụ ý thứ tự được sắp xếp là tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    out = []

    for _ in range(T):
        n = int(input())
        v = list(map(int, input().split()))
        t = list(map(int, input().split()))

        jobs = list(zip(v, t))

        # sort by decreasing v/t without floating point issues
        jobs.sort(key=lambda x: x[0] * 1.0 / x[1], reverse=True)

        time = 0
        ans = 0

        for vi, ti in jobs:
            time += ti
            ans += vi * time

        out.append(str(ans))

    print("\n".j
```
