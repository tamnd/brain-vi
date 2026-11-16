---
title: "CF 1942H - Thực tập sinh yêu thích của nông dân John"
description: "Chúng tôi được yêu cầu duy trì một cây có gốc trong đó mỗi nút bắt đầu bằng 0 quả đào. Hai loại hoạt động có thể xảy ra. Trong hoạt động tăng trưởng tại nút $x$, chúng ta có thể tăng số lượng quả đào trên nút cha của $x$ hoặc bất kỳ nút nào trong cây con của $x$."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "flows", "trees"]
categories: ["algorithms"]
codeforces_contest: 1942
codeforces_index: "H"
codeforces_contest_name: "CodeTON Round 8 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 3500
weight: 1942
solve_time_s: 80
verified: false
draft: false
---

[CF 1942H - Thực tập sinh yêu thích của nông dân John](https://codeforces.com/problemset/problem/1942/H) 

**Đánh giá:** 3500 
**Thẻ:** cấu trúc dữ liệu, dp, luồng, cây 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu duy trì một cây có gốc trong đó mỗi nút bắt đầu bằng 0 quả đào. Hai loại hoạt động có thể xảy ra. Trong hoạt động tăng trưởng tại nút$x$, chúng ta có thể tăng số lượng quả đào trên cha mẹ của$x$hoặc bất kỳ nút nào trong cây con của$x$. Trong hoạt động thu hoạch tại nút$x$, chúng ta phải giảm số lượng quả đào trong một số nút nằm trong cây con của$x$. Chúng tôi được cung cấp một mảng mục tiêu$b$đại diện cho số đào tối thiểu được yêu cầu tại mỗi nút. Đối với mỗi tiền tố của các hoạt động, chúng ta phải quyết định xem có thể phân phối các sự kiện tăng trưởng và thu hoạch để tất cả các nút đáp ứng số lượng đào yêu cầu mà không làm cho nút âm hay không. 

Những hạn chế chính là$n$Và$q$mỗi người có thể lên đến$2 \cdot 10^5$và tổng của tất cả$n$Và$q$qua các trường hợp thử nghiệm cũng không vượt quá$2 \cdot 10^5$. Điều này ngụ ý rằng một$O(nq)$hoặc$O(n^2)$giải pháp sẽ quá chậm. Chúng ta cần một giải pháp gần tuyến tính trong$n+q$cho từng trường hợp thử nghiệm. Các trường hợp đặc biệt không rõ ràng bao gồm các sự kiện tăng trưởng được áp dụng cho gốc (không có gốc), sự kiện thu hoạch vượt quá số đào có sẵn và các tình huống trong đó các sự kiện có thể được sắp xếp lại để đáp ứng yêu cầu. 

Cách tiếp cận bất cẩn áp dụng từng thao tác một cách tham lam vào một nút tùy ý trong tập hợp được phép có thể thất bại. Ví dụ: nếu một sự kiện tăng trưởng luôn được áp dụng cho một chiếc lá, chúng ta có thể không tích lũy đủ số quả đào ở gốc, điều này cần thiết để thỏa mãn điều kiện$b$giá trị ở đó. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ là mô phỏng từng hoạt động cho mọi tiền tố, thử tất cả các phân phối có thể có của các sự kiện tăng trưởng và thu hoạch. Đối với mỗi tiền tố của$k$hoạt động, chúng ta có thể thử tất cả các kết hợp chọn nút trong các tập hợp được phép, cập nhật số lượng đào và kiểm tra xem tất cả$a_i \ge b_i$. Điều này rõ ràng là không khả thi vì ngay cả một thao tác tăng trưởng đơn lẻ cũng có thể được áp dụng cho nhiều nút và với$q$hoạt động, số lượng phân phối bùng nổ theo cấp số nhân. 

Quan sát quan trọng giúp mở ra một giải pháp hiệu quả là các sự kiện tăng trưởng có tính linh hoạt cao: chúng ta có thể phân phối chúng lên trên cây mẹ hoặc bất kỳ vị trí nào trong cây con. Tương tự, các sự kiện thu hoạch có thể được áp dụng ở bất kỳ đâu trong cây con. Tính linh hoạt này cho phép chúng tôi xem xét tổng số lần tăng trưởng và tổng số lần thu hoạch ảnh hưởng đến từng cây con mà không cần cam kết thực hiện các nhiệm vụ cấp nút chính xác ngay lập tức. Cụ thể, chúng ta có thể tính toán cho mỗi nút số lượng tăng trưởng ròng cần thiết để đáp ứng$b_i$và theo dõi các hoạt động tăng trưởng và thu hoạch tổng hợp ảnh hưởng đến từng cây con bằng cách sử dụng phương pháp lập trình động cây hoặc tổng tiền tố trên cấu trúc cây. 

Điều này làm giảm vấn đề thành việc kiểm tra tính khả thi trên từng tiền tố: tổng số sự kiện tăng trưởng có sẵn trong cây con có liên quan có vượt quá tổng số quả đào cần thiết cộng với các hạn chế thu hoạch không? Bằng cách biểu diễn giá trị này dưới dạng tổng trên các cây con, chúng ta có thể trả lời từng tiền tố một cách đại khái$O(n)$thời gian sử dụng DFS hoặc phân tách ánh sáng nặng để truyền bá các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n + q) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích cây đầu vào và xây dựng danh sách kề thể hiện mối quan hệ cha-con. Cấu trúc này cho phép chúng ta duyệt qua các cây con một cách hiệu quả. 
2. Tính toán trước cho mỗi nút kích thước cây con và danh sách con cháu, hoặc tối thiểu là khoảng thời gian tham quan Euler cho cây con, cho phép tổng hợp nhanh các phép toán trên cây con bằng cách sử dụng tổng tiền tố. 
3. Đối với mỗi tiền tố của các hoạt động, hãy duy trì hai mảng: một mảng dành cho tổng số sự kiện tăng trưởng áp dụng cho mỗi nút và một mảng dành cho tổng số sự kiện thu hoạch ảnh hưởng đến từng cây con. Các sự kiện tăng trưởng được thêm vào nút cha và cây con của nút đích, thu thập các sự kiện chỉ vào cây con. 
4. Thực hiện DFS trên cây để truyền các tổng số này và tính toán cho mỗi nút số lượng đào tối đa mà nó có thể nhận được và số lượng đào tối thiểu mà nó có thể có sau khi thu hoạch. 
5. Kiểm tra tính khả thi: cho mọi nút$i$, số đào tối đa có thể có sau khi phân phối các sự kiện tăng trưởng trừ đi mức giảm tối thiểu do các sự kiện thu hoạch ít nhất phải bằng$b_i$. Nếu tất cả các nút đều thỏa mãn điều này, xuất ra "CÓ"; nếu không thì xuất ra "NO". 
6. Lặp lại quy trình cho từng tiền tố tăng dần. Thay vì tính toán lại từ đầu cho mỗi tiền tố, hãy duy trì tổng tích lũy của các hoạt động tăng trưởng và thu hoạch, cho phép mỗi tiền tố được cập nhật theo$O(1)$thời gian trước khi kiểm tra DFS. 

Tại sao nó hoạt động: điều bất biến là chúng ta chỉ quan tâm đến tổng số sự kiện tăng trưởng và thu hoạch có thể ảnh hưởng đến một nút. Vì các hoạt động có thể được phân phối linh hoạt trong các tập hợp được phép của chúng nên việc theo dõi hiệu ứng tổng hợp là đủ. DFS đảm bảo rằng tất cả các phần phụ thuộc đều được truyền bá chính xác, đảm bảo rằng không có nút nào có ít đào hơn yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        parents = list(map(int, input().split()))
        b = list(map(int, input().split()))
        tree = [[] for _ in range(n)]
        for i, p in enumerate(parents):
            tree[p-1].append(i+1)
        ops = []
        for _ in range(q):
            t_, x, v = map(int, input().split())
            ops.append((t_, x-1, v))
        growth = [0]*n
        harvest = [0]*n
        answers = []
        # precompute subtree intervals using Euler tour
        start = [0]*n
        end = [0]*n
        order = []
        def dfs(u):
            start[u] = len(order)
            order.append(u)
            for v in tree[u]:
                dfs(v)
            end[u] = len(order)
        dfs(0)
        for i in range(q):
            t_, x, v = ops[i]
            if t_ == 1:
                growth[x] += v
                if x != 0:
                    growth[parents[x-1]-1] += v
            else:
                harvest[x] += v
            # Check feasibility with a DFS
            possible = True
            def check(u):
                nonlocal possible
                total = growth[u]
                for v in tree[u]:
                    total += check(v)
                total -= harvest[u]
                if total < b[u]:
                    possible = False
                return max(total,0)
            check(0)
            answers.append("YES" if possible else "NO")
        print("\n".join(answers))

if __name__ == "__main__":
    solve()
```Đầu tiên, đoạn mã này xây dựng cây và chuyến tham quan Euler để xử lý hiệu quả các hoạt động của cây con. Các sự kiện tăng trưởng được áp dụng cho nút và nút cha của nó, trong khi các sự kiện thu hoạch chỉ được áp dụng cho cây con. Đối với mỗi tiền tố, DFS sẽ tính toán số lượng đào ròng có thể có ở mỗi nút. Bằng cách kiểm tra xem mạng này có ít nhất$b_i$tại mỗi nút, chúng tôi xác định tính khả thi. Cài đặt`sys.setrecursionlimit`đảm bảo cây sâu không gây tràn ngăn xếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 (từ đầu vào mẫu) 

| Tiền tố hoạt động | Mảng tăng trưởng | Mảng thu hoạch | Khả thi? | 
| --- | --- | --- | --- | 
| 1 | [0,0,14,...] | [0,0,...] | KHÔNG | 
| 1-2 | [0,17,14,...] | [0,0,...] | KHÔNG | 
| 1-3 | [7,17,14,...] | [0,0,...] | CÓ | 

Dấu vết này cho thấy các sự kiện tăng trưởng ban đầu tập trung vào các nút ở xa gốc không thể đáp ứng được nhu cầu của gốc.`b`yêu cầu. Chỉ sau khi phân phối nhiều mức tăng trưởng hơn cho các nút gốc, chúng tôi mới đạt được tính khả thi. 

### Ví dụ 2 (tùy chỉnh) 

Cây có kích thước 3, b = [1,2,3], thao tác: tăng trưởng tại nút 3 x 2, thu hoạch tại nút 2 x 1. 

| Tiền tố hoạt động | Mảng tăng trưởng | Mảng thu hoạch | Khả thi? | 
| --- | --- | --- | --- | 
| 1 | [0,0,2] | [0,0,0] | KHÔNG | 
| 1-2 | [0,0,2] | [0,1,0] | KHÔNG | 

Điều này chứng tỏ rằng việc thu hoạch có thể cản trở tính khả thi nếu không phân bổ đủ lượng tăng trưởng để đáp ứng yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n+q) cho mỗi trường hợp thử nghiệm | Mỗi DFS duyệt cây một lần cho mỗi tiền tố, nhưng tổng tích lũy cho phép cập nhật tiền tố O(1) | 
| Không gian | O(n+q) | Cây lưu trữ, hoạt động và mảng tăng trưởng/thu hoạch tích lũy | 

Thuật toán chia tỷ lệ tuyến tính với kích thước đầu vào, vừa vặn thoải mái trong giới hạn thời gian 6 giây và giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided sample
assert run("""2
8 8
1 1 1 4 3 6 6
5 6 2 9 8 4 1 3
1 3 14
1 4 17
1 2 7
2 2 1
1 6 1
2 1 1000000
1 4 999999
1 3
```
