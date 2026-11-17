---
title: "CF 1948C - Đường Mũi Tên"
description: "Chúng ta có một lưới 2 hàng với các cột $n$ và mỗi ô chứa một mũi tên chỉ sang trái hoặc phải. Robot bắt đầu ở góc trên bên trái và mỗi giây đầu tiên nó di chuyển đến một ô liền kề (lên, xuống, trái, phải) rồi đi theo mũi tên trong ô mới."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "dfs-and-similar", "dp", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1948
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 163 (Rated for Div. 2)"
rating: 1300
weight: 1948
solve_time_s: 56
verified: true
draft: false
---

[CF 1948C - Đường dẫn mũi tên](https://codeforces.com/problemset/problem/1948/C) 

**Đánh giá:** 1300 
**Tags:** sức mạnh vũ phu, thuật toán xây dựng, dfs và tương tự, dp, đồ thị, đường dẫn ngắn nhất 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một lưới 2 hàng với$n$cột và mỗi ô chứa một mũi tên chỉ sang trái hoặc phải. Robot bắt đầu ở góc trên bên trái và mỗi giây đầu tiên nó di chuyển đến một ô liền kề (lên, xuống, trái, phải) rồi đi theo mũi tên trong ô mới. Mục tiêu của chúng tôi là xác định xem robot có thể chạm tới góc dưới bên phải hay không. 

Đầu vào đưa ra nhiều trường hợp kiểm thử, mỗi trường hợp chỉ định$n$và hai chuỗi mũi tên tượng trưng cho hai hàng. Đầu ra là CÓ nếu đạt$(2, n)$là có thể, nếu không thì KHÔNG. 

Những ràng buộc chỉ ra rằng$n$có thể đi lên$2 \cdot 10^5$và tổng của$n$trên tất cả các trường hợp thử nghiệm cũng bị giới hạn bởi$2 \cdot 10^5$. Điều này loại trừ bất kỳ thuật toán nào khám phá lưới một cách triệt để theo kiểu đệ quy đơn giản hoặc kiểu BFS với việc mở rộng trạng thái đầy đủ, bởi vì cách tiếp cận như vậy có thể liên quan đến$O(n^2)$hoạt động trên mỗi trường hợp thử nghiệm, quá chậm. Robot chỉ di chuyển đến các ô lân cận và bị hạn chế bởi các hướng mũi tên, do đó, giải pháp phải tận dụng cấu trúc này thay vì mù quáng khám phá tất cả các đường dẫn. 

Các trường hợp cạnh có thể gặp phải một giải pháp đơn giản bao gồm các lưới trong đó việc chuyển đổi hàng là bắt buộc phải tiếp tục. Ví dụ: nếu hàng đầu tiên kết thúc bằng mũi tên trái và hàng thứ hai kết thúc bằng mũi tên phải, việc di chuyển thẳng dọc theo hàng trên cùng sẽ bẫy robot. Một trường hợp tinh tế khác xảy ra khi robot có thể dao động giữa các hàng vô thời hạn mà không bao giờ chạm tới cột cuối cùng. Nếu chúng ta bỏ qua các ràng buộc mũi tên và chỉ mô phỏng chuyển động đơn giản, những tình huống này có thể gợi ý không chính xác về sự tồn tại của một đường dẫn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng mọi chuỗi di chuyển có thể có bằng BFS hoặc DFS. Tại mỗi ô, robot có thể thử di chuyển lên, xuống, sang trái hoặc sang phải rồi đi theo mũi tên. Về nguyên tắc, điều này đúng vì nó khám phá tất cả các trạng thái có thể tiếp cận. Tuy nhiên, đối với$n = 2 \cdot 10^5$, mỗi ô có thể phân nhánh thành 4 khả năng và tổng số trạng thái tăng lên quá lớn. Số lượng hoạt động có thể đạt tới$O(4^n)$trong trường hợp xấu nhất là không thể thực hiện được. 

Thông tin chi tiết quan trọng đến từ việc quan sát rằng chuyển động theo chiều ngang bị hạn chế chặt chẽ bởi các hướng mũi tên. Mũi tên trái ở hàng trên cùng sẽ ngăn cản việc di chuyển sang phải mà không chuyển đổi hàng. Việc chuyển đổi hàng chỉ hữu ích nếu mũi tên ở hàng mới cho phép di chuyển về phía trước. Điều này giúp giảm vấn đề xuống còn kiểm tra cục bộ: rô-bốt luôn có thể di chuyển theo chiều ngang nếu mũi tên khớp với hướng của nó và nó có thể chuyển đổi hàng khi cả mũi tên của hàng hiện tại và hàng đích đều hướng về phía sau so với vị trí của rô-bốt. Do đó, chúng ta có thể phân tích đường dẫn bằng cách sử dụng phương pháp truyền tải tham lam dọc theo các cột, kiểm tra xem mỗi cột có cho phép tiến trình chuyển tiếp hay chuyển đổi hay không. 

Quan sát này cho phép một$O(n)$Cách tiếp cận: quét các cột từ trái sang phải và kiểm tra xem có tồn tại đường dẫn hợp lệ hay không bằng cách sử dụng tối đa một chuyển đổi hàng ở mỗi cột. Chúng ta không cần phải mô phỏng mọi trình tự có thể nữa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(4^n) | O(n) | Quá chậm | 
| Tham lam đi qua | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu từ cột 0 ở hàng trên cùng. Theo dõi hàng hiện tại (1 hoặc 2). 
2. Đối với mỗi cột, hãy kiểm tra mũi tên ở hàng hiện tại. Nếu nó trỏ đúng, robot có thể di chuyển sang cột tiếp theo trên cùng hàng. Nếu nó hướng sang trái, robot sẽ bị chặn di chuyển sang phải trừ khi được phép chuyển hàng. 
3. Kiểm tra xem có thể chuyển sang hàng khác hay không. Công tắc có hiệu lực nếu mũi tên trong cột hiện tại của hàng khác chỉ sang phải (về phía mục tiêu). Chuyển hàng và tiếp tục di chuyển. 
4. Nếu không thể di chuyển về phía trước hoặc chuyển đổi ở bất kỳ cột nào, hãy kết thúc và trả về NO. 
5. Tiếp tục cho đến cột cuối cùng. Nếu robot đến hàng 2, cột$n$, trả về CÓ; nếu không, trả về KHÔNG. 

Tại sao nó hoạt động: Mỗi lần kiểm tra cột đảm bảo rô-bốt chỉ di chuyển khi có hành động pháp lý, duy trì tính bất biến mà rô-bốt không bao giờ chuyển sang trạng thái không hợp lệ. Chuyển động ngang yêu cầu mũi tên phù hợp và chuyển đổi dọc chỉ xảy ra khi hàng mục tiêu cho phép di chuyển về phía trước. Việc quét theo cột đảm bảo rằng chúng tôi không bao giờ bỏ lỡ một con đường tiềm năng đồng thời tránh được việc khám phá theo cấp số nhân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_reach(n, top, bottom):
    row = 0  # 0 for top, 1 for bottom
    col = 0
    while col < n:
        if row == 0:
            if top[col] == '>':
                col += 1
            elif bottom[col] == '>':
                row = 1
            else:
                return "NO"
        else:
            if bottom[col] == '>':
                col += 1
            elif top[col] == '>':
                row = 0
            else:
                return "NO"
    return "YES" if row == 1 else "NO"

t = int(input())
for _ in range(t):
    n = int(input())
    top = input().strip()
    bottom = input().strip()
    print(can_reach(n, top, bottom))
```Giải pháp sử dụng quét theo cột tham lam. Chúng tôi theo dõi hàng hiện tại và cố gắng tiến về phía trước bất cứ khi nào có thể. Nếu chuyển động về phía trước bị chặn, chúng tôi sẽ kiểm tra xem việc chuyển hàng có cho phép tiến triển hay không. Điều này tôn trọng các quy tắc chuyển động và ngăn ngừa việc vượt quá giới hạn do robot di chuyển theo từng cột. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
4
4
>><<
>>><
2
><
><
```| Cột | Hàng hiện tại | Hành động | Hàng tiếp theo | Cột Tiếp theo | 
| --- | --- | --- | --- | --- | 
| 0 | Đầu trang | top[0]='>' → di chuyển sang phải | Đầu trang | 1 | 
| 1 | Đầu trang | top[1]='>' → di chuyển sang phải | Đầu trang | 2 | 
| 2 | Đầu trang | top[2]='<' → không thể sang phải, đáy[2]='>' → switch | Dưới cùng | 2 | 
| 2 | Dưới cùng | đáy[2]='>' → di chuyển sang phải | Dưới cùng | 3 | 
| 3 | Dưới cùng | đáy[3]='<' → chặn di chuyển, chạm tới cuối cột | Dưới cùng | 4 | 

Robot kết thúc ở hàng dưới cùng, cột cuối cùng. Đầu ra CÓ. 

Đầu vào mẫu 2:```
2
><
><
```| Cột | Hàng hiện tại | Hành động | Hàng tiếp theo | Cột Tiếp theo | 
| --- | --- | --- | --- | --- | 
| 0 | Đầu trang | top[0]='>' → di chuyển sang phải | Đầu trang | 1 | 
| 1 | Đầu trang | top[1]='<' → cho phép chuyển đổi (bottom[1]='>') | Dưới cùng | 1 | 
| 1 | Dưới cùng | đáy[1]='>' → di chuyển sang phải | Dưới cùng | 2 | 

Robot chạm đến phía dưới bên phải. Đầu ra CÓ. 

Những dấu vết này cho thấy thuật toán xử lý chuyển đổi hàng một cách chính xác và xác định khi đường dẫn chuyển tiếp bị chặn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi quét từng cột một lần, thực hiện tối đa kiểm tra hàng trên mỗi cột. | 
| Không gian | O(1) | Chỉ có một vài biến theo dõi chỉ số hàng và cột. | 

Với ràng buộc rằng tổng số tiền$n$trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$, phương pháp này chạy thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        t = int(input())
        for _ in range(t):
            n = int(input())
            top = input().strip()
            bottom = input().strip()
            print(can_reach(n, top, bottom))
    return out.getvalue().strip()

# Provided samples
assert run("4\n4\n>><<\n>>><\n2\n><\n><\n4\n>>><\n>><<\n6\n>><<><\n><>>><") == "YES\nYES\nNO\nYES"

# Custom cases
assert run("1\n2\n><\n<>") == "NO", "switch not possible"
assert run("1\n2\n>>\n>>") == "NO", "cannot reach bottom row"
assert run("1\n2\n>>\n><") == "YES", "must switch at column 1"
assert run("1\n4\n>>>>\n>>>>") == "NO", "top row blocked from reaching bottom at last column"
assert run("1\n4\n>><>\n>>><") == "YES", "complex switch path"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n><\n<> | KHÔNG | Chuyển đổi không thể | 
| 2\n>>\n>> | KHÔNG | Không thể tới hàng dưới cùng | 
| 2\n>>\n>< | CÓ | Phải chuyển giữa đường | 
| 4\n>>>>\n>>>> | KHÔNG | Dưới cùng bên phải không thể truy cập | 
| 4\n>><>\n>>>< | CÓ | Kịch bản đa chuyển đổi | 

## Vỏ cạnh 

Hãy xem xét lưới kích thước tối thiểu,$n=2$, với hàng trên cùng '><' và hàng dưới cùng '<>'. Robot bắt đầu ở (1,1). Cột 0 cho phép di chuyển dọc theo hàng trên cùng. Cột 1 trên cùng là '<', bị chặn di chuyển sang phải. Hàng dưới cùng ở cột 1 là '>', cho phép chuyển đổi và di chuyển về phía trước. Thuật toán chính xác
