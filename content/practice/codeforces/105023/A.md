---
title: "CF 105023A - Xe tải đồ chơi"
description: "Chúng ta được cấp một số nguyên duy nhất biểu thị chiều dài của một chiếc xe tải đồ chơi tính bằng inch. Nhiệm vụ là quyết định xem chiếc xe tải này có thể được đặt bên trong hộp đồ chơi hay không. Hạn chế duy nhất cho việc lắp đặt là chiều dài của xe tải không được vượt quá 10 inch."
date: "2026-06-28T01:42:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105023
codeforces_index: "A"
codeforces_contest_name: "HPI 2024 Novice"
rating: 0
weight: 105023
solve_time_s: 53
verified: true
draft: false
---

[CF 105023A - Xe tải đồ chơi](https://codeforces.com/problemset/problem/105023/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số nguyên duy nhất biểu thị chiều dài của một chiếc xe tải đồ chơi tính bằng inch. Nhiệm vụ là quyết định xem chiếc xe tải này có thể được đặt bên trong hộp đồ chơi hay không. Hạn chế duy nhất cho việc lắp đặt là chiều dài của xe tải không được vượt quá 10 inch. Nếu chiều dài từ 10 trở xuống thì xe vừa vặn; nếu không thì không. 

Mặc dù bài toán có cấu trúc cực kỳ nhỏ nhưng nó vẫn có hình dạng giống như nhiều bài toán quyết định: đọc một giá trị, so sánh nó với một ngưỡng cố định và đưa ra một trong hai câu trả lời khả dĩ dựa trên sự so sánh đó. 

Kích thước đầu vào là tối thiểu, một số nguyên duy nhất được giới hạn từ 1 đến 100. Điều này ngay lập tức loại bỏ mọi nhu cầu về tối ưu hóa thuật toán hoặc cấu trúc dữ liệu. Ngay cả cách tiếp cận ngây thơ nhất cũng thực hiện công việc liên tục. Giới hạn thời gian 1 giây ở đây không liên quan vì việc tính toán chỉ là một so sánh đơn lẻ. 

Không có ràng buộc ẩn phức tạp như nhiều trường hợp thử nghiệm hoặc mảng lớn. Chế độ thất bại tinh tế duy nhất trong các vấn đề thuộc loại này là việc xử lý không chính xác ranh giới so sánh. Ví dụ: coi điều kiện hoàn toàn nhỏ hơn 10 thay vì nhỏ hơn hoặc bằng 10 sẽ từ chối độ dài 10 một cách không chính xác, mặc dù nó phải được chấp nhận. Tương tự, việc nhầm lẫn hướng của bất đẳng thức sẽ đảo ngược tất cả các câu trả lời. 

Trường hợp cạnh bê tông: 

Đầu vào 10 sẽ tạo ra CÓ vì nó khớp chính xác với giới hạn. Một tình trạng bất cẩn như`L < 10`sẽ in sai NO. 

Đầu vào 11 sẽ tạo ra NO vì nó vượt quá kích thước tối đa cho phép. Bất kỳ điều kiện sử dụng`L <= 11`do nhầm lẫn sẽ chấp nhận nó một cách không chính xác. 

Đầu vào 1 sẽ tạo ra CÓ, xác nhận rằng giới hạn dưới không đặc biệt và mọi giá trị dương cho đến 10 đều hợp lệ. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo của vấn đề này vẫn còn tầm thường: đọc số nguyên và kiểm tra xem nó có thỏa mãn ràng buộc bằng cách so sánh trực tiếp hay không. Không có cách giải thích khác cho vấn đề đòi hỏi phải tìm kiếm, sắp xếp hoặc xây dựng các công trình phụ trợ. Ngay cả khi người ta cố gắng đóng khung nó dưới dạng quét các kích thước hộp có thể có hoặc liệt kê các kích thước phù hợp, cấu trúc sẽ sụp đổ ngay lập tức vì chỉ tồn tại một giá trị. 

Quan sát quan trọng là điều kiện khả thi được xác định rõ ràng: xe tải phù hợp khi và chỉ khi chiều dài của nó tối đa là 10. Điều kiện đó đã là giải pháp hoàn chỉnh. Không có trạng thái ẩn hoặc sự phụ thuộc giữa các đầu vào, do đó không cần chuyển đổi ngoài việc đánh giá biểu thức boolean. 

Bởi vì toàn bộ tính toán là một so sánh duy nhất, cả hai phương pháp tiếp cận tối ưu và tối ưu đều trùng khớp. “Tối ưu hóa” chỉ đơn giản là nhận ra rằng không cần tính toán bổ sung. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| So sánh trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên L từ đầu vào. Giá trị này thể hiện chiều dài xe tải phải được xác nhận theo sức chứa của hộp. 
2. So sánh L với 10 bằng cách sử dụng kiểm tra bất đẳng thức không nghiêm ngặt. Điều kiện phải là L ≤ 10, vì định nghĩa bài toán cho phép sự đẳng thức một cách rõ ràng. 
3. Nếu điều kiện đúng, xuất ra "YES", nếu không thì xuất ra "NO". Đầu ra hoàn toàn là một phân loại nhị phân về việc liệu ràng buộc có được thỏa mãn hay không. 

### Tại sao nó hoạt động 

Thuật toán đúng vì bài toán xác định được một điều kiện cần và đủ duy nhất để đảm bảo tính hợp lệ. Không có biến đổi trung gian hoặc ràng buộc thứ cấp. Mọi đầu vào hợp lệ được đặc trưng đầy đủ bởi bất đẳng thức L 10 và mọi đầu vào không hợp lệ đều vi phạm nó. Thuật toán đánh giá trực tiếp vị từ này, do đó nó không thể phân loại sai bất kỳ đầu vào nào miễn là toán tử so sánh đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    L = int(input().strip())
    if L <= 10:
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp đọc một số nguyên duy nhất, cắt bớt đầu vào một cách an toàn và chuyển đổi nó thành số nguyên. Kiểm tra có điều kiện là cốt lõi của việc thực hiện. Điều kiện biên sử dụng`<= 10`, điều này rất cần thiết vì sự bình đẳng được cho phép. 

Cấu trúc được bọc trong một`solve()`chức năng, đó là thông lệ tiêu chuẩn trong lập trình cạnh tranh để làm rõ và tái sử dụng. Bộ bảo vệ chính đảm bảo chức năng được thực thi khi tập lệnh được chạy trực tiếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
43
```| Bước | L | Điều kiện (L ≤ 10) | Đầu ra | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 43 | - | - | 
| Đánh giá | 43 | Sai | KHÔNG | 

Giá trị 43 được so sánh trực tiếp với ngưỡng 10. Vì nó vượt quá giới hạn nên điều kiện không thành công và đầu ra là NO. Điều này xác nhận rằng thuật toán đã loại bỏ chính xác xe tải quá khổ. 

### Ví dụ 2 

đầu vào:```
2
```| Bước | L | Điều kiện (L ≤ 10) | Đầu ra | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 2 | - | - | 
| Đánh giá | 2 | Đúng | CÓ | 

Ở đây giá trị nằm trong phạm vi cho phép. Việc so sánh thành công và đầu ra là CÓ. Điều này xác nhận tính đúng đắn cho các giá trị nhỏ hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Thuật toán thực hiện một lần đọc đầu vào và một so sánh số nguyên | 
| Không gian | O(1) | Chỉ có một biến số nguyên được lưu trữ | 

Các ràng buộc đảm bảo rằng ngay cả việc triển khai đơn giản nhất cũng dễ dàng nằm trong giới hạn. Giải pháp thực hiện công việc liên tục bất kể giá trị đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sysio

    out = sysio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("43\n") == "NO"
assert run("2\n") == "YES"

# custom cases
assert run("10\n") == "YES", "boundary inclusive"
assert run("11\n") == "NO", "just above boundary"
assert run("1\n") == "YES", "minimum value"
assert run("100\n") == "NO", "maximum constraint violation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 | CÓ | độ chính xác bao gồm ranh giới | 
| 11 | KHÔNG | giá trị không hợp lệ đầu tiên trên ngưỡng | 
| 1 | CÓ | trường hợp hợp lệ tối thiểu | 
| 100 | KHÔNG | trường hợp không hợp lệ giới hạn trên | 

## Vỏ cạnh 

Trường hợp cạnh có ý nghĩa duy nhất là ranh giới tại 10. Đối với đầu vào 10, thuật toán đánh giá`10 ≤ 10`, điều này đúng nên kết quả là CÓ. Bất kỳ triển khai nào sử dụng sai bất đẳng thức nghiêm ngặt sẽ thất bại ở đây. 

Đối với đầu vào 11, điều kiện`11 ≤ 10`đánh giá là sai, tạo ra NO như mong đợi. Điều này xác nhận việc từ chối chính xác các giá trị nằm ngoài phạm vi. 

Đối với đầu vào 1, điều kiện`1 ≤ 10`là đúng, xác nhận rằng giới hạn dưới không yêu cầu xử lý đặc biệt và logic được áp dụng thống nhất trên toàn bộ phạm vi hợp lệ.
