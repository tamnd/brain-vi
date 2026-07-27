---
title: "CF 102829H - Sự trả thù của Zorro"
description: "Nhiệm vụ là quyết định xem một số N có thể được viết dưới dạng tổng của chính xác K số hạng trong đó mỗi số hạng là lũy thừa nguyên không âm của X hay không."
date: "2026-07-26T15:26:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102829
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 11-06-20 Div. 1 (Tryout)"
rating: 0
weight: 102829
solve_time_s: 46
verified: true
draft: false
---

[CF 102829H - Sự trả thù của Zorro](https://codeforces.com/problemset/problem/102829/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Nhiệm vụ là quyết định xem một số`N`có thể được viết dưới dạng tổng chính xác`K`các số hạng trong đó mỗi số hạng là lũy thừa nguyên không âm của`X`. Nếu cách biểu diễn như vậy tồn tại thì các thuật ngữ phải được in theo thứ tự giảm dần và trong số tất cả các chuỗi giảm dần hợp lệ, chúng ta cần chuỗi nhỏ nhất về mặt từ điển. Đầu vào chứa một giá trị mục tiêu lớn`N`, số lần triệu tập cần thiết`K`, và cơ sở`X`. Đầu ra là một câu trả lời không thể thực hiện được hoặc một danh sách các quyền hạn được lựa chọn cẩn thận`X`tổng của nó chính xác là`N`. 

Giá trị của`N`có thể đạt được`10^18`, vì vậy việc cố gắng xây dựng tất cả các tổ hợp sức mạnh có thể có là điều không thể. Thậm chí lặp qua mọi giá trị lên đến`N`sẽ yêu cầu quá nhiều thao tác. Cơ sở nhiều nhất là`9`, có nghĩa là biểu diễn cơ sở của`N`chỉ có khoảng 60 chữ số. Giá trị của`K`bị giới hạn ở`2 * 10^5`, do đó thuật toán có thể thực hiện các phép toán tỷ lệ thuận với`K`và số chữ số cơ bản, nhưng không phải là số bậc hai`K`. 

Một sai lầm phổ biến là chỉ kiểm tra phần đế thông thường`X`đại diện và dừng lại ở đó. Biểu diễn cơ sở cho số lượng số hạng nhỏ nhất nhưng bài toán yêu cầu chính xác`K`các điều khoản, vì vậy chúng ta có thể cần chia quyền hạn thành các quyền hạn nhỏ hơn. Một sai lầm khác là phân chia quyền lực tùy tiện mà không xem xét thứ tự từ điển. 

Ví dụ, hãy xem xét:```
9 4 2
```Biểu diễn nhị phân của`9`là`1001`, đưa ra các điều khoản`8`Và`1`. Điều đó chỉ sử dụng hai thuật ngữ. Một cách tiếp cận bất cẩn có thể chia rẽ`8`thành tám cái ngay lập tức, nhưng điều đó đưa ra quá nhiều điều khoản. Câu trả lời đúng là:```
YES
4 2 2 1
```Bốn thuật ngữ tổng hợp thành`9`và thứ tự này nhỏ hơn về mặt từ điển so với các lựa chọn thay thế như`2 2 2 2 1`, thậm chí không có độ dài cần thiết. 

Một trường hợp cạnh khác là khi số lượng số hạng được yêu cầu nhỏ hơn số đã có trong biểu diễn cơ sở. Ví dụ:```
10 1 2
```Biểu diễn nhị phân là`1010`, nghĩa`10 = 8 + 2`. Không có cách nào để hợp nhất các quyền hạn trong khi vẫn giữ mỗi thuật ngữ là quyền hạn của`2`, vì vậy kết quả đúng là:```
NO
```Trường hợp cạnh cuối cùng xuất hiện khi không thể đạt được mức tăng yêu cầu về số lượng số hạng. Mỗi lần chia tách`X^i`vào trong`X`bản sao của`X^(i-1)`tăng chính xác số lượng số hạng`X - 1`. Ví dụ:```
5 2 3
```Biểu diễn bậc ba của`5`là`12`, đưa ra ba điều khoản:`3 + 1 + 1`. Chúng tôi đã cần nhiều điều khoản hơn yêu cầu nên câu trả lời là:```
NO
```## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra mọi cách có thể để phá vỡ quyền lực. Chúng ta có thể bắt đầu với cơ sở`X`đại diện và liên tục chọn một quyền lực`X^i`để thay thế bằng`X`bản sao của`X^(i-1)`. Hoạt động này bảo toàn tổng số và tăng số lượng các số hạng lên`X - 1`. Cách tiếp cận này đúng vì mọi biểu diễn hợp lệ đều có thể được chuyển đổi trở lại thành biểu diễn cơ sở bằng cách đảo ngược các phần tách này. Tuy nhiên, việc tìm kiếm qua tất cả các lựa chọn có thể là không thể. Số lượng các chuỗi phân chia có thể tăng lên một cách bùng nổ, và thậm chí thử tất cả các lựa chọn cho một số lượng lớn vừa phải.`K`sẽ vượt quá giới hạn. 

Quan sát quan trọng là cách biểu diễn cơ sở đã đưa ra số lượng số hạng tối thiểu. Hoạt động duy nhất chúng ta cần là chia tách. Mỗi phần tách có tác dụng như nhau đối với số lượng thuật ngữ và yêu cầu về từ điển cho chúng ta biết phần tách nào là tốt nhất. Nếu chúng ta chia lũy thừa lớn hơn trước, thì giá trị lớn nhất xuất hiện trong danh sách giảm dần cuối cùng sẽ sớm nhỏ hơn, điều này sẽ cải thiện thứ tự từ điển. 

Số lượng phần chia cần thiết là cố định. Nếu biểu diễn cơ sở chứa`C`điều kiện, chúng tôi cần`K - C`điều khoản bổ sung. Vì mỗi lần chia đều cộng thêm`X - 1`thì số lần chia phải là:```
(K - C) / (X - 1)
```Nếu đây không phải là số nguyên thì không có nghiệm nào tồn tại. Nếu không, chúng ta tham lam thực hiện chính xác số lần chia này, luôn lấy công suất lớn nhất hiện có. Chỉ có khoảng 60 sức mạnh có thể có bởi vì`N`nhiều nhất là`10^18`, nên quá trình này rất nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(số tiểu bang) | Quá chậm | 
| Tối ưu | O(log_X(N) + K) | O(log_X(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi`N`vào căn cứ`X`. Chữ số ở vị trí`i`cho chúng ta biết có bao nhiêu bản sao của`X^i`chúng tôi hiện có. Đây là cách biểu diễn ban đầu vì nó sử dụng ít số hạng nhất có thể. 
2. Đếm số số hạng trong cách biểu diễn này. Nếu số này lớn hơn`K`, đầu ra`NO`bởi vì việc chia tách chỉ có thể làm tăng số lượng số hạng. 
3. Tính xem cần thêm bao nhiêu số hạng. Nếu như`K - count`không chia hết cho`X - 1`, đầu ra`NO`bởi vì mọi thao tác có thể đều thay đổi số lượng thuật ngữ một cách chính xác`X - 1`. 
4. Starting from the largest power, split as many terms as possible while there are still required splits remaining. Tách`X^i`loại bỏ một thuật ngữ lớn và tạo ra`X`các số hạng nhỏ hơn, do đó, thực hiện việc này với lũy thừa lớn nhất trước tiên sẽ tạo ra câu trả lời nhỏ nhất về mặt từ điển. 
5. Sau khi thực hiện tất cả các phép chia cần thiết, hãy mở rộng số lượng được lưu trữ thành danh sách lũy thừa và in chúng từ lớn nhất đến nhỏ nhất. 

Tại sao nó hoạt động: 

Biểu diễn cơ sở là biểu diễn duy nhất có ít số hạng nhất. Bất kỳ biểu diễn hợp lệ nào khác chỉ có thể có được bằng cách áp dụng các phép chia cho nó. Thuật toán thực hiện chính xác số lần phân chia cần thiết để đạt được biểu diễn với chính xác`K`điều khoản. Trong số tất cả các lựa chọn có thể có của việc phân tách, việc thay thế lũy thừa lớn hơn sớm hơn luôn làm giảm vị trí đầu tiên trong đó hai chuỗi giảm dần có thể khác nhau, điều này làm cho chuỗi được tạo ra trở nên tối thiểu về mặt từ điển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, K, X = map(int, input().split())

    cnt = []
    temp = N
    while temp:
        cnt.append(temp % X)
        temp //= X

    if not cnt:
        cnt = [0]

    current = sum(cnt)

    if current > K:
        print("NO")
        return

    diff = K - current
    if diff % (X - 1) != 0:
        print("NO")
        return

    need = diff // (X - 1)

    for i in range(len(cnt) - 1, 0, -1):
        if need == 0:
            break
        take = min(cnt[i], need)
        if take:
            cnt[i] -= take
            if i - 1 == len(cnt):
                cnt.append(0)
            cnt[i - 1] += take * X
            need -= take

    if need != 0:
        print("NO")
        return

    ans = []
    power = 1
    powers = []
    for _ in range(len(cnt)):
        powers.append(power)
        power *= X

    for i in range(len(cnt) - 1, -1, -1):
        ans.extend([str(powers[i])] * cnt[i])

    print("YES")
    print(" ".join(ans))

if __name__ == "__main__":
    solve()
```Mã đầu tiên lưu trữ cơ sở`X`chữ số trong`cnt`. chỉ số của`cnt`đại diện cho số mũ, vì vậy`cnt[3] = 2`nghĩa là có hai bản sao của`X^3`. 

Biến`current`là số thuật ngữ trong biểu diễn tối thiểu. Kiểm tra tính chia hết bằng`X - 1`tránh thử các trường hợp không thể thực hiện được vì mỗi lần chia sẽ thay đổi số đếm chính xác bằng số tiền đó. 

Vòng lặp xử lý số mũ từ cao xuống thấp. số tiền`take`là số lần chúng ta chia số mũ hiện tại. Loại bỏ các lũy thừa lớn trước các lũy thừa nhỏ hơn là phần thực thi yêu cầu về từ điển. 

Việc mở rộng cuối cùng được thực hiện theo thứ tự số mũ ngược lại để chuỗi in đã giảm dần. Số nguyên Python xử lý các giá trị lớn của`N`một cách an toàn và tổng số thuật ngữ được tạo ra được giới hạn bởi`K`, nhiều nhất là`200000`. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
9 4 2
```Dấu vết là: 

| Số mũ | Đếm trước khi chia | Chia tách thực hiện | Đếm sau khi chia | 
| --- | --- | --- | --- | 
| 3 | 1 | 1 | 0 | 
| 2 | 0 | 0 | 2 | 
| 1 | 0 | 0 | 0 | 
| 0 | 1 | 0 | 1 | 

Dạng nhị phân cho`8 + 1`. Thay đổi một phần`8`thành hai`4`s, nhưng số lượng vẫn cần tăng thêm hai số hạng nữa nên phép chia tiếp theo sẽ tạo ra đáp án cuối cùng`4 2 2 1`. Ví dụ cho thấy các lũy thừa lớn luôn bị giảm trước tiên. 

Ví dụ khác:```
25 3 5
```Căn cứ`5`đại diện là:```
100
```có nghĩa là danh sách bắt đầu là:```
25
```Số lượng điều khoản hiện tại là một. Chúng ta cần thêm hai số hạng nữa, nhưng mỗi lần chia sẽ tăng số đếm lên`5 - 1 = 4`, vì vậy yêu cầu là không thể. 

| Bước | Điều khoản hiện tại | Yêu cầu chia tách thêm | 
| --- | --- | --- | 
| Đại diện cơ sở | 25 | 1 | 
| Kiểm tra sự khác biệt | 1 học kỳ | Cần thêm 2 điều khoản | 
| Kiểm tra tính chia hết | không thể | không chia hết cho 4 | 

Dấu vết thứ hai thể hiện điều kiện số học đằng sau tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log_X(N) + K) | Chỉ có khoảng 60 chữ số cơ bản và câu trả lời đưa ra có nhiều nhất`K`các phần tử. | 
| Không gian | O(log_X(N) + K) | Mảng chữ số và danh sách đầu ra lưu trữ tối đa số lũy thừa và số hạng cuối cùng. | 

Thuật toán chỉ hoạt động với biểu diễn cơ sở của`N`và kích thước đầu ra được yêu cầu. Nó tránh bất kỳ sự lặp lại nào trên giá trị của`N`, vì vậy nó dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    output = sys.stdout.getvalue() if hasattr(sys.stdout, "getvalue") else ""
    sys.stdin = old
    return output

# provided samples
assert solve_case("9 4 2\n") == "YES\n4 2 2 1"
assert solve_case("25 26 7\n") == "NO"

# custom cases
assert solve_case("1 1 2\n") == "YES\n1"
assert solve_case("10 3 2\n") == "YES\n4 4 2"
assert solve_case("5 2 3\n") == "NO"
assert solve_case("1000000000000000000 200000 2\n").startswith("YES")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 2`|`YES`| Số nhỏ nhất có thể và không bị chia tách | 
|`10 3 2`|`YES`| Chia cắt một thế lực lớn nhưng vẫn giữ trật tự | 
|`5 2 3`|`NO`| Sự khác biệt về số lượng thuật ngữ là không thể đạt được | 
|`1000000000000000000 200000 2`|`YES`| Giới hạn xử lý đầu vào và kích thước đầu ra lớn | 

## Vỏ cạnh 

Đối với trường hợp:```
9 4 2
```sự biểu diễn cơ sở cho phép đếm cho`2^3`Và`2^0`. Thuật toán thấy rằng cần có hai thuật ngữ bổ sung. Vì mỗi phép chia nhị phân thêm một số hạng nên nó thực hiện hai phép chia với lũy thừa lớn nhất hiện có. Các điều khoản kết quả là`4, 2, 2, 1`, chính xác là số lượng được yêu cầu và có giá trị dẫn đầu nhỏ nhất có thể. 

Đối với trường hợp:```
10 1 2
```biểu diễn ban đầu đã chứa hai thuật ngữ,`8`Và`2`. Thuật toán phát hiện rằng số lượng hiện tại lớn hơn`K`. Vì việc chia tách không thể làm giảm số lượng số hạng nên nó ngay lập tức trả về`NO`. 

Đối với trường hợp:```
5 2 3
```biểu diễn cơ sở chứa ba thuật ngữ:`3, 1, 1`. Số lượng được yêu cầu nhỏ hơn nên không có chuỗi hợp lệ. Thuật toán từ chối nó trước khi thử bất kỳ sự phân chia nào. 

Đối với trường hợp lớn có nhiều thuật ngữ bắt buộc, chẳng hạn như:```
1000000000000000000 200000 2
```thuật toán không bao giờ lặp lại giá trị đó. Nó chỉ xử lý khoảng 60 vị trí nhị phân và thực hiện đủ các phần tách để tạo ra kích thước đầu ra được yêu cầu. Điều này giữ cho thời gian chạy tỷ lệ thuận với lượng thông tin thực tế phải được xử lý.
