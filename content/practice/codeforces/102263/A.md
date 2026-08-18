---
title: "CF 102263A - Có dễ không?"
description: "Mỗi đội tham gia cần chính xác một bản in của báo cáo vấn đề. Nếu in 1 bản tốn k xu và có n đội thì tổng chi phí bằng giá 1 bản nhân với số đội. Dữ liệu vào chứa hai số nguyên n và k."
date: "2026-08-17T19:52:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102263
codeforces_index: "A"
codeforces_contest_name: "ArabellaCPC 2019"
rating: 0
weight: 102263
solve_time_s: 49
verified: true
draft: false
---

[CF 102263A - Có dễ không?](https://codeforces.com/problemset/problem/102263/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi đội tham gia cần chính xác một bản in của báo cáo vấn đề. Nếu in một bản có giá`k`tiền xu và có`n`đội, tổng chi phí bằng giá một bản nhân với số đội. 

Đầu vào chứa hai số nguyên,`n`Và`k`. giá trị`n`cho chúng tôi biết có bao nhiêu đội cần bản sao, trong khi`k`là giá in một bản. Đầu ra cần thiết là tổng số xu cần thiết để in một bản sao cho mỗi đội, đơn giản là`n * k`. 

Cả hai giá trị đều nằm trong khoảng từ 1 đến 1000. Các giới hạn này cực kỳ nhỏ, do đó, ngay cả thuật toán tuyến tính cũng sẽ kết thúc ngay lập tức. Không cần mảng, đồ thị, lập trình động hoặc bất kỳ quy trình lặp nào. Vì tổng được lấy trực tiếp từ hai giá trị đầu vào nên thời gian không đổi là giải pháp tự nhiên. 

Đầu vào nhỏ nhất có thể là`1 1`, tạo ra`1`. Việc triển khai bất cẩn giả định rằng có nhiều nhóm có thể xử lý sai trường hợp này, nhưng về mặt toán học thì không có gì đặc biệt về một nhóm duy nhất. 

Ví dụ, với đầu vào`1 7`, chỉ cần một bản sao, vì vậy kết quả đầu ra là`7`. Việc triển khai vô tình thêm giá một lần nữa do ranh giới vòng lặp không chính xác có thể tạo ra`14`. 

Các giá trị lớn nhất có thể cũng đơn giản. Với đầu vào`1000 1000`, câu trả lời là`1000000`. Kết quả lớn hơn cả hai đầu vào, do đó việc triển khai sử dụng loại số nguyên hẹp không cần thiết trong ngôn ngữ có loại số nguyên có chiều rộng cố định có thể tràn trong một phiên bản khác của vấn đề này. Số nguyên Python không có vấn đề này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ mô phỏng quá trình in. Bắt đầu với tổng số 0 và lặp lại`n`lần, thêm`k`đến tổng số trên mỗi lần lặp. Điều này đúng vì mỗi lần lặp lại đại diện cho một đội nhận được một bản in, vì vậy sau tất cả các lần lặp, giá trị tích lũy chính xác là giá của tất cả các bản sao. 

Với các ràng buộc thực tế, phương pháp brute-force này thực hiện tối đa 1000 phép cộng. Nó không quá chậm chút nào và nó sẽ được chấp nhận dễ dàng. Ở đây không có kích thước đầu vào trung thực nào mà tại đó phương pháp bạo lực cụ thể này trở thành một vấn đề thực tế. 

Quan sát tốt hơn là liên tục thêm cùng một giá trị`k`, chính xác`n`lần, là định nghĩa của phép nhân. Thay vì biểu diễn`k + k + ... + k`thông qua một vòng lặp, chúng ta có thể tính toán`n * k`trực tiếp. Điều này làm giảm công việc từ tỷ lệ thuận đến`n`tới một số phép tính số học cố định. 

Lực lượng vũ phu hoạt động vì mỗi đội đóng góp chính xác`k`xu, nhưng nó thực hiện phép tính giống hệt đó một cách riêng biệt cho mỗi đội. Cấu trúc của bài toán cho chúng ta biết rằng mọi đóng góp đều bằng nhau, do đó toàn bộ tổng có thể được quy về một phép nhân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Được chấp nhận cho những ràng buộc này | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai số nguyên`n`Và`k`. Giá trị đầu tiên thể hiện số lượng đội và giá trị thứ hai thể hiện chi phí in của một bản sao. 
2. Tính toán`n * k`. Mỗi trong số`n`các đội yêu cầu một bản sao và mỗi bản sao đều có giá chính xác`k`, do đó phép nhân sẽ cho ra chi phí hoàn chỉnh mà không cần mô phỏng rõ ràng từng nhóm riêng lẻ. 
3. In giá trị kết quả. Không cần xử lý bổ sung vì phép nhân đã thể hiện tổng số được yêu cầu. 

### Tại sao nó hoạt động 

Với mỗi đội, ban giám khảo dành chính xác`k`tiền xu. Với`n`đội, tổng số có thể được viết dưới dạng tổng lặp lại`k + k + ... + k`, chứa`n`điều khoản. Theo định nghĩa của phép nhân, tổng này bằng`n * k`. Thuật toán tính toán chính xác giá trị này nên kết quả in ra là tổng chi phí cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
print(n * k)
```Dòng đầu tiên nhập`sys`, cho phép sử dụng dung dịch`sys.stdin.readline`theo yêu cầu đối với đầu vào lập trình cạnh tranh. 

Dòng thứ hai đọc dòng đầu vào duy nhất và chuyển đổi hai giá trị được phân tách bằng khoảng trắng thành số nguyên. Không cần phải xử lý nhiều trường hợp kiểm thử vì bài toán chứa chính xác một cặp giá trị. 

Dòng cuối cùng thực hiện phép tính đầy đủ và in ra. Không có ranh giới vòng lặp hoặc hoạt động lập chỉ mục, do đó không có vấn đề riêng lẻ. Kiểu số nguyên của Python cũng xử lý toàn bộ kết quả được phép một cách an toàn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với đầu vào`5 3`, có năm đội và mỗi bản in có giá ba xu. 

| n | k | Tổng cộng | 
| --- | --- | --- | 
| 5 | 3 | 15 | 

phép nhân`5 * 3`cho`15`, vậy ban giám khảo cần 15 xu. 

### Mẫu 2 

Đối với đầu vào`4 1`, có bốn đội và mỗi đội có giá một xu. 

| n | k | Tổng cộng | 
| --- | --- | --- | 
| 4 | 1 | 4 | 

Ở đây phép nhân là`4 * 1`, cho`4`. Điều này cũng thể hiện hành vi ranh giới khi chi phí của một bản sao là giá trị tối thiểu có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Giải pháp thực hiện một phép nhân và một thao tác đầu ra. | 
| Không gian | O(1) | Chỉ có hai số nguyên đầu vào và kết quả được lưu trữ. | 

Những ràng buộc cho phép`n`Và`k`tối đa là 1000, do đó, ngay cả phương pháp mô phỏng O(n) cũng sẽ đủ nhanh. Giải pháp tối ưu O(1) đơn giản hơn và loại bỏ hoàn toàn sự lặp lại không cần thiết. Việc sử dụng bộ nhớ của nó cũng không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    n, k = map(int, input().split())
    print(n * k)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# provided samples
assert run("5 3\n") == "15\n", "sample 1"
assert run("4 1\n") == "4\n", "sample 2"

# minimum-size input
assert run("1 1\n") == "1\n", "minimum values"

# maximum-size input
assert run("1000 1000\n") == "1000000\n", "maximum values"

# one team with a larger printing cost
assert run("1 1000\n") == "1000\n", "single team"

# minimum printing cost with the maximum number of teams
assert run("1000 1\n") == "1000\n", "unit cost"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Giá trị tối thiểu được phép | 
|`1000 1000`|`1000000`| Giá trị tối đa và phép nhân | 
|`1 1000`|`1000`| Trường hợp ranh giới một đội | 
|`1000 1`|`1000`| Chi phí tối thiểu với nhiều đội | 

## Vỏ cạnh 

đầu vào`1 1`là trường hợp hợp lệ nhỏ nhất. Thuật toán đọc`n = 1`Và`k = 1`, tính toán`1 * 1`, và bản in`1`. Không có gì cần phải có vỏ đặc biệt, điều này tốt hơn là đưa ra một điều kiện có thể tạo ra lỗi biên. 

đầu vào`1 7`đại diện cho chính xác một đội trả tiền cho đúng một bản sao với bảy đồng xu. Việc tính toán là`1 * 7 = 7`, vì vậy đầu ra là`7`. Việc triển khai dựa trên vòng lặp có phạm vi không chính xác, chẳng hạn như`range(1, n)`sẽ thực hiện 0 lần và tạo ra số 0 không chính xác. 

đầu vào`1000 1000`thực hiện các giá trị lớn nhất mà bài toán cho phép. Việc tính toán là`1000 * 1000 = 1000000`. Thuật toán không phụ thuộc vào độ lớn của`n`về số lượng phép toán của nó và Python có thể biểu diễn trực tiếp số nguyên kết quả. 

đầu vào`1000 1`kiểm tra ranh giới khác trong đó mỗi bản sao đều có giá tối thiểu. Tổng cộng là`1000 * 1 = 1000`. Điều này xác nhận rằng số lượng nhóm, chứ không phải chi phí của một bản sao riêng lẻ, sẽ quyết định số lượng đóng góp vào tổng số.
