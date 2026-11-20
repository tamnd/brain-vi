---
title: "CF 2179E - Blackslex và các cô gái"
description: "Chúng tôi được yêu cầu chia cử tri thành các khu vực theo mô hình nhị phân đồng thời tôn trọng quy mô khu vực tối thiểu. Cụ thể, chúng ta có hai đảng A và B với tổng số cử tri lần lượt là x và y."
date: "2026-06-07T22:18:14+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 2179
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1071 (Div. 3)"
rating: 1800
weight: 2179
solve_time_s: 127
verified: false
draft: false
---

[CF 2179E - Blackslex và Girls](https://codeforces.com/problemset/problem/2179/E) 

**Đánh giá:** 1800 
**Tags:** giải thuật xây dựng, hình học, toán học 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu chia cử tri thành các khu vực theo mô hình nhị phân đồng thời tôn trọng quy mô khu vực tối thiểu. Cụ thể, chúng ta có hai bên A và B với tổng số`x`Và`y`cử tri tương ứng. có`n`huyện và từng huyện`i`yêu cầu ít nhất`p_i`tổng số cử tri. Chúng tôi cũng được cung cấp một chuỗi nhị phân`s`chiều dài`n`cho biết người chiến thắng mong muốn của mỗi quận: nếu`s_i = 0`, bên A phải có nhiều cử tri hơn bên B ở quận`i`; nếu như`s_i = 1`, bên B phải có nhiều cử tri hơn A. 

Nhiệm vụ của chúng tôi là xác định xem có thể phân bổ số cử tri theo cách mà mỗi quận đều có một người chiến thắng khớp với chuỗi hay không`s`đồng thời đáp ứng số lượng cử tri tối thiểu. 

Những hạn chế là đáng kể.`n`có thể lên tới 2·10^5 và tổng của`n`trên tất cả các trường hợp thử nghiệm không vượt quá 2·10^5. Mỗi số cử tri`x`,`y`và tối thiểu của quận`p_i`có thể lên tới 10^9. Một cách tiếp cận ngây thơ thử tất cả các phân bổ là hoàn toàn không khả thi vì nó sẽ đòi hỏi một số lượng lớn các hoạt động. Bất kỳ giải pháp nào cũng phải chạy theo thời gian tuyến tính so với`n`cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh không rõ ràng phát sinh khi quy mô quận tối thiểu`p_i`thu hút nhiều cử tri hơn mức có sẵn hoặc khi việc phân phối để đáp ứng điều kiện người chiến thắng là không thể. Ví dụ, nếu một huyện có`p_i = 5`,`x = 2`,`y = 3`, Và`s_i = 0`, không có cách nào để A có nhiều cử tri hơn B vì một mình A không thể đạt được đa số tối thiểu. Tương tự, nếu tất cả các quận đều yêu cầu B thắng nhưng`y`quá nhỏ nên không thể giải quyết được. Những ràng buộc này cho thấy vấn đề không chỉ là về tổng số mà còn về sự phân bổ cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét tất cả các tổ hợp cử tri ở mỗi quận, kiểm tra mọi phân bổ có thể có cho các mảng`a`Và`b`. Cách tiếp cận này đúng vì cuối cùng nó sẽ liệt kê mọi cấu hình hợp lệ, nhưng rõ ràng là không khả thi. Vì`n = 10^5`và số lượng cử tri lên tới 10^9, số lượng phân phối tiềm năng tăng theo cấp số nhân, vượt xa mọi thời gian tính toán thực tế. 

Quan sát quan trọng là đối với mỗi quận, yêu cầu giảm xuống mức bất bình đẳng tuyến tính: nếu`s_i = 0`, chúng ta phải có`a_i > b_i`Và`a_i + b_i >= p_i`; nếu như`s_i = 1`, chúng tôi yêu cầu`b_i > a_i`Và`a_i + b_i >= p_i`. Từ`a_i`Và`b_i`là các số nguyên không âm, chúng ta có thể tính giới hạn phân bổ tối thiểu và tối đa cho từng quận riêng lẻ và sau đó kiểm tra xem tổng số cử tri có`x`Và`y`có thể đáp ứng các giới hạn này trên toàn cầu. Điều này làm giảm vấn đề xuống còn hai phạm vi: tổng số cử tri phải đi đến A và tổng số cử tri phải đi đến B, có thể được kiểm tra theo thời gian tuyến tính. 

Quan sát cho thấy các yêu cầu của mỗi khu vực xác định phạm vi số lượng cử tri từ A (hoặc B) cho phép chúng tôi xây dựng phép kiểm tra tổng tối thiểu và tổng tối đa đơn giản. Nếu tổng số cử tri của mỗi đảng nằm trong phạm vi này thì sẽ có giải pháp; nếu không thì không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(tổng(p_i))) | O(n) | Quá chậm | 
| Phân bổ dựa trên phạm vi | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi huyện`i`, xác định số lượng cử tri tối thiểu phải về bên A (`min_a`) và mức tối thiểu cho bên B (`min_b`). Nếu như`s_i = 0`, A phải thắng nên mức phân bổ tối thiểu cho A là`(p_i + 1) // 2`. Nếu như`s_i = 1`, B phải thắng nên mức phân bổ tối thiểu cho A là`0`. 
2. Đối với cùng một quận, hãy xác định mức phân bổ tối đa cho A. Điều này bị hạn chế bởi tổng số cử tri trong quận: nếu`s_i = 0`, số lớn nhất mà A có thể chịu được là`p_i - 0`(tất cả cử tri nếu B không chọn). Nếu như`s_i = 1`, số lớn nhất mà A có thể chịu được là`p_i - (p_i + 1) // 2`để đảm bảo B thắng. 
3. Tính tổng số tiền phân bổ tối thiểu trên tất cả các quận để có được`min_total_a`. Tổng số phân bổ tối đa để có được`max_total_a`. Điều này xác định phạm vi khả thi toàn cầu cho A. 
4. Kiểm tra xem`x`(tổng số cử tri của A) nằm trong`[min_total_a, max_total_a]`. Tương tự, hãy xác minh rằng`y`phù hợp với phân phối bổ sung. 
5. Nếu cả hai tổng đều nằm trong phạm vi khả thi, hãy in CÓ. Ngược lại in ra NO. 

**Tại sao nó hoạt động:** Mỗi quận xác định phạm vi khả thi cục bộ cho A và B một cách độc lập. Tổng các mức tối thiểu và tối đa cho một phạm vi khả thi toàn cầu. Nếu tổng số cử tri nằm ngoài phạm vi này, một số quận không thể đáp ứng cả yêu cầu tối thiểu và người chiến thắng nên không có giải pháp nào tồn tại. Nếu nó nằm bên trong, việc phân bổ hợp lệ luôn có thể được xây dựng một cách tham lam bằng cách phân bổ số cử tri dư thừa theo bất kỳ thứ tự nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, x, y = map(int, input().split())
        s = input().strip()
        p = list(map(int, input().split()))
        
        min_a_total = 0
        max_a_total = 0
        
        for i in range(n):
            if s[i] == '0':
                min_a = (p[i] + 1) // 2
                max_a = p[i]
            else:
                min_a = 0
                max_a = p[i] // 2
            min_a_total += min_a
            max_a_total += max_a
        
        if min_a_total <= x <= max_a_total and (sum(p) - x) <= y:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên tính toán số cử tri tối thiểu và tối đa theo quận cho bên A dựa trên yêu cầu của người chiến thắng. Tính tổng tất cả các quận sẽ cho ra phạm vi tổng khả thi cho A. Sau đó, chúng tôi kiểm tra xem liệu`x`nằm trong phạm vi này và liệu các cử tri còn lại có thể đáp ứng B. Thứ tự thực hiện các phép tính rất quan trọng: phép chia số nguyên được sử dụng cẩn thận để xử lý số lẻ và số chẵn`p_i`giá trị, đảm bảo rằng đa số tồn tại khi cần thiết. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:```
n=3, x=5, y=5
s=010
p=[2,4,3]
```| tôi | s_i | p_i | phút_a | max_a | tổng cộng tối thiểu | max_a_total | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 2 | 1 | 2 | 1 | 2 | 
| 2 | 1 | 4 | 0 | 2 | 1 | 4 | 
| 3 | 0 | 3 | 2 | 3 | 3 | 7 |`min_total_a = 3`,`max_total_a = 7`. Từ`x = 5`ở trong`[3, 7]`, Và`y = 5`đủ để bao gồm những cử tri còn lại, kết quả là CÓ. 

**Ví dụ 2:** 

đầu vào:```
n=2, x=3, y=3
s=00
p=[4,3]
```| tôi | s_i | p_i | phút_a | max_a | tổng cộng tối thiểu | max_a_total | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 4 | 2 | 4 | 2 | 4 | 
| 2 | 0 | 3 | 2 | 3 | 4 | 7 |`min_total_a = 4`,`max_total_a = 7`.`x = 3`ít hơn`min_total_a`, vì vậy đầu ra là KHÔNG. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi quận được xử lý một lần; các phép tính tổng là tuyến tính | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng`p`có chiều dài n | 

Thuật toán dễ dàng xử lý`n`lên tới 2·10^5 và tổng của`n`trên tất cả các trường hợp thử nghiệm lên tới 2·10^5. Việc sử dụng bộ nhớ nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("6\n3 5 5\n010\n2 4 3\n4 2 3\n0001\n1 1 1 1\n2 4 2\n00\n3 3\n4 23 20\n1111\n2 2 2 2\n1 25 26\n0\n51\n2 4 2\n00\n3 4") == \
"YES\nNO\nYES\nNO\nNO\nNO"

# Custom cases
assert run("1\n1 1 1\n0\n1") == "YES", "single district, minimal input"
assert run("1\n1
```
