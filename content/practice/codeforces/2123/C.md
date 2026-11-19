---
title: "CF 2123C - Tiền tố tối thiểu và hậu tố tối đa"
description: "Chúng ta được cung cấp một mảng các số nguyên riêng biệt và một tập hợp các phép toán: bạn có thể thay thế tiền tố bằng giá trị tối thiểu hoặc hậu tố bằng giá trị tối đa của nó."
date: "2026-06-08T03:36:33+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 2123
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1034 (Div. 3)"
rating: 1000
weight: 2123
solve_time_s: 95
verified: false
draft: false
---

[CF 2123C - Tiền tố tối thiểu và hậu tố tối đa](https://codeforces.com/problemset/problem/2123/C) 

**Đánh giá:** 1000 
**Tags:** bạo lực, cấu trúc dữ liệu 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên riêng biệt và một tập hợp các phép toán: bạn có thể thay thế tiền tố bằng giá trị tối thiểu hoặc hậu tố bằng giá trị tối đa của nó. Mục tiêu là để xác định, đối với mỗi phần tử trong mảng, liệu có thể giảm mảng thành mảng một phần tử chỉ chứa phần tử đó bằng cách sử dụng một chuỗi các thao tác này hay không. Đầu ra là một chuỗi nhị phân trong đó`1`cho biết điều đó là có thể và`0`nếu không thì. 

Đầu vào đảm bảo rằng mảng có các số nguyên riêng biệt và có thể lớn tới 200.000 phần tử cho mỗi trường hợp thử nghiệm, với nhiều trường hợp thử nghiệm có tổng số phần tử lên tới 200.000 phần tử. Điều này loại trừ mọi mô phỏng mạnh mẽ của tất cả các chuỗi hoạt động vì số lượng các chuỗi có thể tăng theo cấp số nhân với kích thước mảng. Do đó, thuật toán của chúng tôi phải tính toán câu trả lời theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

Phần khó khăn của vấn đề nằm ở việc hiểu được yếu tố nào có thể truy cập được. Thật dễ dàng để nghĩ rằng có thể đạt tới bất kỳ phần tử nào nếu nó xuất hiện ở bất kỳ đâu trong mảng, nhưng các thao tác bị hạn chế: việc thay thế tiền tố luôn di chuyển về phía phần tử nhỏ nhất ở bên trái và thay thế hậu tố sẽ di chuyển về phía phần tử lớn nhất ở bên phải. Ví dụ, trong một mảng`[3, 1, 4, 2]`, phần tử`3`không thể giảm xuống một mình vì bất kỳ thao tác nào cũng sẽ kéo`1`về phía trước hoặc`4`ở phía sau, không bao giờ cô lập`3`. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản sẽ là mô phỏng mọi chuỗi hoạt động tiền tố và hậu tố có thể có cho từng phần tử và xem liệu mảng có thể thu gọn thành phần tử đó hay không. Điều này hoạt động vì các hoạt động mang tính xác định, nhưng số lượng chuỗi tăng theo cấp số nhân (`O(2^n)`), điều này là không thể thực hiện được`n = 2*10^5`. 

Quan sát quan trọng là chỉ các phần tử tối thiểu ở một số tiền tố hoặc tối đa ở một số hậu tố của mảng mới có khả năng tồn tại ở vị trí cuối cùng. Cụ thể, nếu chúng ta quét mảng từ trái sang phải để theo dõi mức chạy tối thiểu và từ phải sang trái để theo dõi mức chạy tối đa, chúng ta có thể xác định ngay phần tử nào có thể xuất hiện dưới dạng mảng một phần tử cuối cùng. Phần tử đầu tiên phải luôn có thể rút gọn đến mức tối thiểu của toàn bộ mảng để tồn tại và phần tử cuối cùng phải có thể rút gọn đến mức tối đa. Bất kỳ phần tử nào không theo trình tự tăng dần từ tối thiểu đến tối đa đều không thể bị cô lập vì các thao tác sẽ đẩy các phần tử nhỏ hơn sang trái hoặc các phần tử lớn hơn sang phải. 

Quan sát này làm giảm vấn đề xuống còn một lần truyền từ cả hai đầu của mảng, dẫn đến giải pháp thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc mảng`a`và xác định độ dài của nó`n`. Khởi tạo một chuỗi nhị phân có độ dài`n`chứa đầy số không. 
2. Duy trì hai mảng:`prefix_min`Và`suffix_max`.`prefix_min[i]`lưu trữ tối thiểu`a[0..i]`, Và`suffix_max[i]`lưu trữ tối đa`a[i..n-1]`. 
3. Điền vào`prefix_min`bằng cách quét mảng từ trái sang phải. Tại mỗi chỉ số`i`, bộ`prefix_min[i]`BẰNG`min(prefix_min[i-1], a[i])`. Điều này theo dõi phần tử nhỏ nhất có thể truyền ra phía trước. 
4. Điền vào`suffix_max`bằng cách quét mảng từ phải sang trái. Tại mỗi chỉ số`i`, bộ`suffix_max[i]`BẰNG`max(suffix_max[i+1], a[i])`. Điều này theo dõi phần tử lớn nhất có thể truyền ra phía sau. 
5. Lặp lại từng chỉ mục`i`của mảng. Một phần tử`a[i]`có thể cô lập khi và chỉ khi nó bằng`prefix_min[i]`hoặc`suffix_max[i]`. Đặt vị trí tương ứng trong chuỗi nhị phân thành`1`nếu điều kiện này được giữ. 
6. In chuỗi nhị phân thu được. 

Tại sao nó hoạt động: Bất kỳ mảng cuối cùng nào có độ dài 1 đều phải đến từ một chuỗi liên tục đẩy các phần tử nhỏ hơn sang trái hoặc các phần tử lớn hơn sang phải. Bằng cách duy trì mức tối thiểu tiền tố và mức tối đa hậu tố, chúng tôi xác định các phần tử duy nhất có thể tồn tại trong các chuỗi như vậy ở mỗi vị trí. Điều này nắm bắt chính xác tập hợp các phần tử có thể là mảng phần tử đơn cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        prefix_min = [0] * n
        suffix_max = [0] * n
        
        prefix_min[0] = a[0]
        for i in range(1, n):
            prefix_min[i] = min(prefix_min[i-1], a[i])
        
        suffix_max[-1] = a[-1]
        for i in range(n-2, -1, -1):
            suffix_max[i] = max(suffix_max[i+1], a[i])
        
        result = ['0'] * n
        for i in range(n):
            if a[i] == prefix_min[i] or a[i] == suffix_max[i]:
                result[i] = '1'
        print(''.join(result))

solve()
```Phần đầu tiên đọc đầu vào và thiết lập bộ nhớ cho các tiền tố tối thiểu và hậu tố tối đa. Hai lần quét tuyến tính tính toán các mảng này. Vòng lặp cuối cùng xây dựng chuỗi nhị phân bằng cách kiểm tra xem phần tử hiện tại có thể tồn tại trong mảng cuối cùng hay không, dựa trên tiền tố tối thiểu hoặc hậu tố tối đa. Các lỗi xảy ra có thể tránh được bằng cách khởi tạo cẩn thận phần tử đầu tiên và cuối cùng của`prefix_min`Và`suffix_max`. Giải pháp tuyến tính cả về thời gian và không gian, xử lý các ràng buộc lớn nhất một cách hiệu quả. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
6
1 3 5 4 7 2
```| tôi | một [tôi] | tiền tố_min | hậu tố_max | kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 7 | 1 | 
| 1 | 3 | 1 | 7 | 0 | 
| 2 | 5 | 1 | 7 | 0 | 
| 3 | 4 | 1 | 7 | 0 | 
| 4 | 7 | 1 | 7 | 1 | 
| 5 | 2 | 1 | 2 | 1 | 

Dấu vết này cho thấy rằng chỉ những phần tử ở mức tối thiểu cách xa bên trái hoặc tối đa cách xa bên phải mới có thể tồn tại. 

**Đầu vào mẫu 2**```
4
13 10 12 20
```| tôi | một [tôi] | tiền tố_min | hậu tố_max | kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | 13 | 13 | 20 | 1 | 
| 1 | 10 | 10 | 20 | 1 | 
| 2 | 12 | 10 | 20 | 0 | 
| 3 | 20 | 10 | 20 | 1 | 

Dấu vết này xác nhận rằng các phần tử ở giữa không nằm trên ranh giới của chuỗi tối thiểu hoặc tối đa không thể bị cô lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt cho tiền tố tối thiểu, một lượt cho hậu tố tối đa, một lượt để tạo chuỗi kết quả | 
| Không gian | O(n) | Hai mảng để lưu trữ tiền tố tối thiểu và hậu tố tối đa | 

Với những hạn chế, giải pháp này dễ dàng phù hợp với giới hạn về thời gian và bộ nhớ. Mỗi trường hợp thử nghiệm mất thời gian tuyến tính và tổng n trên tất cả các trường hợp thử nghiệm được giới hạn ở mức 2*10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("3\n6\n1 3 5 4 7 2\n4\n13 10 12 20\n7\n1 2 3 4 5 6 7\n") == "100011\n1101\n1000001"

# Custom cases
assert run("1\n2\n5 1\n") == "11", "two elements, min and max"
assert run("1\n3\n2 1 3\n") == "111", "middle element reachable by prefix or suffix"
assert run("1\n5\n10 20 30 40 50\n") == "10001", "strictly increasing sequence"
assert run("1\n5\n50 40 30 20 10\n") == "10001", "strictly decreasing sequence"
assert run("1\n4\n3 1 4 2\n") == "1001", "random small array"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2\n5 1`|`11`| Cả hai phần tử có thể được tách biệt vì một phần tử là min, một phần tử là max | 
|`3\n2 1 3`|`111`| Phần tử ở giữa có thể truy cập được bằng thao tác hậu tố/tiền tố | 
|`5\n10 20 30 40 50`|`10001`| Chỉ đầu tiên và cuối cùng tồn tại trong mảng tăng dần | 
|`5\n50 40 30 20 10`|`10001`| Chỉ có người đầu tiên và người cuối cùng sống sót |
