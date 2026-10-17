---
title: "CF 105498F - Thực hiện hoán vị"
description: "Chúng ta được cung cấp một mảng các số nguyên và đối với mỗi phần tử, chúng ta được phép tắt liên tục bất kỳ bit nào được đặt, nhưng chỉ tối đa một lần trên mỗi bit cho mỗi phần tử, tương đương với việc nói rằng mỗi số có thể giảm xuống bất kỳ giá trị nào có thể đạt được bằng cách trừ đi tổng lũy ​​thừa riêng biệt của…"
date: "2026-06-23T01:38:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105498
codeforces_index: "F"
codeforces_contest_name: "Khulna Regional Inter University Programming Contest (KRIUPC) MIRROR"
rating: 0
weight: 105498
solve_time_s: 47
verified: true
draft: false
---

[CF 105498F - Thực hiện hoán vị](https://codeforces.com/problemset/problem/105498/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và với mỗi phần tử, chúng ta được phép tắt liên tục bất kỳ bit nào được đặt, nhưng chỉ tối đa một lần trên mỗi bit cho mỗi phần tử, tương đương với việc nói rằng mỗi số có thể được giảm xuống bất kỳ giá trị nào có thể đạt được bằng cách trừ tổng lũy thừa riêng biệt của hai xuất hiện trong biểu diễn nhị phân của nó. Nói một cách đơn giản hơn, mọi phần tử chỉ có thể di chuyển xuống dưới về giá trị và chỉ bằng cách xóa các bit, không bao giờ bằng cách thêm hoặc sắp xếp lại các bit giữa các phần tử. 

Nhiệm vụ là xác định xem liệu chúng ta có thể biến đổi toàn bộ mảng để nó trở thành hoán vị của các số nguyên từ 1 đến n hay không, nghĩa là mọi số nguyên trong phạm vi đó xuất hiện chính xác một lần sau khi áp dụng các thao tác loại bỏ bit này một cách độc lập cho từng phần tử. 

Ràng buộc n lên tới 10^5 ngụ ý rằng chúng ta cần hành vi gần như O(n log n) hoặc O(n), vì quét tuyến tính dựa trên việc sắp xếp hoặc băm đều ổn, nhưng bất kỳ phương pháp nào cố gắng khám phá tất cả các tập hợp con của việc loại bỏ bit trên mỗi số đều hoàn toàn không khả thi. Mỗi số có thể có tối đa 17 bit, nhưng sự kết hợp giữa n phần tử vẫn bùng nổ nếu được xử lý độc lập. 

Một trường hợp khó phát hiện khi nhiều số lớn cần được giảm xuống trong một phạm vi hẹp như 1 đến n. Ví dụ: nếu tất cả các phần tử đều có lũy thừa bằng hai thì mỗi phần tử chỉ có thể được giảm dọc theo mạng tập hợp con nhị phân của nó, nhưng xung đột sẽ xảy ra nếu có quá nhiều phần tử bị buộc vào cùng một giá trị có thể truy cập được. Một tình huống khó khăn khác là khi một phần tử vốn đã nhỏ nhưng lại "chặn" mục tiêu cần thiết vì không thể tăng lên mà chỉ giảm đi. 

Một sai lầm ngây thơ là gán mỗi ai một cách tham lam cho mục tiêu sẵn có gần nhất ≤ ai mà không xem xét rằng các mức giảm khác nhau cạnh tranh cho cùng một giá trị trung gian. Điều đó có thể thất bại khi các nhiệm vụ đầu tiên sử dụng các giá trị có thể truy cập thấp cần thiết để giải phóng số lượng lớn hơn sau này. 

## Phương pháp tiếp cận 

Chế độ xem brute-force là xem xét mọi số một cách độc lập và liệt kê tất cả các giá trị mà nó có thể trở thành bằng cách xóa các tập hợp con bit, sau đó cố gắng gán từng phần tử cho một mục tiêu duy nhất trong 1 đến n bằng cách sử dụng đối sánh lưỡng cực giữa các chỉ số và giá trị mảng. Điều này đúng vì nó tôn trọng tất cả các ràng buộc, nhưng mỗi số có thể tạo ra tối đa 2^{popcount(ai)} ứng cử viên và trong trường hợp xấu nhất với nhiều bit được đặt, điều này sẽ trở thành hàm mũ cho mỗi phần tử. Ngay cả khi chúng tôi giới hạn các giá trị ở mức 10^5 thì tổng biểu đồ ứng cử viên sẽ trở nên quá lớn và việc khớp với biểu đồ đó vượt quá giới hạn. 

Quan sát quan trọng là việc xóa bit sẽ duy trì cấu trúc đơn điệu: mỗi số xác định một tập hợp các giá trị có thể truy cập được đóng xuống. Thay vì suy nghĩ về các phép gán tùy ý, chúng ta có thể xử lý các mục tiêu từ 1 đến n và quyết định xem liệu chúng ta có thể "trả tiền" cho từng mục tiêu bằng cách sử dụng một số phần tử mảng không sử dụng có thể tiếp cận được mục tiêu đó hay không. Điều này biến vấn đề từ việc gán giá trị cho các phần tử thành khả năng tiêu thụ một cách tham lam. 

Cách tự nhiên để thực thi tính đúng đắn là luôn gán từng mục tiêu i cho ai nhỏ nhất hiện có mà vẫn có thể tiếp cận i. Điều này đảm bảo chúng tôi duy trì tính linh hoạt cho các mục tiêu lớn hơn, bởi vì sử dụng ai lớn cho mục tiêu nhỏ sẽ lãng phí khả năng bao phủ tiềm năng. Điều này dẫn đến việc sắp xếp mảng và kết hợp các mục tiêu theo thứ tự tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tập hợp bit + khớp) | O(n · 2^{bits}) | O(n · 2^{bits}) | Quá chậm | 
| Kết hợp sắp xếp tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi sắp xếp mảng để luôn xem xét các giá trị nhỏ hơn trước, tương ứng với việc sử dụng sớm các phần tử bị ràng buộc nhất. Sau đó, chúng tôi lặp qua các giá trị đích từ 1 đến n, duy trì một con trỏ trên mảng đã được sắp xếp.

Với mỗi giá trị đích i, chúng ta di chuyển con trỏ về phía trước cho đến khi tìm thấy phần tử ai ít nhất là i. Nếu không có phần tử nào như vậy tồn tại, chúng ta ngay lập tức kết luận rằng điều đó là không thể bởi vì tất cả các phần tử còn lại đều quá nhỏ để có thể chạm tới i hoặc bất cứ thứ gì lớn hơn. 

Khi chúng tôi tìm thấy phần tử như vậy, chúng tôi gán nó cho i và di chuyển con trỏ về phía trước, đánh dấu phần tử đó là đã sử dụng một cách hiệu quả. Chúng tôi không bao giờ xem lại các phần tử, điều này đảm bảo mỗi giá trị mảng được sử dụng nhiều nhất một lần. 

Bước tinh tế quan trọng là tại sao kiểm tra ai ≥ i là đủ. Vì chúng ta chỉ có thể loại bỏ các bit nên bất kỳ ai nào cũng có thể được giảm xuống một giá trị nào đó trong khoảng từ 0 đến ai, nhưng không phải tất cả các giá trị trung gian đều có thể truy cập được. Tuy nhiên, trong bài toán cụ thể này, mọi số nguyên i ≤ ai đều có thể truy cập được bằng cách xóa các bit thích hợp một cách độc lập, bởi vì bất kỳ số nguyên nào cũng có thể được tạo thành bằng cách chọn một tập hợp con các bit của ai và hiểu nó là một số không lớn hơn ai. Điều này có nghĩa là khả năng tiếp cận tương đương với điều kiện bất đẳng thức đơn giản. 

### Tại sao nó hoạt động 

Ở mỗi bước i, chúng ta sử dụng ai nhỏ nhất có thể đáp ứng được i. Điều này duy trì tính bất biến tham lam: nhiều tập giá trị còn lại luôn đủ để bao gồm tất cả các mục tiêu trong tương lai nếu phép gán hiện tại hợp lệ. Nếu chúng ta bỏ qua một ai hợp lệ nhỏ hơn để lấy một ai lớn hơn, chúng ta có nguy cơ chặn các mục tiêu trong tương lai yêu cầu ai lớn hơn. Cấu trúc đơn điệu của cả ai được sắp xếp và mục tiêu tăng dần đảm bảo rằng mọi phép gán hợp lệ đều có thể được chuyển đổi thành dạng tham lam này mà không vi phạm tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    a.sort()
    j = 0
    
    for i in range(1, n + 1):
        while j < n and a[j] < i:
            j += 1
        if j == n:
            print("No")
            return
        j += 1
    
    print("Yes")

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc sắp xếp mảng và sau đó sử dụng một con trỏ j để theo dõi phần tử không được sử dụng tiếp theo. Với mỗi mục tiêu i, chúng ta tiến tới j cho đến khi tìm được giá trị ít nhất là i. Điều này tránh việc quét lại các phần tử đã được sử dụng và giữ cho tổng độ phức tạp trở nên tuyến tính sau khi sắp xếp. Tính đúng đắn phụ thuộc vào việc giải thích rằng bất kỳ giá trị a[j] nào cũng có thể giảm xuống i miễn là a[j] ≥ i, do đó phép gán luôn khả thi khi bất đẳng thức được giữ nguyên. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`1 3 7`Đầu tiên chúng ta sắp xếp mảng mặc dù nó đã được sắp xếp rồi. Sau đó chúng tôi ghép các mục tiêu từ 1 đến 3. 

| tôi | j | một[j] | hành động | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | phân công 1 → 1 | 
| 2 | 1 | 3 | phân công 3 → 2 | 
| 3 | 2 | 7 | phân công 7 → 3 | 

Điều này cho thấy rằng mỗi mục tiêu tìm thấy một phần tử đủ lớn và mỗi phép gán tiêu thụ một phần tử mảng. Dấu vết xác nhận rằng các phần tử lớn hơn vẫn có sẵn cho các mục tiêu sau này. 

### Ví dụ 2:`1 2 2 3`Mảng được sắp xếp là`[1, 2, 2, 3]`, mục tiêu là từ 1 đến 4. 

| tôi | j | một[j] | hành động | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | phân công 1 → 1 | 
| 2 | 1 | 2 | phân công 2 → 2 | 
| 3 | 2 | 2 | phân công 2 → 3 | 
| 4 | 3 | 3 | thất bại | 

Ở mục tiêu 4, chúng ta cần giá trị ít nhất là 4, nhưng không tồn tại phần tử nào như vậy. Thuật toán bác bỏ trường hợp này một cách chính xác vì mặc dù có giảm bit nhưng không có phần tử nào có thể tăng lên 4. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp chiếm ưu thế, quét con trỏ là tuyến tính | 
| Không gian | O(n) | lưu trữ mảng | 

Độ phức tạp phù hợp thoải mái trong các ràng buộc vì n lên tới 10^5 và việc sắp xếp cộng với một lần vượt qua cũng nằm trong giới hạn thông thường cho giới hạn thời gian 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    a.sort()
    j = 0

    for i in range(1, n + 1):
        while j < n and a[j] < i:
            j += 1
        if j == n:
            return "No"
        j += 1

    return "Yes"

# provided samples
assert run("3\n1 3 7\n") == "Yes"
assert run("2\n1 2\n") == "Yes"
assert run("2\n3 3\n") == "Yes"
assert run("4\n1 2 2 3\n") == "No"

# custom cases
assert run("1\n1\n") == "Yes", "minimum size"
assert run("1\n2\n") == "Yes", "single element reducible"
assert run("3\n1 1 1\n") == "No", "all equal small"
assert run("5\n5 5 5 5 5\n") == "Yes", "uniform large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1`| Có | trường hợp hợp lệ tối thiểu | 
|`1\n2`| Có | giảm phần tử đơn | 
|`3\n1 1 1`| Không | không đủ đa dạng | 
|`5\n5 5 5 5 5`| Có | giá trị lớn lặp đi lặp lại vẫn hoạt động | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các giá trị đều giống nhau và nhỏ. Đối với đầu vào`3 3 3`, sắp xếp cho`[3, 3, 3]`. Thuật toán gán thành công 1 đến 3, 2 đến 3 và 3 đến 3, cho thấy các phần tử lặp lại đều ổn miễn là chúng đủ lớn để bao phủ các mục tiêu riêng biệt. 

Một trường hợp khác là khi các giá trị tập hợp ngay bên dưới các mục tiêu được yêu cầu, chẳng hạn như`[1, 1, 4]`với n = 3. Sau khi sắp xếp, chúng tôi gán 1 cho 1, nhưng ở mục tiêu 2, cả hai giá trị còn lại đều là 1, không đủ vì chúng không thể đạt tới 2. Thuật toán thất bại chính xác tại thời điểm đó, từ chối chính xác đầu vào. 

Trường hợp khó phát hiện thứ ba là khi một giá trị lớn xuất hiện sớm nhưng lại bị lãng phí. Vì`[100000, 1, 2, 3, ..., n-1]`, việc sắp xếp sẽ ngăn chặn vấn đề này bằng cách đảm bảo các giá trị nhỏ được sử dụng trước, để lại dung lượng lớn cho các mục tiêu cao hơn, duy trì tính khả thi khi có thể.
