---
title: "CF 102163K - Masaoud YÊU PIZZA"
description: "Chúng ta có một dãy số lượng bánh pizza, trong đó A[i] là số lát trên đĩa của học sinh thứ i. Masaoud phải chọn một phân đoạn liền kề không trống của mảng này, nghĩa là một nhóm sinh viên liên tiếp và tổng các giá trị được chọn phải nhỏ hơn X."
date: "2026-08-19T14:57:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102163
codeforces_index: "K"
codeforces_contest_name: "NCD 2019"
rating: 0
weight: 102163
solve_time_s: 568
verified: false
draft: false
---

[CF 102163K - Masaoud YÊU PIZZAS](https://codeforces.com/problemset/problem/102163/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một loạt số lượng bánh pizza, trong đó`A[i]`là số lát cắt trên`i`-đĩa của học sinh thứ Masaoud phải chọn một phân đoạn liền kề không trống của mảng này, nghĩa là một nhóm sinh viên liên tiếp và tổng các giá trị được chọn phải nhỏ hơn rất nhiều so với`X`. 

Nhiệm vụ là đếm mọi mảng con liền kề có tổng nhỏ hơn`X`. Các vị trí khác nhau xác định các nhóm khác nhau, do đó, ngay cả các giá trị bằng nhau ở các vị trí khác nhau cũng thể hiện các lựa chọn khác nhau. 

Các ràng buộc đủ lớn để loại trừ việc kiểm tra trực tiếp mọi mảng con. Với`N = 10^5`, có`N(N+1)/2`, hoặc về`5 * 10^9`, các nhóm liền kề có thể xảy ra trong trường hợp xấu nhất. Ngay cả thuật toán O(N²) cũng vượt xa giới hạn thời gian 1 giây có thể xử lý. Các giá trị dương trong mảng là thuộc tính cấu trúc quan trọng cho phép giải quyết vấn đề theo thời gian tuyến tính. 

Có một số trường hợp ranh giới có thể bộc lộ việc triển khai bất cẩn. Đầu tiên, điều kiện là nghiêm ngặt`< X`, không`<= X`. Ví dụ, với`N = 1`,`X = 4`, Và`A = [4]`, đầu ra đúng là`0`, bởi vì mảng con duy nhất có tổng chính xác`4`. Một triển khai sử dụng`sum <= X`sẽ đếm sai. 

Trường hợp thứ hai là khi mọi phần tử đều đã quá lớn. Vì`N = 3`,`X = 5`, Và`A = [5, 6, 7]`, câu trả lời là`0`. Việc triển khai cửa sổ trượt phải loại bỏ các phần tử cho đến khi tổng hiện tại ở dưới`X`trước khi đếm bất cứ điều gì. 

Trường hợp thứ ba là khi toàn bộ mảng hợp lệ. Vì`N = 3`,`X = 10`, Và`A = [1, 2, 3]`, mọi mảng con không trống đều có tổng bên dưới`10`, vậy câu trả lời là`6`. Số đếm phải bao gồm các mảng con có độ dài bất kỳ có thể, không chỉ các phần tử đơn lẻ. 

Cuối cùng, bản thân câu trả lời có thể lớn hơn nhiều so với`N`. Với`N = 10^5`và đủ lớn`X`, mọi mảng con đều hợp lệ, cho`10^5 * 100001 / 2 = 5,000,050,000`các nhóm hợp lệ. Số nguyên Python xử lý việc này một cách tự nhiên, trong khi ngôn ngữ sử dụng số nguyên 32 bit sẽ tràn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi vị trí bắt đầu có thể và mọi vị trí kết thúc có thể có, tính tổng của mảng con đó và tăng câu trả lời bất cứ khi nào tổng nhỏ hơn`X`. có`N(N+1)/2`mảng con. Nếu mỗi tổng được tính bằng cách mở rộng điểm cuối bên phải và thêm một phần tử thì tổng công là O(N2), khoảng`5 * 10^9`lặp đi lặp lại khi`N = 10^5`. Tổng tiền tố có thể tạo ra tổng mỗi mảng con riêng lẻ là O(1), nhưng vẫn có các cặp điểm cuối O(N²), do đó độ phức tạp tổng thể vẫn là bậc hai. 

Phương pháp brute-force hoạt động vì mỗi nhóm liền kề được xem xét chính xác một lần. Vấn đề là nó tốn thời gian xem xét các nhóm mà giá trị của chúng có thể được suy ra từ các nhóm đã được kiểm tra. 

Quan sát quan trọng xuất phát từ thực tế là mọi`A[i]`là tích cực. Giả sử chúng ta sửa một điểm cuối phù hợp`r`và xem xét các mảng con kết thúc tại`r`. Khi chúng ta di chuyển điểm cuối bên trái của chúng xa hơn về bên trái, tổng của chúng chỉ có thể tăng lên. Khi một điểm cuối bên trái cụ thể tạo ra một tổng ít nhất là`X`, mọi điểm cuối bên trái trước đó cũng sẽ tạo ra một tổng ít nhất`X`. 

Hành vi đơn điệu này cho phép một cửa sổ trượt hai con trỏ. Duy trì một cửa sổ`[left, right]`tổng của nó hoàn toàn nhỏ hơn`X`. Khi`right`tiến lên, phần tử mới làm tăng tổng. Nếu tổng đạt hoặc vượt quá`X`, di chuyển`left`chuyển tiếp và trừ các giá trị đã xóa cho đến khi cửa sổ trở lại hợp lệ. 

Một lần`[left, right]`là hợp lệ, mọi mảng con kết thúc tại`right`và bắt đầu từ bất cứ đâu`left`bởi vì`right`cũng hợp lệ. Có chính xác`right - left + 1`mảng con như vậy. Việc thêm số đó sẽ đếm tất cả các nhóm hợp lệ kết thúc ở vị trí hiện tại mà không liệt kê chúng một cách rõ ràng. 

Tính tích cực của mảng là điều làm cho sự chuyển động`left`an toàn. Việc loại bỏ các phần tử chỉ có thể làm giảm tổng và việc mở rộng điểm cuối bên phải chỉ có thể làm tăng tổng. Nếu các giá trị âm được cho phép, mối quan hệ đơn điệu này sẽ biến mất và đối số cửa sổ trượt tương tự sẽ không còn hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Quá chậm | 
| Cửa sổ trượt tối ưu | O(N) | O(1) không gian phụ trợ | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đặt`left = 0`,`current_sum = 0`, Và`answer = 0`. Con trỏ`left`sẽ đại diện cho vị trí bắt đầu nhỏ nhất mà vẫn có thể tạo ra một mảng con hợp lệ kết thúc ở điểm cuối bên phải hiện tại. 
2. Di chuyển`right`từ`0`bởi vì`N - 1`. Thêm vào`A[right]`ĐẾN`current_sum`, vì cửa sổ hiện tại vừa được mở rộng để bao gồm học sinh mới. 
3. Trong khi`current_sum >= X`, di dời`A[left]`từ`current_sum`và tăng dần`left`. Cửa sổ phải nhỏ hơn`X`, do đó bình đẳng với`X`cũng không hợp lệ. Vì mọi giá trị mảng đều dương nên di chuyển`left`chuyển tiếp chỉ có thể giảm tổng, do đó cuối cùng cửa sổ trở nên hợp lệ trừ khi ngay cả phần tử đơn lẻ`A[right]`ít nhất là`X`. 
4. Sau vòng lặp,`[left, right]`có tổng nhỏ hơn`X`. Mỗi vị trí xuất phát từ`left`bởi vì`right`đưa ra một mảng con hợp lệ khác kết thúc tại`right`. Như vậy thêm`right - left + 1`ĐẾN`answer`. 
5. Lặp lại cho đến khi mọi điểm cuối bên phải có thể được xử lý. In`answer`cho trường hợp thử nghiệm. 

### Tại sao nó hoạt động 

Điều bất biến là sau giai đoạn thu nhỏ,`current_sum`là tổng của`[left, right]`và hoàn toàn nhỏ hơn`X`, trong khi mọi mảng con kết thúc tại`right`vị trí bắt đầu của nó là trước`left`có tổng ít nhất`X`. 

Khi`right`được sửa, việc loại bỏ các phần tử ở bên trái sẽ làm cho tổng nhỏ hơn vì tất cả các giá trị đều dương. Do đó, giá trị đầu tiên hợp lệ`left`chia tất cả các vị trí bắt đầu có thể thành hai nhóm: các vị trí từ`0`bởi vì`left - 1`không hợp lệ, trong khi các vị trí từ`left`bởi vì`right`là hợp lệ. Có chính xác`right - left + 1`lựa chọn hợp lệ, vì vậy số tiền được thêm vào câu trả lời là chính xác. 

Mỗi con trỏ chỉ di chuyển về phía trước. Con trỏ bên phải di chuyển`N`lần, và mặc dù vòng lặp bên trong có thể trông giống như thực hiện nhiều thao tác,`left`cũng di chuyển nhiều nhất`N`lần trong toàn bộ trường hợp thử nghiệm. Điều đó mang lại tổng công tuyến tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))

        left = 0
        current_sum = 0
        answer = 0

        for right in range(n):
            current_sum += a[right]

            while current_sum >= x and left <= right:
                current_sum -= a[left]
                left += 1

            answer += right - left + 1

        print(answer)

if __name__ == "__main__":
    solve()
```Phần đầu vào đọc số lượng ca kiểm thử và sau đó là mảng cho từng trường hợp. Việc lưu trữ mảng rất thuận tiện vì con trỏ bên trái có thể cần trừ các giá trị đã được thêm vào trước đó. 

Vòng lặp chính mở rộng cửa sổ bằng cách thêm`a[right]`. các`while`điều kiện sử dụng`>= x`, còn hơn là`> x`, vì điều kiện yêu cầu nhỏ hơn`X`. 

Nếu bản thân phần tử mới được thêm vào ít nhất là`X`, vòng lặp thu hẹp cuối cùng cũng loại bỏ phần tử đó. Sau đó`left`trở thành`right + 1`,`current_sum`trở thành số không, và`right - left + 1`là số không. Điều này tính chính xác không có mảng con hợp lệ nào kết thúc ở vị trí đó. 

Câu trả lời chỉ được cập nhật sau khi cửa sổ hợp lệ. biểu hiện`right - left + 1`đếm tất cả các vị trí bắt đầu có thể có trong phạm vi hợp lệ`[left, right]`. 

Số nguyên Python có độ chính xác tùy ý, vì vậy câu trả lời có thể vượt quá phạm vi số nguyên 32 bit một cách an toàn. Trên thực tế, với`N = 10^5`, đáp án tối đa là`5,000,050,000`. 

Thứ tự thực hiện cũng quan trọng. Trước tiên, chúng tôi thêm điểm cuối bên phải mới, sau đó thu nhỏ cho đến khi thỏa mãn bất đẳng thức nghiêm ngặt và chỉ sau đó mới tính số lần bắt đầu hợp lệ. Việc đếm trước khi thu nhỏ sẽ bao gồm các mảng con không hợp lệ. 

## Ví dụ đã hoạt động 

### Mẫu 1, test case 1 

Đầu vào là`N = 1`,`X = 4`, Và`A = [3]`. Nhóm duy nhất có thể chứa một học sinh duy nhất và tổng của nó là`3`, hợp lệ. 

| đúng | giá trị gia tăng | current_sum trước khi thu nhỏ | còn lại sau khi thu nhỏ | nhóm hợp lệ được thêm vào | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 3 | 0 | 1 | 1 | 

Cửa sổ`[0, 0]`là hợp lệ, do đó có chính xác một vị trí bắt đầu hợp lệ. Câu trả lời là`1`. 

### Mẫu 1, test case 2 

đây`N = 2`,`X = 4`, Và`A = [1, 5]`. Các nhóm có thể là`[1]`,`[5]`, Và`[1, 5]`. Chỉ một`[1]`có tổng dưới đây`4`. 

| đúng | giá trị gia tăng | current_sum trước khi thu nhỏ | còn lại sau khi thu nhỏ | nhóm hợp lệ được thêm vào | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 1 | 1 | 
| 1 | 5 | 6 | 2 | 0 | 1 | 

Khi`5`được thêm vào, tổng sẽ trở thành`6`. Đang xóa`1`lá`5`, vẫn còn quá lớn, vì vậy`5`chính nó được loại bỏ. Cửa sổ trở nên trống rỗng, được biểu thị bằng`left = 2`. Không có mảng con hợp lệ nào kết thúc ở chỉ mục`1`, vậy đáp án cuối cùng vẫn là`1`. 

### Dấu vết bổ sung, tất cả các mảng con đều hợp lệ 

Hãy xem xét`N = 3`,`X = 10`, Và`A = [1, 2, 3]`. 

| đúng | giá trị gia tăng | current_sum trước khi thu nhỏ | còn lại sau khi thu nhỏ | nhóm hợp lệ được thêm vào | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 1 | 1 | 
| 1 | 2 | 3 | 0 | 2 | 3 | 
| 2 | 3 | 6 | 0 | 3 | 6 | 

Tại mỗi vị trí toàn bộ tiền tố kết thúc tại`right`vẫn còn hiệu lực. Thuật toán bổ sung`1 + 2 + 3 = 6`, bằng tổng số mảng con không trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) cho mỗi trường hợp thử nghiệm |`right`di chuyển từ trái sang phải một lần và`left`cũng tiến về phía trước nhiều nhất`N`lần | 
| Không gian | O(N) | Mảng được lưu trữ; bản thân trạng thái cửa sổ trượt sử dụng không gian phụ O(1) | 

Trên tất cả các trường hợp thử nghiệm, thời gian là O(tổng của`N`) và không gian mảng được lưu trữ là O(N) cho trường hợp thử nghiệm hiện tại. Với`N`lên đến`10^5`, thuật toán chỉ thực hiện một số thao tác không đổi cho mỗi phần tử và vừa vặn trong giới hạn. Cách tiếp cận bậc hai ban đầu sẽ yêu cầu hàng tỷ phép tính trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))

        left = 0
        current_sum = 0
        answer = 0

        for right in range(n):
            current_sum += a[right]

            while current_sum >= x and left <= right:
                current_sum -= a[left]
                left += 1

            answer += right - left + 1

        out.append(str(answer))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

assert run("""\
2
1 4
3
2 4
1 5
""") == """\
1
1
""", "provided sample"

assert run("""\
1
1 4
4
""") == """\
0
""", "exactly X must not be counted"

assert run("""\
1
3 10
1 2 3
""") == """\
6
""", "every subarray is valid"

assert run("""\
1
3 5
5 6 7
""") == """\
0
""", "every individual element is invalid"

assert run("""\
1
4 6
1 1 1 1
""") == """\
10
""", "all equal values, every subarray is valid"

assert run("""\
1
3 3
1 2 1
""") == """\
3
""", "strict boundary and shrinking"

assert run("""\
1
100000 1000000000000
""" + "1 " * 99999 + "1\n") == """\
5000050000
""", "maximum N and maximum answer")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 4 / 4`|`0`| Nghiêm ngặt`< X`ranh giới | 
|`3 / 10 / 1 2 3`|`6`| Mọi mảng con có thể đều hợp lệ | 
|`3 / 5 / 5 6 7`|`0`| Các phần tử riêng lẻ bằng hoặc cao hơn`X`| 
|`4 / 6 / 1 1 1 1`|`10`| Giá trị bằng nhau và đếm tất cả độ dài | 
|`3 / 3 / 1 2 1`|`3`| Chỉnh sửa thu hẹp khi tổng đạt`X`| 
|`100000 / 10^12 / 1 ... 1`|`5,000,050,000`| Tối đa`N`và trả lời phạm vi lớn hơn 32 bit | 

Cấu trúc thử nghiệm kích thước tối đa`100000`những cái đó và chọn một`X`lớn hơn tổng số tiền của họ. Do đó mỗi một trong số`5,000,050,000`mảng con không trống là hợp lệ. Điều này kiểm tra cả việc truyền tải tuyến tính và khả năng biểu diễn một câu trả lời lớn. 

## Vỏ cạnh 

Đối với trường hợp bất đẳng thức nghiêm ngặt, hãy xem xét:```
1
1 4
4
```Thuật toán bổ sung`4`, thấy thế`current_sum >= X`và loại bỏ phần tử duy nhất. Cửa sổ kết quả trống, vì vậy`right - left + 1 = 0`. Đầu ra là`0`, đúng như yêu cầu. Một triển khai sử dụng`while current_sum > X`sẽ đếm sai mảng con này. 

Đối với trường hợp mọi phần tử đều không hợp lệ, hãy xem xét:```
1
3 5
5 6 7
```Tại`right = 0`, tổng là`5`, do đó phần tử bị loại bỏ và phần đóng góp bằng không. Tại`right = 1`, điều tương tự cũng xảy ra với`6`, và tại`right = 2`nó xảy ra với`7`. Câu trả lời cuối cùng là`0`. Cửa sổ trống là trạng thái nội bộ hợp lệ vì sự cố yêu cầu các nhóm không trống và đóng góp bằng 0 sẽ loại trừ nó một cách chính xác. 

Đối với trường hợp mọi mảng con đều hợp lệ, hãy xem xét:```
1
3 10
1 2 3
```Cửa sổ không bao giờ cần phải thu nhỏ lại. Tại ba điểm cuối bên phải, thuật toán đóng góp`1`,`2`, Và`3`, sản xuất`6`. Những đóng góp đó tương ứng với tất cả các mảng con kết thúc ở mỗi vị trí tương ứng. 

Đối với trường hợp câu trả lời lớn, hãy xem xét một mảng`100000`những cái có`X = 10^12`. Vì thậm chí toàn bộ mảng chỉ có tổng`100000`, không xảy ra hiện tượng co rút. Tại chỉ mục`r`, chính xác`r + 1`mảng con kết thúc ở đó là hợp lệ, vì vậy tổng số là`1 + 2 + ... + 100000 = 5,000,050,000`. 

Python lưu trữ giá trị này mà không bị tràn và thuật toán vẫn chỉ thực hiện công việc O(N).
