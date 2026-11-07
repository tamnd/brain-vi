---
title: "CF 135A - Thay thế"
description: "Chúng ta bắt đầu với một dãy số nguyên dương. Chúng ta phải thay đổi chính xác một phần tử thành một giá trị khác, vẫn nằm trong khoảng từ 1 đến 10^9. Sau lần thay thế duy nhất đó, chúng ta sắp xếp mảng theo thứ tự không giảm."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 135
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 97 (Div. 1)"
rating: 1300
weight: 135
solve_time_s: 94
verified: true
draft: false
---

[CF 135A - Thay thế](https://codeforces.com/problemset/problem/135/A) 

**Đánh giá:** 1300 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một dãy số nguyên dương. Chúng ta phải thay đổi chính xác một phần tử thành một giá trị khác, vẫn ở giữa`1`Và`10^9`. Sau lần thay thế duy nhất đó, chúng ta sắp xếp mảng theo thứ tự không giảm. 

Nhiệm vụ là tạo ra mảng được sắp xếp nhỏ nhất có thể về mặt từ điển sau khi thực hiện chính xác một thay thế. 

Cách diễn đạt trong câu lệnh ban đầu về “các giá trị tối thiểu có thể có cho mỗi vị trí” là một cách khác để mô tả mảng được sắp xếp tối thiểu theo từ điển. Chúng tôi muốn vị trí đầu tiên càng nhỏ càng tốt, sau đó là vị trí thứ hai, v.v. 

Ràng buộc`n ≤ 10^5`ngay lập tức loại trừ bất cứ điều gì bậc hai. Thậm chí đơn giản`O(n^2)`thuật toán sẽ yêu cầu khoảng`10^10`trong trường hợp xấu nhất vượt xa giới hạn. Việc phân loại ở đây đủ rẻ vì`O(n log n)`vì`10^5`các phần tử có thể dễ dàng quản lý bằng Python. 

Phần khó khăn là hiểu được sự thay thế nào thực sự tạo ra mảng được sắp xếp cuối cùng nhỏ nhất. 

Giả sử mảng đã được sắp xếp:```
1 2 3 4 5
```Nếu chúng ta thay thế giá trị lớn nhất`5`với`1`, chúng tôi nhận được:```
1 1 2 3 4
```Điều này rõ ràng là tối ưu vì việc tạo một số nhỏ hơn sẽ giúp sắp xếp thứ tự càng sớm càng tốt. 

Bây giờ hãy xem xét một mảng trong đó mọi phần tử đều đã có sẵn`1`:```
1 1 1
```Chúng tôi buộc phải thay đổi một yếu tố thành một yếu tố khác. Chúng ta không thể thay thế một`1`với người khác`1`. Tùy chọn tốt nhất là tăng tối thiểu một yếu tố:```
1 1 2
```Việc triển khai bất cẩn vẫn có thể cố gắng thay thế một cái gì đó bằng`1`, tạo ra mảng không thay đổi, điều này là bất hợp pháp. 

Một trường hợp tinh tế khác là khi mảng chứa nhiều giá trị tối thiểu nhưng cũng có số lượng lớn hơn:```
1 1 5
```Câu trả lời đúng là:```
1 1 1
```Chúng tôi thay thế`5`với`1`. Chiến lược ngây thơ “luôn tăng một giá trị” sẽ bỏ lỡ rằng chúng ta vẫn có thể làm cho mảng tổng thể nhỏ hơn. 

Sự khác biệt thực sự là liệu mảng đó đã bao gồm toàn bộ`1`s`. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mọi vị trí, hãy thử thay thế phần tử đó bằng mọi giá trị ứng viên có thể quan trọng, sắp xếp mảng kết quả và giữ câu trả lời nhỏ nhất về mặt từ điển. 

Điều này hoạt động về mặt khái niệm vì vấn đề yêu cầu trực tiếp mảng được sắp xếp cuối cùng nhỏ nhất có thể. Nếu chúng ta liệt kê mọi sự thay thế hợp lệ thì một trong số chúng phải là tối ưu. 

Vấn đề là số lượng khả năng. Ngay cả khi chúng ta chỉ thử một tập hợp nhỏ các giá trị thay thế ứng cử viên, việc sắp xếp một mảng có kích thước`n`cho mọi vị trí đã có chi phí`O(n^2 log n)`. Với`n = 10^5`, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chỉ có giá trị thay thế nhỏ nhất có thể mới quan trọng. 

Nếu mảng chứa bất kỳ giá trị nào lớn hơn`1`, sau đó thay thế một giá trị như vậy bằng`1`cải thiện nghiêm ngặt mảng được sắp xếp. Trong số tất cả các lựa chọn, thay thế phần tử lớn nhất là tốt nhất vì nó loại bỏ số lớn nhất trong khi thêm số nhỏ nhất được phép. 

Ví dụ:```
1 2 3 4 5
```trở thành:```
1 1 2 3 4
```sau khi thay thế`5`với`1`. 

Lần duy nhất chiến lược này thất bại là khi mọi phần tử đều bằng nhau`1`. Thế thì chúng ta không thể thay thế bất cứ điều gì bằng`1`, vì giá trị thực sự phải thay đổi. Sửa đổi hợp lệ nhỏ nhất là thay thế một`1`với`2`. 

Vì vậy, chiến lược tối ưu trở nên rất nhỏ: 

1. Sắp xếp mảng. 
2. Nếu tất cả các phần tử đều`1`, thay đổi cái cuối cùng thành`2`. 
3. Nếu không, hãy thay đổi phần tử lớn nhất thành`1`. 
4. Sắp xếp lại nếu cần. 

Vì mảng đã được sắp xếp nên việc sắp xếp cuối cùng được thực hiện ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² log n) hoặc tệ hơn | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(1) bổ sung, không bao gồm sắp xếp | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và sắp xếp theo thứ tự không giảm. 

Việc sắp xếp cho phép chúng ta suy luận trực tiếp về thứ tự từ điển. Các yếu tố nhỏ nhất xuất hiện đầu tiên, do đó việc cải thiện các vị trí trước đó trở nên rõ ràng. 
2. Kiểm tra xem phần tử lớn nhất có bằng không`1`. 

Vì mảng đã được sắp xếp nên điều kiện này có nghĩa là mọi phần tử đều được`1`. 
3. Nếu tất cả các phần tử đều`1`, thay thế phần tử cuối cùng bằng`2`. 

Chúng ta phải sửa đổi chính xác một giá trị và`2`là số hợp lệ nhỏ nhất khác với`1`. 
4. Nếu không, hãy thay thế phần tử cuối cùng, hiện là phần tử lớn nhất, bằng`1`. 

Thao tác này sẽ loại bỏ giá trị lớn nhất và chèn giá trị nhỏ nhất có thể, tạo ra mảng được sắp xếp nhỏ nhất theo từ điển. 
5. Sắp xếp lại mảng nếu cần hoặc chỉ cần in thứ tự sắp xếp đã sửa đổi. 

Sau khi thay phần tử lớn nhất bằng`1`, cái mới`1`thuộc về phía trước. Thực hiện một loại khác là đơn giản và vẫn đủ hiệu quả. 

### Tại sao nó hoạt động 

Thứ tự từ điển được xác định bởi vị trí khác nhau sớm nhất. Để giảm thiểu mảng được sắp xếp cuối cùng, chúng tôi muốn có càng nhiều giá trị nhỏ càng tốt ở đầu. 

Nếu mảng chứa một số giá trị lớn hơn`1`, thay thế một giá trị như vậy bằng`1`tăng số lượng phần tử tối thiểu và loại bỏ số lượng lớn hơn. Việc thay thế giá trị lớn nhất ít nhất cũng tốt như thay thế bất kỳ giá trị nhỏ hơn nào, vì tiền tố sau khi sắp xếp vẫn giữ nguyên trong khi các vị trí sau trở nên nhỏ hơn. 

Nếu mọi phần tử đã có sẵn`1`, việc giảm bất kỳ giá trị nào là không thể. Thay đổi pháp lý nhỏ nhất là tăng chính xác một yếu tố lên`2`, đưa ra kết quả được sắp xếp nhỏ nhất có thể trong số tất cả các sửa đổi hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

a.sort()

if a[-1] == 1:
    a[-1] = 2
else:
    a[-1] = 1

a.sort()

print(*a)
```Giải pháp bắt đầu bằng cách sắp xếp mảng. Điều này làm cho phần tử tối đa dễ dàng truy cập tại`a[-1]`. 

điều kiện`a[-1] == 1`là đủ để xác định xem tất cả các phần tử có`1`, vì mảng đã được sắp xếp. Nếu tối đa là`1`, thì mọi giá trị cũng phải là`1`. 

Khi tất cả các phần tử đều`1`, chúng tôi thay thế phần tử cuối cùng bằng`2`. Việc chọn bất kỳ số lớn hơn nào sẽ tạo ra một mảng tệ hơn về mặt từ điển. 

Ngược lại, chúng ta thay thế phần tử tối đa bằng`1`. Điều này giới thiệu thêm một giá trị tối thiểu trong khi loại bỏ số lớn nhất hiện có. 

Sắp xếp cuối cùng khôi phục thứ tự không giảm. Chi phí của nó vẫn có thể chấp nhận được vì việc phân loại`10^5`số nguyên hai lần dễ dàng nằm trong giới hạn. 

Một lỗi phổ biến là quên rằng việc thay thế phải thực sự thay đổi giá trị. Đối với một mảng toàn bộ, thay thế một`1`với người khác`1`không hợp lệ. 

Một lỗi dễ xảy ra khác là thay thế phần tử không phải một nhỏ nhất thay vì phần tử lớn nhất. Điều đó để lại một số lượng lớn hơn trong mảng và tạo ra kết quả từ vựng kém hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 3 4 5
```| Bước | Trạng thái mảng | 
| --- | --- | 
| Mảng ban đầu | 1 2 3 4 5 | 
| Sau khi sắp xếp | 1 2 3 4 5 | 
| Phần tử lớn nhất không phải là 1 | thay 5 bằng 1 | 
| Mảng sửa đổi | 1 2 3 4 1 | 
| Sau khi sắp xếp cuối cùng | 1 1 2 3 4 | 

Đầu ra cuối cùng:```
1 1 2 3 4
```Dấu vết này cho thấy ý tưởng tham lam chính. Loại bỏ số lớn nhất trong khi cộng`1`tạo ra tiền tố nhỏ nhất có thể sau khi sắp xếp. 

### Ví dụ 2 

đầu vào:```
3
1 1 1
```| Bước | Trạng thái mảng | 
| --- | --- | 
| Mảng ban đầu | 1 1 1 | 
| Sau khi sắp xếp | 1 1 1 | 
| Phần tử lớn nhất là 1 | thay thế 1 cuối cùng bằng 2 | 
| Mảng sửa đổi | 1 1 2 | 
| Sau khi sắp xếp cuối cùng | 1 1 2 | 

Đầu ra cuối cùng:```
1 1 2
```Ví dụ này minh họa trường hợp đặc biệt khi việc giảm bất kỳ giá trị nào là không thể. Thay vào đó, thuật toán thực hiện mức tăng hợp pháp nhỏ nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Thống trị bằng cách sắp xếp | 
| Không gian | O(1) bổ sung, không bao gồm sắp xếp | Chỉ có một vài biến được sử dụng | 

Với`n = 10^5`, MỘT`O(n log n)`giải pháp dễ dàng phù hợp trong thời hạn. Tính năng sắp xếp tích hợp của Python được tối ưu hóa cao và xử lý kích thước đầu vào này một cách thoải mái. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    a.sort()

    if a[-1] == 1:
        a[-1] = 2
    else:
        a[-1] = 1

    a.sort()

    print(*a)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("5\n1 2 3 4 5\n") == "1 1 2 3 4\n", "sample 1"

# minimum size
assert run("1\n1\n") == "2\n", "single element equal to 1"

# single non-one element
assert run("1\n7\n") == "1\n", "replace maximum with 1"

# all equal values
assert run("4\n1 1 1 1\n") == "1 1 1 2\n", "all ones case"

# mixed values
assert run("5\n1 1 5 7 9\n") == "1 1 1 5 7\n", "replace largest value"

# already sorted but with duplicates
assert run("6\n2 2 2 2 2 2\n") == "1 2 2 2 2 2\n", "duplicate non-one values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`2`| Hộp đựng tất cả các kích thước tối thiểu | 
|`1 / 7`|`1`| Mảng không một phần tử | 
|`1 1 1 1`|`1 1 1 2`| Buộc tăng khi mọi giá trị là 1 | 
|`1 1 5 7 9`|`1 1 1 5 7`| Thay thế tối đa là tối ưu | 
|`2 2 2 2 2 2`|`1 2 2 2 2 2`| Xử lý các giá trị lớn lặp đi lặp lại | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1
1
```Mảng được sắp xếp vẫn`[1]`. Vì mọi phần tử đều bằng nhau`1`, thay thế bằng`1`là bất hợp pháp vì giá trị phải thay đổi. Thuật toán thay đổi phần tử duy nhất thành`2`, sản xuất:```
2
```Bây giờ hãy xem xét:```
3
1 1 5
```Sau khi sắp xếp, mảng là:```
1 1 5
```Giá trị lớn nhất không`1`, do đó thuật toán thay thế`5`với`1`:```
1 1 1
```Điều này là tối ưu vì vị trí khác biệt đầu tiên so với bất kỳ câu trả lời hợp lệ nào khác trở nên nhỏ nhất có thể. 

Một trường hợp phức tạp khác là:```
4
2 2 2 2
```Thuật toán thay thế giá trị lớn nhất`2`với`1`:```
1 2 2 2
```Việc thay thế bất kỳ phần tử nào khác sẽ tạo ra nhiều tập hợp giống nhau sau khi sắp xếp, vì vậy việc chọn phần tử cuối cùng là đủ. 

Cuối cùng, hãy xem xét:```
5
1 1 1 1 1000000000
```Thuật toán thay thế giá trị lớn bằng`1`, sản xuất:```
1 1 1 1 1
```Điều này xác nhận rằng độ lớn thực tế của giá trị bị loại bỏ không thành vấn đề. Bất kỳ giá trị nào lớn hơn`1`nên trở thành`1`và loại bỏ mức tối đa sẽ mang lại kết quả từ điển tốt nhất.
