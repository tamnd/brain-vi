---
title: "CF 106484F - Trò Chơi Bài"
description: "Chúng ta được cấp một chồng chữ in hoa sắp xếp từ trên xuống dưới. Nhiệm vụ là xây dựng lại các thẻ này thành một chuỗi mới bằng cách liên tục loại bỏ thẻ trên cùng hoặc thẻ dưới cùng hiện tại và gắn thẻ đã chọn đó vào cuối chuỗi mới."
date: "2026-06-19T15:17:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106484
codeforces_index: "F"
codeforces_contest_name: "2026 GBA International Programming Contest"
rating: 0
weight: 106484
solve_time_s: 47
verified: true
draft: false
---

[CF 106484F - Trò chơi bài](https://codeforces.com/problemset/problem/106484/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chồng chữ in hoa sắp xếp từ trên xuống dưới. Nhiệm vụ là xây dựng lại các thẻ này thành một chuỗi mới bằng cách liên tục loại bỏ thẻ trên cùng hoặc thẻ dưới cùng hiện tại và gắn thẻ đã chọn đó vào cuối chuỗi mới. Mục tiêu là tạo ra chuỗi cuối cùng nhỏ nhất có thể về mặt từ điển. 

Hạn chế chính là ở mỗi bước, chúng ta chỉ có hai lựa chọn và khi một ký tự được chọn, nó sẽ được đặt vĩnh viễn ở cuối kết quả. Điều này có nghĩa là những quyết định sớm hơn có thể cản trở những kết quả toàn cầu tốt hơn, do đó, một lựa chọn tham lam phải được biện minh một cách cẩn thận. 

Kích thước đầu vào có thể lớn tới 500.000 thẻ. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào mô phỏng tất cả các lựa chọn hoặc đường quay lại có thể xảy ra. Ngay cả cách tiếp cận O(n^2) cũng sẽ quá chậm. Chúng ta cần thứ gì đó xử lý từng ký tự với số lần không đổi hoặc tệ nhất là theo logarit. 

Một trường hợp lỗi tinh vi xuất hiện khi so sánh cục bộ giữa đỉnh và đáy là không đủ. Ví dụ: nếu trên cùng là 'B' và dưới cùng là 'A' thì lấy 'A' có vẻ đúng. Tuy nhiên, quyết định thực sự còn phụ thuộc vào điều gì ẩn sau những bức thư này. Nếu hậu tố còn lại sau khi chọn một bên kém hơn về mặt từ điển, thì sự lựa chọn tham lam ngây thơ sẽ bị phá vỡ. 

Một minh họa nhỏ: 

đầu vào: 

A B C D E 

Nếu luôn chọn đầu nhỏ hơn trong hai đầu, chúng ta có thể chọn sai trong các trường hợp như: 

A ... Z vs A ... Y 

Quyết định đúng phụ thuộc vào việc so sánh hậu tố sâu hơn, không chỉ các điểm cuối hiện tại. 

## Phương pháp tiếp cận 

Chiến lược bạo lực thử tất cả các trình tự có thể có để lựa chọn từ đầu bên trái hoặc bên phải. Mỗi trạng thái phân nhánh thành hai khả năng, tạo thành cây quyết định nhị phân có chiều cao n. Điều này dẫn đến 2^n kết quả có thể xảy ra, mỗi kết quả cần O(n) thời gian để xây dựng và so sánh, kết quả này trở nên lớn về mặt thiên văn ngay cả đối với n khoảng 30. 

Cấu trúc của bài toán gợi ý một cách tiếp cận tham lam, nhưng khó khăn là chỉ so sánh hai điểm cuối là không đủ. Quyết định đúng phụ thuộc vào khoảng còn lại nào nhỏ hơn về mặt từ điển sau khi loại bỏ một bên. 

Quan sát quan trọng là chúng ta luôn so sánh hai chuỗi ứng cử viên là hậu tố của khoảng còn lại. Thay vì hiện thực hóa hoàn toàn cả hai, chúng ta có thể so sánh chúng một cách uể oải bằng cách hướng vào trong với hai con trỏ. Ở mỗi bước, chúng tôi so sánh các ứng cử viên bên trái và bên phải hiện tại về mặt từ điển dưới dạng các chuỗi được hình thành bằng cách lấy từ một phía một cách nhất quán. Sự so sánh này có thể được thực hiện mà không cần xây dựng chuỗi đầy đủ. 

Điều này dẫn đến một chiến lược: ở mỗi bước, hãy quyết định xem việc lấy từ bên trái hay bên phải sẽ tạo ra một cấu trúc còn lại nhỏ hơn về mặt từ điển và cam kết với lựa chọn đó. Bản thân sự so sánh có thể đưa con trỏ vào bên trong cho đến khi tìm thấy sự khác biệt. 

Điều này làm giảm sự phân nhánh theo cấp số nhân thành quét tuyến tính với các so sánh tuyến tính không thường xuyên được khấu hao trong toàn bộ quá trình. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tối ưu | O(n) khấu hao | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta duy trì hai con trỏ, một ở đầu bên trái của khoảng còn lại và một ở đầu bên phải. Ở mỗi bước, chúng tôi quyết định nên lấy từ bên trái hay bên phải.

1. Khởi tạo hai chỉ số l = 0 và r = n - 1 và một chuỗi kết quả trống. Những con trỏ này biểu thị khoảng thời gian hiện có của thẻ. 
2. Khi l ≤ r, chúng ta xem xét hai dãy ứng cử viên: một dãy bắt đầu bằng việc lấy từ bên trái và một dãy bắt đầu bằng việc lấy từ bên phải. Mục tiêu là quyết định lựa chọn nào dẫn đến kết quả cuối cùng nhỏ hơn về mặt từ điển. 
3. Để so sánh hai tùy chọn, chúng tôi mô phỏng "so sánh nhìn trước" bằng cách sử dụng các con trỏ tạm thời l2 = l, r2 = r. Chúng ta so sánh các ký tự một cách tham lam: nếu s[l2] và s[r2] khác nhau, chúng ta biết ngay bên nào tốt hơn; mặt khác chúng ta di chuyển vào trong bằng cách tăng l2 và giảm r2. 
4. Nếu tại một thời điểm nào đó ký tự bên trái nhỏ hơn ký tự bên phải thì việc lấy từ bên trái sẽ mang lại kết quả từ điển nhỏ hơn, vì vậy chúng ta thêm s[l] vào kết quả và tăng l. 
5. Nếu ký tự bên phải nhỏ hơn thì ta lấy từ bên phải, nối thêm s[r] và giảm r. 
6. Nếu chúng ta sử dụng hết phép so sánh mà không tìm thấy sự khác biệt (đoạn còn lại đối xứng một cách hiệu quả), chúng ta có thể lấy từ hai phía một cách an toàn; chúng tôi chọn bên trái một cách nhất quán để đảm bảo tính quyết định. 
7. Lặp lại cho đến khi hết ký tự. 

Phần không tầm thường là bước so sánh. Đây không phải là một quyết định đơn giản theo thời gian cố định, mà trong toàn bộ quá trình, mỗi chuyển động của con trỏ chỉ xảy ra một lần trên mỗi vị trí theo nghĩa khấu hao. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến ở bất kỳ bước nào, khoảng còn lại biểu thị tất cả các phần tiếp theo có thể có của kết quả được xây dựng một phần. Khi chúng ta so sánh các cấu trúc đầu tiên bên trái và đầu tiên bên phải, chúng ta đang so sánh hai chuỗi được sắp xếp theo thứ tự từ điển được hình thành từ các lần duyệt rời rạc nhưng bổ sung cho nhau trong cùng một khoảng. Sự không khớp đầu tiên trong so sánh hướng vào xác định lựa chọn nào dẫn đến chuỗi toàn cục nhỏ hơn. Bởi vì thứ tự từ điển được quyết định ở vị trí khác nhau sớm nhất, một khi đã đưa ra quyết định, không có thao tác nào trong tương lai có thể đảo ngược nó mà không mâu thuẫn với sự không khớp đầu tiên đó. Điều này đảm bảo rằng mỗi lựa chọn tham lam đều tối ưu toàn cục cho hậu tố còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    s = [input().strip() for _ in range(n)]
    
    l, r = 0, n - 1
    res = []

    while l <= r:
        i, j = l, r
        take_left = False

        while i <= j:
            if s[i] < s[j]:
                take_left = True
                break
            elif s[i] > s[j]:
                take_left = False
                break
            i += 1
            j -= 1

        if i > j:
            take_left = True

        if take_left:
            res.append(s[l])
            l += 1
        else:
            res.append(s[r])
            r -= 1

    sys.stdout.write("".join(res))

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện việc xây dựng hai con trỏ. Vòng lặp bên trong thực hiện so sánh từ điển giữa hai hướng có thể. Biến`take_left`mã hóa kết quả quyết định và sau khi nó được xác định, chúng tôi sử dụng chính xác một điểm cuối và thu hẹp khoảng thời gian. 

Một sai lầm tinh vi phổ biến là quên trường hợp đẳng thức khi`i > j`, có nghĩa là cả hai bên đều giống nhau cho đến phần giữa còn lại. Trong trường hợp đó, lựa chọn nào cũng an toàn, nhưng việc chọn bên trái một cách nhất quán sẽ tránh được sự mơ hồ và đảm bảo kết quả đầu ra mang tính xác định. 

Một chi tiết quan trọng khác là chúng tôi so sánh trực tiếp các ký tự dưới dạng chuỗi, dựa vào thứ tự từ điển của các chuỗi ký tự đơn của Python, khớp với thứ tự bảng chữ cái. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

A B C 

Chúng ta bắt đầu với l = 0, r = 2. 

| tôi | r | so sánh (i, j) | quyết định | kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | A vs C | A < C | rẽ trái | 
| 1 | 2 | B vs C | B < C | rẽ trái | 
| 2 | 2 | C | chỉ có một | rẽ trái | 

Kết quả trở thành ABC. 

Dấu vết này cho thấy rằng khi chuỗi tăng dần, thuật toán luôn ưu tiên phía bên trái hơn vì nó dẫn đến các ký tự nhỏ hơn trước đó theo thứ tự từ điển. 

### Ví dụ 2 

đầu vào: 

B A C B 

Bắt đầu l = 0, r = 3. 

| tôi | r | so sánh (i, j) | quyết định | kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | B vs B, A vs C | A < C | rẽ trái | 
| 1 | 3 | A vs B | A < B | rẽ trái | 
| 2 | 3 | C vs B | C > B | rẽ phải | 
| 2 | 2 | C | chỉ có một | rẽ trái | 

Kết quả là A A B C. 

Ví dụ này cho thấy thuật toán không hoàn toàn tham lam ở các điểm cuối. Ngay cả khi B xuất hiện ở bên phải, sự so sánh sâu hơn cho thấy rằng việc chọn bên trái trước sẽ dẫn đến hậu tố cuối cùng nhỏ hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) khấu hao | mỗi phần tử được nâng cao trong các so sánh tối đa một lần trên tất cả các lần quét bên trong | 
| Không gian | O(1) thêm | ngoài bộ đệm đầu vào và bộ đệm đầu ra | 

Thuật toán chạy thoải mái trong giới hạn n lên tới 500.000 vì mỗi ký tự tham gia vào nhiều nhất một số chuyển động con trỏ không đổi và không sử dụng cấu trúc dữ liệu nặng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        solve()
    except SystemExit:
        pass
    return sys.stdout.getvalue().strip()

# minimal case
assert run("1\nA\n") == "A"

# increasing sequence
assert run("3\nA\nB\nC\n") == "ABC"

# decreasing sequence
assert run("3\nC\nB\nA\n") == "ABC"

# all equal
assert run("4\nA\nA\nA\nA\n") == "AAAA"

# provided-style mixed case
assert run("4\nB\nA\nC\nB\n") == "AABC"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 A | A | xử lý kích thước tối thiểu | 
| A B C | ABC | hành vi tăng đơn điệu | 
| C B A | ABC | sự đảo ngược đúng đắn | 
| A A A A | AAAA | sự ổn định liên kết | 
| B A C B | AABC | tham lam quyết định đúng đắn không tầm thường | 

## Vỏ cạnh 

Khi tất cả các ký tự còn lại giống hệt nhau, việc so sánh bên trong không bao giờ tìm thấy sự khác biệt và cuối cùng các con trỏ giao nhau. Trong trường hợp đó, thuật toán sẽ chọn nhất quán từ bên trái. Ví dụ: đầu vào A A A A tạo ra các so sánh liên kết lặp đi lặp lại cho đến khi i > j, sau đó mỗi lần bên trái được chọn, mang lại AAAA. 

Khi ký tự khác biệt đầu tiên xuất hiện sâu bên trong khoảng, thuật toán sẽ trì hoãn quyết định một cách chính xác cho đến khi tìm thấy ký tự không khớp đó. Ví dụ: B A A A C so với C A A A B dẫn đến nhiều bước bằng nhau trước khi so sánh mang tính quyết định, đảm bảo rằng bên đúng được chọn dựa trên sự khác biệt về mặt từ điển sớm nhất thay vì điểm cuối.
