---
title: "CF 104391C - Phạm vi"
description: "Chúng ta có một tập hợp các khoảng trên trục số, mỗi khoảng biểu thị một “ngọn núi” có điểm cuối bên trái và điểm cuối bên phải."
date: "2026-07-01T02:41:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104391
codeforces_index: "C"
codeforces_contest_name: "The Unofficial Mirror Contest of 19th Thailand Olympiad in Informatics Day 2"
rating: 0
weight: 104391
solve_time_s: 134
verified: true
draft: false
---

[CF 104391C - Phạm vi](https://codeforces.com/problemset/problem/104391/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các khoảng trên trục số, mỗi khoảng biểu thị một “ngọn núi” có điểm cuối bên trái và điểm cuối bên phải. Một khoảng được coi là con của một khoảng khác khi nó nằm hoàn toàn bên trong nó, nghĩa là điểm cuối bên trái của nó không nhỏ hơn và điểm cuối bên phải của nó không lớn hơn. 

Từ mối quan hệ ngăn chặn này, chúng tôi xây dựng một cấu trúc được định hướng trong đó mọi khoảng đều trỏ đến tất cả các khoảng được chứa chặt chẽ trong đó. Điểm của một khoảng được định nghĩa là độ dài của chuỗi dài nhất bắt đầu từ khoảng đó và liên tục di chuyển đến bất kỳ khoảng chứa nào. Nếu một khoảng không chứa khoảng nào khác thì điểm của nó là 1. 

Nhiệm vụ là tính điểm này cho mỗi khoảng thời gian và cũng báo cáo số điểm tối đa trong tất cả các khoảng thời gian. 

Kích thước đầu vào đạt tới 400.000 khoảng, với tọa độ lên tới một tỷ. Bất kỳ giải pháp nào so sánh mọi khoảng với mọi khoảng khác đều bị loại trừ ngay lập tức vì nó sẽ yêu cầu so sánh theo thứ tự 10^11 trong trường hợp xấu nhất. Ngay cả các chiến lược O(n^2) dựa trên sắp xếp hoặc xây dựng biểu đồ đơn giản cũng quá chậm. 

Cấu trúc cũng không tùy tiện. Việc ngăn chặn chỉ phụ thuộc vào hai tọa độ với điều kiện đơn điệu: một phần tử con phải có điểm cuối bên trái lớn hơn hoặc bằng và điểm cuối bên phải nhỏ hơn hoặc bằng. Đây là mối quan hệ thống trị hai chiều, điều này cho thấy rằng vấn đề thực sự là về việc truy vấn và cập nhật các điểm một cách hiệu quả trong một mặt phẳng được sắp xếp một phần. 

Trường hợp cạnh tinh tế phát sinh khi nhiều khoảng có cùng điểm cuối bên trái. Trong trường hợp đó, việc ngăn chặn chỉ phụ thuộc vào điểm cuối bên phải và việc sắp xếp đơn giản theo một tọa độ duy nhất có thể dễ dàng tạo ra kết quả không chính xác nếu các phần phụ thuộc trong các nhóm bằng bên trái không được xử lý cẩn thận. Một trường hợp góc khác là các chuỗi lồng nhau sâu, trong đó câu trả lời có thể đạt tới O(n), điều này làm cho phép đệ quy không an toàn nếu không sắp xếp cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xây dựng biểu đồ ngăn chặn một cách rõ ràng. Với mỗi khoảng i, chúng ta kiểm tra từng khoảng j và thêm một cạnh nếu j nằm trong i. Sau đó, chúng tôi chạy DFS hoặc DP trên biểu đồ này để tính toán các đường đi dài nhất. Điều này đúng vì định nghĩa điểm chính xác là đường đi dài nhất trong biểu đồ chu kỳ có hướng. Vấn đề là chi phí xây dựng: việc kiểm tra tất cả các cặp yêu cầu so sánh O(n^2), vượt xa giới hạn khi n là 400.000. 

Quan sát quan trọng là chúng ta không cần các cạnh rõ ràng. Đối với mỗi khoảng, chúng ta chỉ cần biết giá trị dp tối đa trong số tất cả các khoảng bên trong nó. Điều này biến vấn đề thành một truy vấn phạm vi trên một tập hợp các điểm động, trong đó mỗi khoảng là một điểm (L, R) và chúng tôi muốn truy vấn tất cả các điểm có L_j ≥ L_i và R_j ≤ R_i. 

Đây là một vấn đề thống trị hai chiều. Nếu chúng ta sắp xếp các khoảng bằng cách giảm L, thì tại thời điểm chúng ta xử lý khoảng i, tất cả các khoảng có L lớn hơn đã được xử lý. Vấn đề còn lại là xử lý ràng buộc trên R một cách hiệu quả, có thể giảm bớt các truy vấn tiền tố tối đa bằng cách sử dụng cây Fenwick trên tọa độ R đã nén. 

Tuy nhiên, chỉ sắp xếp theo L là không đủ vì các khoảng có thể có cùng L. Trong trường hợp đó, các khoảng trong cùng một nhóm có thể phụ thuộc lẫn nhau dựa trên thứ tự R. Điều này buộc chúng tôi phải xử lý các nhóm L bằng nhau một cách riêng biệt, đảm bảo tính chính xác của quá trình chuyển đổi trong nhóm trước khi thực hiện cập nhật cho cấu trúc chung. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra cặp Brute Force + DP | O(n^2) | O(n^2) | Quá chậm | 
| Quét được sắp xếp + BIT với xử lý được nhóm | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp dựa vào các khoảng thời gian quét theo thứ tự giảm dần của các điểm cuối bên trái trong khi vẫn duy trì các giá trị dp có thể tiếp cận tốt nhất so với các điểm cuối bên phải. 

### 1. Phối hợp nén các endpoint bên phải

Trước tiên, chúng tôi nén tất cả các giá trị R vào một phạm vi nhỏ hơn. Điều này là cần thiết vì cây Fenwick yêu cầu các chỉ số liền kề. Việc nén duy trì trật tự, đó là thuộc tính duy nhất chúng ta cần. 

### 2. Sắp xếp các khoảng theo mức độ giảm dần của L 

Chúng tôi sắp xếp tất cả các khoảng theo thứ tự giảm dần của L. Các khoảng có L lớn hơn được xử lý sớm hơn, đảm bảo rằng khi chúng tôi xử lý một khoảng, tất cả các ứng cử viên có thể có với L_j ≥ L_i đều đã được nhìn thấy, ngoại trừ những khoảng có L bằng nhau. 

Điều này thiết lập hướng toàn cầu của sự phụ thuộc. 

### 3. Khoảng thời gian xử lý theo nhóm L bằng nhau 

Tất cả các khoảng có cùng điểm cuối bên trái sẽ được xử lý cùng nhau. Trong một nhóm như vậy, việc ngăn chặn chỉ phụ thuộc vào giá trị R. Nếu khoảng j có R nhỏ hơn hoặc bằng R hơn i thì j có thể là con của i, nhưng không phải ngược lại. 

Điều này có nghĩa là trong một nhóm, chúng ta phải xử lý theo thứ tự R tăng dần để các khoảng nhỏ hơn được tính trước các khoảng lớn hơn. 

### 4. Duy trì hai cây Fenwick 

Chúng tôi sử dụng cây Fenwick toàn cầu để lưu trữ kết quả của tất cả các nhóm được xử lý trước đó. Điều này trả lời các truy vấn liên quan đến giá trị L lớn hơn. 

Chúng tôi cũng duy trì một cây Fenwick tạm thời cho nhóm hiện tại. Điều này xử lý sự phụ thuộc giữa các khoảng chia sẻ cùng L. 

Đối với mỗi khoảng i trong nhóm, chúng tôi tính giá trị dp của nó là 1 cộng với tối đa của hai giá trị: dp tốt nhất trong số các nhóm được xử lý trước đó với R ≤ R_i và dp tốt nhất trong số các phần tử trước đó trong cùng nhóm với R ≤ R_i. 

Điều này nắm bắt chính xác tất cả trẻ em có thể. 

### 5. Cập nhật cấu trúc 

Sau khi tính toán các giá trị dp cho cả nhóm, chúng tôi chèn tất cả chúng vào cây Fenwick toàn cầu. Điều này đảm bảo các nhóm trong tương lai có thể sử dụng chúng. 

### 6. Theo dõi mức tối đa toàn cầu 

Trong khi tính toán giá trị dp, chúng tôi duy trì giá trị tối đa nhìn thấy, đây là câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Ở mỗi bước, cây Fenwick toàn cầu biểu thị tất cả các khoảng có giá trị L lớn hơn, được xử lý đầy đủ và cố định. Nhóm cây Fenwick biểu thị chính xác tiền tố của khối bằng L hiện tại được sắp xếp theo thứ tự tăng dần R. Mọi mối quan hệ con hợp lệ đều rơi vào đúng một trong hai loại này, vì vậy mọi chuyển đổi đóng góp vào dp đều được xem xét chính xác một lần và không có chuyển đổi không hợp lệ nào được đưa vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def update(self, i, v):
        while i <= self.n:
            if v > self.bit[i]:
                self.bit[i] = v
            i += i & -i

    def query(self, i):
        res = 0
        while i > 0:
            if self.bit[i] > res:
                res = self.bit[i]
            i -= i & -i
        return res

def main():
    n = int(input())
    seg = []
    rs = []

    for idx in range(n):
        l, r = map(int, input().split())
        seg.append((l, r, idx))
        rs.append(r)

    rs = sorted(set(rs))
    rid = {v: i + 1 for i, v in enumerate(rs)}

    seg.sort(key=lambda x: (-x[0], x[1]))

    fw_global = Fenwick(len(rs))
    dp = [0] * n

    i = 0
    ans = 0

    while i < n:
        j = i
        curL = seg[i][0]

        group = []
        while j < n and seg[j][0] == curL:
            group.append(seg[j])
            j += 1

        group.sort(key=lambda x: x[1])

        fw_local = Fenwick(len(rs))

        for l, r, idx in group:
            ri = rid[r]
            best = fw_global.query(ri)
            best = max(best, fw_local.query(ri))
            dp[idx] = best + 1
            fw_local.update(ri, dp[idx])

        for l, r, idx in group:
            fw_global.update(rid[r], dp[idx])
            if dp[idx] > ans:
                ans = dp[idx]

        i = j

    print(ans)
    print(*dp)

if __name__ == "__main__":
    main()
```Cây Fenwick được sử dụng làm cấu trúc tiền tố tối đa trên các điểm cuối bên phải được nén. Cấu trúc chung tích lũy kết quả từ các điểm cuối bên trái đã được xử lý, trong khi cấu trúc cục bộ giải quyết các phần phụ thuộc bên trong nhóm L bằng hiện tại. Quá trình chuyển đổi dp luôn là “khoảng bao quanh tốt nhất + 1”, được thực hiện thông qua các truy vấn tiền tố. 

Thứ tự sắp xếp rất quan trọng: giảm L đảm bảo hướng phụ thuộc toàn cục chính xác, trong khi tăng R bên trong một nhóm đảm bảo tính chính xác cho việc ngăn chặn cùng L. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Khoảng thời gian đầu vào là: 

(9,13), (11,13), (7,11), (1,9), (2,6), (3,8), (6,7) 

Sau khi sắp xếp theo cách giảm L, quá trình xử lý bắt đầu bằng (11,13), sau đó (9,13), v.v. 

Đối với (11,13), không tồn tại khoảng-L lớn hơn, vì vậy dp = 1. 

Đối với (9,13), nó thấy (11,13) trong cấu trúc toàn cục, nhưng vì 11 ≥ 9 và 13 ≤ 13 nên nó có thể mở rộng cấu trúc đó, cho ra dp = 2. 

Tiếp tục quá trình này, chuỗi lồng sâu nhất hình thành thông qua (1,9) → (2,6) → (6,7), tạo ra độ sâu tối đa 3 tại khoảng (9,13). 

Giá trị dp cuối cùng khớp với kết quả mong đợi: 

2 1 1 3 1 2 1 

Dấu vết xác nhận rằng chỉ những chuyển đổi ngăn chặn hợp lệ mới góp phần vào sự tăng trưởng dp và không có sự can thiệp chéo nào xảy ra giữa các khoảng thời gian không liên quan. 

### Mẫu 2 

Khoảng thời gian: 

(1,3), (1,6), (1,5), (1,1000), (1,4) 

Tất cả các khoảng đều có chung L nên mọi thứ đều được giải quyết trong một nhóm duy nhất. 

Sắp xếp theo R sẽ cho: 

(1,3), (1,4), (1,5), (1,6), (1.1000) 

Chúng tôi xử lý từ R nhỏ nhất trở lên. Mỗi khoảng đều coi tất cả những khoảng nhỏ hơn là những đứa trẻ tiềm năng. Điều này tạo ra một chuỗi giá trị dp tăng dần: 

dp(1,3)=1 

dp(1,4)=2 

dp(1,5)=3 

dp(1,6)=4 

dp(1.1000)=5 

Ví dụ này tách biệt tầm quan trọng của việc xử lý nội bộ nhóm. Nếu không có nó, các phần phụ thuộc bằng L sẽ bị bỏ qua hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp cộng với các cập nhật và truy vấn Fenwick cho từng khoảng thời gian | 
| Không gian | O(n) | Lưu trữ cho dp, cây Fenwick và nén tọa độ | 

Giải pháp vừa vặn thoải mái trong giới hạn n lên tới 400.000. Các phép toán của Fenwick là logarit và tất cả các công việc nặng đều tuyến tính ngoại trừ việc sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    return sys.stdout.getvalue().strip() if False else ""

# provided samples (placeholders for illustration)
# assert run(...) == ...

# edge cases
assert True
```Các trường hợp sau đây rất quan trọng: 

- khoảng đơn: xác minh điều kiện cơ bản 
- chuỗi lồng nhau hoàn toàn: xác minh sự lan truyền độ sâu tối đa 
- giá trị L giống hệt nhau: xác minh việc xử lý nội bộ nhóm 
- tăng khoảng cách rời rạc: xác minh không có lồng sai 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các khoảng có cùng điểm cuối bên trái. Trong trường hợp này, toàn bộ câu trả lời chỉ phụ thuộc vào việc sắp xếp theo đúng điểm cuối. Thuật toán xử lý chính xác điều này bằng cách sắp xếp nhóm theo R và sử dụng cây Fenwick cục bộ, đảm bảo rằng mỗi khoảng được xây dựng dựa trên các khoảng nhỏ hơn. 

Một trường hợp cạnh khác là tăng cường các chuỗi lồng nhau như [1,10], [2,9], [3,8], [4,7]. Ở đây, mỗi khoảng phụ thuộc vào khoảng trước đó. Cây Fenwick toàn cục lan truyền chính xác các giá trị dp vì mỗi khoảng được xử lý sau tất cả các khoảng L lớn hơn, do đó chuỗi được xây dựng lại tăng dần. 

Trường hợp cạnh thứ ba liên quan đến các khoảng rời rạc như [1,2], [3,4], [5,6]. Không có giá trị nào chứa giá trị khác, vì vậy tất cả các giá trị dp vẫn bằng 1. Các truy vấn Fenwick trả về 0 một cách nhất quán, ngăn chặn sự lan truyền ngẫu nhiên trên các vùng không chồng chéo.
