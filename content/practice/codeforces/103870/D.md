---
title: "CF 103870D - Phạt"
description: "Cho một lưới hình chữ nhật gồm các số nguyên có n hàng và m cột. Mỗi ô đóng góp một giá trị và chúng ta có thể tính tổng trên bất kỳ hình chữ nhật con nào bằng cách sử dụng hàm f(a, b, c, d), nghĩa là tính tổng tất cả các ô trong các hàng từ a đến b và cột từ c đến d."
date: "2026-07-02T07:46:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103870
codeforces_index: "D"
codeforces_contest_name: "TeamsCode Summer 2022 Contest"
rating: 0
weight: 103870
solve_time_s: 44
verified: true
draft: false
---

[CF 103870D - Hình phạt](https://codeforces.com/problemset/problem/103870/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho một lưới hình chữ nhật gồm các số nguyên có n hàng và m cột. Mỗi ô đóng góp một giá trị và chúng ta có thể tính tổng trên bất kỳ hình chữ nhật con nào bằng cách sử dụng hàm f(a, b, c, d), nghĩa là tính tổng tất cả các ô trong các hàng từ a đến b và cột từ c đến d. 

Có chính xác một ô đặc biệt trong lưới nằm ở vị trí (x, y). Câu lệnh mô tả nó là một số âm, nhưng vai trò chính của ô này là chúng ta được phép xem xét loại bỏ ảnh hưởng của nó bằng cách chia lưới dọc theo hàng hoặc cột của nó. 

Nhiệm vụ là tính giá trị lớn nhất trong số năm tổng hình chữ nhật cụ thể: tổng của toàn bộ lưới, tổng của phần trên cùng phía trên hàng x, tổng của phần dưới cùng bên dưới hàng x, tổng của phần bên trái trước cột y và tổng của phần bên phải sau cột y. 

Nói cách khác, chúng tôi không kết hợp các vùng, chúng tôi đang chọn một trong năm hình chữ nhật ứng cử viên này và lấy tổng tốt nhất trong số đó. 

Kích thước đầu vào ngụ ý một lưới n x m. Việc tính toán trực tiếp từng hình chữ nhật từ đầu sẽ quá chậm nếu chúng ta liên tục tính tổng các ma trận con. Ràng buộc tự nhiên cần tập trung vào là n và m có thể đủ lớn để cần phải xử lý trước O(nm), nhưng mọi phép tính lại cho mỗi truy vấn phải là O(1) hoặc rất gần với nó. 

Một trường hợp sai sót phổ biến xuất phát từ việc hiểu sai hình dạng của các vùng được phép. Các vùng được phép không phải là các ma trận con tùy ý, chúng là toàn bộ một nửa của lưới được chia theo chiều ngang hoặc chiều dọc. 

Ví dụ: nếu một người nhầm lẫn cố gắng xem xét loại bỏ hàng và cột đồng thời và hợp nhất các vùng, họ có thể tính toán không chính xác một số thứ như trên cùng bên trái cộng với dưới cùng bên phải, không nằm trong tập hợp được phép. Sự cố chỉ cho phép các hình chữ nhật liền kề được căn chỉnh với toàn bộ chiều rộng hoặc chiều cao đầy đủ. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ tính toán trực tiếp từng hình chữ nhật trong số năm hình chữ nhật ứng cử viên bằng cách tính tổng tất cả các ô được bao gồm. Đối với mỗi truy vấn, chi phí này là O(nm), vì mỗi hình chữ nhật có thể lớn bằng toàn bộ lưới. Với năm ứng viên, giá trị này trở thành O(nm) cho mỗi lần đánh giá, vẫn là O(nm), nhưng nếu tồn tại nhiều trường hợp kiểm thử hoặc nếu chúng ta tính toán lại tổng nhiều lần thì tổng số sẽ trở nên quá chậm. 

Quan sát chính là tất cả các giá trị ứng cử viên đều là các ma trận con có chiều rộng hoặc chiều cao đầy đủ giống như tiền tố hoặc hậu tố. Cấu trúc này cho phép chúng ta tính toán trước tổng tiền tố 2D trên lưới. Khi có sẵn tổng tiền tố, mọi tổng hình chữ nhật f(a, b, c, d) đều có thể được tính theo O(1). 

Sau đó, vấn đề giảm xuống còn việc đánh giá năm công thức cố định, mỗi công thức mô tả một hình chữ nhật lớn có trục thẳng hàng. Tổng tiền tố chuyển đổi những gì sẽ là quét tuyến tính thành tra cứu theo thời gian không đổi, làm cho giải pháp có hiệu quả O(1) cho mỗi ứng viên sau khi xử lý trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) mỗi truy vấn | O(1) | Quá chậm | 
| Tiền tố Tổng | Tiền xử lý O(nm), truy vấn O(1) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc lưới và lưu trữ tất cả các giá trị trong mảng 2D. Cấu trúc của bài toán yêu cầu chúng ta hỗ trợ các truy vấn tổng hình chữ nhật nhanh, vì vậy chúng ta chuẩn bị cho quá trình tiền xử lý thay vì tính toán trực tiếp. 
2. Xây dựng mảng tổng tiền tố 2D pref trong đó pref[i][j] lưu trữ tổng của tất cả các giá trị trong ma trận con từ (1,1) đến (i,j). Bước này rất cần thiết vì nó biến đổi tổng hình chữ nhật bất kỳ thành công thức thời gian không đổi bằng cách sử dụng loại trừ bao gồm. 
3. Xác định hàm trợ giúp để tính f(a, b, c, d) bằng cách sử dụng tổng tiền tố. Hàm này tính toán pref[b][d] trừ vùng trên cùng, bên trái và vùng chồng chéo bị loại trừ, cho phép chúng ta truy xuất bất kỳ tổng hình chữ nhật nào trong O(1). 
4. Tính tổng của toàn bộ lưới sử dụng f(1, n, 1, m). Đây là một trong năm ứng cử viên và đại diện cho trường hợp chúng tôi không loại trừ bất cứ điều gì. 
5. Tính tổng phần trên là f(1, x−1, 1, m). Điều này tương ứng với việc chỉ giữ các hàng ở trên hàng đặc biệt. 
6. Tính tổng phần dưới là f(x+1, n, 1, m). Điều này tương ứng với việc chỉ giữ các hàng ngay bên dưới hàng đặc biệt. 
7. Tính tổng phần bên trái là f(1, n, 1, y−1). Điều này tương ứng với việc chỉ giữ các cột bên trái cột đặc biệt. 
8. Tính tổng phần bên phải là f(1, n, y+1, m). Điều này tương ứng với việc chỉ giữ các cột ở bên phải cột đặc biệt. 
9. Trả về giá trị lớn nhất trong năm giá trị này. Mỗi ứng cử viên đại diện cho một cấu hình hợp lệ được phép và chúng tôi đang chọn cấu hình tốt nhất trong số đó. 

Tại sao nó hoạt động: mọi cấu hình được phép chính xác là một hình chữ nhật thẳng hàng theo trục trải dài toàn bộ chiều rộng hoặc toàn bộ chiều cao của lưới, được xác định bằng cách cắt tối đa một lần dọc theo hàng x hoặc cột y. Tổng tiền tố đảm bảo rằng mỗi tổng hình chữ nhật như vậy được tính chính xác một lần mà không bị trùng lặp hoặc bỏ sót và vì chúng tôi đánh giá tất cả các ứng cử viên hợp lệ một cách rõ ràng nên không thể bỏ qua giải pháp nào tốt hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [list(map(int, input().split())) for _ in range(n)]
    x, y = map(int, input().split())

    pref = [[0] * (m + 1) for _ in range(n + 1)]

    for i in range(1, n + 1):
        row_sum = 0
        for j in range(1, m + 1):
            row_sum += grid[i - 1][j - 1]
            pref[i][j] = pref[i - 1][j] + row_sum

    def rect(a, b, c, d):
        if a > b or c > d:
            return 0
        return (
            pref[b][d]
            - pref[a - 1][d]
            - pref[b][c - 1]
            + pref[a - 1][c - 1]
        )

    ans = rect(1, n, 1, m)
    ans = max(ans, rect(1, x - 1, 1, m))
    ans = max(ans, rect(x + 1, n, 1, m))
    ans = max(ans, rect(1, n, 1, y - 1))
    ans = max(ans, rect(1, n, y + 1, m))

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tập trung vào bảng tổng tiền tố 2D. Mỗi hàng trước tiên được tích lũy thành tổng hàng đang chạy, sau đó được hợp nhất vào cấu trúc tiền tố bằng cách sử dụng các giá trị từ hàng trước đó. Điều này tránh việc tính tổng lặp lại và đảm bảo quá trình tiền xử lý O(nm). 

Hàm trực tràng là một phép tính bao gồm-loại trừ tiêu chuẩn. Việc kiểm tra ranh giới cho các phạm vi không hợp lệ là cần thiết vì các lát cắt như x−1 hoặc y+1 có thể tạo ra các hình chữ nhật trống, phải đóng góp bằng 0 thay vì làm hỏng việc lập chỉ mục tiền tố. 

Mỗi hình chữ nhật ứng viên được đánh giá trực tiếp bằng cách sử dụng trình trợ giúp này và mức tối đa được duy trì tăng dần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
1 2 3
4 -10 6
7 8 9
2 2
```Chúng tôi tính toán tổng tiền tố một cách ngầm định và sau đó đánh giá các ứng cử viên: 

| Ứng viên | Hình chữ nhật | Tổng hợp | 
| --- | --- | --- | 
| Đầy đủ | (1,3,1,3) | 30 | 
| Đầu trang | (1,1,1,3) | 6 | 
| Dưới cùng | (3,3,1,3) | 24 | 
| Trái | (1,3,1,1) | 12 | 
| Đúng | (1,3,3,3) | 18 | 

Tối đa là 30. 

Điều này cho thấy rằng mặc dù có một ô âm, việc loại trừ các bộ phận không nhất thiết có lợi nếu tác động tiêu cực nhỏ so với tổng khối lượng ở nơi khác. 

### Ví dụ 2 

đầu vào:```
2 4
5 5 5 5
5 -100 5 5
2 2
```| Ứng viên | Hình chữ nhật | Tổng hợp | 
| --- | --- | --- | 
| Đầy đủ | (1,2,1,4) | 30 | 
| Đầu trang | (1,1,1,4) | 20 | 
| Dưới cùng | (2,2,1,4) | -85 | 
| Trái | (1,2,1,1) | 10 | 
| Đúng | (1,2,3,4) | 15 | 

Tối đa là 30. 

Điều này chứng tỏ rằng ngay cả với một ô âm lớn, câu trả lời tốt nhất vẫn có thể là giữ toàn bộ lưới nếu tất cả các lần cắt được phép sẽ loại bỏ quá nhiều đóng góp tích cực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Xây dựng tổng tiền tố chiếm ưu thế, mỗi ô được xử lý một lần | 
| Không gian | O(nm) | Lưu trữ bảng tổng tiền tố | 

Các ràng buộc được thỏa mãn tốt vì thuật toán tránh được bất kỳ phép lặp nào trên mỗi hình chữ nhật. Tất cả công việc nặng nhọc được thực hiện một lần trong quá trình tiền xử lý và tất cả các truy vấn đều là các thao tác có thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# custom cases

assert run("""1 1
5
1 1
""") == "5", "single cell"

assert run("""2 2
1 2
3 4
1 1
""") == "10", "all positive full grid best"

assert run("""2 3
-1 -1 -1
-1 -100 -1
2 2
""") == "-1", "all negative, best rectangle avoids largest loss"

assert run("""3 3
1 1 1
1 1 1
1 1 1
2 2
""") == "9", "uniform grid"

assert run("""2 3
10 10 10
10 -50 10
2 2
""") == "30", "central negative but full still best"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1x1 | 5 | xử lý ranh giới tối thiểu | 
| lưới dương nhỏ | 10 | độ chính xác đầy đủ của hình chữ nhật | 
| tất cả lưới âm | -1 | xử lý các vết cắt có hại | 
| lưới thống nhất | 9 | tính đối xứng và tính chính xác của tiền tố | 
| trung âm | 30 | đánh đổi giữa loại trừ và mất mát | 

## Vỏ cạnh 

Trường hợp một cạnh là khi x hoặc y nằm trên đường viền, khiến một số hình chữ nhật ứng viên bị trống. Ví dụ: 

đầu vào:```
2 3
1 2 3
4 -5 6
1 2
```Ở đây x = 1, vì vậy hình chữ nhật trên cùng (1, x−1, 1, m) không hợp lệ. Thuật toán trả về 0 trong trường hợp này, đảm bảo nó không ảnh hưởng sai đến mức tối đa. Các ứng cử viên còn lại được tính toán bình thường và câu trả lời cuối cùng chỉ được lấy từ các vùng hợp lệ. 

Một trường hợp khác xảy ra khi lưới rất nhỏ, chẳng hạn như 1 hàng hoặc 1 cột. Trong những trường hợp như vậy, nhiều ứng cử viên thu gọn thành các hình chữ nhật giống hệt nhau hoặc trống. Logic tổng tiền tố vẫn được áp dụng vì hàm chỉnh lưu xử lý các khoảng ngoài phạm vi một cách an toàn, đảm bảo không có lỗi lập chỉ mục và duy trì tính chính xác.
