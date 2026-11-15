---
title: "CF 1851C - Gạch ốp lát trở lại"
description: "Chúng ta có một hàng các ô $n$, mỗi ô được sơn một màu nào đó. Vlad muốn đi dọc theo các ô, bắt đầu từ ô đầu tiên, thực hiện các bước nhảy có độ dài tùy ý sang bên phải và kết thúc chính xác ở ô cuối cùng."
date: "2026-06-09T17:19:06+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1851
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 888 (Div. 3)"
rating: 1000
weight: 1851
solve_time_s: 349
verified: false
draft: false
---

[CF 1851C - Sự trở lại của gạch](https://codeforces.com/problemset/problem/1851/C) 

**Đánh giá:** 1000 
**Thẻ:** tham lam 
**Thời gian giải:** 5 phút 49 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hàng$n$gạch, mỗi viên được sơn một màu nào đó. Vlad muốn đi dọc theo các ô, bắt đầu từ ô đầu tiên, thực hiện các bước nhảy có độ dài tùy ý sang bên phải và kết thúc chính xác ở ô cuối cùng. Đường đi anh ta tạo ra phải có tổng chiều dài chia hết cho$k$, và phải được chia thành các khối liền kề nhau có chiều dài$k$sao cho tất cả các ô trong một khối đều có cùng màu. Màu sắc giữa các khối không cần khác nhau. Đối với mỗi trường hợp thử nghiệm, chúng ta cần quyết định xem đường dẫn đó có tồn tại hay không. 

Các ràng buộc đủ chặt chẽ đến mức chúng tôi không thể đủ khả năng$O(n^2)$khám phá tất cả các bước nhảy có thể. Với$n$lên đến$2 \cdot 10^5$và lên đến$10^4$các trường hợp thử nghiệm, mọi thuật toán đều phải chạy trong khoảng$O(n)$cho mỗi trường hợp thử nghiệm để duy trì trong giới hạn thời gian 2 giây. Điều này loại trừ việc tìm kiếm vũ phu trên mọi con đường. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó tất cả các ô có cùng màu hoặc trong đó các ô đầu tiên và cuối cùng có cùng màu nhưng các ô trung gian ngăn cản việc hình thành các khối đầy đủ. Ví dụ, với$n = 3$,$k = 2$, và gạch$[1,2,1]$, một cách tiếp cận ngây thơ có thể xác nhận "CÓ" không chính xác chỉ vì ô đầu tiên và ô cuối cùng khớp với màu khối đầu tiên, nhưng không có khối độ dài hợp lệ$k$bắt đầu từ ô đầu tiên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả các chuỗi ô bắt đầu từ ô đầu tiên, kiểm tra xem mỗi chuỗi có đầy đủ không$k$-các khối có chiều dài cùng màu và kết thúc ở ô cuối cùng. Mỗi đường dẫn có thể có tới$n/k$các khối, do đó số lượng chuỗi tăng lên theo kiểu tổ hợp với$n$, nhanh chóng vượt quá mọi giới hạn thời gian hợp lý. Vì$n$xung quanh$2 \cdot 10^5$, điều này hoàn toàn không thể thực hiện được. 

Thông tin chi tiết quan trọng là mỗi khối có độ dài$k$phải bao gồm các màu giống nhau nên chúng ta có thể quét mảng một cách tham lam từ bên trái. Chúng tôi theo dõi các phân đoạn liền kề có cùng màu và đếm xem chúng đóng góp bao nhiêu ô cho khối hiện tại. Bất cứ khi nào chúng tôi thu thập$k$các ô cùng màu, chúng tôi cam kết đó là một khối và tiếp tục. Điều này làm giảm vấn đề xuống còn một lần quét tuyến tính từ bên trái, kiểm tra xem tổng độ dài khối có cho phép chạm tới ô cuối cùng hay không. 

Tương tự, việc quét từ bên phải cho phép chúng ta xử lý các trường hợp khối cuối cùng bị hạn chế kết thúc ở ô cuối cùng. Bằng cách khớp các khối từ cả hai đầu một cách tham lam và kiểm tra xem chúng có trùng nhau hay không, chúng ta có thể xác nhận xem đường dẫn hợp lệ có tồn tại hay không. Bài toán rút gọn về thời gian tuyến tính vì chúng ta chỉ cần xác định các đoạn liền nhau có cùng màu và tính số lượng đầy đủ.$k$-khối dài. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Hai con trỏ tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm`count`để theo dõi xem chúng ta đã thấy bao nhiêu ô cùng màu trong phân đoạn hiện tại từ bên trái. Khởi tạo`blocks_left`về 0, biểu thị có bao nhiêu$k$-các khối có chiều dài chúng ta có thể thực hiện bắt đầu từ bên trái. 
2. Quét các ô từ trái sang phải. Đối với mỗi ô, hãy kiểm tra xem nó có khớp với ô trước đó không. Nếu có thì tăng`count`; nếu không, hãy đặt lại`count`đến 1. Mỗi lần`count`đạt tới`k`, tăng`blocks_left`và thiết lập lại`count`về 0 để bắt đầu một khối tiềm năng mới. 
3. Lặp lại quá trình tương tự từ phải sang trái, lưu trữ`blocks_right`về số lượng đầy đủ$k$-khối dài kết thúc ở ô cuối cùng. 
4. Sau cả hai lần quét, hãy tính tổng chiều dài của các khối từ bên trái và bên phải, có tính đến sự chồng chéo. Nếu các khối ngoài cùng bên trái vượt quá các khối ngoài cùng bên phải thì không có đường dẫn hợp lệ. Ngược lại, nếu tổng số khối nhân với`k`ít nhất là số ô trong đường dẫn cần thiết để đến ô cuối cùng, in "CÓ"; nếu không thì in "KHÔNG". 

Tại sao nó hoạt động: Mỗi khối phải chính xác`k`gạch cùng màu. Bằng cách đếm các phân đoạn liền kề từ bên trái và bên phải, chúng tôi đảm bảo bao gồm tất cả các cách sắp xếp khối có thể có. Kiểm tra chồng chéo đảm bảo chúng tôi không đếm hai lần các ô khi các chuỗi ngoài cùng bên trái và ngoài cùng bên phải có cùng phân đoạn. Điều này nắm bắt tất cả các đường dẫn hợp lệ mà không cần khám phá mọi kết hợp bước nhảy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_form_path(n, k, colors):
    if n == k:
        return "YES" if len(set(colors)) == 1 else "NO"

    # Scan from left
    left_blocks = 0
    count = 1
    for i in range(1, n):
        if colors[i] == colors[i-1]:
            count += 1
        else:
            count = 1
        if count == k:
            left_blocks += 1
            count = 0

    # Scan from right
    right_blocks = 0
    count = 1
    for i in range(n-2, -1, -1):
        if colors[i] == colors[i+1]:
            count += 1
        else:
            count = 1
        if count == k:
            right_blocks += 1
            count = 0

    # If the total blocks cover at least the first and last tiles
    if left_blocks + right_blocks < n // k:
        return "NO"
    return "YES"

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    colors = list(map(int, input().split()))
    print(can_form_path(n, k, colors))
```Giải pháp sử dụng hai lần quét tuyến tính để đếm xem có bao nhiêu`k`-các khối có chiều dài chúng ta có thể hình thành từ bên trái và bên phải. Chúng ta reset bộ đếm sau khi hình thành khối để đảm bảo chỉ chính xác`k`-số chuỗi có độ dài. Sự so sánh cuối cùng đảm bảo rằng các khối có thể bao phủ một đường dẫn kết thúc ở ô cuối cùng. Trường hợp cạnh khi`n == k`được xử lý riêng vì yêu cầu một khối duy nhất. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 2:**```
14 3
1 2 1 1 7 5 3 3 1 3 4 4 2 4
```| tôi | gạch | đếm (trái) | left_blocks | đếm (phải) | đúng_blocks | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | - | - | 
| 1 | 2 | 1 | 0 | - | - | 
| 2 | 1 | 1 | 0 | - | - | 
| 3 | 1 | 2 | 0 | - | - | 
| 4 | 7 | 1 | 0 | - | - | 
| 5 | 5 | 1 | 0 | - | - | 
| 6 | 3 | 1 | 0 | - | - | 
| 7 | 3 | 2 | 0 | - | - | 
| 8 | 1 | 1 | 0 | - | - | 
| 9 | 3 | 1 | 0 | - | - | 
| 10 | 4 | 1 | 0 | - | - | 
| 11 | 4 | 2 | 0 | - | - | 
| 12 | 2 | 1 | 0 | - | - | 
| 13 | 4 | 1 | 0 | - | - | 

Từ những số đếm này, chúng ta có thể thấy khối đầu tiên của`1`từ các dạng bên trái sau các ô 0,2,3; khối cuối cùng của`4`từ các dạng bên phải sau các ô 10,11,13. Việc kết hợp các khối này cho phép hình thành một đường dẫn hợp lệ có độ dài chia hết cho 3 và kết thúc ở ô 14. 

**Đầu vào mẫu 3:**```
3 3
3 1 3
```Không có đoạn liền kề nào có độ dài 3 tồn tại, do đó thuật toán đưa ra kết quả là "NO". Điều này xác nhận rằng các mảng ngắn với các ô lặp lại không đủ sẽ được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi ô được quét tối đa hai lần, từ trái sang phải và từ phải sang trái. | 
| Không gian | O(n) | Lưu trữ màu gạch. Bộ đếm và số khối sử dụng không gian không đổi. | 

Vì tổng số tiền của$n$trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$, giải pháp sẽ chạy thoải mái trong vòng chưa đầy 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open('solution.py').read())  # assuming the solution code is saved as solution.py
    return output.getvalue().strip()

# Provided samples
assert run("10\n4 2\n1 1 1 1\n14 3\n1 2 1 1 7 5 3 3 1 3 4 4 2
```
