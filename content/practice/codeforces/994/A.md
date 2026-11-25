---
title: "CF 994A - Dấu vân tay"
description: "Đầu vào mô tả một chuỗi chữ số cố định, như lịch sử bàn phím được ghi lại và một bộ chữ số riêng biệt tương ứng với các phím có dấu vân tay."
date: "2026-06-17T00:08:50+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 994
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 488 by NEAR (Div. 2)"
rating: 800
weight: 994
solve_time_s: 251
verified: true
draft: false
---

[CF 994A - Dấu vân tay](https://codeforces.com/problemset/problem/994/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 4 phút 11s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả một chuỗi chữ số cố định, như lịch sử bàn phím được ghi lại và một bộ chữ số riêng biệt tương ứng với các phím có dấu vân tay. Nhiệm vụ là xây dựng lại mã dài nhất có thể mà bạn có thể tạo bằng cách quét chuỗi gốc từ trái sang phải và chỉ giữ lại những chữ số có khóa được đánh dấu là lấy dấu vân tay. Thứ tự tương đối của các chữ số được giữ không được thay đổi và bạn không được phép sắp xếp lại hoặc bỏ qua tùy ý ngoài việc lọc. 

Vì vậy, đầu ra chỉ đơn giản là chuỗi ban đầu được lọc theo thành viên trong tập dấu vân tay, giữ nguyên thứ tự ban đầu. 

Các ràng buộc là cực kỳ nhỏ, với cả độ dài chuỗi và số chữ số được lấy dấu vân tay giới hạn bởi 10. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào từ lọc tuyến tính đến kiểm tra tập hợp con bậc hai hoặc lực lượng vũ phu sẽ chạy ngay lập tức, nhưng giải pháp dự định có thể là một nhiệm vụ triển khai đơn giản hơn là một vấn đề tìm kiếm. 

Một số trường hợp đặc biệt quan trọng theo những cách tinh tế. 

Nếu không có chữ số nào trong chuỗi được lấy dấu vân tay thì đầu ra trống. Ví dụ, trình tự`3 4 5`có bộ dấu vân tay`1 2 7`không tạo ra gì cả Việc triển khai đơn giản giả định ít nhất một kết quả trùng khớp và in mà không kiểm tra có thể vô tình tạo ra rác hoặc thừa khoảng trống. 

Nếu tất cả các chữ số được lấy dấu vân tay, đầu ra là chuỗi ban đầu không thay đổi. Ví dụ, trình tự`1 2 3`có bộ dấu vân tay`1 2 3`phải tái tạo lại toàn bộ trình tự một cách chính xác. 

Một trường hợp góc cạnh khác là bộ dấu vân tay có thể không xuất hiện theo thứ tự được sắp xếp hoặc có thể không khớp với thứ tự trình tự. Ví dụ, trình tự`5 1 0 7`có dấu vân tay`0 1 7`phải sản xuất`1 0 7`, không được sắp xếp theo giá trị chữ số mà theo sự xuất hiện trong chuỗi. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là xem xét mọi dãy con của dãy đã cho và kiểm tra xem tất cả các phần tử của nó có được chứa trong tập dấu vân tay hay không. Trong số các dãy con hợp lệ đó, chúng tôi chọn dãy con dài nhất. Điều này hiệu quả vì nó khám phá rõ ràng tất cả các khả năng, nhưng số lượng chuỗi con của chuỗi có độ dài n là 2ⁿ, tăng nhanh mặc dù ở đây n nhỏ. Mỗi lần kiểm tra tính hợp lệ cũng yêu cầu quét các phần tử, vì vậy điều này trở thành chi phí không cần thiết. 

Nhận xét quan trọng là chúng ta thực sự không cần phải tìm kiếm giữa các dãy con. Bài toán không áp đặt bất kỳ ràng buộc bổ sung nào như tối đa hóa thứ tự từ điển hoặc lựa chọn giữa các phương án xung đột. Yêu cầu duy nhất là mọi chữ số được chọn phải được cho phép và thứ tự đó được giữ nguyên. Điều này loại bỏ bất kỳ lựa chọn tổ hợp nào: đối với mỗi vị trí trong chuỗi, chúng tôi quyết định độc lập xem có nên giữ nó hay không chỉ dựa trên tư cách thành viên trong bộ dấu vân tay. 

Vì vậy, vấn đề giảm xuống còn một tuyến tính duy nhất: lọc các phần tử theo tư cách thành viên đã đặt. 

Cách tiếp cận bạo lực hoạt động về mặt khái niệm vì nó liệt kê tất cả các khả năng, nhưng không hiệu quả do tăng trưởng theo cấp số nhân. Quan sát thấy rằng không có sự phụ thuộc giữa các quyết định ở các vị trí khác nhau sẽ biến nhiệm vụ thành một bài kiểm tra tư cách thành viên đơn giản cho mỗi phần tử, đó là O(1) bằng cách sử dụng một tập hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2ⁿ · n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc độ dài chuỗi, số lượng dấu vân tay, chính chuỗi đó và các chữ số dấu vân tay. Mục đích là để nhanh chóng xác định những chữ số nào được phép. 
2. Lưu trữ các chữ số dấu vân tay trong bộ băm. Điều này được thực hiện để các truy vấn thành viên cho một chữ số có thể được trả lời trong thời gian không đổi. 
3. Lặp lại trình tự ban đầu từ trái sang phải. Đối với mỗi chữ số, hãy kiểm tra xem nó có tồn tại trong bộ dấu vân tay hay không. 
4. Nếu chữ số nằm trong tập hợp, hãy thêm nó vào chuỗi đầu ra. Nếu không, hãy bỏ qua nó hoàn toàn. 
5. Sau khi xử lý tất cả các chữ số, in chuỗi đã lọc thu được, phân tách bằng dấu cách. Nếu không có gì được thu thập, xuất ra một dòng trống. 

Lý do đằng sau thủ tục này là mỗi vị trí trong chuỗi là độc lập. Việc một chữ số có được đưa vào hay không chỉ phụ thuộc vào việc nó có được phép hay không, chứ không phụ thuộc vào số đến trước hay sau. 

### Tại sao nó hoạt động 

Đầu ra được xây dựng chứa chính xác các phần tử từ chuỗi ban đầu thỏa mãn điều kiện dấu vân tay. Vì chúng ta lặp từ trái sang phải mà không sắp xếp lại nên thứ tự tương đối được giữ nguyên tự động. Không có khả năng tạo ra một chuỗi hợp lệ dài hơn chuỗi này, bởi vì mọi chuỗi con hợp lệ chỉ có thể sử dụng các chữ số đã xuất hiện theo thứ tự và chúng tôi bao gồm từng ứng cử viên hợp lệ riêng lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    allowed = set(map(int, input().split()))

    res = []
    for x in a:
        if x in allowed:
            res.append(str(x))

    print(" ".join(res))

if __name__ == "__main__":
    solve()
```Giải pháp này đọc dữ liệu đầu vào bằng cách sử dụng I/O nhanh để tránh chi phí hoạt động, mặc dù điều này không thực sự cần thiết do có những hạn chế. 

Các chữ số dấu vân tay được lưu trữ trong một bộ gọi là`allowed`, cho phép kiểm tra tư cách thành viên O(1). Đây là sự tối ưu hóa trung tâm thay thế mọi nhu cầu tìm kiếm hoặc các vòng lặp lồng nhau. 

Vòng lặp kết thúc`a`là bộ lọc trực tiếp. Mỗi chữ số chỉ được chuyển đổi thành một chuỗi khi nó được giữ lại, vì định dạng đầu ra cuối cùng yêu cầu các số nguyên được phân tách bằng dấu cách. 

Thao tác nối xử lý chính xác cả đầu ra không trống và đầu ra trống. Nếu như`res`trống rỗng,`" ".join(res)`tạo ra một chuỗi trống, thỏa mãn yêu cầu không in gì hoặc một dòng trống. 

## Ví dụ đã hoạt động 

Xem xét trình tự mẫu`7 5 3 1 6 2 8`bằng chữ số dấu vân tay`1 2 7`. 

Ở mỗi bước chúng tôi kiểm tra tư cách thành viên: 

| Chỉ mục | Chữ số | Trong dấu vân tay | Đầu ra cho đến nay | 
| --- | --- | --- | --- | 
| 0 | 7 | vâng | 7 | 
| 1 | 5 | không | 7 | 
| 2 | 3 | không | 7 | 
| 3 | 1 | vâng | 7 1 | 
| 4 | 6 | không | 7 1 | 
| 5 | 2 | vâng | 7 1 2 | 
| 6 | 8 | không | 7 1 2 | 

Đầu ra cuối cùng là`7 1 2`, chứng tỏ rằng thứ tự được giữ nguyên trong khi quá trình lọc hoàn toàn mang tính cục bộ cho mỗi phần tử. 

Bây giờ hãy xem xét một ví dụ được xây dựng: chuỗi`4 0 9 1`có dấu vân tay`0 1`. 

| Chỉ mục | Chữ số | Trong dấu vân tay | Đầu ra cho đến nay | 
| --- | --- | --- | --- | 
| 0 | 4 | không | | 
| 1 | 0 | vâng | 0 | 
| 2 | 9 | không | 0 | 
| 3 | 1 | vâng | 0 1 | 

Điều này xác nhận rằng thuật toán xử lý chính xác các trường hợp trong đó các chữ số hợp lệ được xen kẽ với các chữ số không hợp lệ mà không làm mất thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần qua chuỗi với kiểm tra tư cách thành viên O(1) cho mỗi phần tử | 
| Không gian | O(m) | Đặt lưu trữ chữ số dấu vân tay | 

Giới hạn đầu vào cực kỳ nhỏ nên quá trình quét tuyến tính này thấp hơn nhiều so với bất kỳ giới hạn hiệu suất nào. Ngay cả đối với những ràng buộc lớn hơn nhiều, phương pháp này vẫn hiệu quả do lọc theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    allowed = set(map(int, input().split()))

    res = []
    for x in a:
        if x in allowed:
            res.append(str(x))
    print(" ".join(res))

assert run("7 3\n3 5 7 1 6 2 8\n1 2 7\n") == "7 1 2", "sample 1"
assert run("3 2\n1 2 3\n4 5\n") == "", "no matches"
assert run("4 3\n1 2 3 4\n1 2 3\n") == "1 2 3", "prefix match"
assert run("5 2\n9 8 7 6 5\n5 9\n") == "9 5", "scattered matches"
assert run("1 1\n0\n0\n") == "0", "single element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 2 / 1 2 3 / 4 5 | trống | không có chữ số hợp lệ | 
| 4 3 / 1 2 3 4 / 1 2 3 | 1 2 3 | bảo quản tiền tố một phần | 
| 5 2/9 8 7 6 5/5 9 | 9 5 | lựa chọn không đơn điệu | 

## Vỏ cạnh 

Khi không có chữ số nào khớp với bộ dấu vân tay, thuật toán sẽ tạo danh sách kết quả trống. Ví dụ, đầu vào`3 2 / 3 4 5 / 1 2`không bao giờ kích hoạt bất kỳ phần bổ sung nào, vì vậy`res`vẫn trống và việc in sẽ tạo ra một dòng trống theo yêu cầu. 

Khi mọi chữ số khớp nhau, chẳng hạn như`4 4 / 1 2 3 4 / 1 2 3 4`, mỗi lần lặp lại sẽ thêm vào kết quả. Đầu ra cuối cùng phản ánh chính xác trình tự đầu vào, xác nhận rằng quá trình lọc không làm sai lệch thứ tự. 

Khi các trận đấu thưa thớt và tách biệt, như`6 2 / 1 0 1 0 1 0 / 1 0`, thuật toán xen kẽ giữa nối thêm và bỏ qua. Mỗi quyết định đều mang tính cục bộ, do đó đầu ra sẽ trở thành`1 0 1 0 1 0`, bảo toàn cấu trúc đầy đủ mà không cần quay lại hoặc xây dựng lại.
