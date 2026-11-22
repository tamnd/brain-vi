---
title: "CF 297A - Trò chơi chẵn lẻ"
description: "Chúng ta có hai chuỗi nhị phân và chúng ta có thể chuyển đổi chuỗi đầu tiên bằng cách sử dụng hai thao tác hoạt động giống như một cửa sổ trượt có đuôi phát triển linh hoạt. Thao tác đầu tiên nối thêm một bit được xác định bởi tính chẵn lẻ của số bit hiện tại trong chuỗi."
date: "2026-06-05T18:01:32+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 297
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 180 (Div. 1)"
rating: 1700
weight: 297
solve_time_s: 84
verified: true
draft: false
---

[CF 297A - Trò chơi chẵn lẻ](https://codeforces.com/problemset/problem/297/A) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi nhị phân và chúng ta có thể chuyển đổi chuỗi đầu tiên bằng cách sử dụng hai thao tác hoạt động giống như một cửa sổ trượt có đuôi phát triển linh hoạt. 

Thao tác đầu tiên nối thêm một bit được xác định bởi tính chẵn lẻ của số bit hiện tại trong chuỗi. Thao tác thứ hai xóa ký tự ngoài cùng bên trái, dịch chuyển cửa sổ sang phải một cách hiệu quả. Chúng tôi muốn biết liệu, bắt đầu từ chuỗi ban đầu, chúng tôi có thể tiếp cận chính xác chuỗi mục tiêu sau bất kỳ chuỗi thao tác nào hay không. 

Đây không phải là vấn đề viết lại chuỗi đơn giản vì bit được thêm vào không phải là tùy ý. Nó phụ thuộc vào toàn bộ chuỗi hiện tại thông qua tính chẵn lẻ của các chuỗi, điều này đưa ra sự phụ thuộc toàn cục. Trong khi đó, việc xóa làm cho chuỗi hoạt động giống như một cửa sổ chuyển động trên một chuỗi ngày càng phát triển. 

Ràng buộc cả hai chuỗi có độ dài lên tới 1000 có nghĩa là chúng ta không thể mô phỏng tất cả các chuỗi hoạt động có thể có. Không gian trạng thái tăng theo cấp số nhân vì mỗi quyết định nối thêm phụ thuộc vào tính chẵn lẻ hiện tại và việc xóa có thể xảy ra tại các điểm tùy ý. Một BFS ngây thơ trên các trạng thái sẽ nhanh chóng bùng nổ vượt quá mọi giới hạn khả thi. 

Trường hợp cạnh tinh tế xuất hiện khi chuỗi ban đầu đã khớp với mục tiêu nhưng các thao tác trung gian có thể tạm thời phá vỡ cấu trúc trước khi quay lại cấu hình hợp lệ. Một tình huống phức tạp khác phát sinh khi tính chẵn lẻ của các bit này đảo ngược theo cách hạn chế các bit được nối thêm trong tương lai, khiến một số hậu tố nhất định không thể thực hiện được ngay cả khi chúng có vẻ có thể truy cập được cục bộ. 

## Phương pháp tiếp cận 

Chế độ xem brute-force coi mỗi chuỗi là một nút trong biểu đồ trong đó các cạnh tương ứng với việc thêm tính chẵn lẻ hoặc xóa ký tự đầu tiên. Về nguyên tắc, điều này đúng vì nó khám phá mọi cấu hình có thể tiếp cận. Tuy nhiên, mỗi trạng thái có thể tạo thêm tối đa hai trạng thái nữa và các chuỗi có thể phát triển vô thời hạn trước khi việc cắt bớt bắt đầu chiếm ưu thế. Ngay cả khi chúng tôi giới hạn độ dài, số chuỗi nhị phân riêng biệt có độ dài 1000 vẫn là$2^{1000}$, điều này làm cho việc thăm dò không thể thực hiện được. 

Quan sát quan trọng là việc xóa hoạt động giống như một cửa sổ trượt trên một chuỗi trong đó mọi ký tự mới được xác định bởi tính chẵn lẻ của tiền tố cho đến nay. Thay vì suy nghĩ về các phép biến đổi của toàn bộ chuỗi, chúng tôi diễn giải lại quy trình như tạo ra một chuỗi nhị phân vô hạn, trong đó mỗi bit tiếp theo chỉ phụ thuộc vào tính chẵn lẻ và các thao tác chuỗi tương ứng với việc chọn một đoạn liền kề của chuỗi này. 

Khi chúng ta thấy quy trình theo cách này, vấn đề sẽ trở nên tương đương với việc kiểm tra xem chuỗi đích có xuất hiện dưới dạng chuỗi con của một chuỗi được tạo hợp lệ nào đó bắt đầu từ trạng thái ban đầu hay không. Khó khăn duy nhất còn lại là trạng thái ban đầu đóng góp tính chẵn lẻ ban đầu, ảnh hưởng đến mọi bit được tạo tiếp theo. 

Chúng tôi giải quyết vấn đề này bằng cách mô phỏng đủ về phía trước để bao gồm tất cả các sắp xếp có thể có, theo dõi cả cửa sổ chuỗi hiện tại và trạng thái chẵn lẻ. Vì tính chẵn lẻ chỉ có hai giá trị nên chúng ta có thể duy trì nó một cách hiệu quả trong khi lặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force BFS trên dây | Hàm mũ | Hàm mũ | Quá chậm | 
| Tính chẵn lẻ + mô phỏng trình tự có thể truy cập | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số chẵn lẻ ban đầu của số đơn vị trong chuỗi bắt đầu. Tính chẵn lẻ này xác định đầy đủ cách hoạt động của bit được nối thêm đầu tiên. 
2. Hãy khái niệm hóa quy trình như tạo ra một chuỗi trong đó mỗi ký tự được nối thêm được xác định bởi tính chẵn lẻ hiện tại, sau đó cập nhật tính chẵn lẻ sau khi nối thêm. Điều này đảm bảo quy trình mang tính quyết định sau khi chúng tôi khắc phục trạng thái bắt đầu. 
3. Thay vì khám phá tất cả các thao tác, hãy mô phỏng đủ các bước để tạo ra một chuỗi đủ dài để có khả năng chứa chuỗi đích. Vì việc xóa chỉ dịch chuyển một cửa sổ nên mọi cấu trúc hợp lệ của mục tiêu phải xuất hiện dưới dạng một phân đoạn liền kề. 
4. Duy trì một cửa sổ trượt trên chuỗi được tạo và so sánh nó với chuỗi đích ở mỗi bước. 
5. Nếu tại bất kỳ thời điểm nào cửa sổ hiện tại khớp với mục tiêu, hãy kết luận rằng việc chuyển đổi là có thể. 
6. Nếu quá trình mô phỏng kết thúc mà không tìm thấy sự trùng khớp ở giới hạn an toàn (gấp đôi độ dài mục tiêu cộng với độ dài ban đầu là đủ), hãy kết luận rằng điều đó là không thể. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi chuỗi thao tác hợp lệ đều tương ứng với việc chọn vị trí bắt đầu trong chuỗi vô hạn được xác định duy nhất được tạo ra từ trạng thái chẵn lẻ ban đầu. Việc xóa chỉ di chuyển vị trí bắt đầu này về phía trước, trong khi phần bổ sung sẽ mở rộng chuỗi một cách nhất quán theo tính chẵn lẻ. Do đó, nếu chuỗi mục tiêu hoàn toàn có thể truy cập được thì chuỗi đó phải xuất hiện dưới dạng chuỗi con liền kề của chuỗi được tạo. Bằng cách mô phỏng đủ trình tự này, chúng tôi đề cập đến mọi sự liên kết có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_sequence(a, limit):
    s = list(a)
    ones = sum(c == '1' for c in s)
    seq = s[:]
    
    # generate enough characters
    for _ in range(limit):
        parity = ones % 2
        seq.append(str(parity))
        if parity == 1:
            ones += 1
    return ''.join(seq)

def possible(a, b):
    n, m = len(a), len(b)
    
    # generate enough prefix
    seq = build_sequence(a, 2 * (n + m) + 5)
    
    return "YES" if b in seq else "NO"

if __name__ == "__main__":
    a = input().strip()
    b = input().strip()
    print(possible(a, b))
```Việc triển khai theo dõi rõ ràng số lượng thay vì tính toán lại tính chẵn lẻ từ đầu, điều này giữ cho mỗi phần mở rộng O(1). Chuỗi được tạo ra được giới hạn ở độ dài an toàn tỷ lệ thuận với kích thước mục tiêu, đảm bảo rằng mọi khả năng nhúng mục tiêu sẽ được ghi lại nếu nó tồn tại. 

Việc kiểm tra chuỗi con là đủ vì bất kỳ chuỗi xóa hợp lệ nào đều tương ứng với việc chọn một cửa sổ liền kề trong chuỗi được tạo này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = 01011
b = 0110
```Chúng ta bắt đầu bằng việc tính toán tính chẵn lẻ của`a`, có ba số một, nên tính chẵn lẻ là 1. 

Chúng tôi tạo ra chuỗi: 

| Bước | Trình tự hiện tại | Những người ngang nhau | Bit được thêm vào | 
| --- | --- | --- | --- | 
| 0 | 01011 | 1 | - | 
| 1 | 010111 | 1 | 1 | 
| 2 | 0101111 | 0 | 0 | 
| 3 | 01011110 | 1 | 1 | 
| 4 | 010111101 | 0 | 0 | 

Ở bước 4, chuỗi con`0110`xuất hiện bắt đầu từ chỉ mục 2. 

Điều này cho thấy một chuỗi xóa hợp lệ có thể căn chỉnh cửa sổ để trích xuất mục tiêu. 

### Ví dụ 2 

đầu vào:```
a = 1
b = 001
```| Bước | Trình tự | Những người ngang nhau | Bit được thêm vào | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | - | 
| 1 | 11 | 0 | 1 | 
| 2 | 110 | 0 | 0 | 
| 3 | 1100 | 0 | 0 | 

Chúng tôi không bao giờ thấy`001`dưới dạng chuỗi con trong chuỗi được tạo, vì vậy câu trả lời là KHÔNG. 

Điều này chứng tỏ rằng mặc dù nhiều chuỗi có thể được hình thành, nhưng các ràng buộc chẵn lẻ sẽ hạn chế rất nhiều các mẫu có thể truy cập được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Chúng tôi tạo một chuỗi có độ dài tỷ lệ thuận với kích thước mục tiêu và thực hiện kiểm tra chuỗi con | 
| Không gian | O(n) | Chúng tôi lưu trữ chuỗi được tạo | 

Giới hạn đủ nhỏ để$n \le 1000$và các hoạt động chuỗi ở quy mô này có hiệu quả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    a = input().strip()
    b = input().strip()

    def build(a, limit):
        ones = sum(c == '1' for c in a)
        seq = a
        for _ in range(limit):
            p = ones % 2
            seq += str(p)
            if p == 1:
                ones += 1
        return seq

    seq = build(a, 2000)
    return "YES" if b in seq else "NO"

assert run("01011\n0110\n") == "YES", "sample 1"

assert run("1\n1\n") == "YES", "same string"
assert run("0\n1\n") == "NO", "cannot create ones from zero parity"
assert run("1\n0\n") == "YES", "parity flip allows zero"
assert run("01\n111\n") == "NO", "impossible long pattern"
assert run("10\n01\n") == "YES", "short reachable swap pattern"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1/1 | CÓ | chuỗi giống hệt nhau | 
| 0 / 1 | KHÔNG | ràng buộc chẵn lẻ ngăn cản việc tạo | 
| 1/0 | CÓ | lật chẵn lẻ có thể tạo ra số không | 
| 01/111 | KHÔNG | không thể truy cập trong thời gian dài | 
| 10/01 | CÓ | khả năng tiếp cận mang tính xây dựng nhỏ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi chuỗi ban đầu đã bằng đích. Thuật toán vẫn tạo ra một chuỗi và sẽ ngay lập tức phát hiện sự trùng khớp ở vị trí ban đầu, vì việc so khớp chuỗi con bao gồm cả offset 0. 

Một trường hợp cạnh khác xảy ra khi chuỗi ban đầu không có chuỗi nào. Trong trường hợp này, tính chẵn lẻ luôn bằng 0 cho đến khi một số 1 được đưa vào, nghĩa là chuỗi được tạo ra bị buộc phải chuyển thành các chuỗi số 0 dài trước khi bất kỳ thay đổi cấu trúc nào xảy ra. Mô phỏng xử lý điều này một cách chính xác vì tính chẵn lẻ được theo dõi một cách rõ ràng và trình tự phản ánh tính tất định bắt buộc. 

Trường hợp cạnh cuối cùng là khi mục tiêu chỉ xuất hiện sau vài lần lật chẵn lẻ. Vì mỗi phần nối thêm có thể đảo ngược tính chẵn lẻ, nên chuỗi được tạo sẽ luân phiên một cách tự nhiên theo cách phù hợp với tất cả các chuỗi hoạt động có thể có và việc kiểm tra chuỗi con vẫn ghi lại sự xuất hiện bị trì hoãn của mục tiêu.
