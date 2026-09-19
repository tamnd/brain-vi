---
title: "CF 104761C - \u0414\u0435\u043b\u0438\u043c\u043e\u0441\u0442\u044c \u043d\u0430 2023"
description: "Chúng ta có hai chữ số riêng biệt, gọi chúng là $A$ và $B$, mỗi chữ số nằm trong khoảng từ 0 đến 9. Chỉ từ hai chữ số này, chúng ta được phép tạo bất kỳ số nguyên dương nào bằng cách ghép chúng theo bất kỳ thứ tự và độ dài nào, miễn là chúng ta không sử dụng bất kỳ chữ số nào khác."
date: "2026-06-29T02:23:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104761
codeforces_index: "C"
codeforces_contest_name: "2023-2024 ICPC NERC (NEERC), Kyrgyzstan Regional Contest"
rating: 0
weight: 104761
solve_time_s: 66
verified: false
draft: false
---

[CF 104761C - \u0414\u0435\u043b\u0438\u043c\u043e\u0441\u0442\u044c \u043d\u0430 2023](https://codeforces.com/problemset/problem/104761/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chữ số riêng biệt, gọi chúng là$A$Và$B$, mỗi số nằm trong khoảng từ 0 đến 9. Chỉ từ hai chữ số này, chúng ta được phép xây dựng bất kỳ số nguyên dương nào bằng cách ghép chúng theo bất kỳ thứ tự và độ dài nào, miễn là chúng ta không sử dụng bất kỳ chữ số nào khác. Nhiệm vụ là xuất ra bất kỳ số nào chia hết cho năm 2023 và nó cũng phải đáp ứng giới hạn kích thước tối đa là 100 chữ số và không được bắt đầu bằng 0. 

Khó khăn cốt lõi không phải là xây dựng các số từ các chữ số mà là tìm ra một số đạt được điều kiện chia hết cụ thể trong một không gian tìm kiếm rất lớn. Ngay cả khi chúng tôi cố định độ dài, vẫn có$2^n$các dây có thể có, do đó, lực mạnh lên tất cả các dây là không thể vượt quá độ dài rất nhỏ. 

Ràng buộc cấu trúc quan trọng là mô đun mục tiêu cố định và nhỏ: 2023. Điều này ngay lập tức gợi ý rằng chúng ta nên nghĩ theo phần dư modulo 2023, vì bất kỳ số nào dài hơn 2023+1 chữ số cuối cùng đều phải lặp lại mẫu số dư. 

Một trường hợp đơn giản có thể dễ dàng đánh lừa việc triển khai là khi một trong các chữ số bằng 0. Ví dụ: nếu$A = 0$Và$B = 7$, một cách xây dựng bất cẩn có thể cố gắng bắt đầu bằng 0, tạo ra các số 0 đứng đầu không hợp lệ như 0077..., không được phép mặc dù hợp lệ về mặt số. Một trường hợp tinh vi khác là khi một chữ số bằng 0 và chữ số kia nhỏ; các công trình tham lam như lặp lại chữ số khác 0 có thể không bao giờ đạt được bội số của năm 2023 mặc dù tồn tại một mẫu hỗn hợp hợp lệ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là tạo ra tất cả các chuỗi trên bảng chữ cái$\{A, B\}$trong việc tăng độ dài và kiểm tra khả năng chia hết của từng chuỗi vào năm 2023. Đối với mỗi chuỗi được tạo, chúng tôi tính toán giá trị modulo 2023 của nó và kiểm tra xem nó có bằng 0 hay không. Điều này đúng vì nó làm kiệt sức tất cả các ứng cử viên. 

Tuy nhiên, số lượng ứng viên tăng theo cấp số nhân theo độ dài. Với chiều dài 50, chúng ta đã có$2^{50}$khả năng vượt xa mọi tính toán khả thi. Ngay cả việc tính toán modulo tăng dần cũng không giúp ích được gì nếu chúng ta vẫn liệt kê tất cả các chuỗi. 

Quan sát quan trọng là chúng ta không quan tâm đến số thực, chỉ quan tâm đến phần dư của nó theo modulo 2023. Mỗi khi chúng ta nối thêm một chữ số, số dư mới chỉ phụ thuộc vào số dư trước đó và chữ số được thêm vào. Điều này chuyển vấn đề thành một vấn đề đồ thị trên các trạng thái$0 \ldots 2022$, trong đó mỗi trạng thái biểu thị phần còn lại và các chuyển đổi tương ứng với việc nối thêm một trong hai chữ số$A$hoặc chữ số$B$. 

Điều này đưa ra một biểu đồ có hướng có tối đa 2023 nút, mỗi nút có hai cạnh hướng ra ngoài. Chúng tôi muốn tìm bất kỳ đường dẫn nào bắt đầu từ một chữ số ban đầu hợp lệ (chữ số đầu khác 0) đạt đến phần còn lại 0. Vì biểu đồ là hữu hạn, BFS đảm bảo chúng tôi sẽ tìm ra giải pháp hoặc cạn kiệt tất cả các trạng thái. Vì giải pháp được đảm bảo tồn tại trong không gian xây dựng dự kiến ​​nên BFS sẽ tìm ra giải pháp một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| BFS trên phần còn lại |$O(2023)$|$O(2023)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lập mô hình mỗi trạng thái dưới dạng một cặp bao gồm phần dư modulo 2023 và chuỗi được xây dựng cho đến nay. Thay vì lưu trữ chuỗi đầy đủ cho mọi trạng thái, chúng tôi lưu trữ các con trỏ gốc để xây dựng lại câu trả lời ở cuối. 

1. Chúng tôi khởi tạo hàng đợi BFS với tất cả các chữ số bắt đầu hợp lệ. Nếu một chữ số bằng 0 thì nó không thể được sử dụng làm ký tự đầu tiên, vì vậy chúng ta chỉ bắt đầu từ các chữ số khác 0. Mỗi chữ số bắt đầu đóng góp phần dư ban đầu bằng chữ số đó modulo 2023. Điều này đảm bảo chúng tôi không bao giờ tạo ra các số 0 đứng đầu không hợp lệ. 
2. Đối với mỗi trạng thái trong hàng đợi, chúng tôi xem xét việc thêm chữ số$A$và chữ số$B$. Nếu số dư hiện tại là$r$, thì phần dư mới sau khi thêm chữ số$d$là$(10r + d) \bmod 2023$. Sự lặp lại này ghi lại chính xác số thập phân phát triển như thế nào. 
3. Nếu đạt đến số dư 0 tại bất kỳ điểm nào, chúng ta dừng ngay lập tức. Đường dẫn từ trạng thái bắt đầu đến trạng thái này xác định một số hợp lệ chia hết cho 2023. 
4. Để xây dựng lại số, chúng ta đi theo các con trỏ cha được lưu trữ ngược từ trạng thái 0 cho đến khi đạt đến điểm bắt đầu. Sau đó chúng tôi đảo ngược chuỗi các chữ số. 
5. Chúng tôi xuất chuỗi được xây dựng lại. 

Lựa chọn thiết kế chính là chỉ lưu trữ các chuỗi trước thay vì chuỗi đầy đủ, giúp giữ giới hạn bộ nhớ và cho phép tái thiết hiệu quả. 

### Tại sao nó hoạt động 

Mỗi trạng thái BFS biểu thị chính xác một phần còn lại có thể truy cập được bằng một chuỗi chữ số cụ thể. Mỗi bước mở rộng đều đảm bảo tính chính xác của phép tính phần dư vì phép nối số thập phân được mô hình hóa một cách trung thực bởi$10r + d$. BFS khám phá các trạng thái theo số chữ số tăng dần, vì vậy lần đầu tiên chúng ta đạt được số dư 0 tương ứng với một cấu trúc hợp lệ. Vì chỉ có 2023 phần dư khả dĩ, nên nếu một giải pháp tồn tại trong bảng chữ cái chữ số cho phép, BFS sẽ gặp giải pháp đó mà không cần phải khám phá rõ ràng các chuỗi có độ dài hàm mũ. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

MOD = 2023

def solve():
    A, B = map(int, input().split())
    digits = [A, B]

    # store visited remainders
    prev = [-1] * MOD
    prev_digit = [-1] * MOD
    visited = [False] * MOD

    q = deque()

    # initialize with valid starting digits (no leading zero)
    for d in digits:
        if d == 0:
            continue
        r = d % MOD
        if not visited[r]:
            visited[r] = True
            prev[r] = -2  # start marker
            prev_digit[r] = d
            q.append(r)

    # BFS over remainder states
    while q:
        r = q.popleft()

        if r == 0:
            break

        for d in digits:
            nr = (r * 10 + d) % MOD
            if not visited[nr]:
                visited[nr] = True
                prev[nr] = r
                prev_digit[nr] = d
                q.append(nr)

    if not visited[0]:
        return ""

    # reconstruct answer
    res = []
    cur = 0
    while prev[cur] != -2:
        res.append(str(prev_digit[cur]))
        cur = prev[cur]

    res.append(str(prev_digit[cur]))
    return "".join(reversed(res))

if __name__ == "__main__":
    print(solve())
```BFS được triển khai hoàn toàn trên phần dư, giúp giữ cho không gian trạng thái nhỏ. Các mảng`prev`Và`prev_digit`cho phép xây dựng lại mà không lưu trữ chuỗi đầy đủ. Điểm đánh dấu`-2`phân biệt trạng thái gốc với trạng thái trung gian. 

Một điểm triển khai tinh tế là initi
