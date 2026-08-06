---
title: "CF 102503B - Bogart bị loại"
description: "Lịch sử trò chuyện được thể hiện bằng một chuỗi tên người dùng. Mỗi tên người dùng tương ứng với một người bạn gửi cùng một tin nhắn, ký tự đơn F."
date: "2026-08-05T16:31:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102503
codeforces_index: "B"
codeforces_contest_name: "National Olympiad in Informatics - Philippines (NOI.PH) Online Eliminations 2020"
rating: 0
weight: 102503
solve_time_s: 120
verified: true
draft: false
---

[CF 102503B - Bogart bị loại](https://codeforces.com/problemset/problem/102503/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Lịch sử trò chuyện được thể hiện bằng một chuỗi tên người dùng. Mỗi tên người dùng tương ứng với một người bạn gửi cùng một tin nhắn, ký tự đơn`F`. Nhiệm vụ là tạo lại nhật ký trò chuyện cuối cùng bằng cách in tên người dùng của từng người bạn, sau đó là`: F`, giữ chính xác thứ tự như đầu vào. 

Kích thước đầu vào là yếu tố cần cân nhắc chính. Số lượng bạn bè có thể lên tới 100.000, do đó, bất kỳ phương pháp nào liên tục tìm kiếm, sắp xếp không cần thiết hoặc thực hiện công việc tỷ lệ thuận với bình phương số lượng bạn bè sẽ là quá chậm. Một giải pháp sẽ xử lý từng tên người dùng với số lần không đổi, điều này dẫn đến cách tiếp cận O(n) một cách tự nhiên. Độ dài tên người dùng nhỏ nên việc xử lý trực tiếp từng chuỗi không tốn kém. 

Có một vài trường hợp đơn giản mà việc triển khai bất cẩn có thể thất bại. Nếu chỉ có một người bạn thì chương trình vẫn phải ra một dòng có định dạng. 

đầu vào:```
1
Alice
```Đầu ra đúng:```
Alice: F
```Việc triển khai giả định nhiều dòng hoặc quên xử lý dòng mới cuối cùng có thể thất bại ở đây. 

Tên người dùng có phân biệt chữ hoa chữ thường. Những cái tên`bob`Và`Bob`là các chuỗi khác nhau, do đó việc thay đổi cách viết hoa hoặc chuẩn hóa đầu vào sẽ tạo ra đầu ra không chính xác. 

đầu vào:```
2
bob
Bob
```Đầu ra đúng:```
bob: F
Bob: F
```Một chương trình chuyển đổi tên người dùng thành chữ thường trước khi in sẽ âm thầm làm mất thông tin. 

Thứ tự của bạn bè rất quan trọng. Tên người dùng đầu tiên trong dữ liệu nhập phải tạo tin nhắn trò chuyện đầu tiên, ngay cả khi tên người dùng khác xuất hiện sớm hơn theo thứ tự bảng chữ cái. 

đầu vào:```
3
zack
anna
mike
```Đầu ra đúng:```
zack: F
anna: F
mike: F
```Việc sắp xếp tên người dùng sẽ thay đổi thứ tự cuộc trò chuyện và đưa ra câu trả lời sai. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp là mô phỏng việc tạo tin nhắn trò chuyện. Với mỗi tên người dùng, chương trình tạo một chuỗi mới chứa tên người dùng, dấu phân cách`: `, và nhân vật`F`. Cách tiếp cận này đã hiệu quả vì không cần cấu trúc dữ liệu hoặc thuật toán phức tạp. Mỗi tên người dùng chỉ được đọc và in một lần. 

Lý do điều này có tác dụng là vì bài toán không yêu cầu chúng ta so sánh bạn bè, tìm kiếm các mẫu hoặc sắp xếp lại dữ liệu. Đầu ra chỉ đơn giản là một phép biến đổi xác định của mọi dòng đầu vào. 

Một cách tiếp cận bạo lực không cần thiết phổ biến sẽ lưu trữ từng cặp tên người dùng và so sánh chúng, có thể cố gắng xác minh tính duy nhất hoặc thứ tự. Mặc dù nó vẫn tạo ra kết quả tương tự nhưng nó sẽ thực hiện so sánh khoảng n2. Với 100.000 bạn bè, điều đó có nghĩa là có khoảng 10 tỷ hoạt động, vượt xa giới hạn. 

Quan sát quan trọng là mọi dòng đầu ra chỉ phụ thuộc vào tên người dùng ở cùng một vị trí trong đầu vào. Không có mối quan hệ giữa những người bạn khác nhau. Vì tính độc lập đó nên chúng ta có thể chuyển đổi ngay từng dòng và nối nó vào đáp án. Brute-force hoạt động vì việc chuyển đổi rất đơn giản, nhưng bất kỳ cách tiếp cận nào đưa ra sự so sánh giữa những người dùng đều bỏ qua cấu trúc của vấn đề. Việc quan sát rằng mỗi dòng là độc lập sẽ giảm tác vụ xuống một lần truyền qua đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm đối với đầu vào lớn nhất | 
| Tối ưu | O(n) | O(n) để lưu trữ đầu ra, thêm O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng bạn bè. Điều này cho chúng tôi biết có bao nhiêu tên người dùng phải được xử lý và ngăn việc đọc thông tin đầu vào không liên quan. 
2. Lặp lại chính xác n lần và đọc một tên người dùng. Mỗi tên người dùng đại diện cho một tin nhắn trong cuộc trò chuyện cuối cùng nên nó phải được xử lý ngay lập tức theo cùng một thứ tự. 
3. Nối văn bản`: F`vào tên người dùng và lưu trữ dòng kết quả. Định dạng đã được cố định nên không cần tính toán thêm. 
4. Sau khi tất cả tên người dùng được xử lý, hãy in tất cả các dòng được tạo ra cùng nhau. Việc nối các dòng tránh phải viết đi viết lại nhiều đoạn đầu ra nhỏ. 

Tại sao nó hoạt động: bất biến được duy trì trong quá trình quét là sau khi xử lý k tên người dùng đầu tiên, kết quả được lưu trữ chứa chính xác k tin nhắn trò chuyện đầu tiên theo đúng thứ tự của chúng. Tên người dùng tiếp theo chỉ tạo thông báo độc lập tiếp theo, do đó việc mở rộng đầu ra sẽ duy trì tính chính xác. Sau khi tất cả n tên người dùng được xử lý, các dòng được lưu trữ mô tả chính xác cuộc trò chuyện hoàn chỉnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    ans = []

    for _ in range(n):
        username = input().rstrip("\n")
        ans.append(username + ": F")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Chương trình đọc số lượng tin nhắn trước, sau đó xử lý từng tên người dùng một. Số vòng lặp khớp với số lượng bạn bè, vì vậy mỗi tên người dùng đầu vào sẽ đóng góp chính xác một dòng đầu ra.`rstrip("\n")`chỉ loại bỏ dòng kết thúc được thêm vào bằng cách đọc đầu vào. Nó không loại bỏ khoảng trắng hoặc sửa đổi các ký tự bên trong tên người dùng. Điều này quan trọng vì tên người dùng có thể chứa dấu gạch dưới, chữ số, chữ hoa và chữ thường và chính tả của chúng phải được giữ nguyên. 

Các dòng được tạo sẽ được lưu trữ trong danh sách và được in một lần. Python xử lý một thao tác đầu ra lớn hiệu quả hơn nhiều lệnh in riêng biệt khi n lớn. 

Không có tính toán lập chỉ mục, điều kiện biên hoặc phép toán số học, do đó không có mối lo ngại về lỗi tràn hoặc lỗi một. 

## Ví dụ đã hoạt động 

Đối với mẫu 1: 

| Bước | Tên người dùng đã đọc | Dòng được tạo | Kích thước đầu ra được lưu trữ | 
| --- | --- | --- | --- | 
| 1 | thợ săn2 | thợ săn2: F | 1 | 
| 2 | kevin | kevin: F | 2 | 
| 3 | trả tiền | trả tiền: F | 3 | 
| 4 | alvin | alvin: F | 4 | 
| 5 | BeRtO | BeRtO: F | 5 | 

Dấu vết cho thấy mọi dòng đầu vào đều được chuyển đổi độc lập. Vốn hóa hỗn hợp trong`BeRtO`được giữ nguyên, xác nhận rằng tên người dùng được coi là chuỗi chính xác. 

Đối với Mẫu 2, quy trình tương tự cũng xảy ra với tất cả 26 tên người dùng. 

| Bước | Tên người dùng đã đọc | Dòng được tạo | Kích thước đầu ra được lưu trữ | 
| --- | --- | --- | --- | 
| 1 | bimb | bimb: F | 1 | 
| 2 | kẻ theo dõi không gian | kẻ theo dõi không gian: F | 2 | 
| 3 | đón bé trai | xe bán tải: F | 3 | 
| 26 | Cassandra | Cassandra: F | 26 | 

Dấu vết này chứng tỏ rằng thuật toán không phụ thuộc vào độ dài hoặc nội dung tên người dùng. Tên dài, chữ số và ký tự lặp lại đều được xử lý bằng cùng một phép biến đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi tên người dùng được đọc và chuyển đổi một lần. | 
| Không gian | O(n) | Các dòng đầu ra được lưu trữ trước khi in. | 

Đầu vào tối đa chứa 100.000 tên người dùng, do đó, giải pháp tuyến tính dễ dàng phù hợp với giới hạn thời gian. Việc sử dụng bộ nhớ cũng nhỏ vì mỗi tên người dùng dài tối đa 12 ký tự, khiến đầu ra được lưu trữ chỉ có vài megabyte. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# Sample 1
assert solution("""5
hunter2
kevin
payton
alvin
BeRtO
""") == """hunter2: F
kevin: F
payton: F
alvin: F
BeRtO: F""", "sample 1"

# Sample 2
assert solution("""3
alice
bob
carol
""") == """alice: F
bob: F
carol: F""", "sample order"

# Minimum size
assert solution("""1
A
""") == """A: F""", "single username"

# Special characters allowed in usernames
assert solution("""2
x_1
USER99
""") == """x_1: F
USER99: F""", "username characters"

# Large count pattern
assert solution("4\na\na\na\na\n") == """a: F
a: F
a: F
a: F""", "repeated values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một tên người dùng | Một tin nhắn được định dạng | Xử lý đầu vào tối thiểu | 
| Một số tên người dùng theo thứ tự khác nhau | Thứ tự đầu ra giống nhau | Không sắp xếp hoặc sắp xếp lại | 
| Tên người dùng với`_`và chữ số | Bảo quản chính xác | Xử lý chuỗi đúng | 
| Tên người dùng lặp đi lặp lại | Mọi dòng đều được xử lý | Không có giả định về tính duy nhất | 

## Vỏ cạnh 

Một người bạn được xử lý theo cùng một vòng lặp như mọi trường hợp khác. Đối với đầu vào:```
1
Alice
```Vòng lặp chạy một lần, tạo ra`Alice: F`, và in nó. Không cần nhánh đặc biệt để tránh lỗi do giả định ít nhất hai thông báo. 

Phân biệt chữ hoa chữ thường được giữ nguyên vì chương trình không bao giờ sửa đổi chuỗi tên người dùng. Đối với đầu vào:```
2
bob
Bob
```lần lặp đầu tiên tạo ra`bob: F`và cái thứ hai tạo ra`Bob: F`. Hai đầu ra vẫn khác biệt. 

Thứ tự đầu vào được giữ nguyên do thuật toán xử lý tên người dùng một cách tuần tự và nối thêm từng tin nhắn được tạo ngay lập tức. Vì:```
3
zack
anna
mike
```danh sách được lưu trữ trở thành`["zack: F", "anna: F", "mike: F"]`. Không có bước sắp xếp nào tồn tại nên cuộc trò chuyện cuối cùng khớp với thứ tự cuộc trò chuyện ban đầu.
