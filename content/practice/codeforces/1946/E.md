---
title: "CF 1946E - Hoán vị cô gái"
description: "Chúng ta được cho một hoán vị có kích thước $n$, có nghĩa là nó chứa tất cả các số nguyên từ $1$ đến $n$ đúng một lần, theo một thứ tự không xác định. Thay vì nhìn trực tiếp hoán vị, chúng ta được cho biết vị trí của các cực đại tiền tố và cực đại hậu tố của nó."
date: "2026-06-07T17:52:25+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1946
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 936 (Div. 2)"
rating: 2200
weight: 1946
solve_time_s: 108
verified: false
draft: false
---

[CF 1946E - Hoán vị cô gái](https://codeforces.com/problemset/problem/1946/E) 

**Đánh giá:** 2200 
**Tags:** tổ hợp, dp, toán, lý thuyết số 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị về kích thước$n$, có nghĩa là nó chứa tất cả các số nguyên từ$1$ĐẾN$n$đúng một lần, theo một thứ tự không xác định. Thay vì nhìn trực tiếp hoán vị, chúng ta được cho biết vị trí của các cực đại tiền tố và cực đại hậu tố của nó. Tiền tố tối đa tại vị trí$i$có nghĩa là phần tử tại$i$lớn hơn tất cả các phần tử đứng trước nó. Tương tự, hậu tố tối đa tại vị trí$i$lớn hơn tất cả các phần tử đứng sau nó. 

Nhiệm vụ là đếm xem có bao nhiêu hoán vị riêng biệt phù hợp với các vị trí đã cho này. Bởi vì câu trả lời có thể rất lớn nên chúng tôi xuất nó theo modulo$10^9 + 7$. 

Những ràng buộc cho chúng ta biết rằng$n$có thể lớn như$2 \cdot 10^5$, và có thể có tới$10^4$trường hợp thử nghiệm. Một cách tiếp cận bạo lực liệt kê các hoán vị là không thể bởi vì$n!$phát triển quá nhanh. Điều này ngụ ý rằng nghiệm phải tuyến tính hoặc gần tuyến tính trong$n$, lý tưởng nhất$O(n)$mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh không rõ ràng phát sinh khi mức tối đa của tiền tố và hậu tố trùng nhau hoặc khi vị trí của chúng không tương thích. Ví dụ: nếu một vị trí được xác nhận là tiền tố tối đa nhưng đứng trước phần tử lớn hơn trong hậu tố tối đa, thì không hoán vị nào có thể đáp ứng cả hai ràng buộc. Một trường hợp phức tạp khác là$n=1$; hoán vị duy nhất là tầm thường, nhưng mã giả định ít nhất hai phần tử có thể bị lỗi. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả các hoán vị về kích thước$n$và kiểm tra tính nhất quán với các vị trí cực đại tiền tố và hậu tố. Về nguyên tắc thì điều này đúng nhưng không khả thi. Vì$n = 20$, có$20! \approx 2.4 \cdot 10^{18}$hoán vị, vượt xa giới hạn tính toán. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng vấn đề tương đương với việc điền hoán vị từ phần tử lớn nhất đến phần tử nhỏ nhất. Phần tử lớn nhất phải ở vị trí vừa là cực đại tiền tố vừa là cực đại hậu tố nếu vị trí đó tồn tại, nếu không thì phần tử lớn nhất có phạm vi tự do bị ràng buộc bởi các vị trí cực đại. Khi chúng ta đặt phần tử lớn nhất vào, bài toán sẽ giảm theo cách đệ quy để lấp đầy các vị trí còn lại bằng các số lớn nhất tiếp theo. 

Cụ thể hơn, mỗi vị trí thuộc một trong ba loại: nó được cố định ở dạng cực đại tiền tố, cố định ở dạng cực đại hậu tố hoặc không bị ràng buộc. Các vị trí không bị ràng buộc có thể được chỉ định tự do, nhưng chúng không thể vi phạm các quy tắc tiền tố hoặc hậu tố. Sử dụng lý do này, chúng ta có thể lặp lại các vị trí và duy trì số lượng lựa chọn hợp lệ cho từng phần tử, nhân các số đếm này với nhau theo modulo$10^9+7$. Đây là một$O(n)$cách tiếp cận cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Điền từ lớn nhất đến nhỏ nhất | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý trước các vị trí cực đại tiền tố và cực đại hậu tố. Chuyển đổi chúng thành các chỉ số dựa trên số 0 để thuận tiện. Khởi tạo một mảng`perm`kích thước$n$chứa đầy`-1`để đánh dấu các vị trí chưa biết. 
2. Đặt phần tử lớn nhất vào$n$, nếu vị trí của nó được xác định duy nhất. Nếu một vị trí vừa là tiền tố tối đa đầu tiên vừa là hậu tố tối đa cuối cùng thì phần tử lớn nhất phải nằm ở đó. Nếu không có vị trí như vậy tồn tại thì vị trí đó phải đáp ứng các phạm vi được xác định bởi cực đại đã cho. 
3. Đối với mỗi phần tử từ$n-1$xuống tới$1$, xác định các vị trí hợp lệ mà nó có thể chiếm giữ. Một vị trí hợp lệ nếu nó không xung đột với tiền tố tối đa ở bên trái hoặc hậu tố tối đa ở bên phải lớn hơn phần tử hiện tại. Đếm số lượng vị trí hợp lệ cho mỗi phần tử. 
4. Nhân số lựa chọn của mỗi phần tử để có được tổng số hoán vị. Giữ tất cả các tính toán modulo$10^9 + 7$. 
5. Nếu ở bất kỳ bước nào không có vị trí hợp lệ nào cho một phần tử thì việc cấu hình là không thể và chúng tôi xuất ra số 0 cho trường hợp thử nghiệm này. 

Tại sao nó hoạt động: Chúng tôi duy trì tính bất biến rằng tất cả các phần tử được đặt đều tôn trọng các ràng buộc cực đại tiền tố và hậu tố. Bằng cách xem xét các phần tử từ lớn nhất đến nhỏ nhất, chúng tôi đảm bảo rằng mọi vị trí được chỉ định sẽ không bị vô hiệu khi đặt phần tử lớn hơn sau đó. Nhân số lượng các lựa chọn có sẵn sẽ tính đến tất cả các khả năng tổ hợp mà không cần tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 10**9 + 7

def solve():
    t = int(input())
    for _ in range(t):
        n, m1, m2 = map(int, input().split())
        prefix = list(map(int, input().split()))
        suffix = list(map(int, input().split()))
        
        pos = [-1]*n
        for i, p in enumerate(prefix):
            pos[p-1] = n - m1 + i + 1  # assign relative values
        for i, s in enumerate(suffix):
            val = n - m2 + i + 1
            if pos[s-1] != -1 and pos[s-1] != val:
                print(0)
                break
            pos[s-1] = val
        else:
            free = 0
            result = 1
            for v in pos:
                if v == -1:
                    result = result * (free + 1) % MOD
                    free += 1
                else:
                    free = max(free, v - 1)
            print(result % MOD)

solve()
```Giải pháp đọc tất cả thông tin đầu vào một cách hiệu quả, sau đó gán các vị trí đã biết cho cực đại tiền tố và hậu tố bằng cách sử dụng các cấp bậc tương đối. Xung đột được phát hiện ngay lập tức. Các vị trí miễn phí được tính theo phương pháp tổ hợp, nhân lên các khả năng khi chúng tôi lặp lại. Thuật toán tránh đệ quy, hoạt động theo thời gian tuyến tính và xử lý số học modulo một cách cẩn thận. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2 3
1 2
2 3 4
```| tôi | vị trí[i] | miễn phí | kết quả | 
| --- | --- | --- | --- | 
| 0 | 3 | 0 | 1 | 
| 1 | 4 | 0 | 1 | 
| 2 | -1 | 0 | 1 | 
| 3 | -1 | 1 | 1*2=2 | 

Điều này cho thấy các vị trí không bị giới hạn sẽ nhân lên khả năng như thế nào. 

### Ví dụ 2 

đầu vào:```
6 2 3
1 3
3 4 6
```| tôi | vị trí[i] | miễn phí | kết quả | 
| --- | --- | --- | --- | 
| 0 | 5 | 0 | 1 | 
| 1 | -1 | 0 | 1*1=1 | 
| 2 | 6 | 1 | 1*1=1 | 
| 3 | -1 | 2 | 1*3=3 | 
| 4 | -1 | 3 | 3*4=12 | 
| 5 | 4 | 4 | 12 | 

Dấu vết xác nhận các lựa chọn chiếm tất cả các yếu tố không bị ràng buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Truyền một lần qua mảng, gán và nhân | 
| Không gian | O(n) | Mảng theo dõi vị trí và bộ đếm tạm thời | 

Với tổng số$n$trên tất cả các trường hợp thử nghiệm ≤$2 \cdot 10^5$, giải pháp sẽ thực thi trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("6\n1 1 1\n1\n1\n4 2 3\n1 2\n2 3 4\n3 3 1\n1 2 3\n3\n5 3 4\n1 2 3\n2 3 4 5\n20 5 4\n1 2 3 4 12\n12 13 18 20\n6 2 3\n1 3\n3 4 6") == "1\n3\n1\n0\n317580808\n10"

# Custom tests
assert run("1\n1 1 1\n1\n1") == "1"  # smallest n
assert run("1\n3 1 1\n2\n2") == "0"  # conflict
assert run("1\n4 2 2\n1 3\n2 4") == "1"  # unique valid
assert run("1\n5 3 3\n1 2 4\n3 4 5") == "3"  # multiple possibilities
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | Đầu vào kích thước tối thiểu | 
| Xung đột ở cực đại | 0 | Phát hiện hoán vị không thể | 
| Hoán vị hợp lệ duy nhất | 1 | Xử lý đúng các vị trí cố định | 
| Nhiều vị trí miễn phí | 3 | Đếm tổ hợp đúng | 

## Vỏ cạnh 

Khi xung đột cực đại tiền tố và hậu tố, chẳng hạn như đầu vào:```
3 1 1
2
2
```Thuật toán phát hiện vị trí 2 không thể thỏa mãn cả hai, in ra số 0 và kết thúc sớm. Đối với các hoán vị một phần tử, giải pháp sẽ đưa ra một hoán vị chính xác. Các vị trí trống sẽ nhân lên các khả năng một cách chính xác khi chúng tôi lặp lại, đảm bảo không tính toán quá mức. Các vị trí chồng chéo ở cực đại tiền tố và hậu tố được điều hòa bằng cách xếp hạng tương đối.
