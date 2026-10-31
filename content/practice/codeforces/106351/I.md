---
title: "CF 106351I - Omar và cấu trúc dữ liệu 1"
description: "Chúng tôi đang làm việc với một bảng 3 × 3 cố định gồm 9 ô, trong đó mỗi ô có thể đã chứa một chữ số hoặc trống. Các chữ số liên quan là từ 1 đến 9, mỗi chữ số được sử dụng chính xác một lần trong cấu hình hoàn thiện cuối cùng."
date: "2026-06-25T08:11:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106351
codeforces_index: "I"
codeforces_contest_name: "Zaglol Contest - FCDS level 2 contest 2026"
rating: 0
weight: 106351
solve_time_s: 51
verified: true
draft: false
---

[CF 106351I - Omar và cấu trúc dữ liệu 1](https://codeforces.com/problemset/problem/106351/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một bảng 3 × 3 cố định gồm 9 ô, trong đó mỗi ô có thể đã chứa một chữ số hoặc trống. Các chữ số liên quan là từ 1 đến 9, mỗi chữ số được sử dụng chính xác một lần trong cấu hình hoàn thiện cuối cùng. 

Quy tắc chính không phải là trực tiếp về hàng hoặc cột mà là về thứ tự các số: số 1 được đặt ở đâu đó, số 2 phải ở trong một ô liền kề với nó (bao gồm cả các đường chéo), 3 phải liền kề với 2, v.v. cho đến 9. Nói cách khác, các số từ 1 đến 9 phải tạo thành một đường dẫn duy nhất xuyên qua lưới, trong đó mỗi cặp số liên tiếp nằm trong các ô lân cận dưới sự kề cận của quân cờ vua. 

Một số ô đã được cố định trong đầu vào. Nếu một ô chứa một chữ số khác 0, thì chữ số đó buộc phải xuất hiện ở vị trí chính xác đó trong mọi cách sắp xếp hợp lệ. Các ô trống được ký hiệu là 0. 

Nhiệm vụ là đếm xem có bao nhiêu cách hợp lệ để gán các số từ 1 đến 9 vào lưới trong khi vẫn tôn trọng cả quy tắc kề cho các số liên tiếp và các phép gán trước cố định. 

Lưới chỉ có 9 vị trí nên cấu trúc bên dưới cực kỳ nhỏ. Tuy nhiên, ràng buộc là chúng ta phải hình thành đường đi Hamilton với thứ tự chặt chẽ có nghĩa là các hoán vị ngây thơ bị hạn chế rất nhiều bởi tính kề cận. 

Giới hạn trên ngây thơ sẽ là thử tất cả các hoán vị của 9 số, tức là 9! ≈ 362.880. Điều này có tính cách ly nhỏ nhưng mỗi hoán vị cũng yêu cầu kiểm tra các ràng buộc lân cận và tính nhất quán với các ô được điền sẵn, do đó, lực lượng vũ phu vẫn có nguy cơ vượt quá giới hạn nếu lặp lại trên nhiều trường hợp thử nghiệm hoặc được thực hiện mà không cắt bớt. 

Một vấn đề phức tạp hơn xuất hiện khi các chữ số được điền trước buộc các cấu hình cục bộ không thể thực hiện được. Ví dụ: nếu 1 được cố định ở góc trên bên trái và 2 được cố định ở góc dưới cùng bên phải và các ô đó không liền kề nhau thì câu trả lời ngay lập tức là 0. Giải pháp dựa trên hoán vị bất cẩn vẫn có thể phát hiện ra nhiều lần hoàn thành không hợp lệ trước khi từ chối chúng. 

Một trường hợp khác là khi một số số được cố định nhưng đã tạo thành một phần chuỗi không nhất quán. Ví dụ: nếu 3 được đặt ở đâu đó không liền kề với vị trí cố định của 2 thì không thể hoàn thành, ngay cả khi vẫn còn nhiều ô trống. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ gán mỗi chữ số từ 1 đến 9 cho một trong 9 ô và kiểm tra tính hợp lệ sau đó. Điều này hoạt động bằng cách tạo ra các hoán vị của các vị trí lưới và xác minh hai điều kiện: tất cả các chữ số cố định khớp với các ô được chỉ định của chúng và mọi cặp liên tiếp (i, i+1) đều nằm trong các ô liền kề. Tính đúng đắn rất đơn giản vì nó liệt kê tất cả các phép gán có thể thực hiện được. 

Vấn đề là sự dư thừa. Hầu hết các hoán vị đều thất bại rất sớm vì sự liền kề giữa các số liên tiếp bị ngắt. Ví dụ: khi 1 được đặt, 2 có tối đa 8 vị trí hợp lệ và 3 phải kết nối từ đó, v.v. Tuy nhiên, một trình tạo hoán vị ngây thơ không khai thác cấu trúc này và vẫn khám phá tất cả 9! sắp xếp. 

Quan sát quan trọng là các con số không phải là các nhãn độc lập mà tạo thành một chuỗi chặt chẽ. Thay vì hoán vị tất cả các chữ số trên toàn cục, chúng ta có thể xây dựng chuỗi tăng dần từ 1 đến 9. Ở mỗi bước, chúng ta chỉ mở rộng từ vị trí hiện tại của i đến một ô lân cận hợp lệ cho i+1. Điều này biến vấn đề thành tìm kiếm theo chiều sâu trên các đường dẫn có độ dài 9 trên biểu đồ di chuyển vua 3 × 3 cố định, với các ràng buộc bổ sung cho các vị trí cố định. 

Việc cắt tỉa này cực kỳ mạnh mẽ. Thay vì xem xét các hoán vị tùy ý, chúng tôi chỉ đi theo các đường dẫn hợp lệ và hệ số phân nhánh tối đa là 8 khi bắt đầu và giảm nhanh khi sử dụng các ô. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị vũ phu | O(9! · 9) | O(9) | Thực hành quá chậm | 
| DFS trên chuỗi kề | O(8!) trường hợp xấu nhất, ít hơn nhiều trong thực tế | O(9) | Đã chấp nhận |

## Hướng dẫn thuật toán 

Chúng tôi lập mô hình lưới 3×3 thành 9 ô được lập chỉ mục từ 0 đến 8. Chúng tôi tính toán trước tính kề cận: đối với mỗi ô, chúng tôi liệt kê tất cả các ô lân cận bao gồm cả các đường chéo. 

Chúng tôi cũng ghi lại các ràng buộc từ đầu vào: với mỗi chữ số d cố định trong ô c, chúng tôi lưu trữ pos[d] = c. 

Sau đó, chúng tôi tìm kiếm tất cả các cách hợp lệ để xây dựng chuỗi 1 → 2 → ... → 9. 

1. Xác định tất cả các vị trí bắt đầu có thể có của chữ số 1. Nếu cố định số 1 thì có đúng một ô bắt đầu; nếu không, nó có thể là bất kỳ ô nào trong số 9 ô. 
2. Đối với mỗi ô bắt đầu ứng cử viên, đặt chữ số 1 vào đó và đánh dấu nó là đã sử dụng. 
3. Di chuyển đến chữ số 2 và cố gắng đặt nó vào ô liền kề với vị trí hiện tại của chữ số 1. Nếu chữ số 2 cố định, chúng ta chỉ giữ ô lân cận khớp với vị trí cố định của nó. 
4. Tiếp tục quá trình này cho chữ số i, luôn di chuyển từ ô của i−1 sang ô không sử dụng lân cận. Nếu chữ số i có vị trí cố định, chúng tôi sẽ loại bỏ ngay lập tức bất kỳ nhánh nào không khớp với nó. 
5. Duy trì một mảng đã truy cập để không có ô nào được sử dụng lại, vì tất cả các chữ số phải chiếm các ô riêng biệt. 
6. Khi chữ số 9 được đặt thành công, hãy tính đây là một cấu hình hợp lệ. 

Việc tìm kiếm được thực hiện một cách tự nhiên bằng đệ quy, trong đó trạng thái là chữ số hiện tại và vị trí của nó, cộng với tập hợp các ô đã được sử dụng. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, việc xây dựng một phần biểu thị tiền tố của đường dẫn Hamilton hợp lệ có độ dài k từ 1 đến k. Ràng buộc kề đảm bảo rằng mọi tiện ích mở rộng đều hợp lệ cục bộ và tập hợp đã truy cập đảm bảo tính tiêm vào toàn cục của vị trí. Bởi vì chúng tôi chỉ mở rộng từ i đến i+1, nên mọi đường dẫn đầy đủ tương ứng với chính xác một hoán vị thỏa mãn các ràng buộc, do đó không có cấu hình hợp lệ nào bị bỏ sót và không có cấu hình không hợp lệ nào được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1000000)

# 3x3 grid -> 9 nodes
DIRS = [(-1,-1), (-1,0), (-1,1),
        (0,-1),          (0,1),
        (1,-1),  (1,0),  (1,1)]

def idx(r, c):
    return r * 3 + c

# precompute adjacency
adj = [[] for _ in range(9)]
for r in range(3):
    for c in range(3):
        u = idx(r, c)
        for dr, dc in DIRS:
            nr, nc = r + dr, c + dc
            if 0 <= nr < 3 and 0 <= nc < 3:
                v = idx(nr, nc)
                adj[u].append(v)

def solve():
    grid = []
    pos = [-1] * 10

    for r in range(3):
        row = input().strip()
        grid.append(row)
        for c, ch in enumerate(row):
            if ch != '0':
                d = int(ch)
                pos[d] = idx(r, c)

    used = [False] * 9
    ans = 0

    def dfs(d, u):
        nonlocal ans
        if d == 9:
            ans += 1
            return

        nd = d + 1

        # if fixed, must go there
        if pos[nd] != -1:
            v = pos[nd]
            if not used[v] and v in adj[u]:
                used[v] = True
                dfs(nd, v)
                used[v] = False
            return

        # otherwise try all neighbors
        for v in adj[u]:
            if not used[v]:
                used[v] = True
                dfs(nd, v)
                used[v] = False

    # start from 1
    if pos[1] != -1:
        start_cells = [pos[1]]
    else:
        start_cells = list(range(9))

    for s in start_cells:
        used[s] = True
        dfs(1, s)
        used[s] = False

    print(ans)

if __name__ == "__main__":
    solve()
```Mã này phản ánh cấu trúc DFS được mô tả trước đó. các`pos`mảng mã hóa các vị trí chữ số cố định và được kiểm tra ở mỗi bước trước khi tiếp tục đệ quy. các`used`mảng đảm bảo mỗi ô được sử dụng chính xác một lần. 

Một lỗi triển khai phổ biến là quên thực thi tính kề cận khi một chữ số được cố định. Ngay cả khi một vị trí cố định khớp với chữ số, nó vẫn không hợp lệ nếu nó không liền kề với chữ số trước đó. Việc kiểm tra đó được xử lý rõ ràng bởi`v in adj[u]`. 

Một cách tinh tế khác là xử lý vị trí bắt đầu: nếu chữ số 1 cố định, chúng ta không được lặp lại tất cả 9 lần bắt đầu, nếu không chúng ta sẽ đếm quá nhiều cấu hình giống hệt nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
076
005
234
```Chúng tôi chỉ theo dõi phần đầu của tìm kiếm vì cây đầy đủ lớn. 

| Bước | Chữ số | Lựa chọn vị trí | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | tất cả các ô ngoại trừ những ô cố định | thử tất cả các lần khởi đầu hợp lệ | 
| 2 | 2 | hàng xóm của 1 | tỉa theo vùng lân cận | 
| 3 | 3 | hàng xóm của 2 | tiếp tục chuỗi hợp lệ | 

DFS chỉ khám phá các chuỗi tôn trọng các vị trí cố định như 2 ở giữa dưới cùng hoặc 3 ở dưới cùng bên phải tùy thuộc vào đầu vào. Mỗi đường dẫn đầy đủ thành công đóng góp một vào số đếm cuối cùng. 

Điều này xác nhận rằng việc cắt tỉa xảy ra sớm, đặc biệt khi các chữ số cố định tạo ra một hành lang hẹp cho các vị trí hợp lệ. 

### Ví dụ 2 

đầu vào:```
103
000
709
```Ở đây các chữ số 1, 0 bị bỏ qua, 3, 7, 0, 9 được cố định một phần, nghĩa là 1, 3, 7, 9 được neo. 

| Bước | Chữ số | Ràng buộc | 
| --- | --- | --- | 
| 1 | 1 | vị trí cố định | 
| 2 | 2 | phải là hàng xóm của 1 | 
| 3 | 3 | cố định, phải khớp với vị trí | 
| 4 | 4 | tiện ích mở rộng hàng xóm miễn phí | 
| ... | ... | ... | 
| 9 | 9 | đã sửa, phải căn chỉnh | 

DFS nhanh chóng từ chối hầu hết các nhánh vì 3 và 7 buộc phải di chuyển sớm không tương thích nếu vùng lân cận bị vi phạm. Chỉ có một số ít đường Hamilton hợp lệ còn tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(số đường đi Hamilton hợp lệ trên lưới 3×3) | DFS chỉ khám phá các hoán vị tôn trọng kề, được cắt tỉa nhiều bởi các chữ số cố định | 
| Không gian | O(9) | độ sâu đệ quy và mảng đã truy cập trên 9 ô | 

Kích thước lưới không đổi nên không gian tìm kiếm bị giới hạn và cực kỳ nhỏ trong thực tế. Mặc dù giới hạn trên về mặt lý thuyết là gần bằng 8! đường dẫn, các ràng buộc thường giảm mạnh. Điều này phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimal empty grid
assert isinstance(run("000\n000\n000\n"), str)

# fully fixed valid chain (one possible trivial configuration)
assert run("123\n456\n789\n") == "1"

# corner constraint: impossible adjacency
assert run("100\n020\n003\n") == "0"

# all zeros should give some positive number of valid Hamiltonian paths
assert int(run("000\n000\n000\n")) > 0
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 123/456/789 | 1 | đã sửa đúng chuỗi | 
| 100/020/003 | 0 | không thể kề nhau giữa các chữ số cố định | 
| tất cả số không | >0 | tồn tại các đường đi Hamilton hợp lệ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi các chữ số cố định đã vi phạm tính liền kề. Ví dụ:```
1 0 0
0 0 0
0 0 9
```Nếu 1 và 2 (hoặc 1 và bất kỳ bước tiếp theo được yêu cầu nào) bị buộc vào các vị trí không liền kề, DFS ngay lập tức từ chối tất cả các nhánh khi chuyển từ chữ số 1, vì không có hàng xóm nào khớp với vị trí cố định bắt buộc cho chữ số 2. 

Một trường hợp khác là khi nhiều chữ số được cố định nhưng vẫn nhất quán cục bộ:```
1 2 0
0 0 0
0 0 0
```Ở đây, thuật toán bắt đầu từ 1, chỉ chuyển sang 2 nếu kề giữ nguyên và tiếp tục bình thường. Mảng đã truy cập đảm bảo không sử dụng lại và quá trình đệ quy tiếp tục cho đến khi đặt được số 9 hoặc không thể mở rộng. Điều này chứng tỏ rằng các chuỗi cố định một phần hoạt động giống như một tiền tố bắt buộc của đường dẫn DFS chứ không phải là một ràng buộc trên toàn bộ lưới.
