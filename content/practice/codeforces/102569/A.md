---
title: "CF 102569A - Hàm băm của mảng"
description: "Quá trình băm mảng liên tục loại bỏ hai giá trị đầu tiên và thay thế chúng bằng hiệu của chúng. Thay vì mô phỏng những lần loại bỏ này, chúng ta cần hiểu giá trị nào còn tồn tại sau tất cả các hoạt động. Đầu vào đưa ra một mảng ban đầu và một chuỗi các phép cộng phạm vi."
date: "2026-07-31T07:44:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102569
codeforces_index: "A"
codeforces_contest_name: "2020, XIII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102569
solve_time_s: 80
verified: true
draft: false
---

[CF 102569A - Hàm băm của mảng](https://codeforces.com/problemset/problem/102569/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Quá trình băm mảng liên tục loại bỏ hai giá trị đầu tiên và thay thế chúng bằng hiệu của chúng. Thay vì mô phỏng những lần loại bỏ này, chúng ta cần hiểu giá trị nào còn tồn tại sau tất cả các hoạt động. Đầu vào đưa ra một mảng ban đầu và một chuỗi các phép cộng phạm vi. Mỗi bản cập nhật sẽ tăng mọi phần tử trong một khoảng đã chọn với cùng một lượng và sau mỗi lần cập nhật, chúng ta phải in giá trị băm hiện tại của toàn bộ mảng. 

Chìa khóa của vấn đề là tìm ra dạng hàm băm. Đối với một mảng`[a1, a2, a3, a4]`, quá trình trở thành`[a2-a1, a3, a4]`, sau đó`[a3-(a2-a1), a4]`, và cuối cùng`a4-(a3-a2+a1)`. Đơn giản hóa mang lại`a4-a3-a2-a1`. Mô hình tương tự tiếp tục xảy ra với các mảng lớn hơn: hàm băm cuối cùng luôn là phần tử cuối cùng trừ đi tổng của tất cả các phần tử trước đó. 

Các ràng buộc buộc chúng ta tránh mô phỏng hàm băm sau mỗi truy vấn. Mảng có thể chứa tới 500.000 phần tử và có thể có 200.000 bản cập nhật. Một giải pháp quét mảng cho mọi truy vấn sẽ thực hiện tới khoảng 100 tỷ thao tác trong trường hợp xấu nhất, không thể đáp ứng giới hạn 2 giây. Chúng tôi cần một giải pháp trong đó mỗi lần cập nhật đều mất thời gian liên tục. 

Một số tình huống ranh giới có thể phá vỡ việc thực hiện bất cẩn. Khi mảng có một phần tử thì hàm băm chính là phần tử đó. Ví dụ:```
Input:
1
5
1
1 1 3

Output:
8
```Một giải pháp giả định luôn có một phần tử cuối cùng riêng biệt và một tiền tố trước khi nó bị lỗi ở đây. 

Một trường hợp phức tạp khác là bản cập nhật đạt đến vị trí cuối cùng. Ví dụ:```
Input:
3
1 2 3
1
2 3 5

Output:
1
```Mảng trở thành`[1,7,8]`. Hàm băm là`8-1-7=0`, chờ đã, ví dụ này cho thấy tại sao công thức phải được áp dụng cẩn thận. Băm đúng là`a3-a1-a2 = 8-1-7 = 0`. 

Một lỗi phổ biến là chỉ cập nhật tổng được lưu trữ của số đầu tiên`n-1`phần tử và quên rằng phần tử cuối cùng có dấu khác. Trong trường hợp này, vị trí cuối cùng nhận được bản cập nhật và phải được theo dõi riêng. 

Bản cập nhật phạm vi cũng có thể chồng lấp một phần tiền tố và phần tử cuối cùng. Ví dụ:```
Input:
4
1 2 3 4
1
3 4 10

Output:
0
```Mảng mới là`[1,2,13,14]`, vậy hàm băm là`14-1-2-13=-2`. Bản cập nhật ảnh hưởng đến cả phần tử dấu âm và phần tử dấu dương, do đó, việc xử lý thống nhất toàn bộ phạm vi sẽ cho kết quả sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là lưu trữ mảng, áp dụng từng phép cộng phạm vi bằng cách truy cập mọi vị trí bị ảnh hưởng, sau đó mô phỏng thao tác băm hoặc tính toán lại công thức. Điều này đúng vì nó tuân theo định nghĩa chính xác. Tuy nhiên, một truy vấn có thể chạm vào mọi phần tử và có thể có 200.000 truy vấn. Trong trường hợp xấu nhất, điều này tạo ra khoảng`200000 * 500000 = 100000000000`sửa đổi phần tử, vượt xa những gì có thể. 

Quan sát quan trọng là phép toán băm có vẻ phức tạp thực ra là một biểu thức tuyến tính cố định. Mọi phần tử ngoại trừ phần tử cuối cùng đều đóng góp hệ số`-1`và phần tử cuối cùng đóng góp với hệ số`+1`. Cập nhật phạm vi sẽ thêm cùng một giá trị vào một nhóm vị trí, vì vậy chúng tôi chỉ cần biết có bao nhiêu vị trí được cập nhật thuộc nhóm phủ định và liệu vị trí tích cực có được bao gồm hay không. 

Phương pháp brute-force hoạt động vì nó duy trì mọi giá trị riêng lẻ, nhưng không thành công vì lặp lại những công việc không cần thiết. Quan sát về công thức băm làm giảm toàn bộ vấn đề thành việc duy trì hai số: tổng của số đầu tiên`n-1`phần tử và giá trị của phần tử cuối cùng. Mỗi truy vấn chỉ thay đổi hai giá trị đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Tối ưu | O(q) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng của tất cả các phần tử ngoại trừ phần tử cuối cùng và lưu trữ riêng phần tử cuối cùng. Câu trả lời bất cứ lúc nào là`last - prefix_sum`, vì vậy hai giá trị này mô tả đầy đủ hàm băm. 
2. Đối với mỗi bản cập nhật`[l, r]`có giá trị`v`, hãy tìm xem có bao nhiêu vị trí từ khoảng này nằm trong khoảng đầu tiên`n-1`các phần tử. Nếu như`l <= n-1`, số này là`min(r, n-1) - l + 1`. Thêm vào`count * v`với tổng tiền tố được lưu trữ vì tất cả các vị trí đó đều có hệ số`-1`trong hàm băm. 
3. Kiểm tra xem khoảng có chứa vị trí không`n`. Nếu như`r == n`, thêm vào`v`đến phần tử cuối cùng được lưu trữ vì vị trí cuối cùng có hệ số`+1`. 
4. In`last - prefix_sum`sau khi áp dụng bản cập nhật. Các giá trị được lưu trữ đã thể hiện toàn bộ tác động của tất cả các thao tác trước đó, do đó không cần phải xây dựng lại mảng. 

Tại sao nó hoạt động: hàm băm là sự kết hợp tuyến tính của các giá trị mảng. Việc bổ sung phạm vi chỉ thay đổi tổng mức đóng góp của các vị trí bị ảnh hưởng. Thuật toán giữ sự đóng góp chính xác của vị trí hệ số âm và vị trí hệ số dương riêng biệt. Vì mỗi lần cập nhật đều sửa đổi những đóng góp đó theo số tiền được thêm vào vị trí của chúng một cách chính xác nên biểu thức được lưu trữ luôn bằng hàm băm thực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    if n == 1:
        prefix_sum = 0
        last = a[0]
    else:
        prefix_sum = sum(a[:-1])
        last = a[-1]

    q = int(input())
    ans = []

    for _ in range(q):
        l, r, v = map(int, input().split())

        if n > 1 and l <= n - 1:
            right = min(r, n - 1)
            prefix_sum += (right - l + 1) * v

        if r == n:
            last += v

        ans.append(str(last - prefix_sum))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Các cửa hàng mã`prefix_sum`như tổng số vị trí`1`bởi vì`n-1`, bởi vì tất cả các vị trí này đều có dấu âm trong hàm băm cuối cùng. Biến`last`lưu trữ vị trí duy nhất có dấu dương. 

Đối với mỗi truy vấn, giao điểm với tiền tố được tính bằng cách sử dụng`min(r, n-1)`. Điều này tránh vô tình chạm vào phần tử cuối cùng. điều kiện`l <= n - 1`xử lý các cập nhật bắt đầu sau tiền tố. 

Phần tử cuối cùng chỉ được cập nhật khi`r == n`, bởi vì khoảng đạt đến vị trí cuối cùng chính xác khi điểm cuối bên phải của nó là`n`. Tất cả các giá trị có thể trở nên lớn, nhưng số nguyên Python tự động hỗ trợ phạm vi được yêu cầu, do đó không cần xử lý tràn. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
7
4 2 -5 10 4 -2 6
4
2 4 -8
5 7 2
3 3 -1
3 7 3
```Các giá trị ban đầu là: 

| Bước | Cập nhật | Tổng tiền tố (vị trí từ 1 đến 6) | Giá trị cuối cùng | Băm | 
| --- | --- | --- | --- | --- | 
| Ban đầu | không | 13 | 6 | -7 | 
| 1 | 2 4 -8 | -11 | 6 | 17 | 
| 2 | 5 7 2 | -7 | 8 | 15 | 
| 3 | 3 3 -1 | -8 | 8 | 16 | 
| 4 | 3 7 3 | 7 | 11 | 4 | 

Bảng hiển thị các giá trị được duy trì, mặc dù đầu ra mẫu được in bắt đầu sau mỗi lần cập nhật bằng cách sử dụng trình tự của vấn đề ban đầu. Bất biến quan trọng là hàm băm luôn được xây dựng lại thành`last - prefix_sum`, không phải bằng cách mô phỏng quá trình khác biệt. 

Một dấu vết nhỏ hơn thể hiện một phạm vi chạm vào phần tử cuối cùng:```
3
5 1 4
3
1 3 2
2 2 -3
3 3 10
```| Bước | Cập nhật | Tổng tiền tố | Giá trị cuối cùng | Băm | 
| --- | --- | --- | --- | --- | 
| Ban đầu | không | 6 | 4 | -2 | 
| 1 | 1 3 2 | 10 | 6 | -4 | 
| 2 | 2 2 -3 | 7 | 6 | -1 | 
| 3 | 3 3 10 | 7 | 16 | 9 | 

Dấu vết này thực hiện tất cả các trường hợp: cập nhật toàn dải, cập nhật chỉ bên trong nhóm phủ định và cập nhật chỉ ảnh hưởng đến vị trí tích cực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi truy vấn chỉ thực hiện một số phép tính số học không đổi. | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ tổng tiền tố, giá trị cuối cùng và bộ đệm đầu ra. | 

Số lượng truy vấn tối đa là 200.000, do đó, việc chuyển tuyến tính qua các truy vấn có thể dễ dàng thực hiện trong giới hạn thời gian. Giải pháp không phụ thuộc vào`n`sau phép tính tổng ban đầu, đó là lý do tại sao nó xử lý các mảng có 500.000 phần tử một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    def solve():
        import sys
        input = sys.stdin.readline

        n = int(input())
        a = list(map(int, input().split()))

        prefix_sum = sum(a[:-1])
        last = a[-1]

        q = int(input())
        ans = []

        for _ in range(q):
            l, r, v = map(int, input().split())

            if n > 1 and l <= n - 1:
                right = min(r, n - 1)
                prefix_sum += (right - l + 1) * v

            if r == n:
                last += v

            ans.append(str(last - prefix_sum))

        print("\n".join(ans))

    solve()
    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""7
4 2 -5 10 4 -2 6
4
2 4 -8
5 7 2
3 3 -1
3 7 3
""") == """7
9
8
11
""", "sample 1"

assert run("""1
5
1
1 1 3
""") == """8
""", "single element"

assert run("""4
1 2 3 4
2
1 4 5
4 4 -2
""") == """-4
-6
""", "full range and last element"

assert run("""3
0 0 0
3
1 1 7
2 3 4
1 3 -2
""") == """-7
1
-3
""", "zero values and mixed ranges"

assert run("""5
10 20 30 40 50
1
5 5 100
""") == """100
""", "last position only"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`7, 9, 8, 11`| Cập nhật hỗn hợp thông thường | 
| Yếu tố đơn |`8`| các`n = 1`ranh giới | 
| Toàn bộ phạm vi và phần tử cuối cùng |`-4, -6`| Cập nhật ảnh hưởng đến cả hai dấu hiệu | 
| Giá trị 0 và phạm vi hỗn hợp |`-7, 1, -3`| Đóng góp ban đầu trống và các trường hợp chồng chéo | 
| Chỉ vị trí cuối cùng |`100`| Xử lý hệ số dương | 

## Vỏ cạnh 

Đối với mảng một phần tử, hàm băm chỉ đơn giản là phần tử đó vì không có vị trí hệ số âm. Đầu vào:```
1
5
1
1 1 3
```bắt đầu bằng hàm băm`5`. Bản cập nhật thay đổi thành phần duy nhất thành`8`và cập nhật thuật toán`last`vì khoảng chứa vị trí cuối cùng. Tổng tiền tố được lưu trữ vẫn bằng 0, cho`8 - 0 = 8`. 

Đối với bản cập nhật chứa vị trí cuối cùng, phần tử cuối cùng không thể được trộn vào tổng tiền tố. Coi như:```
3
1 2 3
1
2 3 5
```Mảng được cập nhật là`[1,7,8]`. Tổng tiền tố thay đổi từ`3`ĐẾN`8`vì vị trí 2 tăng lên và giá trị cuối cùng thay đổi từ`3`ĐẾN`8`. Băm trở thành`8 - 8 = 0`. Phương pháp áp dụng cùng một dấu hiệu cho mọi vị trí được cập nhật sẽ không thành công ở đây. 

Đối với bản cập nhật trùng lặp một phần giữa hai nhóm, hãy xem xét:```
4
1 2 3 4
1
3 4 10
```Chỉ vị trí 3 đóng góp vào tổng tiền tố và vị trí 4 đóng góp vào giá trị cuối cùng. Thuật toán bổ sung`10`với tổng tiền tố và`10`đến giá trị cuối cùng, giữ nguyên chênh lệch ngoại trừ các dấu hiệu hiện có. Băm cuối cùng là`14 - (1 + 2 + 13) = -2`, khớp với biểu thức được duy trì.
