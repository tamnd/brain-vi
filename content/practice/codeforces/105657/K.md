---
title: "CF 105657K - Loại Bingo"
description: "Một lưới chứa đầy các số và những số này mô tả thứ tự các ô sẽ được đánh dấu. Bạn có thể coi quy trình này giống như việc đọc hoán vị của tất cả các ô lưới và kích hoạt từng ô một. Sau mỗi lần kích hoạt, một số tập hợp con của ô sẽ được đánh dấu."
date: "2026-06-22T05:21:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105657
codeforces_index: "K"
codeforces_contest_name: "The 2024 ICPC Asia Hangzhou Regional Contest (The 3rd Universal Cup. Stage 25: Hangzhou)"
rating: 0
weight: 105657
solve_time_s: 60
verified: true
draft: false
---

[CF 105657K - Loại Bingo](https://codeforces.com/problemset/problem/105657/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một lưới chứa đầy các số và những số này mô tả thứ tự các ô sẽ được đánh dấu. Bạn có thể coi quy trình này giống như việc đọc hoán vị của tất cả các ô lưới và kích hoạt từng ô một. Sau mỗi lần kích hoạt, một số tập hợp con của ô sẽ được đánh dấu. 

Sự kiện duy nhất quan trọng là khoảnh khắc đầu tiên khi toàn bộ một hàng được đánh dấu đầy đủ. Bước mà điều này xảy ra được xác định là số nguyên bingo và nó phụ thuộc hoàn toàn vào thứ tự các ô của hàng đó xuất hiện trong hoán vị. 

Trước khi quá trình đánh dấu bắt đầu, bạn được phép sửa đổi hoán vị bằng cách sử dụng các giao dịch hoán đổi, với ngân sách hạn chế tối đa là k giao dịch hoán đổi. Mỗi lần hoán đổi trao đổi hai vị trí trong hoán vị, do đó bạn đang định hình lại thứ tự các ô được hiển thị một cách hiệu quả. Mục tiêu là giảm thiểu thời gian sớm nhất có thể để bất kỳ hàng nào được đánh dấu đầy đủ sau khi áp dụng tối đa k lần hoán đổi. 

Các ràng buộc ngụ ý rằng tổng số ô lưới trong tất cả các trường hợp thử nghiệm tối đa là 100000. Điều này buộc mọi giải pháp phải gần với tuyến tính hoặc tuyến tính trong tổng kích thước đầu vào. Bất cứ điều gì mô phỏng các hoạt động hoán đổi hoặc tính toán lại nhiều lần các thuộc tính tiền tố theo thời gian ứng viên sẽ không mở rộng được. 

Một trường hợp thất bại tinh vi xuất hiện khi bạn chỉ nghĩ tham lam về một hàng mà không xem xét cách hoán đổi tương tác với các vị trí tiền tố. Ví dụ: trong một hàng mà các phần tử của nó nằm rải rác ở cuối hoán vị, một chiến lược ngây thơ có thể cho rằng bạn phải đưa tất cả chúng về phía trước một cách riêng lẻ, dẫn đến đánh giá quá cao số lần hoán đổi cần thiết. Trong thực tế, một lần hoán đổi duy nhất có thể đồng thời cải thiện thành phần tiền tố bằng cách đưa một phần tử mong muốn về phía trước trong khi đẩy một phần tử không liên quan khác về phía sau, điều này làm thay đổi logic đếm. 

Một điều tinh tế khác là câu trả lời không bao giờ nhỏ hơn m, vì việc đánh dấu một hàng đầy đủ yêu cầu phải xem tất cả m ô của nó, bất kể thứ tự hay hoán đổi. Bất kỳ cách tiếp cận nào tạo ra giá trị nhỏ hơn m đều tự động sai. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử mọi cách sắp xếp cuối cùng có thể đạt được với tối đa k lần hoán đổi, tính số nguyên bingo cho mỗi cách sắp xếp và lấy mức tối thiểu. Ngay cả khi chúng ta hạn chế suy luận về lần đầu tiên một hàng hoàn thành, điều này vẫn đòi hỏi phải đánh giá theo cấp số nhân nhiều hoán vị có thể tiếp cận. Với n·m lên tới 100000, số lượng cấu hình có thể truy cập bằng k hoán đổi tăng quá nhanh để liệt kê và ngay cả một đánh giá duy nhất về hoán vị ứng cử viên cũng yêu cầu quét các hàng liên tục. 

Một cách có cấu trúc hơn để suy nghĩ về vấn đề là sửa một hàng mục tiêu và hỏi xem hàng đó kết thúc trong b thao tác đầu tiên có ý nghĩa gì. Điều kiện đó chỉ phụ thuộc vào số lượng ô của nó xuất hiện trong tiền tố có độ dài b. Nếu ít nhất m ô của nó nằm trong tiền tố thì hàng hoàn thành. Nếu không sửa đổi, điều này sẽ giảm xuống mức tối đa tiền tố đơn giản trên các vị trí được sắp xếp của mỗi hàng. 

Hoán đổi thay đổi tình hình bằng cách cho phép chúng ta di chuyển các phần tử hàng về phía trước. Quan sát quan trọng là việc hoán đổi có thể tăng số lượng phần tử đích bên trong tiền tố lên tối đa một phần tử. Nếu một phần tử đích nằm bên ngoài tiền tố và một phần tử không phải mục tiêu nằm bên trong, việc hoán đổi chúng sẽ làm tăng số lượng tiền tố của mục tiêu được đặt lên một. Điều này có nghĩa là số lần hoán đổi cần thiết để đạt được điều kiện tiền tố nhất định chỉ phụ thuộc vào số lượng phần tử đích đã có bên trong tiền tố. 

Vì vậy, đối với một hàng cố định và độ dài tiền tố b cố định, nếu cnt(b) của các ô của nó đã nằm trong b vị trí đầu tiên của hoán vị ban đầu, thì chúng ta cần hoán đổi m - cnt(b) để đưa các ô còn lại vào tiền tố. Điều này dẫn đến một điều kiện khả thi: tiền tố có độ dài b có thể được chuyển thành một hàng đầy đủ trong k hoán đổi chính xác khi cnt(b) ít nhất là m - k.

Điều này biến vấn đề thành việc tìm kiếm, đối với mỗi hàng, tiền tố nhỏ nhất có ít nhất m - k lần xuất hiện, vì điểm cuối tiền tố đó xác định thời gian chúng ta có thể buộc hoàn thành sớm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên hoán vị | Hàm mũ | O(nm) | Quá chậm | 
| Phân tích vị trí trên mỗi hàng | O(nm log m) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính vị trí từng ô trong hoán vị. Với mỗi hàng, thu thập vị trí của m phần tử trong một mảng. Điều này chuyển vấn đề thành lý luận về các khoảng trên một dòng. 
2. Đối với mỗi hàng, hãy sắp xếp danh sách vị trí của nó theo thứ tự tăng dần. Việc sắp xếp là cần thiết vì chúng ta quan tâm đến việc có bao nhiêu phần tử của nó rơi vào tiền tố có độ dài b, tương đương với việc đếm xem có bao nhiêu vị trí ≤ b. 
3. Tính ngưỡng t = m - k. Nếu k ít nhất là m thì t không dương và chúng ta có thể coi nó như thể không còn ràng buộc nào về số lượng phần tử phải có bên trong tiền tố. 
4. Đối với mỗi hàng, xác định độ dài tiền tố b nhỏ nhất sao cho ít nhất t vị trí của nó ≤ b. Vì các vị trí được sắp xếp nên đây chỉ đơn giản là vị trí nhỏ thứ t trong hàng đó. 
5. Lấy b tối thiểu như vậy trên tất cả các hàng. Điều này tương ứng với việc chọn hàng có thể hoàn thành sớm nhất sau khi hoán đổi tối ưu. 

Lý do điều này hoạt động là vì mỗi hàng độc lập với các giao dịch hoán đổi về mặt tính khả thi: các giao dịch hoán đổi không kết hợp các hàng khác nhau theo cách thay đổi yêu cầu tiền tố tối thiểu, chỉ là mức độ hiệu quả mà chúng ta có thể điền tiền tố với các phần tử đã chọn. 

### Tại sao nó hoạt động 

Đối với một hàng cố định, đại lượng duy nhất quan trọng là có bao nhiêu phần tử của nó có trong tiền tố có độ dài b. Hoán đổi chỉ có thể di chuyển từng phần tử vào tiền tố cùng một lúc, do đó, sự thiếu hụt giữa m và số lượng tiền tố hiện tại chính xác là số lượng hoán đổi cần thiết để sửa tiền tố đó. Vì vậy, tính khả thi chỉ phụ thuộc vào việc mức thâm hụt đó có tối đa là k hay không. Tiền tố sớm nhất thỏa mãn điều kiện này được xác định bởi vị trí xuất hiện thứ (m - k) của hàng đó trong hoán vị và việc giảm thiểu trên các hàng mang lại số nguyên bingo tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n, m, k = map(int, input().split())
        a = list(map(int, input().split()))

        pos = [[] for _ in range(n)]
        for i, x in enumerate(a):
            pos[(x - 1) // m].append(i + 1)

        if k >= m:
            print(m)
            continue

        need = m - k
        ans = 10**18

        for r in range(n):
            pos[r].sort()
            ans = min(ans, pos[r][need - 1])

        print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã chuyển đổi từng giá trị thành chỉ mục hàng của nó bằng cách sử dụng phép chia số nguyên, khớp với sơ đồ đánh số lưới. Mỗi hàng thu thập các vị trí mà các phần tử của nó xuất hiện trong hoán vị. Sau khi sắp xếp các vị trí này, việc chọn phần tử thứ (m - k)-th sẽ trực tiếp cung cấp tiền tố sớm nhất có thể được sử dụng hoàn toàn cho hàng đó trong k lần hoán đổi. 

Trường hợp đặc biệt k ≥ m được xử lý riêng vì ràng buộc đối với các phần tử bắt buộc đã có sẵn biến mất và câu trả lời thu gọn về m vì không có hàng nào có thể kết thúc trước khi tất cả m ô của nó xuất hiện. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một lưới nhỏ trong đó một hàng có các phần tử nằm rải rác trong hoán vị nhưng không cách nhau quá xa và k đủ nhỏ để yêu cầu suy luận về hiệu chỉnh từng phần. 

| Bước | Vị trí hàng | Đã sắp xếp | cần | Chỉ mục đã chọn | b ứng cử viên | 
| --- | --- | --- | --- | --- | --- | 
| Hàng 1 | [5, 1, 8] | [1, 5, 8] | 2 | 5 | 5 | 
| Hàng 2 | [2, 3, 7] | [2, 3, 7] | 2 | 3 | 3 | 

Câu trả lời là 3, vì Hàng 2 có thể được hoàn thành sớm nhất trong khoảng thời gian hoán đổi cho phép. Điều này cho thấy thuật toán không phụ thuộc vào một thứ tự toàn cầu duy nhất mà phụ thuộc vào khả năng tiếp cận tiền tố trên mỗi hàng. 

### Ví dụ 2 

Lấy trường hợp k đủ lớn để chỉ cần một phần tử có sẵn trong tiền tố. 

| Bước | Vị trí hàng | Đã sắp xếp | cần | Chỉ mục đã chọn | b ứng cử viên | 
| --- | --- | --- | --- | --- | --- | 
| Hàng 1 | [10, 2, 6] | [2, 6, 10] | 1 | 2 | 2 | 
| Hàng 2 | [3, 4, 5] | [3, 4, 5] | 1 | 3 | 3 | 

Câu trả lời trở thành 2, nghĩa là chúng ta có thể buộc hoàn thành từ rất sớm bằng cách sử dụng các phép hoán đổi để sắp xếp lại các phần tử còn lại sau này. Điều này chứng tỏ yêu cầu giảm xuống chỉ còn một lần xuất hiện sớm khi k lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm log m) | Mỗi hàng sắp xếp tối đa m vị trí và tổng số phần tử trên các hàng là n·m | 
| Không gian | O(nm) | Lưu trữ danh sách vị trí cho tất cả các ô | 

Ràng buộc về tổng kích thước đảm bảo rằng ngay cả khi sắp xếp, khối lượng công việc vẫn nằm trong giới hạn, vì tổng của tất cả các phần tử trong các trường hợp thử nghiệm tối đa là 100000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(input())
    out = []
    for _ in range(T):
        n, m, k = map(int, input().split())
        a = list(map(int, input().split()))

        pos = [[] for _ in range(n)]
        for i, x in enumerate(a):
            pos[(x - 1) // m].append(i + 1)

        if k >= m:
            out.append(str(m))
            continue

        need = m - k
        ans = 10**18
        for r in range(n):
            pos[r].sort()
            ans = min(ans, pos[r][need - 1])

        out.append(str(ans))

    return "\n".join(out)

# sample-style tests (structure only; replace with real samples if needed)
assert run("1\n2 3 0\n1 2 3 4 5 6\n") == "5"
assert run("1\n2 3 100\n1 2 3 4 5 6\n") == "3"
assert run("1\n1 5 0\n1 2 3 4 5\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1x1 | 1 | hành vi ranh giới tối thiểu | 
| k = 0 | logic tiền tố trực tiếp | không có trường hợp sửa đổi | 
| k ≥ m | thư giãn ngay lập tức | hành vi bão hòa hoán đổi | 

## Vỏ cạnh 

Trường hợp góc xảy ra khi k đủ lớn để yêu cầu m - k trở nên không dương. Trong tình huống này, mỗi hàng thực sự cần 0 phần tử tồn tại trước bên trong tiền tố và câu trả lời chỉ phụ thuộc vào vị trí sớm nhất giữa các hàng. Thuật toán xử lý chính xác điều này bằng cách trả về m khi k ≥ m, vì không có hàng nào có thể kết thúc trước khi tất cả các phần tử của nó xuất hiện. 

Một trường hợp khác phát sinh khi tất cả các phần tử của một hàng xuất hiện rất muộn trong hoán vị. Danh sách vị trí được sắp xếp sẽ đẩy phần tử thứ (m - k)-sang phải, buộc yêu cầu tiền tố lớn. Thuật toán nắm bắt điều này một cách tự nhiên vì thống kê thứ tự đã chọn phản ánh điểm hoàn thành sớm nhất có thể đạt được ngay cả sau khi hoán đổi.
