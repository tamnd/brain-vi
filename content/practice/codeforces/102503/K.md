---
title: "CF 102503K - Shoedoku"
description: "Ta có một bảng hình chữ nhật có j hàng và g cột. Chúng ta cần đặt p đôi giày sao cho hai chiếc giày trong mỗi đôi cách nhau đúng c ô theo một trong bốn hướng chính. Không có ô nào có thể chứa hai chiếc giày."
date: "2026-08-05T17:16:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102503
codeforces_index: "K"
codeforces_contest_name: "National Olympiad in Informatics - Philippines (NOI.PH) Online Eliminations 2020"
rating: 0
weight: 102503
solve_time_s: 514
verified: false
draft: false
---

[CF 102503K - Shoedoku](https://codeforces.com/problemset/problem/102503/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8 phút 34 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Ta có một tấm bảng hình chữ nhật có`j`hàng và`g`cột. Chúng ta cần đặt`p`các đôi giày sao cho hai chiếc giày trong mỗi đôi cách nhau một khoảng chính xác`c`các tế bào theo một trong bốn hướng chính. Không có ô nào có thể chứa hai chiếc giày. Câu hỏi đặt ra là liệu hội đồng quản trị có đủ các cặp vị trí độc lập có giá trị hay không. 

Khó khăn đến từ những giới hạn rất lớn. Số lượng ca kiểm thử có thể đạt tới`100000`và mọi chiều đều có thể lớn bằng`10^18`. Bất kỳ giải pháp nào xây dựng bảng, quét tất cả các ô hoặc thậm chí lặp lại trên tất cả các hàng hoặc cột đều không thể thực hiện được. Chúng ta cần tính toán theo thời gian không đổi hoặc logarit chỉ sử dụng số học trên các chiều. 

Một lỗi phổ biến là đếm riêng các cặp ngang và cặp dọc rồi cộng chúng lại. Cơ hội này sẽ được tính gấp đôi vì một ô được sử dụng theo cặp ngang cũng không thể được sử dụng theo cặp dọc. Sự tương tác giữa các hướng là phần chính của vấn đề. 

Một trường hợp cạnh khác xuất hiện khi khoảng cách lớn hơn một chiều. Ví dụ, với đầu vào`1 5 10 1`, không thể kết nối hai ô vì bảng quá hẹp và quá ngắn cho khoảng cách`10`. Câu trả lời đúng là`NAY`. Giải pháp chỉ kiểm tra tổng số ô có thể được chấp nhận không chính xác vì có sẵn 5 ô. 

Trường hợp cạnh thứ hai xảy ra khi kích thước nén là số lẻ. Ví dụ,`3 3 2 5`có chín ô trong thành phần cặn duy nhất. Số lượng cặp tối đa chỉ có`4`, vậy câu trả lời là`NAY`. Một giải pháp bất cẩn có thể cho rằng cứ hai ô có thể tạo thành một cặp và sử dụng`floor(total_cells / 2)`trên toàn cầu mà không xem xét sự chuyển động đó bởi`c`chia bảng thành các thành phần riêng biệt. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ mô hình hóa mọi ô dưới dạng một đỉnh đồ thị. Hai đỉnh sẽ có một cạnh khi các ô của chúng bằng nhau`c`đặt cách nhau theo chiều ngang hoặc chiều dọc. Sau đó, vấn đề trở thành tìm kích thước phù hợp tối đa. Điều này đúng vì mỗi cạnh khớp được chọn sẽ tương ứng với một đôi giày và việc khớp đảm bảo rằng không có ô nào được sử dụng lại. 

Vấn đề là đồ thị có thể chứa tới`10^36`các ô, do đó việc lưu trữ các đỉnh là không thể. Ngay cả trên các bảng nhỏ, việc chạy thuật toán so khớp chung là công việc không cần thiết vì biểu đồ có cấu trúc rất đều đặn. 

Quan sát quan trọng là việc di chuyển chính xác`c`hàng hoặc cột không bao giờ thay đổi modulo hàng của ô`c`hoặc modulo cột`c`. Do đó, các tế bào phân chia thành các thành phần độc lập dựa trên hai phần dư của chúng. Bên trong một thành phần, sau khi nén tọa độ theo hệ số`c`, biểu đồ sẽ trở thành một lưới hình chữ nhật thông thường nơi các ô nén lân cận được kết nối. 

Một đồ thị lưới hình chữ nhật có`a`hàng và`b`các cột có mức khớp tối đa là`floor(a*b/2)`. Nếu số ô là số chẵn thì màu bàn cờ sẽ có các cạnh bằng nhau và tồn tại một kết quả khớp hoàn hảo. Nếu số lượng ô là số lẻ thì tối đa một ô vẫn chưa khớp. 

Nhiệm vụ còn lại là tính tổng`floor(rows_in_residue * cols_in_residue / 2)`trên tất cả các cặp dư lượng mà không lặp lại các dư lượng. Số lượng hàng có dạng đơn giản. Trong số`c`dư lượng hàng có thể, số lượng khác nhau nhiều nhất là một. Điều này cũng đúng với các cột. Chúng ta chỉ cần biết có bao nhiêu nhóm dư lượng có kích thước lớn hơn. 

Cho phép:`A = j // c`,`ar = j % c`là kích thước nhóm hàng cơ sở và số lượng hàng còn lại nhận được một hàng bổ sung. Tương tự,`B = g // c`,`br = g % c`mô tả các cột 

có`ar`nhóm hàng có kích thước`A + 1`Và`c - ar`nhóm kích thước`A`, nhưng chỉ`min(j, c)`các nhóm thực sự tồn tại. Việc điều chỉnh tương tự được áp dụng khi`c`lớn hơn một chiều. 

Tổng có thể được chia thành bốn loại thành phần dư tùy thuộc vào nhóm hàng lớn hay nhỏ và nhóm cột lớn hay nhỏ. Số lượng của bốn kết hợp này là sản phẩm và mỗi thành phần đóng góp một nửa diện tích được làm tròn xuống. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(j * g) đỉnh và công việc khớp | O(j * g) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số nhóm dư hàng và nhóm dư cột thực tế. Nếu như`c`lớn hơn một chiều thì chỉ có bấy nhiêu dư lượng xuất hiện. 
2. Chia nhóm hàng thành hai loại. Các nhóm hàng nhỏ hơn có kích thước`j // c`, Và`j % c`các nhóm là một lớn hơn. Làm tương tự cho các cột. 
3. Tính tổng số cặp phù hợp được đóng góp bởi bốn loại thành phần có thể có. Đối với mỗi danh mục hàng và danh mục cột, hãy nhân số lượng thành phần đó với`floor(component_height * component_width / 2)`. 
4. So sánh số lượng cặp tối đa thu được với`p`. In`YAY`nếu tối đa là ít nhất`p`, nếu không thì in`NAY`. 

Tại sao nó hoạt động: mỗi đôi giày hợp lệ nằm bên trong chính xác một thành phần dư vì cả hai tọa độ đều giữ phần dư của chúng theo modulo`c`. Các thành phần này độc lập nên có thể thêm các kết quả khớp tối đa của chúng. Mỗi thành phần là một lưới hình chữ nhật và lưới hình chữ nhật luôn khớp với tất cả các ô ngoại trừ một ô. Công thức tính toán chính xác số ô trong mỗi loại thành phần nên tổng chính xác là số lượng đôi giày lớn nhất có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(j, g, c, p):
    row_groups = min(j, c)
    col_groups = min(g, c)

    row_small = j // c
    row_big_count = min(j % c, row_groups)
    row_small_count = row_groups - row_big_count

    col_small = g // c
    col_big_count = min(g % c, col_groups)
    col_small_count = col_groups - col_big_count

    ans = 0

    ans += row_small_count * col_small_count * ((row_small * col_small) // 2)
    ans += row_small_count * col_big_count * ((row_small * (col_small + 1)) // 2)
    ans += row_big_count * col_small_count * (((row_small + 1) * col_small) // 2)
    ans += row_big_count * col_big_count * (((row_small + 1) * (col_small + 1)) // 2)

    return "YAY" if ans >= p else "NAY"

def main():
    t = int(input())
    out = []
    for _ in range(t):
        j, g, c, p = map(int, input().split())
        out.append(solve_case(j, g, c, p))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Đầu tiên, mã sẽ tính toán có bao nhiêu lớp dư lượng thực sự xuất hiện. Điều này quan trọng khi`c`lớn hơn`j`hoặc`g`, vì không thể có nhiều nhóm không trống hơn số hàng hoặc cột. 

Các biến`row_big_count`Và`col_big_count`đại diện cho các lớp dư lượng nhận thêm một hàng hoặc cột. Các lớp còn lại có quy mô nhỏ hơn. Bốn sự bổ sung tương ứng trực tiếp với bốn sự kết hợp của các loại này. 

Số nguyên Python xử lý các giá trị trong phạm vi được yêu cầu mà không phải lo lắng về tình trạng tràn. Phép nhân lớn nhất là xung quanh`10^36`, vẫn được hỗ trợ an toàn bởi các số nguyên chính xác tùy ý của Python. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,`j = 1`,`g = 2`,`c = 1`,`p = 1`. 

| nhóm hàng | nhóm cột | cặp tối đa | cần thiết | kết quả | 
| --- | --- | --- | --- | --- | 
| một nhóm cỡ 1 | một nhóm cỡ 2 | 1 | 1 | CÓ | 

Thành phần duy nhất là toàn bộ bảng. Nó chứa hai ô được kết nối bằng khoảng cách một, vì vậy có thể có một cặp. 

Đối với mẫu thứ hai,`j = 3`,`g = 3`,`c = 2`,`p = 3`. 

| nhóm hàng | nhóm cột | khu vực thành phần | cặp tối đa | cần thiết | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| cỡ 2, 1 | cỡ 2, 1 | 4, 2, 2, 1 | 2+1+1+0 = 4 | 3 | CÓ | 

Bảng chia thành bốn thành phần còn lại. Sự kết hợp của chúng là độc lập và chúng cùng nhau cung cấp đủ các cặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ một số phép tính số học cố định được thực hiện | 
| Không gian | O(1) | Không có cấu trúc dữ liệu nào tùy thuộc vào kích thước đầu vào được tạo | 

Với`100000`các trường hợp thử nghiệm, thuật toán chỉ thực hiện vài triệu phép tính số nguyên đơn giản, phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ vẫn không đổi. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_all(inp):
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve_case(j, g, c, p):
        row_groups = min(j, c)
        col_groups = min(g, c)

        row_small = j // c
        row_big_count = min(j % c, row_groups)
        row_small_count = row_groups - row_big_count

        col_small = g // c
        col_big_count = min(g % c, col_groups)
        col_small_count = col_groups - col_big_count

        ans = 0
        ans += row_small_count * col_small_count * ((row_small * col_small) // 2)
        ans += row_small_count * col_big_count * ((row_small * (col_small + 1)) // 2)
        ans += row_big_count * col_small_count * (((row_small + 1) * col_small) // 2)
        ans += row_big_count * col_big_count * (((row_small + 1) * (col_small + 1)) // 2)

        return "YAY" if ans >= p else "NAY"

    t = int(input())
    res = []
    for _ in range(t):
        j, g, c, p = map(int, input().split())
        res.append(solve_case(j, g, c, p))

    sys.stdin = old
    return "\n".join(res)

assert solve_all("""2
1 2 1 1
3 3 2 3
""") == "YAY\nYAY"

assert solve_all("""1
1 1 1 1
""") == "NAY"

assert solve_all("""1
5 5 10 1
""") == "NAY"

assert solve_all("""1
1000000000000000000 1000000000000000000 1 500000000000000000000000000000000000
""") == "YAY"

assert solve_all("""1
3 3 2 5
""") == "NAY"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 1 1`|`YAY`| Cung cấp mẫu và cặp hợp lệ nhỏ nhất có thể | 
|`1 1 1 1`|`NAY`| Một ô không thể tạo thành một cặp | 
|`5 5 10 1`|`NAY`| Khoảng cách lớn hơn cả hai chiều | 
| Hình vuông lớn với`c = 1`|`YAY`| Số học số nguyên lớn | 
|`3 3 2 5`|`NAY`| Giới hạn thành phần dư lượng lẻ | 

## Vỏ cạnh 

cho`1 5 10 1`, thuật toán tính toán một nhóm hàng và năm nhóm cột, nhưng mọi thành phần đều có chiều rộng nhỏ hơn chuyển động cần thiết sau khi nén. Kết quả khớp tối đa được tính toán là 0, vì vậy nó trả về`NAY`. 

Vì`3 3 2 5`, các nhóm dư lượng có kích thước hai và một ở cả hai chiều. Bốn lĩnh vực thành phần là`4`,`2`,`2`, Và`1`, đóng góp`2`,`1`,`1`, Và`0`. Tổng cộng là`4`, nhỏ hơn`5`, do đó thuật toán trả về`NAY`. 

Vì`1 1 1 1`, thành phần duy nhất chứa một ô duy nhất. Đóng góp phù hợp của nó là`floor(1/2) = 0`, ngăn chặn câu trả lời không hợp lệ cho rằng mọi ô đều có thể được ghép nối.
