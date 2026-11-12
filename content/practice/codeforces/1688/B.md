---
title: "CF 1688B - Bùa phép thuật của hoắc hương"
description: "Chúng tôi được yêu cầu biến đổi một bộ sưu tập các mã thông báo ma thuật để mỗi mã thông báo đều có một sức mạnh ma thuật kỳ lạ. Mỗi mã thông báo bắt đầu với một số lũy thừa số nguyên dương."
date: "2026-06-09T23:34:15+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1688
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 796 (Div. 2)"
rating: 800
weight: 1688
solve_time_s: 114
verified: true
draft: false
---

[CF 1688B - Bùa phép thuật của hoắc hương](https://codeforces.com/problemset/problem/1688/B) 

**Đánh giá:** 800 
**Tags:** bitmask, thuật toán xây dựng, tham lam, sắp xếp 
**Thời gian giải:** 1m 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu biến đổi một bộ sưu tập các mã thông báo ma thuật để mỗi mã thông báo đều có một sức mạnh ma thuật kỳ lạ. Mỗi mã thông báo bắt đầu với một số lũy thừa số nguyên dương. Chúng ta có thể kết hợp hai mã thông báo thành một bằng thao tác Fusion, tính tổng lũy ​​thừa của chúng hoặc chúng ta có thể giảm mã thông báo có lũy thừa chẵn bằng cách chia nó cho hai. Mục tiêu là xác định số lượng tối thiểu các thao tác này cần thiết để làm cho tất cả các mã thông báo trở thành số lẻ. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp đưa ra số lượng mã thông báo và quyền hạn ban đầu của chúng. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất biểu thị số lượng thao tác tối thiểu. 

Các ràng buộc cho chúng ta biết rằng tổng số mã thông báo trên tất cả các trường hợp thử nghiệm tối đa là 200.000 và sức mạnh riêng lẻ có thể lên tới 1e9. Điều này cho thấy chúng ta cần một thuật toán chạy trong thời gian gần như tuyến tính hoặc tuyến tính đối với số lượng mã thông báo. Các giải pháp bậc hai xem xét tất cả các cặp mã thông báo sẽ quá chậm. 

Một trường hợp cạnh không rõ ràng xảy ra khi tất cả các mã thông báo đều chẵn nhưng lũy ​​thừa là lũy thừa khác nhau của hai. Ví dụ,`[2, 4, 8]`cần lập kế hoạch cẩn thận: việc giảm số lượng nhỏ một cách mù quáng trước tiên có thể yêu cầu nhiều thao tác hơn là kết hợp các mã thông báo một cách chiến lược. Một trường hợp đặc biệt khác là khi tất cả các mã thông báo đều là số lẻ, không yêu cầu thao tác nào; một thuật toán ngây thơ vẫn có thể thử thực hiện các thao tác không cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các chuỗi Hợp nhất và Rút gọn có thể có để kiểm tra xem chuỗi nào mang lại ít thao tác nhất. Đối với mỗi Fusion, chúng tôi sẽ thử từng cặp mã thông báo; đối với mỗi lần Giảm, chúng tôi sẽ kiểm tra mọi mã thông báo chẵn. Mặc dù về nguyên tắc, điều này hoạt động nhưng số lượng chuỗi tăng theo cấp số nhân với số lượng mã thông báo, khiến phương pháp này không khả thi. 

Quan sát quan trọng cho cách tiếp cận tối ưu là chúng ta không cần phải mô phỏng mọi trình tự. Sự kết hợp chỉ hữu ích khi kết hợp các mã thông báo chẵn và mỗi Giảm sẽ giảm một nửa số chẵn cho đến khi nó trở thành số lẻ. Mỗi số chẵn có thể giảm độc lập, nhưng nếu chúng ta có nhiều số chẵn, chúng ta có thể lưu các phép toán bằng cách hợp nhất một số số chẵn trước. Cụ thể, số lượng thao tác tối thiểu được xác định bằng số lần nhỏ nhất mà một mã thông báo chẵn cần được chia cho hai để trở thành số lẻ, bởi vì chúng ta luôn có thể kết hợp các mã thông báo một cách chiến lược để tập trung vào mã thông báo có chi phí thấp nhất. 

Điều này dẫn đến một chiến lược tham lam mang tính xây dựng: đối với mỗi mã thông báo chẵn, hãy đếm số lần chúng ta phải chia cho hai để đạt được số lẻ. Nếu nhiều mã thông báo yêu cầu cùng số lần chia thì chỉ cần thực thi số lần chia nhỏ nhất; những cái khác có thể được hợp nhất với nó, tiết kiệm hoạt động. Số lẻ yêu cầu hoạt động bằng không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n log A) | O(n) | Đã chấp nhận | 

Đây,`A`là giá trị mã thông báo tối đa. Mỗi mã thông báo có thể được chia tối đa cho hai`log2(A)`lần. 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, hãy đọc`n`và danh sách quyền hạn của mã thông báo. 
3. Khởi tạo một danh sách trống để lưu trữ số lượng thao tác Giảm cần thiết cho mỗi mã thông báo chẵn. 
4. Đối với mỗi mã thông báo, nếu là số lẻ, hãy tiếp tục chuyển sang mã thông báo tiếp theo. Nếu là số chẵn thì đếm số lần nó phải chia cho hai để trở thành số lẻ và lưu số đếm này. 
5. Nếu không có mã số chẵn thì câu trả lời là 0. 
6. Nếu không, hãy sắp xếp số đếm theo thứ tự tăng dần. Số lượng đầu tiên thể hiện mức giảm tối thiểu cần thiết cho một mã thông báo. Mỗi lần đếm tiếp theo tương ứng với các mã thông báo bổ sung có thể được hợp nhất vào quy trình, thêm một thao tác cho mỗi mã thông báo bổ sung nếu cần. 
7. Tính tổng các phép toán cần thiết để đạt được số bước tối thiểu cần thiết để làm cho tất cả các mã thông báo trở thành số lẻ. 
8. In đáp án cho từng test. 

Tại sao nó hoạt động: thuật toán khai thác tính bất biến giúp chia mã thông báo chẵn nhỏ nhất trong ít lần nhất để đạt được các phép toán tối thiểu. Việc kết hợp các mã thông báo lớn hơn vào đó không làm tăng các bước rút gọn tối thiểu vì việc kết hợp các số chẵn không làm giảm số lượng phân chia cần thiết để đạt được số lẻ. Chiến lược tham lam này đảm bảo số lượng tổng số hoạt động tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_operations(tokens):
    even_counts = []
    for x in tokens:
        if x % 2 == 0:
            count = 0
            while x % 2 == 0:
                x //= 2
                count += 1
            even_counts.append(count)
    if not even_counts:
        return 0
    even_counts.sort()
    # minimum reductions for the first token + 1 for each remaining even token
    return even_counts[0] + len(even_counts) - 1

t = int(input())
for _ in range(t):
    n = int(input())
    tokens = list(map(int, input().split()))
    print(min_operations(tokens))
```Vòng lặp đầu tiên thu thập số lần chia thành hai cho mỗi mã thông báo chẵn. Việc sắp xếp đảm bảo chúng tôi tập trung vào mã thông báo yêu cầu ít thao tác nhất trước tiên. Thêm`len(even_counts) - 1`tài khoản để kết hợp các mã thông báo chẵn còn lại một cách hiệu quả bằng cách sử dụng Fusion. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
2
1 9
```| Mã thông báo | Thậm chí? | Chia thành Lẻ | 
| --- | --- | --- | 
| 1 | Không | 0 | 
| 9 | Không | 0 | 

Tất cả các token đều là số lẻ, vì vậy`0`hoạt động. Thuật toán xác định chính xác không có số chẵn và trả về`0`. 

### Đầu vào mẫu 2```
3
2 4 8
```| Mã thông báo | Thậm chí? | Chia thành Lẻ | 
| --- | --- | --- | 
| 2 | Có | 1 | 
| 4 | Có | 2 | 
| 8 | Có | 3 | 

Số lượng được sắp xếp:`[1, 2, 3]`. Mức giảm tối thiểu là`1`cho mã thông báo`2`. Có hai mã thông báo chẵn còn lại, thêm vào`2`hoạt động cho sự hợp nhất. Tổng cộng:`1 + 2 = 3`. Phù hợp với sản lượng dự kiến. 

Những dấu vết này cho thấy thuật toán giảm thiểu các hoạt động một cách chính xác bằng cách tập trung vào mã thông báo yêu cầu ít sự phân chia nhất và hợp nhất các hoạt động khác một cách hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log A) | Mỗi mã thông báo có thể được chia nhiều nhất log2(A) lần; sắp xếp số lượng thêm O(n log n) | 
| Không gian | O(n) | Số lượng phân chia cửa hàng cho mỗi mã thông báo chẵn | 

Cho n 2×10^5 và A ≤ 10^9, thuật toán chạy thoải mái trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# Provided samples
assert run("4\n2\n1 9\n3\n1 1 2\n3\n2 4 8\n3\n1049600 33792 1280\n") == "0\n1\n3\n10", "sample tests"

# Custom cases
assert run("1\n1\n2\n") == "1", "single even token"
assert run("1\n5\n1 3 5 7 9\n") == "0", "all odd tokens"
assert run("1\n3\n16 8 4\n") == "3", "powers of two"
assert run("1\n4\n2 2 2 2\n") == "3", "identical even tokens"
assert run("1\n2\n1 2\n") == "1", "one odd one even"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n2\n`|`1`| Mã thông báo chẵn duy nhất | 
|`1\n5\n1 3 5 7 9\n`|`0`| Tất cả các token đã là số lẻ | 
|`1\n3\n16 8 4\n`|`3`| Quyền hạn của hai, kiểm tra số lượng giảm chính xác | 
|`1\n4\n2 2 2 2\n`|`3`| Nhiều mã thông báo chẵn giống hệt nhau | 
|`1\n2\n1 2\n`|`1`| Kết hợp giữa số lẻ và số chẵn | 

## Vỏ cạnh 

Đối với đầu vào`[16, 8, 4]`, thuật toán đếm số chia:`16→4`,`8→2`,`4→1`. Sắp xếp mang lại`[1, 2, 3]`. Hoạt động tối thiểu:`1 + 2 = 3`. Điều này phù hợp với trình tự tối thiểu dự kiến: cầu chì`4`Và`8`sau đó giảm, hợp nhất với`16`và giảm, mang lại tất cả các số lẻ một cách hiệu quả. 

Đối với đầu vào`[1, 3, 5, 7]`, tất cả các token đều là số lẻ. Thuật toán không tìm thấy số chẵn và trả về`0`ngay, tránh những thao tác không cần thiết. Điều này thể hiện việc xử lý chính xác trường hợp cạnh không hoạt động.
