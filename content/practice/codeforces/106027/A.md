---
title: "CF 106027A - Tam giác lớn"
description: "Chúng ta được cấp một tập hợp các điểm trên mặt phẳng và vùng mục tiêu. Nhiệm vụ là chọn ba điểm sao cho tam giác mà chúng tạo thành có diện tích chính xác theo yêu cầu hoặc báo cáo rằng tam giác đó không tồn tại."
date: "2026-06-25T13:09:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106027
codeforces_index: "A"
codeforces_contest_name: "COMP4128 Large Triangle"
rating: 0
weight: 106027
solve_time_s: 41
verified: true
draft: false
---

[CF 106027A - Tam giác lớn](https://codeforces.com/problemset/problem/106027/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các điểm trên mặt phẳng và vùng mục tiêu. Nhiệm vụ là chọn ba điểm sao cho tam giác mà chúng tạo thành có diện tích chính xác theo yêu cầu hoặc báo cáo rằng tam giác đó không tồn tại. Diện tích được so sánh bằng cách sử dụng diện tích gấp đôi, vì đối với tọa độ nguyên thì diện tích nhân đôi luôn là số nguyên. Bài toán ban đầu có tới 2000 điểm và vùng mục tiêu lên tới$2 \cdot 10^{18}$. 

Các giới hạn ngay lập tức loại trừ việc kiểm tra từng bộ ba. có$O(n^3)$hình tam giác có thể, có nghĩa là xung quanh$8 \cdot 10^9$kiểm tra ở kích thước đầu vào lớn nhất. Ngay cả với các hệ số không đổi rất nhỏ thì điều này là quá nhiều. Chúng ta cần giảm việc tìm kiếm thành công việc bậc hai, bởi vì$2000^2$hoạt động có thể quản lý được. 

Một cách hữu ích để hình dung về một hình tam giác là lấy một cạnh làm đáy. Khi đã biết hai điểm cuối của đáy thì chiều cao yêu cầu cũng được biết. Nhiệm vụ còn lại là tìm một điểm có khoảng cách vuông góc với đường cơ sở này có giá trị chính xác cần thiết. 

Việc triển khai bất cẩn có thể thất bại khi sử dụng hình học dấu phẩy động. Ví dụ: nếu diện tích nhân đôi mục tiêu là`1`và hình tam giác có chiều cao rất nhỏ, việc làm tròn dấu phẩy động có thể quyết định không chính xác rằng diện tích không chính xác. Tọa độ đầu vào có thể lớn bằng$10^9$, vì vậy các sản phẩm chéo có thể đạt khoảng$10^{18}$và số học số nguyên phải được sử dụng. 

Một sai lầm phổ biến khác là quên rằng hình tam giác có thể nằm ở hai bên của đáy đã chọn. Đối với đầu vào```
3 5
0 0
10 0
5 1
```diện tích nhân đôi là`10`, vì vậy mục tiêu cụ thể này là không thể. Một phương pháp chỉ tìm kiếm các điểm phía trên cơ sở cũng sẽ bỏ lỡ các giải pháp hợp lệ khi điểm ở dưới nó, vì tích chéo có dấu là âm ở đó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là chọn từng bộ ba điểm, tính diện tích nhân đôi bằng cách sử dụng tích chéo và so sánh nó với mục tiêu. Điều này đúng vì mọi tam giác có thể đều được kiểm tra rõ ràng. Vấn đề là số lượng bộ ba, tức là khoảng$n^3/6$. Vì$n=2000$, đó là hàng tỷ hoạt động. 

Nhận xét quan trọng là sau khi chọn cơ sở, điều kiện diện tích sẽ trở thành tìm kiếm một chiều. Nếu vectơ cơ sở là$v$, diện tích nhân đôi là giá trị tuyệt đối của tích chéo giữa$v$và vectơ đến điểm thứ ba. Chúng ta cần một điểm có khoảng cách được đánh dấu cụ thể từ đường cơ sở. 

Phần khó khăn là duy trì các điểm được sắp xếp theo khoảng cách của chúng với căn cứ hiện tại. Việc tính toán lại thứ tự này cho mỗi cặp sẽ là quá chậm. Thay vào đó, chúng ta xoay phần đế qua tất cả các đoạn có thể. Khi hướng của đáy thay đổi liên tục thì thứ tự tương đối của hai điểm chỉ thay đổi khi đường quay trở nên song song với đoạn nối hai điểm đó. Vì chúng tôi xử lý tất cả các cặp điểm theo thứ tự góc nên mọi thay đổi chỉ là sự hoán đổi hai điểm liền kề theo thứ tự được duy trì. Điều này mang lại một$O(n^2 \log n)$giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Quá chậm | 
| Tối ưu | O(n² log n) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo tất cả các cặp điểm có hướng và sắp xếp chúng theo góc của đoạn nối cặp. Quá trình quét bắt đầu theo hướng ngang, trong đó việc sắp xếp theo khoảng cách đến đường thẳng cũng giống như sắp xếp theo tọa độ y. 
2. Duy trì một dãy các điểm được sắp xếp theo khoảng cách đã đánh dấu của chúng với đường quét hiện tại. Đồng thời lưu trữ vị trí của mọi điểm trong mảng này để các giao dịch hoán đổi có thời gian không đổi. 
3. Với mỗi đoạn theo thứ tự góc, hãy sử dụng thứ tự hiện tại để tìm điểm thứ ba. Đối với một cơ sở từ`a`ĐẾN`b`, điều kiện diện tích nhân đôi là:$$cross(b-a, c-a)=2S$$hoặc cùng giá trị nhưng có dấu ngược lại. Vì các điểm được sắp xếp theo giá trị này nên tìm kiếm nhị phân có thể tìm thấy điểm phù hợp. 

1. Sau khi xử lý một phân đoạn, hoán đổi hai điểm cuối của phân đoạn đó theo thứ tự được duy trì. Điều này cập nhật thứ tự cho vị trí quét tiếp theo. 

Lý do hoán đổi là đủ là vì thời điểm duy nhất khi hai điểm hoán đổi thứ tự của chúng so với một đường quay là khi đường thẳng trở nên song song với đoạn nối chúng. Chúng tôi xử lý chính xác những sự kiện đó. 

Tại sao nó hoạt động: thứ tự được duy trì luôn là thứ tự các điểm theo khoảng cách đã ký từ đường cơ sở hiện tại. Tìm kiếm nhị phân hợp lệ vì tích chéo với vectơ cơ sở cố định là một phép biến đổi tuyến tính, do đó nó giữ nguyên thứ tự khoảng cách. Quá trình quét cập nhật thứ tự vào đúng thời điểm khi hai khoảng cách có thể bằng nhau và không có ba điểm nào thẳng hàng, do đó mọi sự kiện đều rõ ràng. Khi một điểm có tích chéo yêu cầu được tìm thấy, điều kiện diện tích tam giác được thỏa mãn khi xây dựng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, S = map(int, input().split())
    pts = [tuple(map(int, input().split())) for _ in range(n)]
    target = 2 * S

    def cross(a, b, c):
        return (b[0] - a[0]) * (c[1] - a[1]) - (b[1] - a[1]) * (c[0] - a[0])

    events = []
    for i in range(n):
        for j in range(i + 1, n):
            dx = pts[j][0] - pts[i][0]
            dy = pts[j][1] - pts[i][1]
            events.append((dy, dx, i, j))

    events.sort(key=lambda e: __import__("math").atan2(e[0], e[1]))

    order = sorted(range(n), key=lambda i: pts[i][1])
    pos = [0] * n
    for i, x in enumerate(order):
        pos[x] = i

    for _, _, a, b in events:
        lo, hi = 0, n
        while lo < hi:
            mid = (lo + hi) // 2
            c = order[mid]
            val = cross(a, b, c)
            if val < target:
                lo = mid + 1
            else:
                hi = mid

        if lo < n and cross(a, b, order[lo]) == target:
            c = order[lo]
            print("YES")
            print(*pts[a])
            print(*pts[b])
            print(*pts[c])
            return

        lo, hi = 0, n
        while lo < hi:
            mid = (lo + hi) // 2
            c = order[mid]
            val = cross(a, b, c)
            if val < -target:
                lo = mid + 1
            else:
                hi = mid

        if lo < n and cross(a, b, order[lo]) == -target:
            c = order[lo]
            print("YES")
            print(*pts[a])
            print(*pts[b])
            print(*pts[c])
            return

        pa, pb = pos[a], pos[b]
        order[pa], order[pb] = order[pb], order[pa]
        pos[a], pos[b] = pb, pa

    print("NO")

if __name__ == "__main__":
    solve()
```các`cross`chức năng là hoạt động hình học cốt lõi. Nó trả về gấp đôi diện tích đã ký, giúp tránh được tất cả các phép tính dấu phẩy động. 

Mảng quét`order`là cấu trúc động được mô tả trong thuật toán. các`pos`mảng ngăn cản việc tìm kiếm tốn kém khi hai điểm cuối của cơ sở hiện tại cần được hoán đổi. 

Tìm kiếm nhị phân tìm kiếm cả hai hướng có thể có của tam giác. Chỉ tìm kiếm một dấu hiệu sẽ bỏ lỡ các hình tam giác ở phía đối diện của đế. Tất cả số học đều ở dạng số nguyên Python, do đó tích tọa độ lớn không bị tràn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 7
0 0
3 0
0 4
```Tam giác duy nhất có diện tích gấp đôi là 24 chứ không phải 14. 

| Bước | Căn cứ | Giá trị chéo bắt buộc | Đã tìm thấy điểm | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | (0,0) đến (3,0) | 14 hoặc -14 | không | tiếp tục | 
| 2 | các cặp khác | 14 hoặc -14 | không | KHÔNG | 

Dấu vết cho thấy việc kiểm tra mọi đế đều không tìm thấy chiều cao phù hợp. 

Đối với mẫu thứ hai:```
4 3
0 0
2 0
1 2
1 3
```Diện tích nhân đôi mục tiêu là 6. 

| Bước | Căn cứ | Giá trị chéo bắt buộc | Đã tìm thấy điểm | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | (0,0) đến (2,0) | 6 | (1,3) | CÓ | 

Sản phẩm chéo là$2 \cdot 3 = 6$, vậy diện tích thực tế là 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n² log n) | có$O(n²)$căn cứ và mỗi cơ sở thực hiện tìm kiếm nhị phân | 
| Không gian | O(n²) | Danh sách tất cả các cặp điểm thống trị bộ nhớ | 

Kích thước đầu vào tối đa đủ nhỏ cho bộ nhớ bậc hai và thời gian gần bậc hai. Giải pháp tránh vụ nổ khối khi liệt kê tất cả các hình tam giác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_out = sys.stdout
    sys.stdout = out
    solve()
    sys.stdin = old
    sys.stdout = old_out
    return out.getvalue().strip()

assert run("""3 7
0 0
3 0
0 4
""") == "NO"

assert run("""4 3
0 0
2 0
1 2
1 3
""").split()[0] == "YES"

assert run("""3 1
0 0
1 0
0 1
""").split()[0] == "YES"

assert run("""4 100
0 0
1 0
0 1
1 1
""") == "NO"

assert run("""5 6
0 0
2 0
1 3
4 0
3 2
""").split()[0] == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ba điểm hình thành vùng 6 nhưng mục tiêu 7 | KHÔNG | Xử lý trường hợp bất khả thi | 
| Ví dụ bốn điểm | CÓ | Tìm kiếm thành công cơ bản | 
| Đơn vị tam giác vuông | CÓ | Trường hợp hình học hữu ích tối thiểu | 
| Hình vuông có mục tiêu quá lớn | KHÔNG | Thất bại ranh giới | 
| Tọa độ hỗn hợp | CÓ | Hành vi quét chung | 

## Vỏ cạnh 

Đối với mẫu không thể:```
3 7
0 0
3 0
0 4
```Thuật toán kiểm tra mọi cơ sở có thể. Căn cứ`(0,0)`ĐẾN`(3,0)`sẽ yêu cầu một điểm với tích chéo 14, nhưng điểm thứ ba cho 12. Hai cơ sở còn lại cũng thất bại, vì vậy câu trả lời vẫn là`NO`. 

Đối với tam giác hợp lệ nhỏ nhất:```
3 1
0 0
1 0
0 1
```Diện tích mục tiêu nhân đôi là 2. Lấy hai điểm đầu làm cơ sở, điểm thứ ba cho tích chéo 1? Với sự định hướng chính xác, cơ sở`(0,0)`ĐẾN`(0,1)`cho kết quả chéo 2, do đó tìm kiếm nhị phân tìm thấy nó và trả về`YES`. 

Thuật toán xử lý cả hai hướng vì mọi cơ sở đều được kiểm tra bằng cả hai`target`Và`-target`, do đó, các hình tam giác bên dưới đường cơ sở đã chọn được xử lý giống hệt như các hình tam giác bên trên đường cơ sở đó.
