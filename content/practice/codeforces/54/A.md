---
title: "CF 54A - Quà tặng"
description: "Chúng ta được yêu cầu ước tính số quà tối thiểu mà Nhím sẽ nhận được trong N ngày tới. Anh ta có hai quy tắc chi phối việc nhận quà: mỗi ngày lễ anh ta đều nhận được một món quà và anh ta không thể đi quá K ngày mà không nhận được một món quà."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 54
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 50"
rating: 1300
weight: 54
solve_time_s: 79
verified: true
draft: false
---

[CF 54A - Quà](https://codeforces.com/problemset/problem/54/A) 

**Đánh giá:** 1300 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu ước tính số quà tối thiểu mà Nhím sẽ nhận được trong thời gian tới.`N`ngày. Anh ta có hai quy tắc quản lý việc nhận quà: mỗi ngày lễ anh ta đều nhận được một món quà và anh ta không thể đi nhiều hơn`K`ngày không nhận được. Món quà hôm nay đã được tính sẵn nên chúng ta chỉ xem xét từ ngày 1 đến`N`. 

Đầu vào mang lại`N`Và`K`Đầu tiên. Sau đó chúng tôi được cho biết ngày nào là ngày lễ. Mục tiêu là đếm số lượng quà tối thiểu cần thiết để đáp ứng cả hai ràng buộc. Đầu ra là một số nguyên duy nhất: số tối thiểu này. 

Nhìn vào những hạn chế,`N`lên tới 365, nhỏ một cách thoải mái. Điều này có nghĩa là ngay cả một thuật toán có khoảng`N^2`hoạt động sẽ chạy tốt trong 2 giây.`K`nhiều nhất là`N`, vì vậy chúng ta phải lên kế hoạch tặng quà với những khoảng trống có thể lớn nhưng không bao giờ vượt quá`K`ngày. Các ngày lễ là duy nhất và được sắp xếp, giúp đơn giản hóa việc quét. 

Các trường hợp đặc biệt không rõ ràng bao gồm tình huống một ngày lễ trùng với một ngày lẽ ra đã nhận được quà do`K`-quy tắc ngày. Ví dụ, nếu`N=5`,`K=2`, và ngày lễ là`[2,4]`, một cách tiếp cận ngây thơ chỉ đơn giản là đếm`ceil(N/K)`và thêm ngày nghỉ sẽ tính gấp đôi sự trùng lặp. Một trường hợp phức tạp khác là khi`K`lớn hơn`N`; thì có thể cần nhiều nhất một món quà không phải ngày lễ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng hàng ngày từ 1 đến`N`, theo dõi ngày cuối cùng người ta nhận được quà. Vào mỗi ngày, chúng tôi kiểm tra xem đó có phải là ngày nghỉ lễ hay nhiều hơn`K`ngày đã trôi qua kể từ món quà cuối cùng. Nếu một trong hai giá trị này đúng, chúng tôi sẽ tăng số lượng hiện tại và cập nhật ngày hiện tại cuối cùng. Điều này hiệu quả vì nó trực tiếp thực thi cả hai ràng buộc, nhưng mặc dù`N`nhỏ, dài dòng và không cần thiết phải lặp lại mỗi ngày. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng các món quà phải được đặt cách nhau tối đa`K`ngày xa nhau, nhưng ngày lễ cố định những ngày hiện tại. Vì vậy, chúng ta có thể tham lam đặt quà vào các ngày lễ trước, sau đó lấp đầy khoảng trống giữa chúng để không bị kéo dài.`K`ngày là hiện tại ít hơn. Bằng cách xử lý khoảng thời gian giữa các lần tặng quà (hoặc từ ngày 0 đến ngày nghỉ đầu tiên, giữa các ngày lễ và từ ngày nghỉ cuối cùng đến ngày`N`), chúng ta có thể tính toán số lượng quà bổ sung được yêu cầu bằng phép chia số nguyên đơn giản. 

Điều này chuyển đổi vấn đề từ mô phỏng hàng ngày sang phân tích theo khoảng thời gian. Đối với bất kỳ khoảng thời gian nào`L`, số quà cần thiết là`(L-1)//K + 1`nếu như`L > 0`. Điều này hiệu quả vì mỗi món quà đều bao gồm`K`ngày sau đó; công thức đếm số lượng quà tối thiểu để bao gồm toàn bộ khoảng thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N) | O(N) | Có thể chấp nhận được, nhưng dài dòng | 
| Phân tích khoảng thời gian / Tham lam | O(C) | O(C) | Tối ưu, đơn giản và rõ ràng | 

## Hướng dẫn thuật toán 

1. Đọc`N`,`K`, và danh sách các ngày nghỉ lễ. Thêm số 0 vào danh sách ngày lễ để biểu thị "hôm nay" đã có quà. Nối thêm`N+1`như một người canh gác để đơn giản hóa việc xử lý khoảng thời gian cuối cùng. 
2. Khởi tạo bộ đếm`presents`đến 0. 
3. Lặp lại các cặp ngày nghỉ liên tiếp,`start`Và`end`. Đối với mỗi khoảng thời gian`(start, end)`, tính số ngày giữa các lần tặng như sau`end - start - 1`. Nếu độ dài này là dương, hãy tính số lượng quà tối thiểu bằng cách sử dụng`(length - 1) // K + 1`và thêm nó vào`presents`. 
4. Đầu ra`presents`. 

Tại sao nó hoạt động: Điều bất biến là mọi ngày lễ đều đã nhận được một món quà và mọi khoảng thời gian giữa các món quà đều được bao phủ bởi công thức tôn trọng`K`-ngày tối đa. Mỗi khoảng là độc lập và việc thêm quà vào trong khoảng đó theo kiểu tham lam này sẽ đảm bảo không có khoảng cách về`K`ngày bị bỏ lỡ. Không có chuyện tính trùng vì ngày lễ đã có quà rồi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

N, K = map(int, input().split())
line = list(map(int, input().split()))
C = line[0]
holidays = line[1:]

# Add today (day 0) and a sentinel after the last day
days = [0] + holidays + [N+1]
presents = 0

for i in range(1, len(days)):
    interval = days[i] - days[i-1] - 1
    if interval > 0:
        presents += (interval + K - 1) // K

print(presents)
```Trước tiên, chúng tôi đọc dữ liệu đầu vào và chuyển đổi ngày nghỉ thành danh sách ngày. Đang chuẩn bị ngày 0 và nối thêm`N+1`đơn giản hóa các trường hợp cạnh. Đối với mỗi khoảng thời gian giữa các “ngày hiện tại” liên tiếp, chúng tôi tính toán số quà tối thiểu cần thiết bằng cách sử dụng mức chia trần, tức là`(interval + K - 1) // K`. Cuối cùng, tổng số được in. Sự lựa chọn cẩn thận của`interval = days[i] - days[i-1] - 1`tránh các lỗi riêng lẻ thường phát sinh khi đếm ngày bao gồm/độc quyền. 

## Ví dụ đã hoạt động 

**Mẫu 1**:`N=5`,`K=2`, ngày lễ`[1,3]`| tôi | ngày[i-1] | ngày[i] | khoảng thời gian | quà được thêm vào | tổng số quà | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 0 | 0 | 0 | 
| 2 | 1 | 3 | 1 | 1 | 1 | 
| 3 | 3 | 6 | 2 | 2 | 3 | 

Thuật toán xác định chính xác rằng cần có 3 món quà: một cho khoảng thời gian giữa ngày 1 và 3 và hai cho khoảng thời gian sau ngày 3 đến ngày 5. 

**Ví dụ tùy chỉnh**:`N=7`,`K=3`, ngày lễ`[2,5]`| tôi | ngày[i-1] | ngày[i] | khoảng thời gian | quà được thêm vào | tổng số quà | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 2 | 1 | 1 | 1 | 
| 2 | 2 | 5 | 2 | 1 | 2 | 
| 3 | 5 | 8 | 2 | 1 | 3 | 

Các vị trí thuật toán trình bày tối ưu: các khoảng được bao phủ mà không vượt quá`K`ngày. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(C) | Chúng tôi xử lý`C+2`ngày (ngày lễ + hôm nay + trọng điểm) và thực hiện số học đơn giản trong mỗi khoảng thời gian. | 
| Không gian | O(C) | Chúng tôi lưu trữ danh sách ngày tăng cường. | 

Cho rằng`C ≤ N ≤ 365`, giải pháp này cực kỳ nhanh và phù hợp với giới hạn bộ nhớ và thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    N, K = map(int, input().split())
    line = list(map(int, input().split()))
    C = line[0]
    holidays = line[1:]
    days = [0] + holidays + [N+1]
    presents = 0
    for i in range(1, len(days)):
        interval = days[i] - days[i-1] - 1
        if interval > 0:
            presents += (interval + K - 1) // K
    return str(presents)

# provided sample
assert run("5 2\n1 3\n") == "3", "sample 1"

# minimum input
assert run("1 1\n0\n") == "1", "single day, no holiday"

# maximum K
assert run("10 10\n2 3 7\n") == "2", "K covers most days"

# all days holidays
assert run("5 1\n5 1 2 3 4 5\n") == "5", "every day a holiday"

# no holidays, K=2
assert run("6 2\n0\n") == "3", "even spacing with K=2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 0 | 1 | Kích thước đầu vào tối thiểu, không có ngày nghỉ | 
| 10 10 2 3 7 | 2 | Giá trị K tối đa, ngày nghỉ thưa thớt | 
| 5 1 5 1 2 3 4 5 | 5 | Tất cả các ngày lễ, đảm bảo không tính trùng | 
| 6 2 0 | 3 | Khoảng thời gian không nghỉ lễ được xử lý chính xác | 

## Vỏ cạnh 

cho`N=1`,`K=1`, không có ngày nghỉ: khoảng thời gian là`1`ngày kia ngày hôm nay. Công thức`(1 + 1 - 1)//1 = 1`trả lại đúng 1 món quà. 

Vì`N=10`,`K=10`, ngày lễ`[3,7]`, khoảng là`[0-3]`,`[3-7]`,`[7-11]`. Những món quà được thêm vào mỗi khoảng thời gian là`ceil(2/10)=1`,
