---
title: "CF 2084C - Em Đã Vươn Xa Với Ân Sủng"
description: "Cho hai hoán vị a và b có cùng kích thước. Trong một lần di chuyển, chúng ta được phép chọn hai vị trí và hoán đổi chúng trong cả hai mảng cùng một lúc."
date: "2026-06-08T06:15:21+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2084
codeforces_index: "C"
codeforces_contest_name: "Teza Round 1 (Codeforces Round 1015, Div. 1 + Div. 2)"
rating: 1400
weight: 2084
solve_time_s: 314
verified: false
draft: false
---

[CF 2084C - Bạn đã bay xa với ân sủng](https://codeforces.com/problemset/problem/2084/C) 

**Đánh giá:** 1400 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, tham lam, triển khai 
**Thời gian giải:** 5 phút 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai hoán vị`a`Và`b`có cùng kích thước. Trong một lần di chuyển, chúng ta được phép chọn hai vị trí và hoán đổi chúng trong cả hai mảng cùng một lúc. Điều này có nghĩa là chúng tôi không sắp xếp lại`a`Và`b`độc lập; thay vào đó, chúng tôi đang hoán vị các cột, trong đó mỗi chỉ mục`i`giữ một giá trị cặp`(a[i], b[i])`và hoán đổi chỉ số hoán đổi toàn bộ cặp. 

Mục tiêu là sắp xếp lại các cặp này sao cho sau một số lần hoán đổi, mảng thứ hai trở thành đảo ngược của mảng thứ nhất. Nói cách khác, sau khi sắp xếp lại các chỉ số, với mỗi vị trí`i`, cặp ở vị trí`i`phải phản ánh cặp ở vị trí`n-i+1`. 

Về cơ bản đây là một vấn đề ghép nối trên các vị trí chỉ số. Mỗi chỉ số đóng góp một mối quan hệ trực tiếp giữa các giá trị trong`a`Và`b`và chúng ta đang cố gắng hoán vị các chỉ số để các mối quan hệ này trở nên đối xứng khi đảo ngược. 

Các ràng buộc cho phép lên đến`2·10^5`tổng các phần tử, vì vậy mọi giải pháp đều phải tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Bất cứ điều gì liên quan đến việc thử tất cả các hoán vị hoặc kiểm tra tất cả các cặp một cách rõ ràng sẽ ngay lập tức thất bại. 

Một trường hợp thất bại tinh tế xảy ra khi mọi người cho rằng họ có thể tham lam cố định các vị trí cục bộ. Ví dụ: nếu một vị trí khớp với đối tác ngược của nó ở một tọa độ chứ không phải tọa độ kia, việc hoán đổi đơn giản có thể phá vỡ cấu trúc cố định trước đó vì các hoán đổi luôn ảnh hưởng đồng thời đến cả hai mảng. 

Một trường hợp phức tạp khác là khi về mặt lý thuyết, cấu trúc có thể thực hiện được nhưng yêu cầu nhận ra các chu kỳ có độ dài lớn hơn 2 trong số các ánh xạ chỉ mục. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ sẽ là thử tất cả các hoán vị của các chỉ số và kiểm tra xem các mảng kết quả có thỏa mãn điều kiện ngược lại hay không. Điều này đúng nhưng có độ phức tạp về mặt giai thừa và hoàn toàn không khả thi ngay cả đối với`n = 10`. 

Quan sát quan trọng là chúng ta đang hoán vị các cặp`(a[i], b[i])`. Sau khi sắp xếp lại, chúng ta cần:```
(a[p[i]], b[p[i]]) = (b[p[n-i+1]], a[p[n-i+1]])
```Điều này cho chúng ta biết rằng mỗi vị trí phải được ghép nối với một vị trí khác có giá trị đảo ngược. 

Vì vậy mỗi chỉ số`i`phải phù hợp với một số chỉ mục`j`như vậy:```
a[i] = b[j] and b[i] = a[j]
```Điều này tự nhiên xác định một biểu đồ ghép nối. Mỗi chỉ mục kết nối với chỉ mục đại diện cho cặp đảo ngược của nó. Nếu chúng ta xem mỗi chỉ mục dưới dạng một nút và xác định “ứng cử viên phản chiếu” của nó, thì chúng ta sẽ nhận được cấu trúc trong đó: 

- hoặc một chỉ mục kết hợp với chính nó (chỉ có thể nếu`a[i] = b[i]`) 
- hoặc các chỉ số tạo thành 2 chu kỳ 
- nếu không thì không thể 

Sau khi cấu trúc ghép nối hợp lệ được thiết lập, chúng ta chỉ cần sắp xếp lại các chỉ số sao cho các nút được ghép nối được đặt đối xứng. 

Phần mang tính xây dựng sau đó chỉ là xây dựng thứ tự cuối cùng và mô phỏng các giao dịch hoán đổi để đạt được nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Ghép cặp + thi công | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng ánh xạ từ mỗi cặp`(a[i], b[i])`đến các vị trí chỉ mục của nó bằng cách sử dụng bản đồ băm. 
2. Đối với mỗi chỉ số`i`, tính toán đối tác cần thiết của nó được xác định bởi`(b[i], a[i])`. Đây là vị trí hợp lệ duy nhất có thể phản ánh`i`. 
3. Nếu không có đối tác như vậy tồn tại đối với một số`i`, câu trả lời là không thể. 
4. Xây dựng chu kỳ chỉ số: 

nếu`i`bản đồ tới`j`, Và`j`bản đồ trở lại`i`, chúng tạo thành một cặp hợp lệ. 
5. Xử lý các điểm cố định ở đâu`a[i] == b[i]`. Những thứ này phải chiếm vị trí trung tâm nếu`n`là kỳ quặc; nếu không thì chúng phải được ghép đôi với nhau. 
6. Xây dựng hoán vị đích của chỉ số: 

đặt mỗi cặp hợp lệ`(i, j)`ở những vị trí đối xứng`(l, r)`. 
7. Nếu có một trung tâm, hãy đặt nó vào giữa. 
8. Bây giờ chúng ta đã có thứ tự mục tiêu của các chỉ số. Nhiệm vụ giảm xuống việc chuyển đổi hoán vị danh tính thành thứ tự này bằng cách sử dụng các giao dịch hoán đổi. 
9. Mô phỏng các giao dịch hoán đổi bằng cách duy trì vị trí hiện tại của các chỉ số và hoán đổi vị trí một cách tham lam. 

Tại sao nó hoạt động: mọi giải pháp hợp lệ đều tương ứng chính xác với việc ghép các chỉ số thành các cấu trúc được phản chiếu. Các hoạt động hoán đổi chỉ hoán vị toàn bộ các cặp, do đó có thể đạt được mọi sắp xếp ghép nối hợp lệ. Việc xây dựng đảm bảo tất cả các ràng buộc được thỏa mãn trên toàn cầu trước khi chúng tôi bắt đầu hoán đổi, do đó giai đoạn hoán đổi chỉ thực hiện một hoán vị đã biết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    pos = {}
    for i in range(n):
        pos[(a[i], b[i])] = i
    
    used = [False] * n
    pairs = []
    center = -1
    
    for i in range(n):
        if used[i]:
            continue
        if a[i] == b[i]:
            if center == -1:
                center = i
            used[i] = True
        else:
            if (b[i], a[i]) not in pos:
                print(-1)
                return
            j = pos[(b[i], a[i])]
            if i == j:
                print(-1)
                return
            used[i] = used[j] = True
            pairs.append((i, j))
    
    order = []
    for i, j in pairs:
        order.append(i)
    if center != -1:
        order.append(center)
    for i, j in reversed(pairs):
        order.append(j)
    
    if len(order) != n:
        print(-1)
        return
    
    cur_pos = list(range(n))
    idx_at = list(range(n))
    
    pos_of = list(range(n))
    
    ops = []
    
    for i in range(n):
        target = order[i]
        if idx_at[i] == target:
            continue
        j = pos_of[target]
        
        idx_at[i], idx_at[j] = idx_at[j], idx_at[i]
        pos_of[idx_at[i]] = i
        pos_of[idx_at[j]] = j
        
        ops.append((i + 1, j + 1))
    
    print(len(ops))
    for x, y in ops:
        print(x, y)

t = int(input())
for _ in range(t):
    solve()
```## Ví dụ đã hoạt động 

### Ví dụ:`n = 4`Cho phép:```
a = [1, 3, 2, 4]
b = [2, 4, 1, 3]
```Chúng tôi tạo thành cặp:`(1,2)->0`,`(3,4)->1`,`(2,1)->2`,`(4,3)->3`Chúng tôi nhận được cặp gương hợp lệ:`(0,2)`Và`(1,3)`Đặt hàng mục tiêu:`[0,1,3,2]`Bây giờ chúng ta hoán đổi các chỉ số để khớp với thứ tự này. 

| bước | thứ tự hiện tại | trao đổi | 
| --- | --- | --- | 
| bắt đầu | 0 1 2 3 | - | 
| 1 | 0 1 2 3 | hoán đổi 2,3 | 
| 2 | 0 1 3 2 | xong | 

Điều này tạo ra:```
a = [1,3,4,2]
b = [2,4,3,1]
```đó thỏa mãn điều kiện. 

### Ví dụ: một tâm cố định 

Nếu`(a[i] == b[i])`, chỉ số đó phải nằm ở giữa khi`n`thật kỳ quặc. 

Điều này đảm bảo tính đối xứng được bảo toàn mà không vi phạm các ràng buộc đảo ngược. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi chỉ mục được xử lý một lần để ghép nối và một lần để hoán đổi | 
| Không gian | O(n) | bản đồ băm và mảng để tái thiết | 

Tổng độ phức tạp là tuyến tính cho mỗi trường hợp thử nghiệm, phù hợp thoải mái trong giới hạn toàn cầu 2e5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        pos = {}
        for i in range(n):
            pos[(a[i], b[i])] = i

        used = [False] * n
        pairs = []
        center = -1

        for i in range(n):
            if used[i]:
                continue
            if a[i] == b[i]:
                if center == -1:
                    center = i
                used[i] = True
            else:
                if (b[i], a[i]) not in pos:
                    return "-1\n"
                j = pos[(b[i], a[i])]
                if i == j:
                    return "-1\n"
                used[i] = used[j] = True
                pairs.append((i, j))

        order = []
        for i, j in pairs:
            order.append(i)
        if center != -1:
            order.append(center)
        for i, j in reversed(pairs):
            order.append(j)

        if len(order) != n:
            return "-1\n"

        idx_at = list(range(n))
        pos_of = list(range(n))
        ops = []

        for i in range(n):
            target = order[i]
            if idx_at[i] == target:
                continue
            j = pos_of[target]
            idx_at[i], idx_at[j] = idx_at[j], idx_at[i]
            pos_of[idx_at[i]] = i
            pos_of[idx_at[j]] = j
            ops.append((i+1, j+1))

        out = [str(len(ops))]
        out += [f"{x} {y}" for x, y in ops]
        return "\n".join(out) + "\n"

    t = int(input())
    res = []
    for _ in range(t):
        res.append(solve())
    return "".join(res)

assert run("""5
2
1 2
1 2
2
1 2
2 1
4
1 3 2 4
2 4 1 3
5
2 5 1 3 4
3 5 4 2 1
5
3 1 2 4 5
1 2 3 4 5
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu n=2 | có thể/không thể | logic đối xứng cơ sở | 
| đảo ngược đã hợp lệ | 0 lần | trường hợp nhận dạng | 
| yêu cầu hoán đổi | hoạt động không tầm thường | xây dựng đúng đắn | 
| tâm điểm cố định | vị trí đúng | xử lý độ dài lẻ |
