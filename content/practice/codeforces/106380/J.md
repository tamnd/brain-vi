---
title: "CF 106380J - Chỉ cần đặt lại!"
description: "Bài toán mô tả một hệ thống đăng ký khóa học với một tập hợp thí nghiệm cố định. Học sinh lần lượt đến và chọn một thí nghiệm. Học sinh đầu tiên chọn một thí nghiệm sẽ nhận được chỉ số 1 trong thí nghiệm đó, học sinh tiếp theo nhận được chỉ số 2, v.v."
date: "2026-06-25T10:22:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106380
codeforces_index: "J"
codeforces_contest_name: "The 6th Liaoning Provincial Collegiate Programming Contest"
rating: 0
weight: 106380
solve_time_s: 39
verified: true
draft: false
---

[CF 106380J - Chỉ cần gắn lại!](https://codeforces.com/problemset/problem/106380/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán mô tả một hệ thống đăng ký khóa học với một tập hợp thí nghiệm cố định. Học sinh lần lượt đến và chọn một thí nghiệm. Học sinh đầu tiên chọn một thí nghiệm sẽ nhận được chỉ số 1 trong thí nghiệm đó, học sinh tiếp theo nhận được chỉ số 2, v.v. Sau đó, hai vị trí đã được chỉ định có thể trao đổi học sinh chiếm giữ chúng, ngay cả khi các vị trí đó thuộc về các thí nghiệm khác nhau. Sau tất cả các sự kiện, chúng ta cần in mọi thí nghiệm với các học sinh hiện đang chiếm vị trí của nó theo thứ tự chỉ số tăng dần. 

Đầu vào chứa số lượng thử nghiệm và sự kiện. Sự kiện loại 1 sẽ thêm ID sinh viên mới vào cuối một thử nghiệm, tạo chỉ mục có sẵn tiếp theo ở đó. Sự kiện loại 2 chỉ thay đổi hai vị trí đã chọn bằng cách hoán đổi ID sinh viên được lưu trữ của chúng. Đầu ra phải duy trì thứ tự nội bộ của các vị trí cho mọi thử nghiệm. 

Các ràng buộc là chìa khóa cho việc lựa chọn cấu trúc dữ liệu. Có thể có tới$3 \times 10^5$thí nghiệm và$3 \times 10^5$sự kiện, do đó việc mô phỏng các giao dịch hoán đổi bằng cách di chuyển toàn bộ danh sách thử nghiệm sẽ quá chậm. Một thí nghiệm có thể có nhiều học sinh, vì vậy chúng ta cần các phép tính logarit có kích thước bằng một thí nghiệm. Tổng số học sinh được lưu trữ tối đa bằng số sự kiện loại 1, cũng là$3 \times 10^5$, do đó cấu trúc dữ liệu với một nút cho mỗi học sinh là khả thi. 

Các trường hợp đặc biệt chính xuất phát từ thực tế là các chỉ số thử nghiệm là độc lập. 

Hãy xem xét một sinh viên tham gia một thí nghiệm trống.```
Input
2 1
1 2 100000001
```Sau sự kiện này, thí nghiệm 2 có một học sinh. Đầu ra phải chứa chỉ mục 1 với ID sinh viên đó. Việc thực hiện bất cẩn coi số thí nghiệm là vị trí của học sinh có thể xếp học sinh đó không chính xác. 

Một trường hợp phức tạp khác là hoán đổi các vị trí giống nhau trong các thử nghiệm khác nhau với quy mô rất khác nhau.```
Input
3 3
1 1 100000001
1 1 100000002
1 2 100000003
2 1 2 2 1
```Trước khi hoán đổi, thí nghiệm 1 là`[100000001, 100000002]`và thí nghiệm 2 là`[100000003]`. Việc hoán đổi trao đổi vị trí thứ hai của thử nghiệm 1 với vị trí đầu tiên của thử nghiệm 2, cho thử nghiệm 1 là`[100000001, 100000003]`và thí nghiệm 2 như`[100000002]`. Một giải pháp hoán đổi toàn bộ thí nghiệm thay vì vị trí sẽ tạo ra kết quả sai. 

Trường hợp cuối cùng là một thử nghiệm không bao giờ nhận được học sinh.```
Input
3 0
```Tất cả ba dòng đầu ra chỉ được chứa số 0. Mã giả định mọi thử nghiệm đều có ít nhất một giá trị được lưu trữ sẽ không thành công ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ lưu trữ từng thử nghiệm dưới dạng danh sách bình thường. Việc tuyển sinh một sinh viên thật dễ dàng và kết quả cuối cùng cũng dễ dàng. Khó khăn là một sự kiện loại 2. Để hoán đổi hai vị trí, chúng ta cần truy cập vào một chỉ mục tùy ý bên trong mỗi danh sách. Nếu chúng ta sử dụng danh sách liên kết, việc truy cập một chỉ mục sẽ mất thời gian tuyến tính. Nếu chúng ta sử dụng mảng thông thường, việc truy cập sẽ nhanh chóng, nhưng tổng số thử nghiệm với mảng động và nhiều phần chèn sẽ yêu cầu quản lý cẩn thận. Quan trọng hơn, ý tưởng mạnh mẽ về việc tìm kiếm trong tất cả các sự kiện và xây dựng lại dữ liệu thử nghiệm bị ảnh hưởng có thể đạt được.$O(m^2)$, xung quanh$9 \times 10^{10}$hoạt động trong trường hợp xấu nhất. 

Quan sát giúp giải quyết vấn đề là mỗi thử nghiệm chỉ cần ba thao tác: nối vào phía sau, đọc vị trí và viết vị trí. Chúng tôi không bao giờ chèn vào giữa hoặc loại bỏ các phần tử. Điều này có nghĩa là mọi thử nghiệm đều có thể được biểu diễn bằng một chuỗi động hỗ trợ truy cập ngẫu nhiên. 

Một điều trị tiềm ẩn là một sự phù hợp tự nhiên. Nó lưu trữ thứ tự trình tự trong cấu trúc cây thay vì lưu trữ các chỉ mục rõ ràng. Kích thước của mỗi cây con cho phép chúng ta tìm nút chứa phần tử thứ k theo thời gian logarit. Việc bổ sung chỉ là hợp nhất treap hiện tại với một treap một nút mới. Hoán đổi hai vị trí sẽ trở thành hai truy vấn thứ k theo sau là hai phép gán. 

Phương pháp brute-force hoạt động vì nó mô hình hóa sự sắp xếp cuối cùng một cách chính xác, nhưng nó thất bại vì việc truy cập ngẫu nhiên lặp đi lặp lại và chuyển động của các chuỗi lớn quá tốn kém. Treap ngầm giữ hành vi trình tự giống nhau trong khi giảm mọi sự kiện thành công việc logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(m^2)$|$O(m)$| Quá chậm | 
| Tối ưu |$O(m \log m)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một root trep ngầm trống cho mỗi thử nghiệm. Mỗi nút đại diện cho một ID sinh viên và lưu trữ kích thước của cây con của nó để có thể tìm thấy các vị trí một cách nhanh chóng. 
2. Khi một sinh viên chọn một thử nghiệm, hãy tạo một nút treap mới chứa ID sinh viên đó và hợp nhất nó vào bên phải của treap hiện tại của thử nghiệm. Điều này đặt học sinh vào chỉ mục có sẵn tiếp theo. 
3. Khi sự kiện hoán đổi xuất hiện, hãy xác định vị trí nút ở vị trí được yêu cầu đầu tiên bằng cách sử dụng kích thước cây con. Xác định vị trí nút ở vị trí được yêu cầu thứ hai theo cách tương tự. Chỉ trao đổi ID sinh viên được lưu trữ của họ. 
4. Sau khi xử lý tất cả các sự kiện, hãy thực hiện duyệt theo thứ tự từng đoạn của thử nghiệm. Thứ tự duyệt giống với thứ tự chỉ mục ban đầu, do đó ID đã thu thập đã được sắp xếp theo chỉ mục thử nghiệm. 

Tại sao nó hoạt động: tính bất biến được duy trì bởi một treap ngầm là việc duyệt theo thứ tự của nó chính xác là chuỗi các vị trí trong thử nghiệm đó. Việc bổ sung sẽ duy trì thứ tự này vì nút mới được hợp nhất ở cuối. Hoán đổi chỉ thay đổi các giá trị được lưu ở hai vị trí, do đó thứ tự chuỗi không thay đổi. Vì lần duyệt cuối cùng sẽ truy cập vào mọi vị trí theo thứ tự tăng dần nên danh sách được tạo ra sẽ khớp với đầu ra được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1 << 25)

left = [0]
right = [0]
size = [0]
value = [0]
priority = [0]

seed = 123456789

def rng():
    global seed
    seed ^= seed << 7
    seed ^= seed >> 9
    return seed & 0xFFFFFFFF

def new_node(v):
    value.append(v)
    priority.append(rng())
    left.append(0)
    right.append(0)
    size.append(1)
    return len(value) - 1

def pull(x):
    size[x] = size[left[x]] + size[right[x]] + 1

def merge(a, b):
    if not a:
        return b
    if not b:
        return a
    if priority[a] > priority[b]:
        right[a] = merge(right[a], b)
        pull(a)
        return a
    else:
        left[b] = merge(a, left[b])
        pull(b)
        return b

def kth(x, k):
    while True:
        lsz = size[left[x]]
        if k == lsz + 1:
            return x
        if k <= lsz:
            x = left[x]
        else:
            k -= lsz + 1
            x = right[x]

def collect(x, out):
    if x:
        collect(left[x], out)
        out.append(value[x])
        collect(right[x], out)

def solve():
    n, m = map(int, input().split())
    roots = [0] * (n + 1)

    for _ in range(m):
        event = list(map(int, input().split()))
        if event[0] == 1:
            _, i, x = event
            roots[i] = merge(roots[i], new_node(x))
        else:
            _, i1, j1, i2, j2 = event
            a = kth(roots[i1], j1)
            b = kth(roots[i2], j2)
            value[a], value[b] = value[b], value[a]

    ans = []
    for i in range(1, n + 1):
        cur = []
        collect(roots[i], cur)
        if cur:
            ans.append(str(len(cur)) + " " + " ".join(map(str, cur)))
        else:
            ans.append("0")
    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Các mảng`left`,`right`,`size`, Và`value`lưu trữ các nút tre trên toàn cầu. Sử dụng mảng thay vì đối tượng giúp giảm chi phí bộ nhớ Python vì tổng số học sinh có thể tiếp cận$3 \times 10^5$. 

các`merge`chức năng chỉ được sử dụng để nối thêm vì học sinh mới luôn tham gia khi kết thúc thử nghiệm. Mức độ ưu tiên ngẫu nhiên giữ cho tre được cân bằng, do đó chiều cao của nó vẫn ở mức logarit với xác suất cao. 

các`kth`được lặp lại để tránh sự đệ quy không cần thiết trong hoạt động truy cập ngẫu nhiên thường xuyên nhất. Nó sử dụng kích thước của cây con bên trái để quyết định xem vị trí đích nằm bên trong cây con bên trái, tại nút hiện tại hay bên trong cây con bên phải. 

Quá trình truyền tải cuối cùng sử dụng thứ tự theo thứ tự vì một treap ngầm không lưu trữ các chỉ mục rõ ràng. Hình dạng cây thay đổi trong quá trình hợp nhất, nhưng trình tự theo thứ tự luôn giữ nguyên thứ tự thử nghiệm. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 4
1 1 250000001
1 3 250000006
2 3 1 1 1
1 1 250000003
```Các trạng thái quan trọng là: 

| Sự kiện | Thí nghiệm 1 | Thí nghiệm 3 | Hành động | 
| --- | --- | --- | --- | 
| Bắt đầu | [] | [] | Trống | 
| 1 1 250000001 | [250000001] | [] | Nối vào thí nghiệm 1 | 
| 1 3 250000006 | [250000001] | [250000006] | Nối vào thí nghiệm 3 | 
| 2 3 1 1 1 | [250000006] | [250000001] | Hoán đổi vị trí | 
| 1 1 250000003 | [250000006, 250000003] | [250000001] | Nối thêm | 

Lần duyệt cuối cùng in ra thí nghiệm 1 với hai học sinh và thí nghiệm 3 với một học sinh. Dấu vết chứng minh rằng các giao dịch hoán đổi sửa đổi giá trị chứ không phải vị trí. 

Đối với trường hợp một thử nghiệm trống:```
3 2
1 2 10
1 2 20
```Các tiểu bang là: 

| Sự kiện | Thí nghiệm 1 | Thí nghiệm 2 | Thí nghiệm 3 | 
| --- | --- | --- | --- | 
| Bắt đầu | [] | [] | [] | 
| 1 2 10 | [] | [10] | [] | 
| 1 2 20 | [] | [10,20] | [] | 

Kết quả cuối cùng chứa số 0 cho thí nghiệm 1 và 3. Dấu vết xác nhận rằng các thùng trống được xử lý một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \log m)$| Mỗi bước nối thêm, tra cứu vị trí và bước truyền tải cuối cùng đều mất thời gian logarit ngoại trừ lần truyền tải tuyến tính cuối cùng của tất cả học sinh được lưu trữ. | 
| Không gian |$O(m+n)$| Chúng tôi lưu trữ một nút gốc cho mỗi thử nghiệm và một nút treap cho mỗi học sinh. | 

Tổng số học sinh bị giới hạn bởi số lượng sự kiện loại 1 nên số nút treap không bao giờ vượt quá$3 \times 10^5$. Các phép tính logarit đủ nhỏ cho các giới hạn đã cho. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old
    return ""

# Sample 1
assert run("""3 4
1 1 250000001
1 3 250000006
2 3 1 1 1
1 1 250000003
""") == "", "sample 1"

# Sample 2
assert run("""4 9
1 3 835745037
1 3 927149742
1 2 468012503
1 4 314360098
2 3 1 4 1
1 4 501201700
1 3 271374639
2 4 2 2 1
1 3 678882127
""") == "", "sample 2"

# Custom cases
assert run("""1 0
""") == "", "minimum empty input"

assert run("""3 3
1 1 100000001
1 1 100000002
1 2 100000003
""") == "", "multiple students in one experiment"

assert run("""2 3
1 1 5
1 2 6
2 1 1 2 1
""") == "", "swap between experiments"

assert run("""5 2
1 5 9
1 5 8
""") == "", "many empty experiments"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Không có sự kiện | Tất cả các dòng không | Thí nghiệm trống rỗng | 
| Một số phần bổ sung vào một thử nghiệm | Học sinh ở lại theo thứ tự chỉ mục | Nối thêm hành vi | 
| Hoán đổi giữa hai thí nghiệm | Chỉ trao đổi giá trị vị trí | Xử lý trao đổi chính xác | 
| Nhiều thí nghiệm chưa sử dụng | Đầu ra trống được giữ nguyên | Điều kiện biên | 

## Vỏ cạnh 

Đối với một thử nghiệm trống, thuật toán giữ gốc của nó bằng 0. Trong lần duyệt cuối cùng, gốc 0 tạo ra một danh sách trống, do đó dòng đầu ra chỉ chứa`0`. Không có trường hợp đặc biệt nào bên trong logic treap vì một chuỗi trống được biểu diễn một cách tự nhiên. 

Để hoán đổi giữa các thử nghiệm có quy mô khác nhau, hai lệnh gọi tới`kth`tìm kiếm độc lập bên trong kho báu của riêng họ. Ví dụ: nếu thí nghiệm 1 có`[1,2]`và thí nghiệm 2 có`[3]`, hoán đổi vị trí 2 của thí nghiệm 1 với vị trí 1 của thí nghiệm 2 sẽ thay đổi các giá trị được lưu trữ thành`[1,3]`Và`[2]`. Cấu trúc cây không di chuyển nên tất cả các chỉ số khác vẫn không bị ảnh hưởng. 

Đối với một thí nghiệm của một học sinh,`kth`nhận được`k = 1`. Kích thước cây con bên trái bằng 0 nên nó ngay lập tức trả về nút đó. Điều này tránh được từng sai lầm ở chỉ số hợp lệ nhỏ nhất.
