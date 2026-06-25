---
title: "CF 105230B - Trò chơi bài"
description: "Mỗi trong số 60 thẻ tương ứng với một mẫu cố định trên các số nguyên dương. Một số xuất hiện trên thẻ một cách chính xác khi nó thỏa mãn một điều kiện nhị phân nhất định bắt nguồn từ chỉ mục của thẻ đó."
date: "2026-06-24T15:57:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105230
codeforces_index: "B"
codeforces_contest_name: "2024-2025 ICPC Bolivia Pre-National Contest"
rating: 0
weight: 105230
solve_time_s: 78
verified: true
draft: false
---

[CF 105230B - Trò chơi bài](https://codeforces.com/problemset/problem/105230/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trong số 60 thẻ tương ứng với một mẫu cố định trên các số nguyên dương. Một số xuất hiện trên thẻ một cách chính xác khi nó thỏa mãn một điều kiện nhị phân nhất định bắt nguồn từ chỉ mục của thẻ đó. Cụ thể, nếu chúng ta xem các ví dụ, thẻ 1 chứa tất cả các số lẻ, thẻ 2 chứa các số có bit nhị phân thứ hai được đặt, thẻ 4 tương ứng với bit thứ ba, v.v. Cấu trúc nhất quán với mỗi thẻ biểu thị một vị trí bit đơn trong biểu diễn nhị phân của số. 

Trong mọi truy vấn, chúng ta được cung cấp một tập hợp con các chỉ số thẻ. Tập hợp con này thể hiện chính xác bộ thẻ có số ẩn xuất hiện. Nhiệm vụ của chúng ta là tái tạo lại con số ẩn đó. Vì mỗi thẻ tương ứng với một vị trí bit nên tập hợp con sẽ trực tiếp cho chúng ta biết bit nào được đặt trong số đó. 

Các ràng buộc là rất nhỏ trong chiều liên quan. Có tối đa 1000 truy vấn và tối đa 60 thẻ cho mỗi truy vấn. Bất kỳ giải pháp nào xử lý từng truy vấn theo thời gian tuyến tính theo số lượng thẻ đều đủ nhanh. Giá trị ẩn có thể lớn tới 10^18, nghĩa là tối đa 60 bit là đủ, khớp chính xác với số lượng thẻ. Đây là một gợi ý mạnh mẽ rằng biểu diễn là nhị phân và đầy đủ. 

Một trường hợp thất bại tinh vi đối với cách giải thích ngây thơ là coi các thẻ như các tập hợp tùy ý và cố gắng giao nhau hoặc tìm kiếm thông qua các danh sách rõ ràng. Ví dụ: nếu chúng tôi cố gắng mô phỏng tư cách thành viên bằng cách quét các chuỗi được tạo, chúng tôi sẽ nhanh chóng vượt quá giới hạn vì các chuỗi là vô hạn. Một giả định không chính xác khác là cho rằng ánh xạ không phải là một-một, điều này sẽ dẫn đến sự mơ hồ. Cấu trúc thực sự đảm bảo tính duy nhất vì mỗi số có một biểu diễn nhị phân duy nhất, do đó có sự kết hợp duy nhất giữa các thẻ. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là diễn giải từng thẻ theo nghĩa đen như một danh sách vô hạn và cố gắng xác định số ẩn bằng cách kiểm tra các ứng cử viên. Người ta có thể tưởng tượng việc lặp lại tất cả các số lên tới 10^18 và kiểm tra xem chúng có xuất hiện chính xác trong bộ thẻ đã cho hay không. Điều này đúng về mặt logic vì tư cách thành viên đã được xác định rõ ràng nhưng hoàn toàn không khả thi. Ngay cả việc kiểm tra một số cũng yêu cầu xác minh tới 60 điều kiện và việc lặp lại trên toàn bộ phạm vi sẽ yêu cầu 10^18 thao tác. 

Quan sát quan trọng là hệ thống thẻ mã hóa các số ở dạng nhị phân. Mỗi thẻ tương ứng với lũy thừa của hai và một số xuất hiện trên thẻ nếu bit tương ứng được đặt ở dạng biểu diễn nhị phân của nó. Điều này biến đổi vấn đề từ tập hợp thành viên trên các chuỗi vô hạn thành việc xây dựng lại một số nguyên từ mặt nạ nhị phân của nó. 

Khi điều này được nhận ra, mỗi truy vấn sẽ trở thành một nhiệm vụ tái tạo bit đơn giản. Chúng ta khởi tạo câu trả lời về 0 và đặt bit thứ i nếu thẻ i có trong tập con đầu vào. Điều này có tác dụng vì mỗi thẻ đóng góp độc lập và duy nhất cho số cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force qua các con số | O(10^18 · 60) | O(1) | Quá chậm | 
| Tái thiết bit | O(60) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi dựa vào thực tế là thẻ i tương ứng với bit 2^(i-1) của số. 

1. Khởi tạo kết quả cho truy vấn hiện tại bằng 0. Giá trị này sẽ dần dần tích lũy đóng góp từ mỗi thẻ được chọn. 
2. Đọc danh sách các chỉ số thẻ được cung cấp trong truy vấn. Mỗi chỉ số đại diện cho một vị trí bit phải được bật ở số cuối cùng. 
3. Với mỗi chỉ số lá bài a, hãy tính lũy thừa tương ứng của 2 khi dịch chuyển sang trái 1 (a - 1). Thêm giá trị này vào kết quả bằng cách sử dụng bitwise OR hoặc phép cộng. 
4. Sau khi xử lý tất cả các chỉ số, xuất ra số kết quả. Giá trị này là số nguyên duy nhất có biểu diễn nhị phân khớp chính xác với các thẻ đã chọn.

Tính đúng đắn xuất phát từ thực tế là mỗi số có một phân tách nhị phân duy nhất. Các thẻ thực sự là một mã hóa phân tán của quá trình phân tách đó, vì vậy việc chọn thẻ tương đương với việc chọn bit. Không có tương tác nào tồn tại giữa các bit khác nhau, vì vậy việc kết hợp chúng là an toàn và không bị mất mát. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    for _ in range(q):
        data = list(map(int, input().split()))
        k = data[0]
        cards = data[1:]
        res = 0
        for c in cards:
            res |= (1 << (c - 1))
        print(res)

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp việc giải thích bit. Mỗi chỉ số thẻ được chuyển đổi thành lũy thừa của hai bằng cách sử dụng dịch chuyển trái, đây là cách biểu thị tiêu chuẩn của các vị trí nhị phân. Bitwise OR được sử dụng thay cho phép cộng, mặc dù phép cộng cũng sẽ an toàn vì mỗi vị trí bit là duy nhất và không bao giờ trùng nhau. 

Một lỗi thường gặp là sai sót trong ca làm việc. Thẻ 1 tương ứng với bit có trọng số nhỏ nhất, do đó độ dịch đúng là (c - 1), không phải c. Một sai lầm khác là cố gắng mô phỏng chuỗi vô hạn, điều này là không cần thiết và không thể thực hiện được. 

## Ví dụ đã hoạt động 

Hãy xem xét truy vấn mẫu đầu tiên có thẻ 1, 2, 3, 4. 

| Bước | Thẻ | Hoạt động | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 1 | res = 1 << 0 = 1 | 1 | 
| 2 | 2 | độ phân giải = 2 | 3 | 
| 3 | 3 | độ phân giải = 4 | 7 | 
| 4 | 4 | độ phân giải = 8 | 15 | 

Điều này cho thấy rằng việc chọn bốn bit đầu tiên sẽ tạo ra 15, tức là 1111 nhị phân. 

Bây giờ hãy xem xét truy vấn chỉ với thẻ 1 và 2. 

| Bước | Thẻ | Hoạt động | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 << 0 = 1 | 1 | 
| 2 | 2 | 1 << 1 = 2 | 3 | 

Điều này tạo ra 3, tương ứng với số nhị phân 11. Điều này xác nhận rằng việc tái tạo chính xác là mã hóa nhị phân được ngụ ý bởi tư cách thành viên thẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q · 60) | Mỗi truy vấn xử lý tối đa 60 thẻ, mỗi thẻ gây ra hoạt động bit O(1) | 
| Không gian | O(1) | Chỉ một số số nguyên không đổi được sử dụng cho mỗi truy vấn | 

Giới hạn q 1000 và 60 thao tác trên mỗi truy vấn mang lại tối đa 60000 thao tác, điều này không đáng kể trong giới hạn thời gian. Việc sử dụng bộ nhớ không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    q = int(input())
    for _ in range(q):
        data = list(map(int, input().split()))
        k = data[0]
        cards = data[1:]
        res = 0
        for c in cards:
            res |= (1 << (c - 1))
        print(res)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.strip()

# provided sample
assert run("""4
4 1 2 3 4
2 1 2
3 1 3 4
7 3 6 9 10 22 29 45
""") == """15
3
13
17592456577828"""

# single card
assert run("""1
1 1
""") == "1"

# high bit only
assert run("""1
1 60
""") == str(1 << 59)

# multiple sparse bits
assert run("""1
3 2 10 20
""") == str((1<<1) + (1<<9) + (1<<19))

# all first 5 bits
assert run("""1
5 1 2 3 4 5
""") == str((1<<5) - 1)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thẻ đơn 1 | 1 | trường hợp cơ sở đúng đắn | 
| chỉ có thẻ 60 | 2^59 | xử lý bit cao | 
| lựa chọn thưa thớt | tổng hợp quyền hạn | bit không liền kề | 
| năm lá bài đầu tiên | 31 | mặt nạ bit liền kề | 

## Vỏ cạnh 

Trường hợp một cạnh là khi chỉ có thẻ cao nhất, chẳng hạn như thẻ 60. Thuật toán tính 1 << 59, nằm trong phạm vi 10^18 được phép và khớp một cách an toàn với số nguyên Python. Cách tiếp cận mô phỏng đơn giản có thể thất bại ở đây do lo ngại tràn các loại có chiều rộng cố định. 

Một trường hợp khác là khi chỉ chọn thẻ 1. Kết quả phải chính xác là 1 và điều này kiểm tra tính chính xác của từng dịch chuyển. Nếu sử dụng nhầm 1 << c thay vì 1 << (c - 1), kết quả đầu ra sẽ trở thành 2 không chính xác, điều này đã vi phạm trường hợp hợp lệ nhỏ nhất. 

Trường hợp cạnh cuối cùng là khi tất cả các thẻ được chọn trong một truy vấn. Kết quả trở thành (2^60 - 1), một mặt nạ bit dày đặc. Điều này khẳng định việc kết hợp tất cả các đóng góp độc lập vẫn tôn trọng cấu trúc nhị phân và không gây ra sự chồng chéo hay mất mát thông tin.
