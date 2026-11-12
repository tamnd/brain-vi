---
title: "CF 1621D - Chuyến đi bộ mùa đông"
description: "Chúng ta có một lưới vuông có kích thước $2n nhân 2n$. Góc phần tư trên cùng bên trái, bao gồm các hàng $n$ đầu tiên và các cột $n$ đầu tiên, ban đầu chứa chính xác một người bạn trên mỗi ô."
date: "2026-06-10T05:55:15+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1621
codeforces_index: "D"
codeforces_contest_name: "Hello 2022"
rating: 2100
weight: 1621
solve_time_s: 100
verified: false
draft: false
---

[CF 1621D - Chuyến đi bộ mùa đông](https://codeforces.com/problemset/problem/1621/D) 

**Xếp hạng:** 2100 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới vuông có kích thước$2n \times 2n$. Góc phần tư phía trên bên trái, bao gồm góc phần tư đầu tiên$n$hàng và đầu tiên$n$cột, ban đầu chứa chính xác một người bạn trên mỗi ô. Góc phần tư phía dưới bên phải, góc phần tư cuối cùng$n$hàng và cuối cùng$n$cột, cuối cùng phải chứa chính xác một người bạn trên mỗi ô. Một số ô bên ngoài góc phần tư trên cùng bên trái bị bao phủ bởi tuyết và không thể vượt qua trừ khi tuyết được loại bỏ với một mức giá nhất định. Mục tiêu là tìm ra chi phí tối thiểu để loại bỏ tuyết để tất cả bạn bè có thể đến góc phần tư phía dưới bên phải chỉ bằng cách di chuyển hàng và cột theo chu kỳ. 

Vấn đề đặt ra là bạn bè có thể được di chuyển dọc theo hàng hoặc cột theo cách tuần hoàn: di chuyển qua hàng hoặc cột cuối cùng bao quanh hàng hoặc cột đầu tiên. Tuy nhiên, nếu một người bạn bước lên một phòng giam phủ đầy tuyết, họ sẽ bị ốm, vì vậy tất cả tuyết trên đường di chuyển phải được loại bỏ trước. 

Những hạn chế chính đó là$1 \le n \le 250$và tổng số tiền của$n$trên các trường hợp thử nghiệm nhiều nhất là 250. Điều này có nghĩa là một giải pháp lặp lại trên tất cả$2n \times 2n$các ô trong mỗi trường hợp thử nghiệm là khả thi. Chi phí để dọn tuyết có thể lớn bằng$10^9$, vì vậy chúng ta phải tránh những hoạt động không cần thiết để tránh bội chi. 

Một điểm tinh tế là chỉ có góc phần tư phía dưới bên phải mới quan trọng đối với vị trí cuối cùng của bạn bè, nhưng tuyết có thể bị trôi qua trong các lần di chuyển theo chu kỳ ở phần đầu tiên hoặc phần cuối cùng.$n$các hàng/cột bên ngoài góc phần tư trên cùng bên trái cũng có thể chặn chuyển động. Một cách tiếp cận bất cẩn có thể cố gắng dọn sạch tất cả tuyết ở góc phần tư phía dưới bên phải, điều này là không cần thiết. Thay vào đó, chúng ta chỉ cần xem xét các ô liền kề với góc phần tư trên cùng bên trái có thể chặn đường dẫn "lối vào" vào góc phần tư phía dưới bên phải. 

Ví dụ, nếu$n=1$, lưới là$2 \times 2$. Phía trên bên trái có một người bạn ở$(1,1)$và phía dưới bên phải là$(2,2)$. Chỉ có các tế bào$(1,2)$,$(2,1)$, Và$(2,2)$có thể phù hợp với chi phí. Một cách tiếp cận ngây thơ khi xem xét tất cả tuyết ở góc phần tư phía dưới bên phải sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng việc di chuyển tất cả bạn bè dọc theo mọi chuỗi di chuyển theo hàng và cột theo chu kỳ có thể có, theo dõi ô nào bị tuyết chặn. Điều này đúng về mặt lý thuyết nhưng không khả thi về mặt tính toán vì số lượng các chuỗi có thể tăng theo cấp số nhân với$n$. Ngay cả việc kiểm tra tất cả các hoán vị của việc di chuyển hàng/cột cũng vượt xa$1$giới hạn lần thứ hai. 

Điểm mấu chốt là chi phí dọn tuyết tối thiểu có thể được xác định mà không cần mô phỏng từng người bạn. Bởi vì những người bạn không thể phân biệt được và các bước di chuyển có tính chu kỳ, chỉ có tuyết có thể chặn "lối vào" từ góc phần tư trên cùng bên trái đến góc phần tư phía dưới bên phải mới quan trọng. Cụ thể, có chính xác bốn góc bên ngoài góc phần tư trên cùng bên trái có thể ảnh hưởng ngay lập tức đến khả năng chuyển bạn bè sang góc phần tư phía dưới bên phải thông qua việc di chuyển một hàng hoặc một cột. Những tế bào này là$(n, n+1)$,$(n+1, n)$,$(n+1, 2n)$, Và$(2n, n+1)$cho một$2n \times 2n$lưới. Chi phí tối thiểu chỉ đơn giản là tổng của tất cả chi phí dọn tuyết ở góc phần tư phía dưới bên phải (không thể tránh khỏi) cộng với chi phí tối thiểu trong số bốn ô “lối vào” này. 

Quan sát này làm giảm vấn đề từ tiềm năng$O((2n)^2)$kiểm tra một cách đơn giản$O(1)$tính toán cho mỗi trường hợp thử nghiệm về chi phí đầu vào, cộng với tổng tuyến tính trên góc phần tư phía dưới bên phải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((2n)^2 * n!) | O((2n)^2) | Quá chậm | 
| Tối ưu | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$và sau đó đọc$2n \times 2n$lưới chi phí dọn tuyết. 
3. Khởi tạo chi phí tối thiểu bằng 0. Điều này sẽ tích lũy chi phí để loại bỏ tất cả tuyết ở góc phần tư phía dưới bên phải. 
4. Lặp lại trên góc phần tư phía dưới bên phải (các hàng$n+1$ĐẾN$2n$, cột$n+1$ĐẾN$2n$) và cộng tất cả chi phí dọn tuyết. Điều này đảm bảo rằng mọi người bạn đều có ô cuối cùng hợp lệ ở góc phần tư phía dưới bên phải. 
5. Xác định bốn ô "lối vào" ngay bên ngoài góc phần tư trên cùng bên trái có thể chặn một người bạn di chuyển từ góc phần tư trên cùng bên trái sang góc phần tư dưới cùng bên phải: 

- Lối vào phía trên bên phải:$(1\text{ to } n, n+1)$- Lối vào phía dưới bên trái:$(n+1, 1\text{ to } n)$- Lối vào hàng dưới cùng bên phải:$(n+1, 2n)$- Lối vào cột dưới cùng bên phải:$(2n, n+1)$6. Tìm chi phí dọn tuyết tối thiểu trong số bốn ô vào này. Thêm chi phí này vào tổng từ bước 4. 
7. Xuất ra tổng chi phí tối thiểu cho mỗi test case. 

Tại sao nó hoạt động: Bạn bè không thể phân biệt được và chỉ có vị trí tương đối trong góc phần tư phía dưới bên phải mới quan trọng. Tuyết bên trong góc phần tư phía dưới bên phải phải luôn được dọn sạch. Bốn ô liền kề biểu thị lượng tuyết bổ sung tối thiểu được yêu cầu để cho phép di chuyển theo chu kỳ từ góc phần tư trên cùng bên trái sang góc phần tư dưới cùng bên phải. Bằng cách chọn mức tối thiểu trong số đó, chúng tôi đảm bảo rằng luôn có một con đường hợp lệ mà không phải chi tiêu quá mức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        grid = [list(map(int, input().split())) for _ in range(2*n)]
        
        # sum of all snow costs in bottom-right quadrant
        total = 0
        for i in range(n, 2*n):
            for j in range(n, 2*n):
                total += grid[i][j]
        
        # four "entrance" cells
        entrance_costs = [
            grid[0][n],     # row 1, column n+1
            grid[n-1][2*n-1], # row n, column 2n
            grid[n][0],     # row n+1, column 1
            grid[2*n-1][n-1] # row 2n, column n
        ]
        total += min(entrance_costs)
        print(total)

if __name__ == "__main__":
    solve()
```Mã này đọc tất cả các trường hợp kiểm thử và xây dựng$2n \times 2n$lưới. Nó tính tổng tất cả chi phí dọn tuyết ở góc phần tư phía dưới bên phải vì những ô này phải được để trống cho bạn bè chiếm giữ. Bốn ô lối vào được lập chỉ mục cẩn thận: một ô ở cột cuối cùng của góc phần tư trên cùng bên trái, một ô ở cột đầu tiên của góc phần tư phía dưới bên trái và hai ô còn lại ở các cạnh liền kề với góc phần tư phía dưới bên phải. Chúng tôi lấy chi phí tối thiểu trong số đó và cộng nó vào tổng phía dưới bên phải. 

## Ví dụ đã hoạt động 

**Ví dụ 1**: 

đầu vào:```
1
1
0 8
1 99
```Dấu vết bước: 

| Tế bào | Chi phí | Lý do | 
| --- | --- | --- | 
| (2,2) | 99 | Phải được xóa cho vị trí cuối cùng | 
| Số lần vào: (1,2)=8, (2,1)=1, (2,2)=99, (1,2)=8 | phút=1 | Chỉ cần một chi phí vào cửa tối thiểu | 

Tổng cộng = 99 + 1 = 100. Phù hợp với sản lượng dự kiến. 

**Ví dụ 2**: 

đầu vào:```
2
2
0 0 0 0
0 0 0 0
9 9 2 2
9 9 9 9
```Tổng góc phần tư dưới cùng bên phải: 2+2+9+9=22 

Các ô đầu vào: (1,3)=0, (2,4)=0, (3,1)=9, (4,2)=9 → min=0 

Tổng cộng = 22 + 0 = 22. Phù hợp với sản lượng dự kiến. 

Những dấu vết này cho thấy chỉ có góc phần tư phía dưới bên phải và bốn ô lối vào là có liên quan. Thuật toán bỏ qua tuyết không liên quan, giảm vấn đề thành lựa chọn thời gian không đổi cho lối vào cộng với tổng tuyến tính trên góc phần tư. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Tính tổng góc phần tư phía dưới bên phải yêu cầu$n \times n$lần lặp lại. Kiểm tra bốn ô vào là O(1). | 
| Không gian | O(n^2) | Chúng tôi lưu trữ đầy đủ$2n \times 2n$lưới cho từng trường hợp thử nghiệm. | 

Với$n \le 250$và tổng của$n \le 250$trên tất cả các trường hợp thử nghiệm, tổng số thao tác nhiều nhất là$2*250*250 = 125,000$, vừa vặn thoải mái trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("4\n1\n0 8\n1 99\n2\n0 0 0 0\n0 0 0 0\n9 9 2 2\n
```
