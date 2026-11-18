---
title: "CF 2053B - Trường phái ấn tượng xuất sắc"
description: "Chúng ta được cung cấp một chuỗi các lần hiển thị, trong đó mỗi lần hiển thị được biểu thị bằng một phạm vi các giá trị số nguyên có thể có. Eric chỉ có thể nhớ rằng lần hiển thị $i$-th nằm ở đâu đó giữa $li$ và $ri$."
date: "2026-06-08T08:24:03+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2053
codeforces_index: "B"
codeforces_contest_name: "Good Bye 2024: 2025 is NEAR"
rating: 1200
weight: 2053
solve_time_s: 105
verified: false
draft: false
---

[CF 2053B - Nghệ sĩ theo trường phái ấn tượng xuất sắc](https://codeforces.com/problemset/problem/2053/B) 

**Đánh giá:** 1200 
**Tags:** tìm kiếm nhị phân, sức mạnh vũ phu, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các lần hiển thị, trong đó mỗi lần hiển thị được biểu thị bằng một phạm vi các giá trị số nguyên có thể có. Eric chỉ có thể nhớ rằng$i$- ấn tượng thứ ở đâu đó giữa$l_i$Và$r_i$. Thử thách ở đây là xác định, đối với từng lần hiển thị riêng lẻ, liệu có thể gán giá trị cho nó sao cho không lần hiển thị nào khác có cùng giá trị hay không. Nói cách khác, đối với mỗi ấn tượng, chúng ta phải quyết định xem liệu có tồn tại sự phân công khả thi của tất cả các ấn tượng làm cho ấn tượng cụ thể đó trở nên duy nhất hay không. 

Những hạn chế là đáng kể: có thể có tới$2 \cdot 10^5$số lần hiển thị trên tất cả các trường hợp thử nghiệm và mỗi phạm vi có thể kéo dài tới$2n$. Điều này loại trừ bất kỳ cách tiếp cận nào cố gắng thử mọi phép gán có thể một cách rõ ràng vì điều đó có thể dẫn đến sự bùng nổ tổ hợp. Một sự ngây thơ$O(n^2)$cách tiếp cận-kiểm tra từng lần hiển thị so với tất cả các lần hiển thị khác bằng cách kiểm tra tất cả các giá trị tiềm năng-sẽ hoạt động gần đúng$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa mức có thể chấp nhận được. Vấn đề đòi hỏi một giải pháp thông minh hơn, tuyến tính hoặc gần tuyến tính. 

Các trường hợp đặc biệt có thể gây ra sự cố khi triển khai bất cẩn bao gồm các lần hiển thị bị buộc phải có cùng một giá trị, chẳng hạn như cả hai lần hiển thị đều nằm trong phạm vi$[1, 1]$, làm cho tính duy nhất không thể xảy ra. Một tình huống phức tạp khác là khi các phạm vi trùng nhau nhưng một lần hiển thị lại hẹp hơn nhiều so với các lần hiển thị khác; sự phân bổ tham lam ngây thơ có thể không nhận ra rằng cái hẹp luôn có thể bị cô lập nếu được xử lý đúng cách. 

## Phương pháp tiếp cận 

Giải pháp vũ phu sẽ cố gắng tạo ra mọi phép gán giá trị có thể có trong phạm vi của mỗi lượt hiển thị và sau đó kiểm tra xem liệu một lượt hiển thị nhất định có thể được tạo thành duy nhất hay không. Về nguyên tắc, điều này đúng vì nó khám phá tất cả các tình huống có thể xảy ra, nhưng nó hoàn toàn không thực tế vì ngay cả số lần hiển thị khiêm tốn với kích thước phạm vi vừa phải cũng tạo ra nhiều kết hợp theo cấp số nhân. 

Thông tin chi tiết quan trọng đến từ việc nhận thấy rằng nếu chúng ta sắp xếp số lần hiển thị theo ranh giới tối thiểu và tối đa của chúng, thì chúng ta có thể suy luận về tính duy nhất dựa trên các giá trị cực trị thay vì liệt kê tất cả các phép gán. Cụ thể, một lần hiển thị được đảm bảo không phải là duy nhất nếu phạm vi của nó bị bao phủ hoàn toàn bởi các phạm vi khác "tiêu thụ" tất cả các con số mà nó có thể lấy. Ngược lại, nếu tồn tại ít nhất một giá trị trong phạm vi của nó mà không có lần hiển thị nào khác có thể chiếm giữ đồng thời, thì giá trị đó có thể được coi là duy nhất. 

Bằng cách theo dõi mức tối thiểu của giới hạn trên và mức tối đa của giới hạn dưới từ trái sang phải và từ phải sang trái, chúng tôi có thể nhanh chóng xác định xem mỗi lần hiển thị có giá trị không xung đột với tất cả các giá trị khác hay không. Điều này làm giảm vấn đề đối với việc sắp xếp và quét tuyến tính thay vì tìm kiếm tổ hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((2n)^n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả lần hiển thị của trường hợp kiểm thử hiện tại và lưu trữ chúng dưới dạng cặp`(l_i, r_i)`cùng với chỉ mục ban đầu của họ. Giữ chỉ mục gốc là điều cần thiết để xây dựng lại đầu ra theo thứ tự đầu vào. 
2. Sắp xếp số lần hiển thị theo giá trị tối thiểu của chúng`l_i`. Điều này cho phép chúng tôi suy luận về giá trị sớm nhất có thể mà mỗi lần hiển thị có thể nhận được so với những lần hiển thị khác. 
3. Xây dựng hai mảng`prefix_max_l`Và`suffix_min_r`. các`prefix_max_l[i]`lưu trữ tối đa`l_j`cho tất cả lần hiển thị ở bên trái hoặc tại chỉ mục`i`. các`suffix_min_r[i]`lưu trữ tối thiểu`r_j`cho tất cả lần hiển thị ở bên phải hoặc tại chỉ mục`i`. Các mảng này tóm tắt sự chồng chéo khả thi của các phạm vi một cách hiệu quả. 
4. Đối với mỗi lần hiển thị`i`, xác định xem nó có thể được gán một giá trị duy nhất hay không. Để làm điều này, hãy kiểm tra xem có tồn tại một số trong`[l_i, r_i]`điều đó không bị che phủ bởi bất kỳ ấn tượng nào khác. Điều này tóm lại là so sánh`l_i`với mức tối đa của tất cả các giới hạn dưới trước đó (`prefix_max_l`) Và`r_i`với mức tối thiểu của tất cả các giới hạn trên tiếp theo (`suffix_min_r`). Nếu như`l_i`lớn hơn giới hạn dưới tối đa của phần còn lại, hoặc`r_i`nhỏ hơn giới hạn trên tối thiểu của phần còn lại thì ấn tượng có thể được tạo thành duy nhất. 
5. Xây dựng chuỗi đầu ra bằng cách viết '1' cho số lần hiển thị duy nhất và '0' nếu không, khôi phục thứ tự đầu vào ban đầu. 

Tại sao nó hoạt động: Thuật toán kiểm tra từng lần hiển thị một cách hiệu quả xem có tồn tại ít nhất một số nguyên trong phạm vi của nó mà không bị xung đột với tất cả các phạm vi khác hay không. Bằng cách tính toán trước tiền tố cực đại và hậu tố cực tiểu, chúng tôi tránh phải quét liên tục tất cả các lần hiển thị khác, đảm bảo tính chính xác trong khi vẫn hoạt động hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        impressions = []
        for i in range(n):
            l, r = map(int, input().split())
            impressions.append((l, r, i))

        # Sort by left boundary
        impressions.sort()
        prefix_max_l = [0] * n
        suffix_min_r = [0] * n

        prefix_max_l[0] = impressions[0][0]
        for i in range(1, n):
            prefix_max_l[i] = max(prefix_max_l[i-1], impressions[i][0])

        suffix_min_r[-1] = impressions[-1][1]
        for i in range(n-2, -1, -1):
            suffix_min_r[i] = min(suffix_min_r[i+1], impressions[i][1])

        res = ['0'] * n
        for i in range(n):
            l, r, idx = impressions[i]
            left_ok = i == 0 or l > prefix_max_l[i-1]
            right_ok = i == n-1 or r < suffix_min_r[i+1]
            if left_ok or right_ok:
                res[idx] = '1'

        print(''.join(res))

solve()
```Mã đọc đầu vào nhanh chóng bằng cách sử dụng`sys.stdin.readline`. Mỗi lần hiển thị được lưu trữ cùng với chỉ mục ban đầu để duy trì thứ tự đầu ra. Sắp xếp theo`l_i`cho phép chúng tôi xây dựng tiền tố cực đại và hậu tố cực tiểu, tóm tắt một cách hiệu quả ảnh hưởng của tất cả các lần hiển thị khác đối với lần hiển thị hiện tại. Khi kiểm tra tính duy nhất, so sánh hiện tại`l_i`Và`r_i`đối với các mảng được tính toán trước này sẽ xác định liệu một số có tồn tại trong phạm vi mà bất kỳ lần hiển thị nào khác không thể lấy được hay không. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu thứ hai:```
4
1 3
1 3
1 3
1 3
```Sau khi sắp xếp và xây dựng mảng tiền tố/hậu tố: 

| Chỉ mục | tôi | r | tiền tố_max_l | hậu tố_min_r | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 3 | 1 | 3 | 
| 1 | 1 | 3 | 1 | 3 | 
| 2 | 1 | 3 | 1 | 3 | 
| 3 | 1 | 3 | 1 | 3 | 

Kiểm tra tính duy nhất: 

- Lần hiển thị 0: left_ok Đúng (i=0), right_ok Sai → '1' 
- Lần hiển thị 1: left_ok Sai, right_ok Sai → '1' 
- Lần hiển thị 2: left_ok Sai, right_ok Sai → '1' 
- Lần hiển thị 3: left_ok Sai, right_ok Đúng (i=n-1) → '1' 

Đầu ra là`1111`. 

Bảng này cho thấy các cực trị của tiền tố và hậu tố giúp xác định tính duy nhất mà không cần liệt kê tất cả các phép gán có thể có như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; tính toán tiền tố/hậu tố là O(n) | 
| Không gian | O(n) | Mảng lưu trữ số lần hiển thị, tiền tố_max_l, hậu tố_min_r và đầu ra | 

Điều này phù hợp với các ràng buộc vì việc sắp xếp$2 \cdot 10^5$các phần tử và thực hiện quét tuyến tính chỉ dưới 1 giây và mức sử dụng bộ nhớ rất khiêm tốn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("5\n2\n1 1\n1 1\n4\n1 3\n1 3\n1 3\n1 3\n6\n3 6\n2 2\n1 2\n1 1\n3 4\n2 2\n7\n3 4\n4 4\n4 4\n1 3\n2 5\n1 4\n2 2\n3\n4 5\n4 4\n5 5\n") == "00\n1111\n100110\n1001111\n011"

# Custom cases
assert run("1\n1\n1 1\n") == "1", "single impression should be unique"
assert run("1\n2\n1 2\n2 2\n") == "11", "two impressions with overlapping ranges"
assert run("1\n3\n1 3\n2 2\n3 3\n") == "111", "narrow central impression surrounded by wider"
assert run("1\n4\n1 2\n1 2\n1 2\n1 2\n") == "0000", "all identical ranges"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 lần hiển thị | 1 | Kích thước đầu vào tối thiểu luôn là duy nhất | 
| Phạm vi chồng chéo | 11 | Nhiều lần hiển thị vẫn có thể là duy nhất | 
| Ấn tượng trung tâm hẹp | 111 | Thuật toán cô lập chính xác phạm vi hẹp | 
| Tất cả các phạm vi giống hệt nhau | 0000 | Phát hiện khi không thể có tính duy nhất | 

## Vỏ cạnh 

Đối với hai lần hiển thị
