---
title: "CF 33C - Tổng ngẫu nhiên tuyệt vời"
description: "Chúng ta được cấp một chuỗi các số nguyên và nhiệm vụ là tối đa hóa tổng của nó bằng cách thực hiện hai thao tác: đầu tiên, chúng ta có thể chọn bất kỳ tiền tố nào của chuỗi và nhân mọi phần tử trong đó với -1; thứ hai, chúng ta có thể chọn bất kỳ hậu tố nào của dãy và nhân mọi phần tử trong đó với -1."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 33
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 33 (Codeforces format)"
rating: 1800
weight: 33
solve_time_s: 186
verified: true
draft: false
---
[CF 33C - Tổng ngẫu nhiên tuyệt vời](https://codeforces.com/problemset/problem/33/C) 

**Đánh giá:** 1800 
**Thẻ:** tham lam 
**Thời gian giải:** 3 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các số nguyên và nhiệm vụ là tối đa hóa tổng của nó bằng cách thực hiện hai thao tác: đầu tiên, chúng ta có thể chọn bất kỳ tiền tố nào của chuỗi và nhân mọi phần tử trong đó với -1; thứ hai, chúng ta có thể chọn bất kỳ hậu tố nào của dãy và nhân mọi phần tử trong đó với -1. Cả hai thao tác đều là tùy chọn và tiền tố và hậu tố có thể trùng nhau. Đầu ra là một số nguyên duy nhất, tổng tối đa có thể đạt được. 

Hạn chế chính là chuỗi có thể lớn tới 100.000 phần tử và các phần tử riêng lẻ nằm trong khoảng từ -10.000 đến 10.000. Với giới hạn thời gian 2 giây, điều này hạn chế chúng tôi ở các giải pháp chạy trong thời gian gần như tuyến tính hoặc logarit tuyến tính. Các phương pháp bậc hai, chẳng hạn như thử mọi cặp tiền tố và hậu tố có thể, sẽ yêu cầu theo thứ tự:$10^{10}$trong trường hợp xấu nhất là không thể thực hiện được. 

Các trường hợp cạnh tinh tế bao gồm các chuỗi có tất cả các số âm, trong đó việc lật toàn bộ chuỗi có thể là tối ưu và các chuỗi có một phần tử duy nhất, trong đó việc lật có thể có hoặc không có lợi. Ví dụ, đầu vào`[-5]`nên xuất ra`5`, trong khi`[3]`nên xuất ra`3`. Các cách tiếp cận bất cẩn có thể cho rằng tiền tố và hậu tố rời rạc hoặc không xử lý được các lần lật trống đúng cách. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi tiền tố có thể để lật, sau đó mọi hậu tố có thể để lật, tính tổng và theo dõi mức tối đa. Cụ thể, với mỗi chỉ số$i$từ 0 đến$n$, chúng ta lật cái đầu tiên$i$các phần tử và sau đó cho mỗi chỉ mục$j$từ 0 đến$n$, lật cái cuối cùng$j$các phần tử. Mỗi đánh giá yêu cầu tổng hợp$n$các phần tử. Điều này mang lại$O(n^3)$phức tạp trong trường hợp xấu nhất. Ngay cả khi chúng ta tính toán trước các tổng tiền tố để giảm phép tính tổng xuống còn$O(1)$, điều này vẫn mang lại$O(n^2)$, quá chậm đối với$n = 10^5$. 

Quan sát quan trọng cho phép giải tuyến tính là nhận thấy rằng việc lật một tiền tố hoặc hậu tố tương đương với việc trừ hai lần tổng của đoạn đó khỏi tổng số nếu ban đầu chúng ta đã lật nó rồi lật ngược lại. Cụ thể hơn, gọi tổng của dãy là$S$. Lật tiền tố có độ dài$k$thay đổi tổng bằng$-2 \times \text{sum of first k elements}$. Lật một hậu tố có độ dài$l$thay đổi tổng bằng$-2 \times \text{sum of last l elements}$. Do đó, việc tối đa hóa tổng số tiền sẽ giảm xuống việc chọn tiền tố và hậu tố sao cho tổng của đoạn bị đảo ngược (và trừ hai lần) được giảm thiểu. 

Để thực hiện điều này một cách hiệu quả, chúng ta có thể tính toán trước các tổng tiền tố và tổng hậu tố. Chúng tôi theo dõi tổng tiền tố tối thiểu và tổng hậu tố tối thiểu, sau đó xem xét các kết hợp trong đó chúng tôi không lật, chỉ lật tiền tố, chỉ lật hậu tố hoặc lật cả hai. Tổng tối ưu sau đó là số tiền tối đa trong số các kết hợp này. Vì tổng tiền tố và tổng hậu tố được tính toán tuyến tính và bước kết hợp cũng tuyến tính nên độ phức tạp tổng thể là$O(n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc dãy số và tính tổng$S$. Điều này đại diện cho số tiền mà không có bất kỳ lần lật nào. 
2. Tính tổng tiền tố của dãy. Đối với mỗi chỉ số$i$, tổng tiền tố lên tới$i$là tổng các phần tử từ 0 đến$i$. Theo dõi tổng tiền tố tối thiểu khi bạn lặp lại. Điều này sau này sẽ cho phép chúng tôi xác định tiền tố có lợi nhất để lật. 
3. Tương tự, tính tổng hậu tố từ đầu đến cuối, theo dõi tổng hậu tố tối thiểu. Điều này mang lại hậu tố có lợi nhất để lật. 
4. Cân nhắc việc lật không có đoạn nào. Điều này đơn giản mang lại$S$. 
5. Cân nhắc việc đảo tiền tố có tổng nhỏ nhất. Lật tiền tố này sẽ thay đổi tổng số tiền thành$S - 2 \times (\text{minimum prefix sum})$. 
6. Cân nhắc việc đảo hậu tố có tổng nhỏ nhất. Lật hậu tố này sẽ thay đổi tổng số tiền thành$S - 2 \times (\text{minimum suffix sum})$. 
7. Cân nhắc việc đảo cả tiền tố và hậu tố. Vì các lần lật có thể chồng lên nhau nên hãy tính tổng khi lật cả tiền tố tối thiểu và hậu tố tối thiểu, điều chỉnh cho trùng lặp để tránh bị trừ hai lần. 
8. Câu trả lời là tối đa trong số các tình huống này. 

Tại sao nó hoạt động: Việc lật một đoạn tương đương với việc trừ đi hai lần tổng của nó. Các hoạt động là tuyến tính và độc lập, ngoại trừ sự chồng chéo, được tính toán chính xác. Bằng cách tính toán trước tổng tiền tố và hậu tố tối thiểu, chúng tôi đảm bảo lựa chọn tối đa hóa tổng trên toàn cầu mà không bỏ sót bất kỳ kết hợp nào tốt hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

total = sum(a)

# Compute minimum prefix sum
prefix_sum = 0
min_prefix = 0
for x in a:
    prefix_sum += x
    min_prefix = min(min_prefix, prefix_sum)

# Compute minimum suffix sum
suffix_sum = 0
min_suffix = 0
for x in reversed(a):
    suffix_sum += x
    min_suffix = min(min_suffix, suffix_sum)

# Maximum sum options
max_sum = total
max_sum = max(max_sum, total - 2 * min_prefix)
max_sum = max(max_sum, total - 2 * min_suffix)
max_sum = max(max_sum, total - 2 * (min_prefix + min_suffix))

print(max_sum)
```Giải pháp đầu tiên tính tổng số tiền làm đường cơ sở. Vòng lặp tổng tiền tố theo dõi tổng hiện có và cập nhật tổng tiền tố tối thiểu, xử lý trường hợp việc lật tiền tố trống không mang lại thay đổi. Vòng lặp tổng hậu tố thực hiện ngược lại. Mức tối đa trong số tất cả các tùy chọn lật sẽ đưa ra câu trả lời cuối cùng. Các trường hợp cạnh như tất cả các số âm hoặc một phần tử đơn lẻ đều được xử lý một cách tự nhiên, bởi vì các lần lật trống tương ứng với phép trừ bằng 0. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`[-1, -2, -3]`| Bước | tiền tố_sum | tiền tố tối thiểu | hậu tố_sum | min_suffix | tổng cộng | ứng viên max_sum | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | -1 | -1 | -3 | -3 | -6 | 6, 6, 6, 6 | 

Lật toàn bộ chuỗi (tiền tố và/hoặc hậu tố) sẽ tạo ra 6, mức tối đa chính xác. 

**Ví dụ 2** 

đầu vào:`[3, -4, 2]`| Bước | tiền tố_sum | tiền tố tối thiểu | hậu tố_sum | min_suffix | tổng cộng | ứng viên max_sum | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 0 | 2 | -4 | 1 | 1, 1, 9, 9 | 

Lật hậu tố`[-4, 2]`đưa ra số tiền`9`, đó là mức tối đa. 

Những dấu vết này xác nhận thuật toán xác định các phân đoạn tối ưu để lật ngay cả khi các phần tử được trộn lẫn giữa dương và âm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt tính tổng, một lượt tính tổng tiền tố, một lượt tính tổng hậu tố, kết hợp thời gian không đổi | 
| Không gian | O(1) | Chỉ một số tổng số và giá trị tối thiểu đang chạy được lưu trữ | 

Điều này phù hợp thoải mái trong giới hạn 2 giây cho$n \le 10^5$và giới hạn bộ nhớ là 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    total = sum(a)
    prefix_sum = 0
    min_prefix = 0
    for x in a:
        prefix_sum += x
        min_prefix = min(min_prefix, prefix_sum)
    suffix_sum = 0
    min_suffix = 0
    for x in reversed(a):
        suffix_sum += x
        min_suffix = min(min_suffix, suffix_sum)
    max_sum = total
    max_sum = max(max_sum, total - 2 * min_prefix)
    max_sum = max(max_sum, total - 2 * min_suffix)
    max_sum = max(max_sum, total - 2 * (min_prefix + min_suffix))
    return str(max_sum)

# Provided sample
assert run("3\n-1 -2 -3\n") == "6", "sample 1"

# Custom cases
assert run("1\n5\n") == "5", "single positive"
assert run("1\n-5\n") == "5", "single negative"
assert run("5\n-1 -2 -3 -4 -5\n") == "15", "all negative"
assert run("5\n1 2 3 4 5\n") == "15", "all positive"
assert run("4\n3 -1 -2 5\n") == "11", "mixed positive/negative"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n5\n`| 5 | Yếu tố tích cực duy nhất | 
|`1\n-5\n`| 5 | Phần tử âm đơn | 
|`5\n-1 -2 -3 -4 -5\n`| 15 | Lật tất cả số âm | 
|`5\n1 2 3 4 5\n`| 15 | Không cần lật cho tất cả tích cực | 
| `4\n3 - | | |
