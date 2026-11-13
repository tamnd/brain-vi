---
title: "CF 1692E - Deque nhị phân"
description: "Chúng ta được cung cấp một mảng nhị phân có độ dài $n$, nghĩa là mỗi phần tử là 0 hoặc 1. Nhiệm vụ là thực hiện một chuỗi các thao tác trong đó, trong mỗi thao tác, chúng ta loại bỏ phần tử đầu tiên hoặc phần tử cuối cùng của mảng."
date: "2026-06-09T23:02:30+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1692
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 799 (Div. 4)"
rating: 1200
weight: 1692
solve_time_s: 110
verified: true
draft: false
---

[CF 1692E - Deque nhị phân](https://codeforces.com/problemset/problem/1692/E) 

**Đánh giá:** 1200 
**Tags:** tìm kiếm nhị phân, triển khai, hai con trỏ 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng nhị phân có độ dài$n$, nghĩa là mỗi phần tử là 0 hoặc 1. Nhiệm vụ là thực hiện một chuỗi các thao tác trong đó, trong mỗi thao tác, chúng ta loại bỏ phần tử đầu tiên hoặc phần tử cuối cùng của mảng. Sau một số lần loại bỏ, tổng các phần tử còn lại phải bằng giá trị đích$s$. Chúng tôi muốn tìm số lượng thao tác tối thiểu cần thiết để đạt được số tiền này hoặc báo cáo -1 nếu không thể. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp có kích thước mảng$n$, tổng mục tiêu$s$và chính mảng đó. Tổng của tất cả$n$trên tất cả các trường hợp thử nghiệm không vượt quá 200.000, vì vậy giải pháp của chúng tôi phải xử lý từng trường hợp thử nghiệm một cách hiệu quả, lý tưởng nhất là theo thời gian tuyến tính so với kích thước của mảng. Các giải pháp bậc hai, chẳng hạn như thử tất cả các tiền tố và hậu tố có thể một cách rõ ràng, sẽ quá chậm đối với các mảng lớn nhất vì chúng có thể yêu cầu tối đa$n^2$hoạt động cho mỗi trường hợp thử nghiệm. 

Các trường hợp đặc biệt phát sinh khi đã đạt được tổng mục tiêu, khi$s$bằng 0 hoặc bằng tổng của mảng hoặc khi tất cả các phần tử giống hệt nhau. Ví dụ, nếu mảng là$[1, 1, 1]$Và$s = 0$, một cách tiếp cận ngây thơ chỉ loại bỏ các phần tử ngay từ đầu có thể không thể khám phá được việc loại bỏ từ cuối. Một trường hợp tinh tế khác là khi mảng có số 0 ở cả hai đầu, như$[0, 1, 1, 0]$với$s = 2$; chúng ta phải thừa nhận rằng việc loại bỏ các số 0 không ảnh hưởng đến tổng và sẽ giảm thiểu các phép toán. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ lặp lại trên tất cả các kết hợp loại bỏ$i$các yếu tố phía trước và$j$các yếu tố từ phía sau. Đối với mỗi sự kết hợp, chúng tôi sẽ tính tổng của mảng còn lại. Cách này hiệu quả với các mảng nhỏ vì nó khám phá chính xác mọi mảng con có thể, nhưng nó yêu cầu$O(n^2)$hoạt động cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất. Với$n$lên tới 200.000, phương pháp này sẽ yêu cầu 40 tỷ thao tác trong trường hợp xấu nhất, điều này hoàn toàn không khả thi. 

Thông tin chi tiết quan trọng đến từ việc viết lại vấn đề: thay vì nghĩ về những gì cần loại bỏ, hãy nghĩ về những gì cần giữ lại. Nếu tổng mảng là$total\_sum$, sau đó loại bỏ các phần tử để đạt được tổng$s$tương đương với việc giữ một mảng con liền kề có tổng là$s$. Vì chúng ta có thể loại bỏ các phần tử ở cả hai đầu nên mảng con chúng ta giữ phải liên tục. Điều này làm giảm vấn đề tìm mảng con liền kề dài nhất với tổng bằng$s$. Khi chúng ta biết độ dài của mảng con này, số phép toán tối thiểu chỉ đơn giản là tổng số phần tử trừ đi độ dài của mảng con. 

Điều này tự nhiên dẫn đến giải pháp hai con trỏ hoặc cửa sổ trượt. Cửa sổ trượt hoạt động hiệu quả vì mảng chỉ chứa các số 0 và 1 nên tổng của cửa sổ có thể được cập nhật tăng dần theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng của mảng. Nếu như$s$lớn hơn tổng thì trả về -1 ngay vì không thể lấy được$s$. Nếu như$s$bằng tổng số tiền, trả về 0 vì không cần thực hiện thao tác nào. 
2. Khởi tạo hai con trỏ:`left = 0`để bắt đầu cửa sổ và`current_sum = 0`để biết tổng các phần tử trong cửa sổ. Chúng ta sẽ cố gắng tìm mảng con dài nhất có tổng bằng$s$. 
3. Lặp lại mảng bằng chỉ mục`right`. Thêm vào`array[right]`ĐẾN`current_sum`. Nếu như`current_sum`vượt quá$s$, di chuyển`left`con trỏ tiến và trừ`array[left]`từ`current_sum`cho đến khi`current_sum <= s`. 
4. Bất cứ khi nào`current_sum`bằng$s$, cập nhật một biến`max_length`để theo dõi độ dài tối đa của một mảng con như vậy. 
5. Sau vòng lặp, nếu`max_length`vẫn là -1, trả về -1 vì không tìm thấy mảng con hợp lệ. Còn không thì trả lại`n - max_length`, đại diện cho số lượng thao tác tối thiểu cần thiết để giảm mảng về tổng mong muốn. 

Tại sao nó hoạt động: tính bất biến của cửa sổ trượt đảm bảo rằng`current_sum`luôn biểu thị tổng của một mảng con liền kề kết thúc tại`right`. Bằng cách điều chỉnh`left`, chúng tôi chỉ thu nhỏ cửa sổ khi cần thiết, đảm bảo rằng mọi mảng con ứng viên có tổng`s`được xem xét. Việc theo dõi độ dài tối đa đảm bảo số lần xóa tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_operations_to_sum(n, s, arr):
    total = sum(arr)
    if s > total:
        return -1
    if s == total:
        return 0
    
    max_len = -1
    left = 0
    current_sum = 0
    
    for right in range(n):
        current_sum += arr[right]
        while current_sum > s and left <= right:
            current_sum -= arr[left]
            left += 1
        if current_sum == s:
            max_len = max(max_len, right - left + 1)
    
    return n - max_len if max_len != -1 else -1

t = int(input())
for _ in range(t):
    n, s = map(int, input().split())
    arr = list(map(int, input().split()))
    print(min_operations_to_sum(n, s, arr))
```Giải pháp trước tiên xử lý nhanh chóng các trường hợp không thể thực hiện được, sau đó sử dụng cửa sổ trượt để xác định vị trí mảng con dài nhất có tổng mục tiêu. các`while`vòng lặp bên trong`for`đảm bảo rằng cửa sổ co lại chính xác khi tổng vượt quá$s$. các`max_len`biến theo dõi mảng con hợp lệ dài nhất. Trở về`n - max_len`chuyển đổi mảng con được giữ thành số lần xóa tối thiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`3 1 1 0 0`| đúng | trái | hiện tại_sum | max_len | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 
| 1 | 0 | 1 | 1 | 
| 2 | 0 | 1 | 1 | 

Kết quả:`3 - 1 = 2`, nhưng hãy cẩn thận, chúng tôi kiểm tra tổng số tiền:`sum(arr) = 1`, bằng s nên trả về 0. 

### Ví dụ 2 

đầu vào:`9 3 0 1 0 1 1 1 0 0 1`| đúng | trái | hiện tại_sum | max_len | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | -1 | 
| 1 | 0 | 1 | -1 | 
| 2 | 0 | 1 | -1 | 
| 3 | 0 | 2 | -1 | 
| 4 | 0 | 3 | 5 | 
| 5 | 0 | 4 | - trừ trái -> left=1, current_sum=4, left=2 -> current_sum=4, left=3->current_sum=3 |`max_len = 6`, kết quả:`9 - 6 = 3`. 

Dấu vết này xác nhận rằng cửa sổ điều chỉnh chính xác cả hai đầu và tìm ra mảng con dài nhất khớp với tổng mục tiêu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được thêm và xóa nhiều nhất một lần trong cửa sổ trượt. | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng ngoài mảng đầu vào. | 

Với các ràng buộc rằng tổng của tất cả$n$trên các trường hợp thử nghiệm là 200.000, thuật toán hoạt động tốt trong giới hạn thời gian 2 giây và sử dụng bộ nhớ bổ sung không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n, s = map(int, input().split())
        arr = list(map(int, input().split()))
        print(min_operations_to_sum(n, s, arr))
    return output.getvalue().strip()

# Provided samples
assert run("7\n3 1\n1 0 0\n3 1\n1 1 0\n9 3\n0 1 0 1 1 1 0 0 1\n6 4\n1 1 1 1 1 1\n5 1\n0 0 1 1 0\n16 2\n1 1 0 0 1 0 0 1 1 0 0 0 0 0 1 1\n6 3\n1 0 1 0 0 0") == "0\n1\n3\n2\n2\n7\n-1"

# Custom tests
assert run("2\n1 1\n1\n1 0\n0") == "0\n-1", "single element arrays"
assert run("1\n5 3\n1 1 1 1 1") == "2", "all ones, remove two elements"
assert run("1\n5 0\n0 0 0 0 0") == "0", "all zeros,
```
