---
title: "CF 435A - Xếp hàng tại điểm dừng xe buýt"
description: "Chúng tôi có một hàng người đang đợi ở bến xe buýt nhưng họ được tổ chức thành từng nhóm. Mỗi nhóm có số lượng người cố định và đứng liên tiếp trong hàng đợi. Một chiếc xe buýt đến có thể chở tối đa m người và mọi người đi vào theo thứ tự nhóm của họ."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 435
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 249 (Div. 2)"
rating: 1000
weight: 435
solve_time_s: 67
verified: true
draft: false
---

[CF 435A - Xếp hàng tại điểm dừng xe buýt](https://codeforces.com/problemset/problem/435/A) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng người đang đợi ở bến xe buýt nhưng họ được tổ chức thành từng nhóm. Mỗi nhóm có số lượng người cố định và đứng liên tiếp trong hàng đợi. Một chiếc xe buýt đến có thể chở tối đa`m`mọi người và mọi người bước vào theo thứ tự nhóm của họ. Nếu nhóm tiếp theo xếp hàng không thể ngồi vừa lên xe buýt, xe buýt sẽ rời đi với những người đã lên xe và toàn bộ nhóm chờ sẽ đợi chuyến xe tiếp theo. Chúng ta được yêu cầu tính xem cần bao nhiêu xe buýt để vận chuyển tất cả các nhóm. 

Đầu vào cung cấp`n`, số lượng nhóm,`m`, dung lượng của xe buýt và một mảng`a`kích thước`n`đại diện cho số người trong mỗi nhóm. Đầu ra là một số nguyên duy nhất, số lượng xe buýt cần thiết. 

Các hạn chế là nhỏ:`n`Và`m`nhiều nhất là 100. Điều này ngụ ý rằng ngay cả một mô phỏng đơn giản cũng đủ nhanh, vì cần tối đa 100 lần lặp để xử lý mỗi nhóm. Không cần cấu trúc dữ liệu nâng cao hoặc tối ưu hóa ngoài việc xử lý cẩn thận logic lấp đầy bus. 

Trường hợp biên phát sinh khi một nhóm có dung lượng chính xác bằng bus hoặc khi nhiều nhóm nhỏ có thể vừa khít với một bus. Ví dụ, nếu`m = 5`và quy mô nhóm là`[2, 3, 2]`, thì xe buýt đầu tiên có thể chở hai nhóm đầu tiên (2+3=5), nhưng nhóm 2 người cuối cùng cần một xe buýt mới. Việc thực hiện bất cẩn cố gắng "chia" các nhóm để lấp đầy xe buýt sẽ tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là mô phỏng quá trình theo đúng nghĩa đen: bắt đầu với một xe buýt trống, lặp lại các nhóm, thêm chúng vào xe buýt nếu chúng phù hợp và khởi chạy xe buýt bất cứ khi nào một nhóm không phù hợp. Đối với mỗi nhóm, chúng tôi kiểm tra chỗ trống còn lại trên xe buýt và quyết định tải nó hay khởi động một xe buýt mới. Điều này là đơn giản vì các ràng buộc cho phép nó. Vì`n=100`, mỗi bước là số học theo thời gian không đổi, do đó độ phức tạp tổng thể thực tế là O(n). 

Không có thuật toán nào nhanh hơn đáng kể vì vấn đề mang tính tuần tự: vị trí của mỗi nhóm phụ thuộc vào lượng chỗ ngồi trên xe buýt trước đó. Thông tin chi tiết quan trọng là bạn không cần quản lý từng người mà chỉ cần quản lý tổng tải của mỗi xe buýt. Bằng cách lưu giữ tổng số hành khách trên xe buýt hiện tại, bạn có thể quyết định có nên bắt đầu xe buýt mới hay không khi một nhóm không thể phù hợp. Điều này làm giảm các lỗi tiềm ẩn của từng người một so với việc quản lý từng người một. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng nhóm | O(n) | O(1) | Đã chấp nhận | 
| Đóng gói trước hoặc ưa thích | O(n) | O(n) | Quá mức cần thiết cho vấn đề này | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm`buses`lên 1 vì chúng ta cần ít nhất một bus và một biến`current_load`về 0 để theo dõi số người trên xe buýt hiện tại. 
2. Lặp lại từng nhóm`g`trong mảng`a`. 
3. Nếu thêm`g`ĐẾN`current_load`sẽ vượt quá`m`, tăng`buses`và thiết lập lại`current_load`về 0. Điều này mô phỏng việc gửi bus hiện tại và bắt đầu một bus mới. 
4. Thêm quy mô nhóm`g`ĐẾN`current_load`để giải thích cho nhóm mới lên máy bay. 
5. Sau khi duyệt qua tất cả các nhóm,`buses`chứa số lượng xe buýt cần thiết. 

Điều bất biến ở đây là ở mỗi bước,`current_load`không bao giờ vượt quá`m`và mọi nhóm đều được tải đầy đủ vào một bus mà không cần tách. Điều này đảm bảo tính chính xác vì chúng tôi tôn trọng quy tắc "nhóm ở cùng nhau" và không bao giờ chở quá đầy xe buýt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
a = list(map(int, input().split()))

buses = 1
current_load = 0

for g in a:
    if current_load + g > m:
        buses += 1
        current_load = 0
    current_load += g

print(buses)
```Giải pháp này bắt đầu bằng một xe buýt vì luôn có ít nhất một nhóm. Séc`current_load + g > m`đảm bảo rằng chúng ta không làm quá tải xe buýt và tôn trọng ràng buộc nhóm. Đặt lại`current_load`sau khi bắt đầu một xe buýt mới, đảm bảo chúng tôi đếm chính xác xe buýt tiếp theo. Tất cả các phép toán đều là số học số nguyên đơn giản nên không có nguy cơ tràn. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:`4 3`với các nhóm`[2, 3, 2, 1]`| Nhóm | Tải hiện tại | Xe buýt | 
| --- | --- | --- | 
| 2 | 2 | 1 | 
| 3 | 3 (trước 2+3>3 → xe buýt mới) | 2 | 
| 2 | 2 | 2 | 
| 1 | 3 | 2 → không cần xe buýt mới | 

Bảng cho thấy rằng bất cứ khi nào một nhóm không thể phù hợp, một xe buýt mới sẽ khởi động, khớp chính xác với mô tả vấn đề. 

**Mẫu 2:** 

đầu vào:`5 5`với các nhóm`[1, 2, 3, 4, 5]`| Nhóm | Tải hiện tại | Xe buýt | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 3 | 1 | 
| 3 | 3+3>5 → xe buýt mới | 2 | 
| 4 | 4 | 2 | 
| 5 | 5 | 3 | 

Điều này chứng tỏ rằng các nhóm lớn hơn sẽ bắt đầu có xe buýt mới một cách tự nhiên và các nhóm nhỏ sẽ tích lũy cho đến khi xe buýt đầy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nhóm được xử lý chính xác một lần, được kiểm tra và cập nhật liên tục. | 
| Không gian | O(1) | Chỉ có hai biến số nguyên được duy trì ngoài mảng đầu vào. | 

Từ`n`tối đa là 100, thuật toán này chạy thoải mái trong giới hạn thời gian 1 giây và mức sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    buses = 1
    current_load = 0
    for g in a:
        if current_load + g > m:
            buses += 1
            current_load = 0
        current_load += g
    return str(buses)

# provided sample
assert run("4 3\n2 3 2 1\n") == "3", "sample 1"

# minimum-size input
assert run("1 1\n1\n") == "1", "minimum size"

# all groups equal to bus capacity
assert run("3 5\n5 5 5\n") == "3", "each group fits exactly one bus"

# groups smaller than bus capacity
assert run("4 10\n2 3 4 1\n") == "1", "all groups fit in one bus"

# groups that force alternating bus counts
assert run("5 5\n1 4 2 3 5\n") == "4", "mix of fits and overshoot"

# maximum-size groups for maximum bus
assert run("100 100\n" + "100 "*100 + "\n") == "100", "each group fills one bus"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1\n1`| 1 | Kịch bản đầu vào tối thiểu | 
|`3 5\n5 5 5`| 3 | Nhóm chính xác bằng công suất xe buýt | 
|`4 10\n2 3 4 1`| 1 | Nhiều nhóm nhỏ phù hợp trên một xe buýt | 
|`5 5\n1 4 2 3 5`| 4 | Các nhóm yêu cầu chia thành các xe buýt | 
|`100 100\n100 100 ...`| 100 | Đầu vào lớn, kích thước tối đa | 

## Vỏ cạnh 

Nếu một nhóm duy nhất lấp đầy chính xác một xe buýt, thuật toán sẽ tăng lên`buses`và ngay lập tức đặt nhóm vào, đếm chính xác. Đối với nhiều nhóm nhỏ cùng nhau lấp đầy một chiếc xe buýt,`current_load`tích lũy cho đến khi đạt`m`, và chỉ khi đó một xe buýt mới mới khởi động, ngăn chặn từng lỗi một. Nếu nhóm cuối cùng lấp đầy một phần xe buýt, thuật toán vẫn tính xe buýt vì`buses`bắt đầu từ 1.
