---
title: "CF 105129G - Hệ thống thưởng"
description: "Chúng tôi đang mô phỏng một hệ thống quản lý đào tạo nhỏ để theo dõi học viên và điểm thưởng tích lũy của họ. Mỗi học sinh có một danh tính cố định được đánh số từ 1 đến n và một tên đi kèm. Ban đầu, tất cả học sinh đều bắt đầu với số điểm bằng 0."
date: "2026-06-27T19:21:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105129
codeforces_index: "G"
codeforces_contest_name: "Shorouk Academy 2024 Collegiate Programming Contest"
rating: 0
weight: 105129
solve_time_s: 45
verified: true
draft: false
---

[CF 105129G - Hệ thống thưởng](https://codeforces.com/problemset/problem/105129/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một hệ thống quản lý đào tạo nhỏ để theo dõi học viên và điểm thưởng tích lũy của họ. Mỗi học sinh có một danh tính cố định được đánh số từ 1 đến n và một tên đi kèm. Ban đầu, tất cả học sinh đều bắt đầu với số điểm bằng 0. 

Sau khi khởi tạo, chúng tôi xử lý một chuỗi lệnh. Có hai loại lệnh. Một lệnh sẽ thêm điểm thưởng cho một học sinh cụ thể, nhưng chỉ khi mật khẩu nhất định khớp chính xác với mật khẩu hệ thống. Lệnh còn lại in thứ hạng hiện tại của học sinh dựa trên điểm tích lũy của họ. 

Quy tắc xếp hạng rất đơn giản: học sinh được sắp xếp theo tổng điểm theo thứ tự giảm dần và nếu hai học sinh có cùng số điểm thì học sinh có id nhỏ hơn sẽ xuất hiện trước. Khi in bảng điểm, chúng tôi chỉ hiển thị những học sinh hiện có điểm khác 0 và xếp hạng theo thứ tự sắp xếp này. Điểm bằng nhau chia sẻ cùng một thứ hạng. 

Các ràng buộc là nhỏ, tối đa là 100 sinh viên và 100 thao tác. Điều này ngay lập tức gợi ý rằng cách tiếp cận O(n log n) hoặc thậm chí O(n²) cho mỗi truy vấn là hoàn toàn đủ. Nút thắt cổ chai không phải là độ phức tạp của thuật toán mà là cách xử lý chính xác thứ tự, ràng buộc và lọc. 

Điểm tinh tế chính là việc xếp hạng được tính toán lại theo yêu cầu từ trạng thái hiện tại, không được duy trì tăng dần. Một vấn đề tế nhị khác là việc kiểm tra mật khẩu phải chính xác và phân biệt chữ hoa chữ thường; ngay cả một sự không phù hợp nhỏ cũng sẽ từ chối cập nhật. 

Các trường hợp đặc biệt xuất hiện trong cách chúng tôi tính toán thứ hạng. Nếu chúng ta ngây thơ gán thứ hạng là 1, 2, 3 theo thứ tự sắp xếp, chúng ta sẽ tạo ra kết quả sai khi điểm số bằng nhau. Ví dụ: nếu hai học sinh đều có 10 điểm thì phải có cùng thứ hạng. 

Một trường hợp khó khăn khác là xử lý những học sinh có điểm 0. Chúng không bao giờ được xuất hiện trong đầu ra của bảng điểm ngay cả khi chúng tồn tại trong hệ thống. 

Cuối cùng, việc xác định thứ tự theo id là rất quan trọng. Nếu không có nó, việc sắp xếp sẽ trở nên không ổn định và kết quả có thể khác nhau tùy theo quá trình triển khai. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là duy trì một mảng có kích thước n để lưu trữ điểm hiện tại của mỗi học sinh. Đối với lệnh thưởng, chúng tôi kiểm tra mật khẩu; nếu trùng khớp, chúng tôi sẽ cập nhật điểm theo thời gian liên tục. Nút thắt phức tạp xuất hiện khi chúng ta thực hiện lệnh bảng điểm: chúng ta phải sắp xếp tất cả học sinh theo điểm và id, sau đó tính thứ hạng và in những thứ khác 0. 

Chiến lược vũ phu này đã phù hợp một cách thoải mái. Việc sắp xếp n 100 phần tử tốn O(n log n) và ngay cả khi chúng tôi thực hiện việc đó cho mọi truy vấn q 100 thì tổng công việc là không đáng kể. 

Quan sát chính là không cần cấu trúc dữ liệu phức tạp như cây hoặc đống cân bằng. Tập dữ liệu đủ nhỏ để việc tính toán lại thứ tự từ đầu sẽ đơn giản hơn và ít xảy ra lỗi hơn so với việc duy trì theo từng bước. Mỗi truy vấn bảng điểm đều độc lập nên chúng tôi có thể xây dựng lại chế độ xem được sắp xếp theo yêu cầu. 

Quyết định thiết kế thực sự duy nhất là làm thế nào để phân công thứ hạng một cách chính xác. Sau khi được sắp xếp, chúng tôi quét danh sách và gán cùng một thứ hạng cho các điểm bằng nhau, nếu không thì sẽ tăng bộ đếm thứ hạng một cách thích hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tính toán lại sắp xếp từng truy vấn) | O(q · n log n) | O(n) | Đã chấp nhận | 
| Tối ưu (giống như lực lượng vũ phu do hạn chế) | O(q · n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai mảng: một mảng cho tên học sinh và một mảng cho điểm số hiện tại của họ, cả hai đều được lập chỉ mục theo id sinh viên.

1. Đọc n và mật khẩu hệ thống, sau đó đọc tất cả tên học sinh. Chúng tôi lưu trữ tên trong một mảng để có thể truy cập trực tiếp vào tên của học sinh theo id. 
2. Khởi tạo một mảng điểm có kích thước n với các số 0, vì chưa có phần thưởng nào được trao. 
3. Đối với mỗi lệnh, chúng tôi phân tích loại của nó. Nếu đó là lệnh thưởng, chúng tôi sẽ trích xuất id mục tiêu, giá trị thưởng và chuỗi mật khẩu. 
4. Chúng tôi so sánh mật khẩu được cung cấp với mật khẩu được lưu trữ. Nếu nó không khớp chính xác, chúng tôi sẽ in thông báo từ chối và bỏ qua mọi cập nhật. 
5. Nếu mật khẩu trùng khớp, chúng tôi sẽ cộng điểm thưởng vào điểm của học sinh và in thông báo thành công. 
6. Nếu lệnh là bảng điểm, chúng ta sẽ tạo danh sách tất cả học sinh kèm theo id, tên và điểm của họ. 
7. Chúng tôi sắp xếp danh sách này bằng cách giảm điểm và sau đó tăng id. Thứ tự này đảm bảo thứ hạng xác định ngay cả khi điểm bằng nhau. 
8. Chúng tôi lọc ra tất cả học sinh có điểm bằng 0 vì họ sẽ không xuất hiện trên bảng điểm. 
9. Chúng tôi chỉ định thứ hạng trong khi quét danh sách đã sắp xếp. Học sinh đầu tiên được xếp hạng 1. Đối với mỗi học sinh tiếp theo, nếu điểm của họ bằng điểm của học sinh trước thì họ sẽ nhận được cùng thứ hạng; nếu không, thứ hạng của chúng sẽ trở thành vị trí chỉ mục hiện tại trong số các mục được hiển thị. 
10. Chúng tôi in mỗi mục dưới dạng "điểm tên id xếp hạng". 

Sau các bước này, trạng thái hệ thống sẽ nhất quán và sẵn sàng cho lệnh tiếp theo. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, mảng điểm biểu thị chính xác tổng của tất cả các hoạt động thưởng được chấp nhận. Bảng điểm luôn được lấy trực tiếp từ mảng này, do đó việc sắp xếp sẽ tạo ra thứ tự chung chính xác. Vì việc xếp hạng chỉ phụ thuộc vào sự bằng nhau của các điểm được sắp xếp liền kề nên việc chia sẻ thứ hạng cho các giá trị bằng nhau sẽ duy trì tính chính xác theo định nghĩa được yêu cầu. Việc lọc các số 0 không ảnh hưởng đến thứ tự vì tất cả các phần tử bị loại trừ sẽ không ảnh hưởng đến thứ hạng trong số điểm tích cực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, password = input().split()
    n = int(n)

    names = [""] * (n + 1)
    for i in range(1, n + 1):
        names[i] = input().strip()

    score = [0] * (n + 1)

    q = int(input())
    out = []

    for _ in range(q):
        parts = input().split()

        if parts[0] == "bonus":
            idx = int(parts[1])
            val = int(parts[2])
            pw = parts[3]

            if pw == password:
                score[idx] += val
                out.append("Updated successfully")
            else:
                out.append("Wrong password please try again")

        else:
            arr = []
            for i in range(1, n + 1):
                if score[i] > 0:
                    arr.append((score[i], i, names[i]))

            arr.sort(key=lambda x: (-x[0], x[1]))

            rank_out = []
            current_rank = 0
            prev_score = None
            seen = 0

            for s, idx, name in arr:
                seen += 1
                if prev_score is None or s != prev_score:
                    current_rank = seen
                prev_score = s
                rank_out.append(f"{current_rank} {idx} {name} {s}")

            if rank_out:
                out.extend(rank_out)

        out.append("---")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp lưu trữ tên và điểm số trong các mảng dựa trên chỉ mục trực tiếp để truy cập O(1). Đối với mỗi lệnh thưởng, nó thực hiện kiểm tra mật khẩu liên tục và cập nhật điểm nếu hợp lệ. Đối với các lệnh bảng điểm, nó sẽ xây dựng lại danh sách các học sinh đang hoạt động, sắp xếp chúng bằng cách sử dụng khóa tuple để thực thi cả mức độ ưu tiên điểm số và liên kết id, sau đó chỉ định thứ hạng bằng cách theo dõi khi điểm thay đổi. 

Một điểm tinh tế là việc gán thứ hạng: chúng tôi sử dụng bộ đếm đang chạy`seen`để thể hiện vị trí trong danh sách đã lọc được sắp xếp. Bất cứ khi nào điểm số thay đổi, thứ hạng sẽ được đặt ở vị trí này; nếu không, nó sẽ được sử dụng lại. Điều này đảm bảo điểm bằng nhau chia sẻ các giá trị xếp hạng giống hệt nhau. 

Dòng phân cách được thêm vào sau mỗi lệnh theo yêu cầu, bao gồm cả sau kết quả đầu ra của bảng điểm. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi mẫu đầu tiên ở dạng nhỏ gọn tập trung vào những thay đổi về điểm số và kết quả đầu ra của bảng điểm. 

Trạng thái ban đầu có tất cả điểm bằng 0. 

| Bước | Lệnh | Điểm (xem một phần) | Hành động | 
| --- | --- | --- | --- | 
| 1 | thưởng 3 12 ShAC | 3:12 | cập nhật thành công | 
| 2 | thưởng 1 8 ShAC | 1:8, 3:12 | cập nhật thành công | 
| 3 | thưởng 3 80 ShaC | không thay đổi | sai mật khẩu | 
| 4 | thưởng 6 12 ShAC | 1:8, 3:12, 6:12 | cập nhật thành công | 

Bây giờ sắp xếp bảng điểm (3:12, 6:12, 1:8). Vì 3 và 6 hòa nhau nên họ chia nhau hạng 1. 

| Xếp hạng | ID | Điểm | 
| --- | --- | --- | 
| 1 | 3 | 12 | 
| 1 | 6 | 12 | 
| 3 | 1 | 8 | 

Điều này cho thấy cả việc xử lý ràng buộc và sắp xếp dựa trên id giữa các điểm bằng nhau (id 3 < 6 được giữ nguyên). 

Mẫu thứ hai thực hiện các thao tác cập nhật lặp lại và nhiều truy vấn về bảng điểm. Nó xác nhận rằng việc tính toán lại mỗi lần vẫn nhất quán, vì không cần trạng thái nào khác ngoài mảng điểm số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q · n log n) | Mỗi lần xây dựng lại bảng điểm sẽ sắp xếp tối đa n học sinh, lặp lại q lần | 
| Không gian | O(n) | Lưu trữ tên và điểm số | 

Cho n, q ≤ 100, các phép toán tối đa theo thứ tự 10⁴ các phép toán sắp xếp trên tối đa 100 phần tử, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins

    # assume solve() is defined above
    solve()
    return sys.stdout.getvalue().strip()

# sample-like minimal case
assert run("""1 A
Alice
3
bonus 1 10 A
scoreboard
scoreboard
""").count("Alice") >= 1

# wrong password case
assert "Wrong password" in run("""2 X
A
B
2
bonus 1 5 Y
scoreboard
""")

# tie ranking case
assert "1 1" in run("""2 P
A
B
2
bonus 1 5 P
bonus 2 5 P
scoreboard
""")

# zero filtering case
assert "A" not in run("""1 P
A
1
scoreboard
""")

# ordering by id tie-break
assert run("""2 P
A
B
2
bonus 2 5 P
bonus 1 5 P
scoreboard
""").index("1 1") < run("""2 P
A
B
2
bonus 2 5 P
bonus 1 5 P
scoreboard
""").index("1 2")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sinh viên độc thân tối thiểu | bảng điểm chỉ hiển thị học sinh đó | độ đúng cơ sở | 
| sai mật khẩu | tin nhắn từ chối | logic xác thực | 
| tỷ số hòa | phân công cùng hạng | xếp hạng đúng đắn | 
| không có điểm | bảng điểm trống | lọc bằng không | 
| đặt hàng id | đặt hàng nhất quán | luật tie-break | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả học sinh không có điểm. Trong trường hợp này, lệnh bảng điểm không được tạo ra dòng xếp hạng nào cả mà chỉ tạo ra các dấu phân cách. Thuật toán xử lý việc này vì chúng tôi lọc rõ ràng`score[i] > 0`trước khi sắp xếp. 

Một trường hợp khác là điểm số của nhiều học sinh được lặp lại bằng nhau. Ví dụ: nếu tất cả học sinh đạt 10 điểm thì tất cả họ phải chia sẻ thứ hạng 1. Logic gán thứ hạng sẽ đặt thứ hạng dựa trên lần xuất hiện đầu tiên của một điểm mới, do đó, không có sự gia tăng nào xảy ra cho đến khi điểm thấp hơn xuất hiện. 

Mật khẩu không khớp trên các lệnh thưởng liên tiếp cũng rất quan trọng. Vì chúng tôi không sửa đổi trạng thái trừ khi mật khẩu khớp, các lần thử sai lặp đi lặp lại sẽ không thay đổi mảng điểm số và kết quả đầu ra của bảng điểm sau này chỉ phản ánh các cập nhật hợp lệ.
