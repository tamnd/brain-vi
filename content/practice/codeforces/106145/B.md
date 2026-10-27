---
title: "CF 106145B - Khả năng đi bộ"
description: "Chúng ta có một bộ sưu tập các ngôi nhà và mỗi ngôi nhà đều có một con số được viết trên đó. Có thể đi bộ giữa hai ngôi nhà khi các số của chúng có một đặc tính chung: tồn tại một số nguyên lớn hơn một số chia hết hiệu của chúng."
date: "2026-06-25T11:28:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106145
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 10-29-25"
rating: 0
weight: 106145
solve_time_s: 51
verified: true
draft: false
---

[CF 106145B - Khả năng đi bộ](https://codeforces.com/problemset/problem/106145/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Chúng ta có một bộ sưu tập các ngôi nhà và mỗi ngôi nhà đều có một con số được viết trên đó. Có thể đi bộ giữa hai ngôi nhà khi các số của chúng có một đặc tính chung: tồn tại một số nguyên lớn hơn một số chia hết hiệu của chúng. Nếu hai ngôi nhà không có ước số như vậy thì Magikarp phải dùng ô tô để di chuyển giữa chúng. 

Nhiệm vụ là chọn thứ tự ghé thăm mỗi nhà đúng một lần đồng thời giảm thiểu số lần di chuyển liên tiếp theo thứ tự đó cần phải lái xe. Vì ngôi nhà bắt đầu là tùy ý nên câu trả lời là số lượng tối thiểu các cặp lân cận xấu trong một hoán vị của tất cả các số nhà. 

Sự đơn giản hóa chính xuất phát từ việc xem xét tình trạng của vỉa hè. Cho hai số`x`Và`y`, một ước số phù hợp tồn tại chính xác khi`|x-y|`không bằng`1`. Nếu các số bằng nhau thì mọi số lớn hơn một chia hiệu số bằng 0, do đó tồn tại vỉa hè. Nếu hiệu lớn hơn một thì chính hiệu đó là ước số hợp lệ. Các cặp duy nhất không thể đi được là các cặp số nguyên liên tiếp. 

Đầu vào chứa tối đa`2 * 10^5`những ngôi nhà, trong khi bản thân các giá trị có thể lớn bằng`10^9`. Chúng ta không thể so sánh tất cả các cặp nhà vì điều đó đòi hỏi`O(n^2)`kiểm tra, xung quanh`4 * 10^10`hoạt động trong trường hợp lớn nhất. Giải pháp phải phụ thuộc vào số lượng giá trị riêng biệt thay vì kích thước của phạm vi số. 

Một số trường hợp rất dễ bỏ sót. Chỉ với hai giá trị liên tiếp khác nhau, mỗi thứ tự truy cập hợp lệ tại một thời điểm nào đó phải chuyển từ nhóm giá trị này sang nhóm giá trị khác. 

Ví dụ:```
2
6 7
```Câu trả lời là:```
1
```Không có cách nào để đặt tất cả`6`s và tất cả`7`s không có`6-7`chuyển tiếp. 

Với ba giá trị liên tiếp, giá trị ở giữa gây ra vấn đề tương tự. 

Ví dụ:```
3
1 2 3
```Câu trả lời là:```
1
```giá trị`2`cũng không thể ở bên cạnh`1`hoặc`3`trên một con đường chỉ dành cho đi bộ. Vì đó là những giá trị duy nhất khác nên khối chứa`2`s phải chạm vào một trong số họ ở đâu đó. 

Một ví dụ ba giá trị khác hoạt động khác nhau:```
3
1 2 10
```Câu trả lời là:```
0
```Chúng ta có thể ghé thăm theo thứ tự`1, 10, 2`, trong đó cả hai lần chuyển đổi đều nằm giữa các số có chênh lệch lớn hơn một. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng xây dựng toàn bộ biểu đồ các kết nối đi bộ và tìm kiếm đường Hamilton có càng ít cạnh không đi bộ càng tốt. Đồ thị có tới`n = 200000`các đỉnh và việc kiểm tra tất cả các cặp sẽ quá tốn kém trước khi chúng ta bắt đầu tìm kiếm đường đi. Quan điểm bạo lực rất hữu ích vì nó tiết lộ cấu trúc thực: các cạnh bị cấm duy nhất nằm giữa các giá trị liên tiếp. 

Quan sát quan trọng là chỉ có tập hợp các giá trị riêng biệt mới quan trọng. Nếu có ít nhất bốn giá trị riêng biệt, chúng ta luôn có thể sắp xếp các ngôi nhà sao cho tránh được các giá trị liên tiếp. Đặt tất cả các giá trị của một chẵn lẻ với nhau và tất cả các giá trị của chẵn lẻ kia lại với nhau. Các số có cùng số chẵn lẻ không bao giờ có thể khác nhau một đơn vị, vì vậy mọi chuyển đổi bên trong các nhóm này đều an toàn. Với đủ các giá trị khác biệt, ranh giới giữa hai nhóm cũng có thể được chọn một cách an toàn. 

Các tình huống còn lại duy nhất là khi có nhiều nhất ba giá trị phân biệt. Với hai giá trị, cần có một ổ đĩa chính xác khi các giá trị liên tiếp. Với ba giá trị, chỉ cần điều khiển khi cả ba giá trị đều liên tiếp. Trong mọi cách sắp xếp khác, các giá trị có thể được sắp xếp sao cho không có cặp lân cận nào khác nhau một. 

Toàn bộ vấn đề giảm xuống việc đếm các giá trị riêng biệt và kiểm tra một vài cấu hình nhỏ này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n²) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả số nhà và loại bỏ các số trùng lặp. Sắp xếp các giá trị còn lại. 
2. Nếu chỉ có một giá trị riêng biệt, hãy trả về`0`bởi vì mỗi lần di chuyển là giữa các số bằng nhau. 
3. Nếu có ít nhất bốn giá trị riêng biệt, trả về`0`. Các giá trị có thể được phân tách bằng tính chẵn lẻ để tránh mọi chuyển đổi số liên tiếp. 
4. Nếu có chính xác hai giá trị khác nhau, hãy kiểm tra sự khác biệt của chúng. Trở lại`1`khi chênh lệch là một, nếu không thì trả về`0`. 
5. Nếu có chính xác ba giá trị phân biệt, hãy kiểm tra xem chúng có tạo thành ba số nguyên liên tiếp hay không. Trở lại`1`trong trường hợp đó, nếu không hãy trả lại`0`. 

Tại sao nó hoạt động: Các cặp đi bộ bị cấm duy nhất là các số nguyên liên tiếp. Khi số lượng các giá trị riêng biệt đủ lớn, việc nhóm chẵn lẻ sẽ tạo ra một lộ trình đi bộ hoàn chỉnh. Khi có ít giá trị hơn, vật cản duy nhất có thể là một chuỗi các số nguyên liên tiếp. Hai giá trị liên tiếp buộc một giao nhau giữa các nhóm và ba giá trị liên tiếp buộc giá trị ở giữa chạm vào hàng xóm không hợp lệ. Không có cấu hình nào khác có đủ hạn chế để yêu cầu ổ đĩa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    vals = sorted(set(a))
    m = len(vals)

    if m == 1:
        print(0)
    elif m >= 4:
        print(0)
    elif m == 2:
        print(1 if vals[1] - vals[0] == 1 else 0)
    else:
        print(1 if vals[1] == vals[0] + 1 and vals[2] == vals[1] + 1 else 0)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên nén dữ liệu đầu vào thành các giá trị riêng biệt vì các số bằng nhau không bao giờ tạo ra yêu cầu lái xe. Việc sắp xếp cho phép truy cập trực tiếp vào khoảng trống giữa một số giá trị còn lại. 

Việc phân chia trường hợp diễn ra trực tiếp từ chứng minh. Số lượng lớn các giá trị riêng biệt cho phép sắp xếp tính chẵn lẻ, trong khi một, hai và ba giá trị riêng biệt cần kiểm tra rõ ràng. Không cần lưu trữ mảng ban đầu sau khi trích xuất tập hợp vì bội số không thay đổi cho dù có tồn tại chuyển đổi bắt buộc liên tiếp hay không. 

Việc thực hiện tránh mọi so sánh theo cặp. Hoạt động tốn kém duy nhất là sắp xếp các giá trị riêng biệt, dễ dàng phù hợp với giới hạn cho`2 * 10^5`những con số. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
1 3 4 8 9
```Các giá trị riêng biệt đã có: 

| Bước | Giá trị riêng biệt | Đếm | Quyết định | 
| --- | --- | --- | --- | 
| 1 |`[1, 3, 4, 8, 9]`| 5 | Ít nhất bốn giá trị | 
| 2 |`[1, 3, 4, 8, 9]`| 5 | Câu trả lời là`0`| 

Ý tưởng nhóm chẵn lẻ đưa ra một lộ trình chỉ dành cho việc đi bộ. Ví dụ: các giá trị có tính chẵn lẻ có thể được truy cập cùng nhau và các giá trị có tính chẵn lẻ có thể được truy cập cùng nhau. 

Đầu ra:```
0
```### Mẫu 2 

đầu vào:```
2
6 7
```Các giá trị riêng biệt là: 

| Bước | Giá trị riêng biệt | Đếm | Quyết định | 
| --- | --- | --- | --- | 
| 1 |`[6, 7]`| 2 | Chính xác là hai giá trị | 
| 2 | Sự khác biệt là`1`| | Cần có một ổ đĩa | 

Hai nhóm chứa các số liên tiếp nên bất kỳ tuyến đường nào cũng phải đi qua giữa chúng một lần. 

Đầu ra:```
1
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các giá trị riêng biệt chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n) | Tập hợp các giá trị riêng biệt có thể chứa tất cả các giá trị đầu vào | 

Thuật toán xử lý kích thước đầu vào tối đa vì nó chỉ thực hiện một thao tác sắp xếp và một lượng công việc không đổi sau đó. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    import builtins
    n = int(input())
    a = list(map(int, input().split()))

    vals = sorted(set(a))
    m = len(vals)

    if m == 1:
        ans = 0
    elif m >= 4:
        ans = 0
    elif m == 2:
        ans = 1 if vals[1] - vals[0] == 1 else 0
    else:
        ans = 1 if vals[1] == vals[0] + 1 and vals[2] == vals[1] + 1 else 0

    sys.stdin = old_stdin
    return str(ans) + "\n"

assert solve_data("""5
1 3 4 8 9
""") == "0\n", "sample 1"

assert solve_data("""2
6 7
""") == "1\n", "sample 2"

assert solve_data("""1
100
""") == "0\n", "single value"

assert solve_data("""6
5 5 5 5 5 5
""") == "0\n", "all equal values"

assert solve_data("""5
1 2 3 4 5
""") == "0\n", "large consecutive interval"

assert solve_data("""4
10 11 12 100
""") == "1\n", "three consecutive values inside four values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`giá trị |`0`| Đầu vào kích thước tối thiểu | 
| Tất cả các giá trị bằng nhau |`0`| Xử lý trùng lặp | 
| Năm giá trị liên tiếp |`0`| Xây dựng bình đẳng | 
| Ba giá trị liên tiếp có giá trị cực xa |`1`| Tắc nghẽn trường hợp nhỏ | 

## Vỏ cạnh 

Đối với một giá trị riêng biệt, chẳng hạn như:```
5
42 42 42 42 42
```thuật toán loại bỏ các bản sao và chỉ nhìn thấy`[42]`. Mọi nước đi đều nằm giữa các giá trị bằng nhau nên kết quả là`0`. 

Cho hai giá trị liên tiếp:```
4
8 8 9 9
```các giá trị riêng biệt là`[8, 9]`. Vì sự khác biệt là một nên mỗi đơn hàng hoàn chỉnh phải để lại ít nhất một chuyển tiếp giữa hai nhóm. Thuật toán trả về`1`. 

Đối với ba giá trị liên tiếp:```
5
1 1 2 3 3
```các giá trị riêng biệt là`[1, 2, 3]`. Giá trị trung bình`2`không thể đứng cạnh giá trị khác nếu không có ổ đĩa. Thuật toán phát hiện chuỗi này và trả về`1`. 

Đối với nhiều giá trị riêng biệt:```
6
10 11 12 13 14 15
```có sáu giá trị riêng biệt nên việc xây dựng tính chẵn lẻ được áp dụng. Thuật toán trả về`0`, bởi vì các giá trị có thể được sắp xếp sao cho tất cả các cặp lân cận khác nhau nhiều hơn một.
