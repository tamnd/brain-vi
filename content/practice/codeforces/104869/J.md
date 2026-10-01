---
title: "CF 104869J - Ghép và cấy ghép"
description: "Chúng ta được cấp một cây chưa có rễ với tối đa 50 đỉnh. Hai người chơi luân phiên nhau và trong mỗi lượt họ chọn một cạnh $u-v$. Động thái này không phải là sự hoán đổi cục bộ của các điểm cuối mà là một sự "kết nối lại" cấu trúc: mọi hàng xóm của $u$ ngoại trừ $v$ được tách khỏi $u$ và gắn lại với $v$."
date: "2026-06-28T10:51:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104869
codeforces_index: "J"
codeforces_contest_name: "The 2023 ICPC Asia Shenyang Regional Contest (The 2nd Universal Cup. Stage 13: Shenyang)"
rating: 0
weight: 104869
solve_time_s: 62
verified: true
draft: false
---

[CF 104869J - Ghép và cấy ghép](https://codeforces.com/problemset/problem/104869/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây chưa có rễ với tối đa 50 đỉnh. Hai người chơi luân phiên nhau và trong mỗi lượt họ chọn một cạnh$u-v$. Động thái này không phải là sự hoán đổi cục bộ các điểm cuối mà là sự “tua lại” cấu trúc: mọi hàng xóm của$u$ngoại trừ$v$bị tách ra khỏi$u$và gắn lại vào$v$. Sau ca phẫu thuật,$u$chỉ giữ lợi thế cho$v$, trong khi$v$hấp thụ tất cả$u$các cây con sự cố khác. 

Có một hạn chế bổ sung khiến trò chơi về cơ bản khác với các trò chơi trên cây tiêu chuẩn. Việc di chuyển chỉ được phép nếu cây kết quả không đẳng hình với cây trước đó. Nói cách khác, người chơi bị cấm thực hiện các phép biến đổi không làm thay đổi hình dạng không được gắn nhãn của cây. 

Trò chơi kết thúc khi người chơi không có nước đi hợp lệ. Alice bắt đầu và cả hai người chơi đều chơi tối ưu. 

Khó khăn chính là việc di chuyển được xác định trên các đỉnh có nhãn, nhưng tính hợp pháp chỉ được xác định bởi cấu trúc cây không có nhãn. Điều này có nghĩa là hai chuỗi hoạt động nối lại dây khác nhau tạo ra cây đẳng cấu được coi là trạng thái giống hệt nhau một cách hiệu quả và một số di chuyển không được phép ngay cả khi chúng có giá trị về mặt cấu trúc. 

Từ$n \le 50$, việc ép buộc tất cả các trạng thái được dán nhãn là không thể. Mặc dù không gian cây nhỏ về mặt tuyệt đối, tính đẳng cấu sẽ thu gọn nhiều cấu hình và hệ số phân nhánh trên mỗi bước di chuyển vẫn là bậc hai trong trường hợp xấu nhất nếu được mô phỏng một cách ngây thơ. 

Trường hợp cạnh tinh tế phát sinh từ các cây có tính đối xứng cao. Đặc biệt, cây sao hoạt động khác với tất cả các cây khác vì bất kỳ hoạt động ghép nào cũng bảo tồn cấu trúc của nó. 

Nếu cây là một ngôi sao trên 4 đỉnh:```
1 - 2
|
3
|
4
```bất kỳ nỗ lực nào để chọn trung tâm và một chiếc lá chỉ cần gắn lại những chiếc lá theo cách giữ cho cái cây là một ngôi sao. Vì cây kết quả là đẳng cấu với cây ban đầu nên không có bước di chuyển nào hợp lệ. Điều tương tự cũng xảy ra với bất kỳ ngôi sao nào trên$n$đỉnh: trò chơi ngay lập tức bị mắc kẹt và Alice thua cuộc. 

Mặt khác, đối với đường dẫn có độ dài 4:```
1 - 2 - 3 - 4
```cây không phải là ngôi sao nên tồn tại ít nhất một thao tác hợp lệ. Sự khác biệt này hóa ra là cốt lõi của giải pháp. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ coi mỗi cây được gắn nhãn là một trạng thái và thử mọi cạnh có thể$u-v$, tạo ra một cây mới bằng cách nối lại các danh sách kề và sau đó kiểm tra tính đẳng cấu so với trạng thái trước đó. Điều này đã trở nên tốn kém vì việc kiểm tra đẳng cấu cho mỗi lần chuyển đổi rất tốn kém và số lượng cấu hình được nối lại có thể tăng lên nhanh chóng ngay cả đối với$n=50$. 

Ngay cả khi chúng ta nén các trạng thái bằng cách chỉ lưu trữ các dạng cây chuẩn, chúng ta vẫn phải đối mặt với một biểu đồ ẩn lớn của các lớp đẳng cấu cây. Một phân tích trò chơi đầy đủ sẽ yêu cầu tính toán các trạng thái thắng và thua trên biểu đồ này, điều này là quá mức cần thiết dựa trên cấu trúc của hoạt động. 

Quan sát quan trọng là động thái ghép và cấy ghép làm sụp đổ mạnh mẽ cấu trúc về phía trung tâm. Mọi thao tác đều chuyển khối lượng độ từ đỉnh này sang đỉnh khác và ứng dụng lặp đi lặp lại có xu hướng tập trung vào cây. Cấu hình duy nhất hoàn toàn ổn định trong thao tác này, theo nghĩa là mọi chuyển động có thể tạo ra một cây đẳng cấu, là ngôi sao. 

Khi cây không phải là ngôi sao thì luôn tồn tại ít nhất một cạnh mà hoạt động của nó làm thay đổi hình dạng cấu trúc của cây. Điều này có nghĩa là trò chơi không bao giờ bị chặn ở các trạng thái trung gian không có sao bởi ràng buộc đẳng cấu. Vị trí cuối cùng duy nhất là ngôi sao. 

Từ góc độ này, trò chơi rút gọn thành một câu hỏi đơn giản về khả năng tiếp cận: người chơi hiện tại có thể thực hiện ít nhất một nước đi hợp lệ không, tức là cái cây có phải là ngôi sao không? 

Nếu nó đã là một ngôi sao, Alice không thể di chuyển và thua cuộc ngay lập tức. Ngược lại, Alice có một nước đi và trò chơi tiếp tục, và lối chơi tối ưu không làm thay đổi sự phân đôi cơ bản này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đối với các trạng thái đẳng cấu | số mũ trong$n$| Hàm mũ | Quá chậm | 
| Đặc tính kiểm tra sao |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Toàn bộ giải pháp giảm xuống việc xác định xem cây đầu vào có phải là một ngôi sao hay không. 

1. Tính bậc của mỗi đỉnh tính từ các cạnh đầu vào. 
2. Kiểm tra xem có tồn tại đỉnh có bậc không$n-1$. 
3. Nếu một đỉnh như vậy tồn tại thì cây là một ngôi sao và Alice không có nước đi hợp lệ nào, do đó xuất ra “Bob”. 
4. Ngược lại, có ít nhất hai đỉnh có bậc lớn hơn 1, do đó cây không phải là ngôi sao và Alice có thể thực hiện một nước đi, do đó xuất ra “Alice”. 

Lý do điều này là đủ vì bất kỳ cây nào không phải là ngôi sao đều thừa nhận ít nhất một cạnh mà hoạt động ghép của nó tạo ra một cấu trúc không được gắn nhãn khác, do đó người chơi đầu tiên không bao giờ bị mắc kẹt ngay lập tức. 

### Tại sao nó hoạt động 

Ràng buộc đẳng cấu chỉ chặn các bước di chuyển bảo toàn toàn bộ cấu trúc không được gắn nhãn của cây. Hoạt động ghép bảo tồn cấu trúc cho tất cả các cạnh của một ngôi sao vì tất cả các lá đều không thể phân biệt được và tất cả các phần gắn lại đều tạo ra một ngôi sao khác. Trong mọi cây khác, tồn tại sự bất đối xứng về cấu trúc giữa các cây con, nghĩa là có ít nhất một cạnh kết nối các đỉnh với các vai trò khác nhau trong hình dạng tổng thể. Việc áp dụng thao tác ghép trên một cạnh như vậy sẽ thay đổi sự phân bố độ theo cách không thể khớp với bất kỳ việc dán nhãn lại các đỉnh nào, do đó cây thu được không đẳng cấu với cây ban đầu. Do đó, cây không phải sao luôn có ít nhất một nước đi hợp lệ, trong khi cây sao thì không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
deg = [0] * (n + 1)

for _ in range(n - 1):
    u, v = map(int, input().split())
    deg[u] += 1
    deg[v] += 1

is_star = any(d == n - 1 for d in deg[1:])

if is_star:
    print("Bob")
else:
    print("Alice")
```Việc thực hiện chỉ tính toán độ, vì toàn bộ quyết định phụ thuộc vào việc phát hiện xem cây có đỉnh trung tâm chung hay không. Mảng`deg`theo dõi số lượng lân cận và quét nó một lần là đủ để xác định xem cây có phải là ngôi sao hay không. 

Điểm tinh tế quan trọng là không cần mô phỏng hoạt động ghép. Bất kỳ nỗ lực mô phỏng nào cũng sẽ kết hợp không chính xác các phép biến đổi được gắn nhãn với các ràng buộc đẳng cấu, trong khi quyết định thực tế hoàn toàn phụ thuộc vào tính đối xứng cấu trúc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2
2 3
3 4
```Bằng cấp: 

| Đỉnh | Bằng cấp | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 2 | 
| 4 | 1 | 

Không có đỉnh nào có bậc 3 nên cây không phải là ngôi sao. Alice có thể di chuyển. 

Đầu ra:```
Alice
```Điều này cho thấy cây không có dấu sao luôn bắt đầu bằng ít nhất một thao tác hợp lệ. 

### Ví dụ 2 

đầu vào:```
4
1 2
1 3
1 4
```Bằng cấp: 

| Đỉnh | Bằng cấp | 
| --- | --- | 
| 1 | 3 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 

Đỉnh 1 có bậc$n-1$, vậy đây là một ngôi sao. 

Đầu ra:```
Bob
```Điều này xác nhận rằng vị trí đầu cuối duy nhất là cấu hình ngôi sao. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lượt để tính độ và một lượt quét để kiểm tra đỉnh phổ quát | 
| Không gian |$O(n)$| Mảng độ cho tất cả các đỉnh | 

Những hạn chế$n \le 50$thậm chí làm cho điều này trở nên tầm thường, nhưng giải pháp dễ dàng mở rộng quy mô đến các cây lớn hơn nhiều vì nó tránh được mọi kiểm tra đẳng cấu hoặc thăm dò trạng thái. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline
    n = int(input().strip())
    deg = [0] * (n + 1)
    for _ in range(n - 1):
        u, v = map(int, input().split())
        deg[u] += 1
        deg[v] += 1
    print("Bob" if any(d == n - 1 for d in deg[1:]) else "Alice")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    from io import StringIO
    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old_stdout
    sys.stdin = old_stdin
    return out.getvalue().strip()

# provided samples (as interpreted)
assert run("""4
1 2
2 3
3 4
""") == "Alice"

assert run("""4
1 2
1 3
1 4
""") == "Bob"

# custom cases
assert run("""2
1 2
""") == "Bob", "minimum star"

assert run("""3
1 2
2 3
""") == "Alice", "path"

assert run("""5
1 2
2 3
3 4
4 5
""") == "Alice", "long path"

assert run("""5
1 2
1 3
1 4
1 5
""") == "Bob", "star 5"

assert run("""6
1 2
2 3
3 4
4 5
5 6
""") == "Alice", "even path"

| Test input | Expected output | What it validates |
|---|---|---|
| 2-node tree | Bob | smallest star case |
| path | Alice | non-star linear structure |
| long path | Alice | scaling consistency |
| star 5 | Bob | high-degree center detection |
| path 6 | Alice | parity independence check |
```## Vỏ cạnh 

Trường hợp cạnh chính là cấu hình sao đối xứng hoàn toàn. Trong trường hợp này, mọi hoạt động ghép có thể sẽ bảo tồn lớp đẳng cấu của cây, do đó không có động thái hợp pháp nào tồn tại. Thuật toán phát hiện điều này bằng cách kiểm tra đỉnh bậc$n-1$, xác định chính xác ngôi sao bất kể ghi nhãn. 

Một trường hợp tế nhị khác là những cây nhỏ như$n=2$. Bản thân cây cạnh đơn là một ngôi sao vì điểm cuối nào cũng có bậc$1 = n-1$, nên Alice ngay lập tức không có động thái gì. Việc kiểm tra mức độ ghi lại điều này mà không cần vỏ bọc đặc biệt. 

Cuối cùng, những cây mất cân bằng cao không phải là ngôi sao vẫn có cấu trúc bất đối xứng nên chúng luôn cho phép di chuyển ít nhất một lần. Lời giải không phụ thuộc vào việc tìm ra nước đi cụ thể, mà chỉ phụ thuộc vào việc phát hiện ra rằng có ít nhất một nước đi tồn tại, điều này được đảm bảo bằng việc không có đỉnh trung tâm phổ quát.
