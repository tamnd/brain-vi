---
title: "CF 105535K - Biết thời gian lưu trú của bạn"
description: "Chúng ta được cung cấp một lịch không còn là cấu trúc một năm tiêu chuẩn nữa mà thay vào đó bao gồm một chuỗi các tháng, mỗi tháng có số ngày riêng. Một ngày được biểu diễn dưới dạng một cặp số nguyên: một ngày trong tháng và chỉ số tháng."
date: "2026-06-23T23:06:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105535
codeforces_index: "K"
codeforces_contest_name: "2024 ICPC Belarus Regional Contest"
rating: 0
weight: 105535
solve_time_s: 54
verified: true
draft: false
---

[CF 105535K - Biết thời gian lưu trú của bạn](https://codeforces.com/problemset/problem/105535/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lịch không còn là cấu trúc một năm tiêu chuẩn nữa mà thay vào đó bao gồm một chuỗi các tháng, mỗi tháng có số ngày riêng. Một ngày được biểu diễn dưới dạng một cặp số nguyên: một ngày trong tháng và chỉ số tháng. 

Mỗi truy vấn mô tả một chuyến đi: một khách hàng đến vào một ngày cụ thể trong tháng và rời đi vào một ngày khác trong tháng khác, với sự đảm bảo rằng ngày khởi hành không sớm hơn ngày đến theo chu kỳ của một năm. Nhiệm vụ là tính xem khách hàng ở lại bao nhiêu ngày theo lịch, tính cả ngày đến và ngày đi. 

Cấu trúc gợi ý rằng chúng ta đang làm việc bên trong một dòng thời gian tuyến tính duy nhất có độ dài bằng tổng độ dài của tất cả các tháng. Mỗi ngày có thể được ánh xạ tới một vị trí số nguyên duy nhất trong dòng thời gian này. Khi ánh xạ đó tồn tại, mỗi truy vấn sẽ trở thành một sự khác biệt đơn giản giữa hai vị trí cộng với một. 

Các ràng buộc đủ lớn để bất kỳ giải pháp nào cũng phải xử lý từng truy vấn trong thời gian không đổi sau khi xử lý trước. Tổng số tháng và truy vấn trên tất cả các trường hợp thử nghiệm tối đa là 100000 và tổng số ngày trong một năm có thể lên tới 10^9. Điều này loại trừ mọi mô phỏng cho mỗi truy vấn trong nhiều ngày hoặc nhiều tháng. 

Trường hợp cạnh tinh vi phát sinh từ tính chất bao hàm của khoảng. Nếu ngày đến và ngày đi là cùng ngày thì câu trả lời phải là 1 chứ không phải 0. Một nguy cơ tiềm ẩn khác là xử lý không chính xác ranh giới tháng khi chuyển đổi ngày thành chỉ mục tuyến tính. Ví dụ: nếu tháng là [3, 5] và truy vấn là (ngày thứ 3 của tháng 1) đến (ngày đầu tiên của tháng 2), câu trả lời đúng là 3, không phải 2 hoặc 1 tùy thuộc vào lỗi lập chỉ mục. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là mô phỏng các ngày trôi qua cho mỗi truy vấn. Bắt đầu từ ngày đến, chúng tôi tăng dần từng bước cho đến ngày khởi hành, đếm từng bước. Điều này đúng về mặt khái niệm, nhưng trong trường hợp xấu nhất, một truy vấn có thể kéo dài gần cả năm và có thể có tới 100000 truy vấn. Với kích thước năm có tổng chiều dài lên tới 10^9, điều này trở nên hoàn toàn không khả thi. 

Quan sát quan trọng là lịch là tĩnh. Khi chúng tôi chuyển đổi từng ngày thành vị trí tuyệt đối của nó trong một mảng ngày được làm phẳng, mọi truy vấn sẽ trở thành một sự khác biệt số học đơn giản. Đây là cài đặt tổng tiền tố cổ điển: chúng tôi tính toán trước chỉ số bắt đầu của mỗi tháng theo dòng thời gian được làm phẳng. Sau đó, bất kỳ ngày nào (d, m) ánh xạ tới tiền tố[m - 1] + d. 

Sau phép biến đổi này, mỗi câu trả lời truy vấn được tính toán trong thời gian O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(tổng số ngày cho mỗi truy vấn) | O(1) | Quá chậm | 
| Ánh xạ tổng tiền tố | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi lịch thành một mảng tổng tiền tố theo tháng, sau đó sử dụng nó để ánh xạ ngày thành dòng thời gian tuyến tính. 

1. Xây dựng một mảng tổng tiền tố trong đó prefix[i] lưu trữ tổng số ngày từ tháng 1 đến tháng i. Điều này cho phép chúng ta tính chỉ số ngày tuyệt đối của bất kỳ ngày nào trong thời gian không đổi. Lý do điều này có tác dụng là vì các tháng tạo thành một phân vùng liền kề trong năm. 
2. Đối với mỗi truy vấn, hãy chuyển đổi ngày đến (sd, sm) thành vị trí tuyệt đối bằng cách sử dụng tiền tố[sm - 1] + sd. Điều này đưa ra chỉ số dựa trên 1 của ngày trong cả năm. 
3. Đổi ngày khởi hành (ed, em) tương tự bằng cách sử dụng tiền tố[em - 1] + ed. Cả hai giá trị hiện nằm trên cùng một thang đo tuyến tính. 
4. Câu trả lời là sự khác biệt giữa hai vị trí này cộng một. Tài khoản +1 để tính cả hai điểm cuối. 

Tại sao nó hoạt động

Mảng tổng tiền tố xác định sự phân đôi giữa ngày theo lịch và vị trí số nguyên trong một chuỗi ngày liên tục. Mỗi tháng đóng góp một khối liền kề, do đó không tồn tại sự chồng chéo hoặc khoảng trống trong bản đồ. Vì cả hai điểm cuối đều được ánh xạ theo thứ tự toàn cầu nhất quán này nên khoảng cách giữa chúng trong mảng phẳng chính xác bằng số ngày giữa chúng trong lịch ban đầu, bao gồm cả cả hai điểm cuối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    pref = [0] * (n + 1)
    for i in range(1, n + 1):
        pref[i] = pref[i - 1] + a[i - 1]

    out = []
    for _ in range(m):
        sd, sm, ed, em = map(int, input().split())

        start = pref[sm - 1] + sd
        end = pref[em - 1] + ed

        out.append(str(end - start + 1))

    sys.stdout.write("\n".join(out) + "\n")

if __name__ == "__main__":
    solve()
```Việc xây dựng mảng tiền tố tích lũy độ dài tháng để mỗi tháng được chỉ định một khoảng chênh lệch bắt đầu cố định. Chi tiết chính là sử dụng pref[sm - 1], vì chỉ số tháng dựa trên đầu vào là 1 nhưng tiền tố lại dựa trên 0. Điều tương tự cũng áp dụng cho tháng khởi hành. 

Phép trừ kết thúc - bắt đầu + 1 thực thi tính bao gồm. Nếu không có +1, ngày bắt đầu và ngày kết thúc giống hệt nhau sẽ mang lại kết quả bằng 0. 

## Ví dụ đã hoạt động 

Hãy xem xét một lịch đơn giản với các tháng [3, 5]. 

Mảng tiền tố trở thành [0, 3, 8]. 

Truy vấn đầu tiên: (2, 1) đến (1, 2). 

| Bước | sm | sd | em | ed | bắt đầu | kết thúc | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 2 | 1 | 0 + 2 = 2 | 3 + 1 = 4 | 3 | 

Khoảng thời gian tương ứng với ngày từ 2 đến 4 trong mảng phẳng, do đó kết quả là 3. 

Điều này xác nhận rằng quá trình chuyển đổi giữa các tháng được xử lý rõ ràng bằng ánh xạ tiền tố. 

Bây giờ hãy xem xét trường hợp một tháng với [10], truy vấn (1, 1) đến (10, 1). 

| Bước | sm | sd | em | ed | bắt đầu | kết thúc | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 10 | 1 | 10 | 10 | 

Điều này cho thấy rằng trong vòng một tháng, công thức sẽ giảm chính xác thành sai phân đơn giản bên trong một khối liền kề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Việc xây dựng tiền tố là tuyến tính tính theo tháng, mỗi truy vấn là O(1) | 
| Không gian | O(n) | mảng tiền tố lưu trữ độ dài tháng tích lũy | 

Các ràng buộc cho phép tổng số tháng và truy vấn lên tới 100000, do đó, quá trình xử lý trước tuyến tính và xử lý truy vấn theo thời gian không đổi thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip()

# We assume solve() is defined above; in real use, you'd import it.

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# minimal case
assert run("""1 1
10
1 1 10 1
""") == "10"

# same day
assert run("""2 1
5 7
3 1 3 1
""") == "1"

# cross month
assert run("""2 2
3 5
2 1 1 2
1 2 5 2
""") == "3\n5"

# full year
assert run("""3 1
2 3 4
1 1 4 3
""") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| single month full span | 10 | inclusive counting |
 | same day query | 1 | off-by-one handling |
 | chuyển tiếp qua tháng | 3, 5 | prefix correctness |
 | full year query | 9 | tính chính xác của bản đồ toàn cầu | 

## Vỏ cạnh 

Trường hợp nghiêm trọng là khi cả hai ngày đều nằm trong cùng một tháng và thậm chí cùng một ngày. Ví dụ: trong lịch [5, 6], truy vấn (3, 2) đến (3, 2) ánh xạ cả điểm bắt đầu và kết thúc vào tiền tố[1] + 3, đưa ra các giá trị giống hệt nhau. Công thức kết thúc - bắt đầu + 1 trả về chính xác 1, phù hợp với yêu cầu thời gian lưu trú một ngày là hợp lệ. 

Một trường hợp quan trọng khác là vượt qua ranh giới tháng ở các cạnh chính xác. Trong [4, 7], đi từ (4, 1) đến (1, 2) tạo ra bắt đầu = 4 và kết thúc = 5. Câu trả lời trở thành 2, đếm chính xác ngày cuối cùng của tháng 1 và ngày đầu tiên của tháng 2. Bất kỳ lỗi sai sót nào trong việc lập chỉ mục tiền tố sẽ làm dịch chuyển điều này đi một và phá vỡ tính chính xác của ranh giới. 

Cuối cùng, khi truy vấn kéo dài cả năm, chẳng hạn như (1, 1) đến (a_n, n), cả hai điểm cuối đều ánh xạ rõ ràng tới 1 và tổng (a_i). Phép trừ tự nhiên mang lại độ dài cả năm mà không cần cách viết đặc biệt, điều này khẳng định tính chắc chắn của phương pháp tuyến tính hóa.
