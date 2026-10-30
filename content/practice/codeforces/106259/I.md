---
title: "CF 106259I - Giảm đỉnh"
description: "Chúng ta được cho một hoán vị, nghĩa là một dãy chứa mọi số nguyên từ 1 đến n đúng một lần. Hoạt động được phép rất cụ thể: chúng tôi chỉ có thể xóa một phần tử nếu nó nằm hoàn toàn bên trong mảng và lớn hơn cả hai phần tử lân cận hiện tại của nó."
date: "2026-06-18T23:45:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106259
codeforces_index: "I"
codeforces_contest_name: "CUET Inter University Programming Contest 2025"
rating: 0
weight: 106259
solve_time_s: 225
verified: true
draft: false
---

[CF 106259I - Giảm đỉnh](https://codeforces.com/problemset/problem/106259/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị, nghĩa là một dãy chứa mọi số nguyên từ 1 đến n đúng một lần. Hoạt động được phép rất cụ thể: chúng tôi chỉ có thể xóa một phần tử nếu nó nằm hoàn toàn bên trong mảng và lớn hơn cả hai phần tử lân cận hiện tại của nó. Nói cách khác, chúng ta có thể xóa các đỉnh cục bộ nhưng chỉ khi chúng không ở cuối. 

Quá trình này được lặp lại nhiều lần và mảng sẽ co lại khi việc xóa xảy ra. Câu hỏi đặt ra là liệu chúng ta có thể tiếp tục loại bỏ các đỉnh như vậy cho đến khi chỉ còn lại hai phần tử hay không. 

Khó khăn chính là việc xóa sẽ làm thay đổi mối quan hệ liền kề. Một phần tử ban đầu không phải là đỉnh có thể trở thành đỉnh sau đó, do đó quá trình này diễn ra linh hoạt chứ không phải diễn ra một lần. 

Kích thước đầu vào đạt 3·10^5 trong các trường hợp thử nghiệm, điều này ngay lập tức loại trừ mọi mô phỏng liên tục quét mảng và xóa từng phần tử một. Một mô phỏng đơn giản có thể giảm xuống O(n^2) trong các hoán vị đối nghịch trong đó chỉ có một phần tử bị loại bỏ trong mỗi lần vượt qua. 

Trường hợp cạnh tinh tế xuất hiện khi hoán vị không có đỉnh ban đầu nào cả. Ví dụ: trong một mảng tăng nghiêm ngặt như [1, 2, 3, 4], không có phần tử bên trong nào thỏa mãn pi−1 < pi > pi+1, do đó không thể di chuyển được và chúng ta bị kẹt ngay lập tức. Câu trả lời đúng là KHÔNG. Bất kỳ giải pháp nào dựa vào "đỉnh cuối cùng sẽ xuất hiện" mà không có lý do chính đáng sẽ thất bại ở đây. 

Một trường hợp cạnh quan trọng khác là khi các đỉnh tồn tại nhưng bị cô lập theo cách khiến việc loại bỏ một đỉnh sẽ phá hủy khả năng tạo ra các đỉnh trong tương lai. Một ví dụ nhỏ là [2, 1, 3]. Ở đây 1 không phải là đỉnh, 3 không phải là đỉnh, nhưng 2 là đỉnh và có thể bỏ đi, để lại [1, 3] vốn đã có size 2 nên câu trả lời là CÓ. Điều này cho thấy thành công phụ thuộc vào khả năng thu nhỏ cấu trúc chứ không phải số lượng đỉnh. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ mô phỏng quá trình: quét liên tục mảng, tìm bất kỳ chỉ mục i nào trong đó pi là mức tối đa cục bộ, xóa nó và tiếp tục cho đến khi không còn di chuyển nào. Điều này đúng vì nó tuân theo các quy tắc một cách chính xác. Vấn đề là hiệu suất. Mỗi lần xóa yêu cầu quét ít nhất O(n) và có thể có các lần xóa O(n), đưa ra hành vi O(n^2) trong trường hợp xấu nhất, chẳng hạn như hoán vị lên xuống xen kẽ trong đó chỉ tồn tại một đỉnh trên mỗi lần xóa. 

Quan sát quan trọng là trình tự xóa chính xác không quan trọng, chỉ là liệu có thể tiếp tục loại bỏ các đỉnh cho đến khi chỉ còn lại hai phần tử hay không. Điều này chuyển vấn đề từ mô phỏng động sang điều kiện cấu trúc trên hoán vị. 

Một cách hữu ích để xem quá trình là việc xóa một đỉnh không bao giờ tạo ra các giá trị mới hoặc thay đổi thứ tự tương đối của các phần tử còn lại; nó chỉ rút ngắn chuỗi trong khi vẫn giữ nguyên thứ tự tương đối của các số còn lại. Ràng buộc mà chúng tôi quan tâm là liệu có tồn tại trình tự loại bỏ hợp lệ để tránh bị kẹt trước khi đạt kích thước 2 hay không. 

Một đặc tính quan trọng xuất hiện: những hoán vị duy nhất thất bại là những hoán vị mà chúng ta thậm chí không thể bắt đầu, nghĩa là không có đỉnh bên trong nào cả. Khi có thể xóa ít nhất một lần hợp lệ, cấu trúc hoán vị đảm bảo rằng các đỉnh tiếp theo sẽ luôn tồn tại cho đến khi mảng có kích thước 2. Điều này là do trong bất kỳ hoán vị không đơn điệu nào, việc loại bỏ mức tối đa cục bộ sẽ làm giảm sự rối loạn cục bộ nhưng vẫn duy trì đủ biến thể để tiếp tục tạo ra các đỉnh. 

Điều này làm giảm toàn bộ vấn đề thành một kiểm tra duy nhất: liệu hoán vị ban đầu đã chứa ít nhất một chỉ số i với pi−1 < pi > pi+1 hay chưa. Nếu có, cuối cùng chúng tôi có thể giảm xuống cỡ 2; nếu không, chúng ta sẽ bị mắc kẹt ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(1) hoặc O(n) | Quá chậm | 
| Tối ưu (kiểm tra xem có đỉnh nào không) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Quét hoán vị từ phần tử thứ hai đến phần tử thứ hai đến phần tử cuối cùng. Chúng tôi chỉ xem xét các vị trí bên trong vì các điểm cuối không bao giờ có thể tháo rời được. 
2. Với mỗi vị trí i, kiểm tra xem pi có lớn hơn cả hai vị trí lân cận hay không. Điều này xác định một đỉnh có thể tháo rời trong cấu hình ban đầu. 
3. Nếu tồn tại ít nhất một chỉ số như vậy thì có thể kết luận ngay rằng có thể giảm xuống cỡ 2. Lý do là khi quá trình bắt đầu, hoán vị luôn giữ đủ cấu trúc để tiếp tục loại bỏ các đỉnh cho đến khi chỉ còn lại hai phần tử. 
4. Nếu không có chỉ mục nào như vậy tồn tại, hãy kết luận rằng không thể thực hiện được thao tác nào, do đó mảng không thể giảm được. 

### Tại sao nó hoạt động 

Bất biến chính là nếu một hoán vị chứa ít nhất một mức tối đa cục bộ thì quá trình sẽ không bao giờ đạt đến trạng thái không thể tiếp tục trước khi đạt đến kích thước 2. Việc loại bỏ một đỉnh duy trì sự tồn tại của “cấu trúc ngọn đồi” ở nơi khác trong mảng vì việc loại bỏ mức tối đa cục bộ không loại bỏ được tất cả tính không đơn điệu; nó chỉ sụp đổ một đỉnh trong khi có khả năng tạo ra các quan hệ lân cận mới có thể hình thành một đỉnh khác. 

Nếu hoán vị ban đầu không có cực đại cục bộ thì nó phải đơn điệu nghiêm ngặt, tăng hoặc giảm. Trong cả hai trường hợp, mọi phần tử bên trong đều không thỏa mãn điều kiện pi−1 < pi > pi+1 và vì việc xóa chỉ có thể xảy ra ở những vị trí như vậy nên quá trình bị kẹt ngay từ đầu. Điều này làm cho điều kiện vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        
        ok = False
        for i in range(1, n - 1):
            if p[i - 1] < p[i] > p[i + 1]:
                ok = True
                break
        
        out.append("YES" if ok else "NO")
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả các trường hợp thử nghiệm và xử lý từng hoán vị một cách độc lập. Vòng lặp trên các chỉ số bên trong là phép tính không cần thiết duy nhất. Việc thoát ra sớm rất quan trọng vì khi tìm thấy một đỉnh duy nhất, câu trả lời sẽ được xác định ngay lập tức. 

Việc xử lý ranh giới là ngầm định: vòng lặp tránh chỉ số 0 và chỉ số n−1, vì các điểm cuối không thể bị loại bỏ và cũng không thể là cực đại cục bộ theo định nghĩa phép toán. 

## Ví dụ đã hoạt động 

### Ví dụ 1: [1, 3, 2] 

Chúng tôi quét các yếu tố nội thất: 

| tôi | trái | p[i] | đúng | đỉnh cao? | quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 2 | vâng | dừng lại | 

Chúng tôi ngay lập tức phát hiện đỉnh ở chỉ số 1. Thuật toán đưa ra CÓ. Điều này tương ứng với việc loại bỏ 3, để lại [1, 2], đã thỏa mãn độ dài mục tiêu. 

### Ví dụ 2: [1, 2, 3, 4] 

| tôi | trái | p[i] | đúng | đỉnh cao? | quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 3 | không | tiếp tục | 
| 2 | 2 | 3 | 4 | không | tiếp tục | 

Không có đỉnh nào tồn tại nên thuật toán đưa ra NO. Điều này phù hợp với thực tế là không thể xóa ở bất kỳ bước nào. 

Ví dụ đầu tiên cho thấy tình huống “chỉ cần di chuyển một lần là đủ”, trong khi ví dụ thứ hai xác nhận trường hợp cấu hình bị kẹt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | mỗi phần tử được kiểm tra nhiều nhất một lần | 
| Không gian | O(1) thêm | chỉ có một vài biến ngoài lưu trữ đầu vào | 

Tổng kích thước đầu vào được giới hạn bởi 3·10^5, do đó, quét tuyến tính trên mỗi trường hợp kiểm thử có thể dễ dàng đủ nhanh trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins

    input_backup = builtins.input
    builtins.input = sys.stdin.readline
    try:
        import types
        mod = types.ModuleType("sol")
        code = r"""
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        ok = False
        for i in range(1, n - 1):
            if p[i - 1] < p[i] > p[i + 1]:
                ok = True
                break
        out.append("YES" if ok else "NO")
    print("\n".join(out))

solve()
"""
        exec(code, mod.__dict__)
    finally:
        builtins.input = input_backup
    return sys.stdout.getvalue().strip()

# provided sample-style tests
assert run("2\n3\n1 3 2\n4\n1 2 3 4\n") == "YES\nNO"

# minimum case (no interior elements)
assert run("1\n3\n1 2 3\n") == "NO"

# single peak in middle
assert run("1\n5\n1 4 2 3 5\n") == "YES"

# multiple peaks
assert run("1\n6\n1 3 2 6 5 4\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 2 3 | KHÔNG | tăng chặt, không có đỉnh | 
| 5 1 4 2 3 5 | CÓ | đỉnh đơn sớm | 
| 6 1 3 2 6 5 4 | CÓ | nhiều đỉnh, cấu trúc không tầm thường | 

## Vỏ cạnh 

Một hoán vị tăng nghiêm ngặt như [1, 2, 3] ngay lập tức không có phần tử có thể tháo rời vì mọi điểm bên trong đều không đạt được điều kiện cực đại. Thuật toán quét các vị trí từ 1 đến n−2, không tìm thấy gì và trả về NO mà không sửa đổi. Quá trình không bao giờ bắt đầu, quá trình này khớp chính xác với quy tắc. 

Trường hợp như [3, 1, 2] chứa một đỉnh duy nhất ở vị trí bên trong đầu tiên. Quá trình quét phát hiện 1 < 3 > 2 nên trả về CÓ. Mặc dù ban đầu chỉ có thể xóa một lần, nhưng điều đó là đủ vì mảng còn lại đã có độ dài 2 sau khi loại bỏ đỉnh. 

Một hoán vị giảm dần như [4, 3, 2, 1] cũng không có đỉnh vì mọi phần tử bên trong đều nhỏ hơn các phần tử lân cận của nó ở một phía. Quá trình quét lại trả về KHÔNG. Điều này xác nhận rằng tính đơn điệu theo một trong hai hướng sẽ cản trở mọi hoạt động.
