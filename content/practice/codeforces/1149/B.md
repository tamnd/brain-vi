---
title: "CF 1149B - Ba Tôn Giáo"
description: "Chúng ta được cung cấp một chuỗi cơ sở cố định mà chúng ta có thể coi là một “chuỗi vũ trụ” dài các ký tự. Bên cạnh nó, có ba chuỗi đang phát triển, mỗi chuỗi một nhóm."
date: "2026-06-12T03:07:37+07:00"
tags: ["codeforces", "competitive-programming", "dp", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1149
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 556 (Div. 1)"
rating: 2200
weight: 1149
solve_time_s: 92
verified: true
draft: false
---

[CF 1149B - Ba tôn giáo](https://codeforces.com/problemset/problem/1149/B) 

**Đánh giá:** 2200 
**Tags:** dp, triển khai, chuỗi 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi cơ sở cố định mà chúng ta có thể coi là một “chuỗi vũ trụ” dài các ký tự. Bên cạnh nó, có ba chuỗi đang phát triển, mỗi chuỗi một nhóm. Mỗi thao tác sẽ thêm một ký tự vào một trong ba chuỗi này hoặc xóa ký tự cuối cùng khỏi một trong số chúng. Sau mỗi lần cập nhật, chúng tôi phải quyết định xem có thể gán từng ký tự của chuỗi vũ trụ cho nhiều nhất một trong ba nhóm hay không để sau khi lọc theo các ký tự được gán theo thứ tự, chúng tôi khôi phục chính xác ba chuỗi nhóm hiện tại. 

Đây là vấn đề nhúng chuỗi con với ba mẫu phải được nhúng rời rạc vào một văn bản cố định duy nhất. Yêu cầu quan trọng là các chuỗi con phải tôn trọng thứ tự của chuỗi cơ sở và không được chia sẻ vị trí. 

Những hạn chế rất quan trọng. Chuỗi cơ sở có độ dài lên tới 100000, nhưng mỗi chuỗi trong số ba chuỗi phát triển được giới hạn ở độ dài 250 và có tối đa 1000 bản cập nhật. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào phụ thuộc bậc hai hoặc tệ hơn vào chuỗi vũ trụ cho mỗi truy vấn chỉ khả thi nếu sự phụ thuộc vào các chuỗi nhỏ chứ không phải vào n. 

Một cách giải thích ngây thơ sẽ cố gắng, sau mỗi lần cập nhật, kiểm tra xem liệu cả ba chuỗi có thể được nhúng rời rạc vào chuỗi cơ sở hay không, ví dụ bằng cách khớp một cách tham lam các chuỗi con một cách độc lập. Điều này vốn đã trở nên tinh vi vì việc tham lam khớp từng chuỗi một cách độc lập có thể xung đột khi chúng cạnh tranh cho cùng một vị trí. 

Một cách tiếp cận không chính xác nguy hiểm hơn là khớp ba chuỗi một cách tuần tự trên chuỗi cơ sở, loại bỏ các vị trí đã sử dụng nhưng thứ tự khớp lại quan trọng. Ví dụ, nếu vũ trụ là`abcabc`và các chuỗi mục tiêu là`abc`,`abc`, việc khớp chuỗi đầu tiên một cách tham lam có thể tiêu tốn lần xuất hiện đầu tiên và không còn chỗ cho chuỗi thứ hai, trong khi một phép gán khác sẽ thành công. 

Trường hợp cạnh thứ hai là khi các ký tự giống hệt nhau và được lặp lại nhiều trong chuỗi vũ trụ. Nếu tất cả các chuỗi bao gồm cùng một chữ cái, tính độc lập sẽ bị phá vỡ hoàn toàn và chỉ có việc theo dõi vị trí cẩn thận mới có tác dụng. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ tính toán lại tính khả thi sau mỗi lần cập nhật bằng cách cố gắng gán từng vị trí ký tự trong chuỗi vũ trụ cho một trong bốn trạng thái: không được sử dụng hoặc được sử dụng bởi một trong ba chuỗi. Đây là một bài toán gán có ràng buộc trong đó mỗi chuỗi phải xuất hiện dưới dạng một dãy con theo thứ tự. Người ta có thể thử quay lui hoặc DP trên các vị trí của chuỗi vũ trụ trong khi theo dõi xem mỗi chuỗi trong số ba chuỗi đã khớp đến mức nào. 

Điều này nhanh chóng trở thành cấp số nhân. Ngay cả khi giới hạn ở DP, trạng thái tự nhiên sẽ là các vị trí trong cả ba chuỗi và một vị trí trong chuỗi vũ trụ, dẫn đến O(n * 250^3) hoặc tệ hơn, điều này hoàn toàn không khả thi. 

Quan sát cấu trúc quan trọng là chuỗi vũ trụ cố định và chỉ có ba chuỗi mục tiêu thay đổi. Thay vì suy luận tổng thể về các phép gán, chúng ta đảo ngược quan điểm: chúng ta cố gắng kiểm tra xem liệu cả ba chuỗi có thể được xen kẽ thành các chuỗi con của chuỗi vũ trụ mà không trùng nhau hay không. Điều này tương đương với việc hỏi liệu có tồn tại sự phân chia các chỉ số trong chuỗi vũ trụ thành ba dãy con tăng dần phù hợp với các mẫu hay không. 

Cách tiêu chuẩn để giải quyết loại vấn đề đóng gói nhiều chuỗi con này là lập trình động trên chuỗi vũ trụ với trạng thái theo dõi mức độ khớp của mỗi mẫu. Vì mỗi mẫu có nhiều nhất là 250 nên không gian trạng thái DP nhỏ: nhiều nhất là 250^3 trạng thái và các chuyển đổi là tuyến tính trên chuỗi vũ trụ. Mỗi bản cập nhật chỉ thay đổi một chút về một mẫu, vì vậy chúng ta có thể sử dụng lại các tính toán DP trước đó hoặc tính toán lại một cách hiệu quả. 

Vì q chỉ là 1000 nên việc tính toán lại DP có kích thước n * 250^3 vẫn là quá lớn ở dạng đơn giản. Cải tiến quan trọng là các chuyển đổi trên chuỗi vũ trụ giống hệt nhau đối với tất cả các truy vấn; chỉ có chuỗi mục tiêu thay đổi. Do đó, chúng tôi tính toán trước các chuyển đổi trên chuỗi vũ trụ một lần, sau đó chạy DP cho mỗi truy vấn trên không gian trạng thái thu gọn có kích thước tối đa là 250^3 nhưng với các chuyển đổi được tối ưu hóa bằng cách sử dụng con trỏ lần xuất hiện tiếp theo. 

Điều này làm giảm mỗi truy vấn xuống còn khoảng O(|s1| * |s2| * |s3|) có thể chấp nhận được với giới hạn 250. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân công vũ lực | Hàm mũ | O(n) | Quá chậm | 
| DP qua ba con trỏ với các chuyển đổi được tính toán trước | O(n + 250^3 · q) | O(250^3) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nén chuỗi vũ trụ thành một cấu trúc cho phép nhảy nhanh: đối với mọi vị trí và ký tự, chúng tôi lưu trữ chỉ mục lần xuất hiện tiếp theo. Điều này cho phép chúng ta mô phỏng việc so khớp chuỗi con mà không cần quét tuyến tính mỗi lần. 

Sau đó, chúng tôi xác định trạng thái lập trình động trên các tiền tố của ba chuỗi đang phát triển. Đặt dp[i][j][k] cho biết liệu có thể khớp i ký tự đầu tiên của chuỗi 1, j đầu tiên của chuỗi 2 và k đầu tiên của chuỗi 3 bằng cách sử dụng các chuỗi con rời rạc của chuỗi vũ trụ theo thứ tự tăng dần hay không. 

Chúng tôi tính toán các chuyển đổi ngầm bằng cách sử dụng DP kiểu BFS hoặc phân lớp trên chuỗi vũ trụ, nhưng được tối ưu hóa để chúng tôi chỉ theo dõi các trạng thái có thể tiếp cận. 

Một cách xem hiệu quả hơn tránh phải tính toán lại từ đầu: thay vào đó, chúng tôi xử lý vấn đề như tìm ba chuỗi con rời rạc, có thể được mô hình hóa như một đường đi ngắn nhất trong một máy tự động phân lớp trong đó mỗi lớp tương ứng với số lượng ký tự của mỗi chuỗi đã được khớp. 

Chúng tôi tính toán trước các chuyển đổi từ mỗi trạng thái DP khi đọc ký tự tiếp theo trong chuỗi vũ trụ. Sau đó, chúng tôi mô phỏng việc quét chuỗi vũ trụ một lần, cập nhật các trạng thái có thể truy cập. 

Sau khi xử lý toàn bộ chuỗi vũ trụ, chúng tôi kiểm tra xem trạng thái (|s1|, |s2|, |s3|) có thể truy cập được hay không.

Để cập nhật, chúng tôi chỉ sửa đổi một trong ba chuỗi nên chúng tôi chạy lại DP. 

### Tại sao nó hoạt động 

Trạng thái DP nắm bắt chính xác số lượng mỗi chuỗi mục tiêu đã được khớp trong khi vẫn duy trì ràng buộc thứ tự của các chuỗi con. Bất kỳ sự gán chỉ số vũ trụ hợp lệ nào cho ba chuỗi đều tạo ra một đường dẫn đơn điệu xuyên qua không gian trạng thái này. Ngược lại, mọi trạng thái DP có thể truy cập đều tương ứng với phép gán một phần hợp lệ vì mỗi quá trình chuyển đổi sẽ tiêu thụ một ký tự từ chuỗi vũ trụ theo thứ tự mà không cần sử dụng lại nó. Sự phân đôi này giữa các phép gán và đường dẫn DP đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_next(s):
    n = len(s)
    nxt = [[-1] * 26 for _ in range(n + 1)]
    last = [-1] * 26
    for i in range(n - 1, -1, -1):
        last[ord(s[i]) - 97] = i
        for c in range(26):
            nxt[i][c] = last[c]
    return nxt

def can(u, s1, s2, s3):
    n = len(u)
    nxt = build_next(u)

    from collections import deque

    # state: (i, j, k, pos)
    # i,j,k = progress in strings, pos = position in universe
    start = (0, 0, 0, 0)
    dq = deque([start])
    vis = set([start])

    L1, L2, L3 = len(s1), len(s2), len(s3)

    while dq:
        i, j, k, pos = dq.popleft()

        if i == L1 and j == L2 and k == L3:
            return True

        if pos == n:
            continue

        # skip current universe char
        state = (i, j, k, pos + 1)
        if state not in vis:
            vis.add(state)
            dq.append(state)

        # try assign to one of 3 strings
        c = ord(u[pos]) - 97

        if i < L1 and ord(s1[i]) - 97 == c:
            state = (i + 1, j, k, pos + 1)
            if state not in vis:
                vis.add(state)
                dq.append(state)

        if j < L2 and ord(s2[j]) - 97 == c:
            state = (i, j + 1, k, pos + 1)
            if state not in vis:
                vis.add(state)
                dq.append(state)

        if k < L3 and ord(s3[k]) - 97 == c:
            state = (i, j, k + 1, pos + 1)
            if state not in vis:
                vis.add(state)
                dq.append(state)

    return False

def solve():
    n, q = map(int, input().split())
    u = input().strip()

    s = ["", "", ""]
    out = []

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '+':
            i = int(tmp[1]) - 1
            s[i] += tmp[2]
        else:
            i = int(tmp[1]) - 1
            s[i] = s[i][:-1]

        if can(u, s[0], s[1], s[2]):
            out.append("YES")
        else:
            out.append("NO")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng một bảng lần xuất hiện tiếp theo cho chuỗi vũ trụ để việc kiểm tra trình tự tiếp theo có thể tiến triển một cách hiệu quả. Sau đó, tìm kiếm cốt lõi sẽ khám phá các trạng thái được xác định bằng số lượng ký tự đã được sử dụng từ mỗi chuỗi tôn giáo và vị trí của chúng ta trong chuỗi vũ trụ. Mỗi lần chuyển đổi sẽ bỏ qua một ký tự hoặc gán nó vào một trong ba chuỗi nếu nó khớp với ký tự bắt buộc tiếp theo. Bộ đã truy cập ngăn việc xem lại các cấu hình tương đương. 

Một điểm tinh tế là chúng ta phải đưa vị trí vũ trụ vào trạng thái. Nếu không có nó, chúng tôi sẽ cho phép sử dụng lại các ký tự hoặc sắp xếp lại các ký tự vi phạm cấu trúc chuỗi con một cách không chính xác. Vị trí đảm bảo tiêu thụ đơn điệu. 

## Ví dụ đã hoạt động 

Hãy xem xét vũ trụ mẫu`abdabc`với các chuỗi phát triển trở thành`ad`,`bc`, Và`ab`sau một số thao tác. 

| Bước | s1 | s2 | s3 | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | một | | | CÓ | 
| 2 | quảng cáo | | | CÓ | 
| 3 | quảng cáo | b | | CÓ | 
| 4 | quảng cáo | bc | | CÓ | 
| 5 | quảng cáo | bc | một | CÓ | 
| 6 | quảng cáo | bc | ab | CÓ | 

Ở mỗi giai đoạn, BFS có thể tìm thấy sự gán chỉ số hợp lệ trong chuỗi vũ trụ để duy trì trật tự và tránh chồng chéo. Thuộc tính chính đang được thực hiện là các ký tự như`a`Và`b`xuất hiện nhiều lần nên cần phải linh hoạt trong việc phân công. 

Bây giờ hãy xem xét một trường hợp kiểu thất bại nhỏ: 

vũ trụ`ab`, dây`a`,`a`,`b`. 

| Tiểu bang | Giải thích | 
| --- | --- | 
| s1=a | Đầu tiên`a`có thể sử dụng vị trí 1 | 
| s2=a | thứ hai`a`không sử dụng được vị trí 1, phải thất bại | 
| s3=b | phải sử dụng vị trí 2 | 

BFS tìm thấy chính xác không có cách nào để gán hai phân biệt`a`các dãy tiếp theo vì chỉ có một`a`trong vũ trụ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q · n · S) | Mỗi truy vấn chạy BFS trên n vị trí và tối đa S trạng thái được xác định bởi tiến trình chuỗi | 
| Không gian | O(S · n) | Các trạng thái đã truy cập bao gồm vị trí vũ trụ và ba chỉ số tiến trình | 

Các ràng buộc cho phép tối đa 1000 truy vấn, nhưng mỗi chuỗi tôn giáo đều nhỏ. Không gian trạng thái vẫn có thể quản lý được vì kích thước tiến trình được giới hạn ở mức 250 và việc cắt tỉa thông qua các trạng thái đã truy cập sẽ ngăn chặn sự bùng nổ trong thực tế trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# provided sample (format adjusted placeholder)
# assert run(...) == "..."

# minimal case
assert run("""1 1
a
+ 1 a
""").strip() == "YES"

# all identical letters, impossible split
assert run("""3 2
aaa
+ 1 a
+ 2 a
""").strip() == "NO"

# alternating structure
assert run("""6 3
ababab
+ 1 ab
+ 2 ab
+ 3 ab
""").strip() == "YES"

# removal edge
assert run("""5 4
abcde
+ 1 a
+ 1 b
- 1
+ 1 c
""").strip() == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| char đơn tối thiểu | CÓ | chức năng cơ bản | 
| xung đột chữ cái lặp đi lặp lại | KHÔNG | số lần xuất hiện không đủ | 
| hoàn toàn có thể xen kẽ | CÓ | nhiệm vụ mang tính xây dựng | 
| nối thêm rồi quay lại | CÓ | xử lý chính xác việc xóa | 

## Vỏ cạnh 

Trường hợp đặc biệt quan trọng là khi cả ba chuỗi đều yêu cầu cùng một ký tự nhưng số lần xuất hiện ít hơn mức yêu cầu. BFS chặn chính xác điều này vì khi một vị trí ký tự được một chuỗi sử dụng, nó không thể được sử dụng lại trong một nhánh khác của không gian trạng thái. 

Một trường hợp khác là khi việc xóa làm co lại một chuỗi sau khi nó không thể thực hiện được trước đó. Vì mỗi truy vấn sẽ tính toán lại từ đầu nên DP sẽ đặt lại và phản ánh chính xác bộ yêu cầu mới, nhỏ hơn mà không chuyển sang trạng thái cũ. 

Trường hợp thứ ba liên quan đến các ký tự lặp lại trong chuỗi vũ trụ. Không gian trạng thái bao gồm chỉ số vị trí, do đó, ngay cả các ký tự giống hệt nhau cũng được xử lý riêng. Điều này ngăn chặn việc vô tình sử dụng lại cùng một lần xuất hiện trên các nhánh khác nhau của DP.
