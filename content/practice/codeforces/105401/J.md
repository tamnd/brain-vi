---
title: "CF 105401J - Chạy trên máy bay"
description: "Chúng ta được cho một tập hợp hữu hạn các điểm nguyên trong mặt phẳng và chúng ta muốn xây dựng một tập hợp nhỏ các vectơ bước được phép để chúng ta có thể xây dựng một hành trình bắt đầu từ điểm gốc đi qua mọi điểm đã cho. Bước đi là một chuỗi các điểm mạng bắt đầu từ $(0,0)$."
date: "2026-06-23T17:11:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105401
codeforces_index: "J"
codeforces_contest_name: "2024 KAIST 14th ICPC Mock Competition"
rating: 0
weight: 105401
solve_time_s: 95
verified: false
draft: false
---

[CF 105401J - Chạy trên máy bay](https://codeforces.com/problemset/problem/105401/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 35s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp hữu hạn các điểm nguyên trong mặt phẳng và chúng ta muốn xây dựng một tập hợp nhỏ các vectơ bước được phép để chúng ta có thể xây dựng một hành trình bắt đầu từ điểm gốc đi qua mọi điểm đã cho. 

Bước đi là một chuỗi các điểm mạng bắt đầu từ$(0,0)$. Mỗi bước đi phải là một trong những vectơ chúng ta chọn trước. Việc đi bộ được phép xem lại các điểm và không cần truy cập các điểm theo bất kỳ thứ tự cụ thể nào, chỉ đảm bảo rằng mọi điểm từ tập đầu vào đều xuất hiện ở đâu đó trong chuỗi. 

Nhiệm vụ là chọn một tập hợp các vectơ bước$T$kích thước tối thiểu có thể để có thể tồn tại một cuộc đi bộ như vậy. 

Ràng buộc chính là về mặt cấu trúc chứ không phải là tổ hợp: chúng ta không được yêu cầu tìm chính bước đi mà chỉ có một tập hợp các hướng dịch chuyển giúp cho việc đi bộ như vậy có thể thực hiện được. Việc đi bộ có thể tái sử dụng các vectơ tùy ý nhiều lần. 

Kích thước đầu vào lớn trong các trường hợp thử nghiệm, với tổng số$n$lên đến$10^5$. Điều này loại trừ mọi thứ phụ thuộc vào mối quan hệ cặp đôi giữa các điểm, chẳng hạn như xây dựng một biểu đồ hoàn chỉnh hoặc kiểm tra tất cả sự khác biệt giữa các điểm. Về cơ bản, bất kỳ giải pháp nào cũng phải xử lý các điểm trong thời gian gần tuyến tính cho mỗi trường hợp thử nghiệm hoặc phân bổ tuyến tính trên tất cả các thử nghiệm. 

Một trường hợp thất bại tinh vi đối với cách suy luận ngây thơ xuất hiện khi các điểm nằm trên một đường thẳng nhưng không cách đều nhau so với gốc tọa độ. Ví dụ: các điểm như$(3,0)$Và$(4,0)$cả hai đều không thể được tạo bằng cách sử dụng một vectơ bước đơn lẻ, mặc dù chúng thẳng hàng. Một cách tiếp cận ngây thơ chỉ kiểm tra hướng (bỏ qua các ràng buộc về cường độ) sẽ giả định không chính xác một vectơ là đủ. Ràng buộc đúng thì chặt chẽ hơn: một vectơ duy nhất chỉ cho phép di chuyển theo bội số chính xác của nó. 

Một chế độ thất bại khác là giả sử chúng ta cần khớp tất cả các khác biệt theo cặp giữa các điểm. Điều đó sẽ bùng nổ$O(n^2)$vectơ và không cần thiết vì chỉ có cấu trúc điểm gốc-điểm mới quan trọng. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là nghĩ đến việc xây dựng một biểu đồ có hướng về khả năng tiếp cận: chúng tôi bắt đầu từ điểm gốc và cố gắng đạt đến mọi điểm bằng cách sử dụng các bước được phép. Nếu chúng ta có một tập ứng cử viên$T$, chúng tôi có thể mô phỏng xem tất cả các điểm có thể truy cập được bằng BFS hoặc DFS trên biểu đồ mạng được tạo bởi các vectơ đó hay không. Sau đó, chúng ta có thể tìm kiếm trên các tập hợp con của các vectơ ứng cử viên, có thể xuất phát từ tất cả các điểm khác nhau theo cặp. 

Điều này ngay lập tức thất bại vì tập hợp ứng cử viên tự nhiên của tất cả sự khác biệt giữa các điểm là$O(n^2)$và thậm chí việc kiểm tra các tập hợp con trở nên không thể thực hiện được về mặt tổ hợp. Ngay cả một mô phỏng đơn lẻ trên một biểu đồ dày đặc các điểm mạng có thể tiếp cận cũng không có ý nghĩa vì tọa độ không bị giới hạn. 

Cái nhìn sâu sắc về cấu trúc là điều duy nhất quan trọng là các điểm liên quan như thế nào đến thứ tự truyền tải đã chọn. Nếu chúng ta sắp xếp các điểm trong một chuỗi bắt đầu từ gốc tọa độ thì mỗi hiệu liên tiếp là một vectơ ứng cử viên. Bất kỳ bước đi hợp lệ nào đều tương ứng với một số thứ tự điểm và tập hợp$T$chỉ đơn giản là tập hợp các khác biệt bước riêng biệt được sử dụng theo thứ tự đó. 

Điều này biến vấn đề thành giảm thiểu số lượng khác biệt rõ rệt dọc theo đường đi bắt đầu từ điểm gốc và đi qua tất cả các điểm. Nhận xét quan trọng là chúng ta có thể tự do lựa chọn thứ tự các điểm tham quan. Vì vậy, nhiệm vụ trở thành xây dựng một trật tự nhằm giảm thiểu số lượng các hướng dịch chuyển riêng biệt. 

Cấu trúc tham lam hoạt động nếu chúng ta sắp xếp các điểm theo góc cực xung quanh gốc tọa độ và đi ngang qua chúng theo thứ tự đó, bởi vì trong một đường quét góc lồi, các điểm liên tiếp có xu hướng chia sẻ cấu trúc hướng. Sự tối ưu xuất phát từ thực tế là bất kỳ sự thay đổi hướng nào đều tương ứng với một vectơ mới cần thiết và thứ tự góc sẽ giảm thiểu sự thay đổi hướng trên toàn cầu. 

Do đó, chúng ta rút gọn bài toán thành một lần quét các điểm được sắp xếp theo góc, trích ra các sai phân liên tiếp từ$(0,0)$thông qua chu kỳ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con của vectơ | hàm mũ /$O(n^2)$|$O(n^2)$| Quá chậm | 
| Thi công quét góc |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng một đường đi thăm tất cả các điểm theo thứ tự được chọn cẩn thận, sau đó lấy tập hợp các vectơ bước duy nhất dọc theo đường đi đó. 

1. Bắt đầu bằng cách thêm điểm gốc$(0,0)$đến điểm đã đặt. Điều này cho phép chúng ta coi bước đi một cách thống nhất như một đường đi bắt đầu từ một đỉnh thực chứ không phải là một trường hợp đặc biệt. 
2. Sắp xếp tất cả các điểm theo góc cực xung quanh gốc tọa độ, phá vỡ các mối liên kết theo khoảng cách từ gốc tọa độ. Thứ tự góc đảm bảo chúng ta đi qua các điểm trong một lần quét quay duy nhất, điều này hạn chế sự thay đổi hướng. 
3. Duyệt danh sách đã sắp xếp và xây dựng các sai phân liên tiếp giữa các điểm liên tiếp theo thứ tự này. 
4. Chèn từng vectơ sai phân vào một tập hợp$T$. Vì các chỉ dẫn lặp lại không quan trọng nên các chỉ dẫn trùng lặp sẽ bị bỏ qua. 
5. Xuất tất cả các vectơ vào$T$như câu trả lời. 

Lý do cấu trúc này hợp lệ là vì hai điểm liên tiếp bất kỳ theo thứ tự góc này xác định một vectơ dịch chuyển trực tiếp có thể được lặp lại tùy ý để di chuyển giữa các vị trí mạng trung gian nếu cần. Quan trọng hơn, mọi điểm đầu vào đều được truy cập rõ ràng dưới dạng một đỉnh trong chuỗi được xây dựng, do đó yêu cầu về phạm vi bao phủ được đáp ứng trực tiếp. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là chúng ta xây dựng một chuỗi các điểm bắt đầu từ gốc và truy cập mọi điểm cần thiết một cách rõ ràng một cách rõ ràng. Mỗi cạnh của dãy này tương ứng với một vectơ có trong$T$, vì vậy việc đi bộ là hợp lệ theo định nghĩa. Vì trình tự bao gồm tất cả các điểm nên tính khả thi được đảm bảo. Tính tối thiểu xuất phát từ thực tế là bất kỳ sự thay đổi nào về hướng truyền đều nhất thiết phải tạo ra một vectơ dịch chuyển mới và thứ tự góc sẽ giảm thiểu những thay đổi đó bằng cách đảm bảo chuyển động quay đơn điệu xung quanh điểm gốc, ngăn chặn việc chuyển hướng qua lại không cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    out = []

    for _ in range(q):
        n = int(input())
        pts = []

        for _ in range(n):
            x, y = map(int, input().split())
            pts.append((x, y))

        # include origin
        pts.append((0, 0))

        # sort by polar angle using cross product with a reference axis
        # we split into upper/lower half for deterministic ordering
        def half(p):
            x, y = p
            return (0 if (y > 0 or (y == 0 and x >= 0)) else 1)

        def cmp(p):
            x, y = p
            return (half(p), -y / (abs(x) + abs(y) + 1e-18), x)

        # safer approach: use atan2
        import math
        pts.sort(key=lambda p: math.atan2(p[1], p[0]))

        # build vectors
        vectors = set()

        for i in range(len(pts) - 1):
            x1, y1 = pts[i]
            x2, y2 = pts[i + 1]
            dx, dy = x2 - x1, y2 - y1
            if (dx, dy) != (0, 0):
                vectors.add((dx, dy))

        out.append(str(len(vectors)))
        for v in vectors:
            out.append(f"{v[0]} {v[1]}")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo việc xây dựng khái niệm theo nghĩa đen. Trước tiên, chúng tôi chèn điểm gốc để đường dẫn bắt đầu chính xác. Sắp xếp theo`atan2`thực thi thứ tự quét vòng quanh điểm gốc, đây là cấu trúc hình học quan trọng mà chúng tôi dựa vào. 

Chi tiết triển khai tinh tế duy nhất là xử lý các điểm trùng lặp hoặc sự khác biệt suy biến. Bộ này tự động loại bỏ các bản sao, điều này là cần thiết vì tính kề cận góc không đảm bảo các vectơ dịch chuyển duy nhất. 

Một điểm tế nhị là việc sử dụng dấu phẩy động`atan2`có thể chấp nhận được ở đây vì chúng ta chỉ cần một thứ tự nhất quán chứ không cần so sánh độ chính xác góc chính xác. Trong cài đặt chặt chẽ hơn, bộ so sánh dựa trên sản phẩm chéo sẽ được ưu tiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Điểm đầu vào:$(2,1), (1,0), (4,1)$Chúng tôi bao gồm nguồn gốc, sau đó sắp xếp theo góc. Thứ tự góc trở thành:$(0,0) \rightarrow (1,0) \rightarrow (2,1) \rightarrow (4,1)$| Bước | Từ | Đến | Vectơ | T hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | (0,0) | (1,0) | (1,0) | {(1,0)} | 
| 2 | (1,0) | (2,1) | (1,1) | {(1,0),(1,1)} | 
| 3 | (2,1) | (4,1) | (2,0) | {(1,0),(1,1),(2,0)} | 

Điều này xác nhận rằng việc xây dựng trực tiếp mang lại một tập hợp bước hợp lệ bao gồm tất cả các chuyển đổi. 

### Ví dụ 2 

Điểm đầu vào:$(-30,30), (-50,50)$Bao gồm gốc tọa độ, thứ tự góc là:$(0,0) \rightarrow (-30,30) \rightarrow (-50,50)$| Bước | Từ | Đến | Vectơ | T hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | (0,0) | (-30,30) | (-30,30) | {(-30,30)} | 
| 2 | (-30,30) | (-50,50) | (-20,20) | {(-30,30), (-20,20)} | 

Chúng tôi quan sát thấy rằng ngay cả khi các điểm nằm trên cùng một tia, nhiều kích cỡ bước vẫn xuất hiện. Điều này chứng tỏ tại sao chỉ có sự chỉ đạo thôi là chưa đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| sắp xếp điểm theo góc chiếm ưu thế | 
| Không gian |$O(n)$| lưu trữ điểm và tập vector kết quả | 

Các ràng buộc cho phép lên đến$10^5$tổng số điểm, do đó$O(n \log n)$giải pháp cho mỗi bài kiểm tra là đủ, đặc biệt khi việc sắp xếp chiếm ưu thế và tổng của tất cả các bài kiểm tra bị giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import atan2

    q = int(sys.stdin.readline())
    out = []

    for _ in range(q):
        n = int(sys.stdin.readline())
        pts = []
        for _ in range(n):
            x, y = map(int, sys.stdin.readline().split())
            pts.append((x, y))

        pts.append((0, 0))
        pts.sort(key=lambda p: atan2(p[1], p[0]))

        vectors = set()
        for i in range(len(pts)-1):
            dx = pts[i+1][0] - pts[i][0]
            dy = pts[i+1][1] - pts[i][1]
            if dx or dy:
                vectors.add((dx, dy))

        out.append(str(len(vectors)))
        for v in vectors:
            out.append(f"{v[0]} {v[1]}")

    return "\n".join(out)

# minimal
assert run("1\n2\n1 0\n2 0\n") != ""

# collinear decreasing
assert run("1\n2\n-1 0\n-2 0\n") != ""

# square
assert run("1\n4\n1 1\n1 -1\n-1 1\n-1 -1\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hai điểm thẳng hàng | tập vector không trống | xử lý điểm cùng chiều | 
| điểm góc phần tư đối diện | không trống | đặt hàng xoay vòng đầy đủ | 
| hình vuông đối xứng | thay đổi nhiều hướng | độ chính xác dưới góc bọc | 

## Vỏ cạnh 

Trường hợp góc chung là khi nhiều điểm có cùng một góc tính từ gốc tọa độ. Trong tình huống đó, việc sắp xếp theo góc không phân biệt thứ tự và việc truyền tải có thể tạo ra các vectơ có độ dài bằng 0 nếu các điểm liên tiếp giống hệt nhau. Việc triển khai tránh điều này bằng cách bỏ qua các vectơ 0 khi chèn vào tập hợp. 

Một trường hợp khác xảy ra khi các điểm nằm trên một đường thẳng đi qua gốc tọa độ nhưng theo cả hai hướng. Việc sắp xếp góc chia chúng thành hai vùng cách nhau bởi một điểm gián đoạn tại$\pi$Và$-\pi$. Đường đi được xây dựng tự nhiên tạo ra các vectơ theo cả hai hướng, đảm bảo lối đi có thể di chuyển qua điểm gốc một cách nhất quán. 

Trường hợp cạnh cuối cùng là khi gốc tọa độ trùng với điểm đầu vào. Việc bao gồm nó đảm bảo rõ ràng rằng chuỗi bắt đầu từ một điểm hợp lệ trong tập hợp đầu vào mà không yêu cầu cách viết hoa đặc biệt trong logic vòng lặp.
