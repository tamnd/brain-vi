---
title: "CF 102212B - Đường Đua"
description: "Alice ghi đúng một điểm mỗi lần cô ấy chơi, trong khi Bob ghi đúng b điểm mỗi lần anh ấy chơi. Sau một số ván đấu, tổng số điểm của họ bằng nhau. Nhiệm vụ là tìm ra số điểm dương nhỏ nhất mà cả hai người chơi có thể có được."
date: "2026-08-18T00:25:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102212
codeforces_index: "B"
codeforces_contest_name: "Amazalgo Uni 2019 Practice Contest"
rating: 0
weight: 102212
solve_time_s: 180
verified: false
draft: false
---

[CF 102212B - Đường đua](https://codeforces.com/problemset/problem/102212/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Alice ghi điểm chính xác`a`điểm mỗi lần cô ấy chơi, trong khi Bob ghi điểm chính xác`b`điểm mỗi khi anh ấy chơi. Sau một số ván đấu, tổng số điểm của họ bằng nhau. Nhiệm vụ là tìm ra số điểm dương nhỏ nhất mà cả hai người chơi có thể có được. 

Nếu Alice chơi`x`nhiều lần, điểm của cô ấy là`a * x`. Nếu Bob chơi`y`lần, điểm của anh ấy là`b * y`. Chúng ta cần số nguyên dương nhỏ nhất`c`mà cả hai`a`Và`b`chia`c`. Nói cách khác, bài toán yêu cầu bội chung nhỏ nhất của`a`Và`b`. 

Cả hai giá trị đầu vào đều tối đa`10,000`. Tìm kiếm trực tiếp qua mọi điểm số có thể đạt được`100,000,000`, đủ lớn để thực hiện tìm kiếm tuyến tính không mong muốn dưới giới hạn 1 giây. Thay vào đó, cấu trúc của bài toán cho chúng ta một nghiệm số học theo thời gian không đổi. Bản thân câu trả lời cũng vừa vặn trong giới hạn đã nêu, vì bội số chung nhỏ nhất lớn nhất có thể có của hai giá trị trong phạm vi này nhiều nhất là`10,000 * 9,999 = 99,990,000`. 

Có một vài trường hợp nhỏ có thể bộc lộ việc thực hiện bất cẩn. Đối với đầu vào`1 1`, câu trả lời là`1`, bởi vì cả hai người chơi đều đã đạt được một điểm cho mỗi trận đấu. Việc triển khai bắt đầu tìm kiếm từ`a + b`sẽ bỏ lỡ mức tối thiểu. Đối với đầu vào`1 10000`, câu trả lời là`10000`, bởi vì mọi bội số dương của`10000`cũng chia hết cho`1`; xử lý hai giá trị một cách đối xứng thông qua một công thức không chính xác có thể tạo ra kết quả lớn hơn. Đối với đầu vào`4 6`, câu trả lời là`12`, không`24`, bởi vì`12`đã chia hết cho cả hai giá trị. Điều này nắm bắt các cách tiếp cận chỉ đơn giản là nhân các đầu vào thay vì loại bỏ hệ số chung của chúng. 

## Phương pháp tiếp cận 

Một giải pháp brute-force đơn giản có thể tạo ra các điểm số có thể có cho đến khi tìm thấy một điểm chia hết cho cả hai`a`Và`b`. Ví dụ, bắt đầu bằng`1`, chúng ta có thể kiểm tra xem mỗi số nguyên`c`thỏa mãn`c % a == 0`Và`c % b == 0`. Giá trị đầu tiên như vậy nhất thiết phải là câu trả lời vì mọi giá trị dương nhỏ hơn đều đã bị từ chối. 

Vấn đề với phiên bản này là phạm vi tìm kiếm có thể rất lớn. Vì`a = 10000`Và`b = 9999`, câu trả lời là`99,990,000`, vì vậy việc kiểm tra mọi số nguyên từ`1`thông qua câu trả lời yêu cầu`99,990,000`lần lặp lại. Mặc dù mỗi lần lặp đều đơn giản nhưng việc này tốn nhiều công sức hơn mức cần thiết. 

Chúng ta có thể làm cho việc tìm kiếm nhỏ hơn bằng cách chỉ xem xét bội số của một đầu vào. Nếu chúng ta kiểm tra`a`,`2a`,`3a`, v.v., bội số đầu tiên cũng chia hết cho`b`là bội chung nhỏ nhất. Vì câu trả lời nhiều nhất là`100,000,000`Và`a`ít nhất là`1`, việc này vẫn có thể cần tới khoảng`100,000,000`lặp đi lặp lại khi`a = 1`, vì vậy nó cũng không phải là giải pháp sạch nhất. 

Quan sát quan trọng là phần chung của`a`Và`b`không được tính hai lần. Cho phép`g = gcd(a, b)`. Sản phẩm`a * b`chứa đựng mọi yếu tố của`a`Và`b`, nhưng các yếu tố đại diện bởi`g`xuất hiện ở cả hai. Chia sản phẩm của họ theo`g`loại bỏ chính xác phần trùng lặp đó, đưa ra bội số chung nhỏ nhất:`lcm(a, b) = a * b / gcd(a, b)`. 

Brute-force hoạt động vì nó tìm kiếm số đầu tiên có cả hai thuộc tính chia hết, nhưng không thành công vì nó bỏ qua mối quan hệ số học giữa hai đầu vào. Nhận xét rằng các thừa số chung của chúng có thể được xác định bằng ước số chung lớn nhất cho phép chúng ta tính toán trực tiếp cùng một câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(lcm(a, b)) | O(1) | Quá chậm trong trường hợp xấu nhất | 
| Tối ưu | O(log(min(a, b))) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`a`Và`b`, số điểm Alice và Bob đạt được trong mỗi trò chơi. 
2. Tính toán`g = gcd(a, b)`sử dụng thuật toán Euclide. Gcd chứa chính xác các yếu tố được chia sẻ bởi cả hai giá trị tính điểm. 
3. Tính toán`a * b // g`. Đây là bội số chung nhỏ nhất vì nhân`a`Và`b`kết hợp tất cả các thừa số của chúng, trong khi chia cho gcd sẽ loại bỏ các thừa số được đưa vào hai lần. 
4. In giá trị kết quả. Đó là số điểm dương nhỏ nhất chia hết cho cả hai`a`Và`b`, vì vậy cả hai người chơi đều có thể đạt được nó bằng cách sử dụng một số nguyên trò chơi. 

### Tại sao nó hoạt động 

hãy để`g = gcd(a, b)`. Chúng ta có thể viết`a = g * x`Và`b = g * y`, Ở đâu`x`Và`y`là nguyên tố cùng nhau. Từ`x`Và`y`không có thừa số chung thì số nhỏ nhất chứa tất cả các thừa số cần cho cả hai đầu vào là`g * x * y`. Thay thế`x = a / g`Và`y = b / g`cho`(a / g) * b`, chính xác là`a * b / g`. Giá trị này chia hết cho cả hai`a`Và`b`và bất kỳ bội số chung nào cũng phải chứa cùng các thừa số bắt buộc, do đó không tồn tại bội số chung dương nhỏ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def gcd(a, b):
    while b:
        a, b = b, a % b
    return a

a, b = map(int, input().split())

g = gcd(a, b)
answer = a * b // g

print(answer)
```các`gcd`hàm thực hiện thuật toán Euclide. Tại mỗi lần lặp, cặp`(a, b)`được thay thế bởi`(b, a % b)`. Tập hợp các ước số chung không thay đổi theo phép biến đổi này và giá trị thứ hai cuối cùng trở thành 0. Giá trị đầu tiên còn lại là gcd. 

Sau khi tính toán gcd, mã sẽ đánh giá`a * b // g`. Phép chia số nguyên ở đây chính xác vì`g`chia cả hai`a`Và`b`, do đó nhất thiết phải chia sản phẩm của họ. Số nguyên Python cũng không bị tràn, mặc dù tích trung gian lớn nhất ở đây chỉ là`100,000,000`Dẫu sao thì. 

Không có sự lặp lại về điểm số có thể có, do đó không có điều kiện biên nào liên quan đến chính câu trả lời và không có vấn đề riêng lẻ nào. Các đầu vào được đảm bảo là dương, do đó gcd cũng dương và việc chia cho 0 không thể xảy ra. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`a = 2`Và`b = 3`, hai giá trị không có ước chung nào khác ngoài`1`. 

| một | b | gcd(a, b) | a*b | Trả lời | 
| --- | --- | --- | --- | --- | 
| 2 | 3 | 1 | 6 | 6 | 

gcd là`1`, do đó không cần loại bỏ yếu tố trùng lặp khỏi sản phẩm. Điểm nhỏ nhất chia hết cho cả hai`2`Và`3`là`6`. Alice đạt được nó sau`3`trò chơi, trong khi Bob đạt được nó sau`2`trò chơi. 

### Mẫu 2 

cho`a = 4`Và`b = 6`, các giá trị chia sẻ một yếu tố của`2`. 

| một | b | gcd(a, b) | a*b | Trả lời | 
| --- | --- | --- | --- | --- | 
| 4 | 6 | 2 | 24 | 12 | 

Sản phẩm`24`tính hệ số chung`2`hai lần. Chia cho gcd sẽ loại bỏ sự trùng lặp đó, tạo ra`12`. Alice đạt tới`12`sau đó`3`trò chơi và Bob đạt được nó sau`2`trò chơi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log(min(a, b))) | Thuật toán Euclide liên tục thay thế cặp bằng số dư nhỏ hơn. | 
| Không gian | O(1) | Chỉ có một số lượng biến số nguyên không đổi được lưu trữ. | 

Với tối đa cả hai đầu vào`10,000`, thuật toán Euclide chỉ thực hiện một số lần lặp. Giải pháp dễ dàng trong giới hạn thời gian 1 giây và sử dụng bộ nhớ không đáng kể so với giới hạn 64 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    data = inp.split()
    a, b = map(int, data)

    x, y = a, b
    while y:
        x, y = y, x % y

    return str(a * b // x) + "\n"

def run(inp: str) -> str:
    return solve(inp)

# Provided samples
assert run("2 3") == "6\n", "sample 1"
assert run("4 6") == "12\n", "sample 2"

# Minimum-size inputs
assert run("1 1") == "1\n", "minimum values"

# One value divides the other
assert run("1 10000") == "10000\n", "one divides the other"

# Equal values
assert run("10000 10000") == "10000\n", "equal maximum values"

# Maximum answer in the allowed input range
assert run("9999 10000") == "99990000\n", "maximum LCM"

# Shared factors, catches incorrect multiplication
assert run("12 18") == "36\n", "shared factors"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Đầu vào có kích thước tối thiểu và câu trả lời nhỏ nhất có thể | 
|`1 10000`|`10000`| Một giá trị bằng`1`và trường hợp một đầu vào chia cho đầu vào kia | 
|`10000 10000`|`10000`| Các giá trị bằng nhau, bao gồm đầu vào lớn nhất được phép | 
|`9999 10000`|`99990000`| Ranh giới câu trả lời lớn nhất trong giới hạn | 
|`12 18`|`36`| Loại bỏ chính xác các yếu tố được chia sẻ thay vì trả lại sản phẩm | 

## Vỏ cạnh 

cho`1 1`, gcd là`1`, do đó công thức cho`1 * 1 / 1 = 1`. Thuật toán không giả định rằng câu trả lời phải lớn hơn một trong hai đầu vào và trả về chính xác điểm dương nhỏ nhất có thể. 

Vì`1 10000`, thuật toán Euclide tính toán`gcd(1, 10000) = 1`. Kết quả là`1 * 10000 / 1 = 10000`. Điều này hiệu quả vì mọi số nguyên đều chia hết cho`1`, do đó, điểm tích cực đầu tiên mà Bob có thể đạt được đã là điểm hợp lệ đối với Alice. 

Vì`4 6`, gcd là`2`. Công thức cho`4 * 6 / 2 = 12`. Việc thực hiện bất cẩn chỉ sử dụng`a * b`sẽ trở lại`24`, là bội số chung nhưng không phải là bội số nhỏ nhất. Chia cho gcd chính xác là cách loại bỏ yếu tố trùng lặp. 

Vì`9999 10000`, gcd là`1`, nên không có yếu tố nào bị loại bỏ. Sản phẩm là`99,990,000`, là bội số chung nhỏ nhất và cũng chứng tỏ rằng đáp án có thể gần với giới hạn trên đã nêu của`100,000,000`. Thuật toán xử lý trực tiếp ranh giới này mà không cần lặp qua hàng triệu điểm ứng viên nhỏ hơn.
