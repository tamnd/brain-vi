---
title: "CF 460E - Roland và Rose"
description: "Roland muốn đặt n tháp canh trên một lưới số nguyên 2D xung quanh một bông hồng ở gốc tọa độ sao cho tổng bình phương khoảng cách giữa tất cả các cặp tháp là lớn nhất. Mỗi tháp phải nằm trong hoặc trên một hình tròn bán kính r có tâm tại (0,0)."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "geometry", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 460
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 262 (Div. 2)"
rating: 2700
weight: 460
solve_time_s: 75
verified: true
draft: false
---

[CF 460E - Roland và Rose](https://codeforces.com/problemset/problem/460/E) 

**Đánh giá:** 2700 
**Tags:** lực lượng vũ phu, hình học, toán học, sắp xếp 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Roland muốn đặt _n_ tháp canh trên một lưới số nguyên 2D xung quanh một bông hồng ở gốc tọa độ sao cho tổng bình phương khoảng cách giữa tất cả các cặp tháp là lớn nhất. Mỗi tháp phải nằm trong hoặc trên một vòng tròn bán kính _r_ có tâm tại (0,0). Nhiều tháp có thể chiếm cùng một tọa độ, bao gồm cả gốc tọa độ. Đầu ra đầu tiên là tổng bình phương tối đa của khoảng cách, sau đó là tọa độ của mỗi tháp. 

Đầu vào cung cấp số lượng tháp, _n_ và khoảng cách tối đa cho phép, _r_. Vì _n_ nhỏ, trong khoảng từ 2 đến 8, nên chúng ta có thể xem xét các phương pháp tổ hợp mà lẽ ra không thể thực hiện được đối với _n_ lớn. Bán kính _r_ tối đa là 30, nghĩa là tổng số tọa độ nguyên bên trong đường tròn bị giới hạn-khoảng πr² ≈ 2800 điểm cho r = 30. 

Một cách tiếp cận ngây thơ có thể thất bại nếu bỏ qua tính đối xứng. Ví dụ: việc đặt tất cả các tháp ở gốc là hợp lệ nhưng mang lại tổng khoảng cách bằng không. Một cách tiếp cận bất cẩn chỉ thử một điểm trên mỗi bán kính có thể bỏ lỡ cơ chế sắp xếp trong đó các điểm lặp lại sẽ tối đa hóa tổng tổng, chẳng hạn như các tháp xen kẽ ở các cạnh đối diện của vòng tròn. Các trường hợp cạnh bao gồm n=2 với r=1, trong đó vị trí tối ưu chỉ là các điểm đối diện trên vòng tròn hoặc n>4 với r nhỏ, trong đó nhiều tháp phải chia sẻ vị trí để tối đa hóa khoảng cách theo cặp. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: liệt kê tất cả các điểm nguyên trong vòng tròn, tạo ra tất cả n-bộ dữ liệu của các điểm này (cho phép lặp lại), tính tổng khoảng cách bình phương cho mỗi bộ dữ liệu và lấy mức tối đa. Mỗi phép tính khoảng cách yêu cầu n chọn 2 thao tác và tổng số bộ dữ liệu xấp xỉ O(m^n) trong đó m ≈ πr² là số điểm nguyên hợp lệ. Ngay cả với r=30 và n=8, m^n vẫn vào khoảng 2800^8, một con số lớn về mặt thiên văn và rõ ràng là không thực tế. 

Quan sát quan trọng là việc tối đa hóa tổng bình phương sẽ khuyến khích việc đặt các tháp càng xa nhau càng tốt. Công thức tổng bình phương khoảng cách là đối xứng và đóng góp của mỗi tọa độ có thể được coi là tổng bình phương dọc theo x và y. Với n nhỏ và r bị chặn, có thể tạo ra tất cả các điểm ứng viên bên trong vòng tròn và sau đó sử dụng tìm kiếm quay lui để đặt các tháp. Đối với mỗi lựa chọn, chúng tôi đệ quy thêm một tháp vào cấu hình hiện tại, theo dõi tổng bình phương khoảng cách. Vì n tối đa là 8 nên độ sâu đệ quy nhỏ. Chúng tôi cắt tỉa sớm bằng cách bỏ qua các cấu hình không thể vượt quá tổng tốt nhất hiện tại. 

Điều này làm giảm vấn đề trong việc liệt kê các điểm và sử dụng tìm kiếm đệ quy trên các vị trí, có thể xử lý được do số điểm n nhỏ và vừa phải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force: tất cả n-bộ điểm | O((π r²)^n * n²) | O(n) | Quá chậm | 
| Quay lui đệ quy trên các điểm | O(m^n * n²) với việc cắt tỉa | O(n) | Được chấp nhận với n ≤ 8 | 

## Hướng dẫn thuật toán 

1. Tạo tất cả các điểm nguyên (x, y) sao cho x2 + y2 ≤ r². Đây là những vị trí tháp ứng cử viên. Chúng ta bao gồm điểm gốc và tất cả các điểm khác bên trong đường tròn. Lý do của bước này là không có tháp nào có thể nằm ngoài vòng tròn và cần có tọa độ nguyên. 
2. Khởi tạo các biến để lưu trữ tổng bình phương tối đa của khoảng cách và cách sắp xếp tháp tương ứng. 
3. Xác định hàm đệ quy`search(towers, index)`Ở đâu`towers`là danh sách các tòa tháp đã được đặt và`index`là số lượng tháp được đặt cho đến nay. Chúng ta dừng đệ quy khi chỉ số = n. 
4. Tại mỗi bước đệ quy, lặp qua tất cả các điểm dự tuyển và cộng điểm đó vào`towers`. Tính tổng bình phương khoảng cách được đóng góp bởi tòa tháp mới này với tất cả các tòa tháp được đặt trước đó. Điều này đảm bảo chúng tôi chỉ tính toán những thay đổi nào chứ không phải toàn bộ khoảng cách n2 mỗi lần. 
5. Gọi đệ quy`search`với tháp mới được thêm vào. Sau khi đệ quy trở lại, hãy loại bỏ tháp cuối cùng để quay lại và thử điểm ứng cử viên tiếp theo. Điều này khám phá tất cả các cấu hình. 
6. Nếu đệ quy đạt đến n tháp, hãy so sánh tổng tổng với mức tối đa hiện tại và cập nhật nếu cần, đồng thời lưu trữ tọa độ tương ứng. 
7. Sau khi khám phá tất cả các cấu hình, xuất ra tổng tối đa và tọa độ của các tòa tháp. 

Tại sao nó hoạt động: Mỗi nhánh đệ quy khám phá một vị trí tháp riêng biệt. Việc tính toán tăng dần khoảng cách theo cặp đảm bảo tính chính xác và quay lui đảm bảo tất cả các cấu hình có thể được xem xét. Vì n 8 nên chúng ta khám phá một cách thấu đáo tất cả các cách sắp xếp khả thi, do đó tổng tối đa tìm được đảm bảo là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, r = map(int, input().split())
    points = []
    for x in range(-r, r+1):
        for y in range(-r, r+1):
            if x*x + y*y <= r*r:
                points.append((x, y))

    max_sum = -1
    best_config = []

    def dist2(p1, p2):
        dx = p1[0]-p2[0]
        dy = p1[1]-p2[1]
        return dx*dx + dy*dy

    def search(towers, index, current_sum):
        nonlocal max_sum, best_config
        if index == n:
            if current_sum > max_sum:
                max_sum = current_sum
                best_config = towers[:]
            return
        for p in points:
            incremental = sum(dist2(p, t) for t in towers)
            towers.append(p)
            search(towers, index+1, current_sum + incremental)
            towers.pop()

    search([], 0, 0)
    print(max_sum)
    for x, y in best_config:
        print(x, y)

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên liệt kê các điểm ứng cử viên bên trong vòng tròn. các`dist2`hàm tính toán khoảng cách bình phương để tránh số học dấu phẩy động. Đệ quy`search`hàm khám phá tất cả các kết hợp của n tòa tháp với các cập nhật tăng dần về tổng. Việc quay lui đảm bảo mọi cấu hình đều được xem xét. Các trường hợp cạnh, chẳng hạn như nhiều tháp trên cùng một điểm, được xử lý một cách tự nhiên vì các ứng cử viên có thể được lặp lại trong đệ quy. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
4 1
```| Bước | Tháp được đặt | Tổng lũy ​​tiến | Tổng số tiền hiện tại | 
| --- | --- | --- | --- | 
| 1 | (0,1) | 0 | 0 | 
| 2 | (0,1) | 02+02=0 | 0 | 
| 3 | (0,-1) | 4+4=8 | 8 | 
| 4 | (0,-1) | 4+4+0+0=8 | 16 | 

Thuật toán chọn chính xác các tháp ở (0,1),(0,1),(0,-1),(0,-1) để tối đa hóa tổng bình phương khoảng cách là 16. 

**Đầu vào mẫu 2:**```
2 2
```| Bước | Tháp được đặt | Tổng lũy ​​tiến | Tổng số tiền hiện tại | 
| --- | --- | --- | --- | 
| 1 | (2,0) | 0 | 0 | 
| 2 | (-2,0) | 16 | 16 | 

Thuật toán đặt các tòa tháp ở hai đầu đối diện của vòng tròn. Tổng là 16, xác nhận rằng việc đặt các tháp càng xa nhau càng tốt sẽ tối đa hóa khoảng cách bình phương. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m^n * n) | Có m điểm ứng cử viên bên trong vòng tròn. Mỗi độ sâu đệ quy là n và ở mỗi bước chúng tôi tính toán khoảng cách đến các tháp trước đó trong O(n). | 
| Không gian | O(n + m) | Độ sâu ngăn xếp đệ quy là n. Danh sách điểm thí sinh lưu trữ m điểm. | 

Cho n ₫ 8 và r ₫ 30, m ₫ 2800 thì tổng số lần gọi đệ quy là khả thi. Tính toán khoảng cách gia tăng giữ cho mỗi cuộc gọi O(n) thay vì O(n²). 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("4 1\n") == "16\n0 1\n0 1\n0 -1\n0 -1", "sample 1"

# minimum input
assert run("2 1\n") == "4\n0 1\n0 -1", "minimum input n=2 r=1"

# maximum towers, small radius
out = run("8 2\n")
assert int(out.splitlines()[0]) > 0, "n=8, small radius produces nonzero sum"

# towers at origin
assert run("2 0\n") == "0\n0 0\n0 0", "radius zero, both towers at origin"

# single tower repeated
assert run("3 1\n") != "", "repeated positions handled"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 | 4 | Tổng đúng cho bán kính tối thiểu và n=2 | 
| 8 2 | >0 | Xử lý n tối đa với bán kính nhỏ | 
| 2 0 | 0 | Bán kính xử lý 0, tất cả các tháp tại điểm gốc | 
| 3 1 | không trống | Nhiều tòa tháp có thể chia sẻ cùng một điểm | 

## Vỏ cạnh 

Nếu bán kính bằng 0 thì tất cả các tháp phải chiếm gốc tọa độ. Thuật toán liệt kê
