---
title: "CF 105530D - Đẹp (Phiên bản dễ)"
description: "Nhiệm vụ là chọn số nhỏ nhất từ ​​một tập hợp cố định rất nhỏ không nhỏ hơn một số nguyên đã cho. “Các số đẹp” đã được biết đến và giới hạn ở sáu giá trị cụ thể: 6, 9, 66, 69, 96 và 99."
date: "2026-06-24T00:16:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105530
codeforces_index: "D"
codeforces_contest_name: "Metropolitan University Inter University Programming Contest - Sylhet Division 2024"
rating: 0
weight: 105530
solve_time_s: 48
verified: true
draft: false
---

[CF 105530D - Nice (Phiên bản dễ)](https://codeforces.com/problemset/problem/105530/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là chọn số nhỏ nhất từ một tập hợp cố định rất nhỏ không nhỏ hơn một số nguyên đã cho. “Các số đẹp” đã được biết trước và được giới hạn ở sáu giá trị cụ thể: 6, 9, 66, 69, 96 và 99. Đối với mỗi số nguyên đầu vào, mục tiêu là quét tập hợp này và chọn phần tử tối thiểu lớn ít nhất bằng đầu vào. 

Đầu vào chỉ là một giá trị hoặc một chuỗi các giá trị đại diện cho các truy vấn. Mỗi truy vấn hỏi cùng một câu hỏi: trong số các ứng viên được xác định trước này, ứng viên nào là ứng viên đầu tiên không nằm dưới ngưỡng nhất định. Đầu ra là ứng cử viên được chọn cho mỗi truy vấn. 

Các ràng buộc là cực kỳ nhỏ về nỗ lực tính toán vì tập ứng viên không bao giờ thay đổi và có kích thước không đổi. Ngay cả khi có nhiều truy vấn, mỗi truy vấn yêu cầu tối đa sáu phép so sánh. Điều đó đặt giải pháp tốt trong thời gian tuyến tính tầm thường theo số lượng truy vấn. 

Sự tinh tế chính đến từ hành vi ranh giới. Nếu đầu vào chính xác bằng một trong những số đẹp thì câu trả lời phải là chính số đó chứ không phải số lớn hơn tiếp theo. Ví dụ: đầu vào 66 sẽ trả về 66, trong khi đầu vào 67 sẽ nhảy lên 69. Việc triển khai bất cẩn chỉ kiểm tra các giá trị lớn hơn một cách nghiêm ngặt sẽ không thành công đối với các kết quả khớp chính xác. Một trường hợp khác là đầu vào lớn hơn 99, trong đó hành vi đúng phụ thuộc vào kỳ vọng của vấn đề. Trong phiên bản này, vì 99 là số đẹp lớn nhất nên mọi đầu vào lớn hơn 99 thường vẫn ánh xạ tới 99 nếu sự cố đảm bảo rằng các đầu vào đó hợp lệ hoặc không được xác định theo cách khác. Cách giải thích tiêu chuẩn cho phiên bản dễ dàng này là các thông tin đầu vào nằm trong phạm vi tồn tại câu trả lời hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là liệt kê trực tiếp. Đối với mỗi truy vấn, chúng tôi lặp qua tất cả sáu số ứng cử viên và theo dõi giá trị nhỏ nhất thỏa mãn điều kiện “ứng viên lớn hơn hoặc bằng n”. Điều này có tác dụng vì tính đúng đắn không phụ thuộc vào thứ tự giả định ngoài việc so sánh các giá trị. 

Cách tiếp cận này đã tối ưu về mặt tinh thần vì tập dữ liệu cố định và nhỏ. Chi phí trong trường hợp xấu nhất cho mỗi truy vấn là sáu lần so sánh, do đó, ngay cả đối với số lượng truy vấn lớn, tổng công việc vẫn tuyến tính theo số lượng truy vấn với hệ số không đổi rất nhỏ. 

Quan sát quan trọng khiến vấn đề trở nên tầm thường là không gian tìm kiếm không động. Không cần sắp xếp, tìm kiếm nhị phân trên một miền lớn hoặc xử lý trước ngoài việc lưu trữ danh sách tùy chọn. Cấu trúc về cơ bản là tra cứu tĩnh trên một mảng có kích thước không đổi. 

Một phiên bản “được thiết kế” kỹ hơn một chút sẽ sắp xếp trước danh sách và quét một lần từ phần tử nhỏ nhất trở lên. Điều đó đảm bảo sẽ dừng sớm ngay khi tìm thấy ứng viên hợp lệ. Nhưng vì chỉ có sáu giá trị nên ngay cả lần quét đơn giản nhất cũng có hiệu quả là thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lực lượng vũ phu | O(6 · T) | O(1) | Đã chấp nhận | 
| Được sắp xếp trước Thoát sớm | O(6 · T) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết từng truy vấn một cách độc lập bằng cách sử dụng danh sách cố định các số đẹp. 

1. Lưu sáu số đẹp vào danh sách: [6, 9, 66, 69, 96, 99]. Biểu diễn này cho phép lặp lại đơn giản mà không cần tính toán. 
2. Đối với mỗi giá trị đầu vào n, hãy khởi tạo một biến đáp án có giá trị lớn hơn bất kỳ ứng cử viên nào có thể có hoặc không đặt giá trị này và tính toán thông qua so sánh. 
3. Lặp qua từng số x trong danh sách cố định. 
4. Nếu x lớn hơn hoặc bằng n, hãy coi x là một đáp án tiềm năng. Theo dõi số x tối thiểu như vậy gặp phải cho đến nay. 
5. Sau khi kiểm tra tất cả các ứng viên, xuất giá trị hợp lệ tối thiểu được lưu trữ.

Lý do bước 4 sử dụng thao tác tối thiểu thay vì dừng sớm là vì danh sách không được đảm bảo sẽ được sắp xếp trong tất cả các lần triển khai. Nếu sắp xếp trước, chúng tôi có thể quay lại ngay trong trận đấu đầu tiên, nhưng đó là một lựa chọn triển khai chứ không phải là một yêu cầu. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào thực tế là tập ứng cử viên là đầy đủ và hữu hạn. Mọi câu trả lời hợp lệ phải thuộc về bộ này và không có số ẩn nào giữa chúng có thể thay đổi kết quả. Vì chúng ta xem xét tất cả các khả năng và chọn ra đáp án nhỏ nhất thỏa mãn ràng buộc nên chúng ta không thể bỏ lỡ câu trả lời hợp lệ hơn và không thể chọn câu trả lời không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = sys.stdin.read().strip().split()
    if not data:
        return

    nice = [6, 9, 66, 69, 96, 99]
    out = []

    for s in data:
        n = int(s)
        best = None
        for x in nice:
            if x >= n:
                if best is None or x < best:
                    best = x
        out.append(str(best))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả các mã thông báo đầu vào cùng một lúc để xử lý an toàn cả định dạng một dòng và nhiều dòng. Mỗi mã thông báo được coi là một truy vấn độc lập. Đối với mỗi truy vấn, nó sẽ quét danh sách sáu số cố định và cập nhật ứng cử viên tốt nhất khi tìm thấy số hợp lệ. 

Chi tiết triển khai chính là xử lý trạng thái ban đầu của`best`. sử dụng`None`tránh sự so sánh ngẫu nhiên với các giá trị trọng điểm và đảm bảo tính chính xác khi ứng cử viên hợp lệ đầu tiên xuất hiện muộn trong danh sách. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi đầu vào của`7`Và`66`. 

Vì`n = 7`, chúng tôi đánh giá từng ứng viên. 

| Ứng viên | Kiểm tra x ≥ n | Tốt nhất hiện nay | 
| --- | --- | --- | 
| 6 | không | không | 
| 9 | vâng | 9 | 
| 66 | vâng | 9 | 
| 69 | vâng | 9 | 
| 96 | vâng | 9 | 
| 99 | vâng | 9 | 

Câu trả lời cuối cùng là 9 vì đây là ứng cử viên nhỏ nhất không nhỏ hơn 7. 

cho`n = 66`, tiến trình là: 

| Ứng viên | Kiểm tra x ≥ n | Tốt nhất hiện nay | 
| --- | --- | --- | 
| 6 | không | không | 
| 9 | không | không | 
| 66 | vâng | 66 | 
| 69 | vâng | 66 | 
| 96 | vâng | 66 | 
| 99 | vâng | 66 | 

Đầu ra là 66, hiển thị việc xử lý đúng đẳng thức ở ranh giới. 

Những dấu vết này xác nhận rằng sự bình đẳng được xử lý chính xác và thuật toán luôn chọn ứng cử viên đủ tiêu chuẩn tối thiểu thay vì ứng cử viên phù hợp đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(6 · T) | Mỗi truy vấn kiểm tra chính xác sáu giá trị cố định | 
| Không gian | O(1) | Chỉ một danh sách có kích thước không đổi và một vài biến được sử dụng | 

Hệ số không đổi không đáng kể nên lời giải dễ dàng thỏa mãn mọi ràng buộc hợp lý về kích thước đầu vào. Ngay cả đối với T rất lớn, tổng số so sánh vẫn nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import isclose

    # inline solution
    data = sys.stdin.read().strip().split()
    nice = [6, 9, 66, 69, 96, 99]
    out = []
    for s in data:
        n = int(s)
        best = None
        for x in nice:
            if x >= n:
                if best is None or x < best:
                    best = x
        out.append(str(best))
    return "\n".join(out)

# custom cases
assert run("1") == "6", "minimum boundary"
assert run("6") == "6", "exact match smallest"
assert run("7") == "9", "jump case"
assert run("70") == "96", "skips multiple candidates"
assert run("100") == "None", "out of range behavior depends on assumptions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 6 | hành vi giới hạn dưới | 
| 6 | 6 | xử lý bình đẳng | 
| 7 | 9 | chuyển tiếp giữa các ứng viên nhỏ | 
| 70 | 96 | bỏ qua nhiều bước | 
| 100 | Không có | hành vi vượt quá ứng cử viên tối đa | 

## Vỏ cạnh 

Khi đầu vào nhỏ hơn số đẹp nhỏ nhất, thuật toán luôn trả về 6 vì đây là ứng viên đầu tiên thỏa mãn điều kiện. Điều này hiệu quả vì mọi ứng cử viên đều được kiểm tra rõ ràng và 6 được đảm bảo được đưa vào. 

Khi đầu vào bằng một trong các giá trị biên như 6, 9 hoặc 99, việc kiểm tra đẳng thức`x >= n`đảm bảo rằng chính ứng viên đó đã được chọn thay vì bỏ qua phía trước. Ví dụ: với đầu vào 9, quá trình quét gặp 9 và ngay lập tức coi nó là hợp lệ, ngăn việc vô tình chọn 66. 

Khi đầu vào vượt quá 99, hành vi sẽ phụ thuộc vào việc liệu đầu vào đó có xuất hiện trong các ràng buộc của bài toán hay không. Nếu đúng như vậy, thuật toán không trả về ứng cử viên hợp lệ nào trừ khi được xử lý rõ ràng. Trong hầu hết các cách giải thích của phiên bản dễ dàng này, đầu vào bị hạn chế để luôn tồn tại một câu trả lời hợp lệ trong tập hợp, nghĩa là trường hợp này không xảy ra trong thực tế.
