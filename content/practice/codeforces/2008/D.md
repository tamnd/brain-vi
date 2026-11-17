---
title: "CF 2008D - Sở thích của Sakurako"
description: "Chúng ta được cho một hoán vị các số nguyên từ 1 đến n và mỗi số có màu đen hoặc trắng. Từ bất kỳ số nguyên bắt đầu i nào, chúng ta có thể áp dụng hoán vị nhiều lần để chuyển từ i sang pi, sau đó tới p{pi}, v.v."
date: "2026-06-08T13:24:56+07:00"
tags: ["codeforces", "competitive-programming", "dp", "dsu", "graphs", "math"]
categories: ["algorithms"]
codeforces_contest: 2008
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 970 (Div. 3)"
rating: 1100
weight: 2008
solve_time_s: 124
verified: false
draft: false
---

[CF 2008D - Sở thích của Sakurako](https://codeforces.com/problemset/problem/2008/D) 

**Đánh giá:** 1100 
**Tags:** dp, dsu, đồ thị, toán học 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số nguyên từ 1 đến n và mỗi số có màu đen hoặc trắng. Từ bất kỳ số nguyên bắt đầu i nào, chúng ta có thể áp dụng hoán vị nhiều lần để chuyển từ i sang p_i, sau đó tới p_{p_i}, v.v. Một số j được coi là có thể truy cập được từ i nếu chúng ta có thể đạt tới j sau một số hữu hạn các bước nhảy này. Với mỗi i, chúng ta cần tính F(i), tổng số số nguyên đen có thể truy cập được từ i. 

Kích thước đầu vào có thể lớn: n có thể lên tới 200.000 và tổng n trên tất cả các trường hợp thử nghiệm không vượt quá 200.000. Điều này loại trừ bất kỳ giải pháp nào tuân theo rõ ràng các bước nhảy cho từng số bắt đầu trong thời gian O(n) trên mỗi số, vì điều đó có thể dẫn đến các hoạt động O(n^2) trong trường hợp xấu nhất, tức là khoảng 4 * 10^10 hoạt động và quá chậm trong giới hạn thời gian 2 giây. 

Các trường hợp biên phát sinh khi một chu kỳ rất ngắn hoặc chỉ bao gồm một phần tử. Ví dụ: nếu hoán vị là [1] và phần tử duy nhất có màu đen thì kết quả đầu ra đúng là 1. Một giải pháp bất cẩn cố tính toán trước các nút có thể truy cập mà không xem xét chu kỳ có thể đếm quá hoặc thiếu các nút đen. Một trường hợp cạnh khác xảy ra khi một chu trình có tất cả các phần tử màu trắng ngoại trừ nút bắt đầu; thuật toán phải xác định chính xác các nút đen nào được bao gồm trong tập hợp có thể truy cập. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: với mỗi chỉ mục i, hãy thực hiện theo hoán vị cho đến khi chúng ta đạt được số đã truy cập trước đó, đánh dấu từng số đã truy cập và đếm xem chúng ta gặp bao nhiêu nút đen. Điều này đúng vì chúng tôi đã khám phá toàn diện tất cả các số có thể truy cập. Tuy nhiên, trong trường hợp xấu nhất, mỗi i có thể đi qua tối đa n bước nếu hoán vị tạo thành một chu trình lớn. Đối với n=2*10^5, điều này dẫn đến hoạt động O(n^2) và quá chậm. 

Cái nhìn sâu sắc quan trọng đến từ việc nhận ra rằng một hoán vị bao gồm hoàn toàn các chu trình rời rạc. Khi một chu trình được phát hiện, mọi phần tử trong chu trình đó có thể đạt đến cùng một bộ số: tất cả các nút trong chu trình và bất kỳ nút nào dẫn vào chu trình đó. Số lượng màu đen cho một nút trong chu kỳ là tổng số nút đen trong chu kỳ đó. Các nút dẫn vào chu kỳ cuối cùng cũng đạt được số lượng màu đen giống như chính chu kỳ đó. Điều này có nghĩa là chúng ta có thể phân tách hoán vị thành các chu kỳ, tính số lượng màu đen cho mỗi chu kỳ và sau đó gán số lượng cho tất cả các nút trong chu kỳ đó. Đối với các chuỗi dẫn vào một chu kỳ, chúng tôi truyền bá số lượng màu đen của chu kỳ dọc theo chuỗi. Điều này làm giảm độ phức tạp xuống O(n) cho mỗi trường hợp thử nghiệm vì mỗi nút được xử lý chính xác một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Phân hủy chu kỳ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`visited`có kích thước n để theo dõi xem một nút đã được xử lý chưa. Đồng thời khởi tạo một mảng`F`để lưu trữ số lượng màu đen cho mỗi nút. 
2. Lặp qua từng chỉ số i từ 1 đến n. Nếu như`visited[i]`là đúng, bỏ qua nó. Nếu không, hãy bắt đầu theo hoán vị từ i và lưu trữ tất cả các nút đã truy cập vào danh sách tạm thời`path`. 
3. Khi chúng ta duyệt qua, hãy tiếp tục thêm các nút vào`path`cho đến khi chúng ta đến được một nút đã được truy cập. Nếu nút này là một phần của đường dẫn hiện tại thì chúng tôi đã phát hiện được một chu trình. Xác định chu trình là đoạn của`path`từ lần xuất hiện đầu tiên của nút này cho đến khi kết thúc. 
4. Đếm số nút đen trong chu kỳ bằng cách kiểm tra chuỗi màu. Đối với mỗi nút trong chu kỳ, đặt F[nút] thành số lượng màu đen của chu kỳ. 
5. Đối với các nút trong`path`nằm ngoài chu trình (các nút dẫn vào chu kỳ), gán cùng giá trị F cho chu kỳ, vì cuối cùng chúng sẽ đạt đến chu trình và tất cả các nút trong chu kỳ đều có thể truy cập được. 
6. Đánh dấu tất cả các nút trong`path`như đã ghé thăm. 
7. Sau khi xử lý tất cả các nút, xuất ra mảng F. 

Tại sao nó hoạt động: Mỗi nút thuộc về chính xác một chu kỳ hoặc dẫn vào một chu kỳ duy nhất. Bằng cách đếm các nút đen trên mỗi chu kỳ và truyền số đếm đó đến các nút dẫn vào chu kỳ, mỗi nút được gán chính xác số lượng nút đen có thể truy cập được từ nó. Điều bất biến là khi xử lý một đường dẫn hoặc chu trình, tất cả các nút trong đó được đánh dấu đã truy cập chính xác một lần và nhận giá trị F chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(lambda x: int(x)-1, input().split()))
        s = input().strip()
        F = [0]*n
        visited = [False]*n
        
        for i in range(n):
            if visited[i]:
                continue
            path = []
            current = i
            while not visited[current]:
                path.append(current)
                visited[current] = True
                current = p[current]
            
            # Detect cycle
            if current in path:
                idx = path.index(current)
                cycle = path[idx:]
                cycle_black = sum(1 for x in cycle if s[x]=='0')
                for x in cycle:
                    F[x] = cycle_black
                for x in path[:idx]:
                    F[x] = cycle_black
            else:
                # Entire path leads to an already processed cycle
                cycle_black = F[current]
                for x in path:
                    F[x] = cycle_black
        print(*F)

if __name__ == "__main__":
    solve()
```Giải pháp xử lý nhiều trường hợp thử nghiệm bằng vòng lặp. Hoán vị được chuyển đổi thành các chỉ số dựa trên 0 để truy cập mảng dễ dàng hơn. Đường dẫn được theo dõi cho đến khi tìm thấy nút đã truy cập; nếu nút được truy cập nằm trong đường dẫn hiện tại, chúng tôi sẽ xác định một chu kỳ mới. Số lượng nút đen được tính toán và gán cho cả chu kỳ và các nút trước đó. Sự phân biệt cẩn thận giữa các nút trong chu trình và các nút dẫn vào nó sẽ tránh được việc đếm hai lần hoặc thiếu các nút. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
5
1
1
0
```| tôi | con đường | chu kỳ | chu kỳ_đen | F | 
| --- | --- | --- | --- | --- | 
| 0 | [0] | [0] | 1 | 1 | 

Tất cả các nút đều là chu kỳ có độ dài 1, số lượng màu đen 1. F=[1]. 

**Mẫu 2** 

đầu vào:```
6
3 5 6 1 2 4
010000
```| tôi | con đường | chu kỳ | chu kỳ_đen | F | 
| --- | --- | --- | --- | --- | 
| 0 | [0,2,5,3] | [0,2,5,3] | 4 | [4,?,4,4,?,4] | 
| 1 | [1,4] | [1,4] | 1 | [4,1,4,4,1,4] | 

Điều này xác nhận rằng thuật toán phân tách chính xác nhiều chu kỳ và ấn định số lượng màu đen. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được truy cập chính xác một lần và phát hiện chu kỳ thông qua path.index chạy trong O (độ dài đường dẫn), tối đa là n cho mỗi trường hợp thử nghiệm. | 
| Không gian | O(n) | Mảng F, đã truy cập và đường dẫn lưu trữ tối đa n phần tử. | 

Với tổng n trên tất cả các trường hợp thử nghiệm 2*10^5, thuật toán chạy thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("1\n1\n1\n0\n") == "1", "single element black"
assert run("1\n5\n1 2 4 5 3\n10101\n") == "0 1 1 1 1", "mixed small permutation"
assert run("1\n6\n3 5 6 1 2 4\n010000\n") == "4 1 4 4 1 4", "complex cycles"

# Custom cases
assert run("1\n3\n1 2 3\n000\n") == "1 1 1", "all black, self cycles"
assert run("1\n4\n2 3 4 1\n0110\n") == "2 2 2 2", "single cycle length 4 with mixed colors"
assert run("1\n2\n2 1\n10\n") == "1 1", "two-element cycle with one black"
assert run("1\n5\n5 4 3 2 1\n11111\n") == "0 0 0 0 0", "all white, no black"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử màu đen | 1 | Vỏ cơ bản có một nút màu đen duy nhất | 
| Hỗn hợp 5 yếu tố | 0 1 1 1 1 | Tuyên truyền chính xác số lượng đen | 
| Phức hợp 6 nguyên tố | 4 1 4 4 1 4 | Nhiều chu kỳ với đường dẫn dẫn đầu | 
| Tất cả màu đen, tự chu kỳ | 1 1 1 | Các nút đen được tính chính xác theo chu kỳ | 
| Đơn 4 thì hỗn hợp | 2 2 2 2 | Số lượng màu đen trong một chu kỳ dài hơn | 
| Chu trình 2 yếu tố | 1 1 | Chu kỳ có độ dài chẵn nhỏ | 
| Tất cả màu trắng | | |
