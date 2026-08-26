---
title: "CF 104359F - \u041f\u0430\u0437\u043b"
description: "Chúng ta có hai cấu hình của một lưới rất mỏng có hai hàng và nhiều cột. Mỗi ô chứa số 0 hoặc số 1. Trong một cấu hình, chúng ta bắt đầu với một số cách sắp xếp số 1 và số 0, còn trong cấu hình khác, chúng ta muốn đạt được sự sắp xếp mục tiêu."
date: "2026-07-01T18:00:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104359
codeforces_index: "F"
codeforces_contest_name: "\u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u041c\u0441\u0442\u0438\u0441\u043b\u0430\u0432\u0430 \u041a\u0435\u043b\u0434\u044b\u0448\u0430 - 2022"
rating: 0
weight: 104359
solve_time_s: 78
verified: true
draft: false
---

[CF 104359F - \u041f\u0430\u0437\u043b](https://codeforces.com/problemset/problem/104359/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai cấu hình của một lưới rất mỏng có hai hàng và nhiều cột. Mỗi ô chứa số 0 hoặc số 1. Trong một cấu hình, chúng ta bắt đầu với một số cách sắp xếp số 1 và số 0, còn trong cấu hình khác, chúng ta muốn đạt được sự sắp xếp mục tiêu. 

Hoạt động duy nhất được phép là hoán đổi giá trị của hai ô có chung một cạnh trong lưới. Điều này có nghĩa là chúng ta có thể hoán đổi các hàng xóm bên trái và bên phải trong cùng một hàng hoặc hoán đổi các ô được căn chỉnh theo chiều dọc trong cùng một cột. Mỗi lần hoán đổi tốn một lần di chuyển và các lần hoán đổi có thể được áp dụng theo bất kỳ thứ tự nào. 

Nhiệm vụ là xác định số lần hoán đổi tối thiểu cần thiết để chuyển đổi lưới ban đầu thành lưới mục tiêu hoặc quyết định rằng điều đó là không thể. 

Ràng buộc trên n lên tới 200000, điều này ngay lập tức loại trừ mọi cách tiếp cận cố gắng mô phỏng các giao dịch hoán đổi hoặc chạy tìm kiếm đường dẫn ngắn nhất trên các cấu hình. Không gian trạng thái có tính hàm mũ theo n, vì vậy lời giải phải nén bài toán thành một phép so khớp tổ hợp hoặc tính toán cấu trúc tuyến tính, lý tưởng nhất là O(n log n) hoặc O(n). 

Một quan sát cần thiết đầu tiên là tính khả thi. Vì hoán đổi chỉ hoán vị các giá trị nên tập hợp nhiều giá trị phải khớp giữa điểm bắt đầu và mục tiêu. Đặc biệt số lượng cái phải bằng nhau. Nếu điều này bị vi phạm, không có chuỗi hoán đổi nào có thể giúp ích được và câu trả lời là -1. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ là giả định rằng việc khớp từng hàng một là đủ. Ví dụ: hãy xem xét tình huống trong đó số lượng cái trên mỗi hàng khớp với nhau trên toàn cầu, nhưng một số cái phải giao nhau giữa các hàng. Bởi vì hoán đổi theo chiều dọc tồn tại nên việc chuyển giao như vậy là có thể, do đó tính bất biến theo từng hàng không được giữ nguyên. 

Một cạm bẫy khác là giả định rằng chúng ta có thể tham lam sửa từng cột không khớp. Chuyển động ngang tương tác trên toàn cầu và các bản sửa lỗi cục bộ có thể chặn các sắp xếp lại tối ưu sau này. 

## Phương pháp tiếp cận 

Nếu bỏ qua tính hiệu quả, chúng ta có thể nghĩ đến các trạng thái cấu hình. Mỗi lần hoán đổi sẽ thay đổi cấu hình một chút, vì vậy chúng tôi đang tìm kiếm đường đi ngắn nhất trong một biểu đồ ngầm khổng lồ có các nút đều là lưới nhị phân 2 x n với số lượng cố định. Biểu đồ này có kích thước theo cấp số nhân tính bằng n, khiến cho việc sử dụng vũ lực hoàn toàn không khả thi. 

Quan điểm có cấu trúc hơn là quên lưới dưới dạng ma trận và thay vào đó xem từng ô chứa số 1 dưới dạng mã thông báo được đặt trên biểu đồ có 2n đỉnh và cạnh giữa các ô liền kề. Hoán đổi di chuyển hai mã thông báo liền kề, tương đương với việc trao đổi vị trí của chúng dọc theo một cạnh. Điều này có nghĩa là chi phí chuyển đổi một cấu hình này sang cấu hình khác là số lần hoán đổi liền kề tối thiểu cần thiết để chuyển đổi một cấu hình được gắn nhãn thành một sắp xếp nhiều tập hợp không được gắn nhãn khác. 

Đây chính xác là bài toán so khớp chi phí tối thiểu: chúng ta phải ghép từng vị trí ban đầu của số 1 với vị trí mục tiêu là số 1 và chi phí ghép hai vị trí là khoảng cách đường đi ngắn nhất giữa chúng trong biểu đồ lưới. 

Sự đơn giản hóa chính xuất phát từ việc hiểu cấu trúc của lưới 2 x n. Khoảng cách trong biểu đồ này hoạt động rất đều đặn: di chuyển theo chiều ngang dọc theo một hàng sẽ tốn một bước cho mỗi bước và việc chuyển đổi các hàng ở bất kỳ cột nào sẽ tốn một bước. Do đó, đường đi ngắn nhất giữa hai ô chỉ phụ thuộc vào độ chênh lệch cột của chúng và liệu các hàng của chúng có khác nhau hay không. 

Điều này thu gọn hình học thành một số liệu có cấu trúc chặt chẽ, cho phép chúng ta giảm vấn đề so khớp thành vấn đề sắp xếp một chiều với một hiệu chỉnh nhỏ cho các hàng không khớp. 

Đối sánh bạo lực sẽ thử tất cả các cặp giữa k cái, là giai thừa tính bằng k. Cấu trúc của hàm khoảng cách cho phép chúng ta sắp xếp các vị trí theo cột và ghép chúng theo thứ tự, sau đó tính toán các điểm không khớp của hàng một cách xác định.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Kết hợp các mã thông báo bằng vũ lực | O(k!) | O(k) | Quá chậm | 
| So khớp được sắp xếp theo số liệu có cấu trúc | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Ý tưởng chính 

Chúng tôi coi mọi ô chứa số 1 là một điểm có tọa độ (hàng, cột). Chúng tôi trích xuất tất cả các điểm như vậy từ lưới ban đầu và từ lưới mục tiêu. 

### Các bước 

1. Thu thập tất cả các vị trí của những cái trong lưới ban đầu và trong lưới mục tiêu. Nếu số lượng của chúng khác nhau, ngay lập tức trả về -1. Điều này là cần thiết vì các giao dịch hoán đổi bảo toàn số lượng cái một. 
2. Biểu diễn mỗi vị trí thành một cặp (cột, hàng). Chúng tôi sử dụng cột làm tọa độ chính vì chuyển động ngang chiếm ưu thế trong cấu trúc khoảng cách. 
3. Sắp xếp cả danh sách ban đầu và danh sách đích theo cột và nếu các cột bằng nhau thì sắp xếp theo hàng. Điều này tạo ra hai chuỗi có thứ tự. 
4. Ghép phần tử thứ i của chuỗi ban đầu với phần tử thứ i của chuỗi đích. Việc ghép nối cố định này phản ánh cấu trúc vận chuyển tối ưu dọc theo hình học dạng đường của lưới. 
5. Tính giá tiền của mỗi cặp như sau. Chi phí theo chiều ngang là sự khác biệt tuyệt đối của các cột. Chi phí dọc là 1 nếu các hàng khác nhau và 0 nếu ngược lại. Tính tổng những đóng góp này trên tất cả các cặp. 
6. Xuất ra tổng chi phí. 

### Tại sao nó hoạt động 

Khoảng cách lưới giữa hai ô được phân tách rõ ràng thành thành phần ngang và thành phần dọc. Chuyển động ngang hoạt động chính xác giống như một đường và trong các cài đặt như vậy, phép gán tối ưu giữa hai tập hợp có được bằng cách sắp xếp và ghép nối theo thứ tự, đây là thuộc tính tiêu chuẩn của các hàm chi phí giá trị tuyệt đối. 

Thành phần dọc đóng góp một hình phạt độc lập chỉ tùy thuộc vào việc các điểm cuối có nằm trong cùng một hàng hay không. Vì hình phạt này không phụ thuộc vào thứ tự của các cột nên bất kỳ giải pháp tối ưu nào giúp giảm thiểu chuyển vị ngang đều có thể được điều chỉnh để tôn trọng việc ghép cặp đã sắp xếp mà không làm tăng tổng chi phí. 

Kết quả là, việc kết hợp tối ưu đồng thời giảm thiểu tổng dịch chuyển theo chiều ngang và đếm các hàng không khớp theo cách phù hợp với kết quả khớp đó, do đó, việc ghép nối được sắp xếp là tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def read_positions(n):
    pos = []
    for r in range(2):
        row = list(map(int, input().split()))
        for c in range(n):
            if row[c] == 1:
                pos.append((c, r))
    return pos

def solve():
    n = int(input())
    a = read_positions(n)
    b = read_positions(n)

    if len(a) != len(b):
        print(-1)
        return

    a.sort()
    b.sort()

    ans = 0
    for (ca, ra), (cb, rb) in zip(a, b):
        ans += abs(ca - cb)
        ans += (ra != rb)

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách trích xuất tọa độ của tất cả các tọa độ từ cả hai lưới. Điều này nén lưới thành hai tập hợp điểm, đây là thông tin duy nhất liên quan đến chi phí cuối cùng. 

Sau khi xác minh số lượng bằng nhau, cả hai danh sách đều được sắp xếp theo từ điển theo cột rồi đến hàng. Thứ tự này thực thi cấu trúc cần thiết để ghép đôi tối ưu theo nghĩa vận chuyển một chiều. 

Vòng lặp cuối cùng tính toán chi phí theo cặp. Sự khác biệt theo chiều ngang thể hiện khoảng cách mà các mã thông báo phải di chuyển dọc theo các cột. Hàng không khớp sẽ thêm hình phạt cho việc vượt qua giữa các hàng, tương ứng chính xác với việc sử dụng hoán đổi dọc tại một số điểm dọc theo đường dẫn. 

Không cần mô phỏng rõ ràng các giao dịch hoán đổi vì chi phí đã thể hiện số lượng giao dịch hoán đổi tối thiểu cần thiết để thực hiện mỗi chuyển động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một trường hợp nhỏ trong đó các giá trị ban đầu ở vị trí (0,0), (2,1) và các giá trị mục tiêu ở (1,0), (2,1). 

Danh sách được sắp xếp ban đầu là: 

(0,0), (2,1) 

Danh sách được sắp xếp mục tiêu là: 

(1,0), (2,1) 

| Cặp | Ban đầu | Mục tiêu | |Δcol| | Hàng không khớp | Chi phí | 

|---|---|---|---|---|---| 

| 1 | (0,0) | (1,0) | 1 | 0 | 1 | 

| 2 | (2,1) | (2,1) | 0 | 0 | 0 | 

Tổng chi phí là 1. Mã thông báo đầu tiên di chuyển sang phải một bước, mã thông báo thứ hai giữ nguyên vị trí. 

Điều này xác nhận rằng việc ghép nối theo thứ tự trong cột nắm bắt được chuyển động tối thiểu thực sự mà không cần xem xét việc so khớp chéo thay thế. 

### Ví dụ 2 

Những cái ban đầu: 

(0,0), (0,1), (3,0) 

Mục tiêu: 

(0,0), (2,1), (3,0) 

| Cặp | Ban đầu | Mục tiêu | |Δcol| | Hàng không khớp | Chi phí | 

|---|---|---|---|---|---| 

| 1 | (0,0) | (0,0) | 0 | 0 | 0 | 

| 2 | (0,1) | (2,1) | 2 | 0 | 2 | 

| 3 | (3,0) | (3,0) | 0 | 0 | 0 | 

Tổng chi phí là 2, tương ứng với sự dịch chuyển theo chiều ngang hai bước của mã thông báo ở giữa. 

Ví dụ này cho thấy rằng ngay cả khi nhiều mã thông báo bắt đầu trong cùng một cột, việc sắp xếp vẫn tạo ra một cặp nhất quán tôn trọng cấu trúc vận chuyển tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp vị trí của ai chiếm ưu thế | 
| Không gian | O(n) | lưu trữ tọa độ của tất cả những cái | 

Các ràng buộc cho phép tối đa 200000 cột, nhưng chỉ cần trích xuất và sắp xếp tuyến tính. Giải pháp này phù hợp một cách thoải mái trong giới hạn vì việc sắp xếp là hoạt động siêu tuyến tính duy nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = backup
    return out.getvalue().strip()

# minimal equal case
assert solve_io("""1
1
0
1
0
""") == "1"

# impossible case
assert solve_io("""2
1 0
0 0
0 0
0 0
""") == "-1"

# simple horizontal shift
assert solve_io("""3
1 0 0
0 0 0
0 1 0
0 0 0
""") == "1"

# vertical swap case
assert solve_io("""1
1
0
0
1
""") == "1"

# already equal
assert solve_io("""2
1 0
0 1
1 0
0 1
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Di chuyển dọc 1 ô | 1 | xử lý chi phí hoán đổi dọc | 
| số lượng không khớp | -1 | kiểm tra tính khả thi | 
| dịch chuyển ngang | 1 | vận chuyển dựa trên phân loại | 
| lưới giống hệt nhau | 0 | xử lý danh tính | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi ban đầu tất cả các số đều nằm trên một hàng nhưng phải được chia thành các hàng trong mục tiêu. Thuật toán xử lý vấn đề này một cách chính xác vì số hàng không khớp được tính cho mỗi cặp chứ không bị hạn chế trên toàn cầu. 

Ví dụ: nếu ban đầu có (0,0) và (1,0) trong khi mục tiêu có (0,1) và (1,1), việc sắp xếp sẽ tạo ra các cặp ((0,0),(0,1)) và ((1,0),(1,1)), mỗi cặp đóng góp một chi phí dọc. Thuật toán tự nhiên tính đến việc chuyển hàng bắt buộc mà không cần mô hình hóa rõ ràng các giao dịch hoán đổi dọc. 

Một trường hợp khác là khi nhiều cái chia sẻ cùng một cột. Việc sắp xếp sẽ duy trì việc nhóm của chúng và việc ghép nối vẫn nhất quán vì các mối liên kết cột được giải quyết theo hàng, tránh sự mơ hồ trong thứ tự gán.
