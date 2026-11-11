---
title: "CF 158B - Taxi"
description: "Chúng tôi có một nhóm học sinh được tổ chức thành các nhóm và mỗi nhóm muốn cùng nhau đi du lịch bằng taxi. Mỗi nhóm có từ một đến bốn trẻ em và mỗi chiếc taxi có thể chở tối đa bốn trẻ em."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 158
codeforces_index: "B"
codeforces_contest_name: "VK Cup 2012 Qualification Round 1"
rating: 1100
weight: 158
solve_time_s: 81
verified: true
draft: false
---

[CF 158B - Taxi](https://codeforces.com/problemset/problem/158/B) 

**Đánh giá:** 1100 
**Tags:** *đặc biệt, tham lam, thực hiện 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một nhóm học sinh được tổ chức thành các nhóm và mỗi nhóm muốn cùng nhau đi du lịch bằng taxi. Mỗi nhóm có từ một đến bốn trẻ em và mỗi chiếc taxi có thể chở tối đa bốn trẻ em. Mục tiêu là xác định số lượng taxi tối thiểu cần thiết để vận chuyển tất cả các nhóm trong khi tôn trọng rằng các nhóm không thể chia thành nhiều taxi, mặc dù nhiều nhóm có thể chia sẻ một taxi nếu tổng số nhóm của họ không vượt quá bốn. 

Đầu vào bao gồm số lượng nhóm, tiếp theo là kích thước của mỗi nhóm. Đầu ra là một số nguyên biểu thị số lượng taxi tối thiểu cần thiết. 

Hạn chế chính là có thể có tới 100.000 nhóm và giới hạn thời gian là ba giây. Điều này ngụ ý rằng chúng ta không thể chấp nhận bất cứ điều gì tệ hơn thời gian tuyến tính đối với số lượng nhóm. Các cách tiếp cận bậc hai hoặc ngây thơ cố gắng thực hiện tất cả các kết hợp của các nhóm sẽ quá chậm, bởi vì số lượng các kết hợp có thể tăng theo cấp số nhân với số lượng nhóm. 

Các trường hợp đặc biệt cần xem xét bao gồm các kịch bản trong đó tất cả các nhóm đều có quy mô bốn, sẽ buộc phải có một taxi cho mỗi nhóm và các kịch bản trong đó tất cả các nhóm đều có quy mô một, cho phép xếp hoàn hảo vào taxi bốn người. Một trường hợp tinh tế khác là sự kết hợp của các quy mô nhóm sao cho việc đóng gói tham lam là không hề tầm thường - ví dụ, một nhóm ba người và một nhóm hai người không thể khớp với nhau, vì vậy thứ tự của sự kết hợp rất quan trọng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng thử tất cả các hoán vị của các nhóm và cố gắng xếp chúng vào các taxi theo mọi thứ tự có thể. Điều này đúng vì nó khám phá mọi cấu hình, nhưng nó không khả thi về mặt tính toán với n lên tới 100.000. Ngay cả khi mỗi phép kiểm tra hoán vị là tuyến tính thì việc tăng giai thừa của các hoán vị cũng khiến điều đó là không thể. 

Cái nhìn sâu sắc quan trọng là các nhóm chỉ có bốn kích cỡ có thể. Phạm vi giới hạn này cho phép tiếp cận dựa trên tần số. Chúng ta có thể đếm xem có bao nhiêu nhóm thuộc mỗi quy mô và kết hợp chúng một cách tối ưu. Các nhóm cỡ bốn luôn tự mình đi taxi. Các nhóm có kích thước ba có thể được ghép nối với các nhóm có kích thước một nếu có. Các nhóm có kích thước hai có thể được ghép nối với một nhóm có kích thước hai khác hoặc với tối đa hai nhóm có kích thước một. Cuối cùng, các nhóm có kích thước một còn sót lại có thể được kết hợp thành bốn nhóm. Cấu trúc của vấn đề cho phép sử dụng chiến lược tham lam: xử lý các nhóm lớn nhất trước và lấp đầy khoảng trống còn lại bằng các nhóm nhỏ hơn. Điều này đảm bảo số lượng taxi tối thiểu vì các nhóm lớn hơn là yếu tố hạn chế và các nhóm nhỏ hơn có thể được sử dụng để lấp đầy khoảng trống mà không vi phạm các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu (tần số + tham lam) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đếm xem có bao nhiêu nhóm thuộc mỗi nhóm từ 1 đến 4. Điều này cho chúng ta một bảng tần số nhỏ mà chúng ta có thể sử dụng cho các quyết định tham lam. 
2. Mỗi nhóm cỡ 4 cần có một chiếc taxi riêng. Tăng quầy taxi theo số nhóm cỡ 4. 
3. Ghép từng nhóm cỡ ba với nhóm cỡ một nếu có thể. Đối với mỗi cặp, tăng bộ đếm taxi và giảm số lượng nhóm cỡ một tương ứng. Nếu không đủ nhóm cỡ một thì nhóm cỡ ba vẫn chiếm một taxi riêng. 
4. Ghép các nhóm có kích thước hai lại với nhau. Mỗi cặp nhóm cỡ hai vừa vặn trong một chiếc taxi. Nếu vẫn còn một nhóm lẻ có kích thước hai, thì nó có thể được ghép nối với tối đa hai nhóm có kích thước một. Tăng bộ đếm taxi và giảm kích thước nhóm một cho phù hợp. Nếu không còn nhóm cỡ một thì nhóm cỡ hai sẽ đi taxi một mình. 
5. Cuối cùng, xử lý các nhóm cỡ một còn lại. Họ có thể chia sẻ bốn chiếc taxi cho mỗi chiếc taxi. Tính số lượng taxi bằng tổng số xe còn thừa chia cho 4. 
6. Tổng tất cả các taxi được tính ở các bước này sẽ cho ra số lượng taxi tối thiểu cần có. 

Tại sao nó hoạt động: điều bất biến là ở mỗi bước, chúng tôi sẽ lấp đầy tối đa các xe taxi với các nhóm lớn nhất còn lại và chỉ sau đó sử dụng các nhóm nhỏ hơn để lấp đầy các khoảng trống. Vì quy mô nhóm bị giới hạn từ 1 đến 4 nên không có cấu hình nào trong đó việc kết hợp khác nhau sẽ làm giảm tổng số taxi, khiến cách tiếp cận tham lam này trở nên tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
sizes = list(map(int, input().split()))

count = [0] * 5  # index 0 unused
for s in sizes:
    count[s] += 1

taxis = 0

# size 4 groups
taxis += count[4]

# size 3 groups + size 1 groups
pair_3_1 = min(count[3], count[1])
taxis += pair_3_1
count[3] -= pair_3_1
count[1] -= pair_3_1

# remaining size 3 groups
taxis += count[3]

# size 2 groups
taxis += count[2] // 2
if count[2] % 2:
    taxis += 1
    count[1] -= min(2, count[1])

# size 1 groups
if count[1] > 0:
    taxis += (count[1] + 3) // 4  # ceil division

print(taxis)
```Mã khởi tạo một mảng tần số cho các nhóm thuộc từng kích cỡ. Nó đếm trực tiếp số taxi cần thiết cho nhóm bốn người, sau đó ghép ba chiếc với một chiếc để tối ưu hóa việc sử dụng không gian. Ba và hai còn lại được xử lý cẩn thận, những chiếc còn sót lại sẽ được lấp đầy trên taxi sau cùng. Bộ phận trần xử lý những phần còn sót lại một cách hiệu quả và tất cả các hoạt động đều có thời gian không đổi ngoại trừ lần đếm ban đầu, tạo ra độ phức tạp về thời gian tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`5 1 2 4 3 3`| Bước | đếm[1] | đếm[2] | đếm[3] | đếm[4] | taxi | Giải thích | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 1 | 1 | 2 | 1 | 0 | đếm kích thước | 
| cỡ 4 | 1 | 1 | 2 | 1 | 1 | nhóm 4 người đi taxi | 
| cặp 3&1 | 0 | 1 | 1 | 1 | 2 | ghép một nhóm 3 với một nhóm 1 | 
| 3 còn lại | 0 | 1 | 1 | 1 | 3 | 3 còn lại chiếm taxi | 
| cặp 2&2 | 0 | 1 | 1 | 1 | 3 | chỉ có một 2, sẽ xử lý tiếp theo | 
| còn sót lại 2 + cái | 0 | 1 | 1 | 1 | 4 | 2 người còn lại đi taxi; không có ai để ghép | 
| còn sót lại 1 | 0 | 1 | 1 | 1 | 4 | không còn ai | 

Đầu ra là 4, phù hợp với mẫu. 

### Ví dụ 2 

đầu vào:`8 1 1 1 1 2 2 3 4`| Bước | đếm[1] | đếm[2] | đếm[3] | đếm[4] | taxi | Giải thích | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 4 | 2 | 1 | 1 | 0 | đếm kích thước | 
| cỡ 4 | 4 | 2 | 1 | 1 | 1 | taxi cho 4 người | 
| cặp 3&1 | 3 | 2 | 0 | 1 | 2 | 3 ghép với 1 | 
| còn lại 2 | 3 | 2 | 0 | 1 | 3 | hai 2 hình thành một chiếc taxi | 
| còn sót lại 1 | 3 | 0 | 0 | 1 | 4 | 3 người còn lại cần một taxi | 

Đầu ra: 4. Dấu vết thể hiện thuật toán xử lý chính xác nhiều cặp và các cặp còn sót lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để đếm kích thước, sau đó thực hiện các thao tác liên tục để tính toán taxi | 
| Không gian | O(1) | Dải tần có kích thước 5 độc lập với n | 

Thời gian tuyến tính có thể chấp nhận được với n lên tới 100.000 và không gian không đổi đảm bảo thỏa mãn giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    sizes = list(map(int, input().split()))
    count = [0] * 5
    for s in sizes:
        count[s] += 1
    taxis = 0
    taxis += count[4]
    pair_3_1 = min(count[3], count[1])
    taxis += pair_3_1
    count[3] -= pair_3_1
    count[1] -= pair_3_1
    taxis += count[3]
    taxis += count[2] // 2
    if count[2] % 2:
        taxis += 1
        count[1] -= min(2, count[1])
    if count[1] > 0:
        taxis += (count[1] + 3) // 4
    return str(taxis)

# Provided sample
assert run("5\n1 2 4 3 3\n") == "4", "sample 1"

# Custom cases
assert run("4\n4 4 4 4\n") == "4", "all size 4"
assert run("8\n1 1 1 1 2 2 3 4\n") == "4", "mixed sizes"
assert run("5\n1 1 1
```
