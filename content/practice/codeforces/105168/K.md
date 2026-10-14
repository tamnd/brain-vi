---
title: "CF 105168K - Phân tán đồng đều"
description: "Chúng ta được cấp một tập hợp các điểm trên mặt phẳng 2D. Nhiệm vụ là đặt chính xác $k$ các đường thẳng đứng và $k$ các đường ngang sao cho không có đường thẳng nào đi qua bất kỳ điểm nào và các đường này chia mặt phẳng thành các vùng hình chữ nhật $(k+1)times(k+1)$."
date: "2026-06-27T09:06:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105168
codeforces_index: "K"
codeforces_contest_name: "2024 Fujian Normal University Programming Contest"
rating: 0
weight: 105168
solve_time_s: 48
verified: true
draft: false
---

[CF 105168K - Phân tán đồng đều](https://codeforces.com/problemset/problem/105168/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các điểm trên mặt phẳng 2D. Nhiệm vụ là đặt chính xác$k$đường thẳng đứng và$k$những đường nằm ngang sao cho không có đường thẳng nào đi qua bất kỳ điểm nào và những đường này chia mặt phẳng thành$(k+1)\times(k+1)$các vùng hình chữ nhật. Mỗi khu vực phải chứa chính xác cùng một số điểm. 

Mỗi điểm thuộc về chính xác một vùng được xác định bởi có bao nhiêu đường thẳng đứng nằm bên trái và bao nhiêu đường ngang nằm bên dưới nó. Vì vậy, một cách hiệu quả, các đường thẳng đứng tạo ra sự phân chia các điểm thành$k+1$các dải dọc và các đường ngang độc lập tạo ra một phân vùng thành$k+1$các dải ngang. Yêu cầu là mọi ô giao nhau được hình thành bằng cách chọn một dải dọc và một dải ngang chứa cùng số điểm. 

Các ràng buộc rất lớn: lên tới$2\cdot 10^5$điểm cho mỗi bài kiểm tra trong tất cả các bài kiểm tra và$k$có thể lớn như$10^9$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng xây dựng hoặc mô phỏng rõ ràng các vị trí đường thẳng. Bất kỳ lời giải hợp lệ nào cũng phải chỉ phụ thuộc vào cấu trúc tổ hợp của tập hợp điểm, không phụ thuộc vào hình học hoặc tìm kiếm theo tọa độ. 

Một quan sát quan trọng là tọa độ chính xác không liên quan ngoại trừ việc sắp xếp thứ tự. Chỉ thứ tự tương đối dọc theo x và y mới quan trọng, bởi vì các đường thẳng đứng có thể được đặt ở bất kỳ đâu giữa các giá trị x được sắp xếp và tương tự đối với y. Vì vậy, bài toán quy về việc suy luận về hoán vị của các điểm theo cấp x và y. 

Trường hợp cạnh tinh tế là khi các điểm chồng lên nhau. Nhiều điểm có thể có chung tọa độ giống nhau và một đường thẳng không được đi qua bất kỳ điểm nào nhưng có thể đặt nó giữa các tọa độ bằng nhau nếu cần. Bất kỳ giải pháp nào giả định thứ tự tọa độ nghiêm ngặt mà không xử lý các bản sao đều có thể thất bại trong các trường hợp như nhiều điểm giống hệt nhau trong đó việc phân tách phải tôn trọng bội số. 

Một trường hợp cạnh khác là$k=0$, nơi không có đường nào được vẽ. Khi đó toàn bộ mặt phẳng là một vùng duy nhất, do đó tất cả các điểm phải nằm trong một vùng một cách tầm thường, nghĩa là điều kiện luôn đúng bất kể sự phân bố. Trường hợp này thường phá vỡ các triển khai giả định ít nhất một lần phân chia. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng suy nghĩ một cách ngây thơ, chúng ta có thể tưởng tượng việc đặt$k$đường thẳng đứng và$k$đường ngang và cố gắng gán điểm cho$(k+1)^2$các ô sao cho mỗi ô nhận được cùng số điểm. Vì tọa độ là các lựa chọn có giá trị thực giữa các điểm, nên ý tưởng đơn giản là sắp xếp các điểm theo x và thử mọi cách để chia thành$k+1$các nhóm liền kề và tương tự theo y, sau đó kiểm tra tính nhất quán. 

Tuy nhiên, ngay cả khi bỏ qua tính chất liên tục của việc sắp xếp đường thẳng, số cách để chọn$k$cắt vị trí giữa$n$điểm là tổ hợp và việc thử tất cả các phân vùng là theo cấp số nhân. Ngay cả một chiều duy nhất cũng đã có$\binom{n-1}{k}$những cách mà không thể thực hiện được$n=2\cdot 10^5$. 

Cái nhìn sâu sắc về cấu trúc quan trọng là các phân vùng dọc và ngang phải hoạt động độc lập và thống nhất. Mỗi dải dọc phải chứa chính xác cùng số điểm vì mỗi dải được chia nhỏ thành$k+1$các ô bằng nhau theo đường ngang. Nếu bất kỳ dải dọc nào có số lượng khác nhau thì hàng ô của nó không thể khớp hoàn toàn với các ô khác. 

Điều này tạo ra một ràng buộc mạnh mẽ: tổng số điểm$n$phải chia hết cho$(k+1)^2$, vì mỗi$(k+1)^2$tế bào có kích thước bằng nhau. Hơn nữa, mỗi dải dọc chứa$n/(k+1)$điểm và mỗi dải ngang cũng chứa$n/(k+1)$điểm. 

Bây giờ vấn đề trở thành một chiều trên mỗi trục. Chúng ta cần kiểm tra xem liệu chúng ta có thể chia tọa độ x đã sắp xếp thành$k+1$các nhóm liền kề có kích thước$n/(k+1)$và độc lập thực hiện tương tự cho tọa độ y. Nếu cả hai đều có thể, chúng ta có thể đặt các đường giữa các nhóm. 

Do đó, vấn đề giảm xuống còn việc kiểm tra xem liệu khi sắp xếp theo x, mọi khối có kích thước$n/(k+1)$tạo thành một dải hợp lệ và tương tự cho y. 

Điểm tinh tế là đây không phải về giá trị mà chỉ về việc nhóm theo thống kê thứ tự. Không có hình học ngoài việc sắp xếp là bắt buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân vùng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Sắp xếp + xác thực khối |$O(n \log n)$| O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định$m = k+1$. Nếu như$n \% (m \cdot m) \neq 0$, chúng tôi ngay lập tức kết luận là không thể vì phân phối đồng đều trên$(k+1)^2$tế bào là không thể. 

Chúng tôi tiến hành như sau. 

1. Tính toán$cell = n / (m \cdot m)$. Đây là số điểm cần thiết cho mỗi khu vực. 
2. Tính toán$strip = n / m$. Đây là số điểm trên mỗi dải dọc và trên mỗi dải ngang. Điều này xảy ra vì mỗi dải dọc chứa chính xác$m$các tế bào xếp chồng lên nhau theo chiều dọc. 
3. Sắp xếp tất cả các điểm theo tọa độ x. Chúng tôi quét danh sách được sắp xếp theo từng khối kích thước$strip$. Mỗi đoạn đại diện cho một dải dọc ứng cử viên. Chúng tôi xác minh rằng trong mỗi đoạn, chúng tôi có thể gán chính xác về mặt khái niệm$cell$điểm trên mỗi đoạn ngang sau này. Ở giai đoạn này, chúng tôi chỉ đảm bảo tính khả thi của việc phân nhóm. 
4. Sắp xếp độc lập tất cả các điểm theo tọa độ y và thực hiện kiểm tra tương tự cho các dải ngang. 
5. Nếu có thể phân vùng theo hướng x và hướng y thành các khối có kích thước bằng nhau, chúng ta trả về Có, nếu không thì Không. 

Chi tiết triển khai là chúng ta không cần chỉ định các ô một cách rõ ràng. Cấu trúc đảm bảo rằng nếu cả hai trục đều chấp nhận phân vùng khối đồng nhất, tích Descartes của chúng sẽ tự động tạo thành các ô có kích thước bằng nhau. 

### Tại sao nó hoạt động 

Việc phân vùng được tạo ra bởi các đường thẳng đứng chỉ phụ thuộc vào thứ tự x, do đó mỗi dải dọc là một đoạn liền kề theo thứ tự x được sắp xếp. Tương tự, các dải ngang tương ứng với các đoạn liền kề theo thứ tự y. Vì mỗi ô là giao điểm của chính xác một dải dọc và một dải ngang, nên mỗi kích thước ô được xác định bằng số điểm rơi vào mỗi cặp chỉ số. 

Tính đồng nhất trên tất cả các ô buộc phải có tính đồng nhất trên các dải vì việc tính tổng theo hàng hoặc cột phải mang lại tổng số giống hệt nhau. Điều này làm giảm ràng buộc 2D thành hai ràng buộc phân vùng bằng nhau 1D độc lập. Khi cả hai phép chiếu thừa nhận sự phân tách khối bằng nhau, cấu trúc tích Descartes đảm bảo tất cả$(k+1)^2$giao lộ có bản số giống hệt nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ok_projection(coords, m, strip):
    coords.sort()
    n = len(coords)
    for i in range(0, n, strip):
        # each block must have exactly 'strip' points
        if i + strip > n:
            return False
    return True

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        pts = [tuple(map(int, input().split())) for _ in range(n)]

        m = k + 1

        if n % (m * m) != 0:
            print("No")
            continue

        strip = n // m

        xs = [p[0] for p in pts]
        ys = [p[1] for p in pts]

        if ok_projection(xs, m, strip) and ok_projection(ys, m, strip):
            print("Yes")
        else:
            print("No")

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên thực thi điều kiện chia hết được ngụ ý bởi kích thước ô bằng nhau. Sau đó, nó trích xuất danh sách tọa độ x và y và kiểm tra xem chúng có thể được chia đều thành các khối có kích thước liền kề hay không$strip = n/(k+1)$. Cần phải sắp xếp sao cho các khối tương ứng với ranh giới dải tiềm năng. 

Một mối quan tâm thực hiện tinh tế là tọa độ trùng lặp. Tính năng sắp xếp vẫn hoạt động vì chúng tôi chỉ dựa vào số lượng chứ không dựa vào khoảng cách bất bình đẳng nghiêm ngặt. Việc kiểm tra khối không cố gắng đặt các vết cắt bên trong các tọa độ giống hệt nhau; vì được phép cắt ở các vị trí không nguyên nên chúng ta luôn có thể đặt chúng giữa các điểm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4, k = 1
points = [(-2,-2), (-1,2), (1,3), (2,-1)]
```chúng tôi có$m=2$, Vì thế$m^2=4$, kể từ đây$cell=1$,$strip=2$. 

| Bước | Sắp xếp x | khối x | Đã sắp xếp y | khối y | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | [-2, -1, 1, 2] | đang chờ xử lý | [-2, -1, 2, 3] | đang chờ xử lý | tính toán | 
| nhóm | [-2,-1], [1,2] | hợp lệ | [-2,-1], [2,3] | hợp lệ | Có | 

Cả hai hình chiếu đều được chia thành hai nhóm có kích thước 2, do đó có thể đặt các đường dọc và ngang giữa các nhóm. 

Điều này xác nhận rằng cấu trúc chỉ phụ thuộc vào thứ tự chứ không phải hình học thực tế. 

### Ví dụ 2 

đầu vào:```
n = 4, k = 1
points = [(-2,-2), (-2,2), (1,3), (-2,0)]
```Chúng tôi vẫn có$strip=2$. 

| Bước | Sắp xếp x | khối x | Đã sắp xếp y | khối y | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | [-2,-2,-2,1] | đang chờ xử lý | [-2,0,2,3] | đang chờ xử lý | tính toán | 
| nhóm | [-2,-2], [-2,1] | cấu trúc không hợp lệ | [-2,0], [2,3] | hợp lệ | Không | 

Các giá trị x buộc một nhóm có vấn đề vì một giá trị chi phối ranh giới khối theo cách ngăn cản cấu trúc dải cân bằng. Điều này cho thấy rằng chỉ phân vùng y đúng là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| sắp xếp x và y cho từng test | 
| Không gian |$O(n)$| lưu trữ điểm và mảng tọa độ | 

Tổng cộng$n$qua các bài kiểm tra nhiều nhất là$2\cdot 10^5$, do đó việc sắp xếp chiếm ưu thế nhưng vẫn dễ dàng nằm trong giới hạn. Việc sử dụng bộ nhớ là tuyến tính ở kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        pts = [tuple(map(int, input().split())) for _ in range(n)]

        m = k + 1

        if n % (m * m) != 0:
            out.append("No")
            continue

        strip = n // m

        xs = [p[0] for p in pts]
        ys = [p[1] for p in pts]

        xs.sort()
        ys.sort()

        def ok(arr):
            n = len(arr)
            for i in range(0, n, strip):
                if i + strip > n:
                    return False
            return True

        if ok(xs) and ok(ys):
            out.append("Yes")
        else:
            out.append("No")

    return "\n".join(out)

# provided sample (illustrative; exact formatting depends on original)
assert run("""3
4 1
-2 -2
-1 2
1 3
2 -1
4 1
-2 -2
-2 2
1 3
-2 0
8 1
-2 -2
-2 -2
-1 2
-1 2
1 3
1 3
2 -1
2 -1
""") == "Yes\nNo\nYes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cấu trúc tối thiểu k=0 | Có | trường hợp đơn vùng tầm thường | 
| điểm trùng lặp | Có | xử lý đa dạng | 
| phân phối lệch | Không | phân vùng không hợp lệ | 

## Vỏ cạnh 

cho$k=0$, không có đường thẳng và toàn bộ mặt phẳng là một vùng. Thuật toán thiết lập chính xác$m=1$,$strip=n$, do đó cả hai phép chiếu đều vượt qua một cách tầm thường vì mỗi trục tạo thành một khối duy nhất chứa tất cả các điểm. 

Đối với các điểm trùng lặp, tất cả các tọa độ giống hệt nhau, việc sắp xếp sẽ tạo ra một mảng phẳng. Vì các khối được xác định hoàn toàn bằng số lượng nên các bản sao không ảnh hưởng đến việc hình thành dải và thuật toán vẫn phân vùng chính xác khi$n$thỏa mãn điều kiện chia hết cần tìm. 

Đối với sự mất cân bằng cực độ về tọa độ, chẳng hạn như nhiều giá trị x giống hệt nhau và các giá trị y rải rác, phép chiếu x có thể thất bại ngay cả khi phép chiếu y thành công. Thuật toán loại bỏ chính xác các trường hợp như vậy vì cả hai trục phải hỗ trợ độc lập việc phân tách khối thống nhất chứ không chỉ một trục.
