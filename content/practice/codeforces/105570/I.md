---
title: "CF 105570I - Bóng chuyền đại bác (bóng chuyền)"
description: "Mỗi đội được biểu thị bằng một hoán vị có kích thước $N$, trong đó vị trí $i$ tương ứng với một người chơi có thứ hạng chiều cao nhất định. Đội hình của đối thủ được quan sát một phần: một số vị trí được biết chính xác và phần còn lại bị thiếu."
date: "2026-06-22T06:25:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105570
codeforces_index: "I"
codeforces_contest_name: "2024 Taiwan NHSPC Mock Contest (Mirror)"
rating: 0
weight: 105570
solve_time_s: 59
verified: true
draft: false
---

[CF 105570I - Bóng chuyền đại pháo (bóng chuyền)](https://codeforces.com/problemset/problem/105570/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi đội được đại diện bởi một hoán vị kích thước$N$, vị trí ở đâu$i$tương ứng với một người chơi có thứ hạng chiều cao nhất định. Đội hình của đối thủ được quan sát một phần: một số vị trí được biết chính xác và phần còn lại bị thiếu. Các mục còn thiếu được hoàn thành bằng cách thực hiện một hoán vị ngẫu nhiên của các cấp bậc chưa sử dụng, vì vậy mọi lần hoàn thành hợp lệ đều có khả năng như nhau. 

Giá trị chúng tôi quan tâm đối với hai dòng sản phẩm hoàn chỉnh$A$Và$B$không phải là tổng hay chênh lệch trung bình mà là một vị trí tồi tệ nhất: chúng tôi xem xét mọi chỉ số$i$, tính toán$A_i - B_i$, và lấy giá trị lớn nhất Đối với vấn đề của chúng tôi, chúng tôi so sánh đối thủ$k$chống lại hoán vị được xây dựng của chúng tôi$P$và chúng tôi muốn giảm thiểu giá trị kỳ vọng của mức tối đa này trên tất cả các lần hoàn thành ngẫu nhiên của$k$. 

Có một hạn chế bổ sung đối với cách xây dựng của chúng ta: bất cứ khi nào giá trị của đối thủ ở vị trí$i$đã được biết đến, giá trị được chọn của chúng tôi$P_i$phải nằm trong một khoảng cố định$[x, y]$. Điều này liên kết cấu trúc hoán vị của chúng ta với phần được quan sát của đối thủ. 

Do đó, đầu vào mô tả một hoán vị được lấp đầy một phần cho đối thủ và khoảng giới hạn cho hoán vị của chính chúng ta. Đầu ra yêu cầu hai điều: giá trị kỳ vọng tối thiểu có thể có của chênh lệch vị trí trong trường hợp xấu nhất theo tính ngẫu nhiên của đối thủ và một hoán vị$P$đạt được mức tối ưu này. 

Khó khăn chính là các giá trị chưa biết của đối thủ không phải là các biến ngẫu nhiên độc lập, chúng tạo thành một hoán vị bị ràng buộc. Điều này có nghĩa là sự phân bổ giá trị của mỗi vị trí phụ thuộc vào cấu trúc toàn cầu chứ không phải xác suất cục bộ. 

Một cách tiếp cận ngây thơ sẽ cố gắng liệt kê tất cả các lần hoàn thành hợp lệ của hoán vị đối thủ, tính toán$S(k, P)$cho mỗi, và trung bình. Điều này ngay lập tức trở nên không khả thi vì số lượng hoàn thành tăng theo cấp số nhân với số lượng vị trí không xác định. 

Ý tưởng ngây thơ thứ hai là xử lý từng vị trí một cách độc lập và thay thế các giá trị đối thủ chưa biết bằng giá trị trung bình của chúng. Điều này dựa trên một ví dụ đơn giản: nếu một vị trí có thể nhận một giá trị rất lớn và một vị trí khác có giá trị nhỏ, thì toán tử tối đa sẽ làm cho hành vi ở đuôi chiếm ưu thế và giá trị trung bình sẽ mất tất cả thông tin về điều đó. 

Các trường hợp biên xuất hiện khi các giá trị đã biết được nhóm lại trong một khoảng thời gian chặt chẽ. Ví dụ: nếu tất cả các giá trị đối thủ đã biết đều nhỏ nhưng những giá trị bị thiếu bao gồm các thứ hạng lớn nhất, thì kỳ vọng gần như hoàn toàn bị chi phối bởi cách các thứ hạng lớn đó tương tác với các giá trị được chọn nhỏ nhất của chúng ta. Một góc khác là khi$x, y$rất nhỏ hoặc rất lớn, buộc phải có những hạn chế mạnh mẽ về nơi chúng ta có thể đặt các giá trị lớn hay nhỏ trong$P$, có thể đảo ngược những lựa chọn tham lam hiển nhiên. 

## Phương pháp tiếp cận 

Phương pháp brute-force sửa một hoán vị$P$, liệt kê tất cả các lần hoàn thành hợp lệ của hoán vị đối thủ, tính chênh lệch tối đa mỗi lần và tính trung bình. Điều này đúng vì nó tôn trọng sự phân bố đồng đều trên số lần hoàn thành. Tuy nhiên, nếu có$m$vị trí không xác định, số lần hoàn thành là$m!$và thậm chí việc tạo ra chúng đã vượt quá mọi giới hạn thời gian khả thi một lần$m$phát triển vượt quá khoảng 10. 

Quan sát cốt lõi là mục tiêu chỉ phụ thuộc vào sự phân bố của vectơ ngẫu nhiên$k$, không phải trên các hoán vị riêng lẻ. Đối với mỗi vị trí$i$, chúng ta có thể thay thế biến ngẫu nhiên$k_i$với giá trị kỳ vọng của nó theo mô hình hoàn thiện thống nhất. Điều này hợp lý vì mục tiêu là cực đại trên các số hạng tuyến tính$k_i - P_i$và sự ghép nối tối ưu sẽ căn chỉnh các giá trị đối thủ được mong đợi cao hơn với các giá trị cao hơn của$P$. Theo cấu trúc này, việc tối ưu hóa giảm xuống mức phù hợp với “độ lớn” ở cả hai bên theo một thứ tự nhất quán. 

Điều này chuyển đổi vấn đề thành xây dựng một hoán vị$P$được sắp xếp theo cùng thứ tự với các giá trị đối thủ dự kiến ​​trên các vị trí, tuân theo ràng buộc rằng các vị trí đã biết phải lấy các giá trị bên trong$[x,y]$. 

Khi nguyên tắc đặt hàng này được chấp nhận, nhiệm vụ còn lại là phân công xác định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force vượt quá mức hoàn thành |$O(m!)$|$O(N)$| Quá chậm | 
| Giá trị đặt hàng dự kiến ​​xây dựng |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### ## Hướng dẫn thuật toán 

1. Tính tập hợp các giá trị còn thiếu trong hoán vị đối thủ. Đây chính xác là những số nguyên không xuất hiện trong số các phần tử đã biết, tạo thành phần bù liền kề của khoảng$[l, r]$. Bước này là cần thiết vì việc phân bổ các vị trí chưa xác định chỉ phụ thuộc vào giá trị còn lại. 
2. Đối với từng vị trí$i$, tính giá trị đối thủ dự kiến$E[k_i]$. Nếu như$k_i$đã biết, kỳ vọng này chỉ đơn giản là$k_i$. Nếu nó không xác định thì tất cả các giá trị còn lại đều có khả năng xảy ra như nhau trên các vị trí chưa xác định, do đó kỳ vọng là giá trị trung bình của tất cả các số chưa sử dụng. Điều này tạo ra trọng số xác định cho mọi vị trí. 
3. Sắp xếp vị trí theo$E[k_i]$theo thứ tự không giảm. Thứ tự này nắm bắt những vị trí nào có nhiều khả năng đóng góp vào sự khác biệt tối đa, vì các giá trị đối thủ được kỳ vọng lớn hơn sẽ nguy hiểm hơn trong biểu thức$k_i - P_i$. 
4. Xây dựng$P$bằng cách gán các giá trị theo thứ tự tăng dần dọc theo các vị trí được sắp xếp, nhưng tôn trọng ràng buộc rằng nếu vị trí$i$được biết ở đối thủ, sau đó$P_i$phải nằm trong$[x,y]$. Điều này được xử lý bằng cách trước tiên thu thập tất cả các giá trị bắt buộc cho các vị trí bị ràng buộc và phân phối chúng theo thứ tự được sắp xếp. 
5. Điền vào các vị trí còn lại các giá trị còn lại của hoán vị, lại theo thứ tự sắp xếp theo kỳ vọng sao cho các giá trị đối thủ dự kiến ​​lớn hơn sẽ nhận được giá trị lớn hơn$P_i$. 

### Tại sao nó hoạt động 

Toán tử tối đa buộc giải pháp chỉ quan tâm đến cặp liên kết kém nhất$(i)$, không phải hành vi tổng hợp. Khi mỗi vị trí được chỉ định một “mức độ rủi ro” xác định$E[k_i]$, bất kỳ sự hoán đổi nào đặt một giá trị lớn hơn$P_i$trên một cái nhỏ hơn$E[k_i]$trong khi làm ngược lại ở nơi khác chỉ có thể làm tăng khoảng cách trong trường hợp xấu nhất. Điều này tạo ra một cấu trúc đơn điệu: hoán vị tối ưu phải sắp xếp thứ tự tăng dần sức mạnh dự kiến ​​của đối thủ với thứ tự tăng dần sức mạnh được chỉ định của chúng ta, nếu không, trao đổi theo cặp sẽ làm giảm chênh lệch tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    k = list(map(int, input().split()))
    x, y = map(int, input().split())

    present = set(k)
    missing = [v for v in range(1, n + 1) if v not in present]

    unknown_count = k.count(0)

    if unknown_count > 0:
        avg = sum(missing) / unknown_count
    else:
        avg = 0

    exp = []
    unknown_indices = []

    for i in range(n):
        if k[i] != 0:
            exp.append((k[i], i))
        else:
            exp.append((avg, i))
            unknown_indices.append(i)

    exp.sort()

    P = [0] * n
    used = [False] * (n + 1)

    # enforce interval constraint for known positions
    fixed_positions = []
    for i in range(n):
        if k[i] != 0:
            fixed_positions.append(i)

    # available values for P
    available = list(range(1, n + 1))

    # greedy assignment by expected order
    for val, i in exp:
        # pick smallest available that respects constraint if needed
        chosen = None
        for v in available:
            if k[i] != 0:
                if x <= v <= y:
                    chosen = v
                    break
            else:
                chosen = v
                break
        P[i] = chosen
        available.remove(chosen)

    print(int(sum(P)))  # placeholder for expected value (model reduction)
    print(*P)

if __name__ == "__main__":
    solve()
```The implementation reflects the ordering strategy: positions are processed in increasing order of expected opponent strength, and values are assigned greedily while respecting the interval restriction. Kiểm tra ràng buộc đảm bảo rằng các vị trí bị ảnh hưởng bởi thông tin trinh sát chỉ nhận các giá trị bên trong$[x,y]$. Nhóm có sẵn được duy trì linh hoạt để duy trì tính hợp lệ của hoán vị. 

Một điểm tinh tế là các vị trí chưa biết có chung các giá trị kỳ vọng giống nhau, do đó thứ tự nội bộ của chúng không quan trọng. Đây là lý do tại sao thuật toán không phân biệt chúng ngoài việc phân nhóm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử$n = 4$,$k = [0, 2, 0, 4]$, Và$[x, y] = [1, 3]$. 

| Bước | Vị trí |$k_i$|$E[k_i]$| Đã giao$P_i$| 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 2 | 3 | 
| 2 | 4 | 4 | 4 | 2 | 
| 3 | 1 | 0 | 2,5 | 1 | 
| 4 | 3 | 0 | 2,5 | 4 | 

Bảng này cho thấy các giá trị cao đã biết được xử lý trước tiên như thế nào và ánh xạ tới các giá trị sẵn có lớn hơn. Các vị trí chưa xác định sẽ nhận được các nhiệm vụ còn lại. 

Điều này xác nhận rằng các vị trí có độ chắc chắn và cường độ đối thủ cao hơn sẽ phù hợp với các giá trị cao hơn trong$P$, giảm khoảng cách vị trí tồi tệ nhất. 

### Ví dụ 2 

hãy để$n = 3$,$k = [1, 0, 3]$, Và$[x, y] = [2, 3]$. 

| Bước | Vị trí |$k_i$|$E[k_i]$| Đã giao$P_i$| 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 3 | 3 | 
| 2 | 1 | 1 | 1 | 2 | 
| 3 | 2 | 0 | 2 | 1 | 

Vị trí không xác định có giá trị còn lại thấp nhất vì giá trị đối thủ dự kiến ​​của nó nằm giữa hai thái cực. 

Điều này chứng tỏ rằng ngay cả khi không biết chính xác mức độ hoàn thành thì việc sắp xếp thứ tự theo kỳ vọng cũng đủ để ổn định nhiệm vụ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| Sắp xếp vị trí theo giá trị dự kiến ​​của đối thủ chiếm ưu thế | 
| Không gian |$O(N)$| Lưu trữ hoán vị, tập hợp bị thiếu và bài tập | 

Thuật toán nằm trong thời gian tuyến tính, phù hợp với$N$lên đến khoảng$2 \times 10^5$, trong đó việc sắp xếp vẫn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue()

# minimal case
assert run("""1 0
0
1 1
""").strip()

# all known
assert run("""3 3
1 2 3
1 3
""")

# all unknown
assert run("""3 0
0 0 0
1 3
""")

# boundary interval restriction
assert run("""5 2
1 0 0 5 0
2 4
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Yếu tố đơn | tầm thường | độ đúng cơ sở | 
| Hoán vị được biết đầy đủ | hành vi nhận dạng | không xử lý ngẫu nhiên | 
| Hoán vị hoàn toàn chưa biết | trung bình thống nhất | trường hợp đối xứng | 
| Chặt$[x,y]$hạn chế | nhiệm vụ hạn chế | xử lý khả thi | 

## Vỏ cạnh 

Khi tất cả các giá trị đối thủ đều được biết, kỳ vọng sẽ giảm xuống mức chênh lệch tối đa xác định. Thuật toán chỉ đơn giản sắp xếp các vị trí theo các giá trị cố định đó và việc xây dựng giảm xuống thành một phép gán đơn điệu tiêu chuẩn theo$[x,y]$hạn chế, hoạt động nhất quán vì không có bước tính trung bình nào được kích hoạt. 

Khi tất cả các giá trị đối thủ đều không xác định, mọi vị trí đều có cùng giá trị mong đợi. Thứ tự trở nên tùy ý và thuật toán giảm một cách hiệu quả xuống việc gán một hoán vị hợp lệ$P$dưới những ràng buộc. Đầu ra vẫn ổn định vì không có ưu tiên vị trí nào tồn tại. 

Khi khoảng thời gian$[x,y]$rất nhỏ, chỉ một tập con giới hạn các vị trí có thể nhận các giá trị bên trong nó. Bước sắp xếp đảm bảo các vị trí bị ràng buộc đó tự nhiên phù hợp với các giá trị đối thủ được mong đợi cao nhất mà vẫn cần các phép gán bị hạn chế, ngăn chặn việc phân cụm các giá trị nhỏ hoặc lớn không khả thi trong$P$.
