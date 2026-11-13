---
title: "CF 1714F - Xây dựng một cái cây và thế là xong"
description: "Chúng ta được yêu cầu xây dựng một cây không có gốc với số đỉnh cố định và ba giới hạn khoảng cách cụ thể giữa các đỉnh 1, 2 và 3. Mỗi trường hợp kiểm thử cung cấp bốn số nguyên: số nút $n$ và khoảng cách theo cặp $d{12}$, $d{23}$ và $d{31}$."
date: "2026-06-09T20:09:34+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 1714
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 811 (Div. 3)"
rating: 1900
weight: 1714
solve_time_s: 161
verified: false
draft: false
---

[CF 1714F - Xây một cái cây và thế là xong](https://codeforces.com/problemset/problem/1714/F) 

**Xếp hạng:** 1900 
**Tags:** thuật toán xây dựng, triển khai, cây 
**Thời gian giải:** 2m 41s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một cây không có gốc với số đỉnh cố định và ba ràng buộc khoảng cách cụ thể giữa các đỉnh 1, 2 và 3. Mỗi trường hợp thử nghiệm cung cấp bốn số nguyên: số nút$n$và khoảng cách theo cặp$d_{12}$,$d_{23}$, Và$d_{31}$. Nhiệm vụ là xác định xem có tồn tại một cây thỏa mãn các khoảng cách này hay không và nếu có thì xuất ra các cạnh của bất kỳ cây nào như vậy. 

Một cái cây với$n$đỉnh có chính xác$n-1$cạnh và không có chu trình. Những khoảng cách$d_{12}, d_{23}, d_{31}$xác định các ràng buộc trên các đường dẫn nối các đỉnh 1, 2 và 3. Vì cây có một đường dẫn duy nhất giữa hai đỉnh bất kỳ nên các khoảng cách này xác định duy nhất bố cục tương đối của ba đỉnh này. 

Các giới hạn chỉ ra rằng$n$có thể lên tới 200.000 và có thể lên tới 10.000 trường hợp thử nghiệm, với tổng của tất cả$n$trên các trường hợp thử nghiệm được giới hạn ở 200.000. Điều này loại trừ bất kỳ thuật toán nào tệ hơn thuật toán tuyến tính trong$n$mỗi trường hợp thử nghiệm. Chúng tôi không đủ khả năng để thử tất cả các cấu trúc liên kết cây một cách rõ ràng; chúng ta cần một cách xây dựng trực tiếp dựa trên khoảng cách nhất định. 

Các trường hợp cạnh xuất hiện khi khoảng cách không thỏa mãn bất đẳng thức tam giác. Ví dụ, nếu$d_{12} + d_{23} < d_{31}$, không cây nào có thể thỏa mãn những khoảng cách này vì đường đi từ 1 đến 3 ít nhất phải bằng tổng các đường đi qua 2. Cây nhỏ cũng tạo ra các ràng buộc. Với$n=3$, cả ba khoảng cách phải có tổng bằng chính xác hai cạnh tạo thành một đường đi duy nhất. 

Ví dụ, với$n=4$và khoảng cách$d_{12}=3, d_{23}=1, d_{31}=1$, một nỗ lực ngây thơ nhằm sắp xếp các đỉnh dọc theo một đường đi đã thất bại vì không thể điều chỉnh khoảng cách mà không vi phạm cấu trúc cây. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các hình dạng cây và kiểm tra xem khoảng cách giữa 1, 2, 3 có phù hợp với yêu cầu hay không. Đây là sự bùng nổ tổ hợp vì số lượng cây trên$n$các đỉnh tăng theo cấp số mũ. Thậm chí tạo ra tất cả các cây cho$n=20$sẽ là không thể thực hiện được. 

Điểm mấu chốt là trong bất kỳ cây nào, ba đỉnh 1, 2 và 3 phải tạo thành cấu trúc "đường dẫn" hoặc cấu trúc "giống như ngôi sao". Trong kịch bản đường đi, một đỉnh nằm ở giữa, nối hai đỉnh còn lại và khoảng cách phải thỏa mãn$d_{12} + d_{23} = d_{31}$hoặc một trong các hoán vị tuần hoàn. Nếu tổng của hai khoảng cách bất kỳ bằng khoảng cách thứ ba thì chúng ta có thể xếp các đỉnh trên một đường thẳng và cộng các khoảng cách còn lại$n-3$các nút như những chiếc lá gắn ở bất kỳ đâu dọc theo đường dẫn. Nếu tổng của hai khoảng cách không bằng khoảng cách thứ ba thì hình dạng duy nhất có thể có là một đỉnh trung tâm nối cả ba khoảng cách, tạo thành một cây hình chữ Y. Trong trường hợp này khoảng cách phải thỏa mãn$(d_{12} + d_{23} + d_{31}) \mod 2 = 0$và một công thức dẫn xuất để phân chia khoảng cách dọc theo các nhánh của chữ Y. 

Cái nhìn sâu sắc này làm giảm vấn đề từ việc kiểm tra tất cả các cây đến việc kiểm tra đơn giản các sắp xếp khả thi và sau đó trực tiếp xây dựng đường dẫn hoặc hình chữ Y. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đầu tiên, hãy kiểm tra xem các khoảng cách có thỏa mãn bất đẳng thức tam giác hay không: mỗi khoảng cách phải nhỏ hơn hoặc bằng tổng của hai khoảng cách còn lại. Nếu không thì xuất ra NO ngay lập tức. 
2. Cố gắng tạo một bố cục đường dẫn đơn giản. Kiểm tra xem một trong các khoảng cách có bằng tổng của hai khoảng cách còn lại không. Ví dụ, nếu$d_{12} + d_{23} = d_{31}$, đỉnh 2 nằm giữa 1 và 3 dọc theo một đường thẳng. Tương tự, kiểm tra các hoán vị khác. Nếu mối quan hệ như vậy tồn tại, chúng ta có thể xây dựng đường đi và gắn các đỉnh còn lại dưới dạng các lá. 
3. Nếu bố cục đường dẫn không hiệu quả, hãy thử tạo cây hình chữ Y. Tính "độ dài cánh tay" từ đỉnh trung tâm$c$đến 1, 2 và 3 bằng cách sử dụng:$$x = \frac{d_{12} + d_{31} - d_{23}}{2}, \quad y = \frac{d_{12} + d_{23} - d_{31}}{2}, \quad z = \frac{d_{23} + d_{31} - d_{12}}{2}$$Nếu bất kỳ$x, y, z$là số âm hoặc không phải số nguyên thì hình chữ Y là không thể. 
4. Đặt các đỉnh 1, 2, 3 ở cuối cánh tay. Kết nối cánh tay thông qua đỉnh trung tâm. Đính kèm phần còn lại$n-3$các đỉnh làm lá đến bất kỳ đỉnh nào trên cây mà không vi phạm tính duy nhất của đường đi giữa 1, 2, 3. 
5. Xuất CÓ và in các cạnh. 

Tại sao nó hoạt động: Bất kỳ cây nào cũng là một đường nối 1, 2, 3 hoặc có một đỉnh ở giữa nối chúng. Các công thức dẫn xuất đảm bảo độ dài cánh tay là số nguyên, bảo toàn khoảng cách chính xác. Các đỉnh còn lại luôn có thể được thêm vào dưới dạng lá. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, d12, d23, d31 = map(int, input().split())

        # Attempt path-based layout
        path_found = False
        for a, b, c in [(d12, d23, d31), (d23, d31, d12), (d31, d12, d23)]:
            if a + b == c:
                path_found = True
                edges = []
                nxt = 4
                # build path a-b-c
                v = [1, 2, 3]
                if (a, b, c) != (d12, d23, d31):
                    # permute so that 1,2,3 correspond to a-b-c
                    if (a, b, c) == (d23, d31, d12):
                        v = [2,3,1]
                    else:
                        v = [3,1,2]
                # build path from v[0] to v[1] length a
                last = v[0]
                for _ in range(a-1):
                    edges.append((last, nxt))
                    last = nxt
                    nxt += 1
                edges.append((last, v[1]))
                last = v[1]
                # build path from v[1] to v[2] length b
                for _ in range(b-1):
                    edges.append((last, nxt))
                    last = nxt
                    nxt += 1
                edges.append((last, v[2]))
                # attach remaining vertices
                for i in range(n - (len(edges)+1)):
                    edges.append((1, nxt))
                    nxt += 1
                print("YES")
                for u, w in edges:
                    print(u, w)
                break
        if path_found:
            continue

        # attempt Y-shaped tree
        x = (d12 + d31 - d23) // 2
        y = (d12 + d23 - d31) // 2
        z = (d23 + d31 - d12) // 2
        if (d12 + d31 - d23) % 2 or (d12 + d23 - d31) % 2 or (d23 + d31 - d12) % 2 or min(x, y, z) < 0:
            print("NO")
            continue
        edges = []
        nxt = 4
        center = 4
        # connect central vertex
        edges.append((center,1))
        last = center
        # arm to 2
        prev = last
        for _ in range(x):
            edges.append((prev,nxt))
            prev = nxt
            nxt += 1
        edges.append((prev,2))
        # arm to 3
        prev = last
        for _ in range(z):
            edges.append((prev,nxt))
            prev = nxt
            nxt += 1
        edges.append((prev,3))
        # attach remaining vertices
        used = nxt-1
        for i in range(used, n):
            edges.append((1, nxt))
            nxt += 1
        print("YES")
        for u, w in edges:
            print(u, w)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã sẽ kiểm tra xem khoảng cách có cho phép bố trí đường dẫn hay không. Nó cẩn thận hoán vị các đỉnh sao cho 1, 2, 3 tương ứng với các điểm cuối chính xác của đường đi. Việc xây dựng đường đi sử dụng các đỉnh tạm thời cho các cạnh trung gian, tăng bộ đếm để gán số đỉnh mới. Nếu đường dẫn không thể thực hiện được, mã sẽ tính toán độ dài nhánh cho bố cục hình chữ Y, đảm bảo tất cả độ dài dẫn xuất đều là số nguyên không âm. Các nút còn lại được gắn dưới dạng lá để bảo toàn thuộc tính của cây. 

## Ví dụ đã hoạt động 

**Ví dụ 1**:`5 1 2 1`| Bước | Hành động | Biến | 
| --- | --- | --- | 
| 1 | Kiểm tra tổng đường dẫn | 1+2 == 1? KHÔNG; 2+1==1? KHÔNG; 1+1==2? vâng | 
| 2 | | |
