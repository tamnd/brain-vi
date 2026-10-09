---
title: "CF 104974A - Lễ tình nhân vui vẻ"
description: "Chúng ta được cung cấp một chuỗi ngắn các điều chỉnh số nguyên được áp dụng lần lượt cho tổng số đang chạy. Mỗi giá trị có thể tăng hoặc giảm tổng tùy thuộc vào việc nó dương hay không dương, nhưng trong thực tế, chúng chỉ đơn giản được thêm vào như hiện trạng."
date: "2026-06-28T06:09:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104974
codeforces_index: "A"
codeforces_contest_name: "Codentines Day"
rating: 0
weight: 104974
solve_time_s: 72
verified: false
draft: false
---

[CF 104974A - Chúc mừng ngày lễ tình nhân](https://codeforces.com/problemset/problem/104974/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi ngắn các điều chỉnh số nguyên được áp dụng lần lượt cho tổng số đang chạy. Mỗi giá trị có thể tăng hoặc giảm tổng tùy thuộc vào việc nó dương hay không dương, nhưng trong thực tế, chúng chỉ đơn giản được thêm vào như hiện trạng. 

Câu hỏi không chỉ về trình tự đầy đủ cuối cùng. Chúng tôi được phép chọn bất kỳ tập hợp con nào trong số các giá trị này, giữ nguyên giá trị ban đầu của chúng và quyết định xem có tồn tại ít nhất một tập hợp con có tổng chính xác là 14 hay không. Nếu lựa chọn như vậy tồn tại, đầu ra là khẳng định, nếu không thì nó là âm. 

Về cơ bản, cấu trúc này hỏi xem liệu 14 có thể được hình thành dưới dạng tổng tập hợp con từ một danh sách nhỏ các số nguyên có thể bao gồm số âm, số 0 và số dương hay không. 

Ràng buộc về số phần tử là rất nhỏ, với n nhiều nhất là 19. Giới hạn này là tín hiệu chính của bài toán. Danh sách có kích thước này cho phép khám phá theo cấp số nhân trên tất cả các tập hợp con, vì tổng số tập hợp con là 2^19, tức là hơn nửa triệu một chút. Kích thước này đủ nhỏ để đánh giá trực tiếp trong giới hạn thời gian trong Python mà không cần các kỹ thuật tối ưu hóa nâng cao như các bộ bit lập trình động hoặc gặp nhau. 

Phạm vi giá trị của mỗi phần tử, từ -10 đến 19, không yêu cầu xử lý đặc biệt ngoài số học số nguyên thông thường. Tuy nhiên, điều đó có nghĩa là tổng trung gian có thể âm hoặc vượt quá 14, do đó, chiến lược cắt tỉa phải được sử dụng cẩn thận nếu áp dụng. 

Một số tình huống nguy hiểm đáng được chú ý. Nếu tất cả các số đều dương và lớn hơn 14 thì không có tập hợp con nào có thể đạt tới 14 và câu trả lời đúng ngay lập tức là số âm. Ví dụ, đầu vào`[20, 30]`rõ ràng không thể tạo thành 14. Ngược lại, nếu có một phần tử duy nhất bằng 14 thì câu trả lời ngay lập tức là khẳng định. Một trường hợp tế nhị khác là khi số không tồn tại. Các số 0 không thay đổi tổng nhưng vẫn được tính là các lựa chọn tập hợp con riêng biệt, do đó, một tập hợp con đạt đến 14 vẫn hợp lệ bất kể có thêm các số 0 hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là thử mọi tập con có thể có của các số đã cho và tính tổng của nó. Vì mỗi phần tử có hai lựa chọn, hoặc được bao gồm hoặc không được bao gồm, điều này dẫn đến 2^n khả năng. Đối với mỗi tập hợp con, chúng tôi tính tổng bằng O(n) hoặc duy trì tổng tăng dần trong quá trình liệt kê mặt nạ bit. Do đó, tổng công việc theo thứ tự n lần 2^n trong một triển khai đơn giản. 

Với n = 19, giới hạn trên này là khoảng 19 × 524.288, tức là khoảng mười triệu phép tính nguyên thủy. Trong Python, điều này vẫn có thể chấp nhận được, đặc biệt vì các phép toán cộng và bit rất rẻ. Vì vậy, ngay cả vũ lực cũng đã đủ. 

Một cách xem khác là lập trình động trên các tổng có thể đạt được, nhưng vì các giá trị bao gồm số âm, nên một DP ba lô cổ điển với độ lệch cố định sẽ trở nên cồng kềnh hơn một chút. Chúng tôi sẽ cần phải thay đổi phạm vi hoặc sử dụng một bộ để theo dõi số tiền có thể tiếp cận. Điều đó cũng có tác dụng, nhưng nó không cải thiện độ phức tạp tiệm cận so với cách liệt kê tập hợp con đơn giản trong chế độ ràng buộc này. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần tối ưu hóa ngoài việc liệt kê các tập hợp con. Ràng buộc trên n được chọn rõ ràng để làm cho việc tìm kiếm toàn diện trở thành giải pháp mong muốn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(n · 2^n) | O(1) hoặc O(n) | Đã chấp nhận | 
| DP / Bộ tổng | O(n · S) trong đó S là phạm vi tổng | O(S) | Được chấp nhận nhưng không cần thiết | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi tập hợp con là một mặt nạ nhị phân có độ dài n. Mỗi bit cho biết liệu một phần tử tương ứng có được bao gồm trong tập hợp con hay không. 

1. Đọc mảng số nguyên và lưu nó vào danh sách. Điều này cho phép chúng tôi truy cập được lập chỉ mục trực tiếp để xây dựng tập hợp con. 
2. Lặp lại tất cả các số nguyên từ 0 đến 2^n − 1. Mỗi số nguyên đại diện cho một tập hợp con của mảng. Biểu diễn nhị phân mã hóa sự bao gồm hoặc loại trừ từng phần tử. 
3. Đối với mỗi mặt nạ tập hợp con, hãy tính tổng của tất cả các phần tử có bit tương ứng được đặt. Điều này được thực hiện bằng cách quét tất cả các vị trí từ 0 đến n − 1 và kiểm tra xem bit có hoạt động hay không. Lý do tính toán lại thay vì duy trì tổng tăng dần là vì tính đơn giản và chi phí vẫn có thể chấp nhận được nếu có những hạn chế. 
4. Nếu tại bất kỳ thời điểm nào, tổng được tính bằng 14, chúng ta có thể trả về thành công ngay lập tức vì chúng ta chỉ cần tồn tại một tập hợp con hợp lệ. 
5. Nếu không có tập hợp con nào tạo ra tổng 14 sau khi sử dụng hết tất cả các mặt nạ thì chúng ta kết luận rằng điều đó là không thể. 

### Tại sao nó hoạt động 

Mỗi tập hợp con của mảng tương ứng với chính xác một mặt nạ nhị phân và mỗi mặt nạ nhị phân tương ứng với chính xác một tập hợp con. Phép song ánh này đảm bảo rằng việc lặp qua tất cả các mặt nạ sẽ liệt kê tất cả các tập hợp con có thể có mà không bỏ sót hoặc trùng lặp. Vì chúng tôi kiểm tra tổng cho từng tập hợp con một cách rõ ràng nên mọi kết hợp hợp lệ có tổng bằng 14 đều phải gặp trong quá trình lặp. Do đó, nếu tồn tại, thuật toán sẽ tìm thấy nó; nếu không tồn tại, việc liệt kê đầy đủ sẽ hoàn thành mà không dẫn đến thành công. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = input().strip().split()
    if not data:
        return
    n = int(data[0])
    arr = list(map(int, data[1:]))

    # in case input is split across lines
    while len(arr) < n:
        arr.extend(map(int, input().split()))

    target = 14

    for mask in range(1 << n):
        s = 0
        for i in range(n):
            if mask & (1 << i):
                s += arr[i]
        if s == target:
            print("YES")
            return

    print("NO")

if __name__ == "__main__":
    solve()
```Bước phân tích cú pháp tính đến định dạng đầu vào hơi bất thường trong đó các số có thể được chia thành các dòng hoặc kết hợp không nhất quán. Logic cốt lõi là phép liệt kê bitmask. Mỗi mặt nạ được diễn giải độc lập và chúng tôi tích lũy một biến tổng cục bộ`s`. Việc thoát sớm khi tìm 14 sẽ ngăn chặn việc khám phá không cần thiết khi tìm thấy tập hợp con hợp lệ. 

Chi tiết triển khai tinh tế nhằm đảm bảo rằng phân tích cú pháp đầu vào không giả định cấu trúc dòng nghiêm ngặt ngoài mã thông báo đầu tiên. Trong môi trường lập trình cạnh tranh, phân tích cú pháp phòng thủ sẽ tránh được các lỗi thời gian chạy trên khoảng cách không đúng định dạng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đầu vào tương ứng với một mảng`[3, 11]`. 

Chúng tôi liệt kê các tập hợp con: 

| mặt nạ | các yếu tố được chọn | tổng hợp | kiểm tra | 
| --- | --- | --- | --- | 
| 00 | [] | 0 | không | 
| 01 | [3] | 3 | không | 
| 10 | [11] | 11 | không | 
| 11 | [3, 11] | 14 | vâng | 

Tại mặt nạ`11`, tổng trở thành chính xác 14, do đó thuật toán dừng và xuất ra CÓ. 

Điều này chứng tỏ rằng thuật toán xác định chính xác tập hợp đầy đủ là tập hợp con hợp lệ mà không yêu cầu bất kỳ việc cắt bớt hoặc phỏng đoán nào. 

### Ví dụ 2 

Đầu vào tương ứng với`[6, -8, -4, -3, 0, 12, 17]`. 

Một phần dấu vết: 

| mặt nạ | các yếu tố được chọn | tổng hợp | kiểm tra | 
| --- | --- | --- | --- | 
| 0000001 | [6] | 6 | không | 
| 0000010 | [-8] | -8 | không | 
| 0001000 | [-3] | -3 | không | 
| 0010000 | [0] | 0 | không | 
| 0100000 | [12] | 12 | không | 
| 1000000 | [17] | 17 | không | 
| 0001010 | [-8, -3] | -11 | không | 
| 0100101 | [6, 12] | 18 | không | 
| 0100011 | [6, -8, 12] | 10 | không | 
| 0100101 | [6, 12] lại mẫu | 18 | không | 

Cuối cùng, một trong các mặt nạ tạo ra tổng 14 và quá trình liệt kê dừng lại. 

Ví dụ này cho thấy tại sao chúng ta không thể dựa vào sự lựa chọn tham lam. Chỉ riêng số dương không đảm bảo đạt được 14 và giá trị âm có thể cần thiết để điều chỉnh tổng một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 2^n) | mỗi mặt nạ tập hợp con được đánh giá bằng cách quét tối đa n phần tử | 
| Không gian | O(1) | chỉ sửa các biến phụ ngoài mảng đầu vào | 

Ràng buộc n 19 đảm bảo rằng 2^n vẫn đủ nhỏ để liệt kê đầy đủ. Trường hợp xấu nhất nằm trong giới hạn điển hình của Python, vì tổng số thao tác vẫn ở khoảng vài triệu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples (interpreted with standard formatting)
assert run("2\n3 11\n") == "YES", "sample 1"
assert run("6\n-8 -4 -3 0 12 17\n") == "YES", "sample 2"

# minimum size, single element equals target
assert run("1\n14\n") == "YES", "single element success"

# minimum size, single element not target
assert run("1\n5\n") == "NO", "single element failure"

# all negative values
assert run("3\n-1 -2 -3\n") == "NO", "all negative cannot reach 14"

# mixture requiring subset
assert run("4\n10 5 4 -5\n") == "YES", "needs combination 10+4"

# zero-heavy case
assert run("5\n0 0 0 14 1\n") == "YES", "direct presence and zeros"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| độc thân 14 | CÓ | trường hợp cơ sở khớp trực tiếp | 
| đơn 5 | KHÔNG | trường hợp tầm thường không thể | 
| tất cả tiêu cực | KHÔNG | mục tiêu tích cực không thể tiếp cận | 
| kết hợp hỗn hợp | CÓ | sự cần thiết kết hợp tập hợp con | 
| bao gồm số không | CÓ | số không không ảnh hưởng đến tính khả thi | 

## Vỏ cạnh 

Trường hợp có một phần tử duy nhất bằng 14 được xử lý ngay lập tức bằng mặt nạ tập hợp con trong đó chỉ phần tử đó được chọn. Trong quá trình lặp, mặt nạ có đúng một bit được đặt tương ứng với phần tử đó tạo ra tổng 14 và kích hoạt việc kết thúc sớm. 

Trường hợp tất cả các số đều âm chứng tỏ rằng không có tập hợp con nào có thể tăng tổng lên mục tiêu dương. Thuật toán vẫn liệt kê tất cả các tập hợp con, nhưng mọi tổng được tính vẫn không dương, do đó việc kiểm tra đẳng thức cho 14 không bao giờ được kích hoạt và đầu ra cuối cùng là KHÔNG. 

Trường hợp chứa số 0 cho thấy rằng các phần tử bổ sung không đóng góp gì sẽ không ảnh hưởng đến tính chính xác. Tập hợp con chỉ chứa phần tử 14 vẫn tồn tại giữa các mặt nạ và các số 0 bổ sung chỉ đơn giản tạo ra các tổng trùng lặp nhưng không ảnh hưởng đến việc phát hiện tập hợp con đích.
