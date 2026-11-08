---
title: "CF 144A - Sự Đến Của Đại Tướng"
description: "Chúng ta có một hàng lính, mỗi người có một chiều cao. Tướng quân chỉ quan tâm đến hai vị trí trong đội hình. Người lính cao nhất phải đứng ở phía trước và người lính thấp nhất phải đứng ở cuối. Thứ tự của những người khác là không liên quan."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 144
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 103 (Div. 2)"
rating: 800
weight: 144
solve_time_s: 112
verified: true
draft: false
---

[CF 144A - Sự xuất hiện của Đại tướng](https://codeforces.com/problemset/problem/144/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hàng lính, mỗi người có một chiều cao. Tướng quân chỉ quan tâm đến hai vị trí trong đội hình. Người lính cao nhất phải đứng ở phía trước và người lính thấp nhất phải đứng ở cuối. Thứ tự của những người khác là không liên quan. 

Trong một giây, chúng ta có thể hoán đổi hai người lính lân cận. Nhiệm vụ là tính toán số lần hoán đổi tối thiểu cần thiết để đạt được sự sắp xếp cần thiết. 

Đầu vào đưa ra số lượng binh lính và chiều cao của họ theo thứ tự hiện tại. Đầu ra là một số nguyên duy nhất, số lần hoán đổi liền kề tối thiểu được yêu cầu. 

Hạn chế rất nhỏ, chỉ có tối đa 100 binh sĩ. Ngay cả một mô phỏng bậc hai cũng sẽ phù hợp thoải mái trong thời gian giới hạn. Tuy nhiên, vấn đề có khả năng quan sát thời gian tuyến tính rõ ràng hơn và thách thức thực sự là xử lý chính xác các độ cao tối đa và tối thiểu trùng lặp. 

Phần khó khăn là quyết định lần xuất hiện nào của những người lính cao nhất và thấp nhất sẽ di chuyển. Nếu nhiều binh sĩ chia sẻ chiều cao tối đa, chúng ta nên di chuyển người ngoài cùng bên trái lên phía trước, vì nó đòi hỏi ít sự hoán đổi nhất. Nếu nhiều binh sĩ có cùng chiều cao tối thiểu, chúng ta nên di chuyển người ngoài cùng bên phải về phía sau, một lần nữa giảm thiểu việc hoán đổi. 

Việc thực hiện bất cẩn thường thất bại khi mức tối đa đã chọn vượt qua mức tối thiểu đã chọn trong quá trình di chuyển. 

Hãy xem xét đầu vào này:```
5
2 1 2 1 2
```Câu trả lời đúng là:```
3
```Chúng tôi di chuyển ngoài cùng bên trái`2`tại chỉ mục`0`, vốn đã ở phía trước, có giá`0`trao đổi. Sau đó chúng ta di chuyển ngoài cùng bên phải`1`tại chỉ mục`3`đến cuối cùng, chi phí`1`tráo đổi. Tổng chi phí là`1`. 

Nếu chúng ta chọn nhầm mức tối thiểu đầu tiên thay vì mức tối thiểu cuối cùng, chúng ta sẽ cần nhiều giao dịch hoán đổi hơn. 

Một trường hợp tinh tế khác xuất hiện khi người lính tối đa đứng bên phải người lính tối thiểu.```
5
1 3 2 4 5
```Người lính cao nhất`5`đang ở chỉ mục`4`, và người lính lùn nhất`1`đang ở chỉ mục`0`. 

Di chuyển`5`về phía trước mất`4`trao đổi. Trong quá trình này, người lính thấp nhất sẽ dịch chuyển một vị trí sang phải. Điều đó có nghĩa là việc di chuyển mức tối thiểu đến cuối bây giờ yêu cầu ít hoán đổi hơn trước. Việc quên điều chỉnh này sẽ tạo ra một câu trả lời quá lớn. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ mô phỏng các giao dịch hoán đổi theo đúng nghĩa đen. Chúng ta có thể liên tục bong bóng người lính cao nhất về phía trước và sau đó bong bóng người lính thấp nhất về phía sau. Vì mỗi lần hoán đổi thay đổi các vị trí lân cận, điều này trực tiếp mô hình hóa báo cáo vấn đề và rất dễ lý giải. 

Vì`n = 100`, thậm chí là một`O(n^2)`mô phỏng là hoàn toàn chấp nhận được. Trong trường hợp xấu nhất, chúng tôi thực hiện đại khái`100 * 100 = 10,000`hoạt động, đó là rất nhỏ. 

Vấn đề trở nên đơn giản hơn nhiều khi chúng ta nhận thấy rằng chúng ta không bao giờ cần mô phỏng các giao dịch hoán đổi một cách rõ ràng. Số lần hoán đổi cần thiết để di chuyển một phần tử lên phía trước chính xác là chỉ mục của nó. Số lần hoán đổi cần thiết để di chuyển một phần tử về phía sau là chính xác`(n - 1 - index)`. 

Điều này biến vấn đề thành việc lựa chọn các lần xuất hiện chính xác của độ cao tối đa và tối thiểu. 

Chúng tôi chọn: 

1. Sự xuất hiện ngoài cùng bên trái của chiều cao tối đa. 
2. Sự xuất hiện ngoài cùng bên phải của chiều cao tối thiểu. 

Mức tối đa ngoài cùng bên trái giảm thiểu các giao dịch hoán đổi cần thiết để đưa nó lên phía trước. Mức tối thiểu ngoài cùng bên phải giảm thiểu các giao dịch hoán đổi cần thiết để đưa nó về phía sau. 

Có một sự tương tác giữa hai chuyển động này. Nếu mức tối đa bắt đầu ở bên phải mức tối thiểu, thì việc di chuyển mức tối đa lên phía trước sẽ dịch chuyển mức tối thiểu sang phải một bước. Điều đó có nghĩa là mức tối thiểu bây giờ là một bước gần hơn đến điểm cuối, vì vậy chúng tôi trừ đi một từ tổng số. 

Quan sát này làm giảm toàn bộ vấn đề thành một lần quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc quân số và danh sách độ cao. 
2. Tìm lần xuất hiện ngoài cùng bên trái của chiều cao tối đa. 

Người lính này yêu cầu ít lần hoán đổi nhất để có thể ra mặt trận. 
3. Tìm điểm xuất hiện ngoài cùng bên phải của chiều cao tối thiểu. 

Người lính này yêu cầu ít lần hoán đổi nhất để đi đến cuối cùng. 
4. Tính toán số lần hoán đổi cần thiết để di chuyển số lượng binh sĩ tối đa lên phía trước. 

Nếu mức tối đa là ở chỉ số`max_pos`, chi phí chỉ đơn giản là`max_pos`. 
5. Tính toán số lần hoán đổi cần thiết để đưa số lính tối thiểu về cuối. 

Nếu mức tối thiểu là ở chỉ số`min_pos`, chi phí là`(n - 1 - min_pos)`. 
6. Kiểm tra xem`max_pos > min_pos`. 

Nếu đúng, mức tối đa sẽ vượt qua mức tối thiểu khi di chuyển sang trái. Điều này sẽ dịch chuyển một vị trí tối thiểu sang phải, giảm khoảng cách còn lại của nó đến cuối bằng một lần hoán đổi. 
7. Trừ một trong trường hợp đó. 
8. In tổng số cuối cùng. 

### Tại sao nó hoạt động 

Các lần hoán đổi liền kề chỉ thay đổi vị trí từng bước một, vì vậy số lần hoán đổi cần thiết để di chuyển một người lính bằng với khoảng cách đã di chuyển. 

Việc chọn mức tối đa ngoài cùng bên trái đảm bảo khoảng cách nhỏ nhất có thể đến phía trước trong số tất cả những người lính có chiều cao tối đa. Việc chọn mức tối thiểu ngoài cùng bên phải đảm bảo khoảng cách nhỏ nhất có thể đến phía sau trong số tất cả những người lính có chiều cao tối thiểu. 

Sự phụ thuộc duy nhất giữa hai phép toán xảy ra khi giá trị lớn nhất bắt đầu ở bên phải giá trị nhỏ nhất. Trong tình huống đó, việc di chuyển sang trái tối đa sẽ tự động dịch chuyển sang phải một bước tối thiểu. Việc tính đến sự chồng chéo đơn lẻ này sẽ tạo ra số lượng giao dịch hoán đổi tối thiểu chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

max_height = max(a)
min_height = min(a)

max_pos = a.index(max_height)

min_pos = -1
for i in range(n):
    if a[i] == min_height:
        min_pos = i

answer = max_pos + (n - 1 - min_pos)

if max_pos > min_pos:
    answer -= 1

print(answer)
```Giải pháp bắt đầu bằng cách xác định độ cao cao nhất và ngắn nhất bằng cách sử dụng tính năng tích hợp sẵn của Python`max`Và`min`. 

Để có vị trí tối đa,`a.index(max_height)`tự động trả về lần xuất hiện đầu tiên, đó chính xác là những gì chúng ta muốn. 

Đối với vị trí tối thiểu, chúng tôi không thể sử dụng`index`, bởi vì điều đó sẽ xuất hiện lần đầu tiên thay vì lần cuối cùng. Thay vào đó, chúng tôi quét mảng và ghi đè liên tục`min_pos`bất cứ khi nào chúng ta thấy giá trị tối thiểu. Sau khi vòng lặp kết thúc,`min_pos`lưu trữ lần xuất hiện ngoài cùng bên phải. 

Công thức:```
max_pos + (n - 1 - min_pos)
```đếm các giao dịch hoán đổi một cách độc lập. 

Việc điều chỉnh:```
if max_pos > min_pos:
    answer -= 1
```xử lý sự chồng chéo gây ra do di chuyển binh sĩ tối đa trước. 

Điều kiện này rất dễ bị sai. Chúng tôi chỉ trừ khi mức tối đa ban đầu đứng ở bên phải mức tối thiểu. Nếu mức tối đa đã vượt quá mức tối thiểu thì hai chuyển động đó không ảnh hưởng đến nhau. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
33 44 11 22
```| Biến | Giá trị | 
| --- | --- | 
| Chiều cao tối đa | 44 | 
| Chiều cao tối thiểu | 11 | 
| max_pos | 1 | 
| phút_pos | 2 | 
| Hoán đổi tối đa | 1 | 
| Hoán đổi tối thiểu | 1 | 
| Điều chỉnh chồng chéo | 0 | 
| Câu trả lời cuối cùng | 2 | 

Người lính cao nhất di chuyển sang trái một bước và người lính thấp nhất di chuyển sang phải một bước. Đường đi của chúng không trùng nhau nên câu trả lời đơn giản là`1 + 1`. 

### Mẫu 2 

đầu vào:```
7
10 10 58 31 63 40 76
```| Biến | Giá trị | 
| --- | --- | 
| Chiều cao tối đa | 76 | 
| Chiều cao tối thiểu | 10 | 
| max_pos | 6 | 
| phút_pos | 1 | 
| Hoán đổi tối đa | 6 | 
| Hoán đổi tối thiểu | 5 | 
| Điều chỉnh chồng chéo | -1 | 
| Câu trả lời cuối cùng | 10 | 

Người lính cao nhất bắt đầu ở bên phải người lính tối thiểu đã chọn. Trong khi di chuyển`76`sang trái, nó sẽ tự động đẩy bước tối thiểu đến gần điểm cuối. Nếu không trừ đi một, chúng ta sẽ xuất ra không chính xác`11`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi quét mảng một số lần không đổi | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Với tối đa 100 binh sĩ, giải pháp này dễ dàng nằm trong giới hạn. Ngay cả việc mô phỏng lực lượng vũ phu cũng có thể vượt qua, nhưng giải pháp tuyến tính sẽ đơn giản và rõ ràng hơn một khi hiểu được quan sát vị trí. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    max_height = max(a)
    min_height = min(a)

    max_pos = a.index(max_height)

    min_pos = -1
    for i in range(n):
        if a[i] == min_height:
            min_pos = i

    ans = max_pos + (n - 1 - min_pos)

    if max_pos > min_pos:
        ans -= 1

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    backup_stdout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided samples
assert run("4\n33 44 11 22\n") == "2", "sample 1"
assert run("7\n10 10 58 31 63 40 76\n") == "10", "sample 2"

# minimum size
assert run("2\n1 2\n") == "1", "minimum size"

# all equal values
assert run("5\n7 7 7 7 7\n") == "0", "all equal"

# duplicate maximum and minimum
assert run("5\n2 1 2 1 2\n") == "1", "choose leftmost max and rightmost min"

# overlap adjustment case
assert run("5\n1 3 2 4 5\n") == "7", "crossing positions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 2`|`1`| Kích thước mảng tối thiểu | 
|`5 / 7 7 7 7 7`|`0`| Tất cả các giá trị giống nhau | 
|`5 / 2 1 2 1 2`|`1`| Xử lý trùng lặp đúng | 
|`5 / 1 3 2 4 5`|`7`| Điều chỉnh chồng chéo đúng | 

## Vỏ cạnh 

Hãy xem xét trường hợp tất cả những người lính có cùng chiều cao. 

đầu vào:```
5
7 7 7 7 7
```Mức tối đa bên trái là ở chỉ mục`0`và mức tối thiểu ngoài cùng bên phải là tại chỉ số`4`. 

Việc tính toán trở thành:```
0 + (4 - 4) = 0
```Không cần hoán đổi vì người lính đầu tiên đã là người lính có chiều cao tối đa và người lính cuối cùng đã là người lính có chiều cao tối thiểu. 

Bây giờ hãy xem xét các giá trị tối thiểu trùng lặp. 

đầu vào:```
5
2 1 2 1 2
```Sự lựa chọn đúng là đúng nhất`1`tại chỉ mục`3`. 

Thuật toán tính toán:```
max_pos = 0
min_pos = 3
answer = 0 + (4 - 3) = 1
```Nếu chúng ta sử dụng nhầm cái đầu tiên`1`, chúng ta sẽ nhận được:```
0 + (4 - 1) = 3
```đó không phải là tối thiểu. 

Cuối cùng, hãy xem xét trường hợp chồng chéo. 

đầu vào:```
5
1 3 2 4 5
```Người lính cao nhất`5`bắt đầu tại chỉ mục`4`, và người lính lùn nhất`1`bắt đầu tại chỉ mục`0`. 

Số lượng trao đổi thô là:```
4 + 4 = 8
```Nhưng di chuyển`5`dịch chuyển sang trái`1`tự động sang phải một lần. Mức tối thiểu thực sự là:```
8 - 1 = 7
```Thuật toán phát hiện điều này bằng cách sử dụng điều kiện:```
if max_pos > min_pos:
    answer -= 1
```xử lý chính xác sự tương tác giữa hai chuyển động.
