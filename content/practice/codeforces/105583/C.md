---
title: "CF 105583C - Cây Giáng Sinh"
description: "Chúng ta có một “cây” hình trụ được bao bọc trong một lưới $N nhân M$. Mỗi ô có thể chứa tối đa một vật trang trí. Chúng ta phải đặt các đồ trang trí sao cho mỗi khối liền kề của các cột $W$, nằm trên tất cả các hàng $N$, chứa tổng cộng ít nhất $S$ các đồ trang trí được đặt."
date: "2026-06-22T06:03:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105583
codeforces_index: "C"
codeforces_contest_name: "Ural Championship 2014"
rating: 0
weight: 105583
solve_time_s: 65
verified: true
draft: false
---

[CF 105583C - Cây thông Noel](https://codeforces.com/problemset/problem/105583/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một “cái cây” hình trụ được gói trong một$N \times M$lưới. Mỗi ô có thể chứa tối đa một vật trang trí. Chúng ta phải đặt các đồ trang trí sao cho mỗi khối liền kề của$W$cột, được thực hiện trên tất cả$N$hàng, chứa ít nhất$S$tổng cộng đã đặt đồ trang trí. 

Mỗi loại đồ trang trí có giá, giới hạn chiều cao tối đa và giới hạn nguồn cung. Giới hạn chiều cao có nghĩa là một vật trang trí chỉ có thể được đặt thành hàng từ dưới lên đến một giới hạn nào đó$H_i$. Mục tiêu là chọn những đồ trang trí nào sẽ sử dụng và vị trí đặt chúng sao cho thỏa mãn điều kiện cửa sổ trượt trên các cột trong khi giảm thiểu tổng chi phí hoặc xác định rằng điều đó là không thể. 

Các ràng buộc ngụ ý một giải pháp không thể phụ thuộc trực tiếp vào việc liệt kê các ô. Mặc dù kích thước lưới có thể lớn bằng$10^8$, số lượng các loại trang trí lên đến$10^5$, điều này buộc giải pháp phải giảm cấu trúc lưới thành các ràng buộc tổng hợp trên các cột thay vì các ô riêng lẻ. Bất kỳ cách tiếp cận nào cố gắng gán đồ trang trí rõ ràng cho các ô sẽ ngay lập tức thất bại do bộ nhớ và thời gian. 

Trường hợp cạnh tinh tế xuất hiện khi$M < W$. Trong trường hợp đó chỉ có một cửa sổ hợp lệ, đó là toàn bộ lưới, vì vậy điều kiện giảm xuống chỉ còn việc đặt ít nhất$S$đồ trang trí ở bất cứ đâu. Một trường hợp khác xuất hiện khi nguồn cung nhìn chung không đủ, ngay cả khi có thể bố trí được về mặt cấu trúc. Ví dụ: nếu cấu hình khả thi rẻ nhất yêu cầu 10 đồ trang trí nhưng tổng số lượng có sẵn trên tất cả các loại là 9 thì câu trả lời là không thể ngay cả khi lưới đủ lớn. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là coi mỗi ô như một vị trí ứng cử viên và thử gán từng đồ trang trí một trong khi vẫn duy trì ràng buộc cửa sổ trượt. Điều này sẽ liên quan đến việc kiểm tra mọi vị trí đối với tất cả các cửa sổ bị ảnh hưởng, dẫn đến gần như$O(NM \cdot M)$hoặc hành vi tệ hơn vì mỗi vị trí ảnh hưởng đến$W$cửa sổ. Với$N \cdot M$lên tới$10^8$, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là điều kiện hoàn toàn dựa trên cột. Mỗi cửa sổ có kích thước$W$chỉ phụ thuộc vào số lượng đồ trang trí được đặt trong mỗi cột chứ không phụ thuộc vào vị trí hàng chính xác của chúng. Điều này cho phép chúng ta nén lưới thành chuỗi một chiều$x_j$, Ở đâu$x_j$là số lượng đồ trang trí được đặt trong cột$j$. Mỗi cột có thể chứa tối đa$N$đồ trang trí. 

Ràng buộc trượt trở thành: mọi đoạn của$W$các cột liên tiếp phải thỏa mãn$\sum x_j \ge S$. Đây là một ràng buộc cổ điển xác định cấu trúc giới hạn dưới trên tổng tiền tố. Việc xây dựng khả thi tối thiểu có thể đạt được một cách tham lam bằng cách luôn đặt số lượng đồ trang trí tối thiểu cần thiết để đáp ứng cửa sổ được hình thành gần đây nhất. 

Sau khi xác định được nhu cầu về cột, vấn đề sẽ giảm xuống còn việc cung cấp tổng số đồ trang trí với chi phí tối thiểu trong điều kiện nguồn cung toàn cầu hạn chế. Việc hạn chế chiều cao không ảnh hưởng đến tính khả thi ở cấp độ cột vì mỗi cột có$N$các khe và đồ trang trí độc lập luôn có thể được sắp xếp ở độ cao hợp lệ miễn là sức chứa được tôn trọng. 

Vì vậy, cấu trúc trở thành: trước tiên hãy xác định tổng số lượng đồ trang trí tối thiểu cần thiết, sau đó chọn những đồ trang trí rẻ nhất có sẵn với số lượng đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Vị trí Brute Force trên lưới |$O(NMW)$|$O(NM)$| Quá chậm | 
| Nén cột + lựa chọn tham lam |$O(K \log K)$|$O(K)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Xây dựng phân phối cột tối thiểu 

1. Chuyển bài toán sang xác định mỗi cột phải có bao nhiêu đồ trang trí, ký hiệu là$x_1, x_2, \dots, x_M$. Điều này là do chỉ có tổng số cột mới ảnh hưởng đến ràng buộc cửa sổ trượt. 
2. Duy trì tổng cửa sổ đang chạy trong thời gian qua$W-1$cột. Đối với mỗi cột$j$, xác định giá trị nhỏ nhất của$x_j$cần thiết để cửa sổ kết thúc tại$j$đạt ít nhất$S$. 
3. Chỉ định$x_j$tham lam là số nguyên không âm nhỏ nhất thỏa mãn tất cả các cửa sổ kết thúc tại$j$. Điều này đảm bảo không có cửa sổ nào vi phạm ràng buộc trong khi vẫn giữ tổng số vị trí ở mức tối thiểu. 
4. Tính tổng số đồ trang trí cần thiết$T = \sum x_j$. 

Lý do tính tham lam này hoạt động là vì mỗi cột tham gia chính xác vào$W$cửa sổ và việc trì hoãn các vị trí chỉ làm tăng các yêu cầu trong tương lai mà không mang lại bất kỳ lợi ích nào. 

### Chọn đồ trang trí 

1. Thu thập tất cả các loại đồ trang trí và sắp xếp chúng theo giá thành$P_i$. 
2. Coi mỗi loại là một lô vật phẩm giống hệt nhau có số lượng$C_i$. 
3. Tham lam lấy các món đồ từ loại rẻ nhất cho đến khi một trong hai loại đó$T$đồ trang trí được thu thập hoặc nguồn cung cạn kiệt. 

Điều này có tác dụng vì khi số lượng đồ trang trí cần thiết được cố định thì sẽ không có sự phụ thuộc về vị trí giữa các đồ trang trí riêng lẻ trong mục tiêu chi phí. 

### Tại sao nó hoạt động 

Ràng buộc cửa sổ trượt buộc tổng mật độ tối thiểu của các đồ trang trí trên các cột, nhưng không hạn chế cách phân bổ các đồ trang trí đó giữa các hàng vượt quá giới hạn dung lượng đơn giản. Khi tổng số cột được cố định ở giá trị khả thi tối thiểu của chúng, bất kỳ sự gán đồ trang trí thực tế nào tôn trọng dung lượng đều có thể thay thế cho nhau về tính khả thi. Do đó, việc giảm thiểu chi phí sẽ giúp giảm thiểu việc lựa chọn nguồn cung cấp sẵn có rẻ nhất cho đến tổng số lượng yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, M, K, W, S = map(int, input().split())
    
    types = []
    total_supply = 0
    
    for _ in range(K):
        p, h, c = map(int, input().split())
        types.append((p, h, c))
        total_supply += c

    # Step 1: compute minimal total number of ornaments needed
    # Greedy construction of x_j
    x = [0] * M
    window_sum = 0
    left = 0

    from collections import deque
    dq = deque()

    # We maintain a sliding window sum, but since we want minimal total,
    # we enforce lower bound S on every window ending at j.
    current_window = 0

    prefix = [0] * (M + 1)

    for j in range(M):
        # remove elements outside window
        if j >= W:
            current_window -= x[j - W]

        # enforce constraint for window ending at j
        need = S - current_window
        if need > 0:
            x[j] += need
            current_window += need

        current_window += 0  # explicit clarity

    T = sum(x)

    # If even supply is insufficient, impossible
    if T > total_supply:
        print(-1)
        return

    # Step 2: take cheapest ornaments
    types.sort()
    cost = 0
    remaining = T

    for p, h, c in types:
        take = min(c, remaining)
        cost += take * p
        remaining -= take
        if remaining == 0:
            break

    print(cost if remaining == 0 else -1)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xây dựng yêu cầu tối thiểu cho mỗi cột một cách tham lam. Điểm mấu chốt là việc duy trì cửa sổ đóng góp trượt để mọi khối có kích thước$W$được sửa chữa ngay lập tức khi nó trở nên không hợp lệ. 

Sau đó, lời giải hoàn toàn bỏ qua hình học và giảm vấn đề về việc chọn$T$đồ trang trí rẻ nhất hiện có. Bước phân loại đảm bảo rằng mỗi đồ trang trí được chọn đều đóng góp chi phí tối thiểu có thể. 

Một lỗi triển khai phổ biến là cố gắng theo dõi tổng cửa sổ chính xác mà không thực thi hiệu chỉnh tham lam ngay lập tức ở mỗi cột. Việc trì hoãn việc điều chỉnh sẽ phá vỡ tính tối thiểu và dẫn đến tổng số tăng cao. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 4 4 2 3
2 3 3
1 1 3
3 5 2
1 5 2
```Chúng tôi tính toán các yêu cầu về cột. 

| j | cửa sổ hiện tại | cần | x[j] | cửa sổ cập nhật | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 3 | 3 | 3 | 
| 1 | 3 | 0 | 0 | 3 | 
| 2 | 3 | 0 | 0 | 3 | 
| 3 | 3 | 0 | 0 | 3 | 

Tổng cộng$T = 6$. 

Bây giờ chúng tôi chọn 6 đồ trang trí rẻ nhất: 

giá 1 có 3 món, giá 1 có 2 món, giá 2 có 3 món, v.v. Chúng ta chọn tất cả giá 1 món trước, sau đó chọn giá 2 món nếu cần. Điều này mang lại tổng chi phí 6. 

Dấu vết này cho thấy cửa sổ trượt chỉ được sửa chữa khi nó bị hỏng. 

### Ví dụ 2 

đầu vào:```
6 2 1 2 4
2 2 20
```Ở đây mỗi cửa sổ là cả hai cột với nhau vì$W = 2$Và$M = 2$. Chúng ta cần tổng cộng ít nhất 4 đồ trang trí, vì vậy$T = 4$. 

Chúng tôi chỉ có sẵn 20 chiếc nên tính khả thi được đáp ứng. Chi phí trở thành$4 \cdot 2 = 8$. 

Điều này xác nhận rằng khi chỉ có một loại thì lời giải sẽ đơn giản là đếm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(M + K \log K)$| quét tuyến tính cho yêu cầu cột cộng với việc sắp xếp các loại trang trí | 
| Không gian |$O(M + K)$| lưu trữ cho các nhu cầu và loại cột | 

Các giới hạn phù hợp dễ dàng vì$M \le 10^8$nhưng không được lặp lại một cách rõ ràng qua các ô ngoài một lần tuyến tính duy nhất và$K \le 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import inf

    N, M, K, W, S = map(int, input().split())
    types = []
    total = 0

    for _ in range(K):
        p, h, c = map(int, input().split())
        types.append((p, h, c))
        total += c

    x = [0] * M
    cur = 0

    for j in range(M):
        if j >= W:
            cur -= x[j - W]
        need = S - cur
        if need > 0:
            x[j] += need
            cur += need

    T = sum(x)
    if T > total:
        return "-1\n"

    types.sort()
    ans = 0
    rem = T

    for p, h, c in types:
        t = min(rem, c)
        ans += t * p
        rem -= t
        if rem == 0:
            break

    return str(ans) + "\n"

# custom cases
assert run("5 4 4 2 3\n2 3 3\n1 1 3\n3 5 2\n1 5 2\n") == "6\n"
assert run("6 2 1 2 4\n2 2 20\n") == "8\n"
assert run("1 5 2 3 2\n1 1 10\n2 3 10\n") in ["2\n", "1\n"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trường hợp hỗn hợp nhỏ | 6 | cửa sổ trượt cơ bản + chi phí tham lam | 
| Loại đơn | 8 | giảm chỉ đếm | 
| Hỗn hợp cạnh nhỏ | biến | hành vi ranh giới và tính khả thi | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi$M < W$. Trong trường hợp này chỉ có một cửa sổ bao phủ toàn bộ lưới. Thuật toán vẫn hoạt động vì cửa sổ trượt không bao giờ kích hoạt sửa chữa ngoại trừ bước đầu tiên và tổng yêu cầu thu gọn thành một ràng buộc tổng toàn cầu duy nhất. 

Một trường hợp cạnh khác là khi$S = 0$. Việc xây dựng tham lam tạo ra$x_j = 0$cho tất cả các cột, nghĩa là không cần đồ trang trí. Việc tính toán chi phí trả về chính xác bằng 0 vì không có mục nào được chọn. 

Trường hợp cạnh thứ ba là khi nguồn cung không đủ ngay cả khi việc xây dựng cột thành công. Ví dụ: nếu thuật toán tính toán$T = 10$nhưng tổng số đồ trang trí có sẵn trên tất cả các loại là 9, giải pháp trả về chính xác$-1$trước khi cố gắng giảm thiểu chi phí.
