---
title: "CF 97C - Chiến lược giành chiến thắng"
description: "Mỗi năm trường cử một đội gồm đúng n sinh viên vào vòng chung kết. Một số sinh viên có thể đã tham gia một lần trước đây và số còn lại là người mới."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "graphs", "math", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 97
codeforces_index: "C"
codeforces_contest_name: "Yandex.Algorithm 2011: Finals"
rating: 2400
weight: 97
solve_time_s: 104
verified: true
draft: false
---

[CF 97C - Chiến lược giành chiến thắng](https://codeforces.com/problemset/problem/97/C) 

**Đánh giá:** 2400 
**Tags:** tìm kiếm nhị phân, đồ thị, toán học, đường đi ngắn nhất 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi năm trường đại học cử một đội gồm chính xác`n`học sinh vào vòng chung kết. Một số sinh viên có thể đã tham gia một lần trước đó và số còn lại là người mới. Nếu chính xác`i`các thành viên đã có kinh nghiệm thi chung kết trước đó thì xác suất giành huy chương năm đó là`p[i]`. 

Tổng cộng một học sinh có thể tham gia tối đa hai lần. Hạn chế đó tạo ra hạn chế về nguồn lực lâu dài. Bất cứ khi nào chúng tôi sử dụng lại những sinh viên có kinh nghiệm trong năm nay, chúng tôi sẽ sử dụng diện mạo thứ hai và cuối cùng của họ. Để tiếp tục gửi đội đi mãi mãi, số lượng học sinh tái sử dụng theo thời gian không thể vượt quá số lượng học sinh mới được giới thiệu theo thời gian. 

Chúng ta phải thiết kế một chiến lược vô hạn. Cho mỗi năm`k`, ta chọn một số`a[k]`giữa`0`Và`n`, Ở đâu`a[k]`có nghĩa là có bao nhiêu sinh viên có kinh nghiệm được sử dụng trong năm đó. Mục tiêu là tối đa hóa xác suất giành huy chương trung bình dài hạn:$$\lim_{t \to \infty} \frac{1}{t} \sum_{k=1}^{t} p[a_k]$$chịu sự ràng buộc về sự tham gia. 

Phần khó khăn là chuỗi là vô hạn. Chúng tôi không tối ưu hóa một năm duy nhất, chúng tôi đang tối ưu hóa chính sách bền vững trong tất cả các năm trong tương lai. 

Ràng buộc`n ≤ 100`là nhỏ bé. Bất kì`O(n^3)`hoặc`O(n^4)`giải pháp là hoàn toàn ổn. Thách thức thực sự là mô hình hóa quy trình một cách chính xác chứ không phải làm giảm hiệu suất. 

Một cách giải thích ngây thơ thật hấp dẫn: vì`p[i]`không giảm, luôn chọn`a[k] = n`. Điều đó có nghĩa là mọi đội đều bao gồm toàn những thành viên có kinh nghiệm. Nhưng sau một năm, tất cả những sinh viên đó sẽ nghỉ hưu vĩnh viễn và không có người mới nào đến bổ sung vào nhóm. Quá trình này trở nên không thể. 

Một sai lầm dễ mắc phải khác là chỉ nghĩ về mức trung bình ở địa phương. Ví dụ: với:```
n = 4
p = [0.1, 0.2, 0.3, 1.0, 1.0]
```sử dụng`a = 4`mỗi năm có vẻ tối ưu vì xác suất là tối đa. Nhưng mọi người tham gia được sử dụng lại đều phải là người mới trước đó. Để duy trì mãi mãi bốn học sinh tái sử dụng mỗi năm, chúng tôi cũng phải giới thiệu mãi mãi bốn học sinh mới mỗi năm. Vì mỗi đội chỉ có tổng cộng bốn suất nên điều đó là không thể. 

Điều kiện khả thi thực sự là toàn cầu. Trong nhiều năm, mỗi giao diện được sử dụng lại sẽ tiêu tốn một giao diện mới trước đó. Nếu chúng ta tính trung bình`x`người tham gia được tái sử dụng mỗi năm thì chúng tôi cũng cần mức trung bình`x`người mới mỗi năm. Vì mỗi đội có`n`tổng số thành viên:$$x + x \le n$$Vì thế:$$x \le \frac{n}{2}$$Định luật bảo toàn ẩn này chính là cốt lõi của vấn đề. 

Trường hợp biên khó phát hiện khi số lần sử dụng lại trung bình tối ưu là phân số. Giả định:```
n = 3
p = [0.0, 0.0, 0.0, 1.0]
```Chúng ta không thể sử dụng bền vững 3 người tham gia có kinh nghiệm mỗi năm. Nhưng chúng ta có thể luân phiên:```
0, 3, 0, 3, ...
```Số lần tái sử dụng trung bình dài hạn trở thành`1.5`, chính xác là giá trị bền vững tối đa`n/2`. Chiến lược tối ưu có thể yêu cầu chuyển đổi giữa các trạng thái khác nhau thay vì chọn mãi một số cố định. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là suy nghĩ về các trạng thái. Tại bất kỳ thời điểm nào, chúng tôi có một số nhóm sinh viên đã tham gia đúng một lần và đủ điều kiện để sử dụng lại. Lựa chọn`a[k]`tiêu thụ`a[k]`học sinh như vậy và giới thiệu`n - a[k]`học sinh mới vào hồ bơi. 

Nếu chúng ta cố gắng mô phỏng mọi chiến lược vô hạn có thể thì không gian tìm kiếm sẽ bùng nổ ngay lập tức. Thậm chí cắt ngắn đến phần đầu tiên`T`năm cho`(n+1)^T`khả năng. Điều đó trở nên vô vọng ngay cả đối với nhỏ`T`. 

Quan sát quan trọng là chỉ có số trung bình dài hạn mới quan trọng. 

Giả sử số lượng sinh viên tái sử dụng trung bình mỗi năm là`x`. Rồi mỗi năm cũng giới thiệu`n-x`những người mới đến. Mỗi người mới sau này có thể đóng góp tối đa một giao diện được sử dụng lại. Trong một khoảng thời gian dài, tổng số lần xuất hiện được sử dụng lại không thể vượt quá tổng số lần xuất hiện mới:$$x \le n - x$$mà đơn giản hóa thành:$$x \le \frac{n}{2}$$Bây giờ quá trình vô hạn trở thành một vấn đề tối ưu hóa tĩnh. 

Chúng tôi có thể tự do kết hợp các loại nhóm khác nhau theo thời gian. Nếu phân số`q_i`số năm sử dụng chính xác`i`học sinh có kinh nghiệm thì:$$\sum q_i = 1$$và tính bền vững đòi hỏi:$$\sum i q_i \le \frac{n}{2}$$Xác suất huy chương trung bình trở thành:$$\sum p_i q_i$$Đây hiện là một vấn đề tối ưu hóa tuyến tính chỉ với một hạn chế về tài nguyên. 

Một thực tế tiêu chuẩn từ quy hoạch tuyến tính được áp dụng ở đây: với một ràng buộc tuyến tính, tồn tại một phương án tối ưu bằng cách sử dụng tối đa hai lựa chọn liền kề. Về mặt hình học, chúng ta đang tối đa hóa hàm tuyến tính trên bao lồi. 

Một cách giải thích khác thậm chí còn rõ ràng hơn. Hãy xem xét điểm:$$(i, p_i)$$Chúng tôi muốn giá trị mong đợi tối đa có thể đạt được ở số lần sử dụng lại trung bình`n/2`. Vì chúng ta có thể kết hợp các chiến lược theo xác suất nên mọi tổ hợp điểm lồi đều có thể đạt được. Câu trả lời chính xác là bao lồi trên được đánh giá tại`x = n/2`. 

Điều đó biến vấn đề thành hình học thuần túy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force hơn các chiến lược | Hàm mũ | Hàm mũ | Quá chậm | 
| Thân lồi / tối ưu hóa tuyến tính | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`và xác suất`p[0...n]`. 
2. Giải thích mọi giá trị dưới dạng điểm`(i, p[i])`. 

Lựa chọn loại đội`i`có nghĩa là sử dụng`i`thành viên có kinh nghiệm năm đó và nhận được phần thưởng như mong đợi`p[i]`. 
3. Số lượng thành viên có kinh nghiệm trung bình dài hạn/năm không vượt quá`n/2`. 

Điều này xuất phát từ việc bảo tồn những người tham gia. Mọi lần xuất hiện có kinh nghiệm đều phải bắt nguồn từ lần xuất hiện mới trước đó. 
4. Vì chúng tôi có thể luân phiên giữa các chiến lược trong nhiều năm nên mọi sự kết hợp lồi của điểm đều có thể đạt được. 

Ví dụ: sử dụng loại nhóm`a`trong 30% số năm và loại`b`trong 70% số năm tạo ra số lần tái sử dụng trung bình và xác suất trung bình bằng với mức trung bình có trọng số của các điểm đó. 
5. Chúng ta phải tối đa hóa xác suất kỳ vọng trong số tất cả các tổ hợp lồi có tọa độ x trung bình bằng`n/2`. 
6. Với mỗi cặp chỉ số`i < j`như vậy:$$i \le \frac{n}{2} \le j$$tính giá trị nội suy tuyến tính tại`x = n/2`:$$p_i + \frac{(n/2 - i)(p_j - p_i)}{j - i}$$Điều này tương ứng với chiến lược trộn`i`Và`j`. 

1. Lấy giá trị lớn nhất trong số tất cả các giá trị nội suy như vậy. 
2. In câu trả lời. 

### Tại sao nó hoạt động 

Bất kỳ chiến lược vô hạn khả thi nào cũng tạo ra tần số`q_i`, Ở đâu`q_i`là phần số năm sử dụng loại nhóm`i`. Những tần số đó tạo thành một tổ hợp lồi. 

Số người tham gia có kinh nghiệm trung bình là:$$\sum i q_i$$và tính khả thi đòi hỏi số lượng này tối đa là`n/2`. 

Xác suất huy chương trung bình đạt được là:$$\sum p_i q_i$$Vì vậy, mọi chiến lược khả thi đều tương ứng với một điểm bên trong bao lồi của`(i, p_i)`hạn chế ở`x ≤ n/2`. 

Ngược lại, bất kỳ sự kết hợp lồi nào thỏa mãn ràng buộc đều có thể được thực hiện bằng cách luân chuyển giữa các loại nhóm tương ứng với tần số phù hợp. 

Vì các mục tiêu tuyến tính trên các tập lồi đạt được tối ưu trên các phân đoạn biên nên việc kiểm tra tất cả các cặp và nội suy tại`x = n/2`tìm ra giá trị tối ưu có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    p = list(map(float, input().split()))

    x = n / 2.0
    ans = 0.0

    for i in range(n + 1):
        for j in range(i, n + 1):
            if i <= x <= j:
                if i == j:
                    val = p[i]
                else:
                    t = (x - i) / (j - i)
                    val = p[i] + t * (p[j] - p[i])

                ans = max(ans, val)

    print("{:.10f}".format(ans))

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo cách giải thích kết hợp lồi. 

Biến`x`lưu trữ số lượng người tham gia có kinh nghiệm trung bình bền vững tối đa mỗi năm, đó là`n / 2`. 

Đối với mỗi cặp`(i, j)`xung quanh`x`, chúng tôi tính toán giá trị thu được bằng cách trộn hai chiến lược đó. Hệ số nội suy:```
t = (x - i) / (j - i)
```thể hiện tần suất chúng tôi sử dụng chiến lược`j`. 

Nếu như`i == j`, không cần nội suy. Trường hợp đó quan trọng khi`n`là số chẵn và`x`là một số nguyên. 

Một lỗi phổ biến là chỉ kiểm tra các cặp liền kề. Mặc dù cặp tối ưu thực sự sẽ nằm ở bao lồi phía trên, việc kiểm tra tất cả các cặp đơn giản hơn và vẫn cực kỳ nhanh chóng đối với`n ≤ 100`. 

Một vấn đề tinh tế khác là độ chính xác của dấu phẩy động. Dung sai lỗi yêu cầu là`1e-6`, vì vậy độ chính xác kép tiêu chuẩn là quá đủ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
0.115590 0.384031 0.443128 0.562356
```Đây:$$x = \frac{3}{2} = 1.5$$Chúng tôi kiểm tra tất cả các phân đoạn bao gồm`1.5`. 

| tôi | j | Giá trị nội suy ở mức 1,5 | 
| --- | --- | --- | 
| 0 | 2 | 0,279359 | 
| 0 | 3 | 0.338973 | 
| 1 | 2 | 0,4135795 | 
| 1 | 3 | 0,42861225 | 

Giá trị tốt nhất là:$$0.42861225$$Đầu ra:```
0.4286122500
```Dấu vết này chứng tỏ tại sao việc trộn có thể tốt hơn mọi lựa chọn số nguyên riêng lẻ. Không`p[1]`cũng không`p[2]`đạt đến mức tối ưu nhưng xen kẽ giữa các chiến lược`1`Và`3`làm. 

### Ví dụ 2 

đầu vào:```
4
0.1 0.2 0.3 0.4 0.5
```Hiện nay:$$x = 2$$| tôi | j | Giá trị nội suy tại 2 | 
| --- | --- | --- | 
| 0 | 2 | 0,3 | 
| 0 | 4 | 0,3 | 
| 1 | 3 | 0,3 | 
| 2 | 2 | 0,3 | 
| 2 | 4 | 0,3 | 

Câu trả lời là:```
0.3000000000
```Từ`x`đã là một số nguyên, chỉ cần chọn`a[k] = 2`hàng năm được bền vững và tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Chúng tôi kiểm tra từng cặp`(i, j)`| 
| Không gian | O(n) | Chỉ mảng xác suất được lưu trữ | 

Với`n ≤ 100`, thuật toán thực hiện tối đa khoảng mười nghìn lượt kiểm tra cặp. Thời gian chạy không đáng kể so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    p = list(map(float, input().split()))

    x = n / 2.0
    ans = 0.0

    for i in range(n + 1):
        for j in range(i, n + 1):
            if i <= x <= j:
                if i == j:
                    val = p[i]
                else:
                    t = (x - i) / (j - i)
                    val = p[i] + t * (p[j] - p[i])

                ans = max(ans, val)

    print("{:.10f}".format(ans))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue().strip()

# provided sample
assert run(
    "3\n0.115590 0.384031 0.443128 0.562356\n"
) == "0.4286122500", "sample 1"

# minimum n
assert run(
    "3\n0 0 0 1\n"
) == "0.5000000000", "minimum size with mixing"

# all equal
assert run(
    "5\n1 1 1 1 1 1\n"
) == "1.0000000000", "all equal probabilities"

# even n, integer optimum
assert run(
    "4\n0.1 0.2 0.3 0.4 0.5\n"
) == "0.3000000000", "integer midpoint"

# convex jump catches interpolation
assert run(
    "4\n0 0 0 1 1\n"
) == "0.6666666667", "best mix is between 2 and 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 0 0 0 1`|`0.5000000000`| Tái sử dụng bền vững từng phần | 
| Mọi xác suất đều bằng 1 |`1.0000000000`| Thân lồi thoái hóa | 
|`n = 4`với xác suất tuyến tính |`0.3000000000`| Xử lý điểm giữa số nguyên | 
|`0 0 0 1 1`|`0.6666666667`| Nội suy giữa các điểm ở xa | 

## Vỏ cạnh 

Hãy xem xét:```
3
0 0 0 1
```Một chiến lược tham lam sẽ luôn chọn`3`những người tham gia có kinh nghiệm vì nó mang lại xác suất`1`. Việc đó thất bại ngay lập tức vì không có người mới nào được giới thiệu. 

Thuật toán của chúng tôi tính toán:$$x = 1.5$$Sự kết hợp lồi tốt nhất có thể đạt được nằm ở giữa`(0,0)`Và`(3,1)`:$$0.5$$Điều này tương ứng với xen kẽ:```
0, 3, 0, 3, ...
```đó là sự bền vững mãi mãi. 

Bây giờ hãy xem xét:```
4
0.1 0.2 0.3 0.4 0.5
```Từ`x = 2`, thuật toán cho phép điểm chính xác`(2, 0.3)`. Không cần trộn. Việc triển khai xử lý việc này một cách chính xác vì nó kiểm tra rõ ràng`i == j`. 

Cuối cùng, xét một dãy không lồi:```
4
0 0.1 0.2 0.9 0.91
```Trực tiếp lựa chọn`a = 2`chỉ cho`0.2`. Thay vào đó, thuật toán trộn lẫn giữa`2`Và`3`:$$0.2 + \frac{2-2}{3-2}(0.9-0.2)=0.2$$và cũng kiểm tra các phân đoạn lớn hơn như`(0,3)`:$$0 + \frac{2}{3} \cdot 0.9 = 0.6$$Giá trị có thể đạt được tốt nhất thực sự là`0.6`, thu được bằng cách sử dụng loại`3`trong hai phần ba năm và loại`0`trong một phần ba. Điều này chứng tỏ tại sao việc kiểm tra tất cả các tổ hợp lồi là cần thiết.
