---
problem: 1363B
contest_id: 1363
problem_index: B
name: "Subsequence Hate"
contest_name: "Codeforces Round 646 (Div. 2)"
rating: 1400
tags: ["implementation", "strings"]
answer: passed_samples
verified: true
solve_time_s: 210
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e4654-22f0-83ec-bf43-1ed4f2478ee6
---

#CF 1363B - Hậu Quả Ghét 

**Đánh giá:** 1400 
**Tags:** triển khai, chuỗi 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 3 phút 30 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Share:** https://chatgpt.com/share/6a2e4654-22f0-83ec-bf43-1ed4f2478ee6

 --- 

##Giải pháp 

## Hiểu vấn đề 

We are given a binary string and allowed to flip any character any number of times. Mỗi lần lật thay đổi một bit từ 0 thành 1 hoặc từ 1 thành 0. Mục tiêu là biến đổi chuỗi sao cho nó trở nên “tốt”, nghĩa là nếu chúng ta cố gắng chọn ba chỉ số theo thứ tự tăng dần, chúng ta sẽ không bao giờ có thể tạo thành chuỗi con 0, 1, 0 hoặc chuỗi con 1, 0, 1. 

The important detail is that subsequence here does not require adjacency. It is enough that the characters appear in order, even if they are far apart. Vì vậy, điều kiện là toàn cầu, không phải cục bộ. Chúng tôi không chỉ tránh các mẫu như 010 dưới dạng chuỗi con mà còn tránh dưới dạng chuỗi con ở bất kỳ đâu trong chuỗi. 

The constraint on length is small, at most 1000 per string and up to 100 test cases. Điều đó ngay lập tức loại trừ mọi nhu cầu tối ưu hóa nặng như bậc hai hoặc tệ hơn cho mỗi trường hợp thử nghiệm, nhưng cũng gợi ý rằng giải pháp O(n) hoặc O(n log n) cho mỗi chuỗi là đủ dễ dàng. Ngay cả O(n^2) cũng có thể vượt qua, nhưng cấu trúc của điều kiện cho thấy chúng ta không cần đến nó. 

Trường hợp cạnh tinh tế phát sinh từ thực tế là các mẫu chuỗi con có thể được hình thành ngay cả khi các ký tự không liền kề nhau. For example, in a string like 1001, there is no substring 101, but there is a subsequence 1 (first character), 0 (second), 1 (last). A naive approach that only checks substrings or local patterns would fail here. Another trap is thinking that removing all alternations is enough, because even non-adjacent alternations can still form forbidden subsequences.

 Khó khăn cốt lõi là việc lật một ký tự sẽ ảnh hưởng đến nhiều chuỗi con tiềm năng trên toàn cầu chứ không chỉ cấu trúc cục bộ, vì vậy chúng ta cần mô tả đặc tính toàn cục về chuỗi “tốt” trông như thế nào. 

## Phương pháp tiếp cận 

We start by asking what kind of binary string cannot contain 010 or 101 as a subsequence. Một cách mạnh mẽ để suy nghĩ về nó là thử tất cả các bộ lật có thể, tạo ra tất cả các chuỗi có thể tiếp cận và kiểm tra từng chuỗi để tìm các chuỗi con bị cấm. Việc kiểm tra chuỗi 010 hoặc 101 dưới dạng chuỗi con có thể được thực hiện trong O(n) và có 2^n cấu hình lật có thể xảy ra, vì vậy phương pháp này hoàn toàn không khả thi ngay cả với n = 1000. 

Instead, we try to understand what structural property prevents both patterns. Nếu một chuỗi chứa cả 0 và 1, đồng thời chúng cũng được xen kẽ một cách linh hoạt, thì có thể chọn số 0, sau đó là số 1, rồi số 0 khác hoặc ngược lại. The key observation is that any “mixed” structure with multiple transitions between 0 and 1 creates room for these alternating subsequences.

 Một cách hữu ích để suy nghĩ về nó là xem xét chuỗi cuối cùng. Nếu tốt thì không thể có cả hai mẫu 010 và 101 làm dãy con. Điều đó buộc chuỗi có hình dạng rất hạn chế: nó có thể có nhiều nhất một “chuyển đổi” giữa 0 và 1. Mặt khác, nhiều khối bit khác nhau sẽ cho phép xây dựng các chuỗi con xen kẽ. 

So any good string must be of the form all 0s followed by all 1s, or all 1s followed by all 0s, or entirely uniform. These are exactly the monotone binary strings with at most one boundary between symbols.

 Điều này làm giảm vấn đề thành một nhiệm vụ chuyển đổi đơn giản: với chuỗi gốc, chúng tôi muốn chuyển đổi nó thành 000…0111…1 hoặc 111…1000…0 với số lần lật tối thiểu. Lực lượng mạnh mẽ trên tất cả các điểm phân chia giờ đây thật dễ dàng: đối với mỗi vị trí, tính toán số lần lật cần thiết để làm cho mọi thứ bên trái bằng một giá trị và mọi thứ bên phải bằng giá trị kia và lấy mức tối thiểu trên cả hai hướng. 

This reduces the problem from reasoning about subsequences to a linear scan with prefix counts.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả các tập hợp con lật) | O(2^n · n) | O(n) | Quá chậm | 
| Tối ưu (thử tách + đếm tiền tố) | O(n) mỗi chuỗi | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sẽ xây dựng số tiền tố gồm số 0 và số 1 để đánh giá nhanh bất kỳ sự phân chia nào. 

1. Đối với mỗi trường hợp thử nghiệm, hãy tính toán các mảng tiền tố trong đó chúng ta có thể truy vấn có bao nhiêu số 0 và số 1 xuất hiện trong bất kỳ tiền tố nào của chuỗi. Điều này cho phép đếm thời gian liên tục cho bất kỳ phân đoạn nào. 
2. Cân nhắc việc tạo chuỗi cuối cùng có dạng 000…0111…1. Đối với mọi vị trí phân chia có thể có i, chúng tôi giả định các vị trí [0, i) đều là 0 và các vị trí [i, n) đều là 1. 
3. Đối với phần chia cố định i, hãy tính chi phí bằng số đơn vị ở phần bên trái (chúng phải được lật về 0) cộng với số số 0 ở phần bên phải (chúng phải được lật về 1). Điều này trực tiếp đếm sự không phù hợp với cấu trúc mục tiêu. 
4. Lặp lại quy trình tương tự cho mẫu đảo ngược 111…1000…0, trong đó bên trái trở thành tất cả 1 và bên phải trở thành tất cả 0. Chi phí bằng 0 ở bên trái cộng với 1 ở bên phải. 
5. Lấy chi phí tối thiểu trên tất cả các phần tách và cả hai mẫu. Đó là câu trả lời cho trường hợp thử nghiệm. 

Lý do chính khiến chúng ta có thể hạn chế bản thân trong một cấu trúc phân tách duy nhất là vì bất kỳ chuỗi cuối cùng hợp lệ nào cũng không thể có hai lựa chọn thay thế riêng biệt. Nếu đúng như vậy, nó sẽ ngay lập tức cho phép xây dựng 010 hoặc 101 làm dãy con bằng cách chọn đại diện từ mỗi khu vực. Vì vậy, tất cả các chuỗi hợp lệ sẽ thu gọn lại thành dạng hai khối đơn giản này. 

### Tại sao nó hoạt động 

Bất kỳ chuỗi nào có nhiều hơn một chuyển đổi giữa 0 và 1 nhất thiết phải chứa cả hai mẫu 010 và 101 dưới dạng các chuỗi con, bởi vì chúng ta luôn có thể chọn các ký tự từ các phân đoạn xen kẽ. Do đó, mọi mục tiêu hợp lệ phải có nhiều nhất một lần chuyển đổi. Thuật toán của chúng tôi liệt kê tất cả các mục tiêu hợp lệ như vậy bằng cách thử tất cả các điểm phân chia và cả hai hướng có thể. Vì chúng tôi tính toán khoảng cách Hamming chính xác (số lần lật cần thiết) cho từng cấu trúc ứng cử viên, nên giá trị tối thiểu trên tất cả chúng là số thao tác tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = input().strip()
    n = len(s)

    pref0 = [0] * (n + 1)
    pref1 = [0] * (n + 1)

    for i, ch in enumerate(s):
        pref0[i + 1] = pref0[i] + (ch == '0')
        pref1[i + 1] = pref1[i] + (ch == '1')

    ans = n

    for i in range(n + 1):
        left_ones = pref1[i]
        right_zeros = pref0[n] - pref0[i]
        cost01 = left_ones + right_zeros

        left_zeros = pref0[i]
        right_ones = pref1[n] - pref1[i]
        cost10 = left_zeros + right_ones

        ans = min(ans, cost01, cost10)

    print(ans)
```Việc triển khai dựa vào tổng tiền tố để đánh giá số lượng phân đoạn trong O(1). Vòng lặp trên các điểm phân chia đảm bảo chúng tôi xem xét mọi vị trí có thể có của quá trình chuyển đổi được phép duy nhất. Hai công thức chi phí tương ứng chính xác với việc lật các bit không khớp trong mỗi nửa. Không có vấn đề riêng lẻ nào phát sinh do sự phân chia được xác định là ranh giới tiền tố [0, i) và hậu tố [i, n). 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai đầu vào để xem cách đánh giá phân tách hoạt động. 

Đầu vào đầu tiên là`001`. 

| Chia tôi | giá 000→111? | giá 111→000? | phút | 
| --- | --- | --- | --- | 
| 0 | 0 + 2 = 2 | 1 + 0 = 1 | 1 | 
| 1 | 0 + 1 = 1 | 1 + 1 = 2 | 1 | 
| 2 | 1 + 1 = 2 | 0 + 1 = 1 | 1 | 
| 3 | 2 + 0 = 2 | 0 + 0 = 0 | 0 | 

Tốt nhất là 0 ở phần chia 3 với mẫu 111→000, phù hợp mà không cần thay đổi nếu chúng ta chọn cấu trúc phù hợp. 

Dấu vết này cho thấy rằng ngay cả một chuỗi nhỏ cũng có thể thừa nhận nhiều cấu trúc tối ưu hợp lệ tùy thuộc vào vị trí chuyển đổi được đặt và thuật toán đánh giá tất cả chúng một cách có hệ thống. 

Đầu vào thứ hai là`001100`. 

| Chia tôi | giá 000→111 | giá 111→000 | phút | 
| --- | --- | --- | --- | 
| 0 | 0+4=4 | 2+0=2 | 2 | 
| 2 | 0+2=2 | 2+2=4 | 2 | 
| 4 | 2+0=2 | 4+2=6 | 2 | 
| 6 | 4+0=4 | 0+4=4 | 4 | 

Tối thiểu là 2, tương ứng với việc lật hai bit không khớp để khớp với cấu trúc đơn điệu. Điều này chứng tỏ giải pháp tối ưu không phụ thuộc vào việc nhóm các ký tự giống hệt nhau mà vào việc thực thi một ranh giới chuyển tiếp duy nhất trên toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | tính toán tiền tố cộng với quét tuyến tính qua các điểm phân chia | 
| Không gian | O(n) | mảng tiền tố lưu trữ số lượng số 0 và số 1 | 

Các ràng buộc cho phép tối đa 100 chuỗi có độ dài 1000, do đó, giải pháp O(n) trên mỗi chuỗi phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ là tối thiểu và bị chi phối bởi mảng tiền tố. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        s = input().strip()
        n = len(s)

        pref0 = [0] * (n + 1)
        pref1 = [0] * (n + 1)

        for i, ch in enumerate(s):
            pref0[i + 1] = pref0[i] + (ch == '0')
            pref1[i + 1] = pref1[i] + (ch == '1')

        ans = n
        for i in range(n + 1):
            cost01 = pref1[i] + (pref0[n] - pref0[i])
            cost10 = pref0[i] + (pref1[n] - pref1[i])
            ans = min(ans, cost01, cost10)

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("""7
001
100
101
010
0
1
001100
""") == """0
0
1
1
0
0
2"""

# custom cases
assert run("""3
0
1
01
""") == """0
0
0""", "minimum edge cases"

assert run("""1
000000""") == "0", "already uniform"

assert run("""1
010101""") == "2", "alternating worst structure"

assert run("""1
001111""") == "0", "already monotone split"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ký tự đơn | 0 | trường hợp cơ sở | 
| tất cả số không | 0 | đã tốt rồi | 
| xen kẽ | 2 | cần bình thường hóa | 
| đã đơn điệu rồi | 0 | phân chia cấu trúc đúng đắn | 

## Vỏ cạnh 

Đối với một chuỗi như`010`, thuật toán xem xét tất cả các phần tách. Khi chia i = 1, giá trị của 000→111 là 1 (lật giữa) và đối với 111→000 cũng là 1, cho câu trả lời 1. Điều này phù hợp với trực giác rằng một lần lật là đủ để loại bỏ cấu trúc xen kẽ. 

Đối với một chuỗi như`111000`, mức phân chia tối ưu nằm ở ranh giới giữa các khối, cho ra chi phí 0. Tổng tiền tố phản ánh chính xác rằng không tồn tại sự không khớp nào khi căn chỉnh theo đúng hướng, xác nhận rằng thuật toán tự nhiên phát hiện các chuỗi vốn đã tốt mà không cần xử lý đặc biệt.
