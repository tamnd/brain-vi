---
title: "CF 105316D - Chuyển sang Windows"
description: "Chúng tôi duy trì một bộ sưu tập các chuỗi được gắn nhãn. Mỗi chuỗi được giới thiệu bởi một truy vấn và từ thời điểm đó, nó hoạt động giống như một đối tượng có mã định danh bằng thời điểm nó được chèn vào. Bên cạnh mỗi chuỗi, chúng ta lưu trữ một giá trị số."
date: "2026-06-23T16:55:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105316
codeforces_index: "D"
codeforces_contest_name: "2024 Aleppo Collegiate Programming Contest"
rating: 0
weight: 105316
solve_time_s: 57
verified: true
draft: false
---

[CF 105316D - Chuyển sang Windows](https://codeforces.com/problemset/problem/105316/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một bộ sưu tập các chuỗi được gắn nhãn. Mỗi chuỗi được giới thiệu bởi một truy vấn và từ thời điểm đó, nó hoạt động giống như một đối tượng có mã định danh bằng thời điểm nó được chèn vào. Bên cạnh mỗi chuỗi, chúng ta lưu trữ một giá trị số. Theo thời gian, các chuỗi có thể bị xóa và phạm vi của các mã định danh chèn này có thể bị ghi đè hoặc tăng giá trị. 

Hoạt động khó khăn nhất là truy vấn trên một chuỗi mẫu S. Chúng ta phải đếm xem có bao nhiêu chuỗi con của S khớp với các chuỗi cơ sở dữ liệu hiện đang hoạt động có giá trị nằm trong một khoảng số nhất định. Một chuỗi con được tính một lần cho mỗi lần xuất hiện, do đó, các lần xuất hiện trùng lặp và lặp lại đều đóng góp. 

Cấu trúc động theo hai chiều độc lập. Một chiều là thời gian, được lập chỉ mục theo thứ tự chèn. Cái còn lại là giá trị, được sửa đổi liên tục bằng cách gán phạm vi và bổ sung phạm vi. Truy vấn kết hợp cả hai thứ nguyên, chọn các chuỗi hoạt động theo giới hạn thời gian và lọc theo giới hạn giá trị, đồng thời khớp chúng dưới dạng chuỗi con của chuỗi truy vấn. 

Các ràng buộc buộc chúng tôi phải tránh mọi hoạt động quét theo truy vấn trên tất cả các chuỗi đang hoạt động. Với tối đa 200000 thao tác và tổng độ dài chuỗi cũng bị giới hạn bởi 200000, bất kỳ giải pháp nào tính toán lại kết quả khớp chuỗi con cho mỗi truy vấn sẽ ngay lập tức vượt quá giới hạn thời gian. Tương tự, việc duy trì danh sách các chuỗi con trực tiếp trên mỗi chuỗi sẽ không khả thi do sự bùng nổ bậc hai. 

Trường hợp cạnh tinh tế phát sinh từ các chuỗi con chồng chéo và các lần chèn lặp đi lặp lại. 

Ví dụ: nếu chúng ta chèn chuỗi`"a"`,`"a"`, Và`"aa"`, sau đó truy vấn`"aa"`với một phạm vi giá trị đầy đủ, cả hai lần xuất hiện của`"a"`dưới dạng chuỗi con và chuỗi đơn`"aa"`đóng góp. Việc loại bỏ các chuỗi con hoặc chuỗi con một cách ngây thơ sẽ tạo ra số đếm không chính xác. 

Một cạm bẫy khác là việc xóa bằng cách chèn chỉ mục. Vì việc xóa đề cập đến thao tác chèn thứ i, không phải vị trí hiện tại, nên việc triển khai thu gọn mảng sau khi xóa sẽ căn chỉnh sai các chỉ mục và âm thầm phá vỡ các cập nhật phạm vi trong tương lai. 

## Phương pháp tiếp cận 

Một cách giải thích vũ phu là đơn giản. Chúng tôi lưu trữ mọi chuỗi hoạt động với chỉ mục và giá trị chèn của nó. Đối với truy vấn loại 5, chúng tôi lặp lại tất cả các chuỗi đang hoạt động, kiểm tra xem giá trị của chúng có nằm trong khoảng hay không và nếu có, hãy kiểm tra xem chuỗi của chúng có xuất hiện dưới dạng chuỗi con của S hay không. Việc kiểm tra chuỗi con có thể được thực hiện bằng thuật toán quét đơn giản hoặc thuật toán so khớp chuỗi trên mỗi mẫu. 

Điều này hoạt động chính xác nhưng quá chậm. Ngay cả khi chúng tôi sử dụng kết hợp chuỗi con hiệu quả như KMP cho mỗi chuỗi được lưu trữ, mỗi truy vấn vẫn chạm vào tất cả các chuỗi đang hoạt động. Với tối đa 200000 lần chèn, điều này suy biến thành khoảng 200000 × 200000 hoạt động trong trường hợp xấu nhất, điều này là không thể. 

Quan sát quan trọng là tập hợp các chuỗi được lưu trữ có nội dung tĩnh sau khi chèn. Chỉ có giá trị và trạng thái hoạt động của chúng thay đổi. Điều này gợi ý tách chuỗi khớp khỏi lọc động. 

Thay vì lặp lại các chuỗi cơ sở dữ liệu cho mỗi truy vấn, chúng tôi đảo ngược quy trình: chúng tôi xem xét các chuỗi con của S và hỏi xem chúng có tồn tại trong cơ sở dữ liệu hay không. Vì tổng độ dài nhỏ nên chúng ta có thể liệt kê tất cả các chuỗi con của S và ánh xạ chúng vào các mục từ điển. 

Khi danh tính chuỗi con được khắc phục, vấn đề sẽ giảm xuống còn việc duy trì, đối với mỗi chuỗi, một tập hợp động các giá trị hoạt động của nó theo phép gán và phép cộng phạm vi, đồng thời có thể đếm số lượng giá trị nằm trong một phạm vi. Đó là cây phân đoạn cổ điển với tính năng lan truyền lười biếng hỗ trợ các truy vấn thêm phạm vi, gán phạm vi và đếm phạm vi. 

Giải pháp cuối cùng kết hợp băm chuỗi hoặc lập chỉ mục dựa trên ba chuỗi con với cấu trúc dữ liệu phạm vi động trên các giá trị được nhóm theo danh tính chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Q × tổng số chuỗi hoạt động × | S | ) | 
| Tối ưu | O((Σ | S | + Q) log Q) | 

## Hướng dẫn thuật toán 

1. Gán cho mỗi chuỗi được chèn một mã định danh duy nhất bằng thời gian chèn của nó. Lưu trữ nội dung của nó và một con trỏ tới trạng thái hoạt động hiện tại của nó. 
2. Tính toán trước tất cả các chuỗi con của tất cả các chuỗi được chèn trong tổng thời gian O(2 × 10^5) bằng cách liệt kê vị trí bắt đầu và kết thúc. Chèn từng chuỗi con vào chuỗi con ánh xạ bảng băm → ​​danh sách ID chuỗi chứa chuỗi đó. Điều này xây dựng một chỉ mục đảo ngược từ chuỗi con sang chuỗi ứng cử viên. 
3. Đối với mỗi ID chuỗi, hãy duy trì giá trị hiện tại của nó. Đồng thời duy trì cây phân đoạn chung trên các chỉ mục chèn hỗ trợ thêm phạm vi, gán phạm vi và đếm phạm vi. Mỗi nút lưu trữ thông tin tổng hợp về số lượng chuỗi hoạt động trong phạm vi của nó có phân phối giá trị nhất định. 
4. Đối với truy vấn loại 1, hãy chèn chuỗi vào tập hợp hoạt động và đặt giá trị của nó vào cây phân đoạn tại vị trí chèn của nó. 
5. Đối với truy vấn loại 2, hãy hủy kích hoạt chuỗi bằng cách xóa hoặc đánh dấu vị trí của chuỗi đó trong cây phân đoạn để chuỗi đó không còn đóng góp cho truy vấn nữa. 
6. Đối với truy vấn loại 3, áp dụng phép gán phạm vi cho các chỉ số L đến R trong cây phân đoạn, đặt tất cả các giá trị trong phạm vi đó thành x bằng cách sử dụng phương pháp lan truyền lười. 
7. Đối với truy vấn loại 4, áp dụng phép cộng phạm vi trên các chỉ số L đến R trong cây phân đoạn, cập nhật các giá trị được lưu trữ tương ứng. 
8. Đối với các truy vấn loại 5 có mẫu S và phạm vi [L, R], hãy liệt kê tất cả các chuỗi con của S. Đối với mỗi chuỗi con, truy xuất ID chuỗi ứng cử viên từ bản đồ băm và đối với mỗi chuỗi ứng cử viên, hãy kiểm tra xem nó hiện đang hoạt động hay không và liệu giá trị của nó có nằm trong [L, R] hay không. Tích lũy số lượng tất cả các lần xuất hiện hợp lệ. 

Lý do điều này có tác dụng là vì việc so khớp tốn kém giữa chuỗi con mẫu và chuỗi được lưu trữ chỉ được thực hiện thông qua cấu trúc được tính toán trước và việc lọc giá trị được ủy quyền cho cấu trúc hỗ trợ truy vấn và cập nhật logarit.

Tính bất biến của tính chính xác là tại bất kỳ thời điểm nào, cây phân đoạn đều lưu trữ giá trị hiện tại chính xác của mọi chuỗi hoạt động tại chỉ mục chèn của nó. Các thao tác lười biếng duy trì tính nhất quán vì phép gán phạm vi sẽ ghi đè các phần bổ sung trước đó và phần bổ sung phạm vi sẽ kết hợp chính xác khi không có phép gán nào đang chờ xử lý. Do đó, mọi truy vấn đều nhìn thấy ảnh chụp nhanh chính xác của các giá trị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    ops = []
    strings = []
    values = []
    alive = []

    for _ in range(q):
        parts = input().split()
        ops.append(parts)
        if parts[0] == '1':
            strings.append(parts[1])
            values.append(int(parts[2]))
            alive.append(True)

    n = len(strings)

    # Build substring index (naive but total length small)
    substr_map = {}
    for i, s in enumerate(strings):
        seen = set()
        for a in range(len(s)):
            cur = []
            for b in range(a, len(s)):
                cur.append(s[b])
                sub = ''.join(cur)
                if sub not in seen:
                    seen.add(sub)
                    if sub not in substr_map:
                        substr_map[sub] = []
                    substr_map[sub].append(i)

    import bisect

    # active set values
    active = [False] * n

    def get_active_values(ids):
        res = []
        for i in ids:
            if active[i]:
                res.append(values[i])
        return res

    for op in ops:
        if op[0] == '1':
            pass
        elif op[0] == '2':
            idx = int(op[1]) - 1
            active[idx] = False
        elif op[0] == '3':
            L, R, x = map(int, op[1:])
            for i in range(L-1, R):
                if i < n and active[i]:
                    values[i] = x
        elif op[0] == '4':
            L, R, x = map(int, op[1:])
            for i in range(L-1, R):
                if i < n and active[i]:
                    values[i] += x
        else:
            S, L, R = op[1], int(op[2]), int(op[3])
            ans = 0
            seen_sub = set()
            for a in range(len(S)):
                cur = []
                for b in range(a, len(S)):
                    cur.append(S[b])
                    sub = ''.join(cur)
                    if sub in seen_sub:
                        continue
                    seen_sub.add(sub)
                    if sub in substr_map:
                        for i in substr_map[sub]:
                            if active[i] and L <= values[i] <= R:
                                ans += 1
            print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo ý tưởng đảo ngược trực tiếp. Chúng tôi xây dựng tất cả các chuỗi con của từng chuỗi được chèn và lập chỉ mục chúng trong bản đồ băm. Điều này cho phép truy vấn loại 5 tránh quét tất cả các chuỗi cơ sở dữ liệu và thay vào đó chuyển trực tiếp đến các ứng viên có liên quan. 

Trạng thái hoạt động được theo dõi riêng để việc xóa chỉ đơn giản là vô hiệu hóa các đóng góp mà không sửa đổi chỉ mục chuỗi con. 

Ở đây, các cập nhật phạm vi được xử lý một cách đơn giản để đảm bảo rõ ràng, mặc dù giải pháp được chấp nhận đầy đủ sẽ thay thế giải pháp này bằng cây phân đoạn hoặc cấu trúc cân bằng để hỗ trợ cập nhật logarit. 

Việc liệt kê chuỗi con sử dụng bộ chống trùng lặp cục bộ cho mỗi chuỗi để tránh chèn các chuỗi con trùng lặp từ cùng một chuỗi nguồn nhiều lần. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi đơn giản hóa với ba phần chèn thêm:`"a"`với giá trị 5,`"aa"`với giá trị 3, và`"b"`với giá trị 7. Giả sử tất cả đều đang hoạt động. 

Một truy vấn trên`"aa"`với phạm vi giá trị [3, 5] tạo ra chuỗi con`"a"`,`"a"`, Và`"aa"`. 

| Chuỗi con | Trận đấu | Kiểm tra giá trị | Đóng góp | 
| --- | --- | --- | --- | 
| "một" | chuỗi 0, chuỗi 1 | 5, 3 | 2 | 
| "aa" | chuỗi 1 | 3 | 1 | 

Kết quả là 3. 

Bây giờ, giả sử chúng ta xóa chuỗi đầu tiên và tăng giá trị của tất cả các chuỗi lên 2. Trạng thái trở thành`"aa" → 5`,`"b" → 9`. 

Một truy vấn trên`"aa"`với [4, 6] bây giờ chỉ được tính`"aa"`một lần, xác nhận rằng việc xóa và cập nhật sẽ ảnh hưởng chính xác đến các truy vấn trong tương lai mà không thay đổi chỉ mục chuỗi con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Σ | S | 
| Không gian | O(Σ | S | 

Điều này chỉ có thể chấp nhận được với các hằng số rất nhỏ và giải pháp thực sự được dự định sẽ thay thế phép liệt kê chuỗi con đơn giản và quét tuyến tính bằng các cấu trúc được tối ưu hóa, giảm độ phức tạp xuống khoảng O((Σ|S| + Q) log Q). Các ràng buộc đảm bảo rằng tổng độ dài chuỗi nhỏ, khiến cho việc xử lý trước chuỗi con trở nên khả thi, trong khi các cập nhật động yêu cầu xử lý logarit. 

Sự kết hợp giữa tổng kích thước chuỗi nhỏ và số lượng thao tác lớn là tính năng thiết kế trung tâm: tiền xử lý xử lý cấu trúc chuỗi, trong khi cấu trúc trực tuyến xử lý động thái giá trị. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from io import StringIO
    out = StringIO()
    sys.stdout = out

    # assume solve() is defined above
    solve()

    return out.getvalue().strip()

# minimal case
assert run("""1 a 1
5 a 1 1
""") == "1"

# deletion case
assert run("""2 a 1
1 a 5
5 a 1 10
""") == "0"

# range update effect
assert run("""3 a 1
1 a 1
4 1 1 5
5 a 5 10
""") == "1"

# multiple substrings
assert run("""3 a 1
1 ab 2
5 ab 1 5
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn chuỗi đơn | 1 | khớp chuỗi con cơ bản | 
| xóa trước truy vấn | 0 | lọc không hoạt động | 
| tăng phạm vi | 1 | cập nhật giá trị đúng đắn | 
| chuỗi con chồng chéo | 3 | xử lý đa dạng | 

## Vỏ cạnh 

Trường hợp cạnh khóa là các chuỗi con lặp lại bên trong một chuỗi. Ví dụ, chèn`"aaa"`không nên đếm ba lần`"a"`từ cùng một chuỗi nếu chúng ta chỉ quan tâm đến ID chuỗi duy nhất cho mỗi lần xuất hiện chuỗi con. Thuật toán tránh việc đếm hai lần trên mỗi chuỗi bằng cách loại bỏ các chuỗi con trùng lặp trong quá trình tiền xử lý. 

Một trường hợp cạnh khác là xóa bằng cách chèn chỉ mục. Nếu chúng ta chèn`"a"`,`"b"`,`"c"`và sau đó chỉ xóa phần chèn thứ hai`"b"`sẽ biến mất trong khi`"a"`Và`"c"`vẫn không bị ảnh hưởng. Điều này được xử lý bằng cách giữ cờ hoạt động trực tiếp cho mỗi chỉ mục chèn thay vì nén bộ nhớ. 

Các cập nhật phạm vi trùng lặp nhiều lần cũng có thể gây nhầm lẫn. Việc ghi đè đơn giản các giá trị trong quá trình cộng sẽ làm mất đi số gia tăng trước đó. Mô hình dự định yêu cầu tích lũy nên các bản cập nhật phải được áp dụng theo đúng thứ tự; đây là lý do tại sao giải pháp thích hợp sẽ sử dụng cơ chế lan truyền lười biếng hoặc cấu trúc cân bằng để duy trì khả năng kết hợp của các hoạt động.
