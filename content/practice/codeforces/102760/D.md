---
title: "CF 102760D - Sửa dây điện"
description: "Quá trình cài đặt là một biểu đồ hoàn chỉnh: mỗi cặp nút có một dây nối giữa chúng. Thông tin còn thiếu duy nhất là giá trị thẻ nào thuộc về dây nào. Chúng tôi nhận được tất cả các giá trị thẻ và chúng tôi phải quyết định hai cách gán khác nhau cho các giá trị này."
date: "2026-07-28T23:49:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102760
codeforces_index: "D"
codeforces_contest_name: "2020 KAIST 10th ICPC Mock Contest (XXI Open Cup. Grand Prix of Korea. Division 2)"
rating: 0
weight: 102760
solve_time_s: 66
verified: true
draft: false
---

[CF 102760D - Sửa dây](https://codeforces.com/problemset/problem/102760/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Quá trình cài đặt là một biểu đồ hoàn chỉnh: mỗi cặp nút có một dây nối giữa chúng. Thông tin còn thiếu duy nhất là giá trị thẻ nào thuộc về dây nào. Chúng tôi nhận được tất cả các giá trị thẻ và chúng tôi phải quyết định hai cách gán khác nhau cho các giá trị này. 

Nhiệm vụ đầu tiên phải làm cho cây bao trùm tối thiểu càng rẻ càng tốt. Nhiệm vụ thứ hai phải làm cho cây bao trùm tối thiểu càng đắt càng tốt. Đầu ra là chi phí MST trong cả hai trường hợp. 

Số lượng nút nhiều nhất là 100 nên số lượng dây có thể đạt tới$$\frac{100 \cdot 99}{2}=4950$$đủ nhỏ để chúng tôi có thể sắp xếp tất cả các thẻ và thực hiện quét tuyến tính. Một giải pháp thử mọi cách gán thẻ có thể cho dây sẽ là không thể vì ngay cả 4950! nhiệm vụ tồn tại. Cách tiếp cận dự định phải sử dụng cấu trúc của các đồ thị hoàn chỉnh thay vì khám phá các dây riêng lẻ. 

Các trường hợp phức tạp chính được gây ra bởi các giá trị bằng nhau và bởi các đồ thị rất nhỏ. Các thẻ bằng nhau không thể được coi là các lựa chọn riêng biệt vì việc hoán đổi chúng không thay đổi gì. Các giá trị nhỏ của`N`cũng quan trọng vì các công thức dựa trên nhóm đỉnh vẫn phải hoạt động khi chỉ tồn tại một hoặc hai cạnh MST. 

Ví dụ, với hai nút thì chỉ có một dây.```
Input
2
10

Output
10 10
```Việc triển khai bất cẩn giả định rằng có nhiều cạnh MST có thể truy cập vào các vị trí không tồn tại. 

Một trường hợp khác là khi tất cả các thẻ đều giống hệt nhau.```
Input
4
7 7 7 7 7 7

Output
21 21
```Mọi nhiệm vụ có thể đều tạo ra cùng một chi phí MST. Một cách tiếp cận cố gắng ép buộc những lựa chọn nhỏ nhất và lớn nhất khác nhau sẽ tạo ra câu trả lời sai. 

Trường hợp cạnh thứ ba là một đồ thị hoàn chỉnh nhỏ trong đó phép gán tối đa không đạt được bằng cách lấy giá trị lớn nhất`N - 1`thẻ.```
Input
4
3 5 5 8 8 9

Output
13 16
```Tổng cộng ba thẻ lớn nhất là 25, nhưng số đó không thể bị ép vào MST. Các thẻ nhỏ hơn có thể được ẩn bên trong các nhóm đã được kết nối và MST lớn nhất có thể sử dụng các thẻ từ các vị trí cụ thể theo thứ tự được sắp xếp. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp sẽ cố gắng đặt mọi thẻ trên mọi dây có thể và tính toán MST thu được. Bản thân việc tính toán MST rất dễ dàng với thuật toán Kruskal, nhưng số lượng phép gán là giai thừa của số lượng dây. Ngay cả đối với`N = 10`, đã có 45 dây, khiến phương pháp này hoàn toàn không thể sử dụng được. 

Quan sát đầu tiên là biểu đồ đã hoàn chỉnh. Vì mỗi cặp nút đều có một dây nên việc gán MST rẻ nhất rất đơn giản. Chúng ta có thể đặt cái nhỏ nhất`N - 1`thẻ trên các cạnh của bất kỳ cây nào. Kruskal sẽ chọn chính xác các cạnh đó vì chúng đã kết nối tất cả các đỉnh. Mọi cạnh còn lại có thể nhận giá trị lớn hơn mà không ảnh hưởng đến MST. Câu trả lời tối thiểu là tổng của câu đầu tiên`N - 1`giá trị sau khi sắp xếp. 

Trường hợp tối đa yêu cầu một cái nhìn khác. Kruskal xử lý các cạnh từ nhỏ nhất đến lớn nhất. Cạnh đầu tiên luôn đi vào MST vì ban đầu mọi nút đều bị cô lập. Sau khi cạnh đó kết nối hai nút, cạnh giữa hai nút đó có thể trở nên vô dụng bằng cách gán cho nó một giá trị nhỏ, vì cả hai điểm cuối đều đã nằm trong cùng một thành phần. 

Để trì hoãn các lựa chọn MST sau này, chúng tôi muốn tạo một thành phần được kết nối ngày càng tăng. Sau khi chọn`i - 1`Các cạnh MST, thành phần này có`i`đỉnh. Bên trong nó có$$\frac{i(i-1)}{2}$$các cạnh và tất cả chúng có thể xuất hiện trước cạnh hữu ích tiếp theo mà không được chọn. Điều này có nghĩa là`i`Cạnh MST có thể được đặt ở vị trí$$\frac{i(i-1)}{2}+1$$trong danh sách các thẻ được sắp xếp, sử dụng một chỉ mục dựa trên. 

Ví dụ: với bốn đỉnh, MST tối đa sử dụng các vị trí:```
i = 1: position 1
i = 2: position 2
i = 3: position 4
```Đối với các giá trị mẫu`3 5 5 8 8 9`, điều này mang lại`3 + 5 + 8 = 16`. 

Hai chiến lược này có thể được tóm tắt như sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(M! * M log M) | O(M) | Quá chậm | 
| Tối ưu | O(M log M) | O(M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các giá trị thẻ theo thứ tự không giảm. Thứ tự của các thẻ là thông tin duy nhất cần thiết vì MST phụ thuộc vào trọng số cạnh tương đối. 
2. Tính chi phí MST tối thiểu bằng cách cộng giá trị đầu tiên`N - 1`các giá trị đã sắp xếp. Những giá trị này luôn có thể được đặt trên cây bao trùm, buộc Kruskal phải chọn chúng. 
3. Tính chi phí MST tối đa bằng cách lặp lại lần đầu tiên`N - 1`các cạnh MST. Đối với`i`cạnh đã chọn, thêm giá trị tại chỉ mục`i * (i - 1) / 2`trong lập chỉ mục dựa trên số không. 
4. In hai số tiền tích lũy. 

Công trình xây dựng tối đa vì sau`i - 1`các cạnh được chọn, sự sắp xếp tốt nhất có một thành phần chứa`i`đỉnh. Tất cả các cạnh bên trong thành phần đó có thể được lấp đầy bằng các thẻ nhỏ hơn và bị Kruskal bỏ qua. Cạnh tiếp theo kết nối thành phần này với một đỉnh mới xuất hiện chính xác sau khi tất cả các cạnh bên trong đó đã được xử lý. 

Tại sao nó hoạt động: 

Đối với câu trả lời tối thiểu, không có MST nào có thể chứa ít hơn`N - 1`các cạnh và tổng nhỏ nhất có thể của nhiều thẻ đó đạt được bằng cách sử dụng giá trị nhỏ nhất`N - 1`thẻ. 

Để có câu trả lời tối đa, mỗi cạnh MST phải kết nối hai thành phần khác nhau tại thời điểm Kruskal chọn nó. Cạnh hữu ích đầu tiên chỉ có thể xuất hiện sau 0 cạnh trong, cạnh thứ hai sau một cạnh trong, cạnh thứ ba sau ba cạnh trong, v.v. Việc phát triển một thành phần sẽ tối đa hóa số cạnh có thể bị ẩn trước mỗi cạnh MST. Các vị trí được tạo bởi công thức chính xác là các vị trí mới nhất có thể, do đó tổng kết quả là chi phí MST lớn nhất có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    c = list(map(int, input().split()))
    c.sort()

    mn = sum(c[:n - 1])

    mx = 0
    for i in range(1, n):
        idx = i * (i - 1) // 2
        mx += c[idx]

    print(mn, mx)

if __name__ == "__main__":
    solve()
```Đầu vào chứa chính xác một trường hợp thử nghiệm, vì vậy giải pháp chỉ cần một đường dẫn thực thi. 

Sau khi sắp xếp, phép tính tối thiểu sẽ lấy giá trị đầu tiên`n - 1`mục nhập. Việc chặt cây là an toàn vì có cây trên`n`các đỉnh luôn có chính xác`n - 1`các cạnh. 

Để tính toán tối đa, biến vòng lặp`i`biểu thị số đỉnh đã có bên trong thành phần đang phát triển trước khi thêm cạnh MST tiếp theo. Chỉ số dựa trên số không`i * (i - 1) // 2`giống như vị trí dựa trên một`i * (i - 1) / 2 + 1`. 

Số nguyên Python không bị tràn, điều này quan trọng vì giá trị thẻ có thể lên tới`2 * 10^9`và MST có thể chứa 99 cạnh. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
5 3 8 8 5 9
```Các thẻ được sắp xếp là:```
3 5 5 8 8 9
```| Bước | tôi | Đóng góp tối thiểu | Chỉ số tối đa | Đóng góp tối đa | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 0 | 3 | 
| 2 | 2 | 5 | 1 | 5 | 
| 3 | 3 | 5 | 3 | 8 | 

Câu trả lời tối thiểu là`3 + 5 + 5 = 13`. 

Câu trả lời tối đa là`3 + 5 + 8 = 16`. 

Dấu vết này cho thấy tại sao việc chọn ba giá trị lớn nhất là không chính xác. 

### Mẫu 2 

Một ví dụ tùy chỉnh:```
5
1 2 3 4 5 6 7 8 9 10
```| Bước | tôi | Đóng góp tối thiểu | Chỉ số tối đa | Đóng góp tối đa | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 1 | 
| 2 | 2 | 2 | 1 | 2 | 
| 3 | 3 | 3 | 3 | 4 | 
| 4 | 4 | 4 | 6 | 7 | 

Câu trả lời tối thiểu là`1 + 2 + 3 + 4 = 10`. 

Câu trả lời tối đa là`1 + 2 + 4 + 7 = 14`. 

Dấu vết cho thấy có bao nhiêu cạnh nhỏ có thể được ẩn bên trong thành phần đang phát triển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M log M) | Sắp xếp`M`thẻ thống trị thời gian chạy. | 
| Không gian | O(M) | Mảng giá trị thẻ được lưu trữ trong bộ nhớ. | 

Với`M <= 4950`, việc sắp xếp dễ dàng nằm trong giới hạn. Việc sử dụng bộ nhớ cũng ít vì số lượng thẻ chỉ vài nghìn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    import sys
    input = sys.stdin.readline

    n = int(input())
    c = list(map(int, input().split()))
    c.sort()

    mn = sum(c[:n - 1])
    mx = 0
    for i in range(1, n):
        mx += c[i * (i - 1) // 2]

    ans = f"{mn} {mx}"

    sys.stdin = old_stdin
    return ans

assert solve_case("4\n5 3 8 8 5 9\n") == "13 16", "sample 1"
assert solve_case("2\n10\n") == "10 10", "minimum size"
assert solve_case("4\n7 7 7 7 7 7\n") == "21 21", "all equal values"
assert solve_case("5\n1 2 3 4 5 6 7 8 9 10\n") == "10 14", "larger complete graph"
assert solve_case("3\n1000000000 2000000000 3000000000\n") == "3000000000 3000000000", "boundary values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 / 5 3 8 8 5 9`|`13 16`| Hành vi mẫu và cách xây dựng tối đa | 
|`2 / 10`|`10 10`| Đồ thị cạnh đơn | 
| Tất cả các giá trị bằng nhau |`21 21`| Trọng số trùng lặp | 
| Mười thẻ với năm nút |`10 14`| Nhiều cạnh bên trong ẩn | 
| Giá trị lớn |`3000000000 3000000000`| Xử lý số nguyên lớn | 

## Vỏ cạnh 

Đối với hai nút, chỉ có một dây, vì vậy chi phí MST rẻ nhất và đắt nhất đều như nhau.```
Input
2
10
```Mảng được sắp xếp là`[10]`. Mức tối thiểu sử dụng giá trị duy nhất. Vòng lặp tối đa cũng chạy một lần và chọn chỉ số 0. Đầu ra là:```
10 10
```Khi mọi thẻ đều bằng nhau thì mọi phép gán có thể đều cho cùng một MST.```
Input
4
7 7 7 7 7 7
```Tối thiểu sử dụng ba số bảy. Tối đa cũng chọn ba vị trí, nhưng mỗi vị trí đều có bảy vị trí. Cả hai kết quả là:```
21 21
```Đối với trường hợp lớn nhất`N - 1`thẻ không thể tạo thành MST tối đa:```
Input
4
3 5 5 8 8 9
```Sau khi sắp xếp, vị trí tối đa là chỉ số`0`,`1`, Và`3`. Thuật toán chọn`3`,`5`, Và`8`. Các giá trị`8`Và`9`cả hai đều không thể bị buộc vào MST vì quá nhiều cạnh lớn hơn sẽ khiến các cạnh nhỏ hơn không thể bị ẩn đi. Kết quả là:```
13 16
```Đây chính xác là tình huống mà đối số tăng trưởng thành phần có ý nghĩa quan trọng. Thuật toán không chỉ đơn giản chọn các giá trị lớn; nó chọn các giá trị thực sự có thể xuất hiện dưới dạng các cạnh được chọn của Kruskal.
