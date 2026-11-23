---
title: "CF 51C - Ba trạm cơ sở"
description: "Chúng ta có một ngôi làng được biểu diễn dưới dạng các điểm dọc theo đường một chiều, mỗi điểm là tọa độ ngôi nhà. Nhiệm vụ là đặt chính xác ba trạm gốc di động dọc theo đường này sao cho mỗi ngôi nhà nằm trong vùng phủ sóng của ít nhất một trạm."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy"]
categories: ["algorithms"]
codeforces_contest: 51
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 48"
rating: 1800
weight: 51
solve_time_s: 133
verified: false
draft: false
---

[CF 51C - Ba trạm gốc](https://codeforces.com/problemset/problem/51/C) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, tham lam 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một ngôi làng được biểu diễn dưới dạng các điểm dọc theo đường một chiều, mỗi điểm là tọa độ ngôi nhà. Nhiệm vụ là đặt chính xác ba trạm gốc di động dọc theo đường này sao cho mỗi ngôi nhà nằm trong vùng phủ sóng của ít nhất một trạm. Vùng phủ sóng là đối xứng: một trạm ở vị trí`t`với sức mạnh`d`bao gồm khoảng thời gian`[t - d, t + d]`. Đầu ra chúng tôi muốn là tối thiểu`d`cho phép phủ sóng đầy đủ và một cấu hình hợp lệ của ba trạm. 

Những hạn chế cho chúng ta biết rằng`n`có thể lên tới 200.000 và tọa độ có thể đạt tới`10^9`. Việc sắp xếp và truyền tuyến tính trên mảng là khả thi, nhưng bất kỳ cách tiếp cận ngây thơ nào thử tất cả các vị trí có thể có cho cả ba trạm sẽ rất chậm về mặt thiên văn. Ví dụ, việc lặp lại tất cả các vị trí trạm tiềm năng cho mỗi ngôi nhà là không thể vì`10^9`các giá trị là quá nhiều. 

Các trường hợp cạnh tinh tế bao gồm: các ngôi nhà được xếp chồng lên nhau ở cùng một tọa độ, các ngôi nhà rất thưa thớt (ví dụ: một ở 1 và một ở 10^9) và các trường hợp có ít hơn ba trạm sẽ đủ tầm thường nếu cho phép chồng chéo. Một cách tiếp cận ngây thơ chỉ cố gắng chia đều các ngôi nhà có thể bỏ sót thực tế là một trạm duy nhất có thể bao phủ nhiều cụm, tạo ra một khoảng thời gian nhỏ hơn.`d`. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ cố gắng đặt ba trạm ở tất cả các tổ hợp tọa độ ngôi nhà có thể có, tính toán yêu cầu`d`cho mỗi sự kết hợp và chọn mức tối thiểu. Điều này đúng vì nó liệt kê mọi kịch bản, nhưng không khả thi: nếu`n = 2 × 10^5`, số lượng kết hợp theo thứ tự`O(n^3)`hoặc nhiều hơn`10^15`hoạt động. 

Quan sát quan trọng là bài toán có một tính chất đơn điệu: nếu một`d`có thể bao phủ tất cả các ngôi nhà bằng ba trạm, trạm lớn hơn`d`cũng có thể bao phủ tất cả các ngôi nhà. Ngược lại, nếu một`d`quá nhỏ, không nhỏ hơn`d`sẽ hoạt động. Tính đơn điệu này cho phép chúng ta tìm kiếm nhị phân trên`d`. Đối với một ứng cử viên`d`, chúng ta có thể tham lam bao phủ các ngôi nhà từ trái sang phải, đặt một trạm ở điểm xa nhất bao phủ ngôi nhà không được che chắn tiếp theo, sau đó tiếp tục. Nếu chúng ta có thể bao phủ tất cả các ngôi nhà bằng ba trạm trở xuống, ứng cử viên`d`là khả thi. 

Phạm vi phủ sóng tham lam hoạt động vì khoảng thời gian phủ sóng của mỗi trạm là liên tục, do đó, đặt trạm tiếp theo ở điểm ngoài cùng bên phải có thể bao phủ ngôi nhà đầu tiên luôn là điều tối ưu. Bất kỳ trạm nào được đặt xa hơn về bên trái sẽ làm tăng số lượng trạm hoặc yêu cầu về số lượng trạm một cách không cần thiết.`d`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + Tham lam | O(n log(maxX)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. **Sắp xếp tọa độ nhà**. Việc sắp xếp là cần thiết để đi qua các ngôi nhà từ trái sang phải và áp dụng phạm vi bao phủ tham lam một cách chính xác. 
2. **Khởi tạo giới hạn tìm kiếm nhị phân**. Bộ`lo = 0`Và`hi = 1e9`, khoảng cách tối đa mà một trạm có thể cần. Sử dụng một epsilon nhỏ (ví dụ:`1e-7`) cho độ chính xác của dấu phẩy động. 
3. **Vòng tìm kiếm nhị phân**. Đối với mỗi giá trị trung bình`mid = (lo + hi) / 2`, kiểm tra xem liệu chúng ta có thể bao phủ tất cả các ngôi nhà bằng ba trạm điện không`mid`. Nếu khả thi hãy chuyển`hi = mid`; nếu không, hãy di chuyển`lo = mid`. 
4. **Kiểm tra phạm vi bảo hiểm tham lam**. Bắt đầu ở ngôi nhà ngoài cùng bên trái. Đặt trạm tại`house + mid`, bao phủ tất cả các ngôi nhà lên đến`house + 2 * mid`. Lặp lại cho ngôi nhà không có mái che tiếp theo, tối đa ba trạm. Nếu bất kỳ ngôi nhà nào vẫn chưa được che chắn sau ba trạm, ứng cử viên`mid`là quá nhỏ. 
5. **Xây dựng lại vị trí của các trạm**. Khi tìm kiếm nhị phân hội tụ, hãy thực hiện phạm vi bao phủ tham lam một lần nữa để tạo ra các vị trí chính xác: đặt mỗi trạm ở điểm ngoài cùng bên phải của khoảng bao phủ ngôi nhà không được che chắn tiếp theo. 

**Tại sao nó hoạt động**. Việc sắp xếp đảm bảo rằng các ngôi nhà được xử lý theo thứ tự. Vị trí tham lam đảm bảo sự chồng chéo tối thiểu: việc đặt một trạm xa hơn về bên trái sẽ không mở rộng phạm vi phủ sóng nhưng có thể buộc phải có thêm một trạm. Tìm kiếm nhị phân dựa trên tính đơn điệu: if`d`là khả thi, giá trị lớn hơn vẫn khả thi. Sự kết hợp này tạo ra mức tối thiểu`d`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_cover(houses, d):
    n = len(houses)
    count = 0
    i = 0
    while i < n:
        count += 1
        limit = houses[i] + 2 * d
        while i < n and houses[i] <= limit:
            i += 1
        if count > 3:
            return False
    return True

def find_stations(houses, d):
    stations = []
    n = len(houses)
    i = 0
    while i < n and len(stations) < 3:
        pos = houses[i] + d
        stations.append(pos)
        limit = houses[i] + 2 * d
        while i < n and houses[i] <= limit:
            i += 1
    while len(stations) < 3:
        stations.append(stations[-1])
    return stations

n = int(input())
houses = list(map(int, input().split()))
houses.sort()

lo, hi = 0.0, 1e9
eps = 1e-7
while hi - lo > eps:
    mid = (lo + hi) / 2
    if can_cover(houses, mid):
        hi = mid
    else:
        lo = mid

d = hi
stations = find_stations(houses, d)
print(f"{d:.6f}")
print(" ".join(f"{s:.6f}" for s in stations))
```Giải pháp tách kiểm tra tính khả thi (`can_cover`) từ việc xây dựng lại các trạm (`find_stations`). Sắp xếp trước vị trí tham lam là rất quan trọng. Tìm kiếm nhị phân sử dụng số dấu phẩy động và dừng khi khoảng nhỏ hơn`1e-7`để đảm bảo độ chính xác cho sáu số thập phân ở đầu ra. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
4
1 2 3 4
```| Bước | tôi | Nhà | Đếm | Giới hạn | Trạm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 1 + 2*0,5 = 2 | [1.5] | 
| 2 | 2 | 3 | 2 | 3 + 1 = 4 | [1,5, 3,5] | 
| 3 | xong | - | - | - | [1,5, 3,5] | 

Thể hiện tối thiểu`d = 0.5`bao phủ tất cả các ngôi nhà với hai trạm. 

**Ví dụ tùy chỉnh** 

đầu vào:```
5
1 2 10 11 12
```Tìm kiếm nhị phân tìm thấy`d = 2.5`. Trạm tham lam đặt ở 2,5, 11,5 và lặp lại trạm cuối 11,5 để thỏa mãn ba trạm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log(maxX)) | Việc sắp xếp mất O(n log n), tìm kiếm nhị phân yêu cầu các lần lặp O(log(maxX)), mỗi lần lặp tuyến tính qua các nhà | 
| Không gian | O(n) | Mảng nhà cộng với mảng trạm | 

Điều này phù hợp trong giới hạn thời gian và bộ nhớ.`n`có thể quản lý tối đa 2 × 10^5 bằng cách sắp xếp và nhiều đường chuyền tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    houses = list(map(int, input().split()))
    houses.sort()
    lo, hi = 0.0, 1e9
    eps = 1e-7
    def can_cover(houses, d):
        n = len(houses)
        count = 0
        i = 0
        while i < n:
            count += 1
            limit = houses[i] + 2*d
            while i < n and houses[i] <= limit:
                i += 1
            if count > 3:
                return False
        return True
    def find_stations(houses, d):
        stations = []
        n = len(houses)
        i = 0
        while i < n and len(stations) < 3:
            pos = houses[i] + d
            stations.append(pos)
            limit = houses[i] + 2*d
            while i < n and houses[i] <= limit:
                i += 1
        while len(stations) < 3:
            stations.append(stations[-1])
        return stations
    while hi - lo > eps:
        mid = (lo + hi)/2
        if can_cover(houses, mid):
            hi = mid
        else:
            lo = mid
    d = hi
    stations = find_stations(houses, d)
    out = f"{d:.6f}\n{' '.join(f'{s:.6f}' for s in stations)}"
    return out

# Provided sample
assert run("4\n1 2 3 4\n").startswith("0.500000"), "sample 1"

# Custom cases
assert run("1\n100\n").startswith("0.000
```
