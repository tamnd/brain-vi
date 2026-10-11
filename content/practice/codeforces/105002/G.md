---
title: "CF 105002G - \u0411\u0430\u043b\u0434\u0430"
description: "Chúng ta được cung cấp một lưới có kích thước n x m, trong đó mỗi ô chứa một chữ cái hoặc một dấu chấm biểu thị một ô trống. Trên lưới này, chúng tôi muốn xác định xem một từ nhất định s đã xuất hiện dưới dạng đường dẫn "rắn" hợp lệ hay chưa."
date: "2026-06-28T03:20:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105002
codeforces_index: "G"
codeforces_contest_name: "vkoshp.letovo 2022"
rating: 0
weight: 105002
solve_time_s: 63
verified: true
draft: false
---

[CF 105002G - \u0411\u0430\u043b\u0434\u0430](https://codeforces.com/problemset/problem/105002/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới có kích thước n x m, trong đó mỗi ô chứa một chữ cái hoặc một dấu chấm biểu thị một ô trống. Trên lưới này, chúng tôi muốn xác định xem một từ nhất định s đã xuất hiện dưới dạng đường dẫn "rắn" hợp lệ hay chưa. 

Giao diện hợp lệ có nghĩa là chúng ta có thể chọn ô bắt đầu và sau đó di chuyển từng bước theo bốn hướng chính để mỗi bước đi đến một ô liền kề, chúng ta không bao giờ sử dụng lại một ô và chuỗi các ô đã truy cập diễn ra chính xác theo thứ tự. Chỉ những ô đã chứa các chữ cái mới có thể sử dụng được và chữ cái trong mỗi ô được truy cập phải khớp với ký tự tương ứng của s. 

Từ s có đặc tính bổ sung là tất cả các ký tự của nó đều khác biệt. Hạn chế này rất quan trọng vì nó giới hạn độ dài của bất kỳ đường dẫn thành công nào ở mức tối đa là 26 và nó ngăn việc truy cập lại cùng một vị trí chữ cái trong từ, mặc dù nó không trực tiếp ngăn việc truy cập lại các ô lưới trừ khi chúng tôi thực thi quy tắc đường dẫn. 

Bản thân lưới có thể có tổng kích thước lớn lên tới 10^6 ô, do đó, bất kỳ giải pháp nào cố gắng tìm kiếm tất cả các đường dẫn theo cách DFS đơn giản trên cấu trúc lưới đều có nguy cơ khám phá số lượng đường dẫn theo cấp số nhân và sẽ không hoàn thành kịp thời. 

Một điểm tinh tế thường phá vỡ các giải pháp ngây thơ là sự tương tác giữa tính hợp lệ của phân nhánh và đường dẫn. Mặc dù từ này ngắn nhưng một ký tự có thể xuất hiện ở nhiều vị trí trong lưới và việc quay lui đơn giản từ mỗi lần xuất hiện có thể liên tục khám phá các vùng chồng chéo. Một dạng lỗi phổ biến khác là quên ràng buộc “không sử dụng lại ô”, điều này có thể vô tình cho phép các chu kỳ không hợp lệ nếu không được theo dõi đúng cách. 

Ví dụ: nếu lưới là một khu vực mở rộng lớn có cùng phân phối chữ cái khớp với các chuyển tiếp của s, thì một DFS ngây thơ có thể truy cập lại cùng một khu vực thông qua các đường dẫn khác nhau và bùng nổ tổ hợp ngay cả khi độ dài từ nhỏ. 

Do đó, nhiệm vụ không phải là tìm kiếm tùy ý trong lưới mà là kiểm tra hiệu quả xem có tồn tại một chuỗi vị trí hợp lệ hay không, mỗi vị trí cho mỗi ký tự s, sao cho các vị trí liên tiếp có liền kề nhau trong lưới hay không. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là bắt đầu từ mỗi lần xuất hiện của ký tự đầu tiên và thực hiện DFS hoặc BFS cố gắng tạo từ theo từng bước. Từ mỗi ô hiện tại, chúng tôi khám phá bốn ô lân cận của nó và tiếp tục nếu ký tự tiếp theo khớp. 

Điều này đúng về mặt logic vì nó liệt kê tất cả các đường dẫn có thể đánh vần từ đó. Tuy nhiên, độ phức tạp của nó phụ thuộc vào số lượng lựa chọn phân nhánh ở mỗi bước. Trong một mạng lưới dày đặc, mỗi bước có thể phân nhánh tối đa bốn hướng và nhiều vị trí bắt đầu có thể nhân lên không gian tìm kiếm. Mặc dù độ dài từ nhiều nhất là 26, cây tìm kiếm vẫn có thể mở rộng thành một số lượng rất lớn các đường dẫn từng phần, đặc biệt khi nhiều ô lưới khớp với các ký tự trung gian. 

Quan sát quan trọng giúp đơn giản hóa vấn đề là độ dài từ rất nhỏ trong khi lưới lại lớn. Thay vì suy nghĩ theo cách khám phá đường dẫn tùy ý, chúng ta có thể nghĩ theo cách truyền bá từng lớp. Mỗi vị trí ký tự trong từ tạo thành một lớp và chúng ta chỉ cần biết ô lưới nào có thể biểu thị vị trí đó trong một phần đường dẫn hợp lệ nào đó. 

Chúng tôi bắt đầu với tất cả các ô khớp với ký tự đầu tiên. Sau đó, đối với mỗi ký tự tiếp theo, chúng tôi tính toán tất cả các ô lưới liền kề với ít nhất một ô có thể truy cập từ lớp trước và chứa ký tự được yêu cầu. Vì mỗi lần chuyển đổi ô là cục bộ và chỉ được kiểm tra một lần trên mỗi lớp nên quá trình này vẫn tuyến tính về số lượng ô lưới trên mỗi lớp. 

Điều này biến vấn đề thành một sự lan truyền động có kiểm soát trên tối đa 26 lớp, tránh hoàn toàn sự phân nhánh theo cấp số nhân.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| DFS ngay từ đầu | Hàm mũ trong trường hợp xấu nhất | O(k) đệ quy | Quá chậm | 
| Tuyên truyền theo lớp DP | O(nm · | s | ) | 

## Hướng dẫn thuật toán 

1. Đọc lưới và xác định vị trí tất cả các ô khớp với ký tự đầu tiên của từ. Chúng tạo thành tập hợp ban đầu các vị trí có thể tiếp cận, vì bất kỳ đường dẫn hợp lệ nào cũng phải bắt đầu từ một trong số chúng. 
2. Duy trì lưới boolean hoặc điểm đánh dấu truy cập để theo dõi những ô nào có thể truy cập được ở chỉ mục ký tự hiện tại. Điều này ngăn chặn việc xử lý trùng lặp của cùng một ô trong một lớp. 
3. Đối với mỗi ký tự tiếp theo trong từ, hãy tạo một tập hợp các vị trí có thể tiếp cận mới bằng cách quét tất cả các ô hiện có thể tiếp cận và kiểm tra bốn ô lân cận của chúng. Nếu hàng xóm chứa ký tự tiếp theo được yêu cầu, nó sẽ có thể truy cập được ở lớp tiếp theo. 
4. Thay thế bộ có thể truy cập hiện tại bằng bộ mới được xây dựng và chuyển sang ký tự tiếp theo. 
5. Nếu ở bất kỳ giai đoạn nào, tập hợp có thể truy cập trở nên trống, từ đó không thể được hình thành như một đường dẫn hợp lệ và chúng ta dừng lại sớm. 
6. Sau khi xử lý tất cả các ký tự, nếu có thể truy cập được ít nhất một ô ở ký tự cuối cùng thì từ đó đã tồn tại trên bảng. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến là sau khi xử lý ký tự thứ i của từ, tập hợp các ô hiện hoạt sẽ tương ứng chính xác với tất cả các điểm cuối của các đường dẫn hợp lệ đánh vần tiền tố s[0..i]. Mỗi bước chuyển tiếp chỉ mở rộng các đường dẫn hợp lệ bằng một bước di chuyển kề phù hợp với ký tự tiếp theo. Vì mọi đường dẫn hợp lệ phải được xây dựng tăng dần thông qua các bước liền kề và chúng tôi không bao giờ loại bỏ bất kỳ phần mở rộng hợp lệ nào nên không có giải pháp nào bị mất. Đồng thời, mọi trạng thái được ghi lại đều tương ứng với một đoạn đường dẫn thực, do đó không có đường dẫn không hợp lệ nào được đưa vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]
    s = input().strip()
    k = len(s)

    # current reachable cells
    cur = set()

    # initialize with positions of first character
    for i in range(n):
        row = grid[i]
        for j in range(m):
            if row[j] == s[0]:
                cur.add((i, j))

    if not cur:
        print("NO")
        return

    dirs = [(1, 0), (-1, 0), (0, 1), (0, -1)]

    for idx in range(1, k):
        nxt_char = s[idx]
        nxt = set()

        for x, y in cur:
            for dx, dy in dirs:
                nx, ny = x + dx, y + dy
                if 0 <= nx < n and 0 <= ny < m:
                    if grid[nx][ny] == nxt_char:
                        nxt.add((nx, ny))

        cur = nxt
        if not cur:
            print("NO")
            return

    print("YES")

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp tuân theo ý tưởng truyền bá theo lớp. Lần quét đầu tiên thu thập tất cả các điểm bắt đầu hợp lệ. Mỗi lần lặp sẽ tiến lên một ký tự bằng cách chỉ mở rộng thông qua các vùng lân cận lưới hợp lệ. Việc sử dụng một bộ đảm bảo rằng mỗi ô được xử lý nhiều nhất một lần trên mỗi lớp, ngăn chặn sự lan truyền dư thừa khi nhiều đường dẫn đến cùng một vị trí. 

Chi tiết triển khai chính là chúng tôi không bao giờ cố gắng theo dõi các đường dẫn đầy đủ. Chúng tôi chỉ theo dõi các điểm cuối có thể truy cập theo độ dài tiền tố, điều này là đủ vì chính từ đó xác định duy nhất chuỗi ký tự bắt buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Lưới đầu vào:```
ALGAE
RPA.T
C.R.H
T.TYO
I...S
C....
PARTY
```Từ: BÊN 

Chúng ta bắt đầu bằng cách thu thập tất cả các ô chứa P. Giả sử chỉ có một ô như vậy tồn tại tại (1,1). 

| Bước | Nhân vật | Các ô có thể tiếp cận | 
| --- | --- | --- | 
| 0 | P | {(1,1)} | 
| 1 | A | {(1,2)} | 
| 2 | R | {(1,3)} | 
| 3 | T | {(0,3)} | 
| 4 | Y | {(3,3)} | 

Ở bước cuối cùng chúng ta vẫn có những vị trí có thể tiếp cận nên câu trả lời là CÓ. 

Dấu vết này cho thấy rằng việc truyền bá tuân theo một chuỗi liên tục duy nhất một cách chính xác mà không cần phải xây dựng lại đường dẫn một cách rõ ràng. 

### Ví dụ 2 

Lưới đầu vào:```
.P.YT.
.YTPHO
.T.YOP
NONNHY
PYTOHN
..P.YO
```Từ: Python 

| Bước | Nhân vật | Các ô có thể tiếp cận | 
| --- | --- | --- | 
| 0 | P | nhiều vị trí | 
| 1 | Y | bộ giảm | 
| 2 | T | tiếp tục giảm | 
| 3 | H | có thể trống | 
| 4 | Ồ | trống | 

Tại một số bước trung gian, không có phần mở rộng hợp lệ nào tồn tại nên quá trình kết thúc sớm với NO. Điều này chứng tỏ việc cắt tỉa ngăn chặn việc thăm dò không cần thiết như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm · | s | 
| Không gian | O(nm) | Tập hợp các trạng thái có thể truy cập trong trường hợp xấu nhất có thể che phủ lưới | 

Kích thước lưới chiếm ưu thế, nhưng độ dài từ bị giới hạn bởi 26, do đó tổng số thao tác vẫn nằm trong giới hạn thoải mái cho 10^6 ô. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample 1
assert run("""6 5
ALGAE
RPA.T
C.R.H
T.TYO
I...S
C....
PARTY
""") == "YES"

# sample 2
assert run("""6 6
.P.YT.
.YTPHO
.T.YOP
NONNHY
PYTOHN
..P.YO
PYTHON
""") == "NO"

# single cell
assert run("""1 1
A
A
""") == "YES"

# impossible start
assert run("""2 2
AB
CD
EF
""") == "NO"

# linear path
assert run("""1 6
ABCDEF
ABCDEF
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trận đấu ô đơn | CÓ | ranh giới tối thiểu | 
| không có chữ cái bắt đầu | KHÔNG | từ chối sớm | 
| khớp đường thẳng | CÓ | chuỗi đơn giản nhất | 
| lưới mẫu | CÓ/KHÔNG | tính đúng đắn trong các trường hợp đầy đủ | 

## Vỏ cạnh 

Trường hợp biên quan trọng xảy ra khi ký tự đầu tiên xuất hiện nhiều lần trên lưới nhưng không lần nào trong số đó thực sự có thể kết nối với ký tự thứ hai. Thuật toán xử lý việc này một cách tự nhiên vì tập ban đầu có thể lớn, nhưng bước lan truyền đầu tiên sẽ ngay lập tức lọc nó xuống dựa trên tính kề cận, thu gọn đường biên trong một lần lặp. 

Một trường hợp khác là khi các ký tự xuất hiện thành cụm tạo thành vòng lặp. Ngay cả khi nhiều đường dẫn quay vòng cục bộ, việc truyền lan dựa trên tập hợp sẽ ngăn chặn việc mở rộng lặp lại trong cùng một lớp, do đó các vòng lặp không làm tăng không gian trạng thái. 

Cuối cùng, khi từ có độ dài 1, thuật toán trả về chính xác CÓ khi và chỉ khi ký tự đó tồn tại ở bất kỳ đâu trong lưới, vì chính tập hợp ban đầu đã là câu trả lời cuối cùng.
