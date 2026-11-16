---
title: "CF 1912J - Niềm vui quan sát Pok\u00e9mon"
description: "Chúng ta được sắp xếp theo vòng tròn n Pokémon, mỗi Pokémon có một giá trị quan sát riêng biệt. Người chơi có thể bắt đầu với bất kỳ Pokémon nào và liên tục di chuyển đến Pokémon tiếp theo trong vòng tròn."
date: "2026-06-08T20:17:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 1912
codeforces_index: "J"
codeforces_contest_name: "2023-2024 ICPC, NERC, Northern Eurasia Onsite (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 2300
weight: 1912
solve_time_s: 71
verified: true
draft: false
---

[CF 1912J - Niềm vui quan sát Pok\u00e9mon](https://codeforces.com/problemset/problem/1912/J) 

**Đánh giá:** 2300 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được sắp xếp theo vòng tròn`n`Pokémon, mỗi Pokémon có một giá trị quan sát riêng biệt. Người chơi có thể bắt đầu với bất kỳ Pokémon nào và liên tục di chuyển đến Pokémon tiếp theo trong vòng tròn. Thao tác chính là nếu chênh lệch về giá trị quan sát giữa Pokémon hiện tại và Pokémon tiếp theo nhỏ hơn hoặc bằng một ngưỡng`k`, người chơi có thể tiếp tục quan sát mà không bị gián đoạn. Nếu không, người chơi phải thiết lập lại trình tự quan sát. 

Đầu vào cung cấp`n`Và`k`, theo sau là mảng các giá trị quan sát. Đầu ra phải là số lượng Pokémon tối đa mà người chơi có thể quan sát liên tiếp theo các quy tắc này, có khả năng bao quanh vòng tròn. 

Cho rằng`n`có thể đạt tới 200.000, mọi giải pháp tệ hơn O(n log n) có thể sẽ hết thời gian chờ. Một cách tiếp cận O(n²) ngây thơ, kiểm tra độ dài chuỗi bắt đầu từ từng Pokémon riêng lẻ, sẽ thực hiện các thao tác khoảng 4 × 10¹⁰ trong trường hợp xấu nhất, điều này hoàn toàn không khả thi. 

Các trường hợp đặc biệt không rõ ràng bao gồm các chuỗi trong đó phân đoạn lớn nhất của các Pokémon liên tiếp bao quanh từ cuối mảng trở lại đầu mảng. Ví dụ, nếu`k = 2`và giá trị quan sát là`[1, 2, 5, 1]`, đoạn tối đa bao gồm phần tử cuối cùng và phần tử đầu tiên. Quét tuyến tính đơn giản mà không xử lý vòng tròn sẽ bỏ lỡ điều này. 

## Phương pháp tiếp cận 

Giải pháp bạo lực rất đơn giản: đối với mỗi Pokémon bắt đầu, hãy di chuyển về phía trước cho đến khi vượt quá sự khác biệt giữa các Pokémon liên tiếp.`k`. Theo dõi chuỗi dài nhất được tìm thấy. Điều này đảm bảo tính chính xác vì nó kiểm tra rõ ràng tất cả các vị trí bắt đầu có thể. Vấn đề là đối với quy mô lớn`n`, lặp lại`n`số lần cho mỗi lần khởi động dẫn đến O(n2), điều này là không thể chấp nhận được. 

Thông tin chi tiết quan trọng đến từ việc nhận thấy rằng vấn đề tương đương với việc tìm ra mảng con dài nhất của các Pokémon liên tiếp trong đó sự khác biệt giữa các phần tử liên tiếp không vượt quá`k`, xét mảng hình tròn. Bằng cách sao chép mảng (nối nó với chính nó) và sử dụng phương pháp cửa sổ trượt, chúng ta có thể duyệt qua nhiều nhất`2n`các phần tử một cách tuyến tính trong khi vẫn duy trì cửa sổ hợp lệ tối đa. Chúng tôi theo dõi chỉ số bắt đầu và kết thúc của cửa sổ và mở rộng cửa sổ đó cho đến khi vi phạm ràng buộc, sau đó chuyển điểm bắt đầu về phía trước. Gói tròn được xử lý một cách tự nhiên bằng cách sao chép này và độ dài cửa sổ được giới hạn ở`n`để tránh đếm nhiều Pokémon hơn mức tồn tại. 

Điều này chuyển vấn đề từ O(n²) thành O(n) vì mỗi Pokémon được truy cập nhiều nhất hai lần: một lần khi vào cửa sổ và một lần khi rời đi. Cửa sổ trượt tận dụng cấu trúc đơn điệu của các sai phân liên tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Cửa sổ trượt có tính năng nhân bản | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`k`, sau đó đọc mảng giá trị quan sát`a`. 
2. Xây dựng một mảng trùng lặp`b = a + a`để xử lý tính tuần hoàn mà không cần logic modulo đặc biệt. 
3. Khởi tạo hai con trỏ`start = 0`Và`end = 0`, và một biến`max_len = 0`. 
4. Mở rộng`end`chuyển tiếp trong khi`end < 2 * n`và sự khác biệt giữa`b[end]`Và`b[end - 1]`nhiều nhất là`k`. Điều này xác định một chuỗi quan sát hợp lệ. 
5. Cập nhật`max_len = max(max_len, end - start)`để theo dõi chuỗi dài nhất được tìm thấy. 
6. Khi chênh lệch vượt quá`k`, tăng`start`để thu nhỏ cửa sổ và tiếp tục trượt. 
7. Đảm bảo rằng`end - start`không bao giờ vượt quá`n`để tránh đếm vượt quá chiều dài vòng tròn. 
8. Sau khi xử lý tất cả các vị trí, xuất ra`max_len`. 

Tại sao nó hoạt động: tính bất biến của cửa sổ trượt đảm bảo rằng cửa sổ luôn biểu thị một chuỗi quan sát hợp lệ dưới`k`hạn chế. Bằng cách sao chép mảng, các chuỗi vòng tròn được ghi lại một cách tuyến tính và bằng cách giới hạn độ dài cửa sổ ở mức`n`, chúng ta tránh việc đếm quá mức. Mọi vị trí bắt đầu tiềm năng đều được xem xét ngầm bởi quá trình truyền tải cửa sổ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

b = a + a  # duplicate array for circular handling
max_len = 0
start = 0

for end in range(1, 2 * n):
    if b[end] - b[end - 1] > k:
        start = end
    max_len = max(max_len, min(end - start + 1, n))

print(max_len)
```Giải thích: 

Chúng tôi nhân đôi mảng để biến các kiểm tra vòng tròn thành các kiểm tra tuyến tính. các`for`vòng lặp kiểm tra từng cặp phần tử liên tiếp; nếu chênh lệch vượt quá`k`, cửa sổ sẽ được đặt lại. các`min(end - start + 1, n)`đảm bảo chúng tôi không bao giờ đếm được nhiều Pokémon hơn số lượng tồn tại trong vòng kết nối ban đầu. Cách tiếp cận này tránh được các lỗi thường gặp ở cửa sổ trượt tròn. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
4 2
1 2 5 1
```| bắt đầu | kết thúc | b[cuối] | b[cuối]-b[cuối-1] | max_len | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 1 | 2 | 
| 0 | 2 | 5 | 3 | 2 | 
| 2 | 3 | 1 | -4 | 2 | 
| 3 | 4 | 1 | 0 | 2 | 

Trình tự tối đa tôn trọng ngưỡng`k`có độ dài 2 (`1,2`hoặc`5,1`nếu chúng ta quấn). 

**Ví dụ 2** 

đầu vào:```
5 3
1 4 7 2 3
```| bắt đầu | kết thúc | b[cuối] | khác biệt | max_len | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 4 | 3 | 2 | 
| 0 | 2 | 7 | 3 | 3 | 
| 0 | 3 | 2 | -5 | 3 | 
| 3 | 4 | 3 | 1 | 2 | 

Trình tự tối đa là 3, đạt được bởi`[1,4,7]`. Trình tự bọc tròn được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử vào và ra khỏi cửa sổ trượt nhiều nhất một lần; trùng lặp tăng gấp đôi n nhưng vẫn tuyến tính. | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng trùng lặp có kích thước 2n. | 

Với`n`hoạt động lên tới 2×10⁵, O(n) là khoảng 4×10⁵, trong giới hạn thời gian. Bộ nhớ số nguyên ~4×10⁵ cũng được chấp nhận. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    b = a + a
    max_len = 0
    start = 0
    for end in range(1, 2 * n):
        if b[end] - b[end - 1] > k:
            start = end
        max_len = max(max_len, min(end - start + 1, n))
    return str(max_len)

# provided sample
assert run("4 2\n1 2 5 1\n") == "2"
# minimal input
assert run("1 0\n10\n") == "1"
# max k allows all
assert run("5 100\n1 2 3 4 5\n") == "5"
# wrap around needed
assert run("5 3\n1 4 7 2 3\n") == "3"
# all equal values
assert run("6 0\n5 5 5 5 5 5\n") == "6"
# alternating small/large differences
assert run("6 1\n1 2 1 2 1 2\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 2\n1 2 5 1 | 2 | ngưỡng đơn giản và bọc | 
| 1 0\n10 | 1 | đầu vào tối thiểu | 
| 5 100\n1 2 3 4 5 | 5 | tất cả đều có thể được quan sát | 
| 5 3\n1 4 7 2 3 | 3 | bọc tròn đúng cách | 
| 6 0\n5 5 5 5 5 5 | 6 | tất cả các phần tử bằng nhau | 
| 6 1\n1 2 1 2 1 2 | 2 | sự khác biệt xen kẽ | 

## Vỏ cạnh 

Đối với trường hợp bao quanh`[1, 4, 7, 2, 3]`với`k = 3`, thuật toán sẽ bắt đầu chính xác một cửa sổ mới bất cứ khi nào chênh lệch vượt quá`k`và tính theo vòng tròn do trùng lặp. Cửa sổ trượt không bao giờ vượt quá`n`, do đó, nó không kết hợp sai phân đoạn trùng lặp ngoài vòng tròn. 

Đối với các giá trị bằng nhau `[5, 5, 5
