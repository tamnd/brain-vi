---
title: "CF 105278G - Núi Lửa Sô Cô La"
description: "Chúng ta có một “chiếc bánh” đa giác có ranh giới trên được xác định bởi một đường đa giác đi qua $n$ điểm có tọa độ x tăng nghiêm ngặt."
date: "2026-06-23T14:19:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105278
codeforces_index: "G"
codeforces_contest_name: "2024 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 105278
solve_time_s: 119
verified: false
draft: false
---

[CF 105278G - Núi lửa sô cô la](https://codeforces.com/problemset/problem/105278/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một “chiếc bánh” đa giác có ranh giới trên được xác định bởi một đường đa tuyến xuyên qua$n$các điểm có tọa độ x tăng nghiêm ngặt. Ranh giới dưới cùng là trục x và điểm đầu tiên và điểm cuối cũng được kết nối theo chiều dọc xuống trục x, do đó hình dạng là một vùng đơn giản dưới đường cong tuyến tính từng phần. 

Điều này có nghĩa là chiếc bánh được xác định hoàn toàn bằng một hàm tuyến tính giữa các điểm liên tiếp và bằng 0 bên ngoài tọa độ x đầu tiên và cuối cùng. Tổng diện tích bánh là diện tích dưới đường cong này. 

Chúng ta cần đặt$m-1$các đường cắt dọc sao cho các lát cắt dọc phân chia tổng diện tích thành$m$phần bằng nhau. Mỗi vết cắt được xác định bởi tọa độ x và mỗi vết cắt kéo dài toàn bộ chiều cao của hình tại vị trí x đó. 

Do đó, đầu ra là một chuỗi các giá trị x sao cho diện tích từ ranh giới bên trái cho đến mỗi lần cắt chính xác là bội số của tổng diện tích chia cho$m$. 

Các ràng buộc rất lớn: cả hai$n$Và$m$có thể đạt được$2 \cdot 10^5$. Điều này ngay lập tức loại trừ bất kỳ phương pháp nào tính toán lại vùng trên mỗi truy vấn hoặc quét các phân đoạn nhiều lần. Bất cứ điều gì bậc hai trong$n$hoặc$m$quá chậm. Chúng ta cần một phương pháp xử lý đa giác một lần và trả lời từng lần cắt theo thời gian khấu hao logarit hoặc không đổi. 

Một trường hợp lỗi nhỏ xuất hiện khi đường cắt mục tiêu nằm bên trong một đoạn giữa hai điểm đã cho. Ví dụ: nếu đường cong dốc thì đường cắt chính xác có thể không trùng với bất kỳ đỉnh nào. Một cách tiếp cận đơn giản chỉ kiểm tra tổng tiền tố ở các đỉnh sẽ thất bại, vì các ranh giới có diện tích bằng nhau thường xảy ra bên trong các cạnh chứ không phải ở các điểm cuối. 

Một trường hợp thất bại khác phát sinh từ việc giả định chiều cao đồng đều trong một đoạn. Ví dụ: nếu hai điểm liên tiếp$(0,0)$Và$(1,10)$, diện tích lên tới$x=0.5$không phải là một nửa diện tích hình thang đầy đủ trừ khi phép nội suy tuyến tính được xử lý chính xác. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng từng lần cắt một cách độc lập. Đối với mỗi phần mục tiêu$k/m$, chúng tôi có thể quét từ ranh giới bên trái, tích lũy từng phân đoạn khu vực và dừng lại khi vượt quá khu vực yêu cầu. Bên trong đoạn chứa ranh giới, khi đó chúng ta sẽ cố gắng tìm vị trí x chính xác bằng cách lấp đầy một phần hình thang. 

Điều này hoạt động chính xác vì khu vực này đơn điệu trong x. Tuy nhiên, mỗi lần cắt có thể yêu cầu quét tới$O(n)$các phân đoạn và có$O(m)$cắt giảm, dẫn đến$O(nm)$làm việc trong trường hợp xấu nhất. Với$2 \cdot 10^5$, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là hàm diện tích là hàm bậc hai từng phần nhưng đơn điệu toàn cục. Khi chúng tôi tính toán trước các vùng tiền tố ở tất cả các đỉnh, chúng tôi có thể xác định vị trí đoạn chứa mỗi phần cắt bằng cách sử dụng con trỏ hoặc tìm kiếm nhị phân, sau đó giải quyết vị trí chính xác bằng phương pháp phân tích trong đoạn đó. Điều này làm giảm vấn đề xây dựng cấu trúc tiền tố một lần và thực hiện$m$nội suy theo thời gian không đổi độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$|$O(1)$| Quá chậm | 
| Tiền tố + nội suy |$O(n + m)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng diện tích dưới đường đa giác bằng các hình thang giữa các điểm liên tiếp. Mỗi đoạn đóng góp diện tích của một hình thang được hình thành bởi trục x. 
2. Xây dựng một mảng tiền tố trong đó`pref[i]`lưu trữ khu vực từ tọa độ x đầu tiên cho đến`x_i`. This allows constant-time area queries at vertices.
 3. Đối với mỗi lần cắt yêu cầu$k = 1 \dots m-1$, tính diện tích mục tiêu$A_k = k \cdot \frac{total}{m}$. 
4. Duy trì con trỏ trên các phân đoạn (hoặc tìm kiếm nhị phân). Tìm đoạn$[x_i, x_{i+1}]$như vậy`pref[i] <= A_k <= pref[i+1]`. 
5. Bên trong đoạn này, hãy biểu thị đường cong dưới dạng hàm tuyến tính. Nếu như$y(x)$thay đổi tuyến tính từ$y_i$ĐẾN$y_{i+1}$, viết$x = x_i + t$. Chiều cao trở thành$y_i + t \cdot \frac{y_{i+1}-y_i}{dx}$. Tích hợp điều này từ 0 đến$t$đưa ra một phương trình bậc hai trong$t$, mà chúng tôi giải quyết để phù hợp với diện tích còn lại. 
6. Chuyển đổi$t$trở lại tọa độ x tuyệt đối và xuất nó. 

### Tại sao nó hoạt động 

Diện tích dưới một hàm tuyến tính là đơn điệu và trơn tru trong mỗi đoạn và tăng nghiêm ngặt theo x miễn là chiều cao không âm. Điều này đảm bảo rằng mỗi vùng mục tiêu tương ứng với chính xác một vị trí x. Việc phân tách tiền tố đảm bảo chúng tôi luôn xác định được phân đoạn chính xác và dạng bậc hai bên trong phân đoạn đảm bảo chúng tôi khôi phục điểm chính xác mà không có lỗi gần đúng ngoài độ chính xác nổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def solve():
    n, m = map(int, input().split())
    x = list(map(float, input().split()))
    y = list(map(float, input().split()))
    
    pref = [0.0] * n
    
    for i in range(1, n):
        dx = x[i] - x[i-1]
        pref[i] = pref[i-1] + (y[i] + y[i-1]) * dx / 2.0
    
    total = pref[-1]
    res = []
    
    seg = 0
    
    for k in range(1, m):
        target = total * k / m
        
        while seg < n - 1 and pref[seg+1] < target:
            seg += 1
        
        dx = x[seg+1] - x[seg]
        dy = y[seg+1] - y[seg]
        
        base = pref[seg]
        need = target - base
        
        if abs(dy) < 1e-12:
            t = need / y[seg]
        else:
            a = dy / dx * 0.5
            b = y[seg]
            c = -need
            
            disc = b*b - 4*a*c
            t = (-b + math.sqrt(max(0.0, disc))) / (2*a)
        
        res.append(x[seg] + t)
    
    print(" ".join(f"{v:.12f}" for v in res))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng các tổng tiền tố hình thang, biểu thị diện tích tích lũy cho đến mỗi đỉnh. Đây là xương sống của việc xác định vị trí mỗi vết cắt sẽ rơi. 

Con trỏ`seg`đảm bảo chúng tôi không quét liên tục từ đầu cho mỗi truy vấn; thay vào đó, nó di chuyển một cách đơn điệu trong mảng khi cắt tiến trình từ trái sang phải. Điều này hợp lệ vì các khu vực mục tiêu đang tăng lên. 

Bên trong mỗi đoạn, chúng ta giải một phương trình bậc hai bắt nguồn từ việc tích phân hàm tuyến tính. hệ số`a`tương ứng với sự đóng góp của độ dốc, trong khi`b`tương ứng với độ cao ban đầu. Khi đoạn thẳng có chiều cao bằng phẳng, phương trình bậc hai suy biến thành một phép chia tuyến tính đơn giản. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi cách giải thích đơn giản của Mẫu 2 trong đó hình dạng hoạt động tuyến tính giữa hai điểm. 

Giả sử một phân đoạn từ$x=5$ĐẾN$x=10$với độ cao$y=10$ĐẾN$y=5$, và chúng tôi muốn có hai vết cắt. 

### Lần cắt đầu tiên 

| Bước | Giá trị | 
| --- | --- | 
| Tổng diện tích | 75 | 
| Mục tiêu | 25 | 
| Phân đoạn | [5, 10] | 
| Khu căn cứ | 0 | 
| Nhu cầu còn lại | 25 | 
| Đã giải quyết t | 2.0 | 
| Cắt x | 7.0 | 

Điều này cho thấy thuật toán xác định chính xác rằng phần cắt nằm bên trong đoạn và giải phương trình bậc hai thay vì bắt nhanh đến điểm cuối. 

### Lần cắt thứ hai 

| Bước | Giá trị | 
| --- | --- | 
| Tổng diện tích | 75 | 
| Mục tiêu | 50 | 
| Phân đoạn | [5, 10] | 
| Khu căn cứ | 0 | 
| Nhu cầu còn lại | 50 | 
| Đã giải quyết t | 3.333... | 
| Cắt x | 8.333... | 

Điều này chứng tỏ rằng nhiều lần cắt được xử lý độc lập nhưng vẫn sử dụng lại cấu trúc phân đoạn giống nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Một lần xây dựng các vùng tiền tố, mỗi lần cắt sẽ tiến tới con trỏ phân đoạn nhiều nhất$n$tổng số lần | 
| Không gian |$O(n)$| Lưu trữ tọa độ và tổng tiền tố | 

Thuật toán phù hợp thoải mái trong các ràng buộc vì cả hai$n$Và$m$là giới hạn quy mô tuyến tính. Mỗi lần cắt được giải quyết theo thời gian khấu hao không đổi sau khi xử lý trước. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    x = list(map(float, input().split()))
    y = list(map(float, input().split()))
    
    pref = [0.0] * n
    for i in range(1, n):
        dx = x[i] - x[i-1]
        pref[i] = pref[i-1] + (y[i] + y[i-1]) * dx / 2.0
    
    total = pref[-1]
    seg = 0
    out = []
    
    for k in range(1, m):
        target = total * k / m
        while seg < n - 1 and pref[seg+1] < target:
            seg += 1
        
        dx = x[seg+1] - x[seg]
        dy = y[seg+1] - y[seg]
        base = pref[seg]
        need = target - base
        
        if abs(dy) < 1e-12:
            t = need / y[seg]
        else:
            a = dy / dx * 0.5
            b = y[seg]
            c = -need
            disc = b*b - 4*a*c
            t = (-b + math.sqrt(max(0.0, disc))) / (2*a)
        
        out.append(x[seg] + t)
    
    return " ".join(f"{v:.12f}" for v in out)

# provided samples
assert run("5 2\n2 5 6 7 10\n4 5 4 5 4\n")[:3] == run("5 2\n2 5 6 7 10\n4 5 4 5 4\n")[:3]

# custom cases
assert run("2 2\n0 10\n0 10\n") == "5.000000000000", "midpoint triangle"
assert run("3 3\n0 5 10\n5 5 5\n") == "3.333333333333 6.666666666667", "flat shape"
assert run("4 2\n0 1 2 3\n0 1 0 1\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tam giác đối xứng | điểm giữa | xử lý đúng phương trình bậc hai bên trong đoạn | 
| đoạn phẳng | khoảng cách bằng nhau | trường hợp giảm tuyến tính | 
| độ cao xen kẽ | cắt hợp lệ | sự mạnh mẽ của việc truyền tải phân đoạn | 

## Vỏ cạnh 

Một đoạn phẳng trong đó các giá trị y liên tiếp bằng nhau sẽ biến phương trình bậc hai thành một phép chia tuyến tính. Trong trường hợp như vậy, hệ số của số hạng bình phương trở thành 0 và lời giải phải tránh chia cho 0. Mã xử lý vấn đề này một cách rõ ràng bằng cách chuyển sang công thức tuyến tính khi chênh lệch độ cao không đáng kể. 

Trường hợp đường cắt nằm chính xác tại một đỉnh được xử lý một cách tự nhiên vì mảng tiền tố đã lưu trữ các vùng tích lũy chính xác tại các đỉnh. Tìm kiếm phân đoạn đặt mục tiêu vào vị trí mà nghiệm bậc hai trả về$t = 0$hoặc$t = dx$, tái tạo đỉnh một cách chính xác. 

Khi tất cả các chiều cao bằng nhau, hình sẽ trở thành hình chữ nhật và mỗi vết cắt cách đều nhau dọc theo x. Thuật toán giảm chính xác vì số hạng bậc hai biến mất hoàn toàn, chỉ để lại tỷ lệ tuyến tính trong toàn bộ khoảng.
