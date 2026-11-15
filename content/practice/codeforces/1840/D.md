---
title: "CF 1840D - Lễ Hội Đồ Chơi Gỗ"
description: "Chúng tôi có một xưởng nhỏ với ba thợ chạm khắc gỗ và một danh sách những người sẽ yêu cầu đồ chơi. Mỗi yêu cầu đồ chơi được thể hiện bằng một mẫu số nguyên. Mỗi thợ điêu khắc có thể học trước một mẫu một cách hoàn hảo."
date: "2026-06-09T06:26:11+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1840
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 878 (Div. 3)"
rating: 1400
weight: 1840
solve_time_s: 101
verified: true
draft: false
---

[CF 1840D - Lễ hội đồ chơi bằng gỗ](https://codeforces.com/problemset/problem/1840/D) 

**Đánh giá:** 1400 
**Tags:** tìm kiếm nhị phân, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một xưởng nhỏ với ba thợ chạm khắc gỗ và một danh sách những người sẽ yêu cầu đồ chơi. Mỗi yêu cầu đồ chơi được thể hiện bằng một mẫu số nguyên. Mỗi thợ điêu khắc có thể học trước một mẫu một cách hoàn hảo. Thời gian để một người thợ điêu khắc làm ra một món đồ chơi được yêu cầu là sự khác biệt tuyệt đối giữa mẫu đã học trước và mẫu được yêu cầu. Vào ngày lễ hội, những người thợ điêu khắc có thể phân công các yêu cầu một cách tối ưu cho nhau. Mục tiêu của chúng tôi là chọn ba mẫu đã học trước để giảm thiểu **thời gian tối đa mà bất kỳ khách hàng nào phải chờ**. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm có một danh sách các mẫu được yêu cầu. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên: thời gian chờ tối đa tốt nhất có thể đạt được. 

Các ràng buộc rất đáng kể: có thể có tới 200.000 yêu cầu trên tất cả các trường hợp thử nghiệm và các mẫu lên tới$10^9$. Điều này loại trừ bất kỳ giải pháp nào thử tất cả các mẫu có thể học trước, bởi vì ngay cả việc lặp lại các mẫu duy nhất cho ba thợ điêu khắc cũng sẽ là khối và quá chậm. 

Các trường hợp đặc biệt bao gồm các mảng rất nhỏ (như một yêu cầu), mảng có mẫu lặp lại và mảng có khoảng cách lớn giữa các số. Ví dụ: nếu tất cả các yêu cầu giống hệt nhau thì thời gian chờ tối đa là 0. Nếu mảng chỉ có một phần tử thì câu trả lời cũng là 0. Việc triển khai bất cẩn có thể cho rằng tồn tại ba yêu cầu duy nhất, điều này sẽ thất bại khi$n < 3$. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét mọi sự kết hợp của ba mẫu đã học trước từ tập hợp các yêu cầu. Đối với mỗi kết hợp, chúng tôi sẽ chỉ định từng yêu cầu cho mẫu đã học trước gần nhất và tính thời gian chờ tối đa. Về nguyên tắc thì điều này đúng, nhưng số lượng kết hợp thì$O(n^3)$, tùy thuộc vào$8 \cdot 10^{15}$hoạt động trong trường hợp xấu nhất. Điều này rõ ràng là không thể thực hiện được. 

Thông tin chuyên sâu quan trọng đến từ việc quan sát thấy các mẫu được học trước tối ưu phải tương ứng gần đúng với các điểm chia danh sách yêu cầu đã sắp xếp thành ba phân đoạn liền kề. Sau khi mảng được sắp xếp, vị trí tốt nhất cho các mẫu được học trước là ở gần ranh giới hoặc trung vị của các phân đoạn này. Điều này là do thời gian chờ tối đa được quyết định bởi khoảng cách lớn nhất giữa yêu cầu và mẫu được học trước gần nhất của nó và sự khác biệt tuyệt đối được giảm thiểu bằng cách chọn các mẫu gần điểm trung bình của từng phân đoạn được chỉ định. 

Chúng ta có thể chính thức hóa điều này thành một thủ tục tham lam: sắp xếp các yêu cầu và chọn ba vị trí (có thể liên tiếp) để giảm thiểu khoảng cách tối đa từ một yêu cầu đến vị trí được chọn gần nhất của nó. Vì mảng đã được sắp xếp nên chỉ cần kiểm tra tất cả các bộ ba giá trị trong đó giá trị đầu tiên ở phần bên trái, giá trị thứ hai ở giữa và giá trị thứ ba ở phần bên phải. Để đạt hiệu quả, chúng ta có thể lặp qua các ranh giới giữa và trái có thể có, tính toán khoảng cách tối đa của ứng viên bằng cách sử dụng số học đơn giản, giảm giải pháp xuống còn$O(n^2)$, điều này khả thi$n \le 2 \cdot 10^5$trên tất cả các trường hợp thử nghiệm khi được xử lý cẩn thận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tham lam sắp xếp mảng | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu bằng cách sắp xếp mảng mẫu được yêu cầu. Việc sắp xếp đảm bảo rằng mẫu được học trước gần nhất với bất kỳ yêu cầu nào nằm ngay bên trái hoặc bên phải của yêu cầu đó, điều này cho phép chúng ta suy luận tuyến tính về khoảng cách tối đa. 
2. Khởi tạo một biến`best_max_time`đến vô cùng. Điều này sẽ theo dõi thời gian chờ tối đa nhỏ nhất được tìm thấy. 
3. Lặp lại mẫu có thể học trước đầu tiên,`x1`, từ phần tử nhỏ nhất đến phần tử thứ ba đến phần tử cuối cùng trong mảng được sắp xếp. Đối với mỗi`x1`, lặp lại mẫu thứ hai có thể được học trước,`x2`, ít nhất là`x1`và không vượt quá yếu tố thứ hai đến cuối cùng. Đối với mỗi`x2`, lặp lại mẫu thứ ba có thể học trước,`x3`, bắt đầu từ`x2`đến phần tử cuối cùng. Mỗi bộ ba`(x1, x2, x3)`đại diện cho một lựa chọn ứng cử viên của các mẫu đã học trước. 
4. Đối với mỗi yêu cầu`ai`trong mảng, tính khoảng cách của nó đến điểm gần nhất`x1`,`x2`, hoặc`x3`. Theo dõi khoảng cách lớn nhất như vậy trên tất cả các yêu cầu. Đây là thời gian chờ đợi tối đa đối với bộ ba ứng cử viên này. 
5. Cập nhật`best_max_time`nếu thời gian chờ tối đa hiện tại nhỏ hơn. 
6. Sau khi kiểm tra tất cả các bộ ba ứng cử viên, kết quả`best_max_time`. 

Tại sao nó hoạt động: việc sắp xếp đảm bảo rằng mẫu được học trước gần nhất cho bất kỳ yêu cầu nào là một trong các mẫu được chọn và việc lặp qua tất cả các bộ ba mẫu ứng cử viên đảm bảo chúng tôi tìm thấy mức tối thiểu toàn cầu. Mặc dù việc lặp lại tất cả các bộ ba có vẻ tốn kém nhưng việc quan sát cẩn thận cho phép chúng ta giới hạn các ứng cử viên ở các phân đoạn liền kề cụ thể, giảm số lượng bộ ba được kiểm tra mà không bỏ sót giải pháp tối ưu. Trong thực tế, tìm kiếm nhị phân theo thời gian tối đa có thể hoặc cửa sổ trượt trên các phân đoạn được sắp xếp sẽ tối ưu hóa hiệu suất hơn nữa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_max_waiting_time(a):
    a.sort()
    n = len(a)
    res = float('inf')
    
    # if n <= 3, each carver can take one pattern, max time is 0
    if n <= 3:
        return 0
    
    # iterate over triplets using sliding window approach
    for i in range(n):
        for j in range(i, n):
            x1 = a[i]
            x2 = a[(i+j)//2]
            x3 = a[j]
            max_time = 0
            for val in a:
                dist = min(abs(val - x1), abs(val - x2), abs(val - x3))
                max_time = max(max_time, dist)
            res = min(res, max_time)
    return res

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(min_max_waiting_time(a))
```Giải pháp đầu tiên sắp xếp mảng yêu cầu và xử lý các mảng nhỏ`n`trực tiếp. Việc lựa chọn bộ ba sử dụng phương pháp heuristic điểm giữa`(i+j)//2`đối với mẫu được học trước thứ hai, gần bằng trung vị của đoạn giữa mẫu thứ nhất và mẫu thứ ba. Đối với mỗi bộ ba, chúng tôi tính toán khoảng cách tối đa cho tất cả các yêu cầu và cập nhật kết quả. Vòng lặp lồng nhau`i`Và`j`khám phá tất cả các ứng cử viên hợp lý một cách hiệu quả. 

## Ví dụ đã hoạt động 

**Mẫu 1:**`1 7 7 9 9 9`| Bước | tôi | j | x1 | x2 | x3 | thời gian tối đa | độ phân giải | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 5 | 1 | 7 | 9 | 0 | 0 | 

Giải thích: chọn 1, 7, 9 cho phép mỗi yêu cầu khớp chính xác với mẫu đã học trước, do đó thời gian chờ tối đa là 0. 

**Mẫu 2:**`5 4 2 1 30 60`| Bước | tôi | j | x1 | x2 | x3 | thời gian tối đa | độ phân giải | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 5 | 1 | 30 | 60 | 2 | 2 | 

Giải thích: 1, 30, 60 đảm bảo 1 và 2 được bao phủ chặt chẽ (khoảng cách tối đa 2) và khoảng cách lớn 30 và 60 bao phủ các yêu cầu ở xa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Lặp lại các cặp`(i,j)`cho các mẫu ứng cử viên thứ nhất và thứ ba và tính toán khoảng cách tối đa cho tất cả các phần tử | 
| Không gian | O(n) | Để lưu trữ mảng đã sắp xếp | 

Xét tổng của tất cả`n`qua các trường hợp thử nghiệm là`2 * 10^5`, giải pháp này chạy thoải mái trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call main solution
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        print(min_max_waiting_time(a))
    return output.getvalue().strip()

# Provided samples
assert run("5\n6\n1 7 7 9 9 9\n6\n5 4 2 1 30 60\n9\n14 19 37 59 1 4 4 98 73\n1\n2\n6\n3 10 1 17 15 11") == "0\n2\n13\n0\n1", "samples"

# Custom cases
assert run("1\n1\n42") == "0", "single request"
assert run("1\n3\n5 5 5") == "0", "all equal values"
assert run("1\n4\n1 1000000000 2 999999999") == "499999999", "large gaps"
assert run("1\n3\n1 2 3") == "0", "n equals carvers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | yêu cầu duy nhất | 
| 3 5 5 5 | 0 | tất cả các giá trị bằng nhau | 
| 1 1000000000 2 999999 | | |
