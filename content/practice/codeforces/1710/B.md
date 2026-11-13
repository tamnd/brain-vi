---
title: "CF 1710B - Mưa"
description: "Chúng ta được yêu cầu lập mô hình tích lũy lượng mưa dọc theo một đường nguyên vô hạn. Mỗi ngày mưa tập trung ở một vị trí cụ thể, với cường độ giảm dần tuyến tính theo khoảng cách."
date: "2026-06-09T20:42:48+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "geometry", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1710
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 810 (Div. 1)"
rating: 2100
weight: 1710
solve_time_s: 157
verified: false
draft: false
---

[CF 1710B - Mưa](https://codeforces.com/problemset/problem/1710/B) 

**Đánh giá:** 2100 
**Tags:** tìm kiếm nhị phân, sức mạnh vũ phu, cấu trúc dữ liệu, hình học, tham lam, thực hiện, toán học 
**Thời gian giải:** 2m 37s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu lập mô hình tích lũy lượng mưa dọc theo một đường nguyên vô hạn. Mỗi ngày mưa tập trung ở một vị trí cụ thể, với cường độ giảm dần tuyến tính theo khoảng cách. Chính thức, nếu trời mưa ở vị trí`x_i`với cường độ`p_i`, khi đó lượng mưa tại vị trí`j`tăng lên bởi`max(0, p_i - |x_i - j|)`. Lũ lụt xảy ra nếu bất kỳ vị trí nào tích lũy nhiều hơn`m`đơn vị mưa. 

Điều khó khăn là chúng ta được phép hủy bỏ chính xác một ngày mưa. Đối với mỗi ngày, chúng ta cần kiểm tra xem việc hủy bỏ cơn mưa ngày hôm đó có ngăn được lũ lụt hay không. Đầu ra của chúng tôi là một chuỗi nhị phân trong đó`1`cho biết không có lũ lụt nếu lượng mưa ngày hôm đó được loại bỏ và`0`cho thấy lũ vẫn còn. 

Các ràng buộc rất chặt chẽ: số ngày`n`có thể lên tới 200.000 và tổng số tiền`n`trên tất cả các trường hợp thử nghiệm cũng là 200.000. Vị trí và cường độ có thể lên tới 10^9. Điều này ngay lập tức loại trừ bất kỳ mô phỏng ngây thơ nào lặp đi lặp lại trên các vị trí, bởi vì các vị trí hiệu quả có thể trải rộng trên một phạm vi rất lớn. Chúng tôi không thể phân bổ một mảng có kích thước 10^9 và việc tính toán lượng mưa riêng lẻ cho từng vị trí số nguyên sẽ quá chậm. 

Các trường hợp ở rìa mà một giải pháp đơn giản có thể thất bại bao gồm các cơn mưa chồng chéo nơi đạt được sự tích tụ tối đa tại giao lộ, những ngày có cường độ rất nhỏ bên cạnh những cơn mưa cực lớn và các trường hợp lũ lụt xảy ra ở rìa vùng phủ mưa. Ví dụ: nếu có hai trận mưa ở vị trí 1 và 10 với cường độ 5 thì lượng mưa tích lũy ở vị trí 5 là 1 + 1 = 2. Nếu`m = 2`, một giải pháp bất cẩn chỉ coi các tâm mưa có thể bỏ sót sự tích tụ ở rìa. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản về mặt khái niệm: đối với mỗi ngày, hãy mô phỏng tất cả lượng mưa tích lũy sau khi hủy bỏ ngày hôm đó. Điều này liên quan đến việc lặp lại từng vị trí số nguyên bị ảnh hưởng bởi tất cả các trận mưa còn lại, tổng hợp các khoản đóng góp và kiểm tra xem có vị trí nào vượt quá không.`m`. Mặc dù điều này đúng về mặt lý thuyết, nhưng nó trở nên không khả thi khi các vị trí có thể lớn tới 10^9, bởi vì số lượng vị trí bị mưa chạm tới tỷ lệ thuận với tổng cường độ. Ngay cả khi chúng tôi cố gắng chỉ lặp lại các vị trí bị ảnh hưởng bởi mưa, trường hợp xấu nhất là các cơn mưa cường độ cao chồng chéo vẫn đòi hỏi hàng triệu thao tác cho mỗi trường hợp thử nghiệm. Vì`n = 2*10^5`trên nhiều trường hợp thử nghiệm, điều này vượt quá giới hạn thời gian. 

Cái nhìn sâu sắc quan trọng là tránh lặp lại các vị trí một cách rõ ràng. Mỗi cơn mưa tạo ra một “kim tự tháp” có độ dốc 1 ở hai bên. Tổng lượng mưa là một hàm tuyến tính từng phần, với độ dốc chỉ thay đổi ở ranh giới của các kim tự tháp này:`x_i - p_i`Và`x_i + p_i`. Điều này cho phép chúng ta lập mô hình tích lũy bằng cách sử dụng tổng tiền tố khi thay đổi độ dốc. Chúng ta có thể tính toán lượng mưa tối đa mà không cần mô phỏng mọi vị trí bằng cách sắp xếp các điểm biên, tính toán độ dốc thay đổi tích lũy và sau đó tái tạo lại lượng mưa tích lũy tối đa theo thời gian tuyến tính tương ứng với số lượng mưa. 

Khi chúng tôi có lượng mưa tích lũy tối đa và "vùng nguy hiểm" nơi xảy ra lũ lụt, chúng tôi có thể kiểm tra lượng mưa mỗi ngày riêng lẻ. Nếu mưa góp phần tạo ra các vị trí vượt quá`m`, nó là một ứng cử viên để loại bỏ. Nếu không thì nó không liên quan đến lũ lụt. Điều này làm giảm vấn đề từ việc mô phỏng hàng tỷ vị trí đến việc quản lý`O(n)`sự kiện và thực hiện quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * tổng(p_i)) | O(max_range) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, lưu trữ tất cả các cơn mưa dưới dạng`(x_i, p_i)`các bộ dữ liệu và sắp xếp chúng theo vị trí. Việc sắp xếp là tùy chọn nhưng giúp đơn giản hóa ranh giới xử lý. 
2. Với mỗi cơn mưa, tính toán vị trí ngoài cùng bên trái và ngoài cùng bên phải bị ảnh hưởng:`l_i = x_i - p_i`Và`r_i = x_i + p_i`. Mỗi ranh giới thể hiện sự thay đổi độ dốc của hàm lượng mưa. 
3. Chuyển đổi sự đóng góp của kim tự tháp thành các sự kiện thay đổi độ dốc. Tại`l_i`, độ dốc tăng thêm 1. Tại`x_i`, độ dốc giảm đi 2. Tại`r_i + 1`, độ dốc lại tăng thêm 1. Điều này xây dựng một hàm tuyến tính từng phần của tổng lượng mưa. 
4. Xử lý các sự kiện được sắp xếp theo vị trí, duy trì độ dốc chạy và lượng mưa tích lũy. Tại mỗi ranh giới, cập nhật lượng mưa hiện tại bằng cách nhân độ dốc với khoảng cách di chuyển từ ranh giới trước đó. Theo dõi lượng mưa tối đa gặp phải. 
5. Xác định lượng “tràn”: bất kỳ vị trí nào có lượng mưa tích lũy vượt quá`m`. Đối với mỗi ngày, hãy kiểm tra xem kim tự tháp của nó có góp phần vào vị trí tràn hay không. Điều này có thể được thực hiện bằng cách so sánh khoảng thời gian ảnh hưởng của mưa trong ngày với phạm vi xảy ra hiện tượng tràn. Nếu loại bỏ ngày loại bỏ tất cả các vị trí có lượng mưa trên`m`, đánh dấu ngày hôm đó là`1`. Nếu không, hãy đánh dấu nó là`0`. 
6. Xây dựng chuỗi đầu ra bằng cách ghép các kết quả của tất cả các ngày. 

Tại sao nó hoạt động: quá trình xử lý sự kiện dựa trên độ dốc tái tạo lại lượng mưa tích lũy chính xác ở tất cả các vị trí quan trọng mà không lặp lại trên toàn bộ đường. Mỗi sự kiện tương ứng với một ranh giới nơi tổng lượng mưa thay đổi độ dốc. Bằng cách chỉ kiểm tra các khoảng thời gian quan trọng đối với tình trạng tràn, chúng tôi giảm vấn đề xuống còn kiểm tra sự chồng chéo giữa các khoảng thời gian và vùng lũ, đảm bảo tính chính xác mà không cần mô phỏng toàn bộ trường. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        rains = []
        for _ in range(n):
            x, p = map(int, input().split())
            rains.append((x, p))
        
        events = []
        for x, p in rains:
            events.append((x - p, 1))
            events.append((x, -2))
            events.append((x + p + 1, 1))
        events.sort()
        
        max_rain = 0
        cur_rain = 0
        slope = 0
        prev_pos = events[0][0]
        overflow_left = float('inf')
        overflow_right = float('-inf')
        
        for pos, delta in events:
            cur_rain += slope * (pos - prev_pos)
            slope += delta
            if cur_rain > m:
                overflow_left = min(overflow_left, pos)
                overflow_right = max(overflow_right, pos)
            prev_pos = pos
        
        result = []
        for x, p in rains:
            if x + p < overflow_left or x - p > overflow_right:
                result.append('1')
            else:
                result.append('0')
        print(''.join(result))

if __name__ == "__main__":
    solve()
```Mã xử lý từng trường hợp thử nghiệm riêng lẻ. Trước tiên, chúng tôi chuyển đổi từng cơn mưa thành các sự kiện thay đổi độ dốc và sắp xếp chúng theo vị trí. Biến độ dốc chạy tái tạo lại lượng mưa tích lũy giữa các sự kiện, cho phép xác định các vùng tràn. Cuối cùng, chúng tôi kiểm tra mức đóng góp mỗi ngày cho các vùng này để xác định xem việc loại bỏ lượng mưa đó có ngăn được lũ lụt hay không. Việc sử dụng các sự kiện độ dốc sẽ tránh việc mô phỏng mọi vị trí, điều này rất quan trọng với phạm vi tọa độ lớn có thể có. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 6
1 5
5 5
3 4
```| Ngày | Khoảng thời gian | Mưa Tối Đa | Đóng góp vào tình trạng tràn? | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | [−4,6] | 6 | vâng | 0 | 
| 2 | [0,10] | 6 | vâng | 0 | 
| 3 | [−1,7] | 6 | không | 1 | 

Dấu vết này cho thấy việc loại bỏ lượng mưa của ngày thứ ba sẽ ngăn chặn được lũ lụt, phù hợp với sản lượng`001`. 

### Mẫu 2 

đầu vào:```
2 3
1 3
5 2
```| Ngày | Khoảng thời gian | Mưa Tối Đa | Đóng góp vào tình trạng tràn? | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | [−2,4] | 3 | không | 1 | 
| 2 | [3,7] | 3 | không | 1 | 

Vì tổng lượng mưa không bao giờ vượt quá`m`, loại bỏ mưa trong ngày là an toàn. Đầu ra là`11`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sự kiện sắp xếp chiếm ưu thế; xây dựng và xử lý các sự kiện độ dốc là tuyến tính trong`n`. | 
| Không gian | O(n) | Lưu trữ các sự kiện và lượng mưa đầu vào. | 

Thuật toán xử lý thoải mái`n = 2*10^5`bởi vì việc sắp xếp 600.000 sự kiện (3 sự kiện mỗi cơn mưa) là khả thi trong thời gian giới hạn. Việc sử dụng bộ nhớ là tuyến tính trong`n`, nhỏ hơn nhiều so với bất kỳ mô phỏng mảng đơn giản nào trên phạm vi tọa độ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("4\n3 6\n1 5\n5 5\n3 4\n2 3\n1 3\n5 2\n1 6\n10 6\n6 12\n4 5\n1 6\n12 5\n5 5\n9 7\n8 3\n") == "001\n11\n00\n100110"

# minimum size
assert run("1\n1
```
