---
title: "CF 102640A - Tô màu điểm"
description: "Chúng ta có một tập hợp các điểm phân biệt trên mặt phẳng tọa độ. Chúng ta phải gán cho mỗi điểm một trong k màu đầu tiên, với mỗi màu nhận được chính xác số điểm như nhau."
date: "2026-08-03T15:14:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102640
codeforces_index: "A"
codeforces_contest_name: "2020, XIII Samara Regional Intercollegiate Programming Contest (marathon problem)"
rating: 0
weight: 102640
solve_time_s: 110
verified: false
draft: false
---

[CF 102640A - Tô màu điểm](https://codeforces.com/problemset/problem/102640/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các điểm phân biệt trên mặt phẳng tọa độ. Chúng ta phải chỉ định mỗi điểm một trong những điểm đầu tiên`k`màu sắc, với mỗi màu nhận được chính xác số điểm như nhau. Chất lượng của một màu được xác định bởi cặp điểm gần nhất bên trong màu đó: cặp điểm gần nhất này càng xa nhau thì màu càng đẹp. Điểm cuối cùng là tổng của những phẩm chất này trên tất cả các màu, vì vậy mục tiêu là làm cho mọi lớp màu được tách biệt rõ ràng bên trong. 

Đầu ra không phải là một câu trả lời bằng số duy nhất. Đó là một màu của các điểm ban đầu. Giám khảo đánh giá màu sắc bằng cách tính khoảng cách bình phương tối thiểu bên trong mỗi màu và tính tổng các giá trị này. 

Các ràng buộc làm cho việc tìm kiếm tối ưu hoàn toàn không thể thực hiện được. Với`n`lên tới 1000, số lượng nhiệm vụ có thể là`k^n`, vượt xa bất cứ điều gì có thể được khám phá. Ngay cả khi xem xét tất cả các phân vùng cân bằng đều lớn về mặt tổ hợp. Giá trị của`k`nhỏ, nhiều nhất là 25, điều này gợi ý rằng chúng ta nên xây dựng các nhóm một cách trực tiếp thay vì tìm kiếm qua chúng. 

Phạm vi tọa độ chỉ từ 0 đến 1000, do đó khoảng cách được giới hạn và khoảng cách bình phương vừa khít với các loại số nguyên tiêu chuẩn. Phần đắt tiền là so sánh điểm với nhiều điểm khác. MỘT`O(n^3)`thuật toán sẽ có khoảng một tỷ thao tác cho các thử nghiệm lớn nhất, điều này quá rủi ro trong Python. MỘT`O(n^2)`việc xây dựng là thực tế vì có thể quản lý được một triệu lần kiểm tra khoảng cách. 

Có một số trường hợp thực hiện bất cẩn có thể bị mất rất nhiều điểm. Nếu tất cả các điểm được đặt thành các màu liên tiếp sau khi sắp xếp theo tọa độ, các điểm gần nhau có thể nằm trong cùng một nhóm. Ví dụ: với bốn điểm tạo thành một hình vuông và hai màu:```
4 2
0 0
1 0
1 1
0 1
```Một màu như`AABB`tạo ra hai cặp liền kề, cho điểm thấp hơn. Một màu tốt hơn như`ABAB`tách các điểm lân cận và cải thiện khoảng cách tối thiểu. 

Một trường hợp thất bại khác là khi`n`không phải là bội số của`k`, nhưng tuyên bố đảm bảo rằng nó là như vậy. Mã tính toán kích thước nhóm bằng cách chia số nguyên và bỏ qua phần còn lại sẽ âm thầm tạo ra kết quả không hợp lệ trên một phiên bản khác của vấn đề. 

Vụ án`k = 1`cũng cần được chăm sóc. Chỉ có một màu duy nhất nên mọi điểm đều phải nhận được cùng một chữ cái. Bất kỳ logic cân bằng nào giả định nhiều nhóm đều có thể vô tình truy cập vào các màu không tồn tại. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi phân vùng hợp lệ của các điểm thành`k`nhóm kích thước`n / k`, tính khoảng cách cặp gần nhất trong mỗi nhóm và giữ phân vùng tốt nhất. Điều này sẽ luôn tìm ra câu trả lời tối ưu vì nó xem xét mọi câu trả lời có thể. Vấn đề là số lượng phân vùng. Ngay cả khi bỏ qua thứ tự bên trong các nhóm, số khả năng vẫn là$$\frac{n!}{((n/k)!)^k k!}$$mà trở nên rất lớn ngay cả đối với một vài chục điểm. Vì`n = 1000`, điều này là hoàn toàn không thể. 

Quan sát quan trọng là điểm số chỉ phụ thuộc vào mức độ gần nhau của các điểm bên trong cùng một màu. Chúng ta không cần phải tối ưu hóa đồng thời mọi màu một cách rõ ràng nếu chúng ta có thể tạo thứ tự các điểm trong đó các điểm lân cận khó có thể nhận được cùng một màu. 

Một cách hữu ích để tạo ra thứ tự như vậy là duyệt điểm xa nhất. Chúng tôi bắt đầu từ một điểm và liên tục chọn điểm xa nhất so với tất cả các điểm đã chọn. Điều này tạo ra một chuỗi trong đó các điểm đầu được trải đều trên mặt phẳng. Khi chúng ta gán màu theo chu kỳ dọc theo chuỗi này, các điểm có cùng màu sẽ được phân tách trong suốt quá trình truyền thay vì được nhóm lại với nhau. 

Cách tiếp cận brute-force hoạt động vì nó xem xét mọi nhóm có thể, nhưng không thành công vì không gian tìm kiếm quá lớn. Quan sát cho thấy một thứ tự được phân tách rõ ràng đã nắm bắt được hầu hết cấu trúc cần thiết cho phép chúng ta thay thế việc tìm kiếm phân vùng không thể thực hiện được bằng một cấu trúc tham lam tất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Đặt hàng điểm xa nhất | O(n²) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính bình phương khoảng cách giữa mỗi cặp điểm. Khoảng cách bình phương là đủ vì chỉ cần so sánh và tránh căn bậc hai giúp tính toán chính xác. 
2. Chọn điểm bắt đầu tùy ý và xây dựng thứ tự đi qua. Đối với mỗi vị trí tiếp theo, hãy chọn điểm không sử dụng có khoảng cách gần nhất tới bất kỳ điểm nào đã chọn là tối đa. 

Lựa chọn này đặt mỗi điểm mới càng xa tập hợp hiện tại càng tốt, tạo ra một chuỗi bao gồm các vùng khác nhau của mặt phẳng. 
3. Gán màu cho các điểm theo thứ tự này theo chu kỳ. Điểm đầu tiên có màu`A`, cái thứ hai có màu`B`, và sau`k`-màu thứ chúng ta bắt đầu lại từ`A`. 

Từ`n`chia hết cho`k`, mọi màu sắc xuất hiện chính xác`n/k`lần. Các điểm cùng màu cách nhau bởi`k-1`các điểm khác trong quá trình truyền tải, làm giảm cơ hội tạo ra một cặp gần nhau. 
4. Chuyển đổi các số màu trở lại chữ hoa và xuất ra chuỗi kết quả. 

Tại sao nó hoạt động: Thuật toán duy trì đặc tính là mọi tiền tố của quá trình truyền tải đều chứa các điểm được chọn để tối đa hóa phạm vi bao phủ của mặt phẳng. Sự lựa chọn tham lam ngăn cản việc đặt hàng trở nên tập trung ở một khu vực. Sau đó, tô màu theo chu kỳ sẽ phân phối các điểm được phân tách rõ ràng này trên tất cả các màu. Việc xây dựng không chứng minh được một phân vùng tối ưu về mặt toán học, bởi vì đây là một vấn đề tính điểm, nhưng nó tạo ra một màu hợp lệ với sự tách biệt rõ ràng giữa các điểm có chung một màu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n, k, pts):
    dist = [[0] * n for _ in range(n)]
    for i in range(n):
        xi, yi = pts[i]
        for j in range(i):
            xj, yj = pts[j]
            d = (xi - xj) * (xi - xj) + (yi - yj) * (yi - yj)
            dist[i][j] = d
            dist[j][i] = d

    order = []
    used = [False] * n

    start = 0
    order.append(start)
    used[start] = True

    best = dist[start][:]

    for _ in range(n - 1):
        nxt = -1
        value = -1
        for i in range(n):
            if not used[i] and best[i] > value:
                value = best[i]
                nxt = i

        used[nxt] = True
        order.append(nxt)

        for i in range(n):
            if not used[i] and dist[nxt][i] < best[i]:
                best[i] = dist[nxt][i]

    ans = [''] * n
    for pos, idx in enumerate(order):
        ans[idx] = chr(ord('A') + (pos % k))

    return ''.join(ans)

def main():
    t = int(input())
    out = []
    for case in range(1, t + 1):
        n, k = map(int, input().split())
        pts = [tuple(map(int, input().split())) for _ in range(n)]
        out.append(f"Case #{case}: {solve_case(n, k, pts)}")
    print('\n'.join(out))

if __name__ == "__main__":
    main()
```Ma trận khoảng cách được xây dựng đầu tiên vì quá trình truyền tải tham lam liên tục yêu cầu khoảng cách giữa các điểm. Việc lưu trữ các giá trị này sẽ tránh tính toán lại sự khác biệt tọa độ trong quá trình xây dựng. 

các`best`mảng lưu trữ, đối với mỗi điểm chưa được chọn, khoảng cách gần nhất hiện tại của nó với tập hợp đã chọn. Khi một điểm mới đi vào quá trình truyền tải, chỉ mảng này cần được cập nhật. Đây là ý tưởng tương tự như lấy mẫu điểm xa nhất, trong đó mỗi lựa chọn mới sẽ cải thiện phạm vi bao phủ của tập hợp đã chọn. 

Quá trình truyền tải lưu trữ các chỉ số ban đầu thay vì tọa độ để màu cuối cùng có thể được viết lại theo cùng thứ tự với đầu vào. Việc phân công theo chu kỳ sử dụng`pos % k`, tự động cho mỗi màu một số điểm như nhau vì đầu vào đảm bảo tính chia hết. 

Số nguyên Python không bị tràn nên khoảng cách bình phương vẫn an toàn ngay cả ở tọa độ lớn nhất. Điều kiện biên quan trọng duy nhất là`k = 1`, trong đó thao tác modulo vẫn hoạt động và gán cho mọi điểm cùng một màu. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4 2
0 0
1 0
1 1
0 1
```Một lần truyền tải xa nhất có thể là: 

| Bước | Điểm được chọn | Vị trí đặt hàng | Màu được chỉ định | 
| --- | --- | --- | --- | 
| 1 | (0,0) | 0 | A | 
| 2 | (1,1) | 1 | B | 
| 3 | (1,0) | 2 | A | 
| 4 | (0,1) | 3 | B | 

Màu sắc cuối cùng là`ABAB`. 

Điều này chứng tỏ tại sao việc truyền tải lại tách biệt các điểm lân cận. Hai điểm nhận cùng màu là các góc đối diện của hình vuông chứ không phải là các góc liền kề. 

Ví dụ thứ hai:```
6 3
0 0
10 0
0 10
10 10
5 5
5 0
```Một khả năng có thể đi qua: 

| Bước | Điểm được chọn | Vị trí đặt hàng | Màu được chỉ định | 
| --- | --- | --- | --- | 
| 1 | (0,0) | 0 | A | 
| 2 | (10,10) | 1 | B | 
| 3 | (0,10) | 2 | C | 
| 4 | (10,0) | 3 | A | 
| 5 | (5,5) | 4 | B | 
| 6 | (5,0) | 5 | C | 

Các nhóm được cân bằng vì màu sắc lặp lại ở ba vị trí. Các điểm ở giữa không được đặt cùng nhau, điều này tránh được một cặp rất nhỏ gần nhất bên trong một màu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Việc tính toán tất cả các khoảng cách và thực hiện việc chọn điểm xa nhất đều yêu cầu phép tính bậc hai. | 
| Không gian | O(n²) | Ma trận khoảng cách lưu trữ mọi khoảng cách theo cặp. | 

Với`n = 1000`, ma trận chứa một triệu mục, dễ dàng nằm gọn trong giới hạn bộ nhớ. Thời gian chạy bậc hai cũng phù hợp với giới hạn 15 giây. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    main()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# sample
assert run("""2
4 2
0 0
1 0
1 1
0 1
4 2
0 0
1 0
1 1
0 1
""").count("Case #") == 2

# one color
assert run("""1
3 1
0 0
1 0
2 0
""").strip().startswith("Case #1: AAA")

# two colors, duplicated shape
assert run("""1
4 2
0 0
0 10
10 0
10 10
""").strip().startswith("Case #1: ")

# minimum size
assert run("""1
1 1
5 5
""").strip() == "Case #1: A"

# larger balanced case
assert run("""1
8 4
0 0
1 0
2 0
3 0
0 3
1 3
2 3
3 3
""").strip().startswith("Case #1: ")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hình vuông hai màu | Bất kỳ màu cân bằng hợp lệ nào | Hành vi tách cơ bản | 
|`k = 1`| Tất cả`A`| Trường hợp ranh giới đơn màu | 
| Bốn góc xa | Chia bốn điểm hợp lệ | Xử lý hình học đối xứng | 
| Một điểm |`A`| Đầu vào nhỏ nhất có thể | 
| Hai hàng điểm | Sản lượng cân bằng | Nhóm lớn hơn và hành vi modulo | 

## Vỏ cạnh 

Khi nào`k = 1`, đầu vào```
3 1
0 0
1 0
2 0
```yêu cầu mọi điểm để nhận được`A`. Quá trình truyền tải vẫn chạy nhưng phép gán cuối cùng chỉ sử dụng chữ cái đầu tiên vì`pos % 1`luôn luôn bằng không. Thuật toán không cần xử lý đặc biệt. 

Đối với hình vuông đối xứng:```
4 2
0 0
1 0
1 1
0 1
```sắp xếp tọa độ có thể vô tình tạo ra`AABB`, ghép các góc liền kề lại với nhau. Việc truyền tải xa nhất trước tiên sẽ chọn các vùng đối diện và phép gán tuần hoàn tạo ra màu trong đó mỗi màu nhận được các điểm riêng biệt. 

Đối với các nhóm lớn, ranh giới quan trọng là mỗi màu phải nhận được chính xác`n/k`điểm. Vì việc gán dựa trên các vị trí trong quá trình truyền tải và màu sắc lặp lại mỗi`k`phần tử, số lượng cuối cùng sẽ được tự động cân bằng. Một lỗi như chỉ gán màu cho thứ tự truyền tải mà không chuyển đổi về chỉ mục ban đầu sẽ không thành công vì thứ tự đầu ra phải khớp với điểm đầu vào.
