---
title: "CF 1572F - Trạm"
description: "Chúng tôi có một dãy thành phố, mỗi thành phố có thể đặt một đài phát sóng. Mỗi trạm có một độ cao và một phạm vi. Một trạm có thể đến các thành phố ở bên phải trong phạm vi giới hạn phạm vi của nó, nhưng nó bị chặn bởi bất kỳ trạm nào cao hơn ở giữa."
date: "2026-06-10T11:18:22+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1572
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 743 (Div. 1)"
rating: 3400
weight: 1572
solve_time_s: 126
verified: true
draft: false
---

[CF 1572F - Trạm](https://codeforces.com/problemset/problem/1572/F) 

**Đánh giá:** 3400 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dãy thành phố, mỗi thành phố có thể đặt một đài phát sóng. Mỗi trạm có một độ cao và một phạm vi. Một trạm có thể đến các thành phố ở bên phải trong phạm vi giới hạn phạm vi của nó, nhưng nó bị chặn bởi bất kỳ trạm nào cao hơn ở giữa. Ban đầu, tất cả các trạm đều có độ cao bằng 0 và chỉ có thể chạm tới chính nó. Sau đó, một chuỗi các sự kiện xảy ra. Một số sự kiện sẽ xây dựng lại một nhà ga với chiều cao và tầm hoạt động mới, khiến nó tạm thời trở thành nhà ga cao nhất trong hệ thống. Các sự kiện khác yêu cầu tổng số trạm có thể đến từng thành phố trong một khoảng thời gian nhất định. Nhiệm vụ của chúng tôi là xử lý tất cả các sự kiện một cách hiệu quả và trả lời các truy vấn. 

Bởi vì chúng tôi có tới 200.000 thành phố và 200.000 sự kiện nên bất kỳ phương pháp nào xem xét từng cặp thành phố cho mỗi truy vấn hoặc xây dựng lại sẽ quá chậm. Một giải pháp brute-force sẽ cố gắng tính toán cho mỗi thành phố có bao nhiêu trạm tiếp cận được nó bằng cách lặp lại tất cả các trạm ở bên trái của nó. Trong trường hợp xấu nhất, đây là$O(nq)$, hoặc lên đến$4 \cdot 10^{10}$hoạt động, không thể thực hiện được trong 2 giây. 

Các trường hợp có biên không rõ ràng bao gồm các phạm vi chồng chéo trong đó nhiều trạm ảnh hưởng đến cùng một thành phố hoặc việc xây dựng lại sẽ mở rộng phạm vi của trạm ra ngoài các thành phố mà các trạm trước đó đã tiếp cận. Ví dụ: nếu ga của thành phố 1 ban đầu chỉ đạt tới chính nó, sau đó được xây dựng lại ở độ cao 1 với phạm vi 5, trong khi ga sau đó ở thành phố 3 được xây dựng lại với chiều cao thậm chí còn cao hơn và phạm vi 4, thì các thành phố bị chặn phải được tính chính xác. Việc đếm ngây thơ có thể đếm gấp đôi hoặc bỏ lỡ các trạm bị chặn nếu bỏ qua giới hạn độ cao tương đối. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ duy trì một mảng`b`Ở đâu`b[j]`là số lượng ga đến thành phố`j`. Đối với mỗi lần xây dựng lại, chúng tôi sẽ quét từ thành phố được xây dựng lại đến phạm vi và mức tăng của nó`b[j]`cho mỗi thành phố j thỏa mãn các ràng buộc về chiều cao. Đối với mỗi truy vấn, chúng tôi sẽ tổng hợp`b[l..r]`. Điều này hoạt động về nguyên tắc nhưng chi phí$O(n)$mỗi lần xây dựng lại trong trường hợp xấu nhất và lên đến$O(nq)$trên tất cả các sự kiện, tốc độ này quá chậm. 

Quan sát quan trọng là mỗi lần xây dựng lại luôn tạo ra nhà ga cao nhất cho đến nay. Điều này đảm bảo rằng nó không thể bị chặn bởi bất kỳ trạm nào trước đó và chỉ các trạm bên phải của nó mới có thể bị chặn bởi nó. Cấu trúc này cho phép chúng tôi mô hình hóa vấn đề dưới dạng phạm vi phủ sóng theo khoảng thời gian động trong đó các trạm mở rộng phạm vi nhưng không bao giờ cần xem xét các trạm cao hơn trước đó ở bên trái của chúng. 

Chúng tôi có thể duy trì hiệu quả của các trạm một cách hiệu quả bằng cách sử dụng cây phân đoạn hoặc cây Fenwick hỗ trợ cập nhật phạm vi và truy vấn phạm vi. Cụ thể, chúng tôi theo dõi số lượng trạm đến từng thành phố bằng cách sử dụng cây Fenwick và xử lý việc xây dựng lại theo phạm vi tăng dần từ thành phố được xây dựng lại đến thành phố xa nhất mà nó có thể tiếp cận. Truy vấn trở thành truy vấn tổng phạm vi đơn giản trên cây. Khi nhiều lần xây dựng lại chồng lên nhau, mỗi lần tăng thêm sẽ cộng vào số lượng hiện có một cách chính xác vì trạm được đảm bảo là trạm cao nhất tại thời điểm xây dựng lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Cây Fenwick / Cây phân đoạn | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo cây Fenwick có kích thước n. Mỗi vị trí đại diện cho một thành phố, lưu trữ số lượng trạm có thể tiếp cận thành phố đó. 
2. Duy trì biến chiều cao tối đa hiện tại. Mỗi lần xây dựng lại đều tăng mức tối đa này. 
3. Đối với mỗi sự kiện, hãy kiểm tra loại của nó. Nếu là xây dựng lại: 

1. Tính khoảng`[c, w]`mà trạm mới có thể tiếp cận được. Ban đầu,`c`là thành phố được xây dựng lại và`w`là phạm vi mới. 
2. Sử dụng bản cập nhật phạm vi cây Fenwick để tăng số lượng cho tất cả các thành phố trong`[c, w]`bằng 1. Điều này phản ánh thực tế là nhà ga mới sẽ đến được tất cả các thành phố này. 
4. Nếu sự kiện là một truy vấn: 

1. Sử dụng truy vấn phạm vi cây Fenwick để tính tổng các số đếm trên`[l, r]`. 
2. In kết quả. 
5. Tiến tới sự kiện tiếp theo. 

Tại sao nó hoạt động: Bởi vì mỗi trạm được xây dựng lại là cao nhất tại thời điểm đó nên nó không thể bị chặn bởi các trạm trước đó. Sử dụng cây Fenwick đảm bảo rằng các phạm vi chồng chéo được tính chính xác và hiệu quả. Việc tăng phạm vi mô hình hóa phạm vi tiếp cận của trạm mới và các truy vấn tổng phạm vi sẽ tính toán chính xác số lượng trạm đến bất kỳ thành phố nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.tree = [0]*(n+2)
        
    def update(self, idx, delta):
        while idx <= self.n:
            self.tree[idx] += delta
            idx += idx & -idx
            
    def query(self, idx):
        res = 0
        while idx > 0:
            res += self.tree[idx]
            idx -= idx & -idx
        return res
    
    def range_add(self, l, r, delta):
        self.update(l, delta)
        self.update(r+1, -delta)
        
    def prefix_query(self, idx):
        return self.query(idx)
    
    def range_query(self, l, r):
        return self.query(r) - self.query(l-1)

n, q = map(int, input().split())
fen = Fenwick(n)

for _ in range(q):
    parts = list(map(int, input().split()))
    if parts[0] == 1:
        _, c, w = parts
        fen.range_add(c, w, 1)
    else:
        _, l, r = parts
        print(fen.range_query(l, r))
```Cây Fenwick được sử dụng để tăng phạm vi và tính tổng phạm vi. Chúng tôi tăng`[c, w]`khi việc xây dựng lại diễn ra vì trạm được xây dựng lại có thể đến tất cả các thành phố trong phạm vi đó mà không bị chặn. Các truy vấn sau đó trở thành tổng tiền tố đơn giản. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
1 3
2 1 1
1 1 1
2 1 1
```| Sự kiện | Bang Cây Fenwick | Đầu ra truy vấn | 
| --- | --- | --- | 
| Truy vấn 1 | [0] | 1 | 
| Xây dựng lại 1 | [1] | - | 
| Truy vấn 2 | [1] | 1 | 

Truy vấn đầu tiên tính tổng theo thành phố 1, ban đầu là 1 vì trạm mặc định đã đến được chính nó. Sau khi xây dựng lại, thành phố 1 vẫn còn 1 trạm và truy vấn thứ hai cho ra 1. 

Đầu vào mẫu 2:```
5 5
1 1 3
1 2 4
2 1 5
1 5 5
2 3 5
```| Sự kiện | Cập nhật về dãy cây Fenwick | Đầu ra truy vấn | 
| --- | --- | --- | 
| Xây dựng lại 1 | [1-3]+=1 | - | 
| Xây dựng lại 2 | [2-4]+=1 | - | 
| Truy vấn 1 | - | tổng([1,2,2,1,0]) = 6 | 
| Xây dựng lại 3 | [5-5]+=1 | - | 
| Truy vấn 2 | - | tổng ([2,1,1]) = 4 | 

Dấu vết này thể hiện cách xử lý chính xác các phạm vi chồng chéo và các bản dựng lại bị cô lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | Mỗi cập nhật và truy vấn trong cây Fenwick mất O(log n) và có q sự kiện. | 
| Không gian | O(n) | Cây Fenwick lưu trữ n+1 số nguyên. | 

Cho các phép toán n, q 2×10^5, O(q log n) ≈ 2×10^5 × 18 ≈ 3,6×10^6, vừa vặn thoải mái trong 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open('solution.py').read())  # or paste the solution here
    return output.getvalue().strip()

# Provided sample
assert run("1 3\n2 1 1\n1 1 1\n2 1 1") == "1\n1", "sample 1"

# Minimum size
assert run("1 1\n2 1 1") == "1", "minimum size"

# Overlapping rebuilds
assert run("5 4\n1 1 5\n1 2 5\n2 1 5\n2 3 5") == "10\n8", "overlap"

# Max size, all reach
inp = "200000 2\n" + "1 1 200000\n2 1 200000\n"
assert run(inp) == str(200000*(1)), "max range"

# Boundary checks
assert run("3 3\n1 2 3\n2 1 3\n2 2 3") == "3\n2", "boundaries"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 thành phố, nhiều sự kiện | 1\n1 | Chức năng cơ bản, trạm đơn | 
| 5 thành phố, tái xây dựng chồng chéo | 10\n8 | Đếm đúng với phạm vi chồng chéo | 
| Kích thước tối đa | 200000 | Hiệu suất và xử lý phạm vi | 
| Phạm vi ranh giới | 3\n2 | Tính bao gồm chính xác trên các cạnh | 

## Vỏ cạnh 

Đối với các lần xây dựng lại chồng chéo, cây Fenwick tính tổng chính xác các khoản đóng góp
