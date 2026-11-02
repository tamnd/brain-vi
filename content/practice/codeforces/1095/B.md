---
title: "CF 1095B - Ổn định mảng"
description: "Chúng ta được cung cấp một danh sách các số đại diện cho một mảng và chúng ta được phép loại bỏ chính xác một phần tử. Sau khi loại bỏ nó, chúng ta xem xét mức độ “trải rộng” của các số còn lại, được định nghĩa là chênh lệch giữa giá trị còn lại lớn nhất và nhỏ nhất."
date: "2026-06-12T05:51:22+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1095
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 529 (Div. 3)"
rating: 900
weight: 1095
solve_time_s: 84
verified: true
draft: false
---

[CF 1095B - Ổn định mảng](https://codeforces.com/problemset/problem/1095/B) 

**Xếp hạng:** 900 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số đại diện cho một mảng và chúng ta được phép loại bỏ chính xác một phần tử. Sau khi loại bỏ nó, chúng ta xem xét mức độ “trải rộng” của các số còn lại, được định nghĩa là chênh lệch giữa giá trị còn lại lớn nhất và nhỏ nhất. 

Nhiệm vụ là chọn phần tử nào cần loại bỏ sao cho mức chênh lệch này trở nên nhỏ nhất có thể, sau đó đưa ra mức chênh lệch tối thiểu có thể đạt được đó. 

Hạn chế chính là mảng có thể lớn, lên tới 100.000 phần tử. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng loại bỏ từng phần tử và tính toán lại mức tối thiểu và tối đa từ đầu theo thời gian tuyến tính cho mỗi lần xóa, vì điều đó sẽ dẫn đến khoảng$O(n^2)$làm việc trong trường hợp xấu nhất. Thay vào đó, chúng ta cần tính toán điều gì đó về cấu trúc toàn cục một lần và sử dụng lại nó. 

Trường hợp cạnh tinh tế xuất hiện khi các giá trị cực trị xuất hiện nhiều lần. Ví dụ: nếu giá trị tối thiểu xuất hiện nhiều lần thì việc loại bỏ chỉ một lần xuất hiện sẽ không làm thay đổi giá trị tối thiểu. Điều tương tự cũng áp dụng cho mức tối đa. Điều này quan trọng vì việc loại bỏ tối ưu thường nhắm tới một trong những thái cực, nhưng không phải lúc nào cũng vậy. 

Hãy xem xét mảng`[1, 100, 100, 100]`. Đang xóa`1`mang lại sự lan tỏa`100 - 100 = 0`. Đang xóa`100`mang lại sự lan tỏa`100 - 1 = 99`. Một cách tiếp cận ngây thơ chỉ xem xét việc loại bỏ phần tử đầu tiên hoặc cuối cùng theo thứ tự được sắp xếp sẽ thất bại nếu nó không tính toán chính xác các bản sao của cực trị. 

Một trường hợp cạnh khác phát sinh khi tất cả các giá trị đều bằng nhau. Khi đó, mọi thao tác xóa vẫn để lại tất cả các giá trị bằng nhau và câu trả lời là 0. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mọi chỉ mục, mô phỏng việc loại bỏ phần tử đó, tính toán mức tối thiểu và tối đa của mảng còn lại và theo dõi chênh lệch kết quả nhỏ nhất. Tính toán min và max sau mỗi lần loại bỏ chi phí$O(n)$, và làm điều đó cho tất cả$n$lợi suất vị trí$O(n^2)$. Với$n = 10^5$, điều này vượt xa giới hạn khả thi. 

Cấu trúc của vấn đề làm cho một chiến lược hiệu quả hơn có thể thực hiện được. Sự mất ổn định chỉ phụ thuộc vào cực tiểu và cực đại của các phần tử còn lại. Khi chúng tôi loại bỏ một phần tử, chỉ có hai điều có thể thay đổi câu trả lời: hoặc chúng tôi loại bỏ phần tử không cực trị và giá trị tối thiểu/tối đa giữ nguyên hoặc chúng tôi xóa một trong các giá trị cực trị và có khả năng chuyển giá trị tối thiểu hoặc tối đa sang phần tử nhỏ nhất hoặc lớn thứ hai. 

Điều này gợi ý rằng chúng ta chỉ cần biết giá trị nhỏ nhất và nhỏ thứ hai cũng như giá trị lớn nhất và lớn thứ hai. Nếu chúng ta loại bỏ phần tử ở giữa thì giá trị min và max không thay đổi nên kết quả không thay đổi. Nếu chúng ta loại bỏ mức tối thiểu hiện tại thì mức tối thiểu mới sẽ trở thành mức tối thiểu thứ hai. Nếu chúng ta loại bỏ mức tối đa hiện tại thì mức tối đa mới sẽ trở thành mức tối đa thứ hai. 

Vì vậy, câu trả lời tối ưu chỉ đến từ hai ứng viên: loại bỏ mức tối thiểu hoặc loại bỏ mức tối đa. Bất kỳ sự loại bỏ nào khác đều không thể cải thiện phạm vi vì nó giữ nguyên cả hai cực trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét mảng một lần để xác định giá trị nhỏ nhất, giá trị nhỏ thứ hai, giá trị lớn nhất và giá trị lớn thứ hai. Bốn con số này mô tả đầy đủ cách phạm vi có thể thay đổi sau khi loại bỏ phần tử cực trị. 
2. Tính phạm vi ban đầu là`max_value - min_value`. Điều này thể hiện trường hợp chúng tôi loại bỏ một phần tử không cực đoan hoặc không cải thiện được điều gì một cách hiệu quả. 
3. Xem xét loại bỏ phần tử tối thiểu. Sau khi loại bỏ, giá trị nhỏ nhất còn lại sẽ trở thành giá trị nhỏ thứ hai, do đó phạm vi kết quả sẽ trở thành`max_value - second_min`. 
4. Xem xét loại bỏ phần tử tối đa. Sau khi loại bỏ, giá trị còn lại lớn nhất sẽ trở thành giá trị lớn thứ hai, do đó phạm vi kết quả sẽ trở thành`second_max - min_value`. 
5. Lấy giá trị tối thiểu trong số ba giá trị sau: phạm vi ban đầu, loại bỏ mức tối thiểu và loại bỏ mức tối đa. Đây là sự mất ổn định tốt nhất có thể đạt được sau khi loại bỏ chính xác một phần tử. 

### Tại sao nó hoạt động 

Tính không ổn định của mảng chỉ phụ thuộc vào các giá trị cực trị của nó. Việc xóa phần tử không cực đoan không thể thay đổi điểm cuối của phạm vi, do đó nó không thể cải thiện kết quả. Việc loại bỏ một cực trị chỉ làm thay đổi một bên của phạm vi và giá trị ứng cử viên tiếp theo ở bên đó được xác định duy nhất bởi phần tử nhỏ thứ hai hoặc lớn thứ hai. Vì mọi thao tác xóa hợp lệ đều thuộc một trong các loại này nên việc kiểm tra các trường hợp này một cách toàn diện sẽ bao gồm tất cả các khả năng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

INF = 10**18

min1 = min2 = INF
max1 = max2 = -INF

for x in a:
    # update smallest and second smallest
    if x < min1:
        min2 = min1
        min1 = x
    elif x < min2:
        min2 = x

    # update largest and second largest
    if x > max1:
        max2 = max1
        max1 = x
    elif x > max2:
        max2 = x

original = max1 - min1
remove_min = max1 - min2
remove_max = max2 - min1

print(min(original, remove_min, remove_max))
```Việc triển khai thực hiện một lần truyền qua mảng, duy trì bốn giá trị đang chạy. Logic cập nhật bảo tồn cẩn thận cả ứng cử viên tốt nhất và tốt nhất thứ hai cho mỗi thái cực, đảm bảo rằng khi một thái cực bị loại bỏ theo giả thuyết thì giá trị thay thế chính xác đã được biết đến. 

Bước cuối cùng đánh giá ba cấu hình có ý nghĩa. Không có hoạt động xóa nào khác được xem xét rõ ràng vì chúng không thể thay đổi ranh giới của phạm vi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 3 3 7
```Chúng tôi theo dõi cực tiểu và cực đại: 

| Bước | Giá trị | phút1 | phút2 | tối đa1 | tối đa2 | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | thông tin | - | - | 
| 2 | 3 | 1 | 3 | 3 | thông tin | 
| 3 | 3 | 1 | 3 | 3 | thông tin | 
| 4 | 7 | 1 | 3 | 7 | 3 | 

Bây giờ hãy tính: 

- ban đầu = 7 - 1 = 6 
- bỏ min = 7 - 3 = 4 
- loại bỏ max = 3 - 1 = 2 

Câu trả lời là 2. 

Dấu vết này cho thấy việc loại bỏ phần tử ở giữa (3) là không liên quan, trong khi loại bỏ phần tử tối đa sẽ tạo ra sự cải thiện tốt nhất. 

### Ví dụ 2 

đầu vào:```
5
5 5 5 5 5
```| Bước | Giá trị | phút1 | phút2 | tối đa1 | tối đa2 | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 5 | thông tin | 5 | -inf | 
| 2 | 5 | 5 | 5 | 5 | 5 | 
| 3 | 5 | 5 | 5 | 5 | 5 | 
| 4 | 5 | 5 | 5 | 5 | 5 | 
| 5 | 5 | 5 | 5 | 5 | 5 | 

Tính toán: 

- ban đầu = 0 
- bỏ min = 5 - 5 = 0 
- loại bỏ max = 5 - 5 = 0 

Câu trả lời là 0. 

Điều này xác nhận rằng các bản sao của các giá trị cực trị được xử lý chính xác: việc xóa một lần xuất hiện không làm thay đổi giá trị tối thiểu hoặc tối đa hiệu dụng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần để tính giá trị cực trị và cực trị thứ hai | 
| Không gian | O(1) | Chỉ có bốn biến phụ được sử dụng | 

Quá trình quét tuyến tính vừa vặn thoải mái trong giới hạn 100.000 phần tử và bộ nhớ không đổi đảm bảo không có chi phí vượt quá dung lượng lưu trữ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    INF = 10**18

    min1 = min2 = INF
    max1 = max2 = -INF

    for x in a:
        if x < min1:
            min2 = min1
            min1 = x
        elif x < min2:
            min2 = x

        if x > max1:
            max2 = max1
            max1 = x
        elif x > max2:
            max2 = x

    return str(min(max1 - min1, max1 - min2, max2 - min1))

# provided sample
assert run("4\n1 3 3 7\n") == "2"

# all equal
assert run("5\n5 5 5 5 5\n") == "0"

# remove min best case
assert run("4\n1 100 100 100\n") == "0"

# remove max best case
assert run("4\n1 2 3 4\n") == "2"

# minimal size
assert run("2\n1 10\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 | ổn định dưới cực trị trùng lặp | 
| lệch với một phút | 0 | loại bỏ tối thiểu cải thiện phạm vi | 
| trình tự tăng dần | 2 | loại bỏ tính chính xác của trường hợp tối đa | 
| cỡ 2 | 0 | hành vi ranh giới tối thiểu | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi mức tối thiểu hoặc tối đa xuất hiện nhiều lần. Đối với đầu vào`[1, 1, 5, 10]`, loại bỏ một`1`không thay đổi mức tối thiểu, vì vậy mức tối thiểu thứ hai vẫn là`1`. Thuật toán xử lý việc này vì`min2`cũng sẽ là`1`, Vì thế`max - min2`phản ánh chính xác không có sự thay đổi trong phạm vi. 

Một trường hợp khác là khi nước đi tốt nhất không thể hiện rõ ràng từ vị trí mà từ sự lặp lại giá trị. TRONG`[1, 2, 3, 4]`, loại bỏ`2`hoặc`3`không làm gì để cải thiện phạm vi, trong khi loại bỏ`1`hoặc`4`cho một phạm vi nhỏ hơn. Thuật toán nắm bắt được điều này bằng cách chỉ đánh giá các lần xóa quá mức, đảm bảo rằng các lần xóa không quá cao không bao giờ cần phải được kiểm tra rõ ràng. 

Cuối cùng, đối với mảng có kích thước hai, việc loại bỏ một phần tử sẽ để lại một giá trị duy nhất, làm cho độ không ổn định bằng 0. Công thức giảm một cách chính xác vì việc loại bỏ cực độ sẽ dẫn đến việc so sánh các điểm cuối còn lại giống hệt nhau.
