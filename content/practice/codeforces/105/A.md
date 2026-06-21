---
title: "CF 105A - Truyền"
description: "Vấn đề mô phỏng một quá trình chuyển đổi duy nhất trong một trò chơi nhập vai. Chúng ta bắt đầu với một nhân vật đã sở hữu một số kỹ năng. Mỗi kỹ năng có một tên và cấp độ kinh nghiệm. Khi quá trình chuyển đổi xảy ra, mọi kỹ năng hiện có đều bị giảm cấp độ đi một hệ số k."
date: "2026-06-01T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 105
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 81"
rating: 1700
weight: 105
solve_time_s: 186
verified: true
draft: false
---

[CF 105A - Chuyển sinh](https://codeforces.com/problemset/problem/105/A) 

**Đánh giá:** 1700 
**Thẻ:** triển khai 
**Thời gian giải:** 3 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề mô phỏng một quá trình chuyển đổi duy nhất trong một trò chơi nhập vai. 

Chúng ta bắt đầu với một nhân vật đã sở hữu một số kỹ năng. Mỗi kỹ năng có một tên và cấp độ kinh nghiệm. Khi quá trình chuyển đổi xảy ra, mọi kỹ năng hiện có đều bị giảm cấp độ một hệ số`k`. Cấp độ mới trở thành phần nguyên của`k × level`. 

Sau bước giảm này, bất kỳ kỹ năng nào có cấp độ hoàn toàn thấp hơn`100`bị lãng quên và biến mất hoàn toàn. 

Tiếp theo, nhân vật nhận được các kỹ năng thuộc lớp mục tiêu. Nếu nhân vật đã có một trong những kỹ năng đó sau giai đoạn giảm bớt thì không có gì thay đổi. Nếu kỹ năng hiện không có, nó sẽ được thêm vào theo cấp độ`0`. 

Nhiệm vụ cuối cùng là xuất ra tất cả các kỹ năng còn lại sau những chuyển đổi này, được sắp xếp theo từ điển theo tên kỹ năng. 

Những hạn chế là rất nhỏ. Cả số lượng kỹ năng hiện tại và số lượng kỹ năng của lớp mục tiêu đều cao nhất`20`. Ngay cả việc triển khai đơn giản bằng cách quét liên tục các bộ sưu tập cũng sẽ đủ nhanh. Thách thức không phải là độ phức tạp của thuật toán mà là tuân thủ chính xác chuỗi các phép biến đổi được mô tả trong câu lệnh. 

Một số trường hợp tinh tế có thể tạo ra câu trả lời sai nếu các quy tắc được áp dụng sai thứ tự. 

Coi như:```
1 1 0.50
fire 300
fire
```Kỹ năng trở nên`150`sau khi giảm và tồn tại. Vì kỹ năng của lớp đã tồn tại nên nó phải được giữ nguyên`150`. 

Đầu ra đúng:```
1
fire 150
```Việc thực hiện bất cẩn có thể ghi đè lên nó bằng`0`. 

Một trường hợp quan trọng khác là khi một kỹ năng của lớp ban đầu tồn tại nhưng bị lãng quên sau khi giảm bớt:```
1 1 0.50
fire 180
fire
```Giá trị giảm đi là`90`, nên kỹ năng này bị lãng quên. Sau đó lớp cấp`fire`một lần nữa ở cấp độ`0`. 

Đầu ra đúng:```
1
fire 0
```Việc triển khai kiểm tra các kỹ năng của lớp trước khi loại bỏ các kỹ năng bị lãng quên sẽ tạo ra kết quả sai. 

Giá trị biên`100`cũng quan trọng:```
1 0 0.50
fire 200
```Giá trị giảm chính xác là`100`, tồn tại vì chỉ có giá trị nhỏ hơn`100`được gỡ bỏ. 

Đầu ra đúng:```
1
fire 100
```sử dụng`<= 100`thay vì`< 100`sẽ xóa kỹ năng không chính xác. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp ngay lập tức gợi ý chính nó. Chúng ta có thể xử lý mọi kỹ năng hiện có, tính toán mức độ giảm của nó và chỉ giữ lại những kỹ năng có cấp độ mới ít nhất là`100`. Sau đó, chúng tôi xử lý các kỹ năng của lớp mục tiêu và bổ sung bất kỳ kỹ năng nào còn thiếu theo cấp độ.`0`. 

Bởi vì có nhiều nhất hai mươi kỹ năng trong cả hai danh sách, ngay cả cách tiếp cận mạnh mẽ bằng cách sử dụng các bản quét lồng nhau cũng hoàn toàn có thể chấp nhận được. Trường hợp xấu nhất sẽ chỉ liên quan đến vài trăm thao tác. 

Một cách tiếp cận rõ ràng hơn một chút là sử dụng từ điển được khóa theo tên kỹ năng. Từ điển đại diện một cách tự nhiên cho tập hợp các kỹ năng mà nhân vật hiện đang sở hữu. Sau khi áp dụng quy tắc giảm và quên, việc thêm kỹ năng của lớp sẽ trở thành một cuộc kiểm tra tư cách thành viên đơn giản. 

Quan sát quan trọng là vấn đề hoàn toàn là một sự chuyển đổi trạng thái. Không có sự tối ưu hóa, tìm kiếm đồ thị, lập trình động hoặc cấu trúc dữ liệu phức tạp nào liên quan. Chúng ta chỉ cần tuân thủ cẩn thận các quy tắc theo đúng thứ tự đã cho. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n + m)^2) | O(n + m) | Đã chấp nhận | 
| Tối ưu | O(n + m + z log z) | O(n + m) | Đã chấp nhận | 

Đây`z`là số kỹ năng trong đáp án cuối cùng và bước sắp xếp chiếm ưu thế trong thời gian thực hiện. 

## Hướng dẫn thuật toán 

1. Đọc`n`,`m`, và hệ số`k`. 
2. Tạo một từ điển trống để lưu trữ các kỹ năng còn sót lại sau khi chuyển sinh. 
3. Đối với mỗi`n`kỹ năng hiện tại, tính toán:```
new_level = floor(k × old_level)
```Câu lệnh rõ ràng yêu cầu lấy phần nguyên. 
4. Nếu`new_level`ít nhất là`100`, hãy chèn kỹ năng đó vào từ điển với giá trị đó. 

Kỹ năng dưới đây`100`bị lãng quên và không xuất hiện trong từ điển. 
5. Đọc tất cả`m`từng kỹ năng của lớp mục tiêu. 
6. Đối với mỗi kỹ năng của lớp, hãy kiểm tra xem nó đã tồn tại trong từ điển chưa. 

Nếu nó không tồn tại, hãy chèn nó theo cấp độ`0`. 

Nếu nó đã tồn tại, hãy giữ nguyên giá trị hiện tại của nó. 
7. Trích xuất tất cả các mục từ điển và sắp xếp chúng theo từ điển theo tên kỹ năng. 
8. In số lượng kỹ năng rồi in từng kỹ năng`(name, level)`cặp theo thứ tự sắp xếp. 

### Tại sao nó hoạt động 

Sau bước 4, từ điển chứa chính xác các kỹ năng sống sót qua giai đoạn rút gọn, vì mọi kỹ năng ban đầu đều được chuyển hóa theo hệ số và mọi kỹ năng bên dưới`100`được gỡ bỏ. 

Sau bước 6, mọi kỹ năng của lớp mục tiêu được đảm bảo tồn tại trong từ điển. Các kỹ năng sống sót hiện tại vẫn giữ nguyên cấp độ đã giảm, trong khi các kỹ năng lớp còn thiếu sẽ được bổ sung theo cấp độ`0`, khớp hoàn toàn với quy luật luân hồi. 

Vì đầu ra cuối cùng chỉ đơn giản là phiên bản được sắp xếp theo thứ tự từ điển của bộ kỹ năng cuối cùng này nên thuật toán sẽ tạo ra kết quả chính xác duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k_str = input().split()

    n = int(n)
    m = int(m)

    num, den = map(int, k_str.split('.'))
    k_num = num * 100 + den

    skills = {}

    for _ in range(n):
        name, exp = input().split()
        exp = int(exp)

        new_exp = (exp * k_num) // 100

        if new_exp >= 100:
            skills[name] = new_exp

    for _ in range(m):
        name = input().strip()

        if name not in skills:
            skills[name] = 0

    items = sorted(skills.items())

    out = [str(len(items))]
    for name, level in items:
        out.append(f"{name} {level}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện theo mô phỏng trực tiếp. 

Một chi tiết nhỏ là cách hệ số`k`được xử lý. Sử dụng số học dấu phẩy động thường có tác dụng đối với các ràng buộc này, nhưng số học số nguyên sạch hơn và tránh mọi khả năng xảy ra vấn đề về độ chính xác. Từ`k`luôn chứa chính xác hai chữ số thập phân, chúng tôi chuyển đổi nó thành tỷ lệ phần trăm nguyên. Ví dụ,`0.75`trở thành`75`và mức giảm được tính như sau:```
floor(exp × 75 / 100)
```dùng phép chia số nguyên. 

Từ điển lưu trữ bộ kỹ năng hiện tại sau giai đoạn quên. Khi kỹ năng của lớp được xử lý, việc kiểm tra tư cách thành viên đơn giản sẽ xác định xem có nên thêm kỹ năng ở cấp độ hay không`0`. 

Cuối cùng, việc sắp xếp các mục trong từ điển sẽ tạo ra thứ tự từ điển cần thiết. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 4 0.75
axe 350
impaler 300
ionize 80
megafire 120
magicboost 220
heal
megafire
shield
magicboost
```Sau khi xử lý các kỹ năng hiện có: 

| Kỹ năng | Bản gốc | Giảm | Giữ? | 
| --- | --- | --- | --- | 
| rìu | 350 | 262 | Có | 
| xiên | 300 | 225 | Có | 
| ion hóa | 80 | 60 | Không | 
| siêu lửa | 120 | 90 | Không | 
| tăng cường ma thuật | 220 | 165 | Có | 

Từ điển hiện tại: 

| Kỹ năng | Cấp độ | 
| --- | --- | 
| rìu | 262 | 
| xiên | 225 | 
| tăng cường ma thuật | 165 | 

Bây giờ xử lý các kỹ năng của lớp: 

| Kỹ Năng Lớp | Đã có mặt chưa? | Hành động | 
| --- | --- | --- | 
| chữa lành | Không | Thêm 0 | 
| siêu lửa | Không | Thêm 0 | 
| khiên | Không | Thêm 0 | 
| tăng cường ma thuật | Có | Giữ 165 | 

Từ điển cuối cùng: 

| Kỹ năng | Cấp độ | 
| --- | --- | 
| rìu | 262 | 
| chữa lành | 0 | 
| xiên | 225 | 
| tăng cường ma thuật | 165 | 
| siêu lửa | 0 | 
| khiên | 0 | 

Đầu ra được sắp xếp:```
6
axe 262
heal 0
impaler 225
magicboost 165
megafire 0
shield 0
```Ví dụ này chứng minh rằng một kỹ năng có thể biến mất trong quá trình giảm bớt và sau đó được giới thiệu lại dưới dạng kỹ năng đẳng cấp với cấp độ`0`. 

### Ví dụ bổ sung 

đầu vào:```
2 2 0.50
fire 300
ice 180
fire
wind
```Giai đoạn giảm: 

| Kỹ năng | Bản gốc | Giảm | Giữ? | 
| --- | --- | --- | --- | 
| lửa | 300 | 150 | Có | 
| băng | 180 | 90 | Không | 

Từ điển sau khi giảm: 

| Kỹ năng | Cấp độ | 
| --- | --- | 
| lửa | 150 | 

Giai đoạn kỹ năng của lớp: 

| Kỹ Năng Lớp | Đã có mặt chưa? | Kết quả | 
| --- | --- | --- | 
| lửa | Có | Giữ 150 | 
| gió | Không | Thêm 0 | 

Trạng thái cuối cùng: 

| Kỹ năng | Cấp độ | 
| --- | --- | 
| lửa | 150 | 
| gió | 0 | 

Đầu ra:```
2
fire 150
wind 0
```Ví dụ này cho thấy các kỹ năng sống sót hiện tại không bao giờ bị ghi đè bởi một kỹ năng đẳng cấp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m + z log z) | Kỹ năng xử lý là tuyến tính, sắp xếp chi phí danh sách cuối cùng`O(z log z)`| 
| Không gian | O(n + m) | Từ điển lưu trữ tất cả các kỹ năng còn sót lại và bổ sung | 

Từ`n`Và`m`nhiều nhất là cả hai`20`, thời gian chạy thực sự là tức thời. Giải pháp này thấp hơn nhiều so với giới hạn cả về thời gian và mức sử dụng bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import io
import sys

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    output = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = output

    def solve():
        input = sys.stdin.readline

        n, m, k_str = input().split()
        n = int(n)
        m = int(m)

        num, den = map(int, k_str.split('.'))
        k_num = num * 100 + den

        skills = {}

        for _ in range(n):
            name, exp = input().split()
            exp = int(exp)

            val = (exp * k_num) // 100
            if val >= 100:
                skills[name] = val

        for _ in range(m):
            name = input().strip()
            if name not in skills:
                skills[name] = 0

        items = sorted(skills.items())

        print(len(items))
        for name, level in items:
            print(name, level)

    solve()

    sys.stdout = old_stdout
    return output.getvalue()

# sample 1
assert run(
"""5 4 0.75
axe 350
impaler 300
ionize 80
megafire 120
magicboost 220
heal
megafire
shield
magicboost
"""
) == """6
axe 262
heal 0
impaler 225
magicboost 165
megafire 0
shield 0
"""

# minimum input
assert run(
"""1 1 0.50
fire 200
fire
"""
) == """1
fire 100
"""

# forgotten then re-added
assert run(
"""1 1 0.50
fire 180
fire
"""
) == """1
fire 0
"""

# surviving class skill keeps level
assert run(
"""1 1 0.50
fire 300
fire
"""
) == """1
fire 150
"""

# all skills forgotten, class introduces new ones
assert run(
"""2 2 0.01
a 9999
b 5000
c
d
"""
) == """2
c 0
d 0
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | Câu trả lời chính thức | Hành vi mô phỏng đầy đủ | 
| Kỹ năng đơn giảm xuống 100 | Kỹ năng sống sót | Chính xác`< 100`kiểm tra | 
| Quên rồi thêm lại | Kỹ năng cấp 0 | Đúng thứ tự thao tác | 
| Kỹ năng sống sót của lớp | Mức giảm ban đầu | Không vô tình ghi đè | 
| Hệ số nhỏ | Chỉ còn lại các kỹ năng đẳng cấp | Hành vi quên hoàn toàn | 

## Vỏ cạnh 

Một kỹ năng bị giảm giá trị sẽ trở nên chính xác`100`phải sống sót. Coi như:```
1 0 0.50
fire 200
```Giá trị giảm đi là`100`. Vì chỉ có các giá trị bên dưới`100`bị lãng quên, đầu ra cuối cùng là:```
1
fire 100
```Việc thực hiện sử dụng`>= 100`, xử lý ranh giới này một cách chính xác. 

Một kỹ năng đẳng cấp có thể đã tồn tại sau giai đoạn giảm bớt. Coi như:```
1 1 0.50
fire 300
fire
```Giá trị giảm đi là`150`, vì vậy kỹ năng này vẫn tồn tại. Khi xử lý các kỹ năng của lớp, từ điển đã chứa`"fire"`, nên không có gì thay đổi. Kết quả cuối cùng vẫn là:```
1
fire 150
```Điều này ngăn chặn sự thay thế ngẫu nhiên với mức`0`. 

Một kỹ năng có thể biến mất và sau đó được cấp lại bởi lớp mới:```
1 1 0.50
fire 180
fire
```Giá trị giảm trở thành`90`, nên kỹ năng này bị lãng quên. Việc xử lý kỹ năng của lớp sau đó sẽ thấy điều đó`"fire"`vắng mặt và chèn nó theo cấp độ`0`. Kết quả là:```
1
fire 0
```Điều này xác minh rằng việc quên xảy ra trước khi thêm các kỹ năng dành riêng cho từng lớp, chính xác theo yêu cầu của tuyên bố.
