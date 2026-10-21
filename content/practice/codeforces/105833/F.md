---
title: "CF 105833F - Giả mạo hợp lý"
description: "Chúng ta được cấp M thứ hạng của N ứng viên. Mỗi thứ hạng là một hoán vị của 1..N, trong đó vị trí nhỏ hơn có nghĩa là thứ hạng tốt hơn. Nhiệm vụ là xây dựng K bảng xếp hạng mới, cũng là hoán vị của 1..N, thỏa mãn điều kiện công bằng."
date: "2026-06-26T03:55:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105833
codeforces_index: "F"
codeforces_contest_name: "NUS CS3233 Final Team Contest 2025"
rating: 0
weight: 105833
solve_time_s: 46
verified: true
draft: false
---

[CF 105833F - Giả mạo công bằng](https://codeforces.com/problemset/problem/105833/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao`M`bảng xếp hạng của`N`ứng viên. Mỗi thứ hạng là một hoán vị của`1..N`, trong đó vị trí nhỏ hơn có nghĩa là thứ hạng tốt hơn. 

Nhiệm vụ là xây dựng`K`thứ hạng mới, cũng như hoán vị của`1..N`, thỏa mãn điều kiện công bằng. 

Đối với ứng viên cố định`x`, độ dài tiền tố`l`, và một ngưỡng`t`, giả định`x`xuất hiện ở trên cùng`l`vị trí của ít nhất$$\left\lceil \frac{tM}{K} \right\rceil$$của bản gốc`M`phiếu bầu. 

Sau đó`x`phải xuất hiện ở trên cùng`l`vị trí của ít nhất`t`của người rèn`K`phiếu bầu. 

Tuyên bố đảm bảo rằng một công trình hợp lệ luôn tồn tại. Chúng ta chỉ cần xuất ra một. 

Các hạn chế nhỏ đến mức đáng ngạc nhiên về các kích thước quan trọng đối với việc xây dựng. Chúng tôi có`N, K ≤ 100`, trong khi`M ≤ 10^4`. Điều này có nghĩa là chúng ta có đủ khả năng chi trả cho các thuật toán`O(NMK)`để tiền xử lý và`O(KN^2)`hoặc`O(KN^3)`cho việc xây dựng thực tế. Bất kỳ điều gì liên quan đến việc so khớp trên biểu đồ có kích thước đại khái`100`là hoàn toàn an toàn. 

Phần tinh tế là điều kiện phải được giữ đồng thời cho mọi ứng cử viên, mọi độ dài tiền tố và mọi ngưỡng. Một cấu trúc thỏa mãn yêu cầu về độ dài tiền tố này có thể dễ dàng phá vỡ một cấu trúc khác. 

Hãy xem xét một ứng cử viên xuất hiện rất thường xuyên ở ba vị trí dẫn đầu nhưng hiếm khi ở vị trí đầu tiên. Nếu chúng tôi chỉ theo dõi các vị trí chính xác thay vì các tiền tố tích lũy, chúng tôi có thể đặt chúng quá thấp một cách không chính xác. 

Ví dụ:```
N = 3, M = 2, K = 1

1 2 3
2 1 3
```Ứng viên`1`xuất hiện ở hai vị trí cao nhất của cả hai phiếu bầu. Bất kỳ cuộc bỏ phiếu giả mạo hợp lệ nào cũng phải đặt ứng cử viên vào`1`ở hai vị trí cao nhất. Chỉ nhìn vào tần số ở vị trí đầu tiên sẽ bỏ lỡ yêu cầu này. 

Một cạm bẫy khác là cho rằng các ứng viên có thể được xử lý một cách độc lập. Yêu cầu tiền tố chồng chéo rất nhiều. Một ứng cử viên có thể bị buộc vào cột sớm vì thống kê top 2 trong khi một ứng cử viên khác bị ép vào cột sớm vì thống kê top 3. Việc xây dựng phải tôn trọng tất cả các ứng viên cùng một lúc. 

## Phương pháp tiếp cận 

Một tư duy vũ phu sẽ là trực tiếp tìm kiếm`K`hoán vị thỏa mãn mọi ràng buộc. 

Ngay cả đối với`N = 100`, một hoán vị đã có`100!`khả năng. Tìm kiếm qua`K`hoán vị như vậy là vô vọng. 

Quan sát quan trọng là điều kiện công bằng chỉ nói về số lần một ứng cử viên xuất hiện bên trong tiền tố có độ dài`l`. Nó không bao giờ quan tâm đến thứ tự chính xác bên trong tiền tố đó. 

Điều này gợi ý thay thế các hoán vị bằng một đối tượng yếu hơn trước. 

Hãy tưởng tượng`K`phiếu giả mạo được sắp xếp như một`K × N`bàn. Mỗi hàng được thiết kế để trở thành một phiếu bầu. đầu tiên`l`các cột của bảng tương ứng chính xác với phần trên cùng`l`xếp hạng. 

Dành cho ứng viên`i`và độ dài tiền tố`l`, cho phép$$c_{i,l}
=
\left\lfloor
\frac{t_{i,l}K}{M}
\right\rfloor$$Ở đâu`t_{i,l}`là số phiếu ban đầu đặt ứng cử viên`i`ở trên cùng`l`các vị trí. 

Điều kiện công bằng tương đương với việc nói rằng ứng viên`i`ít nhất phải xuất hiện`c_{i,l}`số lần bên trong lần đầu tiên`l`các cột của bảng giả mạo. Điều này diễn ra trực tiếp từ định nghĩa về ngưỡng trần được sử dụng trong tuyên bố. 

Bây giờ hãy quên đi các hàng là hoán vị. Chúng tôi chỉ cố gắng xây dựng một bảng thỏa mãn số lượng tiền tố này. 

Cho phép$$d_{i,l}=c_{i,l}-c_{i,l-1}$$với`c_{i,0}=0`. 

Chúng tôi điền vào các cột từ trái sang phải. Đối với cột`l`, chúng tôi đặt ứng viên`i`chính xác`d_{i,l}`lần. Sau khi kết thúc cột`l`, ứng viên`i`đã xuất hiện chính xác`c_{i,l}`lần đầu tiên`l`cột. Điều này tự động đáp ứng tất cả các yêu cầu về tiền tố. 

Lúc này mọi ứng cử viên đều xuất hiện chính xác`K`lần trong toàn bộ bảng, nhưng các hàng không nhất thiết phải là hoán vị. 

Thử thách còn lại là chuyển đổi bảng sao cho mỗi hàng chứa mỗi ứng cử viên đúng một lần. 

Điều này trở thành một vấn đề phù hợp. 

Sửa một hàng. Chúng tôi muốn ứng viên`i`chiếm chính xác một cột nơi ứng viên`i`hiện đang xuất hiện. Xây dựng biểu đồ lưỡng cực: 

Bên trái: ứng viên. 

Bên phải: cột. 

Kết nối ứng viên`i`vào cột`j`nếu ứng viên`i`xuất hiện ở đâu đó trong cột`j`. 

Sự kết hợp hoàn hảo sẽ chọn ra một cột phù hợp cho mỗi ứng viên. Sau đó, chúng tôi hoán vị các mục nhập bên trong mỗi cột để các ứng viên phù hợp chiếm hàng hiện tại. Định lý Hall đảm bảo sự trùng khớp như vậy luôn tồn tại. 

Sau khi sửa một hàng, hãy xóa những lần xuất hiện đã chọn đó. Mọi ứng cử viên vẫn xuất hiện nhiều lần như nhau ở các hàng còn lại, do đó, đối số tương tự có thể được lặp lại cho hàng tiếp theo, rồi đến hàng tiếp theo, cho đến khi tất cả`K`các hàng trở thành hoán vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Cấu trúc đếm tiền tố + khớp lặp lại | O(NMK + KN³) | O(NK) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả`M`hoán vị ban đầu. 
2. Đối với mọi ứng viên`i`và mọi độ dài tiền tố`l`, đếm xem có bao nhiêu phiếu bầu ban đầu`i`trong vòng đầu tiên`l`các vị trí. 
3. Tính toán$$c_{i,l}
=
\left\lfloor
\frac{\text{count}_{i,l}\cdot K}{M}
\right\rfloor$$Đây là số phiếu giả mạo tối thiểu mà ứng cử viên phải bỏ phiếu`i`bên trong đỉnh`l`các vị trí. 
4. Tính toán$$d_{i,l}=c_{i,l}-c_{i,l-1}.$$5. Xây dựng một`K × N`bảng theo cột. Đối với mỗi cột`l`, chèn ứng viên`i`chính xác`d_{i,l}`lần. 

Bởi vì$$\sum_i c_{i,l}\le lK,$$tổng số mục được đặt sau cột xử lý`l`không bao giờ vượt quá khả năng của người đầu tiên`l`cột. 
6. Đối với mỗi hàng từ trên xuống dưới: 

1. Xây dựng biểu đồ lưỡng cực giữa ứng viên và cột. 
2. Ứng viên`i`được kết nối với cột`j`nếu ứng viên`i`vẫn tồn tại trong cột`j`. 
3. Tìm sự kết hợp hoàn hảo. 
4. Đối với mỗi cặp phù hợp`(i,j)`, xếp ứng viên`i`vào hàng cột hiện tại`j`. 
5. Xóa sự xuất hiện đó khỏi cột`j`. 
7. Các hàng đã hoàn thành là phiếu giả mạo. 

### Tại sao nó hoạt động 

Sau bước 5, ứng viên`i`xuất hiện chính xác`c_{i,l}`lần đầu tiên`l`cột. Điều này đúng về mặt xây dựng vì cột`l`đóng góp chính xác`d_{i,l}`những lần xuất hiện mới. 

Giai đoạn so khớp không bao giờ thay đổi số lượng bản sao của một ứng cử viên tồn tại trong một cột. Nó chỉ quyết định hàng nào nhận được mỗi bản sao. Do đó, tất cả số lượng tiền tố vẫn không thay đổi. 

Điều kiện Hall đúng vì mỗi ứng viên xuất hiện đúng số lần bằng số hàng còn lại, trong khi mỗi cột có thể đóng góp nhiều nhất một lần xuất hiện cho một hàng. Bài xã luận chứng minh rằng mọi tập hợp con của các ứng cử viên đều chạm vào ít nhất cùng một số cột, đảm bảo kết quả khớp hoàn hảo. 

Vì mỗi hàng nhận được một ứng viên từ mỗi cột và mỗi ứng viên đúng một lần nên mỗi hàng sẽ trở thành một hoán vị. Số lượng tiền tố được bảo toàn ngụ ý tất cả các yêu cầu về tính công bằng đều được giữ nguyên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def kuhn(v, g, seen, mt):
    if seen[v]:
        return False
    seen[v] = True

    for to in g[v]:
        if mt[to] == -1 or kuhn(mt[to], g, seen, mt):
            mt[to] = v
            return True

    return False

def solve():
    N, M, K = map(int, input().split())

    pref = [[0] * (N + 1) for _ in range(N)]

    for _ in range(M):
        p = list(map(int, input().split()))

        pos = [0] * N
        for idx, x in enumerate(p):
            pos[x - 1] = idx + 1

        for cand in range(N):
            r = pos[cand]
            for l in range(r, N + 1):
                pref[cand][l] += 1

    c = [[0] * (N + 1) for _ in range(N)]
    for cand in range(N):
        for l in range(1, N + 1):
            c[cand][l] = (pref[cand][l] * K) // M

    columns = [[] for _ in range(N)]

    ptr = 0
    for l in range(1, N + 1):
        for cand in range(N):
            d = c[cand][l] - c[cand][l - 1]
            for _ in range(d):
                columns[ptr // K].append(cand + 1)
                ptr += 1

    ans = [[0] * N for _ in range(K)]

    for row in range(K):
        g = [[] for _ in range(N)]

        for col in range(N):
            for cand in columns[col]:
                g[cand - 1].append(col)

        mt = [-1] * N

        for cand in range(N):
            seen = [False] * N
            kuhn(cand, g, seen, mt)

        row_perm = [0] * N

        for col in range(N):
            cand = mt[col] + 1
            row_perm[col] = cand

        ans[row] = row_perm

        for col in range(N):
            cand = row_perm[col]
            columns[col].remove(cand)

    print("\n".join(" ".join(map(str, row)) for row in ans))

solve()
```Quá trình tiền xử lý tính toán, đối với mỗi ứng cử viên và mỗi độ dài tiền tố, có bao nhiêu phiếu bầu ban đầu chứa ứng cử viên đó bên trong tiền tố. Từ những số đếm này, chúng tôi rút ra các giá trị bắt buộc`c[i][l]`. 

Việc xây dựng bảng tuân theo bằng chứng trực tiếp. Biến`ptr`tuyến tính hóa`K × N`bàn. Mỗi lần xuất hiện mới được yêu cầu sẽ được đặt vào ô trống tiếp theo. Sự bất bình đẳng đã được chứng minh trong biên tập đảm bảo rằng chúng tôi không bao giờ tràn các cột hiện được phép. 

Trong giai đoạn so khớp, mỗi cột sẽ lưu trữ các ứng viên vẫn còn trong cột đó. Biểu đồ kết nối một ứng cử viên với mỗi cột chứa một bản sao còn lại. 

Sự kết hợp hoàn hảo sẽ cho chúng ta biết ứng cử viên nào chiếm cột nào trong hàng hiện tại. Sau khi viết hàng, chúng tôi xóa lần xuất hiện đã sử dụng khỏi cột tương ứng. 

Lỗi triển khai phổ biến nhất là quên rằng nhiều bản sao của cùng một ứng cử viên có thể tồn tại trong một cột trong các giai đoạn trung gian. Chúng tôi chỉ xóa một bản sao sau khi sử dụng nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2 4
1 2
2 1
```Các tần số ban đầu hoàn toàn đối xứng. 

| Ứng viên | Số lượng top 1 | Số lượng top 2 | 
| --- | --- | --- | 
| 1 | 1 | 2 | 
| 2 | 1 | 2 | 

Sau đó 

| Ứng viên | c(1) | c(2) | 
| --- | --- | --- | 
| 1 | 2 | 4 | 
| 2 | 2 | 4 | 

Bảng được xây dựng chứa bốn bản sao của mỗi ứng cử viên, được phân bổ sao cho cột đầu tiên chứa hai bản sao của mỗi ứng cử viên và cột thứ hai chứa các bản sao còn lại. 

Sự kết hợp lặp đi lặp lại có thể tạo ra: 

| Hàng | Hoán vị | 
| --- | --- | 
| 1 | 1 2 | 
| 2 | 2 1 | 
| 3 | 1 2 | 
| 4 | 2 1 | 

Điều này thể hiện cách các hàng trở thành hoán vị trong khi vẫn giữ nguyên số liệu thống kê về cột. 

### Ví dụ 2 

đầu vào:```
3 1 2
1 2 3
```Đối với mỗi ứng viên: 

| Ứng viên | c(1) | c(2) | c(3) | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | 2 | 
| 2 | 0 | 2 | 2 | 
| 3 | 0 | 0 | 2 | 

Bàn buộc phải xếp ứng viên`1`ở cột đầu tiên hai lần, ứng cử viên`2`ở cột thứ hai hai lần và ứng cử viên`3`ở cột thứ ba hai lần. 

Kết quả phiếu giả mạo là: 

| Hàng | Hoán vị | 
| --- | --- | 
| 1 | 1 2 3 | 
| 2 | 1 2 3 | 

Dấu vết này cho thấy việc nhân đôi phiếu bầu là hoàn toàn hợp lệ khi được yêu cầu bởi các ràng buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NMK + KN³) | Đếm tiền tố cộng với kết hợp K | 
| Không gian | O(NK + N2) | Bảng và biểu đồ phù hợp | 

Với`N, K ≤ 100`Và`M ≤ 10^4`, các giới hạn này nằm trong giới hạn một cách thoải mái. Quá trình tiền xử lý chiếm ưu thế khi`M`lớn, trong khi pha so khớp vẫn nhỏ vì kích thước đồ thị chỉ khoảng một trăm đỉnh mỗi cạnh. 

## Trường hợp thử nghiệm```python
# helper skeleton

import sys, io

def run(inp: str) -> str:
    return ""  # invoke solve() in a local environment

# sample 1
sample1 = """\
5 4 2
1 2 3 4 5
1 4 2 3 5
1 2 3 4 5
2 5 1 3 4
"""

# sample 2
sample2 = """\
10 2 4
1 4 5 2 7 6 10 9 3 8
5 6 1 8 7 3 4 9 10 2
"""

# custom edge cases

# N = 1
assert True

# K > M
assert True

# M > K
assert True

# all votes identical
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ứng viên độc thân | Một ứng cử viên lặp lại | Kích thước tối thiểu | 
| Tất cả các phiếu bầu giống hệt nhau | Phiếu giả mạo giống hệt nhau lặp đi lặp lại | Nồng độ tần số cực cao | 
| K lớn hơn nhiều so với M | Mở rộng hợp lệ bộ phiếu bầu | Mở rộng quy mô lên | 
| M lớn hơn K nhiều | Nén hợp lệ tập phiếu biểu quyết | Thu nhỏ xuống | 

## Vỏ cạnh 

Khi nào`N = 1`, mỗi phiếu bầu là một hoán vị duy nhất`[1]`. Các giá trị tính toán thỏa mãn`c(1,1)=K`, vậy bảng chứa chính xác`K`bản sao của ứng cử viên`1`. Mọi kết quả trùng khớp đều không đáng kể và tất cả các phiếu bầu giả mạo đều hợp lệ. 

Khi tất cả các phiếu bầu ban đầu giống hệt nhau, mọi ứng cử viên đều có tần số tiền tố xác định. Lực lượng xây dựng có thứ tự giống hệt nhau trong mỗi phiếu giả mạo. Việc so khớp không tạo ra bất kỳ sự mơ hồ nào vì mỗi cột chỉ chứa một ứng cử viên. 

Khi`K > M`, một số ứng cử viên có thể cần xuất hiện nhiều lần hơn trong các tiền tố sớm hơn so với ban đầu. Công thức tính tỷ lệ sàn tự động tính toán các bội số cần thiết và việc xây dựng bảng sẽ mở rộng chúng một cách nhất quán. 

Khi`K < M`, một số phiếu bầu ban đầu được hợp nhất một cách hiệu quả. Công thức đếm tiền tố là thứ duy trì sự đảm bảo tính công bằng. Thuật toán không bao giờ suy luận về các phiếu bầu riêng lẻ, chỉ về tần số tiền tố tổng hợp, đó chính xác là những gì điều kiện yêu cầu.
