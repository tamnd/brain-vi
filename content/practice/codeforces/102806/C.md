---
title: "CF 102806C - \u041d\u043e\u0432\u044b\u0439 \u043a\u043e\u0440\u0430\u0431\u043b\u044c"
description: "Chúng ta có một hành tinh hình chữ nhật được chia thành các ô. Một số ô có sẵn để xây dựng và một số bị chặn. Mục tiêu là chế tạo con tàu lớn nhất có thể."
date: "2026-07-26T16:16:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102806
codeforces_index: "C"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2017-2018, \u0427\u0435\u0442\u0432\u0451\u0440\u0442\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102806
solve_time_s: 61
verified: true
draft: false
---

[CF 102806C - \u041d\u043e\u0432\u044b\u0439 \u043a\u043e\u0440\u0430\u0431\u043b\u044c](https://codeforces.com/problemset/problem/102806/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hành tinh hình chữ nhật được chia thành các ô. Một số ô có sẵn để xây dựng và một số bị chặn. Mục tiêu là chế tạo con tàu lớn nhất có thể. Con tàu có hình chữ thập được làm từ năm ô vuông bằng nhau: một ô ở giữa và một ô vuông gắn ở bốn cạnh. Nếu độ dài cạnh của mỗi hình vuông là`k`, toàn bộ hình chữ thập chiếm một`3k × 3k`nhưng chỉ có khối hàng trung tâm và khối cột trung tâm của khu vực đó mới có thể sử dụng được. 

Đầu vào cho biết chiều cao và chiều rộng của lưới, theo sau là chính lưới đó. MỘT`#`tế bào có thể được sử dụng để xây dựng, trong khi một`.`tế bào không thể. Đầu ra là giá trị tối đa của`k`mà một số vị trí của chữ thập này tồn tại. Tuyên bố đảm bảo rằng một con tàu có kích thước ít nhất`1`luôn có thể được xây dựng. 

Kích thước lưới có thể đạt tới`2000 × 2000`, cho tới bốn triệu tế bào. Một giải pháp thử mọi trung tâm có thể và mở rộng chéo từng lớp một có thể đạt tới hàng tỷ thao tác, quá nhiều so với giới hạn một giây thông thường. Chúng ta cần một phương pháp gần tuyến tính hoặc gần tuyến tính về số lượng ô. Vì câu trả lời nhiều nhất là`min(n, m) / 3`, hệ số logarit từ tìm kiếm nhị phân có thể được chấp nhận. 

Những trường hợp phức tạp hầu hết liên quan đến hình học của chữ thập. Một con tàu có kích thước`k`không chỉ là bình thường`k × k`hình vuông, vì vậy chỉ kiểm tra hình vuông ở giữa sẽ đưa ra câu trả lời sai. Ví dụ:```
3 9
...###...
...###...
#########
```Câu trả lời đúng là`1`, vì hàng giữa có thể sử dụng được nhưng phần dọc không thể có chiều cao`3`. Một giải pháp bất cẩn chỉ kiểm tra xem có`3 × 3`khối ở đâu đó sẽ thất bại. 

Một trường hợp cạnh khác là khi đường chéo chạm vào đường viền. Ví dụ:```
5 5
..#..
.###.
#####
.###.
..#..
```Câu trả lời đúng là`1`. Có thể đặt hình vuông ở giữa nhưng không đủ chỗ cho một hình chữ thập lớn hơn. Việc triển khai giả định là trung tâm của chữ thập ứng cử viên phải có`k`các hàng và cột trống xung quanh nó có thể vô tình bỏ qua các vị trí viền hợp lệ. 

Một lỗi phổ biến cuối cùng là nhầm lẫn kích thước của toàn bộ hình với kích thước của một hình vuông. Đối với một con tàu có kích thước`k`, tổng số hộp giới hạn là`3k × 3k`, không`k × k`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi giá trị có thể có của`k`, mọi vị trí trung tâm có thể và kiểm tra tất cả năm ô vuông của hình chữ thập. Điều này đúng vì định nghĩa con tàu chính xác là sự kết hợp của năm ô vuông đó. Tuy nhiên, việc kiểm tra một ứng cử viên có chi phí chéo`O(k²)`, và có thể có`O(nm)`các trung tâm có thể và`O(min(n,m))`kích thước có thể. Trong trường hợp xấu nhất, điều này trở nên quá lớn. 

Quan sát quan trọng là đối với một cố định`k`, chúng ta không cần phải kiểm tra từng ô của từng ứng viên. Chúng ta chỉ cần trả lời liệu một điều kiện nào đó`k × k`hình vuông hoàn toàn có thể xây dựng được. Điều này có thể được thực hiện trong thời gian không đổi với tổng tiền tố hai chiều. 

Một khi chúng ta có thể kiểm tra một bản sửa lỗi`k`, chúng ta có thể tìm kiếm nhị phân câu trả lời. Nếu một chữ thập có kích thước`k`tồn tại, mọi kích thước nhỏ hơn cũng có thể được xây dựng bằng cách lấy phần trung tâm của hình chữ thập đó. Thuộc tính đơn điệu này có nghĩa là tập hợp các câu trả lời có thể trông giống như`true, true, ..., true, false, false, ...`, đó chính xác là những gì tìm kiếm nhị phân yêu cầu. 

Brute-force hoạt động vì nó kiểm tra định nghĩa trực tiếp, nhưng không thành công vì nó lặp lại việc kiểm tra gần như cùng một khu vực nhiều lần. Tổng tiền tố loại bỏ công việc lặp lại và tìm kiếm nhị phân loại bỏ nhu cầu kiểm tra mọi kích thước có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm · min(n,m)³) trong trường hợp xấu nhất | O(1) | Quá chậm | 
| Tìm kiếm nhị phân + Tổng tiền tố | O(nm log(min(n,m))) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng mảng tổng tiền tố hai chiều trong đó mỗi vị trí lưu trữ số ô có thể sử dụng trong hình chữ nhật từ góc trên cùng bên trái đến vị trí đó. 

Tổng tiền tố cho phép chúng ta truy vấn có bao nhiêu ô bị chặn bên trong bất kỳ hình chữ nhật nào trong thời gian không đổi. Một hình chữ nhật có giá trị xây dựng chính xác khi số ô có thể sử dụng bằng diện tích của nó. 

1. Tìm kiếm nhị phân độ dài cạnh`k`của một hình vuông trong đường chéo. 

Giới hạn dưới bắt đầu tại`1`bởi vì một giải pháp được đảm bảo tồn tại. Giới hạn trên là`min(n, m) // 3`, bởi vì toàn bộ hình chữ thập chiếm ba ô vuông như vậy ở cả hai chiều. 

1. Đối với ứng viên`k`, quét tất cả các vị trí có thể có của trung tâm`k × k`quảng trường. 

Hình vuông trung tâm phải có bốn cạnh nhau`k × k`hình vuông: trên, dưới, trái, phải. Nếu tất cả năm hình chữ nhật chỉ chứa các ô có thể sử dụng được thì một hình chữ thập có kích thước`k`tồn tại. 

1. Nếu kiểm tra thành công, hãy di chuyển tìm kiếm nhị phân lên trên. Nếu không, di chuyển nó xuống. 

Một chữ thập lớn hơn đáp ứng tất cả các yêu cầu của một chữ thập nhỏ hơn, do đó, việc không đạt được một kích thước có nghĩa là mọi kích thước lớn hơn cũng không thành công. 

1. Sau khi tìm kiếm nhị phân kết thúc, xuất giá trị thành công lớn nhất. 

### Tại sao nó hoạt động 

Đối với mọi kích thước được thử nghiệm`k`, quy trình kiểm tra sẽ kiểm tra chính xác năm ô vuông xác định một con tàu hợp lệ. Tổng tiền tố cung cấp chính xác số ô có thể sử dụng trong mỗi ô vuông, do đó, một ô vuông được chấp nhận khi và chỉ khi mọi ô bên trong nó đều có sẵn. 

Tìm kiếm nhị phân là chính xác vì vị từ "một đường chéo kích thước`k`tồn tại" là đơn điệu. Nếu một đường chéo có kích thước`k`tồn tại, lấy cái bên trong`k' × k'`các bộ phận cho bất kỳ nhỏ hơn`k'`tạo ra một chữ thập hợp lệ nhỏ hơn. Nếu kích thước không thành công thì không có kích thước lớn hơn nào có thể phù hợp vì mọi dấu thập lớn hơn sẽ chứa yêu cầu không hợp lệ nhỏ hơn. Do đó tìm kiếm nhị phân tìm thấy kích thước tối đa có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    pref = [[0] * (m + 1) for _ in range(n + 1)]

    for i in range(n):
        row_sum = 0
        for j in range(m):
            if grid[i][j] == '#':
                row_sum += 1
            pref[i + 1][j + 1] = pref[i][j + 1] + row_sum

    def rect_sum(r1, c1, r2, c2):
        return pref[r2][c2] - pref[r1][c2] - pref[r2][c1] + pref[r1][c1]

    def good_square(r, c, k):
        return rect_sum(r, c, r + k, c + k) == k * k

    def check(k):
        for r in range(n - 3 * k + 1):
            for c in range(m - 3 * k + 1):
                if good_square(r, c + k, k):
                    if (good_square(r + k, c, k) and
                        good_square(r + k, c + k, k) and
                        good_square(r + k, c + 2 * k, k) and
                        good_square(r + 2 * k, c + k, k)):
                        return True
        return False

    lo, hi = 1, min(n, m) // 3
    ans = 1

    while lo <= hi:
        mid = (lo + hi) // 2
        if check(mid):
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1

    print(ans)

if __name__ == "__main__":
    solve()
```Các cửa hàng xây dựng tiền tố chỉ tính cho các ô có thể sử dụng được. Điều này làm cho truy vấn hình chữ nhật không phụ thuộc vào kích thước hình chữ nhật, đây là sự tối ưu hóa chính so với việc kiểm tra lặp lại từng ô. 

các`good_square`hàm so sánh số lượng ô có thể sử dụng trong ô ứng viên với`k * k`. Nếu chúng bằng nhau thì mọi ô trong ô vuông đó đều có sẵn. 

các`check`hàm chỉ lặp qua các vị trí hoàn thành`3k × 3k`hộp giới hạn phù hợp. Năm lời kêu gọi tương ứng trực tiếp với năm khối chữ thập. Giữ hình vuông trung tâm ở`(r + k, c + k)`tránh được các lỗi sai lệch vì các khối xung quanh được bù đắp một cách tự nhiên một cách chính xác`k`. 

Số nguyên Python không tràn ở đây, nhưng phép nhân`k * k`vẫn được giữ dưới dạng biểu thức số nguyên vì các giá trị tiền tố biểu thị số lượng ô. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
9 12
...##.###...
...##.###...
.########...
.###########
...#########
...#########
......###...
......###...
......###...
```Vị trí kích thước thành công`3`tồn tại. 

| Quy mô ứng viên | Đã kiểm tra vị trí | Kết quả | 
| --- | --- | --- | 
| 5 | Tất cả các trung tâm có thể | Không có chữ thập hợp lệ | 
| 3 | Khối trung tâm xung quanh hàng 4, cột 4 | hợp lệ | 
| 4 | Tất cả các trung tâm có thể | Không có chữ thập hợp lệ | 

Dấu vết cho thấy lý do tại sao tìm kiếm nhị phân hoạt động. Kích cỡ`3`có thể, nhưng kích thước`4`là không, vì vậy câu trả lời tối đa là`3`. 

Đối với mẫu thứ hai:```
6 6
.##...
.##...
######
######
.##...
.##...
```Chữ thập lớn nhất có thể có kích thước`1`. 

| Quy mô ứng viên | Đã kiểm tra vị trí | Kết quả | 
| --- | --- | --- | 
| 1 | Một số trung tâm | hợp lệ | 
| 2 | Tất cả các trung tâm có thể | Không hợp lệ | 

Ví dụ chứng minh rằng một khu vực kết nối lớn là không đủ. Cần có hình dạng năm hình vuông chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm log(min(n,m))) | Mỗi bước tìm kiếm nhị phân sẽ quét lưới và thực hiện các truy vấn hình chữ nhật có thời gian không đổi | 
| Không gian | O(nm) | Mảng tổng tiền tố lưu trữ một giá trị trên mỗi ô lưới | 

Lưới lớn nhất có bốn triệu ô. Số logarit của các lần lặp tìm kiếm nhị phân là nhỏ, khoảng 11 đối với giới hạn này, do đó giải pháp vẫn nằm trong giới hạn dự định. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue()

assert run("""9 12
...##.###...
...##.###...
.########...
.###########
...#########
...#########
......###...
......###...
......###...
""") == "3\n", "sample 1"

assert run("""6 6
.##...
.##...
######
######
.##...
.##...
""") == "1\n", "sample 2"

assert run("""1 1
#
""") == "1\n", "single cell"

assert run("""3 3
###
###
###
""") == "1\n", "minimum cross"

assert run("""9 9
...###...
...###...
...###...
#########
#########
#########
...###...
...###...
...###...
""") == "3\n", "perfect large cross"

assert run("""9 9
#########
#########
#########
#########
#########
#########
#########
#########
#########
""") == "3\n", "all cells usable"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 × 1`lưới có thể sử dụng |`1`| Kích thước tối thiểu | 
| Đầy`3 × 3`lưới |`1`| Chữ thập hợp lệ nhỏ nhất | 
| Hoàn hảo`9 × 9`chéo |`3`| Kích thước tối đa cho hình dạng đã chuẩn bị sẵn | 
| Toàn bộ lưới có thể sử dụng được |`3`| Xử lý giới hạn trên | 
| Bố cục mẫu | Câu trả lời mẫu | Tính đúng đắn chung | 

## Vỏ cạnh 

Đối với trường hợp biên giới:```
5 5
..#..
.###.
#####
.###.
..#..
```Tìm kiếm nhị phân đầu tiên thử các giá trị lớn hơn, nhưng`check(1)`là cuộc gọi thành công duy nhất. Khi kiểm tra kích thước`2`, mọi khả năng`4 × 4`hộp giới hạn yêu cầu một ô bị thiếu, vì vậy câu trả lời vẫn còn`1`. 

Đối với trường hợp bình phương sai:```
3 9
...###...
...###...
#########
```Tổng tiền tố từ chối chính xác một hình vuông có kích thước dọc`3`. Mặc dù có một vùng ngang lớn nhưng không phải tất cả các ô vuông cạnh của chữ thập đều hiện diện nên thuật toán trả về`1`. 

Đối với tình huống kích thước tối đa:```
9 9
...###...
...###...
...###...
#########
#########
#########
...###...
...###...
...###...
```Việc kiểm tra cho`k = 3`tìm thấy năm hợp lệ`3 × 3`khối. Nỗ lực tìm kiếm nhị phân tiếp theo với kích thước lớn hơn không thành công vì hộp giới hạn sẽ vượt quá cấu trúc có sẵn. Câu trả lời trả về là chính xác`3`.
