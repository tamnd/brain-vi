---
title: "CF 102284L - \u0412\u044b\u0431\u043e\u0440 \u0432\u0435\u0447\u0451\u0440\u043a\u0438"
description: "Có (n) học sinh, và mỗi học sinh nêu tên một trong (k) loại đồ uống có thể dùng. Andrew có các gói chính xác (lceil n/2rceil). Mỗi gói có hai nửa phần, vì vậy một gói có thể đáp ứng tối đa hai học sinh và cả hai học sinh đó đều phải yêu cầu cùng một loại đồ uống."
date: "2026-08-13T09:02:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102284
codeforces_index: "L"
codeforces_contest_name: "\u041b\u041a\u0428 2019, \u0418\u044e\u043b\u044c, \u041c\u0438\u043a\u0441 \u0441\u0442\u0430\u0440\u0448\u0435\u0439 \u0438 \u043c\u043b\u0430\u0434\u0448\u0435\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434"
rating: 0
weight: 102284
solve_time_s: 192
verified: true
draft: false
---

[CF 102284L - \u0412\u044b\u0431\u043e\u0440 \u0432\u0435\u0447\u0451\u0440\u043a\u0438](https://codeforces.com/problemset/problem/102284/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có (n) học sinh, và mỗi học sinh nêu tên một trong (k) loại đồ uống có thể dùng. Andrew có các gói chính xác (\lceil n/2\rceil). Mỗi gói có hai nửa phần, vì vậy một gói có thể đáp ứng tối đa hai học sinh và cả hai học sinh đó đều phải yêu cầu cùng một loại đồ uống. 

Andrew có thể tự do lựa chọn loại đồ uống nào trong mỗi gói. Mục tiêu không phải là làm cho mọi học sinh hài lòng mà là để tối đa hóa số lượng học sinh có đồ uống yêu cầu thực sự được cung cấp cho họ. 

Giả sử một loại đồ uống cụ thể được yêu cầu bởi (c) học sinh. Để thỏa mãn tất cả bọn họ, Andrew cần (\lceil c/2\rceil) gói đồ uống đó. Hai học sinh có cùng yêu cầu có thể dùng chung một gói, trong khi một nhóm có số lượng lẻ cần một gói với một nửa chưa sử dụng. 

Khó khăn chính là mỗi đồ uống có số lượng yêu cầu lẻ sẽ tiêu thụ nhiều hơn nửa gói so với mức mà học sinh đề xuất. Chúng ta cần chọn loại đồ uống phù hợp để tổng số gói yêu cầu không vượt quá (\lceil n/2\rceil). 

Các ràng buộc đủ nhỏ để (O(nk)) có thể chấp nhận được, vì cả (n) và (k) đều nhiều nhất là (1000). Tuy nhiên, cấu trúc của bài toán đưa ra nghiệm (O(n+k)). Chúng tôi chỉ cần tần suất của mỗi đồ uống được yêu cầu và sau đó là số tần suất lẻ. 

Có hai trường hợp dễ xảy ra khi giải pháp bất cẩn có thể thất bại. Đầu tiên, số lượng học sinh lẻ không có nghĩa là mọi nhóm đồ uống có số lượng lẻ đều gây ra vấn đề. Ví dụ,```
1 1
1
```có một học sinh và một gói nên đáp án là (1). Andrew đơn giản có thể lấy nửa gói còn sót lại. 

Trường hợp thú vị hơn là một số nhóm có quy mô lẻ. Coi như```
4 4
1
2
3
4
```Mỗi đồ uống được yêu cầu một lần. Chỉ có hai gói nên nhiều nhất hai trong số bốn học sinh có thể nhận được đồ uống mà mình mong muốn. Câu trả lời đúng là (2). Một giải pháp chỉ tính tổng tất cả các yêu cầu sẽ cho rằng không chính xác rằng cả bốn yêu cầu đều có thể được đáp ứng. 

Tính chẵn lẻ của (n) cũng có vấn đề. Vì```
5 3
1
3
1
1
2
```số lượng yêu cầu là (3,1,1). Có ba gói. Chúng ta có thể sử dụng hai gói đựng đồ uống (1), làm hài lòng ba học sinh và một gói đựng đồ uống (2) hoặc đồ uống (3), thỏa mãn thêm một học sinh. Câu trả lời là (4), không phải (3). 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp là quyết định loại đồ uống nào Andrew sẽ đưa vào lựa chọn buổi tối. Đối với mỗi tập hợp con của (k) đồ uống, chúng ta có thể tính toán số lượng gói được yêu cầu. Nếu đồ uống (i) được yêu cầu (c_i) lần và được chọn, nó sẽ tính phí (\lceil c_i/2\rceil) gói và đóng góp (c_i) làm học sinh hài lòng. Chúng tôi giữ tập hợp con tốt nhất có giá gói tối đa (\lceil n/2\rceil). 

Lực lượng vũ phu này là chính xác bởi vì mọi lựa chọn loại đồ uống có thể có đều được xem xét rõ ràng. Vấn đề là số lượng tập hợp con. Có (2^k) trong số đó và với (k=1000) thì đã có (2^{1000}) khả năng. Ngay cả khi chi phí của mỗi tập hợp con được duy trì theo thời gian không đổi thì việc tìm kiếm vẫn vô vọng. Việc triển khai đơn giản quét tất cả (k) loại đồ uống cho mỗi tập hợp con sẽ hoạt động (O(k2^k)), khoảng (1000\cdot2^{1000}), vượt xa mọi giới hạn thực tế. 

Quan sát hữu ích là mỗi nhóm đồ uống đều có mối quan hệ chi phí trên giá trị rất đặc biệt. Nếu (c) chẵn thì 

[ 
\left\lceil\frac c2\right\rceil=\frac c2, 
] 

vì vậy việc đáp ứng những sinh viên đó tốn chính xác bằng một nửa số gói. Nếu (c) lẻ thì 

[ 
\left\lceil\frac c2\right\rceil=\frac{c+1}{2}. 
] 

Do đó, mỗi nhóm có quy mô lẻ giới thiệu chính xác một gói bổ sung so với chi phí lý tưởng là (c/2). 

Gọi (O) là số loại đồ uống có số lượng yêu cầu là số lẻ. Nếu chúng ta cố gắng làm hài lòng tất cả mọi người thì số kiện hàng yêu cầu sẽ là 

\frac{n+O}{2}. 
] 

Chúng tôi chỉ có các gói (\lceil n/2\rceil). Phần dư thừa chính xác là số lượng nhóm lẻ phải xử lý. 

Thay vì chọn các tập con tùy ý, chúng ta có thể nghĩ đến việc loại bỏ các học sinh khỏi tập các học sinh hài lòng. Việc loại bỏ một học sinh khỏi nhóm lẻ sẽ thay đổi nhóm đó thành nhóm chẵn, giảm số lượng nhóm lẻ đi một. Việc loại bỏ một học sinh khác khỏi cùng nhóm sẽ khiến mọi chuyện trở nên kỳ quặc trở lại, vì vậy việc làm đó chẳng có ích lợi gì trong khi chúng tôi đang cố gắng khắc phục tình trạng thiếu gói. 

Với số chẵn (n) thì số (O) là số chẵn. Chúng ta cần loại bỏ (O/2) sinh viên, mỗi sinh viên trong mỗi nhóm lẻ (O/2). Đối với (n) lẻ, (O) là số lẻ và một nhóm lẻ có thể sử dụng nửa gói duy nhất còn lại. Chúng ta chỉ cần loại bỏ ((O-1)/2) sinh viên. 

Điều này đưa ra một công thức trực tiếp. Với số chẵn (n), câu trả lời là 

[ 
n-\frac O2. 
] 

Đối với số lẻ (n), câu trả lời là 

[ 
n-\frac{O-1}{2}. 
] 

Tương tự, chúng ta có thể tính số học sinh phải hy sinh là (\lfloor O/2\rfloor), vì chẵn lẻ của (O) giống với chẵn lẻ của (n). Vì vậy câu trả lời chỉ đơn giản là 

[ 
\boxed{n-\left\lfloor\frac O2\right\rfloor}. 
] 

Toàn bộ vấn đề bây giờ đã được rút gọn thành việc đếm các tần số và đếm xem có bao nhiêu tần số lẻ trong số đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đối với tập hợp đồ uống | (O(k2^k)) | (O(k)) | Quá chậm | 
| Đếm tần số và nhóm lẻ | (O(n+k)) | (O(k)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`cnt`có kích thước (k+1), trong đó`cnt[x]`sẽ lưu trữ bao nhiêu sinh viên yêu cầu uống (x). Đếm tần số là đủ vì danh tính và thứ tự của học sinh không ảnh hưởng đến cách phân phát các gói hàng. 
2. Đọc tất cả (n) yêu cầu và tăng tần số tương ứng. Sau bước này, toàn bộ dữ liệu đầu vào được biểu thị bằng các giá trị (k) (c_1,c_2,\ldots,c_k). 
3. Đếm xem có bao nhiêu tần số lẻ. Gọi số này`odd`. Mỗi nhóm như vậy cần thêm một nửa gói so với một nhóm chẵn có quy mô tương đương. 
4. Tính toán`odd // 2`. Đây là số lượng sinh viên tối thiểu phải không hài lòng. Việc ghép các nhóm lẻ thành từng cặp sẽ giải thích giá trị này: từ mỗi cặp nhóm lẻ, phải loại bỏ một học sinh để cả hai nhóm có thể được xử lý bằng các gói có sẵn. 
5. Đầu ra`n - odd // 2`. Tính chẵn lẻ của (n) đã xác định liệu một nhóm lẻ có thể sử dụng nửa gói còn sót lại hay không, do đó không cần thêm trường hợp đặc biệt nào. 

Tại sao nó hoạt động. Giả sử số lượng yêu cầu là (c_1,\ldots,c_k) và (O) trong số đó là số lẻ. Nếu chúng ta đáp ứng được mọi học sinh thì số gói cần thiết là ((n+O)/2). Ngân sách của chúng ta là (\lceil n/2\rceil), vì vậy mỗi cặp nhóm lẻ sẽ tạo ra chính xác một gói nhu cầu vượt mức. Việc loại bỏ một học sinh khỏi một nhóm trong một cặp như vậy sẽ làm cho nhóm đó đồng đều và loại bỏ một đơn vị thừa. Do đó ít nhất (\lfloor O/2\rfloor) học sinh phải bị hy sinh. Chúng ta có thể đạt được chính xác điều này bằng cách chọn một học sinh từ một nhóm trong mỗi cặp nhóm lẻ. Sau những lần loại bỏ đó, nhiều nhất vẫn còn một nhóm lẻ khi (n) là số lẻ, phù hợp với gói được sử dụng một phần. Do đó, giới hạn dưới có thể đạt được và câu trả lời là chính xác (n-\lfloor O/2\rfloor). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    cnt = [0] * (k + 1)

    for _ in range(n):
        drink = int(input())
        cnt[drink] += 1

    odd = sum(c & 1 for c in cnt)

    print(n - odd // 2)

if __name__ == "__main__":
    solve()
```Mảng`cnt`lưu trữ tần suất của mỗi lần uống. Vì số lượng đồ uống nằm trong khoảng từ (1) đến (k), việc lập chỉ mục trực tiếp sẽ đơn giản và nhanh hơn so với việc sử dụng từ điển. 

biểu thức`c & 1`kiểm tra xem tần số có phải là số lẻ hay không. Nó tương đương với`c % 2`, nhưng làm cho phép toán chẵn lẻ trở nên rõ ràng. Tổng các giá trị này sẽ cho chính xác số (O) từ chứng minh. 

Biểu thức cuối cùng`n - odd // 2`xử lý cả hai chẵn lẻ của (n). Khi (n) chẵn,`odd`nhất thiết phải chẵn, vì vậy`odd // 2`chính xác là số học sinh cần phải loại bỏ. Khi (n) lẻ,`odd`nhất thiết phải là số lẻ và phép chia số nguyên sẽ cho ((O-1)/2), để lại một nhóm lẻ để sử dụng nửa gói còn lại. 

Không có thủ thuật lập chỉ mục nào liên quan đến số lượng đồ uống ngoài việc phân bổ`k + 1`các phần tử, vì vậy đồ uống (k) được biểu diễn một cách an toàn. Số nguyên Python cũng không gặp vấn đề tràn đối với các ràng buộc này. 

## Ví dụ đã hoạt động 

Đối với mẫu được cung cấp, các yêu cầu là`1, 3, 1, 1, 2`. Tần số trở thành (3,1,1), vì vậy cả ba loại đồ uống đều có tần số lẻ. 

| Uống | Tần số | Số lẻ? | 
| --- | --- | --- | 
| 1 | 3 | vâng | 
| 2 | 1 | vâng | 
| 3 | 1 | vâng | 
| Tổng số nhóm lẻ | 3 | | 

Từ`odd = 3`, chúng ta cần phải rời đi`3 // 2 = 1`sinh viên không hài lòng. Kết quả là (5-1=4). 

Cụ thể, hai gói đồ uống (1) có thể làm hài lòng ba học sinh yêu cầu đồ uống (1), vì một gói phục vụ hai người trong số họ và gói thứ hai phục vụ sinh viên còn lại. Gói thứ ba có thể đáp ứng học sinh yêu cầu đồ uống (2) hoặc học sinh yêu cầu đồ uống (3). Do đó, bốn học sinh có thể nhận được những gì họ yêu cầu. 

Ví dụ thứ hai, hãy xem xét bốn sinh viên đều yêu cầu các loại đồ uống khác nhau.```
4 4
1
2
3
4
```Dấu vết tần số là: 

| Uống | Tần số | Số lẻ? | 
| --- | --- | --- | 
| 1 | 1 | vâng | 
| 2 | 1 | vâng | 
| 3 | 1 | vâng | 
| 4 | 1 | vâng | 
| Tổng số nhóm lẻ | 4 | | 

Đây`odd // 2`bằng (2), nên đáp án là (4-2=2). Chỉ có hai gói và mỗi yêu cầu đơn lẻ cần có gói riêng. Chọn bất kỳ hai loại đồ uống nào đều làm hài lòng chính xác hai học sinh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+k)) | Đọc và đếm (n) yêu cầu, sau đó kiểm tra tần số (k+1) | 
| Không gian | (O(k)) | Lưu trữ một tần suất cho mỗi lần uống | 

Với (n,k\le1000), thao tác này chỉ thực hiện được vài nghìn thao tác cơ bản. Giải pháp này nằm trong giới hạn đã nêu và sử dụng rất ít bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_data(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)

    n = next(it)
    k = next(it)

    cnt = [0] * (k + 1)

    for _ in range(n):
        x = next(it)
        cnt[x] += 1

    odd = sum(c & 1 for c in cnt)

    return str(n - odd // 2)

# provided sample
assert solve_data(
    """5 3
1
3
1
1
2
"""
) == "4", "sample 1"

# minimum-size input
assert solve_data(
    """1 1
1
"""
) == "1", "single student"

# all students want the same drink
assert solve_data(
    """6 3
2
2
2
2
2
2
"""
) == "6", "all equal"

# even n, every drink requested once
assert solve_data(
    """4 4
1
2
3
4
"""
) == "2", "four odd groups"

# odd n, three odd groups
assert solve_data(
    """5 3
1
2
3
1
1
"""
) == "4", "odd n with three odd groups"

# maximum-size input, all students request one drink
max_case = "1000 1000\n" + "1\n" * 1000
assert solve_data(max_case) == "1000", "maximum n and k"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`1`| Kích thước tối thiểu và nửa gói còn sót lại | 
|`6 3 / 2 2 2 2 2 2`|`6`| Tất cả các yêu cầu đều như nhau nên không lãng phí gói hàng | 
|`4 4 / 1 2 3 4`|`2`| Một số nhóm lẻ và ranh giới chẵn-(n) | 
|`5 3 / 1 2 3 1 1`|`4`| Lẻ (n), trong đó một nhóm lẻ có thể sử dụng nửa còn lại | 
|`n=1000`, tất cả các yêu cầu đều bằng nhau |`1000`| Kích thước đầu vào tối đa và đếm tần số | 

## Vỏ cạnh 

Đầu vào nhỏ nhất có thể là```
1 1
1
```Có một gói và một sinh viên. Học sinh nhận được một nửa gói hàng, còn Andrew giữ nửa còn lại. Tần số duy nhất là (1), vì vậy`odd = 1`Và`odd // 2 = 0`. Thuật toán đưa ra (1), là tối ưu. 

Khi tất cả học sinh yêu cầu cùng một loại đồ uống, chỉ có một tần số và lẻ chính xác khi (n) lẻ. Ví dụ,```
5 2
1
1
1
1
1
```cho`odd = 1`, vậy đáp án là (5). Hai gói có thể phục vụ hoàn toàn bốn học sinh và gói thứ ba có thể phục vụ học sinh thứ năm, nửa còn lại dành cho Andrew. 

Trường hợp có nhiều nhóm lẻ là nơi công thức chính có ý nghĩa quan trọng. Vì```
6 6
1
2
3
4
5
6
```cả sáu tần số đều là số lẻ, vì vậy`odd = 6`. Câu trả lời là (6-6/2=3). Có ba gói và mỗi yêu cầu đơn lẻ được đáp ứng cần một gói. Chọn ba trong số sáu đồ uống làm hài lòng ba học sinh. 

Cuối cùng, ranh giới lẻ-(n) khác với trường hợp chẵn-(n). Vì```
5 3
1
2
3
1
1
```tần số là (3,1,1), cho`odd = 3`. Thuật toán chỉ loại bỏ`3 // 2 = 1`sinh viên và trả về (4). Lý do là sau khi đáp ứng được ba học sinh yêu cầu đồ uống (1), một gói bổ sung có thể đáp ứng được yêu cầu của một trong hai học sinh. Người độc thân còn lại chính xác là một sinh viên không thể hài lòng. Một nửa gói cuối cùng chưa được sử dụng là của Andrew, được phép khi số lượng học sinh là số lẻ.
