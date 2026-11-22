---
title: "CF 290C - WTF?"
description: "Đầu vào không phải là một đặc tả toán học thông thường. Thay vào đó, nó là một chương trình nhỏ được viết bằng LOLCODE. Nhiệm vụ là xác định xem chương trình đó sẽ tính cái gì và in ra số thực thu được. Giá trị đầu vào đầu tiên được đọc vào một biến điều khiển vòng lặp."
date: "2026-06-05T16:40:26+07:00"
tags: ["codeforces", "competitive-programming", "*special", "graph-matchings", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 290
codeforces_index: "C"
codeforces_contest_name: "April Fools Day Contest 2013"
rating: 1700
weight: 290
solve_time_s: 127
verified: true
draft: false
---

[CF 290C - WTF?](https://codeforces.com/problemset/problem/290/C) 

**Đánh giá:** 1700 
**Thẻ:** *đặc biệt, so khớp biểu đồ, triển khai, cây 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đầu vào không phải là một đặc tả toán học thông thường. Thay vào đó, nó là một chương trình nhỏ được viết bằng LOLCODE. Nhiệm vụ là xác định xem chương trình đó sẽ tính cái gì và in ra số thực thu được. 

Giá trị đầu vào đầu tiên được đọc vào một biến điều khiển vòng lặp. Hãy để giá trị đó là`n`. Sau đó chương trình sẽ đọc chính xác`n`các chữ số bổ sung, mỗi chữ số nằm giữa`0`Và`9`. 

Trong khi xử lý trình tự, nó duy trì: 

-`FOO`, tổng của tất cả các giá trị nhìn thấy cho đến nay. 
-`BAR`, số lượng giá trị nhìn thấy cho đến nay. 
-`BAZ / QUZ`, một phân số biểu thị tiền tố trung bình tốt nhất được tìm thấy cho đến nay. 

Sau khi đọc từng giá trị mới, chương trình so sánh giá trị trung bình tiền tố hiện tại$$\frac{\text{FOO}}{\text{BAR}}$$với mức trung bình tốt nhất được lưu trữ$$\frac{\text{BAZ}}{\text{QUZ}}.$$Nếu giá trị trung bình hiện tại ít nhất bằng giá trị đó thì nó sẽ thay thế phần được lưu trữ. 

Cuối cùng, nó in$$\frac{\text{BAZ}}{\text{QUZ}},$$đó chính xác là mức trung bình tối đa trên tất cả các tiền tố của chuỗi. 

Từ`n`bản thân nó là một chữ số, nó nằm giữa`0`Và`9`. Câu lệnh cho biết đầu vào chứa từ 1 đến 10 dòng, khớp với một dòng cho`n`và tối đa chín giá trị bổ sung. Kích thước dữ liệu rất nhỏ, vì vậy ngay cả một giải pháp bậc hai cũng có thể phù hợp một cách thoải mái. Thách thức thực sự là hiểu được chương trình làm gì. 

Một lỗi phổ biến là tìm kiếm giá trị riêng lẻ lớn nhất thay vì giá trị trung bình tiền tố tối đa. 

Coi như:```
3
0
1
1
```Giá trị lớn nhất là`1`, nhưng giá trị trung bình tiền tố là:```
0
0.5
0.666667
```Câu trả lời đúng là:```
0.666667
```Một sai lầm dễ mắc phải khác là tối đa hóa giá trị trung bình trên các mảng con tùy ý thay vì tiền tố. 

Vì:```
3
1
0
9
```Mảng con`[9]`có mức trung bình`9`, nhưng chương trình chỉ xem xét tiền tố. Các giá trị trung bình tiền tố là:```
1
0.5
3.333333
```Câu trả lời đúng là:```
3.333333
```Điểm tinh tế thứ ba là sự so sánh giữa các phân số. Chương trình gốc không bao giờ chuyển đổi sang dấu phẩy động. Nó so sánh$$\frac{a}{b} \ge \frac{c}{d}$$bằng cách kiểm tra$$a \cdot d \ge c \cdot b.$$Sử dụng dấu phẩy động ở đây vẫn có tác dụng đối với vấn đề này vì các con số rất nhỏ, nhưng việc tái tạo chương trình một cách chính xác sẽ sạch hơn với số học số nguyên. 

## Phương pháp tiếp cận 

Cách giải thích trực tiếp nhất là mô phỏng chương trình theo nghĩa đen. 

Chúng tôi đọc chuỗi, tính toán mỗi tiền tố trung bình, giữ giá trị lớn nhất được thấy cho đến nay và in nó ở cuối. Vì có nhiều nhất chín giá trị nên thậm chí việc lưu trữ tất cả các giá trị trung bình và quét chúng sau này cũng không đáng kể. 

Phần thú vị là hiểu cách chương trình thực hiện so sánh. Thay vì tính toán số thực, nó lưu giá trị trung bình tốt nhất dưới dạng phân số`BAZ / QUZ`. Khi một tiền tố mới có tổng`FOO`và chiều dài`BAR`xuất hiện, nó so sánh$$\frac{\text{FOO}}{\text{BAR}}$$Và$$\frac{\text{BAZ}}{\text{QUZ}}$$sử dụng phép nhân chéo:$$\text{FOO} \cdot \text{QUZ}$$so với$$\text{BAR} \cdot \text{BAZ}.$$Điều này tránh hoàn toàn số học dấu phẩy động. 

Ý tưởng vũ phu và ý tưởng tối ưu ở đây thực sự giống nhau vì kích thước đầu vào quá nhỏ. Chúng tôi xử lý tiền tố một lần từ trái sang phải và duy trì mức trung bình tốt nhất từng thấy cho đến nay. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (lưu trữ tất cả các giá trị trung bình tiền tố) | O(n) | O(n) | Đã chấp nhận | 
| Tối ưu (theo dõi phân số tốt nhất trực tuyến) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số đầu tiên`n`. 
2. Khởi tạo: 

-`sum_prefix = 0`-`count = 0`-`best_sum = 0`-`best_count = 1`Phân số`best_sum / best_count`đại diện cho tiền tố trung bình tốt nhất được tìm thấy cho đến nay. 
3. Lặp lại`n`lần: 

- Đọc chữ số tiếp theo`x`. 
- Thêm nó vào`sum_prefix`. 
- Tăng`count`. 
4. So sánh giá trị trung bình tiền tố hiện tại với giá trị trung bình tốt nhất được lưu trữ bằng phép nhân chéo: 

- Nếu$$\text{sum\_prefix} \cdot \text{best\_count} \ge \text{count} \cdot \text{best\_sum},$$sau đó cập nhật: 

-`best_sum = sum_prefix`-`best_count = count`Đây chính xác là sự so sánh được thực hiện bởi chương trình LOLCODE. 
5. Sau khi tất cả các giá trị đã được xử lý, xuất ra$$\frac{\text{best\_sum}}{\text{best\_count}}.$$### Tại sao nó hoạt động 

Sau khi xử lý bất kỳ tiền tố nào, cặp`(best_sum, best_count)`lưu trữ mức trung bình tiền tố lớn nhất trong số tất cả các tiền tố được thấy cho đến nay. 

Ban đầu điều này đúng vì giá trị được lưu trữ là`0/1`. Bất cứ khi nào một tiền tố mới được xử lý, chúng tôi sẽ so sánh mức trung bình của nó với mức tối đa được lưu trữ. Nếu nó lớn hơn hoặc bằng, chúng ta thay thế phân số đã lưu. Nếu không, chúng tôi giữ cái trước đó. Bằng quy nạp, sau lần lặp cuối cùng, phân số được lưu trữ biểu thị giá trị trung bình tối đa trên tất cả các tiền tố, đây chính xác là những gì chương trình in ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = [int(line.strip()) for line in sys.stdin if line.strip()]

    n = data[0]

    prefix_sum = 0
    cnt = 0

    best_sum = 0
    best_cnt = 1

    for x in data[1:1 + n]:
        prefix_sum += x
        cnt += 1

        if prefix_sum * best_cnt >= cnt * best_sum:
            best_sum = prefix_sum
            best_cnt = cnt

    print(best_sum / best_cnt)

if __name__ == "__main__":
    solve()
```Giá trị đầu tiên xác định có bao nhiêu số tiếp theo thuộc về chuỗi. Khi mỗi giá trị được đọc, tổng chạy và độ dài tiền tố sẽ được cập nhật. 

Điểm mấu chốt là sự so sánh```
prefix_sum * best_cnt >= cnt * best_sum
```cái nào kiểm tra xem$$\frac{\text{prefix\_sum}}{\text{cnt}} \ge \frac{\text{best\_sum}}{\text{best\_cnt}}$$không sử dụng số học dấu phẩy động. 

Phân số được lưu trữ chỉ được cập nhật khi giá trị trung bình của tiền tố hiện tại ít nhất bằng giá trị trung bình tốt nhất được thấy trước đó. Cuối cùng, việc chia tử số và mẫu số được lưu trữ sẽ tạo ra kết quả cuối cùng của chương trình. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
0
1
1
```| Bước | x | tiền tố_sum | cnt | tốt nhất_sum | tốt nhất_cnt | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | - | 0 | 0 | 0 | 1 | 
| 1 | 0 | 0 | 1 | 0 | 1 | 
| 2 | 1 | 1 | 2 | 1 | 2 | 
| 3 | 1 | 2 | 3 | 2 | 3 | 

Câu trả lời cuối cùng:$$\frac{2}{3}=0.666667$$Dấu vết này cho thấy phần được lưu trữ di chuyển từ`0/1`ĐẾN`1/2`và cuối cùng là`2/3`, khớp với chuỗi trung bình tiền tố tăng dần. 

### Ví dụ 2 

đầu vào:```
3
5
0
0
```| Bước | x | tiền tố_sum | cnt | tốt nhất_sum | tốt nhất_cnt | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | - | 0 | 0 | 0 | 1 | 
| 1 | 5 | 5 | 1 | 5 | 1 | 
| 2 | 0 | 5 | 2 | 5 | 1 | 
| 3 | 0 | 5 | 3 | 5 | 1 | 

Câu trả lời cuối cùng:$$\frac{5}{1}=5$$Tiền tố đầu tiên đã có mức trung bình tối đa. Tiền tố sau có mức trung bình`2.5`Và`1.666...`, do đó phân số được lưu trữ không bao giờ thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi giá trị được xử lý một lần | 
| Không gian | O(1) | Chỉ một số biến đang chạy được lưu trữ | 

Kích thước đầu vào cực kỳ nhỏ, nhưng ngay cả đối với các giá trị lớn hơn nhiều, quá trình quét tuyến tính này vẫn có hiệu quả. Việc sử dụng bộ nhớ không đổi trong suốt quá trình thực thi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    data = [int(line.strip()) for line in sys.stdin if line.strip()]
    n = data[0]

    prefix_sum = 0
    cnt = 0
    best_sum = 0
    best_cnt = 1

    for x in data[1:1 + n]:
        prefix_sum += x
        cnt += 1

        if prefix_sum * best_cnt >= cnt * best_sum:
            best_sum = prefix_sum
            best_cnt = cnt

    return f"{best_sum / best_cnt}\n"

# provided sample
out = float(run("3\n0\n1\n1\n"))
assert abs(out - 0.666667) < 1e-4

# minimum non-empty sequence
assert run("1\n0\n") == "0.0\n", "single zero"

# single positive value
assert run("1\n9\n") == "9.0\n", "single element"

# best prefix is the first one
assert run("3\n5\n0\n0\n") == "5.0\n", "early maximum"

# all equal values
assert run("4\n7\n7\n7\n7\n") == "7.0\n", "constant average"

# maximum average appears at the end
out = float(run("3\n1\n0\n9\n"))
assert abs(out - (10.0 / 3.0)) < 1e-9
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`|`0`| Dãy số có ý nghĩa nhỏ nhất | 
|`1 9`|`9`| Tiền tố một phần tử | 
|`3 5 0 0`|`5`| Tiền tố tốt nhất xảy ra ngay lập tức | 
|`4 7 7 7 7`|`7`| Trung bình bằng nhau trong suốt | 
|`3 1 0 9`|`10/3`| Tiền tố tốt nhất xuất hiện ở cuối | 

## Vỏ cạnh 

Xem xét đầu vào```
1
0
```Chỉ có một tiền tố. Sau khi xử lý giá trị, phần được lưu trữ sẽ trở thành`0/1`và đầu ra của thuật toán`0`. Không cần xử lý đặc biệt. 

Coi như```
3
5
0
0
```Các giá trị trung bình tiền tố là:```
5
2.5
1.666667
```Tiền tố đầu tiên vẫn tối ưu. Phép so sánh nhân chéo loại bỏ chính xác các tiền tố sau vì:$$5 \cdot 1 > 2 \cdot 5$$Và$$5 \cdot 1 > 3 \cdot 5.$$Coi như```
4
7
7
7
7
```Mọi tiền tố trung bình đều bằng`7`. Bởi vì chương trình sử dụng so sánh không nghiêm ngặt (`>=`), nó cập nhật phần được lưu trữ trên các mối quan hệ. Phần được lưu trữ cuối cùng trở thành`28/4`, vẫn đánh giá là`7`. Đầu ra vẫn chính xác bất kể tiền tố trung bình bằng nhau nào được giữ lại. 

Coi như```
3
1
0
9
```Một giải pháp tối đa hóa các mảng con tùy ý sẽ chọn`[9]`và trở lại`9`, điều đó là sai. Thuật toán chỉ đánh giá tiền tố:```
1
0.5
3.333333
```và trả về chính xác`10/3`.
