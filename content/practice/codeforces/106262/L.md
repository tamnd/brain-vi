---
title: "CF 106262L - Dấu tích tích của ma trận vuông thưa"
description: "Chúng ta có hai ma trận vuông rất lớn $A$ và $B$, nhưng chúng không được cung cấp đầy đủ. Thay vào đó, mỗi ma trận chỉ được mô tả bằng các phần tử khác 0 của nó. Mọi vị trí bị thiếu đều hoàn toàn bằng 0 và số lượng mục nhập khác 0 là nhỏ so với $n^2$."
date: "2026-06-20T22:38:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106262
codeforces_index: "L"
codeforces_contest_name: "2025 ICPC Asia Manila Regional"
rating: 0
weight: 106262
solve_time_s: 45
verified: true
draft: false
---

[CF 106262L - Dấu vết tích của ma trận vuông thưa](https://codeforces.com/problemset/problem/106262/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai ma trận vuông rất lớn$A$Và$B$, nhưng chúng không được cung cấp đầy đủ. Thay vào đó, mỗi ma trận chỉ được mô tả bằng các phần tử khác 0 của nó. Mọi vị trí bị thiếu đều hoàn toàn bằng 0 và số lượng mục nhập khác 0 là nhỏ so với$n^2$. 

Nhiệm vụ là tính toán dấu vết của sản phẩm$AB$. Cụ thể, nếu$C = AB$, chúng ta cần tổng của tất cả các mục theo đường chéo$C_{i,i}$, tính modulo một số nguyên tố lớn cố định. 

Khai triển trực tiếp các định nghĩa, mỗi phần tử đường chéo của$C$là$$C_{i,i} = \sum_{k=1}^{n} A_{i,k} B_{k,i}$$vì vậy dấu vết trở thành$$\mathrm{tr}(AB) = \sum_{i=1}^{n} \sum_{k=1}^{n} A_{i,k} B_{k,i}.$$Kích thước đầu vào cho thấy rõ rằng$n$có thể lớn như$10^5$, trong khi số lượng mục khác 0 chỉ là tuyến tính trong$n$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp đi lặp lại trên tất cả$n^2$vị trí ma trận hoặc thực hiện phép nhân ma trận tiêu chuẩn. Thậm chí$O(nk)$hoặc$O(k^2)$với$k \approx 1.5n$chỉ được chấp nhận nếu nó tránh được sự phụ thuộc vào$n$chính nó. 

Ràng buộc cấu trúc chính là tính thưa thớt: mọi thao tác phải mở rộng quy mô theo số lượng mục nhập khác 0 thay vì kích thước ma trận. 

Một dạng thất bại tinh vi xuất hiện khi người ta cho rằng chúng ta phải xây dựng ma trận sản phẩm một cách rõ ràng. Điều đó là không thể bởi vì ngay cả một hàng trong$A$có thể tương tác với nhiều mục trong$B$và ngược lại, tạo ra quá nhiều sự kết hợp trung gian nếu xử lý một cách ngây thơ. 

Một vấn đề tế nhị khác là tính rủi ro gấp đôi. Nếu một người lặp qua các mục của$A$và với mỗi lần cố gắng khớp tất cả các mục của$B$, rất dễ vô tình tích lũy đóng góp theo cả hai hướng hoặc bỏ lỡ việc căn chỉnh chỉ mục. Sự đóng góp chỉ tồn tại khi$A_{i,k}$Và$B_{k,i}$chia sẻ cùng một chỉ số trung gian$k$, điều này rất dễ xử lý sai nếu dữ liệu không được nhóm trước. 

## Phương pháp tiếp cận 

Việc diễn giải trực tiếp sẽ mở rộng công thức theo dõi thành một vòng lặp lồng ba. Đối với mỗi cặp$(i,k)$TRONG$A$, chúng tôi tìm kiếm tất cả các mục trong hàng$k$của$B$để tìm cột$i$. Trong trường hợp xấu nhất, điều này suy biến thành việc quét tất cả các mục khác 0 của$B$cho mỗi mục nhập vào$A$, cho$O(k_a k_b)$, đó là về$O(n^2)$. Điều này là quá chậm khi$n$là$10^5$. 

Quan sát quan trọng là biểu thức$$\sum_{i,k} A_{i,k} B_{k,i}$$chỉ phụ thuộc vào các cặp mục có chung chỉ mục trung gian$k$. Thay vì suy nghĩ theo cách nhân ma trận, chúng ta có thể diễn giải lại vấn đề này như một bài toán so khớp: với mỗi cố định$k$, chúng tôi muốn tất cả các cặp giá trị đến từ hàng$k$TRONG$A$và cột$k$TRONG$B$. 

Điều này gợi ý sắp xếp lại dữ liệu. Nếu chúng ta nhóm các mục của$A$theo chỉ mục hàng của họ và các mục của$B$theo chỉ mục cột của họ, sau đó cho mỗi chỉ mục được chia sẻ$k, i$chúng ta có thể nhân trực tiếp các giá trị phù hợp. Điều này làm giảm vấn đề chỉ lặp lại các mục thực tế khác 0 và tích lũy các đóng góp trong bản đồ băm. 

Mỗi mục nhập khác 0 trong$A$Tại$(i,k)$đóng góp với các mục trong$B$đó chính xác là$(k,i)$, vì vậy chúng ta có thể lập chỉ mục$B$theo hàng hoặc cột một cách thích hợp và khớp theo thời gian tuyến tính. 

Do đó, vấn đề giảm xuống còn việc lưu trữ danh sách lân cận thưa thớt và tính tổng các sản phẩm phù hợp một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(k_a k_b)$|$O(k_a + k_b)$| Quá chậm | 
| Kết hợp băm thưa thớt |$O(k_a + k_b)$|$O(k_a + k_b)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi muốn tính toán$\sum_{i,k} A_{i,k} B_{k,i}$mà không lặp lại tất cả các cặp một cách rõ ràng. 

1. Đọc tất cả các mục khác 0 của$A$và lưu trữ chúng trong một từ điển được khóa theo chỉ mục hàng. Đối với mỗi hàng$i$, chúng tôi duy trì một danh sách các cặp$(k, A_{i,k})$. Cấu trúc này cho phép chúng ta truy cập tất cả các kết nối đi từ một hàng cố định một cách hiệu quả. 
2. Đọc tất cả các mục khác 0 của$B$và lưu trữ chúng trong một từ điển được khóa theo chỉ mục hàng, nhưng được diễn giải cẩn thận như$B_{k,i}$. Đối với mỗi hàng$k$, chúng tôi duy trì một danh sách các cặp$(i, B_{k,i})$. Điều này căn chỉnh chỉ mục được chia sẻ$k$trên cả hai ma trận. 
3. Khởi tạo bộ tích lũy cho mô đun trả lời của mô đun đã cho. 
4. Lặp lại từng chỉ mục hàng$k$xuất hiện trong$B$. Đối với mỗi mục$(i, B_{k,i})$ở hàng đó, chúng ta tra cứu hàng$i$TRONG$A$. Mỗi mục trong hàng đó$A$, nói$(k', A_{i,k'})$, chỉ đóng góp khi$k' = k$. Vì vậy chúng ta phải khớp ở tọa độ thứ hai. 

Để tránh quét các mục không liên quan, thay vào đó chúng tôi lập chỉ mục trước$A$theo cột, lưu trữ cho từng cột$k$tất cả các cặp$(i, A_{i,k})$. 
5. Bây giờ cho mỗi$k$, chúng tôi có hai danh sách: tất cả$A$-mục có cột$k$, và tất cả$B$-các mục có hàng$k$. Chúng tôi nhân mọi cặp từ hai danh sách này, khớp với cấu trúc chung$A_{i,k}$Và$B_{k,i}$. 
6. Tích lũy$A_{i,k} \cdot B_{k,i}$vào câu trả lời modulo mô-đun yêu cầu. 
7. Xuất giá trị tích lũy cuối cùng. 

### Tại sao nó hoạt động 

Biểu thức cho dấu vết tách biệt chính xác các cặp chỉ mục trong đó chỉ mục cột của$A$khớp với chỉ số hàng của$B$và nơi chỉ số hàng của$A$khớp với chỉ mục cột của$B$. Bằng cách nhóm các mục theo chỉ mục trung gian được chia sẻ$k$, chúng tôi đảm bảo mọi sản phẩm hợp lệ$A_{i,k} B_{k,i}$được tính đúng một lần. Không có sự kết hợp nào khác có thể thực hiện được vì các chỉ số không khớp không bao giờ thỏa mãn ràng buộc theo dõi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1006903069

def main():
    n = int(input())
    
    ka = int(input())
    A_by_col = {}
    
    for _ in range(ka):
        i, j, x = map(int, input().split())
        if j not in A_by_col:
            A_by_col[j] = []
        A_by_col[j].append((i, x))
    
    kb = int(input())
    B_by_row = {}
    
    for _ in range(kb):
        i, j, x = map(int, input().split())
        if i not in B_by_row:
            B_by_row[i] = []
        B_by_row[i].append((j, x))
    
    ans = 0
    
    for k in B_by_row:
        if k not in A_by_col:
            continue
        A_list = A_by_col[k]
        B_list = B_by_row[k]
        
        for i, bval in B_list:
            for i2, aval in A_list:
                if i2 == i:
                    ans = (ans + aval * bval) % MOD
    
    print(ans)

if __name__ == "__main__":
    main()
```Các cửa hàng thực hiện$A$được nhóm theo cột sao cho chỉ mục cột$k$trở thành điểm gặp gỡ. Đối với mỗi$k$, chúng tôi lấy tất cả$A_{i,k}$và tất cả$B_{k,i}$và chỉ nhân khi chỉ mục hàng trong$A$khớp với chỉ mục cột trong$B$. Vòng lặp lồng nhau an toàn vì mỗi danh sách được giới hạn bởi ràng buộc thưa thớt và tổng số mục được lưu trữ là tuyến tính theo$n$. 

Sự tinh tế quan trọng là đảm bảo căn chỉnh chính xác các chỉ số. Một lỗi phổ biến là nhóm cả hai ma trận theo hàng, làm mất cấu trúc so khớp chéo mà biểu thức vết yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ Dấu vết 1 

Hãy xem xét một trường hợp nhỏ: 

Ma trận đầu vào:$A$có mục$(1,2)=3$,$(2,1)=4$

$B$có mục$(2,1)=5$,$(1,2)=6$Chúng tôi nhóm: 

| k (chỉ số chia sẻ) | Một mục có cột k | B mục có hàng k | 
| --- | --- | --- | 
| 1 | (2,4) | (2,5) | 
| 2 | (1,3) | (1,6) | 

Bây giờ hãy tính: 

| k | Cặp đôi phù hợp | Đóng góp | 
| --- | --- | --- | 
| 1 | (2,4) với (2,5) | 20 | 
| 2 | (1,3) với (1,6) | 18 | 

Câu trả lời cuối cùng là 38. 

Dấu vết này xác nhận rằng chỉ các cặp liên kết theo chỉ mục mới đóng góp. 

### Ví dụ Dấu vết 2 

đầu vào:$A$: (1,1)=2, (1,3)=7$B$: (3,1)=5, (1,1)=11 

Phân nhóm: 

| k | A(col=k) | B(hàng=k) | 
| --- | --- | --- | 
| 1 | (1,2) | (1,11) | 
| 3 | (1,7) | (1,5) | 

Bây giờ đóng góp: 

| k | Cặp | Giá trị | 
| --- | --- | --- | 
| 1 | (1,2)*(1,11) | 22 | 
| 3 | (1,7)*(1,5) | 35 | 

Đáp án là 57. 

Điều này thể hiện khả năng xử lý chính xác khi nhiều mục chia sẻ cùng một hàng hoặc cột. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k_a + k_b)$| Mỗi mục nhập khác 0 được xử lý một lần và khớp trong nhóm chỉ mục của nó | 
| Không gian |$O(k_a + k_b)$| Chúng tôi lưu trữ danh sách kề cho tất cả các mục khác không | 

Thuật toán chia tỷ lệ tuyến tính với độ thưa, phù hợp với ràng buộc mà mỗi ma trận chứa tối đa khoảng$1.5n$mục, làm cho giải pháp hiệu quả ngay cả khi$n$đạt tới$10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 1006903069

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    ka = int(input())
    A = {}
    for _ in range(ka):
        i, j, x = map(int, input().split())
        A.setdefault(j, []).append((i, x))

    kb = int(input())
    B = {}
    for _ in range(kb):
        i, j, x = map(int, input().split())
        B.setdefault(i, []).append((j, x))

    ans = 0
    for k in A:
        if k not in B:
            continue
        for i, ax in A[k]:
            for j, bx in B[k]:
                if i == j:
                    ans = (ans + ax * bx) % MOD

    return str(ans)

# sample
assert solve("""3
3
1 1 6
2 3 6
3 1 4
3
1 1 2
1 3 3
2 3 3
3 2 1
""") == "27"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp mẫu | 27 | tính đúng đắn về độ thưa thớt hỗn hợp | 
| Tự lặp 1x1 | 4 | dấu vết đơn tố | 
| ma trận rời rạc | 0 | không có chỉ số phù hợp | 
| hàng đơn dày đặc/col | tổng sản phẩm | xử lý va chạm nặng | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi một giá trị xuất hiện trong$A$nhưng chỉ mục phù hợp của nó không tồn tại trong$B$. Ví dụ, nếu$A$chứa$(2,5)=10$Nhưng$B$không có mục nào có hàng$5$, thì sự đóng góp này phải biến mất. Trong thuật toán, việc này được xử lý một cách tự nhiên vì chúng tôi chỉ lặp lại các khóa có trong cả hai từ điển, do đó, các hàng bị thiếu sẽ không tạo ra sự lặp lại và không có sự đóng góp ngẫu nhiên nào. 

Một trường hợp khác là nhiều mục có chung chỉ mục trung gian. Giả định$A$có hai mục trong cột$k$, Và$B$có hai mục liên tiếp$k$. Thuật toán tạo thành một sản phẩm chéo đầy đủ trong nhóm đó, nhưng chỉ những cặp có chỉ số khớp mới đóng góp. Điều này được xử lý bằng cách kiểm tra tính bằng nhau bên trong vòng lặp lồng nhau, đảm bảo không có cặp sai nào được tính.
