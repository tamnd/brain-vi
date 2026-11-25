---
title: "CF 991A - Nếu lúc đầu bạn không thành công..."
description: "Chúng tôi được cung cấp số liệu thống kê tóm tắt về cách ứng xử của học sinh trong một nhóm sau một kỳ thi. Mỗi học sinh thuộc đúng một trong bốn loại: họ chỉ ghé thăm BugDonalds, chỉ BeaverKing, cả hai nhà hàng, hoặc ở nhà vì thi trượt."
date: "2026-06-17T00:27:04+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 991
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 491 (Div. 2)"
rating: 1000
weight: 991
solve_time_s: 74
verified: true
draft: false
---

[CF 991A - Nếu lần đầu bạn không thành công...](https://codeforces.com/problemset/problem/991/A) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp số liệu thống kê tóm tắt về cách ứng xử của học sinh trong một nhóm sau một kỳ thi. Mỗi học sinh thuộc đúng một trong bốn loại: họ chỉ ghé thăm BugDonalds, chỉ BeaverKing, cả hai nhà hàng, hoặc ở nhà vì thi trượt. 

Từ dữ liệu đầu vào, chúng tôi được biết tổng cộng có bao nhiêu sinh viên đã ghé thăm BugDonalds, tổng cộng bao nhiêu sinh viên đã ghé thăm BeaverKing, bao nhiêu sinh viên đã ghé thăm cả hai địa điểm và tổng số sinh viên trong nhóm. Nhiệm vụ là kiểm tra xem liệu những số liệu thống kê này có thể đến từ một số phân bố hợp lệ nào đó về học sinh hay không, và nếu có thể, hãy tính xem có bao nhiêu học sinh thi trượt và phải ở nhà. 

Cấu trúc ẩn chính là số lượng nhà hàng không rời rạc. Giá trị đại diện cho “cả hai nhà hàng” được tính trong cả hai tổng số, vì vậy số lượng khách truy cập thực tế phải được xây dựng lại một cách cẩn thận. Khi chúng tôi biết có bao nhiêu sinh viên đã ghé thăm ít nhất một nhà hàng, những người còn lại phải ở nhà. 

Các ràng buộc rất nhỏ, tất cả các giá trị tối đa là 100, điều này ngay lập tức cho chúng ta biết rằng mọi giải pháp chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm là đủ. Ngay cả việc liệt kê mạnh mẽ trên tất cả các phân bố của bốn loại giới hạn bởi 100 cũng có thể khả thi, nhưng không cần thiết. 

Các trường hợp thất bại tinh tế xuất phát từ sự không nhất quán giữa sự chồng chéo và tổng số. Một lỗi phổ biến là quên rằng sự trùng lặp được bao gồm trong cả A và B, dẫn đến việc đếm âm hoặc đếm quá mức không thể thực hiện được. 

Một số tình huống không hợp lệ cụ thể giúp làm rõ: 

Nếu A = 1, B = 1, C = 1, N = 1 thì cả A và B đều ngụ ý rằng có ít nhất một sinh viên tồn tại trong mỗi nhà hàng, nhưng sự trùng lặp sẽ dẫn đến mâu thuẫn vì một sinh viên không thể đồng thời đáp ứng tất cả các số lượng trừ khi được căn chỉnh cẩn thận. 

Nếu C lớn hơn A hoặc B, chẳng hạn như A = 2, B = 2, C = 3, thì điều này ngay lập tức không thể thực hiện được vì “cả hai” không thể vượt quá “tổng số”. 

Nếu A + B − C vượt quá N, thì ngay cả trước khi tính đến sự trùng lặp, chúng ta đã cần nhiều học sinh hơn số học sinh tồn tại trong nhóm. 

Những mâu thuẫn này là những gì thuật toán phải phát hiện. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ cố gắng gán mỗi học sinh trong số N học sinh vào một trong bốn trạng thái và kiểm tra xem liệu chúng ta có thể khớp chính xác các số lượng A, B và C được yêu cầu hay không. Điều này sẽ liên quan đến việc lặp lại tất cả các phân chia có thể có của N thành bốn nhóm và xác minh các ràng buộc. Số cách phân chia N thành bốn số nguyên không âm theo thứ tự N³, vì việc sửa ba sẽ tự động xác định số thứ tư. Mặc dù N ≤ 100 làm cho điều này hầu như không thể chấp nhận được, nhưng nó lại phức tạp không cần thiết đối với bài toán tái thiết số học trực tiếp về cơ bản. 

Quan sát quan trọng là cấu trúc chồng chéo hoàn toàn xác định phân vùng. Gọi x chỉ là BugDonalds, y chỉ BeaverKing, z cả hai nhà hàng và w ở nhà. Khi đó A = x + z, B = y + z và C = z. Điều này ngay lập tức cho x = A − C và y = B − C. Sau khi cố định, tổng số học sinh đã ghé thăm ít nhất một nhà hàng là x + y + z = A + B − C. Số học sinh còn lại là w = N − (A + B − C). 

Toàn bộ vấn đề nằm ở việc kiểm tra xem các giá trị dẫn xuất này có phải là số nguyên không âm hay không và liệu chúng có tổng nhất quán bằng N hay không. Bất kỳ tín hiệu vi phạm nào cũng không thể được đưa vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N³) | O(1) | Quá chậm/không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng lại bốn loại từ tổng số đã cho bằng cách sử dụng đại số và sau đó xác nhận tính khả thi.

1. Tính số sinh viên đã ghé thăm cả hai nhà hàng, ký hiệu là z = C. Số này được cho trực tiếp nên không cần suy luận. 
2. Tính x = A − C, số học sinh chỉ ghé thăm BugDonalds. Điều này sẽ loại bỏ những người được tính trong cả hai khỏi tổng số khách truy cập BugDonalds. 
3. Tính y = B − C, số học sinh chỉ đến thăm BeaverKing. Đây là lý luận đối xứng được áp dụng cho nhà hàng thứ hai. 
4. Tính số học sinh đã ghé thăm ít nhất một nhà hàng là x + y + z. Điều này đơn giản hóa thành A + B − C, vì z triệt tiêu chính xác. 
5. Tính số học sinh nghỉ học là w = N − (A + B − C). Điều này đại diện cho những người đã thất bại trong kỳ thi. 
6. Kiểm tra điều kiện hợp lệ: x, y, z, w đều không âm. Nếu bất kỳ số âm nào thì số đếm mâu thuẫn với thực tế và chúng tôi xuất ra −1. 
7. Nếu hợp lệ, ghi w là số học sinh trượt. 

Tại sao nó hoạt động: việc phân tách thành bốn nhóm rời rạc bị ép buộc bởi sự bao gồm-loại trừ. Mỗi học sinh được phân loại duy nhất và các công thức là cách duy nhất để đáp ứng tổng số của cả hai nhà hàng cùng một lúc. Bất kỳ vi phạm nào về tính không tiêu cực đều tương ứng với một cấu trúc chồng chéo không thể có hoặc sự không phù hợp giữa tổng quy mô nhóm và sự tham gia được tái cấu trúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    A, B, C, N = map(int, input().split())

    x = A - C
    y = B - C
    z = C

    if x < 0 or y < 0:
        print(-1)
        return

    total_attended = x + y + z  # equals A + B - C
    w = N - total_attended

    if w < 0:
        print(-1)
        return

    print(w)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau các biến được xây dựng lại. Điểm tinh tế duy nhất là đảm bảo rằng phép trừ không âm thầm tạo ra số âm không hợp lệ. Chúng tôi bảo vệ rõ ràng x và y trước khi tiếp tục, vì chúng đại diện cho số lượng học sinh thực tế và không thể âm. Lần kiểm tra cuối cùng đảm bảo rằng số học sinh tham dự được xây dựng lại không vượt quá quy mô nhóm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

A = 10, B = 10, C = 5, N = 20 

Chúng tôi tính toán: 

| Bước | x | y | z | tổng_tham dự | w | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | - | - | - | - | - | 
| Tính toán chồng chéo | - | - | 5 | - | - | 
| Chỉ BugDonalds | 10−5 = 5 | - | 5 | - | - | 
| Chỉ BeaverKing | 5 | 10−5 = 5 | 5 | - | - | 
| Tổng số lượt truy cập | 5 | 5 | 5 | 15 | - | 
| Trang chủ | 5 | 5 | 5 | 15 | 20−15 = 5 | 

Kết quả là hợp lệ vì tất cả các nhóm được xây dựng lại đều không âm và tổng số khớp với kích thước nhóm. Kết quả là 5, nghĩa là có 5 học sinh ở nhà. 

### Ví dụ 2 

đầu vào: 

A = 2, B = 2, C = 2, N = 3 

| Bước | x | y | z | tổng_tham dự | w | 
| --- | --- | --- | --- | --- | --- | 
| Chồng chéo | - | - | 2 | - | - | 
| Chỉ BugDonalds | 2−2 = 0 | - | 2 | - | - | 
| Chỉ BeaverKing | 0 | 2−2 = 0 | 2 | - | - | 
| Tổng số lượt truy cập | 0 | 0 | 2 | 2 | - | 
| Trang chủ | 0 | 0 | 2 | 2 | 3−2 = 1 | 

Điều này hợp lệ và mang lại một học sinh ở nhà. Thay vào đó, nếu N là 1, chúng ta sẽ nhận được w âm, ngay lập tức chứng tỏ sự mâu thuẫn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học không đổi được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc cho phép bất kỳ giải pháp số học theo thời gian không đổi nào và việc tái cấu trúc này phù hợp thoải mái trong các giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    A, B, C, N = map(int, sys.stdin.readline().split())

    x = A - C
    y = B - C
    z = C

    if x < 0 or y < 0:
        return "-1"

    total_attended = x + y + z
    w = N - total_attended

    if w < 0:
        return "-1"

    return str(w)

# provided sample
assert run("10 10 5 20\n") == "5"

# all stayed home
assert run("0 0 0 5\n") == "5"

# impossible overlap
assert run("2 2 3 5\n") == "-1"

# impossible due to group size
assert run("10 10 0 5\n") == "-1"

# exact full attendance
assert run("3 3 3 3\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 0 5 | 5 | toàn bộ học sinh trượt | 
| 2 2 3 5 | -1 | chồng chéo không hợp lệ (C > A,B) | 
| 10 10 0 5 | -1 | số người tham dự vượt quá quy mô nhóm | 
| 3 3 3 3 | 0 | mọi người tham dự, không có ai ở nhà | 

## Vỏ cạnh 

Khi A, B và C đều bằng 0 với N dương, thuật toán sẽ tính x = 0, y = 0, z = 0 và Total_attends = 0, do đó w = N. Điều này tương ứng với tất cả học sinh ở nhà, hợp lệ và được xử lý chính xác vì không kích hoạt kiểm tra tiêu cực. 

Khi C vượt quá A hoặc B, chẳng hạn như A = 2, B = 1, C = 3, phép tính tạo ra x = −1 hoặc y = −2. Thuật toán ngay lập tức loại bỏ trường hợp này trước khi tính tổng, xác định chính xác rằng phần trùng lặp không thể vượt quá tổng danh mục. 

Khi A + B − C vượt quá N, chẳng hạn như A = 10, B = 10, C = 0, N = 15, chúng ta nhận được Total_attends = 20 và w = −5. Kết quả tiêu cực này trực tiếp báo hiệu sự không thể thực hiện được, phản ánh rằng số lượng người tham dự được xây dựng lại vượt quá quy mô nhóm.
