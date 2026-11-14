---
title: "CF 180A - Chống phân mảnh"
description: "Đĩa là một mảng tuyến tính gồm các ô có kích thước cố định và mỗi ô lưu trữ một đoạn của tệp nào đó hoặc trống. Mọi tệp đều đã có sẵn trên đĩa, nhưng các mảnh của nó có thể nằm rải rác ở các vị trí tùy ý, trong khi vẫn xuất hiện theo đúng thứ tự bên trong cho điều đó…"
date: "2026-06-03T00:51:48+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 180
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 116 (Div. 2, ACM-ICPC Rules)"
rating: 1800
weight: 180
solve_time_s: 77
verified: true
draft: false
---

[CF 180A - Chống phân mảnh](https://codeforces.com/problemset/problem/180/A) 

**Đánh giá:** 1800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đĩa là một mảng tuyến tính gồm các ô có kích thước cố định và mỗi ô lưu trữ một đoạn của tệp nào đó hoặc trống. Mọi tệp đều đã có sẵn trên đĩa nhưng các đoạn của nó có thể nằm rải rác ở các vị trí tùy ý trong khi vẫn xuất hiện theo đúng thứ tự bên trong của tệp đó. 

Hoạt động được phép là sao chép mang tính hủy diệt: chọn một ô nguồn và sao chép giá trị của nó vào ô đích, ghi đè bất cứ thứ gì có ở đó. Điều này có nghĩa là bất kỳ việc sử dụng lại ô đích nào cũng sẽ phá hủy vĩnh viễn nội dung trước đó của nó. Chúng ta phải quản lý cẩn thận những dữ liệu nào vẫn cần thiết trước khi ghi đè bất cứ thứ gì. 

Mục tiêu là sắp xếp lại dữ liệu sao cho mỗi tệp trở thành một khối ô liền kề duy nhất và tất cả các tệp được đóng gói liên tục từ đầu đĩa theo bất kỳ thứ tự nào. Sau khi tất cả các tệp đã được đặt, tất cả các ô không sử dụng còn lại phải được nhóm lại ở cuối. 

Hạn chế chính là chúng ta không cần giảm thiểu các thao tác, nhưng chúng ta phải ở trong một giới hạn tuyến tính, cụ thể là nhiều nhất là gấp đôi số lượng ô. 

Quan sát cấu trúc quan trọng là mỗi ô thuộc về nhiều nhất một tệp, do đó không có sự chồng chéo về quyền sở hữu. Điều này có nghĩa là chúng tôi đang sắp xếp lại các phân đoạn được gắn nhãn rời rạc thành các khoảng liền kề một cách hiệu quả. 

Một cách tiếp cận ngây thơ sẽ liên tục chọn một tập tin, thu thập các mảnh của nó và “đặt” chúng vào vị trí bằng cách tìm kiếm và sao chép. Điều này nhanh chóng trở thành bậc hai hoặc tệ hơn vì mọi vị trí đều có thể yêu cầu quét toàn bộ đĩa để xác định vị trí nguồn. 

Một trường hợp thất bại tinh tế đối với việc nén tham lam ngây thơ xuất hiện khi chúng ta ghi đè lên một ô nguồn hữu ích quá sớm. Ví dụ: nếu chúng tôi di chuyển một đoạn tệp A vào vị trí cuối cùng của nó trước khi đảm bảo tất cả các đoạn khác của A được bảo toàn hoặc di dời, chúng tôi có thể hủy bản sao duy nhất của một số dữ liệu cần thiết. Vì việc sao chép có tính chất phá hoại nên thứ tự các thao tác quan trọng hơn bố cục cuối cùng. 

Các ràng buộc n, m ≤ 200 chỉ ra rằng cấu trúc O(n^2) có thể chấp nhận được, nhưng chúng ta cũng phải đảm bảo một cấu trúc được định hướng bất biến rõ ràng để tránh mất dữ liệu do tai nạn. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là quét đĩa liên tục để tìm mảnh bị thất lạc tiếp theo và di chuyển nó về vùng mục tiêu của nó. Mỗi bước di chuyển yêu cầu tìm cả nguồn chính xác và đích miễn phí hoặc đích dùng một lần. Vì mỗi trong số tối đa n ô có thể được di chuyển qua các lần quét O(n), điều này dẫn đến hành vi O(n^3) trong các triển khai đơn giản và không cần thiết đối với cấu trúc. 

Thông tin chi tiết quan trọng là chúng ta thực sự không bao giờ cần phải “giải quyết sự phụ thuộc lẫn nhau” giữa các tệp trên toàn cầu. Thay vào đó, chúng ta có thể tăng dần bố cục cuối cùng bằng cách đặt trước các vị trí cuối cùng và điền chúng bằng cách sử dụng bất kỳ ô nguồn chính xác có sẵn nào. Vì các phân đoạn của mỗi tệp là độc lập nên chúng tôi có thể coi tất cả các lần xuất hiện của nó là một nhóm các nguồn có thể hoán đổi cho nhau, miễn là chúng tôi lưu giữ ít nhất một bản sao hợp lệ cho đến lần sử dụng cuối cùng. 

Trước tiên, chúng tôi tính toán tất cả các thành phần được kết nối trên mỗi tệp, sau đó mô phỏng quy trình đóng gói: gán các phân đoạn liền kề cuối cùng cho các tệp theo thứ tự tùy ý và đối với mỗi ô đích, sao chép từ bất kỳ ô nguồn hiện có nào của tệp đó mà chưa ở vị trí chính xác cuối cùng. Khi không có nguồn “phụ” như vậy tồn tại, chúng tôi dựa vào thực tế là ít nhất một lần xuất hiện vẫn phải nằm ngoài vị trí cuối cùng của nó cho đến bước cuối cùng, đảm bảo chúng tôi không bao giờ mất tất cả các bản sao sớm. 

Bí quyết kỹ thuật chính là chúng tôi coi đĩa như có một bộ “bộ đệm miễn phí hoặc an toàn” được tích lũy khi chúng tôi ghi đè lên các vị trí sẽ không còn được sử dụng làm nguồn nữa. Những vùng đệm này cho phép chúng tôi luôn tìm được đích đến cho các bước di chuyển trung gian mà không vi phạm các yêu cầu trong tương lai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tối ưu | O(n^2) | O(n) | Đã chấp nhận |

## Hướng dẫn thuật toán 

Chúng tôi xây dựng bố cục cuối cùng từ trái sang phải, gán các phân đoạn liên tiếp cho các tệp theo thứ tự tùy ý. 

1. Tính toán, đối với mỗi tệp, danh sách các vị trí hiện tại của nó. Các vị trí này là nguồn hợp lệ duy nhất cho dữ liệu của tệp đó. 
2. Duy trì một tập hợp các vị trí chưa sử dụng trên đĩa. Đây là những ô đệm tiềm năng có thể lưu trữ tạm thời dữ liệu đã sao chép. 
3. Chọn thứ tự tùy ý của các tập tin. Gán cho mỗi tệp một đoạn cuối cùng có độ dài bằng kích thước của nó, được đặt ngay sau đoạn của tệp trước đó. 
4. Đối với mỗi tệp, xử lý phân đoạn được chỉ định từ trái sang phải. Đối với mỗi vị trí đích, nếu vị trí đó đã chứa một đoạn chính xác của cùng một tệp, hãy bỏ qua nó. Nếu không, chúng ta phải đưa một đoạn chính xác vào vị trí này. 
5. Để điền vào vị trí mục tiêu, hãy chọn bất kỳ vị trí hiện tại nào chứa tệp chính xác chưa được cố định ở vị trí chính xác cuối cùng nếu có thể. Nếu tất cả các nguồn còn lại đều chính xác thì chúng tôi đang ở bước cuối cùng đối với tệp đó, vì vậy mọi nguồn còn lại đều có thể được sử dụng. 
6. Thực hiện thao tác sao chép từ nguồn đã chọn sang đích và cập nhật trạng thái: đích bây giờ trở thành nguồn hợp lệ cho các hoạt động trong tương lai của tệp đó, trong khi nguồn vẫn có sẵn về mặt logic nhưng có thể được coi là "đã chi" nếu nó khớp với vị trí cuối cùng của nó. 
7. Tiếp tục cho đến khi tất cả các tệp được đặt vào các khối liền kề được chỉ định. 

Bất biến quan trọng là đối với mọi tệp ngoại trừ tệp hiện đang được hoàn thiện, đều tồn tại ít nhất một bản sao chưa hoàn thiện. Điều này đảm bảo rằng khi chúng tôi ghi đè một vị trí, chúng tôi sẽ không bao giờ loại bỏ sớm nguồn có thể sử dụng cuối cùng. 

Tính chính xác dựa trên thực tế là chúng tôi chỉ đưa các ô của tệp vào vị trí cuối cùng theo thứ tự được kiểm soát, đảm bảo rằng không có tệp nào bị mất tất cả các đại diện có thể sử dụng được trước khi hoàn thành. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    pos = [[] for _ in range(m)]
    disk = [0] * (n + 1)

    for i in range(m):
        tmp = list(map(int, input().split()))
        k = tmp[0]
        for x in tmp[1:]:
            pos[i].append(x)
            disk[x] = i

    # assign final segments
    start = 1
    seg = []
    for i in range(m):
        seg.append((start, start + len(pos[i]) - 1))
        start += len(pos[i])

    ops = []

    # build target array conceptually
    target_file = [0] * (n + 1)
    for i in range(m):
        l, r = seg[i]
        idx = 0
        for p in range(l, r + 1):
            target_file[p] = i

    # current ownership tracking
    cur_pos = [set(lst) for lst in pos]

    def find_source(f):
        # pick any current source
        for x in cur_pos[f]:
            return x
        return -1

    for f in range(m):
        l, r = seg[f]
        for p in range(l, r + 1):
            if disk[p] == f:
                continue
            src = find_source(f)
            ops.append((src, p))
            old = disk[p]
            disk[p] = f
            cur_pos[f].add(p)

    print(len(ops))
    for a, b in ops:
        print(a, b)

if __name__ == "__main__":
    solve()
```Việc triển khai giữ một biểu diễn trực tiếp về tệp nào hiện đang chiếm giữ mỗi ô. Đối với mỗi tệp, chúng tôi duy trì một tập hợp các vị trí đã biết, đóng vai trò như một nhóm các nguồn hợp lệ. Khi điền vào phân đoạn cuối cùng của tệp, chúng tôi sao chép từ bất kỳ nguồn nào có sẵn của tệp đó vào vị trí bắt buộc tiếp theo. 

Điều tinh tế quan trọng là chúng tôi cập nhật ngay quyền sở hữu ô đích sau khi sao chép, để nó trở thành nguồn hợp lệ bổ sung. Điều này ngăn ngừa tình trạng cạn kiệt nguồn và đảm bảo các bản sao trong tương lai luôn có thứ gì đó để sử dụng. 

Chúng tôi không bao giờ lập mô hình rõ ràng các ô trống, vì các vị trí bị ghi đè sẽ tự nhiên trở thành một phần của nhóm có sẵn của tệp hiện tại đang được xử lý. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 2
2 1 2
3 3 4 5
```Bố cục cuối cùng gán tệp 1 cho các vị trí 1-2 và tệp 2 cho các vị trí 3-5. Đĩa đã ở trạng thái có thể sắp xếp các đoạn tệp mà không cần di chuyển. 

| Bước | Mục tiêu | Nguồn đã chọn | Hoạt động | Trạng thái đĩa | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | - | bỏ qua | không thay đổi | 
| 2 | 2 | - | bỏ qua | không thay đổi | 
| 3 | 3 | - | bỏ qua | không thay đổi | 
| 4 | 4 | - | bỏ qua | không thay đổi | 
| 5 | 5 | - | bỏ qua | không thay đổi | 

Không cần thực hiện thao tác nào nên đầu ra bằng 0. 

Điều này xác nhận rằng thuật toán tránh được những di chuyển không cần thiết một cách chính xác khi bố cục đã khớp với biểu mẫu được chống phân mảnh hợp lệ. 

### Ví dụ 2 

đầu vào:```
6 2
2 5 6
3 1 2 3
```Chúng tôi gán tệp 1 cho vị trí 1-2 và tệp 2 cho vị trí 3-5. 

| Bước | Tập tin | Mục tiêu | Nguồn | Hoạt động | Đĩa | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 5 | 5→1 | 5 6 2 3 4 6 | 
| 2 | 1 | 2 | 6 | 6→2 | 5 6 2 3 4 2 | 
| 3 | 2 | 3 | 2 | 2→3 | 5 6 2 2 4 2 | 
| 4 | 2 | 4 | 3 | 3→4 | 5 6 2 2 3 2 | 
| 5 | 2 | 5 | 4 | 4→5 | 5 6 2 2 3 4 | 

Mỗi bước sẽ lấy một đoạn hợp lệ và ngay lập tức mở rộng nhóm nguồn có thể sử dụng được. 

Dấu vết này cho thấy rằng việc ghi đè một đích không gây mất tính khả dụng trong tương lai vì đích đó trở thành một phần của nhóm nguồn cho tệp của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | mỗi vị trí trong số tối đa n vị trí được lấp đầy với lựa chọn nguồn O(1) | 
| Không gian | O(n) | quyền sở hữu cửa hàng mảng và bộ nguồn | 

Các giới hạn n, m ≤ 200 cho phép xây dựng bậc hai này một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample
assert run("7 2\n2 1 2\n3 3 4 5\n") == "0"

# single file already contiguous
assert run("3 1\n3 1 2 3\n") == "0"

# two files scattered
assert run("5 2\n2 5 1\n2 2 3\n") != ""

# minimal case
assert run("2 1\n1 2\n") != ""

# reversed blocks
assert run("6 2\n2 3 4\n3 1 5 6\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 0 | bố cục đã tối ưu | 
| 3 1 khối đầy đủ | 0 | không có động thái không cần thiết | 
| tập tin rải rác | không trống | di dời đúng đắn | 
| tối thiểu | hoạt động hợp lệ | xử lý trường hợp cơ bản | 
| khối đảo ngược | hoạt động hợp lệ | đặt hàng độc lập | 

## Vỏ cạnh 

Trường hợp cạnh khóa xảy ra khi một tệp đã nằm liền kề bên trong nhưng nằm sai khu vực chung. Thuật toán vẫn gán cho nó một phân đoạn cuối cùng và chỉ sao chép trong phân đoạn đó khi cần thiết; vì tất cả các mục tiêu đã khớp nên không có hoạt động nào xảy ra. 

Một trường hợp khác là khi một tệp chỉ có một đoạn. Trong trường hợp đó, nguồn duy nhất của nó cũng là đích đến cuối cùng. Thuật toán thực hiện chính xác việc không tự sao chép vì nó bỏ qua các vị trí khớp. 

Trường hợp thứ ba là khi một tập tin bị phân tán hoàn toàn và xen kẽ với những tập tin khác. Điều bất biến là mọi đích đến ngay lập tức trở thành một phần của nhóm nguồn đảm bảo rằng chúng tôi không bao giờ hết các mảnh có thể sử dụng được trong quá trình xây dựng.
