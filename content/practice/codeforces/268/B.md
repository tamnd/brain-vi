---
title: "CF 268B - Nút"
description: "Có một thứ tự ẩn gồm n nút. Một nút chỉ được nhấn nếu đó là nút đúng tiếp theo theo thứ tự đó. Nếu tại bất kỳ thời điểm nào chúng ta nhấn nhầm nút, mọi nút đã nhấn trước đó sẽ bật ra và chúng ta phải bắt đầu xây dựng lại trình tự từ đầu."
date: "2026-06-05T01:06:47+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 268
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 164 (Div. 2)"
rating: 1000
weight: 268
solve_time_s: 94
verified: true
draft: false
---

[CF 268B - Nút](https://codeforces.com/problemset/problem/268/B) 

**Đánh giá:** 1000 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Có một mệnh lệnh ẩn`n`các nút. Một nút chỉ được nhấn nếu đó là nút đúng tiếp theo theo thứ tự đó. Nếu tại bất kỳ thời điểm nào chúng ta nhấn nhầm nút, mọi nút đã nhấn trước đó sẽ bật ra và chúng ta phải bắt đầu xây dựng lại trình tự từ đầu. 

Manao không biết thứ tự, nhưng anh ta có thể quan sát xem nút nào vẫn được nhấn hay khóa có được đặt lại hay không. Anh luôn chọn những hành động giảm thiểu số lần nhấn cần thiết. Chúng ta phải tính số lần nhấn nút cần thiết theo thứ tự ẩn xấu nhất có thể. 

Đầu vào chứa một số nguyên duy nhất`n`, số lượng nút. Đầu ra là số lần nhấn tối đa mà Manao có thể cần khi sử dụng chiến lược tốt nhất. 

Hạn chế là rất nhỏ,`n ≤ 2000`. Thậm chí một`O(n²)`giải pháp chỉ thực hiện khoảng bốn triệu thao tác, điều này không đáng kể trong giới hạn. Không cần tối ưu hóa nâng cao. Thử thách là nhận biết công thức đếm. 

Một lỗi phổ biến là chỉ tính số lần thử thất bại và quên rằng những lần nhấn thành công cũng tiêu tốn số lần nhấn nút. 

Coi như:```
n = 2
```Câu trả lời là`3`, không`2`. 

Trong trường hợp xấu nhất, Manao thử bấm nhầm nút trước. Sau đó, anh ta biết nút đầu tiên chính xác. Sau khi nhấn thành công, anh vẫn phải nhấn nút thứ hai để mở khóa. 

Một sai lầm dễ mắc phải khác là cho rằng mọi giai đoạn đều yêu cầu kiểm tra tất cả các nút còn lại. 

Coi như:```
n = 3
```Khi đã biết hai nút đầu tiên và vẫn nhấn, nút cuối cùng sẽ được xác định tự động. Không cần phải kiểm tra nó. Câu trả lời đúng là`7`, không`9`. 

Trường hợp cạnh`n = 1`cũng quan trọng.```
1
```Đầu ra:```
1
```Chỉ có một nút bấm nên chỉ cần nhấn một lần là khóa sẽ mở được. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để suy nghĩ về vấn đề là mô phỏng cách Manao khám phá ra trình tự. 

Giả sử anh ta đã biết điều đầu tiên`k - 1`các nút của trình tự. Để khám phá`k`- nút thứ, có`n - k + 1`các nút ứng cử viên còn lại. 

Trong trường hợp xấu nhất, nút đúng là ứng cử viên cuối cùng anh ta thử. 

Đối với mỗi ứng cử viên sai, trước tiên anh ta phải xây dựng lại tiền tố độ dài đã biết`k - 1`, sau đó thực hiện sai. Chi phí đó`k`máy ép. 

Sau khi đánh trượt tất cả các ứng cử viên không chính xác, anh ta xây dựng lại tiền tố lần cuối và nhấn nút đúng. 

Mô phỏng lực lượng vũ phu có thể cộng thêm các chi phí này một cách rõ ràng cho mọi vị trí. Từ`n ≤ 2000`, ngay cả quá trình đếm này cũng đủ nhanh. Khó khăn là thể hiện tổng thể một cách rõ ràng. 

Quan sát quan trọng là đối với vị trí`k`, có chính xác`n - k`ứng viên sai. 

Mỗi ứng viên sai đều phải trả giá`k`máy ép:```
(k - 1) presses to rebuild the prefix
+ 1 press for the wrong button
= k
```Như vậy tổng đóng góp của vị trí`k`là:```
k · (n - k)
```Sau khi tất cả các vị trí ngoại trừ vị trí cuối cùng đã được phát hiện, khóa vẫn cần một lần xây dựng thành công toàn bộ chuỗi. Điều đó góp phần`n`nhiều máy ép hơn. 

Tổng hợp mọi thứ mang lại:```
answer = n + Σ k(n - k)
         for k = 1..n-1
```Điều này đã đủ hiệu quả. Chúng ta có thể tính tổng trực tiếp trong`O(n)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng quá trình khám phá Brute Force | O(n²) | O(1) | Đã chấp nhận | 
| Công thức tính toán | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`. 
2. Khởi tạo câu trả lời bằng`n`. 

Đây là những máy ép từ quá trình xây dựng thành công cuối cùng của toàn bộ chuỗi. 
3. Đối với mọi vị trí`k`từ`1`ĐẾN`n - 1`, thêm vào`k × (n - k)`. 

có`n - k`ứng cử viên không chính xác cho`k`-vị trí thứ. Mỗi ứng viên sai sẽ phải trả giá`k`nhấn vì tiền tố đã biết phải được xây dựng lại trước khi kiểm tra nó. 
4. Xuất ra câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Khi xác định`k`-chính xác là nút thứ`n - k`các nút không chính xác. Trong trường hợp xấu nhất, tất cả chúng đều được kiểm tra trước khi tìm thấy cái đúng. 

Mọi thử nghiệm thất bại như vậy đều yêu cầu phải xây dựng lại tiền tố có độ dài đã biết`k - 1`và sau đó nhấn nhầm một nút, với tổng chi phí là`k`máy ép. 

số lượng`k(n - k)`tính toán chính xác tổng chi phí của tất cả các thất bại trong khi khám phá ra`k`-vị trí thứ. Tổng số này trên tất cả các vị trí sẽ tính mỗi lần thử thất bại chính xác một lần. Cuối cùng, sau khi đã biết tất cả các vị trí, để thực hiện thành công toàn bộ chuỗi yêu cầu`n`máy ép. Không có lần nhấn nào bị bỏ qua và không có lần nhấn nào được tính hai lần, vì vậy công thức tạo ra số lần nhấn nút chính xác trong trường hợp xấu nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    ans = n
    for k in range(1, n):
        ans += k * (n - k)

    print(ans)

solve()
```Biến`ans`bắt đầu lúc`n`, đại diện cho nỗ lực mở thành công cuối cùng. 

Vòng lặp xử lý các vị trí`1`bởi vì`n - 1`. Đối với mỗi vị trí`k`, có`n - k`còn lại những ứng cử viên sai. Mỗi ứng viên thất bại đều phải trả giá`k`máy ép, vì vậy chúng tôi thêm`k * (n - k)`. 

Vị trí cuối cùng không cần giai đoạn khám phá. Một lần đầu tiên`n - 1`nút đã biết, nút cuối cùng là bắt buộc. Đó là lý do tại sao vòng lặp dừng lại ở`n - 1`. 

Tràn số nguyên không phải là vấn đề đáng lo ngại trong Python. Ngay cả đối với`n = 2000`, câu trả lời chỉ tầm vài tỷ thôi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
```| k | Đóng góp k(n-k) | Chạy Trả lời | 
| --- | --- | --- | 
| Ban đầu | - | 2 | 
| 1 | 1 × 1 = 1 | 3 | 

Đầu ra:```
3
```Nút đầu tiên có thể yêu cầu một lần thử không thành công trước khi được xác định. Sau đó, một lần nhấn sẽ phát hiện đúng nút đầu tiên và một lần nhấn nữa sẽ mở khóa. 

### Ví dụ 2 

đầu vào:```
3
```| k | Đóng góp k(n-k) | Chạy Trả lời | 
| --- | --- | --- | 
| Ban đầu | - | 3 | 
| 1 | 1 × 2 = 2 | 5 | 
| 2 | 2 × 1 = 2 | 7 | 

Đầu ra:```
7
```Đối với vị trí đầu tiên, hai nút sai có thể được kiểm tra trước khi xác định được nút đúng. Đối với vị trí thứ hai, có thể kiểm tra một nút sai còn lại. Trình tự thành công cuối cùng đóng góp ba lần nhấn. Tổng số trở thành bảy. 

Ví dụ này minh họa rằng việc khám phá các vị trí sau này trở nên tốn kém hơn vì việc xây dựng lại tiền tố đã biết sẽ tốn thêm nhiều lần nhấn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một vòng lặp từ 1 đến n - 1 | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Với`n ≤ 2000`, MỘT`O(n)`giải pháp chỉ thực hiện vài nghìn lần lặp và dễ dàng phù hợp với giới hạn thời gian. Việc sử dụng bộ nhớ là không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    ans = n
    for k in range(1, n):
        ans += k * (n - k)

    return str(ans)

# provided sample
assert run("2\n") == "3", "sample 1"

# custom cases
assert run("1\n") == "1", "single button"
assert run("3\n") == "7", "small example"
assert run("4\n") == "14", "checks middle terms"
assert run("2000\n") == str(
    2000 + sum(k * (2000 - k) for k in range(1, 2000))
), "maximum input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Kích thước tối thiểu | 
|`3`|`7`| Logic đếm cơ bản | 
|`4`|`14`| Tổng hợp chính xác trên nhiều vị trí | 
|`2000`| Giá trị công thức | Hạn chế tối đa | 

## Vỏ cạnh 

### Trường hợp cạnh 1: Nút đơn 

đầu vào:```
1
```Thực thi:```
ans = 1
loop does not run
```Đầu ra:```
1
```Chỉ có một nút duy nhất nên khóa sẽ mở ngay sau một lần nhấn. 

### Trường hợp cạnh 2: Vị trí cuối cùng được tự động biết 

đầu vào:```
2
```Thực thi: 

| k | Đã thêm | Trả lời | 
| --- | --- | --- | 
| Ban đầu | - | 2 | 
| 1 | 1 | 3 | 

Sau khi xác định được nút đầu tiên, nút thứ hai sẽ được xác định tự động vì đây là nút duy nhất còn lại. Thuật toán nắm bắt điều này một cách chính xác bằng cách không thêm chi phí khám phá cho vị trí`n`. 

### Trường hợp Edge 3: Chi phí xây dựng lại tiền tố lớn hơn 

đầu vào:```
4
```Thực thi: 

| k | Đã thêm | Trả lời | 
| --- | --- | --- | 
| Ban đầu | - | 4 | 
| 1 | 3 | 7 | 
| 2 | 4 | 11 | 
| 3 | 3 | 14 | 

Khi tìm kiếm nút thứ ba, mọi lần thử thất bại đều yêu cầu phải xây dựng lại hai nút đã biết trước. yếu tố`k`TRONG`k(n-k)`chiếm chính xác công việc làm thêm này, ngăn chặn việc đếm thiếu.
