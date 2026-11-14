---
title: "CF 1789B - Phép thuật Phục vụ và Đảo ngược"
description: "Chúng ta được cấp một chuỗi nhị phân chỉ gồm 0 và 1. Nhiệm vụ là xác định xem liệu chúng ta có thể biến nó thành một bảng màu bằng cách lật chính xác một đoạn liền kề của chuỗi hay không. Lật một đoạn có nghĩa là thay đổi mọi số 0 trong đoạn đó thành 1 và mọi số 1 thành 0."
date: "2026-06-09T10:42:03+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "strings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1789
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 853 (Div. 2)"
rating: 800
weight: 1789
solve_time_s: 90
verified: true
draft: false
---

[CF 1789B - Phép thuật phục vụ và đảo ngược](https://codeforces.com/problemset/problem/1789/B) 

**Đánh giá:** 800 
**Tags:** vũ lực, triển khai, chuỗi, hai con trỏ 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi nhị phân chỉ gồm 0 và 1. Nhiệm vụ là xác định xem liệu chúng ta có thể biến nó thành một bảng màu bằng cách lật chính xác một đoạn liền kề của chuỗi hay không. Lật một đoạn có nghĩa là thay đổi mọi số 0 trong đoạn đó thành 1 và mọi số 1 thành 0. 

Ví dụ, đối với một chuỗi`10010`, lật ba ký tự đầu tiên sẽ tạo ra`01110`, đó là một palindrome. Độ dài chuỗi có thể lên tới 100.000 và có thể có tới 10.000 trường hợp thử nghiệm. Vì tổng độ dài không vượt quá 200.000 nên chúng tôi cần một giải pháp chạy tuyến tính theo độ dài chuỗi trên mỗi trường hợp thử nghiệm. Các giải pháp bậc hai thử tất cả các phân đoạn có thể sẽ dẫn đến khoảng 10^10 phép tính, điều này là không khả thi. 

Phần khó khăn là chúng ta được phép **chính xác một lần lật**, không phải nhiều lần. Điều này giới thiệu một số trường hợp cạnh. Nếu chuỗi đã là một chuỗi palindrome, chúng ta không thể làm gì cả - chúng ta vẫn phải lật một đoạn, có thể là đoạn nhỏ nhất. Ví dụ,`11`là một bảng màu, nhưng việc lật một trong hai ký tự sẽ tạo ra`01`hoặc`10`, cả hai đều không phải là palindrome. Điều này nhấn mạnh tầm quan trọng của việc kiểm tra khả năng lật một số đoạn để đạt được bảng màu, chứ không chỉ xem chuỗi hiện tại có phải là một bảng màu hay không. 

Một kịch bản tinh tế khác xảy ra với các chuỗi có độ dài ba, chẳng hạn như`010`. Nếu chúng ta lật ký tự ở giữa, nó vẫn là một bảng màu. Nếu chúng ta lật hai phần bên ngoài thì nó vẫn là một bảng màu. Những trường hợp nhỏ này minh họa rằng việc lựa chọn phân đoạn chính xác là rất quan trọng và chúng ta phải tính đến sự trùng lặp giữa các điểm không khớp bên trái và bên phải trong chuỗi. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp lại trên tất cả các phân đoạn có thể`[l, r]`và lật chúng để xem chuỗi kết quả có phải là một bảng màu hay không. Số lượng phân đoạn là O(n^2) và việc kiểm tra bảng màu có giá O(n). Điều này mang lại độ phức tạp trong trường hợp xấu nhất là O(n^3), quá chậm đối với`n`lên đến 10^5. 

Thông tin chi tiết quan trọng là tập trung vào **các vị trí không khớp**. Để một chuỗi trở thành một palindrome, mỗi ký tự ở vị trí chỉ mục`i`phải khớp với ký tự ở chỉ mục`n-i-1`. Nếu chúng đã khớp rồi thì chúng ta có thể bỏ qua chúng. Nếu chúng khác nhau, việc lật một đoạn phải bao gồm một trong các ký tự chứ không phải ký tự kia, đảo ngược một cách hiệu quả để khớp với ký tự kia. 

Bằng cách đếm số cặp không khớp, chúng ta có thể suy luận: 

- Nếu có **0 điểm không khớp** thì chuỗi đó đã là một chuỗi palindrome. Việc lật bất kỳ đoạn nào có độ dài một sẽ tạo ra chính xác một cặp không khớp, nhưng việc lật một ký tự trong một chuỗi có độ dài chẵn vẫn có thể tạo ra một bảng màu hợp lệ. Trong thực tế, việc kiểm tra độ dài và liệu độ dài chuỗi có cho phép lật đoạn hay không là đủ. 
- Nếu có **chính xác 1 cặp không khớp**, việc lật một đoạn bao gồm một trong các ký tự không khớp sẽ khắc phục được. Điều này có thể thực hiện được nếu phân đoạn bao gồm một trong các cặp và tùy chọn nhiều ký tự hơn mà không tạo thêm các ký tự không khớp ở nơi khác. 
- Nếu có **nhiều hơn 1 cặp không khớp**, chúng ta cần kiểm tra xem các cặp không khớp này có liên tiếp hay có thể được che phủ bằng một lần lật liền kề nhau. Một mô hình không phù hợp như`0110`có sự không khớp ở vị trí 0-3 và 1-2; một cú lật giữa hai cái sẽ sửa được nó. Nếu các điểm không khớp không liền kề trong một mẫu không thể được bao phủ bởi một phân đoạn thì điều đó là không thể. 

Một cách đơn giản hơn để thực hiện là lặp lại với hai con trỏ từ trái và phải về phía giữa. Đếm các vị trí ở đó`s[left] == s[right]`so với`s[left] != s[right]`. Nếu số lượng không khớp bằng 0, chúng ta có thể lật bất kỳ ký tự đơn nào. Nếu số đếm là một hoặc các phần không khớp tạo thành một khối liền kề thì câu trả lời là có. Nếu không thì câu trả lời là không. Điều này làm giảm vấn đề xuống còn quét tuyến tính, O(n) cho mỗi trường hợp thử nghiệm, điều này có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Đếm không khớp hai con trỏ | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai con trỏ:`l = 0`Và`r = n - 1`. 
2. Di chuyển con trỏ về phía giữa. Đối với mỗi cặp`(s[l], s[r])`, kiểm tra xem chúng có bằng nhau không. Nếu bằng nhau thì không cần thực hiện hành động nào, hãy di chuyển`l`chuyển tiếp và`r`lạc hậu. 
3. Nếu chúng không bằng nhau, hãy đánh dấu đây là không khớp. Lưu trữ vị trí không khớp đầu tiên và cuối cùng. Tăng`l`và giảm dần`r`. 
4. Sau khi quét, đếm tổng số điểm không khớp. Nếu không có điểm nào không khớp thì chuỗi đó đã là một chuỗi palindrome. Lật bất kỳ ký tự đơn nào (hoặc ký tự ở giữa nếu độ dài lẻ) sẽ duy trì cấu trúc palindrome. Trả về "Có". 
5. Nếu tồn tại sự không khớp, hãy kiểm tra xem tất cả các chỉ số không khớp có tạo thành một khối liền kề hay không. Điều này có thể được xác định bằng cách xác minh rằng khoảng cách giữa điểm không khớp đầu tiên và điểm không khớp cuối cùng bằng số lượng điểm không khớp. Nếu vậy, một lần lật bao phủ đoạn này sẽ giải quyết được tất cả các điểm không khớp. Trả về "Có". 
6. Nếu các phần không khớp không liền kề nhau thì không một phân đoạn nào có thể sửa được chuỗi. Trả về "Không". 

Lý do điều này có hiệu quả là vì mỗi vị trí không khớp yêu cầu chính xác một lần đảo ngược ở một trong hai vị trí. Một lần lật liền kề có thể đảo ngược nhiều vị trí cùng một lúc. Nếu tất cả các cặp không khớp đều nằm trong một đoạn liền kề, việc lật đoạn đó sẽ căn chỉnh đồng thời tất cả các cặp không khớp. Sự không khớp không liền kề yêu cầu nhiều lần lật riêng biệt, vi phạm ràng buộc của chính xác một thao tác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        
        l, r = 0, n - 1
        mismatches = []
        while l < r:
            if s[l] != s[r]:
                mismatches.append(l)
            l += 1
            r -= 1
        
        if not mismatches:
            print("Yes")
            continue
        
        first, last = mismatches[0], mismatches[-1]
        if last - first + 1 == len(mismatches):
            print("Yes")
        else:
            print("No")

if __name__ == "__main__":
    solve()
```Mã khởi tạo con trỏ và thu thập các chỉ số không khớp. Nếu các chỉ số không khớp liền kề nhau, một lần lật sẽ khắc phục mọi khác biệt. Nếu không có sự không khớp nào, việc lật bất kỳ đoạn tối thiểu nào sẽ giữ cho chuỗi trở thành một bảng màu. Các điều kiện biên, như các chuỗi có độ dài lẻ, được xử lý một cách tự nhiên bằng phép lặp con trỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`1001`| tôi | r | s[l] | s[r] | danh sách không khớp | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 1 | 1 | [] | 
| 1 | 2 | 0 | 0 | [] | 

Không có sự không khớp, chúng ta có thể lật toàn bộ chuỗi để có được`0110`, đó là một palindrome. Đầu ra: Có. 

### Ví dụ 2 

đầu vào:`10010`| tôi | r | s[l] | s[r] | danh sách không khớp | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | 1 | 0 | [0] | 
| 1 | 3 | 0 | 1 | [0,1] | 

Sự không khớp là các vị trí liền kề 0 và 1. Đoạn đảo ngược`[0,1]`cho`01110`, một bảng màu. Đầu ra: Có. 

### Ví dụ 3 

đầu vào:`0111011`| tôi | r | s[l] | s[r] | danh sách không khớp | 
| --- | --- | --- | --- | --- | 
| 0 | 6 | 0 | 1 | [0] | 
| 1 | 5 | 1 | 1 | [0] | 
| 2 | 4 | 1 | 0 | [0,2] | 

Các điểm không khớp không liền kề nhau (vị trí 0 và 2). Không một cú lật nào có thể khắc phục được mọi sự không khớp. Đầu ra: Không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) trên mỗi trường hợp thử nghiệm, O(tổng n) tổng thể | Chúng tôi quét chuỗi một lần bằng hai con trỏ. Tổng của n 2⋅10^5. | 
| Không gian | O(n) trong trường hợp xấu nhất đối với các chỉ số không khớp | Lưu trữ tối đa n/2 thông tin không khớp. Có thể được tối ưu hóa thành O(1) chỉ với sự không khớp đầu tiên và cuối cùng. | 

Thuật toán phù hợp thoải mái trong giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("3\n4\n1001\n5\n10010\n7\n0111011\n") == "Yes\nYes\nNo", "samples"

# minimum-size inputs
assert run("2\n2\n01\n2\n11\n") == "Yes\nYes", "2-char strings"

# all-equal values
assert run("2\n5\n00000\n5\n11111\n") == "Yes\nYes", "all same"

# single mismatch in middle
assert run("1\n3\n010\n") == "Yes", "odd-length single
```
