---
title: "CF 102440B - \u041f\u0435\u0440\u0435\u0441\u0442\u0430\u043d\u043e\u0432\u043a\u0443 \u043d\u0430 \u043f\u0440\u043e\u043a\u0430\u0447\u043a\u0443"
description: "Chúng ta bắt đầu bằng hoán vị p của các số từ 1 đến n. Chúng ta có thể xóa bất kỳ phần tử nào, nhưng các phần tử còn lại phải giữ nguyên thứ tự ban đầu, vì vậy mọi kết quả có thể xảy ra đều là dãy con của hoán vị."
date: "2026-08-08T13:43:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102440
codeforces_index: "B"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Junior"
rating: 0
weight: 102440
solve_time_s: 138
verified: false
draft: false
---

[CF 102440B - \u041f\u0435\u0440\u0435\u0441\u0442\u0430\u043d\u043e\u0432\u043a\u0443 \u043d\u0430 \u043f\u0440\u043e\u043a\u0430\u0447\u043a\u0443](https://codeforces.com/problemset/problem/102440/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một hoán vị`p`của các số từ`1`ĐẾN`n`. Chúng ta có thể xóa bất kỳ phần tử nào, nhưng các phần tử còn lại phải giữ nguyên thứ tự ban đầu, vì vậy mọi kết quả có thể xảy ra đều là dãy con của hoán vị. 

Đối với một chuỗi có độ dài được chọn`m`, vẻ đẹp của nó là`m`trừ đi số lần đảo ngược của nó. Đảo ngược là một cặp phần tử còn lại trong đó phần tử trước lớn hơn phần tử sau. Vì vậy, mỗi phần tử chúng ta giữ lại mang lại cho chúng ta một điểm đẹp, trong khi mọi sự đảo ngược giữa các phần tử được giữ lại sẽ loại bỏ một điểm. 

Nhiệm vụ là chọn một dãy con có giá trị cuối cùng là`number of kept elements - number of inversions`là càng lớn càng tốt. 

Sự ràng buộc`n <= 2 * 10^5`ngay lập tức loại trừ các thuật toán kiểm tra tất cả các dãy con, bởi vì có`2^n`của họ. Ngay cả một thuật toán bậc hai nói chung cũng quá lớn ở quy mô này. Chúng ta cần thứ gì đó xung quanh`O(n log n)`hoặc tệ nhất là gần với thời gian tuyến tính. Thực tế là đầu vào là một hoán vị cũng hữu ích vì mọi giá trị đều khác biệt, vì vậy "tăng" có nghĩa là tăng nghiêm ngặt mà không có bất kỳ sự mơ hồ nào về các giá trị bằng nhau. 

Có một số trường hợp đặc biệt có thể đánh lừa quá trình triển khai nếu bỏ sót quan sát cơ bản. Vì`n = 1`, dãy con duy nhất có thể có là`[1]`, vậy câu trả lời là`1`. Một phương pháp giả sử tồn tại ít nhất một phép đảo ngược có thể trả về 0 không chính xác. 

Coi như```
4
1 2 3 4
```Toàn bộ hoán vị không có nghịch đảo nên vẻ đẹp của nó là`4`. Vì thế câu trả lời là`4`. Việc triển khai xóa các phần tử một cách không cần thiết bất cứ khi nào nó nhìn thấy một mẫu có thể làm mất các phần tử tối ưu. 

Bây giờ hãy xem xét```
2
2 1
```Giữ cả hai yếu tố cho độ dài`2`và một sự đảo ngược, nên vẻ đẹp là`1`. Chỉ giữ lại một trong hai yếu tố cũng mang lại vẻ đẹp`1`. Như vậy câu trả lời là`1`. Một phương thức chỉ trả về độ dài ban đầu sẽ trả về không chính xác`2`. 

Một trường hợp ít rõ ràng hơn là```
5
3 1 2 5 4
```Toàn bộ chuỗi có hai đảo ngược, vì vậy vẻ đẹp của nó là`5 - 2 = 3`. Dãy số sau tăng dần`[1, 2, 5]`có chiều dài`3`và không có sự đảo ngược, thật đẹp`3`. Điều này chứng tỏ rằng một câu trả lời tối ưu không nhất thiết phải giữ số phần tử tối đa có thể. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là xem xét mọi dãy con của hoán vị. Đối với mỗi dãy con được chọn, chúng tôi đếm các nghịch đảo của nó và tính toán vẻ đẹp của nó, sau đó giữ giá trị lớn nhất. Điều này đúng vì mọi kết quả pháp lý đều được biểu diễn bằng đúng một dãy con. 

Vấn đề là số lượng các chuỗi con. có`2^n`lựa chọn, và nếu chúng tôi kiểm tra lên đến`n`các phần tử để xây dựng một chuỗi con và sau đó lên đến`n^2`cặp để đếm số lần đảo ngược của nó, việc triển khai đơn giản có thể mất`O(2^n n^2)`thời gian. Ngay cả việc liệt kê các dãy con cũng là không thể đối với`n = 2 * 10^5`. 

Một nỗ lực có cấu trúc hơn sẽ là sử dụng quy hoạch động trên các chuỗi con, nhưng thuật ngữ đảo ngược sẽ ghép từng cặp phần tử được chọn. Việc chọn một phần tử có thể ảnh hưởng đến chi phí với nhiều phần tử được chọn sau đó, do đó, phép truy toán chuỗi tiếp theo dài nhất đơn giản không mô tả trực tiếp vẻ đẹp. 

Quan sát chính đã loại bỏ hoàn toàn khó khăn đó. 

Giả sử một dãy con được chọn chứa ít nhất một đảo ngược. Chọn bất kỳ yếu tố nào tham gia vào`k`sự đảo ngược. Nếu chúng ta xóa phần tử đó thì độ dài dãy con sẽ giảm đi chính xác`1`, trong khi số lần đảo ngược giảm chính xác`k`. 

Vẻ đẹp của nó thay đổi theo`(-1) - (-k) = k - 1`. 

Nếu như`k >= 1`, xóa phần tử không bao giờ làm giảm vẻ đẹp. Khi`k > 1`, vẻ đẹp thực sự tăng lên. Khi`k = 1`, vẻ đẹp không thay đổi. 

Chúng ta có thể xóa liên tục các phần tử thuộc về nghịch đảo. Cuối cùng, không còn nghịch đảo nào tồn tại, do đó dãy con thu được ngày càng tăng lên. Trong quá trình này vẻ đẹp không bao giờ giảm đi. 

Điều này có nghĩa là mọi dãy con đều có một dãy con tăng dần có vẻ đẹp ít nhất bằng vẻ đẹp của dãy con ban đầu. Dãy con tăng dần có số nghịch đảo bằng 0, vì vậy vẻ đẹp của nó chỉ đơn giản là độ dài của nó. 

Do đó, vẻ đẹp tốt nhất có thể chính xác là độ dài tối đa của dãy con tăng dần của hoán vị ban đầu. Nói cách khác, bài toán tương đương với việc tìm LIS. 

tiêu chuẩn`O(n log n)`Thuật toán LIS duy trì một mảng`tails`, Ở đâu`tails[k]`là giá trị kết thúc nhỏ nhất có thể có của dãy con có độ dài tăng dần`k + 1`thấy cho đến nay. Đối với mọi giá trị hoán vị, tìm kiếm nhị phân sẽ tìm vị trí đầu tiên có giá trị ít nhất là giá trị hiện tại. Việc thay thế giá trị đó sẽ bảo tồn được tiềm năng tốt nhất có thể có trong tương lai. Nếu giá trị hiện tại lớn hơn mọi đuôi được lưu trữ, chúng tôi sẽ mở rộng LIS. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^n n^2)`|`O(n)`mỗi phần tiếp theo | Quá chậm | 
| Tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng trống`tails`. Độ dài của nó sẽ luôn bằng độ dài của dãy con tăng dài nhất được tìm thấy cho đến nay. 
2. Xử lý hoán vị từ trái sang phải. Đối với giá trị hiện tại`x`, tìm kiếm nhị phân cho vị trí đầu tiên`pos`TRONG`tails`như vậy`tails[pos] >= x`. 
3. Nếu có vị trí như vậy, hãy thay thế`tails[pos]`với`x`. Chúng tôi không thay đổi độ dài của dãy con được biểu diễn, nhưng chúng tôi thu được giá trị kết thúc nhỏ hơn, điều này mang lại nhiều cơ hội hơn để mở rộng chuỗi đó với các phần tử trong tương lai. 
4. Nếu không có vị trí đó,`x`lớn hơn mọi cái đuôi hiện tại. Nối nó vào`tails`, vì dãy con tăng dần hiện có thể được mở rộng thêm một phần tử. 
5. Sau khi tất cả các giá trị đã được xử lý, hãy trả về`len(tails)`. Theo quan sát ở trên, độ dài LIS này chính xác là độ đẹp tối đa có thể đạt được. 

Lý do việc thay thế đuôi là an toàn là vì chúng ta chỉ quan tâm đến giá trị kết thúc tốt nhất có thể có cho mỗi độ dài chuỗi con. Giả sử hai dãy con tăng có cùng độ dài nhưng một dãy kết thúc bằng`4`và cái khác kết thúc bằng`7`. Phần tiếp theo kết thúc bằng`4`ít nhất luôn hữu ích cho việc mở rộng sau này, bởi vì mọi giá trị tương lai có thể theo sau`7`cũng có thể làm theo`4`. Vì vậy việc giữ đuôi nhỏ nhất có thể là trạng thái nén chính xác. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ kết quả nào với vẻ đẹp`B`. Nếu nó chứa một sự đảo ngược, hãy chọn một phần tử tham gia vào ít nhất một sự đảo ngược và xóa nó. Nếu phần tử đó tham gia vào`k`nghịch đảo, độ dài giảm đi một và số lần đảo ngược giảm theo`k`, vì vậy vẻ đẹp thay đổi theo`k - 1 >= 0`. Việc lặp lại quá trình này tạo ra một chuỗi con tăng dần với vẻ đẹp ít nhất`B`. Do đó không có dãy con nào có thể đẹp hơn độ dài của LIS. 

Ngược lại, bản thân LIS đang tăng nên nó không có độ nghịch đảo. Vẻ đẹp của nó chính xác là chiều dài của nó. Do đó, độ dài LIS vừa là vẻ đẹp có thể đạt được vừa là giới hạn trên của mọi vẻ đẹp có thể có, khiến nó trở thành câu trả lời chính xác. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

def solve():
    n = int(input())
    p = list(map(int, input().split()))

    tails = []

    for x in p:
        pos = bisect_left(tails, x)

        if pos == len(tails):
            tails.append(x)
        else:
            tails[pos] = x

    print(len(tails))

if __name__ == "__main__":
    solve()
```Đầu vào được đọc bằng cách sử dụng`sys.stdin.readline`, giúp tránh chi phí không cần thiết khi`n`lớn như`2 * 10^5`. 

các`tails`mảng ban đầu trống. Với mọi giá trị`x`,`bisect_left`tìm vị trí đầu tiên chứa giá trị lớn hơn hoặc bằng`x`. Bởi vì đầu vào ban đầu là một hoán vị nên tất cả các giá trị đều khác biệt, nhưng`bisect_left`vẫn là lựa chọn đúng vì dãy con mong muốn tăng chặt. Nếu có thể có giá trị trùng lặp, hãy sử dụng`bisect_right`sẽ cho phép các giá trị bằng nhau mở rộng chuỗi con một cách không chính xác. 

Khi`pos`bằng`len(tails)`,`x`lớn hơn mọi đuôi hiện tại, do đó LIS được biểu diễn có thể được mở rộng. Ngược lại, thay thế`tails[pos]`với`x`giữ nguyên độ dài chuỗi con trong khi giảm giá trị kết thúc của nó. 

Số nguyên Python không tràn, mặc dù câu trả lời nhiều nhất là`n`Dẫu sao thì. Tìm kiếm nhị phân cũng xử lý khoảng trống`tails`mảng chính xác, vì vậy`n = 1`không cần trường hợp đặc biệt. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
5
1 2 3 5 4
```Thuật toán xử lý từng giá trị và duy trì đuôi nhỏ nhất có thể cho mỗi độ dài dãy con tăng dần. 

| Giá trị hiện tại |`tails`sau khi xử lý | chiều dài LIS | 
| --- | --- | --- | 
|`1`|`[1]`|`1`| 
|`2`|`[1, 2]`|`2`| 
|`3`|`[1, 2, 3]`|`3`| 
|`5`|`[1, 2, 3, 5]`|`4`| 
|`4`|`[1, 2, 3, 4]`|`4`| 

Khi`4`đến, nó không thể kéo dài độ dài bốn dãy con vì`4 < 5`. Thay vào đó, nó thay thế`5`là đuôi của dãy con có độ dài bốn tăng dần. Dãy số thực tế là`[1, 2, 3, 4]`, không có nghịch đảo và do đó có vẻ đẹp`4`. 

Câu trả lời là`4`. 

### Mẫu 2 

Đầu vào là```
6
2 1 3 4 5 6
```Nhà nước phát triển như sau. 

| Giá trị hiện tại |`tails`sau khi xử lý | chiều dài LIS | 
| --- | --- | --- | 
|`2`|`[2]`|`1`| 
|`1`|`[1]`|`1`| 
|`3`|`[1, 3]`|`2`| 
|`4`|`[1, 3, 4]`|`3`| 
|`5`|`[1, 3, 4, 5]`|`4`| 
|`6`|`[1, 3, 4, 5, 6]`|`5`| 

Hai phần tử đầu tiên không thể cùng thuộc một dãy con tăng nghiêm ngặt. Khi`1`đến, nó thay thế`2`là đuôi nhỏ nhất có thể có của dãy con có độ dài bằng một. Các giá trị sau này có thể mở rộng trạng thái đó đến độ dài năm. 

LIS kết quả là`[1, 3, 4, 5, 6]`. Vẻ đẹp của nó là`5`, phù hợp với đầu ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Mỗi trong số`n`các giá trị thực hiện một tìm kiếm nhị phân trong`tails`. | 
| Không gian |`O(n)`|`tails`chứa nhiều nhất một giá trị cho mỗi độ dài LIS. | 

Với`n <= 2 * 10^5`,`O(n log n)`chỉ yêu cầu vài triệu thao tác tìm kiếm nhị phân, phù hợp với ràng buộc. Việc sử dụng bộ nhớ là tuyến tính và nằm trong giới hạn dự kiến. 

## Trường hợp thử nghiệm```python
import sys
import io
from bisect import bisect_left

def solve():
    input = sys.stdin.readline
    n = int(input())
    p = list(map(int, input().split()))

    tails = []

    for x in p:
        pos = bisect_left(tails, x)
        if pos == len(tails):
            tails.append(x)
        else:
            tails[pos] = x

    print(len(tails))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("""5
1 2 3 5 4
""") == "4", "sample 1"

assert run("""6
2 1 3 4 5 6
""") == "5", "sample 2"

# Minimum-size valid permutation
assert run("""1
1
""") == "1", "single element"

# Completely decreasing permutation
assert run("""5
5 4 3 2 1
""") == "1", "strictly decreasing"

# Completely increasing permutation
assert run("""5
1 2 3 4 5
""") == "5", "strictly increasing"

# Mixed case with several replacements in tails
assert run("""7
4 1 6 2 5 3 7
""") == "4", "multiple LIS tail replacements"

# Generalized robustness case with equal values.
# Equal values are not valid under the permutation guarantee,
# but bisect_left correctly computes a strictly increasing subsequence.
assert run("""4
2 2 2 2
""") == "1", "duplicate values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1`| Kích thước tối thiểu và ranh giới tìm kiếm nhị phân trống | 
|`5 / 5 4 3 2 1`|`1`| Không có cặp tăng nào tồn tại | 
|`5 / 1 2 3 4 5`|`5`| Toàn bộ hoán vị đã tăng lên | 
|`7 / 4 1 6 2 5 3 7`|`4`| Thay thế đuôi mà không thay đổi độ dài LIS | 
|`4 / 2 2 2 2`|`1`| Tính chặt chẽ của LIS và`bisect_left`; ngoài hợp đồng hoán vị chính thức | 

Trường hợp hoàn toàn bằng nhau được cố tình đánh dấu là một bài kiểm tra độ bền chứ không phải là một trường hợp vấn đề hợp lệ. Câu lệnh đảm bảo một hoán vị, do đó các giá trị bằng nhau không thể xuất hiện trong đầu vào chính thức. Việc bao gồm trường hợp này vẫn hữu ích để kiểm tra xem quy ước tìm kiếm nhị phân của việc triển khai có khớp với định nghĩa về dãy con tăng nghiêm ngặt hay không. 

## Vỏ cạnh 

Đối với đầu vào một phần tử```
1
1
```

`tails`bắt đầu trống rỗng. giá trị`1`được chèn vào vị trí 0, tạo ra`[1]`, và câu trả lời là`1`. Không có sự đảo ngược để trừ đi, vì vậy vẻ đẹp duy nhất có thể thực sự là`1`. 

Đối với hoán vị giảm dần```
2
2 1
```giá trị đầu tiên tạo ra`tails = [2]`. giá trị`1`thay thế`2`, cho`tails = [1]`. Độ dài LIS là`1`. Chuỗi hai yếu tố ban đầu có một sự đảo ngược và vẻ đẹp`2 - 1 = 1`, trong khi xóa một trong hai phần tử cũng mang lại vẻ đẹp`1`. Điều này khẳng định thuật toán không cho rằng việc giữ nhiều yếu tố hơn luôn cải thiện vẻ đẹp. 

Đối với một hoán vị đã tăng dần```
4
1 2 3 4
```mọi giá trị đều lớn hơn đuôi cuối cùng hiện tại, vì vậy`tails`phát triển đến`[1, 2, 3, 4]`. Câu trả lời là`4`. Vì không có sự đảo ngược nên toàn bộ chuỗi đã thể hiện được vẻ đẹp đó. 

Đối với một trình tự có thể loại bỏ sự đảo ngược mà không làm mất vẻ đẹp, hãy xem xét```
5
3 1 2 5 4
```Toàn bộ chuỗi có hai đảo ngược,`(3,1)`,`(3,2)`, và cả`(5,4)`, vậy ra vẻ đẹp của nó thực sự là`5 - 3 = 2`. Thuật toán LIS tạo ra`tails`tiểu bang`[3]`,`[1]`,`[1,2]`,`[1,2,5]`, và cuối cùng`[1,2,4]`, cho một LIS có độ dài`3`. Trình tự tiếp theo`[1,2,4]`không có sự đảo ngược và vẻ đẹp`3`, điều này thực sự tốt hơn là giữ lại mọi thứ. Trường hợp này chứng minh tại sao mục tiêu không thể giảm xuống mức tối đa hóa độ dài chuỗi con trước khi tính đến các nghịch đảo. 

Đối với giá trị biên`n = 2 * 10^5`, thuật toán vẫn thực hiện chính xác một tìm kiếm nhị phân cho mỗi giá trị đầu vào. Kích thước của`tails`nhiều nhất là`2 * 10^5`, vì vậy mọi thao tác vẫn nằm trong`O(log n)`ràng buộc. Không sử dụng đệ quy, tránh những lo ngại về độ sâu đệ quy và biểu diễn số nguyên của Python là quá đủ cho câu trả lời.
