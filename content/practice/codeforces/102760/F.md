---
title: "CF 102760F - Hình vuông, không phải hình chữ nhật"
description: "Chúng ta được cung cấp một biểu đồ được làm bằng các ô xếp dọc liền kề. Ô thứ i có chiều rộng 1 và chiều cao Hi, do đó, một nhóm các ô liên tiếp tạo thành một hình chữ nhật có chiều rộng là số ô được chọn và có chiều cao bị giới hạn bởi ô ngắn nhất trong nhóm đó."
date: "2026-07-28T23:51:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102760
codeforces_index: "F"
codeforces_contest_name: "2020 KAIST 10th ICPC Mock Contest (XXI Open Cup. Grand Prix of Korea. Division 2)"
rating: 0
weight: 102760
solve_time_s: 58
verified: true
draft: false
---

[CF 102760F - Hình vuông, không phải hình chữ nhật](https://codeforces.com/problemset/problem/102760/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ được làm bằng các ô xếp dọc liền kề. các`i`- ô thứ có chiều rộng`1`và chiều cao`H_i`, do đó, một nhóm các ô liên tiếp tạo thành một hình chữ nhật có chiều rộng là số ô được chọn và chiều cao của nó bị giới hạn bởi ô ngắn nhất trong nhóm đó. Nhiệm vụ là tìm hình vuông lớn nhất có thể nằm hoàn toàn bên trong biểu đồ và xuất ra độ dài cạnh của nó. Hình vuông phải có các cạnh thẳng hàng với trục biểu đồ. 

Đầu vào chứa một biểu đồ với`N`gạch theo sau là chiều cao của chúng. Kết quả đầu ra không phải là diện tích hình vuông mà là độ dài cạnh tối đa có thể đạt được. 

Số lượng gạch có thể đạt tới`300000`, do đó thuật toán kiểm tra nhiều khoảng không thể hoạt động được. Việc tìm kiếm trực tiếp trên tất cả các cặp ranh giới trái và phải sẽ yêu cầu khoảng`N^2`kiểm tra theo khoảng thời gian, vượt xa giới hạn thời gian lập trình cạnh tranh thông thường cho phép. Chiều cao cũng có thể lớn như`10^9`, do đó, câu trả lời và so sánh trung gian phải được xử lý bằng số học số nguyên mà không dựa vào các giả định giá trị nhỏ. 

Những trường hợp phức tạp xuất phát từ việc nhầm lẫn hình chữ nhật lớn nhất với hình vuông lớn nhất. Ví dụ: nếu biểu đồ là:```
3
100 1 100
```hình chữ nhật lớn nhất có diện tích`200`, nhưng hình vuông lớn nhất có độ dài cạnh`1`, bởi vì vùng rộng duy nhất là không đủ cao. 

Một lỗi phổ biến khác là quên rằng hình vuông chỉ có thể sử dụng một phần của hình chữ nhật lớn. Vì:```
5
10 10 10 10 10
```câu trả lời là`5`, không`10`, vì biểu đồ chỉ rộng năm ô. Yếu tố giới hạn là chiều rộng và chiều cao nhỏ hơn. 

Trường hợp ranh giới cuối cùng là một ô đơn:```
1
7
```Câu trả lời là`1`, không`7`, bởi vì một ô có chiều rộng chính xác là một. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử mọi dãy ô liên tiếp có thể. Đối với mỗi phạm vi, chúng tôi tìm thấy chiều cao tối thiểu bên trong nó. Chiều cao tối thiểu đó là chiều cao hình vuông tối đa có thể có cho phạm vi này, trong khi chiều dài phạm vi là chiều rộng hình vuông tối đa có thể. Câu trả lời ứng cử viên là giá trị nhỏ hơn trong hai giá trị đó. 

Cách tiếp cận này đúng vì mọi ô vuông có thể đều tương ứng với một phạm vi ô liên tiếp nào đó. Tuy nhiên, có`N(N+1)/2`phạm vi, đó là về`4.5 * 10^10`khi`N = 300000`. Ngay cả với các truy vấn tối thiểu nhanh, việc kiểm tra mọi phạm vi là không thể. 

Quan sát quan trọng là ô ngắn nhất trong một phạm vi sẽ xác định chiều cao tối đa của mỗi ô vuông sử dụng phạm vi đó. Thay vì xem xét mọi phạm vi, chúng ta có thể coi mỗi ô là ô có chiều cao tối thiểu trong phạm vi tốt nhất có thể xung quanh nó. 

Đối với gạch có chiều cao`h`, chúng tôi tìm thấy đoạn liên tiếp rộng nhất trong đó mỗi ô có chiều cao ít nhất`h`. Nếu đoạn đó có chiều rộng`w`, thì hình vuông lớn nhất mà ô này có chiều cao giới hạn có chiều dài cạnh`min(h, w)`. 

Thách thức còn lại là tìm ra những phân khúc rộng nhất này một cách hiệu quả. Ngăn xếp tăng đơn điệu cung cấp chính xác thông tin này. Nó cho phép chúng ta tìm, với mỗi ô, ô nhỏ hơn đầu tiên ở bên trái và ô nhỏ hơn đầu tiên ở bên phải. Hai vị trí đó xác định phạm vi tối đa trong đó chiều cao hiện tại có thể ở mức tối thiểu. 

Phương pháp brute-force có tác dụng vì mọi ô vuông có thể đều thuộc về một khoảng nào đó. Nó thất bại vì có quá nhiều khoảng thời gian. Ngăn xếp đơn điệu loại bỏ nhu cầu liệt kê các khoảng bằng cách trực tiếp tìm khoảng tối đa cho mỗi chiều cao giới hạn có thể có. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Di chuyển biểu đồ từ trái sang phải trong khi vẫn duy trì chồng chỉ số ô tăng dần đơn điệu. Ngăn xếp lưu trữ các ô có chiều cao chưa gặp ô nhỏ hơn ở bên phải của chúng. Giữ chiều cao tăng dần có nghĩa là khi một ô ngắn hơn xuất hiện, chúng ta biết ngay rằng một số ô trước đó đã đạt chiều rộng tối đa. 
2. Khi chiều cao hiện tại nhỏ hơn chiều cao của ô ở đầu ngăn xếp, hãy xóa từng chỉ mục khỏi ngăn xếp. Đối với mỗi ô bị loại bỏ, vị trí hiện tại là ô nhỏ hơn đầu tiên ở bên phải và đỉnh ngăn xếp mới sau khi xóa là ô nhỏ hơn đầu tiên ở bên trái. 
3. Tính chiều rộng của đoạn tối đa cho ô bị loại bỏ. Nếu không có ô nhỏ hơn ở bên trái thì phân đoạn sẽ bắt đầu ở đầu biểu đồ. Nếu không, nó bắt đầu sau ô nhỏ hơn bên trái. Chiều rộng là chỉ số hiện tại trừ đi ranh giới bên trái. 
4. Sử dụng chiều cao và chiều rộng được tính toán của ô đã loại bỏ để tạo thành một ứng cử viên hình vuông. Độ dài cạnh không thể vượt quá một trong hai chiều, vì vậy hãy cập nhật câu trả lời bằng`min(height, width)`. 
5. Đẩy chỉ mục ô hiện tại vào ngăn xếp sau khi tất cả các ô cao hơn đã được xử lý. Điều này giữ cho thuộc tính ngăn xếp hợp lệ cho các ô trong tương lai. 
6. Sau khi quét tất cả các ô, giả sử có thêm một ô có chiều cao`0`. Điều này buộc mọi phần tử ngăn xếp còn lại phải bị loại bỏ và xử lý, bao gồm các phạm vi kéo dài đến cuối biểu đồ. 

Tại sao nó hoạt động: 

Mỗi ô vuông hợp lệ chiếm một số nhóm ô liên tiếp. Đặt chiều cao ô ngắn nhất trong nhóm đó là`h`. Ít nhất một ô trong nhóm có chiều cao chính xác`h`. Khi thuật toán xử lý ô đó, ngăn xếp đơn điệu sẽ tạo ra khoảng lớn nhất có thể trong đó mỗi ô có chiều cao ít nhất`h`. Bất kỳ hình vuông nào sử dụng chiều cao`h`không thể rộng hơn khoảng này và thuật toán sẽ kiểm tra chính xác độ dài cạnh tốt nhất có thể cho nó. Vì mỗi ô vuông có thể có một ô có chiều cao tối thiểu tương ứng được xem xét nên ứng cử viên tối đa được thuật toán tìm thấy là câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    h = list(map(int, input().split()))

    stack = []
    ans = 0

    for i in range(n + 1):
        cur = h[i] if i < n else 0

        while stack and h[stack[-1]] > cur:
            idx = stack.pop()
            height = h[idx]

            if stack:
                width = i - stack[-1] - 1
            else:
                width = i

            ans = max(ans, min(height, width))

        stack.append(i)

    print(ans)

if __name__ == "__main__":
    solve()
```Ngăn xếp lưu trữ các chỉ số thay vì chiều cao vì vị trí của mỗi ô là cần thiết để tính chiều rộng của khoảng hợp lệ tối đa của nó. Ngăn xếp đang tăng dần theo chiều cao, do đó, mỗi khi có một ô ngắn hơn, các ô bị loại bỏ sẽ tìm thấy phần tử nhỏ hơn đầu tiên ở bên phải. 

Việc lặp lại bổ sung với chiều cao`0`là lính canh. Nó không đại diện cho một ô thật nhưng nó đảm bảo rằng tất cả các chiều cao còn lại đều được hiển thị và đánh giá. Nếu không có nó, các ô kéo dài đến cuối biểu đồ sẽ không bao giờ được xem xét. 

Việc tính toán chiều rộng là nơi chính xảy ra lỗi sai sót. Nếu ngăn xếp trống sau khi loại bỏ một ô, thì không có ô nào nhỏ hơn ở bên trái của nó, do đó khoảng bắt đầu từ chỉ mục`0`và có chiều rộng`i`. Ngược lại, khoảng thời gian bắt đầu sau`stack[-1]`, cho chiều rộng`i - stack[-1] - 1`. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
6
3 4 4 4 4 3
```việc thực hiện là: 

| Bước | Chỉ số hiện tại | Chiều cao hiện tại | Xếp chồng sau khi xử lý | Đã xóa chiều cao | Ứng viên | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | [0] | không | không | 
| 2 | 1 | 4 | [0, 1] | không | không | 
| 3 | 2 | 4 | [0, 1, 2] | không | không | 
| 4 | 3 | 4 | [0, 1, 2, 3] | không | không | 
| 5 | 4 | 4 | [0, 1, 2, 3, 4] | không | không | 
| 6 | 5 | 3 | [0, 5] | 4 | 4 | 
| 7 | 6 | 0 | [ ] | 3 | 3 | 

Bốn gạch có chiều cao`4`tạo thành một hình vuông có độ dài cạnh`4`, trở thành câu trả lời. 

Một ví dụ khác:```
5
2 5 5 5 2
```| Bước | Chỉ số hiện tại | Chiều cao hiện tại | Xếp chồng sau khi xử lý | Đã xóa chiều cao | Ứng viên | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 2 | [0] | không | không | 
| 2 | 1 | 5 | [0, 1] | không | không | 
| 3 | 2 | 5 | [0, 1, 2] | không | không | 
| 4 | 3 | 5 | [0, 1, 2, 3] | không | không | 
| 5 | 4 | 2 | [0, 4] | 5 | 3 | 
| 6 | 5 | 0 | [ ] | 2 | 2 | 

Chiều cao`5`vùng có chiều rộng`3`, vậy hình vuông tốt nhất có độ dài cạnh`3`. Chiều cao xung quanh`2`gạch rộng hơn nhưng không thể tạo ra hình vuông lớn hơn vì chiều cao của chúng là kích thước giới hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi chỉ mục ô xếp vào ngăn xếp một lần và rời khỏi ngăn xếp một lần. | 
| Không gian | O(N) | Ngăn xếp có thể chứa mọi chỉ mục ô xếp trong biểu đồ tăng dần. | 

Thuật toán thực hiện một lượng công việc không đổi cho mỗi thao tác ngăn xếp và tổng số lần đẩy và bật là tuyến tính. Với`N = 300000`, điều này dễ dàng phù hợp với giới hạn dự định. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""6
3 4 4 4 4 3
""") == "4\n", "sample 1"

assert run("""1
7
""") == "1\n", "single tile"

assert run("""5
10 10 10 10 10
""") == "5\n", "width limited"

assert run("""3
100 1 100
""") == "1\n", "short middle tile"

assert run("""5
2 5 5 5 2
""") == "3\n", "largest square inside rectangle"

assert run("""6
1 2 3 4 5 6
""") == "3\n", "increasing heights"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`1`| Ranh giới chiều rộng gạch đơn | 
|`10 10 10 10 10`|`5`| Chiều rộng giới hạn câu trả lời | 
|`100 1 100`|`1`| Xử lý tắc nghẽn chiều cao | 
|`2 5 5 5 2`|`3`| Hình vuông bên trong hình chữ nhật lớn hơn | 
|`1 2 3 4 5 6`|`3`| Hành vi ngăn xếp đúng khi tăng độ cao | 

## Vỏ cạnh 

Đối với biểu đồ:```
3
100 1 100
```ngăn xếp đầu tiên lưu trữ độ cao tăng dần`100`, rồi gặp nhau`1`. Hai chiều cao`100`các ô được bật lên nhưng chiều rộng khả dụng của chúng chỉ là`1`, nên chúng tạo ra độ dài cạnh`1`. Chiều cao`1`ô có thể trải rộng trên toàn bộ biểu đồ, nhưng chiều cao của nó cũng giới hạn hình vuông ở`1`. Thuật toán trả về`1`, phù hợp với mức tối đa thực sự. 

Vì:```
5
10 10 10 10 10
```tất cả các ô vẫn còn trong ngăn xếp cho đến khi chiều cao bằng 0 của trọng điểm được xử lý. Mỗi chiều cao`10`ô có thể bao phủ toàn bộ chiều rộng của năm ô, nhưng cạnh hình vuông bị giới hạn bởi chiều rộng. Ứng viên tốt nhất sẽ trở thành`min(10, 5) = 5`. 

Vì:```
1
7
```ô duy nhất được lính canh loại bỏ. Chiều rộng của nó được tính như`1`, vậy ứng cử viên là`min(7, 1) = 1`. Điều này ngăn thuật toán xử lý không chính xác chiều cao của ô là cạnh hình vuông. 

Tôi cũng có thể cung cấp phiên bản biên tập cuộc thi ngắn hơn hoặc phiên bản tập trung vào bằng chứng hơn nếu cần.
