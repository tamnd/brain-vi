---
title: "CF 104720K - Nhẫn Bánh Donut"
description: "Chúng tôi được đưa cho một số chiếc bánh rán, mỗi chiếc được mô tả bằng hai bán kính. Bán kính bên trong xác định một lỗ và bán kính bên ngoài xác định toàn bộ phạm vi của chiếc bánh rán. Một chiếc bánh rán có thể được đặt bên trong lỗ của một chiếc bánh rán khác nếu đường viền bên ngoài của nó vừa khít với lỗ đó."
date: "2026-06-29T07:13:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104720
codeforces_index: "K"
codeforces_contest_name: "UTPC x WiCS Contest 10-06-23"
rating: 0
weight: 104720
solve_time_s: 83
verified: false
draft: false
---

[CF 104720K - Nhẫn bánh rán](https://codeforces.com/problemset/problem/104720/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa cho một số chiếc bánh rán, mỗi chiếc được mô tả bằng hai bán kính. Bán kính bên trong xác định một lỗ và bán kính bên ngoài xác định toàn bộ phạm vi của chiếc bánh rán. Một chiếc bánh rán có thể được đặt bên trong lỗ của một chiếc bánh rán khác nếu đường viền bên ngoài của nó vừa khít với lỗ đó. Việc lồng này có thể tiếp tục, tạo thành một chuỗi trong đó mỗi chiếc bánh rán tiếp theo sẽ nằm gọn trong lỗ của chiếc bánh trước đó. 

Một “vòng bánh rán” hợp lệ là một chuỗi các bánh rán lồng nhau như vậy. Nếu chúng ta lấy một chuỗi, tổng đóng góp chỉ phụ thuộc vào bán kính: mỗi chiếc bánh rán đóng góp diện tích của nó, trong khi lỗ của cấu trúc ngoài cùng sẽ loại bỏ diện tích dưới dạng không gian trống. Sau khi đơn giản hóa hình học, mục tiêu giảm xuống còn tối đa hóa số lượng phụ thuộc tuyến tính vào bán kính bình phương của các bánh rán đã chọn theo thứ tự lồng hợp lệ. 

Cụ thể, đối với mỗi chiếc bánh rán, sự đóng góp vào giá trị cuối cùng có thể được biểu thị bằng$R_i^2 - r_i^2$, trong khi lồng nhau đưa ra các ràng buộc: nếu bánh rán A nằm trong bánh rán B, thì$R_A \le r_B$. Mục tiêu là chọn một chuỗi tối đa hóa điểm toàn cầu bắt nguồn từ các giá trị này, tôn trọng ràng buộc lồng nhau. 

Kích thước đầu vào$n \le 10^5$ngụ ý rằng bất kỳ giải pháp nào tồi tệ hơn$O(n \log n)$có nguy cơ hết thời gian. Một giải pháp bậc hai kiểm tra tất cả các cặp hoặc tất cả các chuỗi là không khả thi ngay lập tức vì nó đòi hỏi tới$10^{10}$hoạt động. 

Một số trường hợp khó nhận thấy có vấn đề: 

Một chiếc bánh rán duy nhất phải được xử lý chính xác. Nếu đó là lựa chọn duy nhất thì câu trả lời đơn giản là$R^2 - r^2$, có thể âm. Ví dụ, một chiếc bánh rán với$r = 5, R = 10$đóng góp$100 - 25 = 75$. 

Một trường hợp khác là khi tất cả các bánh rán đều “không tương thích” để lồng vào nhau, nghĩa là không có hai cái nào thỏa mãn.$R_i \le r_j$. Khi đó câu trả lời là giá trị đơn tối đa, không phải số 0 hoặc chuỗi trống. Ví dụ,$(r,R) = (1,10), (2,3)$đưa ra câu trả lời tốt nhất$\max(100-1, 9-4)$. 

Trường hợp thứ ba là khi nhiều chiếc bánh rán có bán kính giống hệt nhau hoặc gần giống nhau, điều này có thể phá vỡ những cách tiếp cận tham lam ngây thơ vốn giả định trật tự nghiêm ngặt mà không xử lý sự bình đẳng một cách cẩn thận. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ cố gắng xây dựng mọi chuỗi lồng nhau có thể. Đối với mỗi chiếc bánh rán, chúng ta có thể thử nó làm điểm bắt đầu và nối đệ quy bất kỳ chiếc bánh rán nào có bán kính bên ngoài khớp với bán kính bên trong hiện tại. Điều này tạo thành một DAG về các khả năng trong đó mỗi nút có thể phân nhánh tới nhiều nút khác. Trong trường hợp xấu nhất, mỗi chiếc bánh rán có thể vừa với nhiều chiếc bánh rán khác, dẫn đến sự tăng trưởng theo cấp số nhân trong các chuỗi được khám phá. Ngay cả việc cắt tỉa cũng không giúp được gì nhiều vì số lượng chuỗi hợp lệ vẫn có thể cực kỳ lớn. 

Quan sát quan trọng là việc lồng nhau áp đặt một điều kiện thứ tự nghiêm ngặt: nếu bánh rán A đi vào trong bánh rán B thì$R_A \le r_B$. Điều này chuyển vấn đề thành một cấu trúc tuần hoàn có hướng được sắp xếp theo bán kính, trong đó các chuyển đổi chỉ đi từ bán kính bên ngoài nhỏ hơn đến bán kính bên trong lớn hơn. Khi chúng tôi sắp xếp bánh rán theo khóa có ý nghĩa, chúng tôi có thể chuyển đổi vấn đề thành tối ưu hóa một chiều so với các khóa trước hợp lệ. 

Chúng tôi diễn giải lại từng chiếc bánh rán dưới dạng một ràng buộc và giá trị phân khúc. Nhiệm vụ trở thành việc chọn một chuỗi trong đó mỗi phần tử tiếp theo thỏa mãn một điều kiện khả thi đơn điệu và đóng góp một giá trị. Đây là cài đặt cổ điển để lập trình động trên các điểm cuối được sắp xếp, trong đó đối với mỗi chiếc bánh rán, chúng tôi tính toán kết thúc chuỗi tốt nhất ở đó, sử dụng tất cả các bánh rán tương thích trước đó. 

Quá trình chuyển đổi phụ thuộc vào việc tìm ra chiếc bánh rán tốt nhất trước đó có bán kính bên ngoài vừa với bán kính bên trong hiện tại. Sắp xếp theo bán kính bên trong cho phép chúng ta sử dụng tìm kiếm nhị phân hoặc cây Fenwick trên tọa độ nén của bán kính bên ngoài. Điều này làm giảm khả năng kiểm tra tính tương thích từ quét tuyến tính đến truy vấn logarit. 

Do đó, thay vì liệt kê các chuỗi, chúng tôi xử lý bánh rán theo thứ tự tăng dần và duy trì cấu trúc lưu trữ các giá trị tốt nhất có thể đạt được đối với các ngưỡng bán kính bên ngoài nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mỗi chiếc bánh rán thành một cặp tọa độ: điểm cuối ràng buộc và một giá trị. 

## Bước 1: Chuẩn hóa biểu diễn 

Đối với mỗi chiếc bánh rán, hãy tính giá trị đóng góp của nó$v_i = R_i^2 - r_i^2$. Chúng tôi cũng giữ ranh giới tương thích của nó: nó chỉ có thể đi theo những chiếc bánh rán có bán kính ngoài tối đa là$r_i$. 

Điều này làm cho mỗi chiếc bánh rán đồng thời là một “trạng thái” và một “ràng buộc”. 

## Bước 2: Sắp xếp theo ràng buộc 

Sắp xếp bánh rán bằng cách tăng bán kính bên trong$r_i$. Điều này đảm bảo rằng khi xử lý một chiếc bánh rán, tất cả các bánh trước tiềm năng (có yêu cầu bán kính ngoài nhỏ hơn hoặc bằng nhau) đều đã được xem xét. 

Lý do điều này có tác dụng là vì tính khả thi chỉ phụ thuộc vào việc so sánh bán kính bên ngoài của cái này với bán kính bên trong của cái khác, do đó việc sắp xếp theo bán kính bên trong sẽ tạo ra một thứ tự xử lý tự nhiên. 

## Bước 3: Phối hợp nén bán kính ngoài 

Thu thập tất cả$R_i$giá trị và nén chúng. Điều này cho phép chúng ta lập chỉ mục cho chúng trong cây Fenwick hoặc cây phân đoạn. 

Việc nén là cần thiết vì$R_i$có thể lên tới$10^9$, làm cho việc lập chỉ mục trực tiếp là không thể. 

## Bước 4: Cấu trúc lập trình động 

Duy trì một cấu trúc`best[x]`thể hiện vẻ đẹp tối đa có thể đạt được của một chuỗi hợp lệ có chiếc bánh rán cuối cùng có bán kính ngoài nhiều nhất là tọa độ nén$x$. 

Điều này biến khả năng tương thích thành truy vấn tối đa tiền tố. 

## Bước 5: Xử lý bánh donut theo thứ tự sắp xếp 

Cho mỗi chiếc bánh rán$i$, chúng tôi truy vấn chuỗi tốt nhất có thể đứng trước nó. Vì nó phải thỏa mãn$R_{prev} \le r_i$, chúng tôi truy vấn tất cả các trạng thái có bán kính ngoài lên tới$r_i$. 

Sau đó chúng tôi tính toán:$$dp_i = v_i + \max(dp \text{ among valid predecessors})$$Chúng tôi cũng cân nhắc việc bắt đầu một chuỗi mới chỉ với chiếc bánh rán này. 

Sau khi tính toán$dp_i$, chúng tôi cập nhật cấu trúc tại vị trí tương ứng với$R_i$. 

## Bước 6: Câu trả lời cuối cùng 

Câu trả lời là giá trị lớn nhất trong số tất cả$dp_i$. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi xử lý tất cả các bánh rán đạt chỉ mục$i$, cấu trúc lưu trữ các giá trị chuỗi tốt nhất có thể cho mọi ranh giới bán kính bên ngoài khả thi chỉ bằng cách sử dụng các bánh rán hợp lệ trước đó. Bởi vì thứ tự xử lý tuân theo bán kính bên trong tăng dần nên mọi tiền thân hợp lệ đều được đảm bảo đã được đưa vào. Vì quá trình chuyển đổi duy trì tính khả thi và chúng tôi luôn chọn phiên bản tiền nhiệm tốt nhất nên không có chuỗi tối ưu nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [-10**30] * (n + 1)

    def update(self, i, v):
        while i <= self.n:
            if v > self.bit[i]:
                self.bit[i] = v
            i += i & -i

    def query(self, i):
        res = -10**30
        while i > 0:
            if self.bit[i] > res:
                res = self.bit[i]
            i -= i & -i
        return res

def main():
    n = int(input())
    donuts = []
    Rs = []

    for _ in range(n):
        r, R = map(int, input().split())
        donuts.append((r, R))
        Rs.append(R)

    Rs_sorted = sorted(set(Rs))
    comp = {v: i + 1 for i, v in enumerate(Rs_sorted)}

    donuts.sort()

    fw = Fenwick(len(Rs_sorted))

    ans = -10**30

    for r, R in donuts:
        v = R * R - r * r
        # best chain ending with R_i <= r
        # need all previous with outer radius <= r
        # find index of r in compressed Rs
        # upper bound
        import bisect
        idx = bisect.bisect_right(Rs_sorted, r)
        best_prev = fw.query(idx)

        if best_prev < 0:
            best_prev = 0

        cur = best_prev + v
        ans = max(ans, cur)

        fw.update(comp[R], cur)

    print(ans)

if __name__ == "__main__":
    main()
```Giải pháp dựa vào việc duy trì mức tối đa tiền tố trên bán kính ngoài được nén. Cây Fenwick chỉ được sử dụng cho các truy vấn tối đa, do đó các bản cập nhật lưu trữ giá trị tối đa thay vì tổng. Điểm tinh tế là chuyển đổi tính khả thi thành điều kiện tiền tố thông qua việc sắp xếp và sau đó sử dụng tìm kiếm nhị phân để căn chỉnh các ràng buộc bán kính bên trong với hệ tọa độ nén. 

Một sai lầm dễ mắc phải là quên rằng chuỗi có thể bắt đầu ở bất kỳ chiếc bánh rán nào, vì vậy giá trị trước đó phải cho phép bằng 0. Điều đó được xử lý bằng cách kẹp các kết quả âm tính về 0 trước khi gia hạn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
```
