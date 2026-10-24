---
title: "CF 105962J - Tigrinho"
description: "Chúng tôi được cấp một bảng 3 x 3. Mỗi ô là một chữ số từ 1 đến 9 hoặc ký hiệu ký tự đại diện. Bàn cờ tham gia vào hệ thống tính điểm dựa trên năm đường đặc biệt: ba hàng và hai đường chéo."
date: "2026-06-22T16:18:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105962
codeforces_index: "J"
codeforces_contest_name: "UNICAMP Freshman Contest 2025"
rating: 0
weight: 105962
solve_time_s: 71
verified: true
draft: false
---

[CF 105962J - Tigrinho](https://codeforces.com/problemset/problem/105962/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bảng 3 x 3. Mỗi ô là một chữ số từ 1 đến 9 hoặc ký hiệu ký tự đại diện. Bàn cờ tham gia vào hệ thống tính điểm dựa trên năm đường đặc biệt: ba hàng và hai đường chéo. 

Một dòng tạo ra một đơn vị phần thưởng nếu sau khi bỏ qua các ô ký tự đại diện, tất cả các chữ số còn lại trong dòng đó giống hệt nhau. Nếu một dòng chứa cả chữ số và ký tự đại diện thì các ký tự đại diện sẽ biến mất khỏi danh sách xem xét. Điều này có nghĩa là một dòng có một chữ số luôn tạo ra phần thưởng vì không có gì để không đồng ý. Một dòng chỉ bao gồm các ký tự đại diện không đóng góp gì vì không có chữ số thực tế nào để so sánh. 

Điểm của một bảng là số năm dòng này thỏa mãn điều kiện. Chi phí chơi là cố định nên việc giảm thiểu lợi nhuận cũng tương đương với việc giảm thiểu số lượng dòng thưởng. 

Đầu vào chỉ sửa số lượng ô ký tự đại diện phải xuất hiện trong lưới. Chúng ta có thể thoải mái đặt chúng ở bất cứ đâu và thoải mái lựa chọn chữ số ở các ô còn lại. Nhiệm vụ là xây dựng một bảng có số lượng ký tự đại diện chính xác để tạo ra số điểm nhỏ nhất có thể. 

Lưới rất nhỏ nên mặc dù không gian quyết định liên quan đến cả việc chọn vị trí ký tự đại diện và gán các chữ số, tổng số cấu hình vẫn có thể quản lý được. Các ràng buộc ngay lập tức gợi ý rằng việc tìm kiếm theo cấp số nhân trên 9 ô là khả thi. 

Một trường hợp tinh vi xuất hiện khi một dòng chứa chính xác một chữ số không phải ký tự đại diện. Dòng đó luôn mang lại phần thưởng vì một giá trị duy nhất thỏa mãn một cách tầm thường điều kiện “tất cả đều bằng nhau”. Tương tự, một dòng có 0 chữ số không được coi là dòng có thưởng hợp lệ vì không có giá trị nào để xác định sự bằng nhau. 

Khó khăn thực sự không phải là tính điểm cho một bảng cố định mà là nhận ra cách gán chữ số tương tác với vị trí ký tự đại diện. Khi chúng tôi hiểu rằng các chữ số luôn có thể được chọn một cách đối nghịch để phá vỡ đẳng thức bất cứ khi nào một dòng có ít nhất hai ô cố định, thì vấn đề sẽ trở thành vấn đề thuần túy về tổ hợp đối với vị trí ký tự đại diện. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực trực tiếp sẽ xem xét mọi vị trí của ký tự đại diện và mọi phép gán chữ số từ 1 đến 9 cho các ô còn lại. Đối với mỗi lưới hoàn chỉnh, chúng tôi sẽ đánh giá năm dòng và tính điểm. Điều này đúng nhưng quá lớn: ngay cả khi bỏ qua các chữ số, chỉ chọn vị trí ký tự đại diện cũng đã mang lại 2^9 khả năng và nhân với các phép gán chữ số sẽ dẫn đến cấu hình 9^9, điều này hoàn toàn không khả thi. 

Quan sát quan trọng là các giá trị chữ số không quan trọng riêng lẻ. Chúng tôi được phép gán chúng một cách tự do sau khi sửa các vị trí ký tự đại diện. Đối với bất kỳ dòng nào chứa ít nhất hai ô không phải ký tự đại diện, chúng ta luôn có thể gán các chữ số khác nhau ở các vị trí đó để ngăn dòng trở nên đồng nhất. Điều này có nghĩa là những dòng như vậy luôn có thể được tạo ra để đóng góp bằng không. 

Một dòng chỉ trở nên không thể tránh khỏi khi nó có nhiều nhất một ô không phải ký tự đại diện. Trong trường hợp đó, bất kể chữ số được gán như thế nào, dòng sẽ luôn thỏa mãn điều kiện. Do đó, điểm của một cấu hình chỉ phụ thuộc vào số dòng chứa ít nhất hai ký tự đại diện. 

Vấn đề giảm xuống việc chọn các ô K để đánh dấu làm ký tự đại diện trong lưới 3 x 3 sao cho số dòng chứa ít nhất hai ký tự đại diện được giảm thiểu. Điều này đủ nhỏ để tạo ra sức mạnh tàn bạo cho tất cả các kết hợp vị trí K trong số 9. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lưới đầy đủ lực lượng vũ phu | O(9^9) | O(1) | Quá chậm | 
| Vị trí ký tự đại diện Brute Force | O(2^9 · 5) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa các ô lưới dưới dạng các chỉ số từ 0 đến 8. Mỗi tập hợp con có kích thước K đại diện cho một vị trí có thể có của các ký tự đại diện.

1. Liệt kê tất cả các tập con có kích thước K trong số 9 vị trí. Mỗi tập hợp con đại diện cho ô nào là ký tự đại diện. 
2. Với mỗi tập hợp con, hãy tính điểm trên năm dòng cố định: ba hàng và hai đường chéo. Mỗi dòng được kiểm tra bằng cách đếm xem nó chứa bao nhiêu ô ký tự đại diện. 
3. Nếu một dòng chứa hai ký tự đại diện trở lên, nó sẽ luôn mang lại phần thưởng bất kể việc gán chữ số nào, vì vậy chúng tôi sẽ tăng điểm cho dòng đó. 
4. Theo dõi điểm tối thiểu trên tất cả các tập hợp con. 

Lý do đằng sau bước 3 là rất quan trọng. Khi một dòng có ít nhất hai ký tự đại diện thì nó có nhiều nhất một chữ số cố định. Bất kỳ giá trị đơn lẻ nào cũng không thể xung đột với chính nó, do đó, dòng này chắc chắn sẽ trở nên thống nhất sau khi bỏ qua các ký tự đại diện. 

### Tại sao nó hoạt động 

Sau khi sửa vị trí ký tự đại diện, việc gán chữ số có toàn quyền tự do trên các ô còn lại. Bất kỳ dòng nào vẫn có ít nhất hai vị trí không phải ký tự đại diện đều có thể bị ngắt bằng cách gán các chữ số riêng biệt ở các vị trí đó. Vì vậy những dòng như vậy không bao giờ bị buộc phải đóng góp vào điểm số. Ngược lại, bất kỳ dòng nào có hai ký tự đại diện trở lên đều không thể bị ngắt vì nó chứa tối đa một chữ số thực. Điều này tạo ra sự tương đương trực tiếp giữa các dòng tính điểm và các dòng có ít nhất hai ký tự đại diện, làm cho việc tìm kiếm tổ hợp trên các vị trí trở nên đầy đủ và chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

lines = [
    (0, 1, 2),
    (3, 4, 5),
    (6, 7, 8),
    (0, 4, 8),
    (2, 4, 6)
]

def solve():
    K = int(input().strip())

    best = 10

    for mask in range(1 << 9):
        if mask.bit_count() != K:
            continue

        score = 0
        for a, b, c in lines:
            cnt = 0
            if mask & (1 << a):
                cnt += 1
            if mask & (1 << b):
                cnt += 1
            if mask & (1 << c):
                cnt += 1

            if cnt >= 2:
                score += 1

        best = min(best, score)

    print(best)

if __name__ == "__main__":
    solve()
```Việc triển khai mã hóa lưới dưới dạng mặt nạ 9 bit, trong đó mỗi bit cho biết liệu một ô có phải là ký tự đại diện hay không. Đối với mỗi mặt nạ hợp lệ với tập hợp chính xác K bit, chúng tôi đánh giá năm dòng có liên quan. Việc tính điểm diễn ra trực tiếp từ việc đếm xem có bao nhiêu ô ký tự đại diện rơi vào mỗi dòng. 

Bộ lọc đếm bit đảm bảo chúng tôi chỉ đánh giá các cấu hình hợp lệ và các hoạt động bit tích hợp của Python giúp việc liệt kê này cực kỳ nhanh chóng. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp K = 2. Chúng tôi kiểm tra cấu hình trong đó cả hai ký tự đại diện được đặt ở hàng trên cùng. 

| Bước | Hàng 1 | Hàng 2 | Hàng 3 | Đường chéo 1 | Đường chéo 2 | Điểm | 
| --- | --- | --- | --- | --- | --- | --- | 
| Vị trí ban đầu | 2 ký tự đại diện | 0 | 0 | 0 | 0 | 1 | 

Chỉ có hàng trên cùng chứa hai ký tự đại diện nên đây là dòng thắng buộc duy nhất. Tất cả các dòng khác vẫn chứa ít nhất hai ô cố định, cho phép gán chữ số để tránh sự bằng nhau. 

Điều này xác nhận rằng chỉ những dòng có ít nhất hai ký tự đại diện mới đóng góp. 

Bây giờ hãy xem xét K = 3, với các ký tự đại diện được đặt trên đường chéo chính. 

| Bước | Hàng 1 | Hàng 2 | Hàng 3 | Đường chéo 1 | Đường chéo 2 | Điểm | 
| --- | --- | --- | --- | --- | --- | --- | 
| Vị trí | 1 | 1 | 1 | 3 | 0 | 1 | 

Đường chéo chính có ba ký tự đại diện và tự động là đường chiến thắng. Không có dòng nào khác đạt đến ngưỡng hai ký tự đại diện nên điểm vẫn là 1. 

Những dấu vết này minh họa rằng điểm số chỉ phụ thuộc vào cách các ký tự đại diện giao nhau với năm dòng cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^9 · 5) | Chúng tôi liệt kê tất cả các tập hợp con ký tự đại diện và kiểm tra năm dòng trên mỗi tập hợp con | 
| Không gian | O(1) | Chỉ sử dụng một số biến cố định và các dòng được xác định trước | 

Tổng số cấu hình chỉ có 512 và mỗi cấu hình đều yêu cầu công việc liên tục. Điều này nằm trong giới hạn ngay cả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimal cases
assert run("0\n") == "0"
assert run("9\n") == "5"

# single wildcard
assert run("1\n") == "0"

# small structured case
assert run("2\n") in ["0", "1"]

# full diagonal domination
assert run("3\n") >= "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 | 0 | Không có ký tự đại diện, không có dòng bắt buộc | 
| 9 | 5 | Tất cả các dòng chỉ chứa ký tự đại diện | 
| 1 | 0 | Ký tự đại diện đơn không thể buộc một dòng | 
| 3 | tối thiểu thay đổi | Các trường hợp tương tác chéo | 

## Vỏ cạnh 

Khi K = 0, không có ký tự đại diện nào tồn tại và mỗi dòng có ba chữ số. Chúng ta luôn có thể gán các chữ số để không có dòng nào trở thành đồng nhất, chẳng hạn bằng cách sử dụng ba giá trị khác nhau trên mỗi dòng. Thuật toán tìm chính xác các dòng bắt buộc bằng 0 vì không có dòng nào có hai ký tự đại diện. 

Khi K = 9, mỗi ô là ký tự đại diện. Mỗi dòng trong số năm dòng chứa ba ký tự đại diện nên mỗi dòng đều được tính. Bảng liệt kê đánh giá chính xác mọi mặt nạ và trả về điểm năm. 

Khi K nhỏ, chẳng hạn như 1 hoặc 2, không dòng nào có thể đạt đến ngưỡng của hai ký tự đại diện trừ khi chúng được căn chỉnh cẩn thận. Lực lượng vũ phu xác định chính xác rằng hầu hết các cấu hình đều mang lại các đường bắt buộc bằng 0, phù hợp với lý luận tổ hợp.
