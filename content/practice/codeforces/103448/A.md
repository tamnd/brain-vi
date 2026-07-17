---
title: "CF 103448A - \u83ab\u5361\u4e0e MCPC"
description: "Mỗi truy vấn cung cấp một mã trạng thái số nguyên duy nhất được tạo khi người dùng cố gắng truy cập một dịch vụ. Đối với mỗi mã như vậy, hệ thống phải quyết định xem yêu cầu thành công hay thất bại."
date: "2026-07-03T07:25:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103448
codeforces_index: "A"
codeforces_contest_name: "The 16-th Beihang University Collegiate Programming Contest (BCPC 2021) - Preliminary"
rating: 0
weight: 103448
solve_time_s: 49
verified: true
draft: false
---

[CF 103448A - \u83ab\u5361\u4e0e MCPC](https://codeforces.com/problemset/problem/103448/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi truy vấn cung cấp một mã trạng thái số nguyên duy nhất được tạo khi người dùng cố gắng truy cập một dịch vụ. Đối với mỗi mã như vậy, hệ thống phải quyết định xem yêu cầu thành công hay thất bại. Một yêu cầu chỉ được coi là thành công khi số đó thỏa mãn đồng thời hai điều kiện: đó là số nguyên tố và số chẵn. Nếu cả hai điều kiện đều giữ nguyên thì đầu ra là thông báo thành công`OK`, nếu không thì đầu ra là một chuỗi lỗi cố định. 

Đầu vào là một chuỗi các số nguyên độc lập, mỗi số đại diện cho một yêu cầu. Không có sự tương tác giữa các truy vấn, vì vậy mỗi số có thể được xử lý riêng biệt. 

Ràng buộc về các giá trị đủ nhỏ để mỗi số có nhiều nhất là một triệu và có nhiều nhất là một nghìn truy vấn. Điều đó ngay lập tức loại trừ mọi nhu cầu về cấu trúc tính toán trước nặng nề hoặc các hoạt động tốn kém cho mỗi truy vấn. Kiểm tra tính nguyên tố đơn giản cho mỗi số là đủ, vì ngay cả kiểm tra O(√x) lặp lại 1000 lần cũng là nhanh chóng. 

Một điểm tinh tế nằm ở giao điểm của hai điều kiện. Số nguyên tố chẵn duy nhất là 2. Mọi số chẵn khác đều chia hết cho 2 và do đó không phải là số nguyên tố. Điều này có nghĩa là điều kiện “số nguyên tố và số chẵn” được rút gọn thành một trường hợp đặc biệt duy nhất. Việc triển khai bất cẩn để kiểm tra riêng biệt tính nguyên tố và tính đồng đều có thể vẫn đúng, nhưng nó có nguy cơ tính toán không cần thiết trừ khi sự đơn giản hóa này được chú ý. 

Các trường hợp biên xuất hiện khi số đó là 1 hoặc 0, mặc dù các ràng buộc cho biết số nguyên dương bắt đầu từ 1. Đối với x = 1, một phép kiểm tra tính nguyên tố ngây thơ mà quên định nghĩa về số nguyên tố có thể coi nó là số nguyên tố một cách không chính xác, dẫn đến kết quả sai`OK`. Một trường hợp khác là x = 2, đây phải là đầu vào duy nhất được chấp nhận. 

## Phương pháp tiếp cận 

Cách giải thích mạnh mẽ là trực tiếp: đối với mỗi số, hãy kiểm tra xem số đó có phải là số nguyên tố hay không và liệu nó có chia hết cho 2 hay không. Kiểm tra tính nguyên tố thường sẽ thử tất cả các ước số từ 2 đến √x. Điều này đúng vì hợp số phải có hệ số không lớn hơn căn bậc hai của nó. 

Trong trường hợp xấu nhất, mỗi truy vấn yêu cầu khoảng √10^6 ≈ 1000 lần kiểm tra. Với tối đa 1000 truy vấn, điều này dẫn đến khoảng một triệu phép kiểm tra ước số, điều này vẫn không đáng kể trong giới hạn 1 giây trong Python. Vì vậy, ngay cả cách tiếp cận ngây thơ cũng đã trôi qua một cách thoải mái. 

Quan sát quan trọng là điều kiện chẵn và nguyên tố cực kỳ hạn chế. Vì tất cả các số chẵn lớn hơn 2 đều là hợp số nên yêu cầu về tính nguyên tố ngay lập tức loại bỏ mọi số chẵn ngoại trừ 2. Điều này làm giảm vấn đề xuống mức so sánh theo thời gian không đổi cho mỗi truy vấn. 

Vì vậy, thay vì thực hiện kiểm tra tính nguyên tố, chúng tôi chỉ kiểm tra sự bằng nhau với 2. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra tính nguyên thủy của Brute Force | O(n√x) | O(1) | Đã chấp nhận | 
| Kiểm tra liên tục (x == 2) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng truy vấn một cách độc lập. 

1. Đọc số nguyên x cho truy vấn hiện tại. Đây là mã trạng thái chúng ta phải phân loại. 
2. So sánh x với 2. Nếu nó bằng, xuất ra`OK`. 
3. Nếu không thì xuất ra thông báo lỗi. 

Lý do bước 2 là đủ xuất phát từ cấu trúc của số nguyên tố. Bất kỳ số chẵn nào lớn hơn 2 đều chia hết cho 2 và do đó không phải là số nguyên tố. Bất kỳ số nguyên tố lẻ nào cũng không phải là số chẵn. Điều này để lại chính xác một ứng cử viên hợp lệ. 

### Tại sao nó hoạt động 

Thuật toán dựa trên bất biến rằng số nguyên duy nhất thỏa mãn cả “số nguyên tố” và “chẵn” là 2. Đây là hệ quả trực tiếp của định nghĩa số nguyên tố và tính chia hết: số chẵn có thừa số là 2 và số nguyên tố không thể có bất kỳ thừa số không tầm thường nào. Vì 2 là số nguyên tố và chẵn nên nó là điểm cố định duy nhất của cả hai điều kiện. Mọi số nguyên khác đều không đạt ít nhất một điều kiện, vì vậy việc kiểm tra tính bằng nhau là cần thiết và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    for _ in range(n):
        x = int(input())
        if x == 2:
            print("OK")
        else:
            print("An invalid response was received from the upstream server")

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng giá trị và phân loại ngay lập tức bằng cách sử dụng một so sánh duy nhất. Không cần chức năng trợ giúp hoặc tính toán trước. 

Chi tiết triển khai quan trọng là bảo toàn chuỗi đầu ra chính xác cho các trường hợp lỗi vì nó dài và phải khớp chính xác, bao gồm cả khoảng trắng. Bất kỳ sai lệch nào như ngắt dòng hoặc thiếu khoảng trống sẽ dẫn đến câu trả lời sai. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào có ba yêu cầu được thực hiện với các giá trị 2, 3 và 4. 

Đối với mỗi bước, chúng tôi theo dõi quá trình ra quyết định. 

| x | x == 2 | Đầu ra | 
| --- | --- | --- | 
| 2 | Đúng | được | 
| 3 | Sai | thất bại | 
| 4 | Sai | thất bại | 

Trường hợp đầu tiên thành công vì 2 thỏa mãn cả hai điều kiện. Lần thứ hai thất bại vì 3 không chẵn. Lần thứ ba thất bại vì 4 là số chẵn nhưng không phải là số nguyên tố. 

Điều này chứng tỏ rằng tính nguyên thủy không còn phù hợp một khi chúng ta nhận ra ràng buộc về cấu trúc. 

Bây giờ hãy xem xét một đầu vào khác có giá trị 1, 2, 5, 10. 

| x | x == 2 | Đầu ra | 
| --- | --- | --- | 
| 1 | Sai | thất bại | 
| 2 | Đúng | được | 
| 5 | Sai | thất bại | 
| 10 | Sai | thất bại | 

Điều này nhấn mạnh rằng 1 phải bị bác bỏ, vì nó không phải là số nguyên tố cũng không phải số chẵn, đồng thời củng cố rằng không có số nào ngoài 2 có thể thỏa mãn cả hai điều kiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi truy vấn được xử lý bằng một lần kiểm tra đẳng thức duy nhất | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc cho phép tối đa 1000 truy vấn, do đó, việc quét tuyến tính với công việc liên tục trên mỗi truy vấn là không đáng kể trong giới hạn. Việc sử dụng bộ nhớ không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    n = int(input())
    for _ in range(n):
        x = int(input())
        if x == 2:
            output.append("OK")
        else:
            output.append("An invalid response was received from the upstream server")
    return "\n".join(output)

# provided sample-style tests
assert run("3\n2\n3\n4\n") == "OK\nAn invalid response was received from the upstream server\nAn invalid response was received from the upstream server"

# minimum size
assert run("1\n2\n") == "OK"

# all failing
assert run("4\n1\n3\n5\n7\n") == "An invalid response was received from the upstream server\nAn invalid response was received from the upstream server\nAn invalid response was received from the upstream server\nAn invalid response was received from the upstream server"

# boundary mix
assert run("5\n2\n10\n2\n4\n2\n") == "OK\nAn invalid response was received from the upstream server\nOK\nAn invalid response was received from the upstream server\nOK"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giá trị nhỏ hỗn hợp | hỗn hợp | tính đúng đắn của số nguyên tố/không nguyên tố/chẵn/không chẵn | 
| đơn 2 | được | trường hợp vượt qua tối thiểu | 
| tất cả tỷ lệ cược | mọi thất bại | bác bỏ các số nguyên tố không chẵn | 
| xen kẽ 2 và những người khác | xen kẽ | tính nhất quán trên nhiều truy vấn | 

## Vỏ cạnh 

Với x = 1, thuật toán kiểm tra sự bằng nhau với 2 và đưa ra lỗi ngay lập tức. Điều này phù hợp với tính chính xác vì 1 không phải là số nguyên tố. Việc triển khai tính nguyên thủy đơn giản phải từ chối rõ ràng 1, nếu không nó có thể vượt qua sai. 

Với x = 2, việc kiểm tra đẳng thức thành công và xuất ra`OK`. Đây là trường hợp hợp lệ duy nhất và nó khẳng định rằng phép đơn giản hóa không bỏ sót bất kỳ ứng viên bổ sung nào. 

Đối với mọi x > 2 chẵn, chẳng hạn như 4 hoặc 10, việc kiểm tra đẳng thức không thành công và thuật toán đưa ra kết quả không thành công. Điều này phù hợp chính xác với thực tế là những số như vậy không thể là số nguyên tố do chia hết cho 2. 

Đối với các số nguyên tố lẻ như 3, 5 hoặc 7, thuật toán lại đưa ra lỗi vì chúng không đạt được điều kiện chẵn. Điều này khẳng định rằng chỉ tính nguyên sơ không bao giờ là đủ để thành công.
