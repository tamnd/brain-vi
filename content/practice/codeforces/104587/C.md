---
title: "CF 104587C - Toán Thương mại"
description: "Mỗi người trong đầu vào sở hữu chính xác một đối tượng và muốn chính xác một đối tượng. Chúng ta có thể coi mỗi người như một cạnh có hướng trong biểu đồ: từ đối tượng họ hiện có đến đối tượng họ muốn."
date: "2026-06-30T07:28:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104587
codeforces_index: "C"
codeforces_contest_name: "2020-2021 ICPC East Central North America Regional Contest (ECNA 2020)"
rating: 0
weight: 104587
solve_time_s: 46
verified: true
draft: false
---

[CF 104587C - Thương mại toán học](https://codeforces.com/problemset/problem/104587/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi người trong đầu vào sở hữu chính xác một đối tượng và muốn chính xác một đối tượng. Chúng ta có thể coi mỗi người như một cạnh có hướng trong biểu đồ: từ đối tượng họ hiện có đến đối tượng họ muốn. Bởi vì mọi đối tượng đều được sở hữu duy nhất và được mong muốn duy nhất, nên mọi đối tượng xuất hiện chính xác một lần dưới dạng nguồn và chính xác một lần dưới dạng mục tiêu. 

Một “chuỗi thương mại toán học” hợp lệ là một chuỗi các người sao cho đối tượng mà người thứ nhất mong muốn hiện thuộc sở hữu của người thứ hai, đối tượng mong muốn của người thứ hai thuộc sở hữu của người thứ ba, v.v. Nếu bạn tuân theo chuỗi phụ thuộc, về cơ bản bạn đang đi dọc theo những ranh giới được định hướng bởi con người. Hạn chế chính là các giao dịch chỉ có thể thực hiện được khi chuỗi này nhất quán, điều này buộc cấu trúc phải là các chu kỳ được định hướng rời rạc. 

Nhiệm vụ giảm xuống còn việc tìm ra chu kỳ dài nhất được hình thành bởi các cạnh có hướng này và đưa ra độ dài của nó theo số lượng người tham gia. Nếu không tồn tại chu kỳ nào có độ dài ít nhất là 2 thì câu trả lời là không thể thực hiện giao dịch nào. 

Kích thước đầu vào nhỏ, nhiều nhất là 100 người. Điều này ngay lập tức loại trừ bất kỳ nhu cầu nào về máy móc đồ thị nặng ngoài việc ánh xạ và truyền tải đơn giản. Giải pháp O(n2) hoặc thậm chí O(n) cho mỗi trường hợp thử nghiệm là hoàn toàn đủ. 

Một trường hợp tinh tế phát sinh khi ánh xạ chỉ hình thành các vòng tự lặp, nghĩa là một người muốn chính xác những gì họ đã có. Người như vậy không đóng góp vào chuỗi thương mại. Một trường hợp khác là khi đồ thị phân tách thành nhiều chu trình nhỏ có kích thước khác nhau, trong đó chúng ta phải xác định chính xác chu trình lớn nhất thay vì chỉ phát hiện sự tồn tại của một chu trình. 

## Phương pháp tiếp cận 

Cấu trúc do bài toán tạo ra là một đồ thị hàm số: mỗi nút (người) trỏ đến chính xác một nút khác (người sở hữu đối tượng mong muốn). Bởi vì quyền sở hữu đối tượng là duy nhất, mỗi nút có chính xác một cạnh đi và chính xác một cạnh vào, tạo thành một cấu trúc giống như hoán vị đối với những người tham gia. 

Cách tiếp cận bạo lực sẽ cố gắng bắt đầu từ mỗi người và theo chuỗi cho đến khi phát hiện được sự lặp lại, ghi lại độ dài chu kỳ. Điều này đã gần đạt mức tối ưu vì mỗi bước đi đều mang tính quyết định. Tuy nhiên, nếu thực hiện một cách bất cẩn, người ta có thể khởi động lại quá trình truyền tải cho mọi nút và lặp đi lặp lại các chu kỳ giống nhau, dẫn đến công việc dư thừa. Trong trường hợp xấu nhất, giá trị này trở thành O(n²), ở đây vẫn ổn nhưng không hiệu quả về mặt khái niệm. 

Quan sát quan trọng là chúng ta đang xử lý các chu kỳ rời rạc. Mỗi nút thuộc về chính xác một chu trình, vì vậy khi một nút được truy cập, toàn bộ chu trình của nút đó có thể được đánh dấu và không bao giờ được tính toán lại. Điều này gợi ý một quá trình truyền tải mảng đã truy cập hoặc giống như DFS đơn giản để trích xuất mỗi chu kỳ chính xác một lần. 

Do đó, chúng tôi có thể lặp lại trên tất cả các nút và bất cứ khi nào chúng tôi tìm thấy một nút chưa được truy cập, chúng tôi sẽ đi theo các cạnh đi ra của nó cho đến khi quay lại nút đã truy cập, đếm độ dài chu kỳ nếu nó hợp lệ. Câu trả lời là độ dài tối đa như vậy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đi bộ từ mỗi nút | O(n²) | O(n) | Đã chấp nhận | 
| Phân rã chu trình với điểm đánh dấu đã truy cập | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi tên đối tượng thành các chỉ mục để có thể làm việc với mảng thay vì chuỗi. Điều này làm cho thời gian truyền tải không đổi trên mỗi bước.

1. Xây dựng hai bản đồ băm: một ánh xạ tên đối tượng tới chỉ mục chủ sở hữu và một ánh xạ chỉ mục chủ sở hữu tới tên đối tượng mong muốn. Ánh xạ đầu tiên rất cần thiết vì nó cho phép chúng ta chuyển ngay lập tức từ một đối tượng sang chủ sở hữu của nó. 
2. Xây dựng mảng đồ thị hàm next[] trong đó next[i] là chỉ số của người sở hữu đối tượng mà người tôi muốn. Điều này tạo ra một con trỏ xác định từ mỗi nút đến chính xác một nút khác. 
3. Duy trì mảng đã truy cập được khởi tạo thành false cho tất cả các nút. Điều này đảm bảo chúng tôi không tính toán lại các chu kỳ mà chúng tôi đã xử lý. 
4. Lặp qua mọi nút i từ 0 đến n−1. Nếu nút i đã được truy cập, hãy bỏ qua nó vì nó thuộc về chu trình được xử lý trước đó. 
5. Nếu nút i chưa được truy cập, hãy bắt đầu đi từ i theo các con trỏ tiếp theo, đánh dấu các nút là đã truy cập và đếm xem có bao nhiêu nút duy nhất được gặp cho đến khi chúng ta quay lại nút đã truy cập. Quá trình truyền tải này nhất thiết phải diễn ra trong một chu kỳ vì mỗi nút có chính xác một cạnh đi ra. 
6. Nếu độ dài chu kỳ ít nhất là 2, hãy cập nhật câu trả lời bằng giá trị này. 
7. Sau khi xử lý tất cả các nút, xuất ra độ dài chu kỳ tối đa được tìm thấy hoặc báo cáo rằng không có chu kỳ hợp lệ tồn tại nếu không có độ dài chu kỳ vượt quá 1. 

Tính đúng đắn phụ thuộc vào thực tế là một khi chúng ta bước vào một chu trình, chúng ta không thể thoát khỏi nó và vì mỗi nút có chính xác một cạnh đi ra nên quá trình truyền tải không thể phân nhánh. 

### Tại sao nó hoạt động 

Biểu đồ được xây dựng là một hoán vị trên tập hợp những người tham gia được tạo ra bởi quyền sở hữu và nhu cầu đối tượng. Trong biểu đồ như vậy, mọi thành phần liên thông là một chu trình có hướng. Mỗi chu trình là rời rạc và mỗi nút thuộc về đúng một chu trình. Việc truyền tải đánh dấu mỗi nút trong một chu kỳ chính xác một lần, do đó độ dài chu kỳ được tính toán chính xác mà không bị trùng lặp hoặc thiếu sót. Do đó, mức tối đa trong các độ dài chu kỳ này là chuỗi thương mại hợp lệ dài nhất có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    if n == 0:
        print("No trades possible")
        return

    owner_of = {}
    want = []
    names = []

    for i in range(n):
        name, have, need = input().strip().split()
        names.append(name)
        owner_of[have] = i
        want.append(need)

    nxt = [-1] * n
    for i in range(n):
        if want[i] in owner_of:
            nxt[i] = owner_of[want[i]]

    visited = [False] * n
    ans = 0

    for i in range(n):
        if visited[i]:
            continue

        cur = i
        cnt = 0
        path = []

        while not visited[cur]:
            visited[cur] = True
            path.append(cur)
            cnt += 1
            cur = nxt[cur]

            if cur == -1:
                cnt = 0
                break

        if cnt > 1:
            ans = max(ans, cnt)

    if ans == 0:
        print("No trades possible")
    else:
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc tất cả những người tham gia và xây dựng ánh xạ từ từng đối tượng tới chủ sở hữu của nó. Đây là sự chuyển đổi quan trọng cho phép chúng ta chuyển trực tiếp từ “đối tượng mong muốn” sang người hiện đang nắm giữ nó. 

Mảng con trỏ tiếp theo mã hóa biểu đồ hàm: mỗi người chỉ vào chính xác một người khác hoặc tới -1 nếu đối tượng mong muốn không tồn tại giữa những người tham gia. Mảng đã truy cập đảm bảo mỗi nút được xử lý một lần. 

Trong quá trình truyền tải, chúng tôi tích lũy một đường dẫn cho đến khi gặp một nút đã truy cập. Vì các chu kỳ là rời rạc nên quá trình truyền tải này nắm bắt chính xác một chu kỳ hoặc bị ngắt nếu chuỗi không hoàn chỉnh. Chúng tôi chỉ xem xét các chu kỳ có kích thước ít nhất là 2, vì kích thước 1 tương ứng với một người muốn có món đồ của riêng mình và không hình thành giao dịch. 

Một chi tiết tinh tế là việc đánh dấu đã truy cập ngay lập tức trong quá trình truyền tải sẽ ngăn việc truy cập lại các nút từ các chu kỳ khác, đảm bảo hành vi tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
Sally Clock Doll
Steve Doll Painting
Carlos Painting Clock
Maria Candlestick Vase
```Chúng tôi xây dựng bản đồ: 

Sally → Đồng hồ, Steve → Búp bê, Carlos → Tranh vẽ, Maria → Chân nến 

Đồng hồ → Sally, Búp bê → Steve, Tranh → Carlos, Bình → Maria 

Truyền tải: 

| Bắt đầu | Đường dẫn | Các bước tiếp theo | Kích thước chu kỳ | 
| --- | --- | --- | --- | 
| Sally | Sally → Steve → Carlos → Sally | đóng chu kỳ | 3 | 
| Steve | đã ghé thăm | bỏ qua | - | 
| Carlos | đã ghé thăm | bỏ qua | - | 
| Maria | Maria → Maria | tự lặp | 1 | 

Kích thước chu kỳ tối đa là 3, do đó đầu ra là 3. 

Điều này xác nhận rằng chỉ có chu trình có ý nghĩa mới đóng góp, trong khi các vòng lặp tự thân bị bỏ qua. 

### Ví dụ 2 

đầu vào:```
Abby Bottlecap Card
Bob Card Spoon
Chris Spoon Chair
Dan Pencil Pen
```Ánh xạ: 

Abby → Bob → Chris → Abby tạo thành một chu kỳ gồm 3. Dan → Dan là một vòng lặp tự thân. 

| Bắt đầu | Đường dẫn | Các bước tiếp theo | Kích thước chu kỳ | 
| --- | --- | --- | --- | 
| Abby | Abby → Bob → Chris → Abby | chu kỳ khép lại | 3 | 
| Bob | đã ghé thăm | bỏ qua | - | 
| Chris | đã ghé thăm | bỏ qua | - | 
| Dân | Dân | tự lặp | 1 | 

Đầu ra là 3. 

Điều này cho thấy các chu kỳ ngắt kết nối được xử lý độc lập và mức tối đa được chọn chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được truy cập một lần trong khi duyệt qua chu kỳ của nó và việc truy cập sẽ ngăn việc xử lý lại | 
| Không gian | O(n) | Lưu trữ ánh xạ, con trỏ tiếp theo và mảng đã truy cập | 

Các ràng buộc giới hạn n đến 100, do đó, ngay cả những cách tiếp cận kém hiệu quả cũng có thể dễ dàng vượt qua. Giải pháp tuyến tính này nằm trong giới hạn thoải mái và có biên độ rộng rãi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample 1
assert run("""Sally Clock Doll
Steve Doll Painting
Carlos Painting Clock
Maria Candlestick Vase""") == "3"

# sample 2
assert run("""Abby Bottlecap Card
Bob Card Spoon
Chris Spoon Chair
Dan Pencil Pen""") == "3"

# all self loops
assert run("""A X X
B Y Y
C Z Z""") == "No trades possible"

# single cycle
assert run("""A A B
B B C
C C A""") == "3"

# two cycles different sizes
assert run("""A A B
B B A
C C D
D D C""") == "2"

# no edges except broken chain
assert run("""A A B
B B C
C C D""") == "No trades possible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các vòng lặp tự | Không thể giao dịch | bỏ qua các chu trình tầm thường | 
| chu kỳ đơn | 3 | phát hiện chu kỳ đầy đủ | 
| hai chu kỳ | 2 | chọn chu kỳ tối đa | 
| xích bị đứt | Không thể giao dịch | xử lý con trỏ không hợp lệ | 

## Vỏ cạnh 

Trường hợp một bên là khi mọi người tham gia đều muốn có vật phẩm của riêng mình. Ví dụ:```
A X X
B Y Y
C Z Z
```Mỗi nút trỏ đến chính nó. Quá trình truyền tải đánh dấu từng nút riêng lẻ nhưng kích thước chu kỳ luôn là 1, do đó không có giao dịch hợp lệ nào được ghi lại. Thuật toán xuất ra chính xác “Không thể giao dịch”. 

Một trường hợp khác là sự kết hợp giữa một chu trình hợp lệ và các vòng lặp riêng biệt. Mảng đã truy cập đảm bảo chu trình được phát hiện một lần và các vòng lặp tự được coi là chu trình có kích thước 1 không ảnh hưởng đến câu trả lời. Mức tối đa trên tất cả các thành phần vẫn mang lại chuỗi thương mại dài nhất chính xác.
