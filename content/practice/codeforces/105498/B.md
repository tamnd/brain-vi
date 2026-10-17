---
title: "CF 105498B - Xúc xắc may mắn"
description: "Chúng ta được cấp một số nguyên duy nhất biểu thị tổng số điểm mong muốn khi tung xúc xắc sáu mặt tiêu chuẩn hai lần. Mỗi lần tung sẽ tạo ra một giá trị từ 1 đến 6 và kết quả cuối cùng là tổng của hai kết quả đó."
date: "2026-06-24T00:12:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105498
codeforces_index: "B"
codeforces_contest_name: "Khulna Regional Inter University Programming Contest (KRIUPC) MIRROR"
rating: 0
weight: 105498
solve_time_s: 74
verified: true
draft: false
---

[CF 105498B - Xúc xắc may mắn](https://codeforces.com/problemset/problem/105498/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số nguyên duy nhất biểu thị tổng số điểm mong muốn khi tung xúc xắc sáu mặt tiêu chuẩn hai lần. Mỗi lần tung sẽ tạo ra một giá trị từ 1 đến 6 và kết quả cuối cùng là tổng của hai kết quả đó. Nhiệm vụ là quyết định xem có cách nào để đạt được tổng mục tiêu nhất định bằng cách sử dụng hai cuộn như vậy hay không. 

Mặc dù kích thước đầu vào cực kỳ nhỏ nhưng cấu trúc của bài toán là một câu hỏi khả thi về tổng giới hạn cổ điển. Mỗi biến có một miền cố định nên toàn bộ không gian trạng thái là hữu hạn và rất nhỏ. Điều này ngay lập tức ngụ ý rằng bất kỳ giải pháp nào liệt kê tất cả các kết quả đều đã quá đủ nhanh và thậm chí một đặc tính toán học trực tiếp của các tổng có thể là đủ. 

Một lỗi phổ biến xuất phát từ việc đọc sai phạm vi số tiền cho phép. Vì mỗi xúc xắc hiển thị ít nhất là 1 và nhiều nhất là 6, nên tổng nhỏ nhất có thể là 2 và lớn nhất là 12. Bất kỳ giá trị nào nằm ngoài phạm vi này đều không thể thực hiện được bất kể xúc xắc được tung như thế nào. 

Hai tình huống cụ thể bộc lộ những lỗi lý luận điển hình. Nếu mục tiêu là 1, thì giả định ngây thơ rằng “bất kỳ số nhỏ nào cũng có thể truy cập được” không thành công vì tổng tối thiểu đã là 2, vì vậy câu trả lời đúng là Không. Nếu mục tiêu là 12, một số người có thể cho rằng nó hiếm hoặc đặc biệt, nhưng thực tế nó có thể đạt được với (6, 6), vì vậy câu trả lời đúng là Có. 

## Phương pháp tiếp cận 

Phương pháp brute-force trực tiếp xem xét mọi kết quả có thể có của cặp xúc xắc. Có 6 lựa chọn cho lần quay đầu tiên và 6 lựa chọn cho lần quay thứ hai, tạo ra tổng cộng 36 kết hợp. Đối với mỗi cặp, chúng tôi tính tổng và so sánh nó với giá trị đích. Cách tiếp cận này đúng vì nó kiểm tra toàn bộ không gian mẫu mà không bỏ sót. Chi phí của nó là không đổi đối với kích thước đầu vào, vì số lượng kết quả không bao giờ thay đổi. 

Mặc dù ở đây vũ lực đã đủ hiệu quả, nhưng chúng ta có thể đơn giản hóa hơn nữa bằng cách nhận thấy rằng chúng ta không cần phải liệt kê các kết quả. Tổng của hai số nguyên bị chặn chỉ phụ thuộc vào giá trị cực trị của chúng. Tổng nhỏ nhất có thể đạt được là 1 + 1 và lớn nhất là 6 + 6. Mọi số nguyên ở giữa đều có thể đạt được vì tổng phân bố của hai viên xúc xắc liền kề trong khoảng đó. Điều này làm giảm vấn đề xuống một khoảng thời gian kiểm tra đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(36) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên x biểu thị tổng mục tiêu. Đây là giá trị mà chúng tôi muốn xác minh dựa trên phạm vi tổng xúc xắc có thể tiếp cận. 
2. Xác định số tiền tối thiểu và tối đa có thể có từ hai viên xúc xắc. Mức tối thiểu đến từ cả hai viên xúc xắc hiển thị 1 và mức tối đa đến từ cả hai viên xúc xắc hiển thị 6. 
3. Kiểm tra xem x có nằm trong khoảng đóng này từ 2 đến 12 hay không. Nếu đúng như vậy thì phải tồn tại một cặp xúc xắc hợp lệ để tạo ra nó. 
4. Nếu x nằm trong khoảng, xuất ra "Có". Nếu không thì xuất ra "Không". 

### Tại sao nó hoạt động 

Mọi kết quả hợp lệ đều tương ứng với một cặp (a, b) trong đó cả hai giá trị đều bị giới hạn trong một khoảng nguyên cố định. Tập hợp tất cả các tổng có thể có chính xác là ảnh của tích Đề-các dưới phép cộng. Vì cả hai miền đều là các phạm vi số nguyên liên tục nên tập tổng kết quả không có khoảng cách giữa giá trị tối thiểu và tối đa của nó. Do đó, tư cách thành viên trong tập hợp có thể truy cập tương đương với việc kiểm tra ranh giới đơn giản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    x = int(input().strip())
    if 2 <= x <= 12:
        print("Yes")
    else:
        print("No")

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào một lần và so sánh trực tiếp nó với khoảng khả thi được tính toán trước. Không có vòng lặp hoặc cấu trúc dữ liệu bổ sung. Các điều kiện biên là chi tiết quan trọng: cả hai điểm cuối 2 và 12 đều được bao gồm vì chúng tương ứng với cấu hình xúc xắc hợp lệ (1,1) và (6,6). 

## Ví dụ đã hoạt động 

Xét x = 7. Phạm vi hợp lệ là [2, 12], do đó 7 nằm bên trong nó. 

| Bước | x | Tình trạng | Quyết định | 
| --- | --- | --- | --- | 
| Đầu vào | 7 | - | Đọc giá trị | 
| Kiểm tra giới hạn dưới | 7 ≥ 2 | Đúng | Tiếp tục | 
| Kiểm tra giới hạn trên | 7 12 | Đúng | hợp lệ | 

Điều này xác nhận rằng số tiền trung gian luôn có thể đạt được. 

Bây giờ xét x = 1. 

| Bước | x | Tình trạng | Quyết định | 
| --- | --- | --- | --- | 
| Đầu vào | 1 | - | Đọc giá trị | 
| Kiểm tra giới hạn dưới | 1 ≥ 2 | Sai | Từ chối | 

Điều này cho thấy các giá trị dưới giới hạn tối thiểu sẽ bị loại bỏ ngay lập tức như thế nào. 

Ví dụ đầu tiên xác nhận rằng các giá trị bên trong hoạt động, trong khi ví dụ thứ hai chứng minh rằng ranh giới dưới là nghiêm ngặt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một so sánh duy nhất với giới hạn cố định | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Bản chất thời gian không đổi của giải pháp thấp hơn nhiều so với giới hạn của bất kỳ môi trường lập trình cạnh tranh nào. Kích thước đầu vào không liên quan vì tính toán không mở rộng theo kích thước đó. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import sys as _sys
    out = io.StringIO()
    def solve():
        x = int(input().strip())
        print("Yes" if 2 <= x <= 12 else "No")
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples (conceptual since statement has no explicit sample)
assert run("7\n") == "Yes", "typical achievable sum"
assert run("1\n") == "No", "below minimum impossible"
assert run("12\n") == "Yes", "maximum achievable sum"

# custom cases
assert run("2\n") == "Yes", "minimum boundary"
assert run("13\n") == "No", "just above maximum"
assert run("6\n") == "Yes", "middle value"
assert run("20\n") == "No", "far outside range"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | Có | Trường hợp ranh giới dưới | 
| 13 | Không | Ngay trên phạm vi hợp lệ | 
| 6 | Có | Giá trị nội bộ điển hình | 
| 20 | Không | Đầu vào cực kỳ không hợp lệ | 

## Vỏ cạnh 

Đối với trường hợp giới hạn dưới, hãy xem xét đầu vào x = 2. Thuật toán kiểm tra 2 ≤ x ≤ 12, kết quả này đúng. Điều này tương ứng chính xác với kết quả xúc xắc (1,1), xác nhận tính đúng đắn ở ranh giới. 

Đối với trường hợp giới hạn trên, hãy xem xét x = 12. Việc kiểm tra cũng đạt và điều này tương ứng với (6,6). Mặc dù đây là một cấu hình duy nhất nhưng nó vẫn nằm trong tập hợp có thể truy cập nên thuật toán sẽ chấp nhận nó một cách chính xác. 

Đối với một giá trị nằm dưới phạm vi, chẳng hạn như x = 1, việc kiểm tra giới hạn dưới sẽ thất bại ngay lập tức vì 1 < 2. Điều này phản ánh việc không thể thu được tổng nhỏ hơn hai số nguyên dương độc lập, mỗi số ít nhất một.
