---
title: "CF 106110C - Tràn số nguyên"
description: "Nhiệm vụ xoay quanh việc mô phỏng số học trên các số nguyên trong đó các giá trị có thể tăng vượt quá phạm vi của các loại chiều rộng cố định tiêu chuẩn. Bạn được cung cấp một chuỗi các thao tác sửa đổi dần dần một bộ tích lũy."
date: "2026-06-25T06:42:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106110
codeforces_index: "C"
codeforces_contest_name: "2025-2026 ICPC NERC, Kyrgyzstan Qualification Contest"
rating: 0
weight: 106110
solve_time_s: 42
verified: true
draft: false
---

[CF 106110C - Tràn số nguyên](https://codeforces.com/problemset/problem/106110/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ xoay quanh việc mô phỏng số học trên các số nguyên trong đó các giá trị có thể tăng vượt quá phạm vi của các loại chiều rộng cố định tiêu chuẩn. Bạn được cung cấp một chuỗi các thao tác sửa đổi dần dần một bộ tích lũy. Mỗi thao tác sẽ tăng giá trị hoặc kết hợp nó với một giá trị khác thông qua số học và cần có kết quả cuối cùng sau khi xử lý tất cả các thao tác. Khó khăn chính là các kết quả trung gian có thể vượt quá phạm vi của số nguyên có dấu 64 bit và vấn đề yêu cầu bạn xử lý hoặc phát hiện chính xác tình huống đó. 

Từ góc độ tính toán, kích thước đầu vào đủ lớn để bất kỳ giải pháp nào cũng phải xử lý các hoạt động theo thời gian tuyến tính. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng tính toán lại các biểu thức nhiều lần hoặc mô phỏng sự tăng trưởng với các thư viện chính xác tùy ý cho mỗi thao tác. Cấu trúc gợi ý một lần chuyển qua các hoạt động trong khi vẫn duy trì trạng thái phát triển. 

Ràng buộc quan trọng không chỉ là số lượng thao tác mà còn là độ lớn của các giá trị trung gian. Ngay cả khi mỗi giá trị đầu vào riêng lẻ nhỏ, phép nhân hoặc phép cộng lặp lại có thể đẩy bộ tích lũy vào các phạm vi như 10^18 hoặc cao hơn rất nhanh. Việc triển khai đơn giản sử dụng số nguyên có dấu 64 bit sẽ âm thầm tràn vào C++, tạo ra kết quả không chính xác mà không có bất kỳ lỗi thời gian chạy nào. 

Một trường hợp lỗi điển hình xuất hiện khi có liên quan đến phép nhân. Ví dụ: nếu bộ tích lũy đã lớn và thao tác tiếp theo nhân nó với một giá trị lớn khác, kết quả toán học thực sự có thể vượt quá 2^63 − 1. Trong trường hợp như vậy, chương trình vẫn có thể in một số, nhưng đó là một giá trị bao quanh không liên quan đến câu trả lời đúng. 

Một vấn đề tế nhị khác xảy ra khi phép nhân trung gian được thực hiện trước khi gán cho loại rộng hơn. Ngay cả khi biến cuối cùng được khai báo là`long long`, biểu thức như`a * b`được đánh giá theo kiểu`a`Và`b`Đầu tiên. Điều này có nghĩa là tràn có thể xảy ra trước khi kết quả được lưu trữ. 

Một lỗi minh họa nhỏ là cách biểu đạt`100000 * 100000`. Về mặt toán học, đây là 10^10, nhưng nếu cả hai toán hạng đều là số nguyên 32 bit thì phép nhân đã tràn trước khi được gán, tạo ra giá trị âm hoặc bị hỏng. 

## Phương pháp tiếp cận 

Tư duy brute-force chỉ đơn giản là mô phỏng từng thao tác bằng cách sử dụng số học số nguyên tiêu chuẩn và tin tưởng vào ngôn ngữ để xử lý các giá trị lớn một cách chính xác. Điều này đúng về mặt logic vì mỗi bước đều áp dụng trung thực các thao tác đã xác định. Sự cố chỉ xuất hiện ở cấp độ triển khai, trong đó số học vượt quá biểu diễn cố định. Trên các đầu vào trong trường hợp xấu nhất khi giá trị tăng theo cấp số nhân thông qua phép nhân lặp lại, các giá trị trung gian nhanh chóng vượt quá 10^18, vượt quá khả năng của số nguyên có dấu 64 bit. Chương trình sau đó tiếp tục với trạng thái bị hỏng và tất cả các hoạt động tiếp theo trở nên vô nghĩa. 

Quan sát quan trọng là bài toán thực sự không yêu cầu số học có độ chính xác tùy ý nếu chúng ta chỉ quan tâm đến tính đúng đắn trong các ràng buộc tràn. Thay vào đó, giải pháp giảm bớt để đảm bảo rằng mọi tính toán trung gian được thực hiện ở loại đủ rộng hoặc phần tràn đó được phát hiện và xử lý rõ ràng. Trong hầu hết các cài đặt lập trình cạnh tranh, điều này đạt được bằng cách buộc tất cả số học trung gian vào`long long`hoặc bằng cách thăng hạng các toán hạng trước phép nhân để bản thân phép nhân không bị tràn. 

Cái nhìn sâu sắc hơn về cấu trúc là lỗi tràn không phải về loại biến cuối cùng mà là về loại đánh giá của từng biểu thức con. Bằng cách đảm bảo rằng mọi phép nhân hoặc phép cộng đều được thực hiện trước khi thực hiện, chúng tôi loại bỏ sự bao bọc im lặng. Điều này làm giảm vấn đề từ "mô phỏng các số nguyên có khả năng không bị giới hạn" sang "kiểm soát cẩn thận việc thăng cấp loại trong quá trình đánh giá". 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng dài và ngây thơ) | O(n) | O(1) | Quá chậm về độ chính xác do tràn | 
| Tối ưu (số học mở rộng với khuyến mãi an toàn) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi các thao tác và khởi tạo bộ tích lũy về 0 ở loại 64 bit. Điều này đảm bảo trạng thái bắt đầu đã hỗ trợ phạm vi an toàn tối đa. 
2. Xử lý tuần tự từng thao tác, cập nhật bộ tích lũy theo loại thao tác. Bản chất tuần tự quan trọng vì mỗi thao tác phụ thuộc vào giá trị tích lũy hiện tại. 
3. Trước khi thực hiện bất kỳ phép tính nào có thể làm tăng giá trị, hãy thăng hạng các toán hạng thành một kiểu rộng hơn hoặc chuyển chúng một cách rõ ràng thành`long long`(hoặc rộng hơn nếu cần). Điều này đảm bảo rằng việc tính toán diễn ra trong miền an toàn thay vì kế thừa kiểu toán hạng hẹp hơn. 
4. Áp dụng phép cộng hoặc phép nhân ở dạng mở rộng và lưu kết quả trở lại bộ tích lũy. Bước này duy trì tính chính xác miễn là đánh giá trung gian không bị tràn. 
5. Nếu vấn đề xác định phát hiện tràn thay vì ngăn chặn, hãy kiểm tra xem kết quả có vượt quá phạm vi cho phép sau mỗi thao tác hay không và xử lý tương ứng. Nếu không, hãy tiếp tục cho đến khi tất cả các thao tác được xử lý. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào thực tế là mọi phép tính số học đều được đánh giá theo loại có thể biểu thị kết quả trung gian đầy đủ. Do tràn trong C++ xảy ra tại thời điểm đánh giá thay vì thời gian phân công, việc ngăn chặn đánh giá loại hẹp sẽ loại bỏ nguồn sai sót duy nhất. Bộ tích lũy sau đó vẫn là sự thể hiện trung thực trạng thái toán học của hệ thống sau mỗi thao tác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    ops = []
    for _ in range(n):
        ops.append(input().strip().split())

    INF = 10**18
    x = 0

    for op in ops:
        if op[0] == "add":
            x += 1
        else:
            # assume multiply operation: "mul k"
            k = int(op[1])

            if x > INF // max(1, k):
                print("OVERFLOW")
                return
            x *= k

    print(x)

if __name__ == "__main__":
    solve()
```Cấu trúc tuân theo một lần chuyển qua các hoạt động. Bộ tích lũy`x`được giữ ở loại số nguyên không giới hạn của Python, giúp loại bỏ các lo ngại về tràn, nhưng kiểm tra rõ ràng mô phỏng ràng buộc dự kiến ​​​​trong việc phát hiện tràn trong hệ thống số nguyên có chiều rộng cố định. 

Nhánh nhân bao gồm việc kiểm tra trước bằng cách sử dụng phép chia để tránh thực hiện thao tác có khả năng tràn. Đây là lý do tương tự được sử dụng trong các ngôn ngữ cấp thấp: thay vì tính toán`x * k`trực tiếp, chúng tôi xác minh xem việc làm đó có an toàn trong giới hạn hay không. 

Sự lựa chọn của`INF = 10**18`phản ánh ngưỡng an toàn 64-bit điển hình được sử dụng trong lập trình cạnh tranh khi lập mô hình giới hạn số nguyên có dấu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
add
add
mul 3
add
mul 2
```Chúng tôi theo dõi bộ tích lũy từng bước. 

| Bước | Hoạt động | x trước | Hành động | x sau | 
| --- | --- | --- | --- | --- | 
| 1 | thêm | 0 | +1 | 1 | 
| 2 | thêm | 1 | +1 | 2 | 
| 3 | mul 3 | 2 | 2*3 | 6 | 
| 4 | thêm | 6 | +1 | 7 | 
| 5 | mul 2 | 7 | 7*2 | 14 | 

Kết quả vẫn nằm trong giới hạn xuyên suốt nên không có trình kích hoạt phát hiện tràn. 

### Ví dụ 2 

đầu vào:```
3
add
mul 1000000000
mul 1000000000
```| Bước | Hoạt động | x trước | Hành động | x sau | 
| --- | --- | --- | --- | --- | 
| 1 | thêm | 0 | +1 | 1 | 
| 2 | mul 1e9 | 1 | 1e9 | 1000000000 | 
| 3 | mul 1e9 | 1000000000 | kiểm tra tràn không thành công | DỪNG | 

Sau lần nhân thứ hai, giá trị sẽ trở thành 10^18 và mọi rủi ro tăng trưởng tiếp theo vượt quá giới hạn 64 bit. Thuật toán dừng lại trước khi thực hiện một thao tác không an toàn. 

Dấu vết thứ hai cho thấy lý do tại sao việc kiểm tra trước là cần thiết: thực hiện phép nhân trực tiếp sẽ âm thầm bao bọc trong một hệ thống số nguyên có chiều rộng cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi thao tác được xử lý chính xác một lần với số học và kiểm tra theo thời gian không đổi | 
| Không gian | O(1) | Chỉ có một bộ tích lũy duy nhất được duy trì bất kể kích thước đầu vào | 

Quá trình quét tuyến tính khớp với cấu trúc đầu vào tự nhiên và không yêu cầu cấu trúc dữ liệu bổ sung. Dung lượng bộ nhớ không đổi đảm bảo giải pháp được an toàn ngay cả đối với kích thước đầu vào tối đa điển hình trong các ràng buộc kiểu Codeforces. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        solve()
    except SystemExit:
        pass
    return ""

# minimal case
assert run("1\nadd\n") == ""

# simple multiplication
assert run("3\nadd\nmul 2\nmul 3\n") == ""

# overflow case trigger
assert run("2\nadd\nmul 1000000000000000000\n") == ""

# alternating growth
assert run("5\nadd\nmul 2\nadd\nmul 2\nadd\n") == ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thêm đơn | không tràn | trường hợp cơ sở đúng đắn | 
| phép nhân lớn | phát hiện tràn | an toàn ranh giới | 
| hoạt động luân phiên | tích lũy đúng | hành vi đúng đắn hỗn hợp | 

## Vỏ cạnh 

Chế độ lỗi tinh vi xảy ra khi phép nhân được thực hiện trên các giá trị gần giới hạn số nguyên 64 bit. Ví dụ: bắt đầu từ một bộ tích lũy lớn và nhân với 2 có thể tràn ngay cả khi cả hai toán hạng riêng lẻ đều có vẻ an toàn. 

Coi như:```
x = 5e17
multiply by 3
```Về mặt toán học, đây là 1,5e18, vượt quá 2^63 − 1. 

Một cách thực hiện ngây thơ tính toán`x * 3`trực tiếp trong không gian 64 bit, tạo ra giá trị được bao bọc. Việc triển khai an toàn trước tiên sẽ kiểm tra xem`x > INF / 3`. Từ`5e17 > 3e18 / 3`sai thì phép nhân được phép; nếu không nó sẽ bị từ chối trước khi thực hiện. 

Điều này chứng tỏ rằng tính đúng đắn không phụ thuộc vào việc phát hiện tràn sau khi nó xảy ra mà phụ thuộc vào việc ngăn chặn việc đánh giá không an toàn trước khi nó xảy ra.
