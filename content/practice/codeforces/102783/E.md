---
title: "CF 102783E - Những con số ma quái"
description: "Francine có một tập hợp các chữ số thập phân và muốn sắp xếp một số trong số chúng thành số ma quái lớn nhất có thể. Số ma quái là số nguyên không âm chia hết cho 2, 3 và 5, nghĩa là nó phải chia hết cho 30."
date: "2026-07-27T19:58:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102783
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 10-23-20 Div. 2"
rating: 0
weight: 102783
solve_time_s: 50
verified: true
draft: false
---

[CF 102783E - Những con số ma quái](https://codeforces.com/problemset/problem/102783/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Francine có một tập hợp các chữ số thập phân và muốn sắp xếp một số trong số chúng thành số ma quái lớn nhất có thể. Số ma quái là số nguyên không âm chia hết cho 2, 3 và 5, nghĩa là nó phải chia hết cho 30. Cô ấy có thể loại bỏ các chữ số, nhưng các chữ số còn lại phải được sử dụng đúng một lần trong số đã chọn và số kết quả không được chứa các số 0 đứng đầu không cần thiết. Nhiệm vụ là in số hợp lệ lớn nhất có thể tạo được, hoặc`-1`nếu không có số hợp lệ tồn tại. 

Các quy tắc chia hết là những hạn chế chính của thuật toán. Số chia hết cho 30 tương đương với số chia hết cho 3 và có chữ số tận cùng là 0. Vì một số có tối đa`100000`các chữ số không thể được xử lý bằng các phép toán số nguyên thông thường, bất kỳ cách tiếp cận nào cố gắng xây dựng và kiểm tra nhiều số có thể đều là không thể. Ngay cả việc tạo ra một phần nhỏ hoán vị cũng sẽ vượt xa thời gian có sẵn. Giải pháp dự định cần hoạt động bằng cách đếm các chữ số và sử dụng các thuộc tính chia hết thay vì thực hiện số học trên toàn bộ số. 

Số hợp lệ lớn nhất phải bảo toàn càng nhiều chữ số lớn càng tốt đồng thời thỏa mãn các điều kiện chia hết. Việc thực hiện bất cẩn có thể thất bại trong những trường hợp nhỏ nhất. Ví dụ:```
Input
1
0
```Đầu ra đúng là:```
0
```Một giải pháp từ chối tất cả các số có một chữ số vì nó cho rằng chữ số ở đầu khác 0 sẽ in không chính xác`-1`. 

Một trường hợp quan trọng khác là khi các chữ số không chia hết cho 3. Ví dụ:```
Input
6
2 3 1 3 1 0
```Đầu ra đúng là:```
33210
```Sử dụng mỗi chữ số sẽ mang lại`332110`, có tổng các chữ số không chia hết cho 3. Giải pháp tham lam luôn giữ mọi chữ số sẽ không thành công, vì vậy một số chữ số phải bị loại bỏ. 

Trường hợp cạnh thứ ba là khi thiếu chữ số kết thúc duy nhất có thể. Ví dụ:```
Input
4
1 2 3 4
```Đầu ra đúng là:```
-1
```Không có sự sắp xếp nào có thể kết thúc trong`0`, vì vậy không có số nào có thể chia hết cho 30. Chỉ sắp xếp các chữ số mà không kiểm tra điều kiện này sẽ tạo ra câu trả lời không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử các tập con chữ số có thể có, sắp xếp từng tập con theo thứ tự giảm dần và kiểm tra xem số kết quả có chia hết cho 30 hay không. Điều này đúng vì cách sắp xếp hợp lệ lớn nhất cuối cùng sẽ xuất hiện trong số tất cả các lựa chọn. Vấn đề là số lượng khả năng. Với`n`chữ số, có`2^n`các tập hợp con và thậm chí đối với vài trăm chữ số thì điều này là không thể. Vì`n = 100000`, vũ lực không thể thực hiện được từ xa. 

Cấu trúc của bài toán đưa ra một con đường đơn giản hơn nhiều. Một số chia hết cho 30 phải có tận cùng bằng 0 vì bắt buộc phải chia hết cho 10. Nó cũng phải có tổng chữ số chia hết cho 3. Số 0 ở cuối là yêu cầu cố định và nhiệm vụ còn lại chỉ là loại bỏ lượng giá trị chữ số nhỏ nhất cần thiết để làm cho tổng chữ số chính xác. 

Giả sử chúng ta giữ lại tất cả các chữ số có sẵn. Nếu tổng các chữ số đã chia hết cho 3 thì chúng ta chỉ cần sắp xếp tất cả các chữ số giảm dần. Nếu phần còn lại là 1, chúng ta có thể xóa một chữ số có giá trị còn lại 1 hoặc xóa hai chữ số có giá trị còn lại là 2. Nếu phần còn lại là 2 thì lựa chọn đối xứng sẽ được áp dụng. Để tối đa hóa số cuối cùng, việc loại bỏ ít chữ số hơn luôn tốt hơn và nếu chúng ta phải loại bỏ cùng một số chữ số thì việc loại bỏ các chữ số nhỏ nhất có thể sẽ cho kết quả lớn nhất. 

Các quy tắc chia hết làm giảm toàn bộ vấn đề để duy trì số lượng mười chữ số có thể. Sắp xếp các chữ số cuối cùng theo thứ tự giảm dần sẽ cho giá trị số lớn nhất vì mọi chữ số được giữ lại đều được sử dụng và các chữ số lớn hơn xuất hiện sớm hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lần mỗi chữ số xuất hiện và tính tổng các chữ số. Số lượng là đủ vì các chữ số có cùng giá trị có thể hoán đổi cho nhau. 
2. Kiểm tra xem số 0 có tồn tại không. Nếu không có số 0 thì không có số nào chia hết cho 30 nên không thể trả lời được. Số 0 được yêu cầu làm chữ số cuối cùng. 
3. Tính phần còn lại của tổng chữ số theo modulo 3. Nếu nó khác 0, hãy xóa các chữ số cố định phần dư. Nên loại bỏ một chữ số vì nó giữ được nhiều chữ số hơn. Nếu không thể, hãy xóa hai chữ số có phần dư kết hợp là chính xác. 
4. Sau khi loại bỏ các chữ số cần thiết, thu thập tất cả các chữ số còn lại theo thứ tự giảm dần. Các chữ số lớn nhất phải xuất hiện đầu tiên vì số được so sánh theo từ điển khi nó có cùng độ dài. 
5. Xử lý trường hợp đặc biệt khi tất cả các chữ số còn lại đều bằng 0. Câu trả lời phải là duy nhất`0`, không phải là một chuỗi chứa nhiều số 0, vì những số 0 thừa đó không làm tăng giá trị. 

Tại sao nó hoạt động: Điều kiện duy nhất để chia hết cho 30 là tận cùng bằng 0 và có tổng chữ số chia hết cho 3. Thuật toán luôn giữ thỏa mãn yêu cầu bằng 0 và loại bỏ số chữ số tối thiểu có thể có để sửa tổng chữ số. Trong số tất cả các cách có cùng số chữ số bị loại bỏ, nó loại bỏ các chữ số nhỏ nhất, để lại sự sắp xếp lớn nhất có thể về mặt từ điển. Sắp xếp các chữ số còn lại giảm dần sẽ tạo ra số lớn nhất thỏa mãn cả hai điều kiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(data):
    arr = data.split()
    if not arr:
        return ""

    n = int(arr[0])
    digits = list(map(int, arr[1:]))

    cnt = [0] * 10
    total = 0

    for d in digits:
        cnt[d] += 1
        total += d

    if cnt[0] == 0:
        return "-1"

    def remove_digit_with_remainder(rem):
        for d in range(1, 10):
            if d % 3 == rem and cnt[d] > 0:
                cnt[d] -= 1
                return True
        return False

    def remove_two_with_remainder(rem):
        for d in range(1, 10):
            if d % 3 == rem and cnt[d] > 0:
                for e in range(d, 10):
                    if e % 3 == rem and cnt[e] > 0:
                        if d == e and cnt[d] < 2:
                            continue
                        cnt[d] -= 1
                        cnt[e] -= 1
                        return True
        return False

    remainder = total % 3

    if remainder == 1:
        if not remove_digit_with_remainder(1):
            if not remove_two_with_remainder(2):
                return "-1"
    elif remainder == 2:
        if not remove_digit_with_remainder(2):
            if not remove_two_with_remainder(1):
                return "-1"

    ans = []
    for d in range(9, -1, -1):
        ans.append(str(d) * cnt[d])

    result = "".join(ans)

    if result == "":
        return "-1"

    if result[0] == "0":
        return "0"

    return result

if __name__ == "__main__":
    print(solve(sys.stdin.read()))
```Giải pháp đầu tiên lưu trữ tần số chữ số thay vì lưu trữ một số nguyên lớn. Điều này tránh hoàn toàn việc chuyển đổi số nguyên vì số đó có thể chứa`100000`chữ số. 

Trình trợ giúp loại bỏ tìm kiếm các chữ số theo phần còn lại của chúng theo modulo 3. Trình trợ giúp đầu tiên xóa một chữ số, trong khi trình trợ giúp thứ hai xóa hai chữ số. Các chữ số được kiểm tra từ nhỏ đến lớn vì việc loại bỏ các chữ số nhỏ hơn sẽ giữ được số cuối cùng lớn hơn. 

Sau khi cố định tổng chữ số, giai đoạn xây dựng lặp lại từ`9`xuống`0`. Việc lặp lại từng chữ số theo số đếm còn lại của nó sẽ tạo ra thứ tự lớn nhất có thể. Việc kiểm tra bằng 0 sau khi xây dựng xử lý các trường hợp như`0000`, trong đó biểu diễn chính xác chỉ là`0`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
0
```| Bước | Số chữ số | Tổng số dư | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| Bắt đầu |`0:1`| 0 | Không tồn tại | Tiếp tục | 
| Xây dựng |`0`| 0 | Sắp xếp giảm dần |`0`| 

Chữ số duy nhất có sẵn đã đáp ứng mọi điều kiện. Điều này xác nhận trường hợp một cạnh bằng 0. 

### Ví dụ 2 

đầu vào:```
6
2 3 1 3 1 0
```| Bước | Số chữ số | Tổng hợp | Phần còn lại | Hành động | 
| --- | --- | --- | --- | --- | 
| Bắt đầu |`0:1, 1:2, 2:1, 3:2`| 10 | 1 | Cần xóa chữ số còn lại 1 | 
| Xóa |`1:1`vẫn bị loại bỏ | 9 | 0 | Chia hết cho 3 | 
| Xây dựng |`3 3 2 1 0`| 9 | 0 | đầu ra`33210`| 

Dấu vết cho thấy tại sao việc loại bỏ chữ số nhỏ nhất có thể là chính xác. Loại bỏ một`1`sửa lỗi chia hết trong khi vẫn giữ tất cả các chữ số lớn hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chữ số đầu vào được tính một lần và cấu trúc cuối cùng chỉ xử lý các giá trị mười chữ số. | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ các bộ đếm có mười chữ số và một vài biến. | 

Kích thước đầu vào có thể đạt tới`100000`chữ số, do đó cần phải xử lý tuyến tính. Giải pháp này dễ dàng phù hợp vì nó không bao giờ thực hiện các phép tính tỷ lệ với kích thước của số bình phương hoặc bất kỳ chuyển đổi nào thành số nguyên. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    import collections

    def solve(data):
        arr = data.split()
        if not arr:
            return ""
        n = int(arr[0])
        digits = list(map(int, arr[1:]))

        cnt = [0] * 10
        total = 0
        for d in digits:
            cnt[d] += 1
            total += d

        if cnt[0] == 0:
            return "-1"

        def remove_one(rem):
            for d in range(1, 10):
                if d % 3 == rem and cnt[d]:
                    cnt[d] -= 1
                    return True
            return False

        def remove_two(rem):
            for d in range(1, 10):
                if d % 3 == rem and cnt[d]:
                    for e in range(d, 10):
                        if e % 3 == rem and cnt[e] - (d == e) > 0:
                            cnt[d] -= 1
                            cnt[e] -= 1
                            return True
            return False

        r = total % 3
        if r == 1:
            if not remove_one(1) and not remove_two(2):
                return "-1"
        elif r == 2:
            if not remove_one(2) and not remove_two(1):
                return "-1"

        ans = "".join(str(d) * cnt[d] for d in range(9, -1, -1))
        return "0" if ans and ans[0] == "0" else ans

    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    res = solve(sys.stdin.read())
    sys.stdin = old
    return res

assert run("1\n0\n") == "0", "sample 1"
assert run("6\n2 3 1 3 1 0\n") == "33210", "sample 2"

assert run("1\n5\n") == "-1", "no zero"
assert run("3\n0 0 0\n") == "0", "all zeros"
assert run("5\n9 8 7 6 0\n") == "98760", "already divisible"
assert run("4\n1 1 1 0\n") == "110", "remove one digit"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 5`|`-1`| Thiếu số 0 bắt buộc | 
|`3 / 0 0 0`|`0`| Chuẩn hóa hoàn toàn bằng không | 
|`5 / 9 8 7 6 0`|`98760`| Giữ tất cả các chữ số khi đã hợp lệ | 
|`4 / 1 1 1 0`|`110`| Loại bỏ đúng cho tổng chữ số chia hết | 

## Vỏ cạnh 

Đối với đầu vào:```
1
0
```thuật toán thấy rằng số 0 tồn tại và tổng chữ số còn lại đã bằng 0. Giai đoạn xây dựng tạo ra`"0"`. Việc xử lý đặc biệt ngăn không cho đầu ra trở thành một chuỗi số 0 dài hơn. 

Đối với đầu vào:```
6
2 3 1 3 1 0
```tổng số tiền là`10`, để lại phần dư`1`khi chia cho`3`. Thuật toán tìm kiếm một chữ số có số dư`1`và loại bỏ`1`. Các chữ số còn lại có tổng`9`và sắp xếp chúng mang lại`33210`. 

Đối với đầu vào:```
4
1 2 3 4
```bộ đếm số 0 trống, do đó thuật toán dừng ngay lập tức với`-1`. Không có bước sắp xếp nào sau này có thể khắc phục được điều này vì mọi số chia hết cho 30 đều phải có kết thúc bằng 0.
