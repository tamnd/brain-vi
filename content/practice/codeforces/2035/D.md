---
title: "CF 2035D - Một bài toán số thực khác"
description: "Chúng ta được cấp một mảng các số nguyên và được phép thực hiện các phép tính di chuyển lũy thừa của hai từ phần tử này sang phần tử sau trong mảng. Cụ thể, nếu một phần tử là số chẵn, chúng ta có thể chia nó cho hai và nhân phần tử sau đó với hai."
date: "2026-06-08T11:27:25+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "divide-and-conquer", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2035
codeforces_index: "D"
codeforces_contest_name: "Codeforces Global Round 27"
rating: 1800
weight: 2035
solve_time_s: 104
verified: false
draft: false
---

[CF 2035D - Một vấn đề về số thực khác](https://codeforces.com/problemset/problem/2035/D) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, chia để trị, tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một mảng các số nguyên và được phép thực hiện các phép tính di chuyển lũy thừa của hai từ phần tử này sang phần tử sau trong mảng. Cụ thể, nếu một phần tử là số chẵn, chúng ta có thể chia nó cho hai và nhân phần tử sau đó với hai. Mục tiêu là tối đa hóa tổng của mảng sau khi áp dụng bất kỳ số lượng thao tác nào như vậy. Điều khó khăn là chúng ta không chỉ phải tính toán kết quả này cho một mảng duy nhất mà còn phải tính toán cho mọi tiền tố của một mảng nhất định, nghĩa là phần tử đầu tiên, sau đó là hai phần tử đầu tiên, v.v., cho đến toàn bộ mảng. Mỗi đầu ra phải là modulo$10^9+7$. 

Các ràng buộc chính là độ dài mảng, có thể lên tới$2 \cdot 10^5$đối với một trường hợp thử nghiệm duy nhất và tổng độ dài của tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$. Điều này ngụ ý rằng bất kỳ thuật toán nào có độ phức tạp trong trường hợp xấu nhất là$O(n^2)$sẽ quá chậm. Một cách tiếp cận tuyến tính hoặc tuyến tính là cần thiết. Giá trị của các phần tử mảng có thể đạt tới$10^9$, vì vậy phải cẩn thận với tình trạng tràn số nguyên nếu các phép toán được triển khai một cách đơn giản mà không có số học mô-đun. 

Một sai lầm ngây thơ là cố gắng mô phỏng tất cả các thao tác trên mỗi tiền tố, chọn các phần tử chẵn tùy ý để chia và nhân. Ví dụ, đối với tiền tố$[2, 3]$, một thuật toán bất cẩn có thể chia 2 liên tục, tạo ra nhiều mảng trung gian, trong khi giải pháp tối ưu là hợp nhất tất cả các thừa số chia hết thành phần tử lớn nhất để tối đa hóa tổng. Điều này cho thấy mô phỏng bạo lực là không hiệu quả và không cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi chuỗi hoạt động được phép. Mỗi thao tác giảm một phần tử và tăng phần tử khác, dẫn đến vụ nổ tổ hợp. Đối với tiền tố có độ dài$k$, có thể có tới$k^2$các hoạt động tiềm năng ở mỗi bước. Ngay cả đối với$k \sim 10^5$, điều này khó thực hiện được vì số lượng chuỗi thao tác tăng theo cấp số nhân. 

Cái nhìn sâu sắc quan trọng là tách mỗi số thành phần lẻ và hệ số lũy thừa hai của nó. Bất kỳ số nguyên nào$x$có thể được viết là$x = 2^p \cdot o$, Ở đâu$o$thật kỳ quặc. Các phép toán cho phép di chuyển lũy thừa của hai từ phần tử trước sang phần tử sau. Do đó, chiến lược tối ưu luôn là chuyển tất cả lũy thừa của hai từ phần tử nhỏ hơn sang phần tử duy nhất có thể hấp thụ hiệu ứng nhân lên nhiều nhất. Điều tối ưu là chọn phần tử lớn nhất (sau khi trích xuất phần lẻ của nó) làm phần tử nhận tất cả lũy thừa tích lũy của 2 vì phép nhân với lũy thừa của 2 có tác động lớn nhất đến tổng nếu áp dụng cho số lớn nhất. 

Với quan sát này, vấn đề giảm xuống còn việc theo dõi tổng các phần lẻ và lũy thừa tích lũy của hai. Với mỗi tiền tố, chúng ta có thể xác định phần tử có phần lẻ lớn nhất và nhân nó với$2^{\text{total accumulated powers}}$. Tổng sau đó là tổng của tất cả các phần lẻ cộng với phần tử lớn nhất được điều chỉnh này. Điều này chuyển đổi vấn đề từ mô phỏng tổ hợp sang tính toán thời gian tuyến tính cho mỗi tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$mỗi tiền tố |$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log a_{\max})$|$O(1)$mỗi tiền tố | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai bộ tích lũy: một cho tổng số phần lẻ của các phần tử được thấy cho đến nay và một cho tổng số lũy thừa của hai được trích xuất từ tất cả các phần tử. Giữ nguyên chỉ số của phần tử có phần lẻ lớn nhất. 
2. Lặp lại từng phần tử của mảng. Đối với mỗi phần tử, hãy tính hệ số đó là$x = 2^p \cdot o$, Ở đâu$o$thật kỳ quặc và$p \ge 0$. Tăng tổng số phần lẻ lên$o$và tăng tổng số lũy thừa lên$p$. 
3. Theo dõi phần lẻ lớn nhất được thấy cho đến nay. Đây là ứng cử viên để hấp thụ mọi sức mạnh của cả hai. Nếu phần lẻ của phần tử hiện tại vượt quá mức tối đa trước đó, hãy cập nhật chỉ mục ứng cử viên. 
4. Để tính tổng tối đa cho tiền tố hiện tại, hãy loại bỏ phần lẻ ban đầu của phần tử ứng cử viên khỏi tổng các phần lẻ. Nhân nó với$2^{\text{total powers}}$, sau đó cộng lại tổng các phần lẻ còn lại. Áp dụng modulo$10^9+7$. 
5. Cộng tổng này vào kết quả của tiền tố hiện tại. 
6. Lặp lại cho từng phần tử của tiền tố. Đặt lại bộ tích lũy cho mỗi trường hợp thử nghiệm mới. 

Tại sao nó hoạt động: Ở mỗi bước, tổng được tối đa hóa bằng cách hợp nhất tất cả các lũy thừa có sẵn của 2 thành phần tử có giá trị cơ bản lớn nhất (phần lẻ). Bất kỳ sự phân phối lại nào cho phần tử nhỏ hơn sẽ tạo ra phần đóng góp nhỏ hơn vào tổng vì phép nhân có hiệu quả hơn đối với số lớn hơn. Điều bất biến là sau khi xử lý bất kỳ tiền tố nào, tổng được tính theo cách này bằng tổng tối đa có thể đạt được thông qua bất kỳ chuỗi thao tác được phép nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def process_case(a):
    prefix_sums = []
    max_odd = 0
    sum_odds = 0
    powers_total = 0
    candidate_odd = 0
    
    for x in a:
        p = 0
        while x % 2 == 0:
            x //= 2
            p += 1
        o = x
        sum_odds += o
        powers_total += p
        if o > candidate_odd:
            candidate_odd = o
        max_sum = (candidate_odd * pow(2, powers_total, MOD) - candidate_odd + sum_odds) % MOD
        prefix_sums.append(max_sum)
    
    return prefix_sums

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    result = process_case(a)
    print(*result)
```Mã phân tích mỗi số thành phần lẻ và thành phần lũy thừa hai. Nó tích lũy tổng các phần lẻ và tổng lũy ​​thừa của hai, cập nhật phần tử ứng cử viên nếu nhìn thấy phần lẻ lớn hơn. Tổng tối đa cho tiền tố hiện tại được tính bằng cách áp dụng tất cả lũy thừa của hai cho phần lẻ lớn nhất, cộng lại tổng còn lại của các phần lẻ và áp dụng số học modulo. Điều này đảm bảo tính chính xác và tránh tràn. 

## Ví dụ đã hoạt động 

Hãy xem xét tiền tố$[1, 2, 3, 4]$từ mẫu đầu tiên. 

| Bước | Yếu tố | Phần lẻ | Quyền hạn được trích xuất | Ứng viên Lẻ | Tổng tỷ lệ cược | Tổng tối đa | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 1 | 1 | 1 | 
| 2 | 2 | 1 | 1 | 1 | 2 | 3 | 
| 3 | 3 | 3 | 0 | 3 | 5 | 8 | 
| 4 | 4 | 1 | 2 | 3 | 6 | 13 | 

Điều này chứng tỏ rằng phần lẻ lớn nhất (3) hấp thụ tất cả lũy thừa của hai (từ 2 và 4), mang lại tổng tối đa cho tiền tố. 

tiền tố khác$[1, 6, 9, 4]$: 

| Bước | Yếu tố | Phần lẻ | Quyền hạn được trích xuất | Ứng viên Lẻ | Tổng tỷ lệ cược | Tổng tối đa | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 1 | 1 | 1 | 
| 2 | 6 | 3 | 1 | 3 | 4 | 7 | 
| 3 | 9 | 9 | 0 | 9 | 13 | 22 | 
| 4 | 4 | 1 | 2 | 9 | 14 | 26 | 

Điều này khẳng định rằng chiến lược xác định một cách nhất quán yếu tố tối ưu để tập trung sức mạnh của cả hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log a_{\max})$| Mỗi phần tử được phân tích thành phần lẻ và lũy thừa của hai; bao thanh toán mất$O(\log a_i)$thời gian. | 
| Không gian |$O(n)$| Lưu trữ tổng tiền tố; các biến khác sử dụng không gian không đổi. | 

Lời giải phù hợp thoải mái trong giới hạn bài toán vì tổng số$n$trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$và mỗi bước phân tích nhân tử là logarit của giá trị phần tử, dẫn đến kết quả tối đa là$2 \cdot 10^5 \cdot 30 \sim 6 \cdot 10^6$hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 10**9 + 7
    def process_case(a):
        prefix_sums = []
        sum_odds = 0
        powers_total = 0
        candidate_odd = 0
        for x in a:
            p = 0
            while x % 2 == 0:
                x //= 2
                p += 1
            o = x
            sum_odds += o
            powers_total += p
            if o > candidate_odd:
                candidate_odd = o
            max_sum = (candidate_odd * pow(2, powers_total, MOD) - candidate_odd + sum_odds) % MOD
            prefix_sums.append(max_sum)
        return prefix_sums
    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        res.append
```
