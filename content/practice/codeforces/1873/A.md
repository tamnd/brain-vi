---
title: "CF 1873A - Sắp xếp ngắn"
description: "Chúng ta được phát cho ba tấm thẻ riêng biệt có nhãn a, b và c, được sắp xếp theo thứ tự nào đó. Mục tiêu của chúng ta là xác định xem liệu bằng cách sử dụng nhiều nhất một lần hoán đổi hai lá bài, chúng ta có thể chuyển đổi hàng thành chuỗi có thứ tự abc hay không."
date: "2026-06-08T23:12:44+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1873
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 898 (Div. 4)"
rating: 800
weight: 1873
solve_time_s: 79
verified: true
draft: false
---

[CF 1873A - Sắp xếp ngắn](https://codeforces.com/problemset/problem/1873/A) 

**Đánh giá:** 800 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp ba thẻ riêng biệt được dán nhãn`a`,`b`, Và`c`, sắp xếp theo một thứ tự nào đó. Mục tiêu của chúng tôi là xác định xem liệu bằng cách sử dụng nhiều nhất một lần hoán đổi hai thẻ, chúng tôi có thể chuyển đổi hàng thành chuỗi có thứ tự hay không`abc`. Mỗi trường hợp thử nghiệm bao gồm một chuỗi có độ dài ba chứa chính xác ba chữ cái này trong một số hoán vị. Đầu ra cho mỗi trường hợp là "CÓ" nếu có thể lấy được trình tự đã đặt hàng và "KHÔNG" nếu ngược lại. 

Vì chỉ có ba phần tử nên tổng số hoán vị là sáu. Điều này có nghĩa là chúng ta có thể giải quyết vấn đề một cách thấu đáo, nhưng chúng ta cũng nên hiểu các điều kiện chính xác mà theo đó một lần hoán đổi là đủ. Việc hoán đổi sẽ sửa một cặp bị đặt sai vị trí hoặc không cần thiết nếu chuỗi đã được`abc`. Các ràng buộc rất nhỏ: với tối đa sáu trường hợp thử nghiệm, mỗi trường hợp chỉ liên quan đến ba chữ cái, mọi cách tiếp cận hợp lý sẽ chạy trong thời gian không đáng kể. Các trường hợp cạnh bao gồm chuỗi đã được sắp xếp (`abc`), các chuỗi không thể sửa được bằng một lần hoán đổi (`bca`hoặc`cab`) và các chuỗi cách nhau một lần hoán đổi (`acb`,`bac`,`cba`). 

Việc triển khai ngây thơ có thể cho rằng việc hoán đổi hai chữ cái bất kỳ luôn là đủ. Ví dụ,`bca`không thể trở thành`abc`chỉ với một lần hoán đổi duy nhất. Chỉ kiểm tra số lượng chữ cái bị đặt sai vị trí sẽ dẫn đến kết quả dương tính giả ở đây. Thuật toán phải xem xét vị trí của từng ký tự so với mục tiêu`abc`. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử rõ ràng tất cả các cặp vị trí có thể hoán đổi và sau đó kiểm tra xem kết quả có khớp không`abc`. Với ba chữ cái, có ba khả năng hoán đổi: vị trí 0 và 1, vị trí 0 và 2, vị trí 1 và 2. Sau mỗi lần hoán đổi, chúng ta so sánh chuỗi với`abc`. Nếu bất kỳ sự hoán đổi nào mang lại mục tiêu, chúng tôi sẽ in "CÓ"; nếu không thì "KHÔNG". Điều này đúng vì chúng tôi bao gồm tất cả các hoạt động hoán đổi đơn có thể có và nó chạy trong thời gian không đổi với kích thước đầu vào cố định. Hạn chế là cách tiếp cận này không mở rộng sang các mảng lớn hơn, nhưng ở đây nó có thể chấp nhận được. 

Quan sát quan trọng để đơn giản hóa hơn nữa là hoán vị của ba chữ cái tối đa là một hoán đổi từ`abc`nếu và chỉ nếu nó đã có rồi`abc`hoặc nó có chữ cái đầu tiên và cuối cùng được hoán đổi (`acb`,`bac`,`cba`). trình tự`bca`Và`cab`yêu cầu hai lần hoán đổi và không thể sửa được trong một thao tác. Bằng cách kiểm tra chuỗi dựa trên các mẫu này, chúng ta có thể quyết định câu trả lời trong một so sánh duy nhất, hiệu quả là O(1) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả các giao dịch hoán đổi) | O(1) | O(1) | Đã chấp nhận | 
| Kiểm tra mẫu (kiểm tra các hoán vị có thể giải được) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Đối với mỗi trường hợp thử nghiệm, hãy đọc chuỗi ba ký tự đại diện cho chuỗi thẻ. 
3. Kiểm tra xem chuỗi có chính xác không`abc`. Nếu vậy, hãy in "CÓ" và tiếp tục. 
4. Mặt khác, hãy kiểm tra xem chuỗi có khớp với một trong ba hoán vị có thể trở thành`abc`với một lần trao đổi:`acb`,`bac`,`cba`. Nếu có, hãy in "CÓ". 
5. Nếu không đáp ứng được điều kiện nào, hãy in "KHÔNG". Đây là những trình tự`bca`Và`cab`, yêu cầu hai lần hoán đổi. 
6. Lặp lại các bước 2-5 cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: Đối với hoán vị ba phần tử, bất kỳ chuỗi nào đã khớp với mục tiêu đều có thể được sửa bằng cách hoán đổi chính xác hai phần tử hoặc yêu cầu nhiều hơn một lần hoán đổi. Ba mẫu chúng tôi kiểm tra bao gồm tất cả các trường hợp có thể giải quyết được bằng một lần hoán đổi, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = input().strip()
    if s == "abc" or s in {"acb", "bac", "cba"}:
        print("YES")
    else:
        print("NO")
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi đọc chuỗi, loại bỏ mọi khoảng trắng và so sánh nó với mục tiêu`abc`. Nếu chuỗi khớp`abc`hoặc là một trao đổi đi (`acb`,`bac`,`cba`), chúng tôi in "CÓ". Nếu không, chúng tôi in "KHÔNG". Việc sử dụng một bộ mô hình một lần hoán đổi cho phép kiểm tra tư cách thành viên hiệu quả. 

## Ví dụ đã hoạt động 

Hãy xem xét chuỗi đầu vào`acb`. 

| Bước | s | Kiểm tra | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | "acb" | s == "abc"? Không | - | 
| 2 | "acb" | s trong {"acb", "bac", "cba"}? Có | CÓ | 

Chúng tôi phát hiện ra điều đó`acb`có thể trở thành`abc`bằng cách hoán đổi hai ký tự cuối cùng. 

Coi như`bca`: 

| Bước | s | Kiểm tra | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | "bca" | s == "abc"? Không | - | 
| 2 | "bca" | s trong {"acb", "bac", "cba"}? Không | KHÔNG |`bca`yêu cầu hai lần hoán đổi, do đó thuật toán sẽ đưa ra kết quả "KHÔNG" một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm bao gồm một số lượng so sánh chuỗi không đổi và t 6. | 
| Không gian | O(1) | Chỉ có chuỗi đầu vào và một tập hợp mẫu nhỏ được lưu trữ. | 

Giải pháp phù hợp tốt trong các ràng buộc của vấn đề. Mỗi thao tác đều tầm thường và việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution
    t = int(input())
    for _ in range(t):
        s = input().strip()
        if s == "abc" or s in {"acb", "bac", "cba"}:
            print("YES")
        else:
            print("NO")
    return output.getvalue().strip()

# provided samples
assert run("6\nabc\nacb\nbac\nbca\ncab\ncba\n") == "YES\nYES\nYES\nNO\nNO\nYES", "sample 1"

# custom cases
assert run("2\nabc\nbca\n") == "YES\nNO", "already sorted and impossible"
assert run("1\ncba\n") == "YES", "reverse order solvable by one swap"
assert run("1\ncab\n") == "NO", "cannot solve in one swap"
assert run("3\nacb\nbac\nabc\n") == "YES\nYES\nYES", "all one-swap or sorted cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| abc | CÓ | Trình tự đã được sắp xếp | 
| bca | KHÔNG | Yêu cầu hai lần hoán đổi, không thể | 
| cba | CÓ | Thứ tự đảo ngược, có thể giải quyết bằng một lần hoán đổi | 
| taxi | KHÔNG | Không thể thực hiện được giải pháp một lần hoán đổi | 
| acb, bac, abc | CÓ | Xác nhận phát hiện các mẫu một lần hoán đổi | 

## Vỏ cạnh 

Thuật toán xử lý trường hợp cạnh của chuỗi đã được sắp xếp`abc`bằng cách kiểm tra rõ ràng sự bình đẳng trước tiên. Vì`bca`Và`cab`, thuật toán xác định chính xác rằng không có hoán đổi đơn lẻ nào có thể sửa được chuỗi, xuất ra "KHÔNG". Vì`cba`, bị đảo ngược, thuật toán sẽ phát hiện nó dưới dạng trường hợp một lần hoán đổi, xuất ra "CÓ". Quyết định sử dụng một tập hợp cho các hoán vị một lần hoán đổi đảm bảo rằng tất cả các hoán vị có thể giải được đều được ghi lại mà không bỏ sót bất kỳ cấu hình hợp lệ nào.
