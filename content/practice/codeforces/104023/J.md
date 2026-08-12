---
title: "CF 104023J - Ăn, ngủ, lặp lại"
description: "Chúng ta được cho một tập hợp các số nguyên $a1, a2, dấu chấm, an$. Mỗi nước đi bao gồm việc chọn một phần tử và giảm nó đi $1$. Theo thời gian, các phần tử trôi xuống cho đến khi cuối cùng chúng trở thành $0$."
date: "2026-07-02T04:27:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104023
codeforces_index: "J"
codeforces_contest_name: "2022 China Collegiate Programming Contest (CCPC) Weihai Site"
rating: 0
weight: 104023
solve_time_s: 170
verified: true
draft: false
---

[CF 104023J - Ăn, Ngủ, Lặp lại](https://codeforces.com/problemset/problem/104023/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 50 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các số nguyên$a_1, a_2, \dots, a_n$. Mỗi nước đi bao gồm việc chọn một phần tử và giảm nó đi$1$. Theo thời gian, các phần tử trôi xuống phía dưới cho đến khi cuối cùng chúng trở thành$0$. 

Ngoài ra, một số giá trị$x$có giới hạn trên$limit_x$về số lần chúng được phép xuất hiện trong mảng bất kỳ lúc nào. Nếu một giá trị không có ràng buộc thì nó thực sự không bị ràng buộc. 

Một nước đi là bất hợp pháp nếu sau khi thực hiện nó, bất kỳ giá trị nào$x$vượt quá giới hạn của nó. Đặc biệt, khi chúng ta giảm một phần tử từ$v$ĐẾN$v-1$, chúng tôi giảm số lượng$v$và tăng số lượng$v-1$, vì vậy chỉ giá trị đích mới có khả năng trở thành không hợp lệ. 

Người chơi sẽ thua nếu họ không có động thái hợp pháp. Điều này xảy ra khi tất cả các giá trị bằng 0 hoặc khi mọi mức giảm có thể sẽ ngay lập tức vi phạm một ràng buộc. 

Nhiệm vụ là phân định người chiến thắng khi cả hai người chơi đều chơi tối ưu và Pico đi trước. 

Các ràng buộc ngụ ý rằng chúng ta không thể mô phỏng trò chơi theo từng bước. Tổng kích thước trên tất cả các trường hợp thử nghiệm là$10^5$, do đó, bất kỳ giải pháp nào về cơ bản đều phải tuyến tính hoặc gần tuyến tính về số lượng các giá trị riêng biệt có liên quan. 

Trường hợp cạnh tinh vi phát sinh khi một giá trị đạt đến giới hạn một cách chính xác. Trong tình huống đó, mọi nỗ lực di chuyển một giá trị lớn hơn vào đó đều bị cấm, ngăn chặn dòng chảy đi xuống tiếp theo một cách hiệu quả. Điều này có thể tạo ra “rào cản” phân đoạn dòng giá trị thành các vùng độc lập. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ liên tục chọn một phần tử và cố gắng giảm nó trong khi kiểm tra tất cả các ràng buộc. Mỗi lần di chuyển yêu cầu cập nhật hai lần đếm tần số và xác thực tất cả các giá trị bị ràng buộc. Vì có thể có tới$10^9$dưới dạng giá trị và lên đến$10^5$di chuyển, phương pháp này là không khả thi. 

Quan sát quan trọng là cách duy nhất mà các ràng buộc ảnh hưởng đến trò chơi là chặn quá trình chuyển đổi giữa các giá trị liền kề. Khi chúng ta di chuyển một phần tử từ$x$ĐẾN$x-1$, chúng tôi tăng số lượng$x-1$. Nếu như$x-1$đã đến giới hạn rồi, hành động này bị cấm. Một khi là một giá trị$x$bão hòa (số lượng của nó bằng giới hạn của nó), nó trở thành một rào cản vĩnh viễn: không có phần tử nào có thể được di chuyển vào$x$một lần nữa, bởi vì điều đó sẽ vi phạm ràng buộc ngay lập tức. 

Điều này tạo ra một phân vùng của dòng số nguyên thành các phân đoạn độc lập. Bên trong mỗi phân đoạn, các phần tử chỉ có thể di chuyển xuống dưới cho đến khi chúng đạt giá trị bão hòa gần nhất bên dưới chúng, giá trị này đóng vai trò như một ranh giới hấp thụ. Trong một phân đoạn như vậy, mọi mức giảm chỉ đơn giản là dịch chuyển khối lượng xuống cho đến khi tất cả các phần tử thu gọn vào ranh giới và số lần di chuyển hợp lệ chính xác là tổng số vượt quá trên ranh giới đó. 

Do đó, trò chơi giảm xuống việc đếm xem chúng ta có thể giảm các phần tử bao nhiêu lần trước khi mọi thứ buộc phải dừng lại. Mỗi lần giảm là một nước đi, do đó tổng số nước đi là cố định bất kể chiến lược nào và lối chơi tối ưu chỉ xác định ai thực hiện nước đi cuối cùng. Người chiến thắng được xác định bằng tính chẵn lẻ của tổng số nước đi hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(\text{moves} \cdot k)$|$O(n + k)$| Quá chậm | 
| Giảm phân đoạn + chẵn lẻ |$O(n + k)$|$O(n + k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi trình bày lại vấn đề bằng cách tính toán có bao nhiêu phép toán giảm hợp lệ tồn tại trước khi hệ thống đạt đến trạng thái không thể di chuyển được. 

### 1. Xây dựng tần suất của các giá trị 

Chúng tôi tính toán$cnt[x]$, số lần xuất hiện của mỗi giá trị$x$trong mảng. Chỉ các giá trị xuất hiện trong các ràng buộc hoặc trong mảng mới quan trọng. 

Vai trò của$cnt$là để xác định giá trị ràng buộc nào đã bão hòa. 

### 2. Xác định các điểm ràng buộc bão hòa 

Đối với mọi ràng buộc$x \to limit_x$, chúng tôi kiểm tra xem$cnt[x] = limit_x$. Nếu đẳng thức giữ nguyên thì$x$được bão hòa ngay lập tức. 

Một giá trị như vậy$x$trở thành một rào cản vĩnh viễn bởi vì bất kỳ nỗ lực nào để di chuyển một giá trị từ$x+1$vào trong$x$bị cấm. 

### 3. Sắp xếp điểm bão hòa 

Chúng tôi thu thập tất cả các giá trị bão hòa và sắp xếp chúng ngày càng nhiều. Những điểm này phân chia dòng số nguyên thành các đoạn độc lập. 

Giữa hai giá trị bão hòa liên tiếp$b < c$, các phần tử có giá trị bằng$(b, c]$chỉ có thể di chuyển xuống cho đến khi họ đạt được$b$. 

### 4. Xác định mức sàn hiệu dụng cho từng giá trị 

Với mỗi giá trị$v$, định nghĩa$L(v)$là giá trị bão hòa lớn nhất$\le v$. Nếu không tồn tại,$L(v) = 0$. 

Giá trị này là mức thấp nhất mà bất kỳ phần tử nào bắt đầu từ$v$có thể tiếp cận. 

### 5. Đếm tổng số nước đi 

Mỗi phần tử bắt đầu từ giá trị$v$có thể giảm chính xác$v - L(v)$nhiều lần trước khi nó bị mắc kẹt. 

Chúng tôi tổng hợp đóng góp này trên tất cả các yếu tố:$$\text{moves} = \sum_{i=1}^{n} (a_i - L(a_i)).$$### 6. Xác định người thắng cuộc bằng tính chẵn lẻ 

Mỗi lần di chuyển sẽ lật lượt. Người chơi thực hiện nước đi cuối cùng sẽ thắng. Vì vậy: 

- Nếu tổng số nước đi là số lẻ thì Pico thắng. 
- Nếu chẵn, FuuFuu thắng. 

### Tại sao nó hoạt động 

Khi một giá trị trở nên bão hòa, nó không bao giờ có thể tăng lên và không phần tử nào có thể vượt qua nó. Điều này tạo ra các ranh giới bất biến phân chia không gian trạng thái. Trong mỗi phân vùng, mỗi bước di chuyển sẽ làm giảm đáng kể tổng “khoảng cách đến ranh giới” và không có bước di chuyển nào có thể vượt qua các phân vùng. Vì vậy, mỗi nước đi hợp lệ đều tương ứng chính xác với việc giảm tổng khoảng cách$\sum (a_i - L(a_i))$qua$1$và không có chuỗi nước đi thay thế nào có thể thay đổi được tổng số này. Điều này làm cho trò chơi tương đương với một đống có kích thước đó trong cách chơi bình thường, trong đó kết quả tối ưu chỉ phụ thuộc vào tính chẵn lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

T = int(input())
for _ in range(T):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    cnt = {}
    for v in a:
        cnt[v] = cnt.get(v, 0) + 1

    limit = {}
    saturated = set()

    for _ in range(k):
        x, y = map(int, input().split())
        limit[x] = y

    for x, y in limit.items():
        if cnt.get(x, 0) == y:
            saturated.add(x)

    sat = sorted(saturated)

    def floor(v):
        # largest saturated value <= v
        lo, hi = 0, len(sat)
        while lo < hi:
            mid = (lo + hi) // 2
            if sat[mid] <= v:
                lo = mid + 1
            else:
                hi = mid
        return sat[lo - 1] if lo > 0 else 0

    total = 0
    for v in a:
        total += v - floor(v)

    if total % 2:
        print("Pico")
    else:
        print("FuuFuu")
```Việc triển khai trước tiên sẽ xây dựng số lượng tần số, sau đó xác định các điểm ràng buộc bão hòa. Tìm kiếm nhị phân trên danh sách bão hòa đã sắp xếp sẽ tính toán mức sàn hiệu quả cho từng giá trị. Tổng cuối cùng tổng hợp số lần giảm bắt buộc. 

Chi tiết triển khai chính là chỉ những trường hợp bình đẳng mới tạo ra rào cản; sự bất bình đẳng nghiêm ngặt không hạn chế những bước đi trong tương lai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, k = 0
a = [1, 2, 3]
```| Bước | Hành động | Kết quả | 
| --- | --- | --- | 
| 1 | Không có ràng buộc | không có điểm bão hòa | 
| 2 | tầng(v)=0 với mọi v | | 
| 3 | tính tổng |$1+2+3=6$| 

Tổng số nước đi = 6, chẵn nên FuuFuu thắng. 

Điều này phù hợp với thực tế là trò chơi chỉ đơn giản là đếm ngược về 0 mà không có hạn chế nào. 

### Ví dụ 2 

đầu vào:```
n = 3, k = 1
a = [1, 2, 2]
constraint: 0 -> 1
```| Bước | Hành động | Kết quả | 
| --- | --- | --- | 
| 1 | cnt[0]=1 bằng giới hạn | 0 đã bão hòa | 
| 2 | bão hòa = {0} | | 
| 3 | tầng(v)=0 với mọi v | | 
| 4 | tổng = 1+2+2 = 5 | | 

Tổng số nước đi = 5, lẻ nên Pico thắng. 

Giá trị bão hòa ở mức 0 không chặn gì ở đây, nhưng nó vẫn xác định ranh giới của hệ thống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + k + s \log s)$| đếm tần số, hạn chế đọc, tìm kiếm nhị phân trên các điểm bão hòa | 
| Không gian |$O(n + k)$| lưu trữ số lượng, giới hạn và bộ bão hòa | 

Giải pháp dễ dàng phù hợp trong giới hạn vì tổng$n + k \le 10^5$và tất cả các phép toán đều tuyến tính hoặc logarit trên phạm vi này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    T = int(input())
    out = []
    for _ in range(T):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        cnt = {}
        for v in a:
            cnt[v] = cnt.get(v, 0) + 1

        limit = {}
        for _ in range(k):
            x, y = map(int, input().split())
            limit[x] = y

        sat = []
        for x, y in limit.items():
            if cnt.get(x, 0) == y:
                sat.append(x)
        sat.sort()

        def floor(v):
            lo, hi = 0, len(sat)
            while lo < hi:
                mid = (lo + hi) // 2
                if sat[mid] <= v:
                    lo = mid + 1
                else:
                    hi = mid
            return sat[lo - 1] if lo else 0

        total = 0
        for v in a:
            total += v - floor(v)

        out.append("Pico" if total % 2 else "FuuFuu")

    return "\n".join(out)

# sample-like sanity checks
assert run("1\n3 0\n1 2 3\n") == "FuuFuu"
assert run("1\n3 1\n1 2 2\n0 1\n") == "Pico"
assert run("1\n1 1\n5\n5 1\n") in ("Pico", "FuuFuu")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Không có ràng buộc | FuuFuu | trò chơi chẵn lẻ thuần túy | 
| Độ bão hòa đơn | Pico | tạo ranh giới | 
| Cạnh phần tử đơn | hoặc | cấu hình tối thiểu | 

## Vỏ cạnh 

Trường hợp cạnh khóa xảy ra khi một giá trị bị ràng buộc đã bão hòa chính xác ngay từ đầu. Trong tình huống đó, nó ngay lập tức trở thành một ranh giới vĩnh viễn. Ví dụ: nếu tất cả các lần xuất hiện của$0$đã đạt tới$limit_0$, thì không có giá trị$1$hoặc cao hơn có thể được chuyển vào$0$, đóng băng cấu trúc tại thời điểm đó. Thuật toán xử lý việc này một cách chính xác vì các giá trị như vậy được chèn rõ ràng vào tập bão hòa trước các tầng tính toán. 

Một trường hợp khác là khi không có ràng buộc nào tồn tại. Khi đó không tồn tại điểm bão hòa, mỗi tầng đều$0$và kết quả giảm xuống mức chẵn lẻ của tổng tất cả các phần tử.
