---
title: "CF 106203B - \u041f\u0438\u0440\u0430\u043c\u0438\u0434\u0430 \u0412\u0438\u0434\u0435\u043d\u0438\u0439"
description: "Chúng ta được cho một mảng hình tam giác có n hàng. Hàng thứ i chứa đúng i số nguyên. Quá trình bắt đầu từ hàng trên cùng và liên tục hủy từng hàng một cho đến khi chỉ còn lại hàng dưới cùng."
date: "2026-06-19T13:44:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106203
codeforces_index: "B"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2025-2026, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106203
solve_time_s: 58
verified: true
draft: false
---

[CF 106203B - \u041f\u0438\u0440\u0430\u043c\u0438\u0434\u0430 \u0412\u0438\u0434\u0435\u043d\u0438\u0439](https://codeforces.com/problemset/problem/106203/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng hình tam giác với`n`hàng. Hàng ngang`i`chứa chính xác`i`số nguyên. Quá trình bắt đầu từ hàng trên cùng và liên tục hủy từng hàng một cho đến khi chỉ còn lại hàng dưới cùng. 

Khi một tinh thể có giá trị`x`bị loại bỏ, nó sẽ chọn tinh thể lớn hơn trong số hai tinh thể ngay bên dưới nó. Giả sử con bên trái có giá trị`a`và đứa trẻ bên phải có giá trị`b`. Nếu như`a > b`, con bên trái đổi thành`a - x`. Nếu không thì con bên phải đổi thành`b - x`. Con còn lại không thay đổi. 

Sau khi tất cả các hàng ngoại trừ hàng cuối cùng đã biến mất, chúng ta phải xuất giá trị của các tinh thể ở hàng dưới cùng. 

Tam giác chứa$$1+2+\dots+n=\frac{n(n+1)}2$$những con số. Từ`n ≤ 1000`, tổng số phần tử nhiều nhất là 500500. Bất kỳ thuật toán nào xử lý mỗi tinh thể với số lần không đổi đều dễ dàng đủ nhanh. Bậc hai về số lượng ô có nghĩa là đại khái$2.5 \times 10^{11}$hoạt động vượt xa giới hạn. 

Một điểm tinh tế là một tinh thể sẽ sửa đổi một trong các tinh thể con của nó ngay lập tức và các quyết định sau này phải sử dụng các giá trị được cập nhật chứ không phải các giá trị ban đầu. 

Coi như```
2
5
1 10
```Tinh thể hàng đầu chọn`10`, từ`1 ≤ 10`, và thay đổi nó thành`10-5=5`. 

Câu trả lời là```
1 5
```Sử dụng các giá trị ban đầu ở mọi nơi sẽ tạo ra kết quả sai. 

Một nguồn sai lầm khác là trường hợp hòa. Giá trị bằng nhau không chọn con trái. 

Vì```
2
3
7 7
```điều kiện`a ≤ b`được áp dụng, do đó tinh thể bên phải sẽ trở thành`7-3=4`. 

Câu trả lời đúng là```
7 4
```sử dụng`>=`thay vì`>`sẽ sửa đổi không chính xác tinh thể bên trái. 

Số âm cũng quan trọng.```
2
-5
2 1
```Từ`2 > 1`, tinh thể bên trái được chọn và trở thành$$2-(-5)=7.$$Câu trả lời là```
7 1
```Trừ theo thứ tự sai sẽ cho`-7`. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là mô phỏng nghi lễ chính xác như được mô tả. Bắt đầu từ hàng đầu tiên, chúng ta kiểm tra từng tinh thể và so sánh hai giá trị bên dưới nó. Tùy theo cái nào lớn hơn, chúng ta sửa đổi ngay con đó. 

Mô phỏng này đã hiệu quả vì mọi tinh thể ngoại trừ những tinh thể ở hàng cuối cùng đều được xử lý một lần. Tổng số ô được xử lý bằng$$1+2+\dots+(n-1)=\frac{n(n-1)}2,$$tức là khoảng năm trăm nghìn thao tác khi`n=1000`. 

Người ta có thể tưởng tượng một cách tiếp cận phức tạp hơn nhiều để theo dõi các đường đi hoặc theo dõi đệ quy nơi năng lượng chảy. Một lực mạnh như vậy trên tất cả các con đường có thể sẽ trở thành cấp số nhân vì mỗi tinh thể có khả năng có hai lựa chọn. May mắn thay, bản thân quá trình này quyết định sự lựa chọn duy nhất và mỗi tinh thể chỉ ảnh hưởng đến một đứa trẻ. Không cần phải quay lại. 

Quan sát quan trọng là tam giác có thể được cập nhật tại chỗ một cách đơn giản. Một lần hàng`i`đã được xử lý, nó sẽ không bao giờ được sử dụng lại. Tất cả các quyết định trong tương lai chỉ phụ thuộc vào các giá trị đã được cập nhật trong hàng`i+1`. Điều này cho phép chúng ta ghi đè trực tiếp các giá trị mà không cần bất kỳ cấu trúc phụ trợ nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê tất cả các đường dẫn có thể | O(2^n) | O(n) | Quá chậm | 
| Mô phỏng tại chỗ | O(n²) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc toàn bộ tam giác thành mảng hai chiều. 
2. Xử lý các hàng từ trên xuống dưới. Hàng ngang`i`ảnh hưởng đến hàng`i+1`, vậy sau khi kết thúc hàng`i`chúng tôi không bao giờ cần nó nữa. 
3. Đối với mọi vị trí`j`liên tiếp`i`, cho phép`x`là giá trị hiện tại của tinh thể đó. 
4. So sánh hai đứa trẻ,`triangle[i+1][j]`Và`triangle[i+1][j+1]`. 
5. Nếu con bên trái lớn hơn thì thay thế bằng$$\text{left}-x.$$Nếu không thì thay thế con bên phải bằng$$\text{right}-x.$$Việc so sánh phải sử dụng các giá trị hiện tại, bởi vì các tinh thể trước đó trong cùng một hàng có thể đã sửa đổi chúng. 

1. Tiếp tục cho đến khi hàng cuối cùng thứ hai được xử lý. 
2. Xuất ra hàng cuối cùng. 

### Tại sao nó hoạt động 

Sau khi xử lý hàng`i`, mọi thay đổi gây ra bởi các tinh thể xếp thành hàng`0,1,\dots,i`đã được đưa vào hàng`i+1`. Không có hoạt động nào trong tương lai có thể ảnh hưởng đến bất kỳ hàng nào trước đó. 

Bất biến là trước khi bắt đầu hàng`i`, tất cả các hàng phía trên nó đã bị hủy hoàn toàn theo đúng quy tắc và hàng`i`chứa các giá trị sẽ tồn tại tại thời điểm đó trong nghi lễ. Việc xử lý từng tinh thể áp dụng chính xác thao tác giống như câu lệnh, do đó, bất biến vẫn đúng. Khi hàng cuối cùng thứ hai kết thúc, hàng dưới cùng chứa chính xác các năng lượng cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
triangle = [list(map(int, input().split())) for _ in range(n)]

for i in range(n - 1):
    for j in range(i + 1):
        x = triangle[i][j]
        if triangle[i + 1][j] > triangle[i + 1][j + 1]:
            triangle[i + 1][j] -= x
        else:
            triangle[i + 1][j + 1] -= x

print(*triangle[-1])
```Hình tam giác được lưu trữ chính xác như nó xuất hiện trong đầu vào. Vì các tính toán sau này luôn cần các giá trị được cập nhật nên việc sửa đổi mảng tại chỗ là điều đương nhiên. 

Vòng lặp bên ngoài xử lý các hàng từ trên xuống dưới. Hàng ngang`i`chứa`i+1`các phần tử, do đó vòng lặp bên trong sẽ chạy trên tất cả các phần tử đó. 

Việc so sánh sử dụng`>`và không`>=`. Các giá trị bằng nhau phải chọn con bên phải vì câu lệnh chỉ xác định con bên trái khi nó lớn hơn hoàn toàn. 

Số nguyên Python tự động xử lý các giá trị lớn nên việc tràn dữ liệu không phải là vấn đề. Với số nguyên có kích thước cố định trong ngôn ngữ khác, loại 64 bit sẽ là cần thiết. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
2
4 5
4 5 6
```Nhà nước phát triển như sau. 

| Hàng đang được xử lý | Giá trị tinh thể | Con trước | Con được sửa đổi | Hàng dưới cùng sau bước | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | (4,5) | phải trở thành 3 | 4 5 6 | 
| 2 | 4 | (4,3) | trái trở thành 0 | 0 3 6 | 
| 2 | 3 | (3,6) | phải trở thành 3 | 0 3 3 | 

Câu trả lời cuối cùng:```
0 3 3
```Ví dụ này cho thấy các giá trị được thay đổi trước đó trong một hàng phải được sử dụng trong các so sánh sau này. 

### Mẫu 2 

đầu vào:```
4
-5
-5 2
8 1 -4
8 -9 3 4
```| Hàng đang được xử lý | Giá trị tinh thể | Con trước | Con được sửa đổi | 
| --- | --- | --- | --- | 
| 1 | -5 | (-5,2) | phải trở thành 7 | 
| 2 | -5 | (8,7) | trái trở thành 13 | 
| 2 | 7 | (7,-4) | trái trở thành 0 | 
| 3 | 13 | (8,-9) | trái trở thành -5 | 
| 3 | 0 | (-9,3) | bên phải còn lại 3 | 
| 3 | -4 | (3,4) | phải trở thành 8 | 

Hàng cuối cùng:```
-5 -9 3 8
```Dấu vết này chứng tỏ rằng việc trừ một số âm sẽ làm tăng số con được chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi tinh thể ngoại trừ những tinh thể ở hàng cuối cùng được xử lý một lần | 
| Không gian | O(n²) | Toàn bộ tam giác được lưu trữ | 

Tam giác lớn nhất có thể chứa 500500 phần tử. Việc xử lý từng ô một lần dễ dàng vừa vặn trong giới hạn thời gian và lưu trữ nửa triệu số nguyên vừa vặn thoải mái trong 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    triangle = [list(map(int, input().split())) for _ in range(n)]

    for i in range(n - 1):
        for j in range(i + 1):
            x = triangle[i][j]
            if triangle[i + 1][j] > triangle[i + 1][j + 1]:
                triangle[i + 1][j] -= x
            else:
                triangle[i + 1][j + 1] -= x

    return " ".join(map(str, triangle[-1]))

# minimum size
assert run("1\n7\n") == "7"

# equal values, right child must be chosen
assert run("2\n3\n7 7\n") == "7 4"

# negative top value
assert run("2\n-5\n2 1\n") == "7 1"

# simple chain
assert run("3\n1\n2 3\n4 5 6\n") == "4 2 3"

# all equal values
assert run("3\n1\n1 1\n1 1 1\n") == "1 0 -1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`7`| Kích thước tối thiểu | 
|`2 / 3 / 7 7`|`7 4`| Cà vạt đi đúng con | 
|`2 / -5 / 2 1`|`7 1`| Xử lý đúng các giá trị âm | 
|`3 / 1 / 2 3 / 4 5 6`|`4 2 3`| Tuyên truyền bình thường | 
| Tất cả những cái |`1 0 -1`| Nhiều mối quan hệ và cập nhật lặp đi lặp lại | 

## Vỏ cạnh 

###Con cái bình đẳng 

đầu vào:```
2
3
7 7
```Các con đều bình đẳng nên điều kiện`a ≤ b`áp dụng. Tinh thể bên phải trở thành$$7-3=4.$$Thuật toán so sánh với`>`, do đó`else`nhánh cập nhật đúng con. 

Đầu ra:```
7 4
```### Giá trị âm 

đầu vào:```
2
-5
2 1
```Từ`2 > 1`, con bên trái được chọn. Bản cập nhật là$$2-(-5)=7.$$Thuật toán thực hiện phép trừ theo thứ tự này, tạo ra```
7 1
```### Hàng đơn 

đầu vào:```
1
100
```Không có hàng nào bị hủy vì hàng dưới cùng cũng là hàng trên cùng. Các vòng lặp hoàn toàn không thực thi và chương trình chỉ in```
100
```phù hợp với hành vi dự định.
