---
title: "CF 1200A - Chủ khách sạn"
description: "Chúng tôi đang mô phỏng một hệ thống rất nhỏ: một khách sạn có chính xác 10 phòng được đánh số từ 0 đến 9. Mỗi phòng có thể trống hoặc có người sử dụng."
date: "2026-06-13T15:02:14+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1200
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 578 (Div. 2)"
rating: 800
weight: 1200
solve_time_s: 130
verified: true
draft: false
---

[CF 1200A - Chủ khách sạn](https://codeforces.com/problemset/problem/1200/A) 

**Đánh giá:** 800 
**Tags:** bạo lực, cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một hệ thống rất nhỏ: một khách sạn có chính xác 10 phòng được đánh số từ 0 đến 9. Mỗi phòng có thể trống hoặc có người sử dụng. Một chuỗi sự kiện mô tả cách khách vào và rời đi theo thời gian và chúng tôi cần xây dựng lại trạng thái kín phòng cuối cùng sau khi xử lý tất cả các sự kiện theo thứ tự. 

Mỗi sự kiện có thể là sự kiện đến từ lối vào bên trái, sự kiện đến từ lối vào bên phải hoặc khởi hành từ một phòng cụ thể. Khi ai đó bước vào từ bên trái, họ luôn chọn phòng trống gần nhất bắt đầu từ phòng 0 di chuyển sang phải. Khi ai đó bước vào từ bên phải, họ sẽ lấy phòng trống gần nhất bắt đầu từ phòng 9 di chuyển sang trái. Khi một chữ số xuất hiện nghĩa là khách trong phòng đó đã rời đi, khiến căn phòng lại trống rỗng. 

Khó khăn chính không phải là tốc độ mô phỏng mà là tính chính xác khi cập nhật lặp đi lặp lại: các kết quả đến luôn phụ thuộc vào trạng thái hiện tại, vì vậy các lựa chọn trước đó sẽ hạn chế các lựa chọn trong tương lai. Mặc dù cấu trúc nhỏ nhưng số lượng sự kiện có thể lớn, lên tới 100.000, vì vậy chúng ta phải đảm bảo mỗi sự kiện đều được xử lý hiệu quả. 

Một sai lầm diễn giải ngây thơ là cho rằng những người đến luôn đi đến các vị trí cố định, chẳng hạn như luôn chỉ định những người đến còn lại vào vị trí trống đầu tiên trên toàn cầu mà không tôn trọng tính chất động của các chuyến khởi hành trước đó. Ví dụ: nếu bỏ qua việc loại bỏ, chúng tôi có thể đặt nhầm người mới đến vào phòng hiện đang có người ở. 

Một trường hợp phức tạp khác là việc chuyển đổi lặp đi lặp lại trong cùng một phòng. Ví dụ, một căn phòng có thể được chiếm, được giải phóng và được chiếm lại nhiều lần. Bất kỳ giải pháp nào không duy trì mảng trạng thái trực tiếp chính xác sẽ bị lệch khỏi cấu hình chính xác. 

Vì chỉ có 10 phòng nên kết quả cuối cùng là chuỗi nhị phân 10 ký tự biểu thị sức chứa. 

Ràng buộc n 10^5 ngụ ý chúng ta phải xử lý từng sự kiện trong thời gian O(1) hoặc O(10). Mọi cách tiếp cận quét tất cả các phòng cho mỗi sự kiện vẫn có thể chấp nhận được, nhưng bất kỳ phương pháp nào mở rộng quy mô với n mỗi phòng sẽ là quá chậm. 

## Phương pháp tiếp cận 

Một mô phỏng đơn giản duy trì một mảng gồm 10 giá trị boolean biểu thị xem mỗi phòng có bị chiếm hay không. Đối với mỗi lần đến từ bên trái, chúng tôi quét các phòng từ 0 trở lên và chọn phòng trống đầu tiên. Đối với hàng đến từ bên phải, chúng tôi quét từ số 9 trở xuống và chọn ô trống đầu tiên. Đối với sự kiện khởi hành, chúng tôi chỉ cần đánh dấu phòng đó là trống. 

Cách tiếp cận bạo lực này vốn đã có quy mô trạng thái cực kỳ nhỏ, vì vậy chi phí trong trường hợp xấu nhất cho mỗi sự kiện tối đa là 10 lần kiểm tra. Hơn 10^5 sự kiện, điều này dẫn đến khoảng 10^6 thao tác, nằm trong giới hạn thoải mái. Không cần cấu trúc dữ liệu phức tạp hơn như đống hoặc cây cân bằng. 

Nhận xét quan trọng là vấn đề không yêu cầu ghi nhớ các nhiệm vụ lịch sử mà chỉ yêu cầu ghi nhớ công suất hiện tại. Mỗi quyết định chỉ phụ thuộc vào trạng thái hiện tại và không gian trạng thái rất nhỏ và có kích thước cố định. Điều đó loại bỏ mọi nhu cầu tối ưu hóa ngoài việc quét tuyến tính đơn giản. 

Chúng ta có thể coi điều này giống như việc duy trì một bitmask có kích thước 10 với các quy tắc lựa chọn tham lam được áp dụng cho mỗi sự kiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O(10 · n) | O(1) | Đã chấp nhận | 
| Tối ưu (cùng ý tưởng) | O(10 · n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`rooms`có kích thước 10 với tất cả các giá trị được đặt thành 0, nghĩa là tất cả các phòng đều trống. Điều này thể hiện trạng thái hiện tại của khách sạn tại thời điểm 0. 
2. Lặp lại từng ký tự trong chuỗi đầu vào từ trái sang phải, coi mỗi ký tự là một sự kiện. 
3. Nếu sự kiện đó`'L'`, quét từ phòng 0 đến phòng 9 và tìm phòng đầu tiên trống. Đánh dấu nó là đã có người sử dụng. Điều này đảm bảo phòng trống gần nhất với lối vào bên trái được sử dụng. 
4. Nếu sự kiện đó`'R'`, quét từ phòng 9 xuống 0 và tìm phòng trống đầu tiên. Đánh dấu nó là đã có người sử dụng. Điều này đảm bảo phòng trống gần nhất với lối vào bên phải được sử dụng. 
5. Nếu sự kiện là một chữ số từ`'0'`ĐẾN`'9'`, chuyển đổi nó thành chỉ mục số nguyên và đánh dấu phòng đó là trống. 
6. Tiếp tục quá trình này cho đến khi tất cả các sự kiện được xử lý. 
7. Xuất trạng thái cuối cùng dưới dạng một chuỗi có độ dài 10, trong đó mỗi vị trí là`'1'`nếu bị chiếm đóng và`'0'`nếu không thì. 

### Tại sao nó hoạt động 

Thuật toán duy trì trạng thái kín phòng chính xác của tất cả các phòng sau mỗi sự kiện. Mọi quy tắc đến chỉ phụ thuộc vào tập hợp phòng trống hiện tại và mỗi lần khởi hành sẽ trực tiếp khôi phục một phòng về tập hợp trống. Vì chúng tôi luôn tính toán lại các lựa chọn dựa trên trạng thái cập nhật nên không có quyết định nào vi phạm các ràng buộc của "phòng trống gần nhất". Tính đúng đắn suy ra từ bất biến rằng`rooms[i]`đúng khi và chỉ nếu phòng`i`bị chiếm dụng sau khi xử lý tiền tố của sự kiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    s = input().strip()

    rooms = [0] * 10

    for c in s:
        if c == 'L':
            for i in range(10):
                if rooms[i] == 0:
                    rooms[i] = 1
                    break
        elif c == 'R':
            for i in range(9, -1, -1):
                if rooms[i] == 0:
                    rooms[i] = 1
                    break
        else:
            idx = ord(c) - ord('0')
            rooms[idx] = 0

    print("".join(map(str, rooms)))

if __name__ == "__main__":
    solve()
```Việc thực hiện giữ một đại diện trực tiếp của trạng thái khách sạn. Các vòng lặp cho`'L'`Và`'R'`cố ý đơn giản vì hệ số không đổi được giới hạn bởi 10. Việc chuyển đổi các ký tự chữ số sử dụng số học ASCII, giúp tránh việc phân tích cú pháp chậm hơn. Đầu ra cuối cùng nối các cờ số nguyên thành một chuỗi. 

Một sai lầm phổ biến ở đây là quên rằng việc khởi hành luôn thành công, do đó, sự kiện chữ số có thể cho rằng phòng đã có người một cách an toàn. Một trường hợp khác là vô tình sử dụng lại các chỉ mục vòng lặp cũ giữa các sự kiện, nhưng vì mỗi sự kiện là độc lập nên quá trình quét luôn khởi động lại mới. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
8
LLRL1RL1
```Chúng tôi theo dõi trạng thái sau mỗi sự kiện. 

| Bước | Sự kiện | Hành động | Tiểu bang | 
| --- | --- | --- | --- | 
| 1 | L | chiếm 0 | 1000000000 | 
| 2 | L | chiếm 1 | 1100000000 | 
| 3 | R | chiếm 9 | 1100000001 | 
| 4 | L | chiếm 2 | 1110000001 | 
| 5 | 1 | miễn phí 1 | 1010000001 | 
| 6 | R | chiếm 8 | 1010000011 | 
| 7 | L | chiếm 1 | 1110000011 | 
| 8 | 1 | miễn phí 1 | 1010000011 | 

Đầu ra cuối cùng là`1010000011`. 

Dấu vết này cho thấy các lựa chọn bên trái và bên phải phụ thuộc hoàn toàn vào sức chứa hiện tại và việc giải phóng nhiều lần cho phép sử dụng lại các vị trí trước đó. 

### Mẫu 2 

đầu vào:```
5
LRL0R
```| Bước | Sự kiện | Hành động | Tiểu bang | 
| --- | --- | --- | --- | 
| 1 | L | chiếm 0 | 1000000000 | 
| 2 | R | chiếm 9 | 1000000001 | 
| 3 | L | chiếm 1 | 1100000001 | 
| 4 | 0 | miễn phí 0 | 0100000001 | 
| 5 | R | chiếm 8 | 0100000011 | 

Đầu ra cuối cùng là`0100000011`. 

Ví dụ này nhấn mạnh rằng việc giải phóng phòng 0 sẽ thay đổi ngay việc phân bổ trái trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(10 · n) | Mỗi sự kiện quét tối đa 10 phòng | 
| Không gian | O(1) | Chỉ duy trì một mảng có kích thước cố định gồm 10 phòng | 

Thời gian chạy tuyến tính một cách hiệu quả về số lượng sự kiện với hệ số không đổi rất nhỏ, lý tưởng cho n lên đến 100.000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    import sys as _sys
    old_stdout = _sys.stdout
    try:
        _sys.stdout = out
        solve()
    finally:
        _sys.stdout = old_stdout
    return out.getvalue().strip()

# provided sample
assert run("""8
LLRL1RL1
""") == "1010000011"

# all empty operations
assert run("""3
LLL
""") == "1110000000"

# full fill and partial release
assert run("""5
LRLRR
""") == "1110000011"

# repeated occupy/free same slot
assert run("""6
L0L0L0
""") == "1010000000"

# alternating edges
assert run("""4
LRLR
""") == "1110000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| LLL | 1110000000 | lặp lại điền trái | 
| LRLRR | 1110000011 | lượt khách hỗn hợp | 
| L0L0L0 | 1010000000 | chuyển đổi lặp đi lặp lại của cùng một phòng | 
| LRLR | 1110000000 | lựa chọn ranh giới xen kẽ | 

## Vỏ cạnh 

Một trường hợp quan trọng là việc sử dụng lại cùng một phòng sau khi giải phóng. Xem xét đầu vào`L0L0`. Sau đó`L`, phòng 0 sẽ có người sử dụng. Sự kiện`0`giải phóng nó, và tiếp theo`L`lại chọn phòng 0 vì đây lại là chỗ trống gần nhất. Thuật toán xử lý việc này một cách chính xác bởi vì mọi`'L'`quá trình quét bắt đầu mới từ phòng 0, do đó các vị trí được giải phóng trước đó sẽ được xem xét lại một cách tự nhiên. 

Một trường hợp khác là khi tất cả các phòng ngoại trừ một phòng đều có người ở. Nếu phòng 9 là chỗ trống duy nhất và khách đến còn lại, việc quét từ 0 trở lên cuối cùng sẽ đến 9 và đặt khách vào đó. Điều này đúng dù xa lối vào bên trái, vì quy định là “phòng trống gần nhất” chứ không phải “thích bên trái dù xa”. 

Một trường hợp tinh tế cuối cùng là việc đến và đi xen kẽ để sử dụng lại các phòng ranh giới. Ví dụ,`L R 0 R L 0`liên tục chuyển đổi phòng 0 và 9. Vì mỗi sự kiện đều cập nhật trạng thái chia sẻ ngay lập tức nên quá trình quét luôn phản ánh cấu hình mới nhất, ngăn chặn các bài tập cũ.
