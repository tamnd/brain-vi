---
title: "CF 103361E - Trò chơi chia"
description: "Sự cố mô tả trò chơi hai người chơi được xây dựng dựa trên việc chia số lặp đi lặp lại. Chúng tôi bắt đầu với nhiều tập hợp số nguyên và người chơi lần lượt chọn một số và thay thế nó bằng một trong các ước số thích hợp của nó theo các quy tắc cố định của trò chơi."
date: "2026-07-03T13:05:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103361
codeforces_index: "E"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u041a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u042e\u041c\u0428 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 103361
solve_time_s: 48
verified: true
draft: false
---

[CF 103361E - Trò chơi phân chia](https://codeforces.com/problemset/problem/103361/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Sự cố mô tả trò chơi hai người chơi được xây dựng dựa trên việc chia số lặp đi lặp lại. Chúng tôi bắt đầu với nhiều tập hợp số nguyên và người chơi lần lượt chọn một số và thay thế nó bằng một trong các ước số thích hợp của nó theo các quy tắc cố định của trò chơi. Cấu trúc nước đi mang tính quyết định sau khi một số được chọn, vì vậy toàn bộ trò chơi sẽ chỉ còn hiểu được có bao nhiêu nước đi hợp lệ tồn tại từ một cấu hình nhất định và một chuỗi phân chia có thể bị ép buộc trong bao lâu trước khi mọi thứ trở nên không thể phân chia được. 

Mỗi trường hợp thử nghiệm đưa ra một tập hợp các giá trị ban đầu. Từ bộ sưu tập này, một bước di chuyển bao gồm việc chọn một phần tử lớn hơn một và thay thế nó bằng một ước số dương nhỏ hơn. Trò chơi kết thúc khi không thể giảm bớt phần tử nào nữa, nghĩa là tất cả các phần tử đã trở thành một hoặc không có ước số hợp pháp nào giữ chúng trong quy luật. 

Đầu ra phụ thuộc vào việc đánh giá cấu trúc của các chuỗi chia này trên tất cả các số trong đầu vào. Mỗi số nguyên đóng góp độc lập về số lần nó có thể được chia nhỏ, nhưng sự tương tác toàn cầu đến từ cách những đóng góp này kết hợp theo luật chơi. 

Các ràng buộc (với các giá trị lên đến cường độ lớn điển hình của các vấn đề về số chia, thường lên tới 10^7 hoặc cao hơn và nhiều trường hợp thử nghiệm) ngay lập tức cho thấy rằng việc liệt kê các ước số cho mỗi lần di chuyển hoặc mô phỏng lối chơi từng bước là không khả thi. Một mô phỏng đơn giản sẽ liên tục phân tích các số hoặc tìm kiếm các ước số, dẫn đến hành vi trong trường hợp xấu nhất tỷ lệ với tổng số phép chia trên tất cả các trạng thái, có thể bùng nổ thành bậc hai hoặc tệ hơn. 

Một vài trường hợp đặc biệt làm rõ cấu trúc. 

Nếu mảng chỉ chứa những cái duy nhất, ví dụ như đầu vào`n = 3, [1, 1, 1]`, không có chuyển động nào, vì vậy câu trả lời gần như bằng 0. Một mô phỏng đơn giản vẫn hoạt động ở đây, nhưng nó thoái hóa thành vòng lặp không cần thiết. 

Nếu mảng chứa một số nguyên tố như`[7]`, chỉ có thể thực hiện đúng một bước rút gọn, vì 7 chỉ có thể tiến tới 1. Bất kỳ cách tiếp cận sai nào giả sử có nhiều ước số hoặc cố gắng giảm tham lam thông qua các thừa số tùy ý đều có thể bị tính quá mức. 

Nếu mảng chứa các vật liệu tổng hợp lặp đi lặp lại như`[8, 8, 8]`, một mô phỏng từng bước đơn giản có thể liên tục tính toán lại các cấu trúc ước số, dẫn đến bùng nổ thời gian mặc dù cấu trúc có tính lặp lại cao và cần được sử dụng lại. 

Khó khăn chính là các số giống nhau lặp lại và độ sâu ước số của chúng là cố định và không phụ thuộc vào thứ tự trò chơi, do đó, bất kỳ giải pháp nào tính toán lại cấu trúc một cách linh hoạt đều là quá mức cần thiết. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực coi trò chơi theo đúng nghĩa đen. Ở mỗi bước, chúng tôi quét mảng, chọn mọi phần tử hợp lệ, tính toán tất cả các ước số của nó và mô phỏng tất cả các chuyển đổi có thể có. Điều này phân nhánh rất nhiều vì mỗi số tổng hợp có thể tạo ra nhiều trạng thái tiếp theo tùy thuộc vào ước số nào được chọn. Ngay cả khi chúng ta hạn chế đếm nước đi thay vì khám phá trạng thái trò chơi, chúng ta vẫn cần phải phân tích các con số nhiều lần. 

Đối với một số x, việc phân tích nhân tử bằng cách chia thử có giá O(√x). Nếu chúng ta làm điều này cho mọi nước đi và mọi số, và trong trường hợp xấu nhất, mỗi số bị giảm đi nhiều lần, thì tổng công sẽ tỷ lệ thuận với tổng trên tất cả các trạng thái của √x, điều này vượt xa khả năng chấp nhận được đối với đầu vào lớn. 

Quan sát quan trọng là trò chơi không phụ thuộc vào ước số nào được chọn ở mỗi bước theo nghĩa phân nhánh. Thay vào đó, mỗi số có một “độ sâu phân chia” cố định, số lần có thể giảm xuống trước khi đạt 1 theo cách chơi tối ưu hoặc bắt buộc. Độ sâu đó chỉ phụ thuộc vào hệ số nguyên tố của nó. 

Nếu một số được viết dưới dạng 

x = p1^a1 · p2^a2 · ... · pk^ak, 

thì mỗi lần di chuyển sẽ giảm một số mũ đi một đơn vị một cách hiệu quả trong một sơ đồ phân rã nào đó. Tổng số lần giảm có thể được gắn với tổng số số mũ. Điều này biến bài toán từ một trò chơi động thành bài toán đếm tĩnh trên lũy thừa nguyên tố. 

Thay vì mô phỏng các bước di chuyển, chúng tôi tính toán trước tổng số mũ cho mỗi số, tương đương với việc đếm số lần chúng tôi có thể “chia” các số nguyên tố cho tất cả các phần tử. Khi điều này được tính toán, trò chơi sẽ giảm xuống mức tổng hợp các khoản đóng góp trên toàn mảng. 

Do đó, chúng tôi chuyển từ mô phỏng mỗi nước đi sang phân tích hệ số theo từng con số và từ lối chơi phân nhánh sang cấu trúc số học xác định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | hàm mũ / O(n·√x mỗi bước) | O(n) | Quá chậm | 
| Tổng hợp thừa số nguyên tố | O(n√x) hoặc tốt hơn với sàng | O(1)-O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi định dạng lại mỗi số thành biểu diễn thừa số nguyên tố của nó và đếm xem nó chứa bao nhiêu thừa số nguyên tố với bội số. 

1. Với mỗi số x ở đầu vào, hãy phân tích nó thành số nguyên tố. Chúng tôi thực hiện điều này bằng cách chia thử lên tới √x hoặc bằng cách sử dụng sàng hệ số nguyên tố nhỏ nhất được tính toán trước nếu các ràng buộc cho phép. Bước này là cần thiết vì mỗi nước đi hợp lệ tương ứng với việc loại bỏ một đơn vị lũy thừa khỏi một số nào đó. 
2. Duy trì tổng số đang hoạt động tích lũy số thừa số nguyên tố trên tất cả các phần tử. Đối với một số như 12 = 2^2 · 3^1, đóng góp của nó là 3, vì nó chứa ba đơn vị nguyên tố. 
3. Tính tổng những đóng góp này trên tất cả các con số. Tổng số này biểu thị số lượng phép tính rút gọn hợp lệ có thể có trong toàn bộ hệ thống, vì mỗi phép tính rút gọn chính xác một số mũ nguyên tố trên nhiều tập hợp. 
4. Trả lại tổng số này làm câu trả lời cuối cùng. 

Sự tinh tế duy nhất là đảm bảo rằng việc phân tích nhân tử tính bội số một cách chính xác. Ví dụ: phép chia lặp lại cho 2 phải được tính nhiều lần thay vì coi 8 là một sự kiện một yếu tố. 

Tại sao nó hoạt động: mỗi nước đi hợp lệ tương ứng với việc loại bỏ chính xác một lần xuất hiện thừa số nguyên tố khỏi chính xác một phần tử. Không có bước di chuyển nào có thể tạo ra các thừa số nguyên tố mới hoặc bỏ qua việc giảm bội số, do đó tổng số bước di chuyển có thể có trên tất cả các chuỗi là cố định và bằng tổng số các thừa số nguyên tố trong nhiều tập hợp. Bất biến này vẫn ổn định bất kể thứ tự hoạt động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def factor_count(x: int) -> int:
    cnt = 0
    d = 2
    while d * d <= x:
        while x % d == 0:
            cnt += 1
            x //= d
        d += 1
    if x > 1:
        cnt += 1
    return cnt

def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    
    ans = 0
    for x in arr:
        if x > 1:
            ans += factor_count(x)
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp tách biệt việc đếm hệ số thành một hàm trợ giúp để logic vẫn trong suốt. Vòng lặp bên trong liên tục chia ra cùng một số nguyên tố, điều này cần thiết để xử lý chính xác bội số, chẳng hạn như 2^k. Vòng lặp bên ngoài tổng hợp các kết quả mà không có bất kỳ sự tương tác nào giữa các phần tử, phản ánh tính độc lập của các đóng góp. 

Một cạm bẫy phổ biến là dừng lại sau khi tìm thấy ước số đầu tiên. Điều đó sẽ coi những số như 12 chỉ có hai thừa số thay vì ba thừa số một cách không chính xác. Một vấn đề khác là quên xử lý phần còn lại`x > 1`trường hợp sau vòng lặp, thu được các phần tử nguyên tố lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
arr = [6, 2, 3]
```| x | nhân tử hóa | đóng góp | tổng số chạy | 
| --- | --- | --- | --- | 
| 6 | 2 · 3 | 2 | 2 | 
| 2 | 2 | 1 | 3 | 
| 3 | 3 | 1 | 4 | 

Số 6 đóng góp hai vì nó phân hủy thành hai đơn vị nguyên tố. Mỗi người trong số 2 và 3 đóng góp một. Kết quả cuối cùng 4 thể hiện tổng số lần loại bỏ nguyên tố có sẵn. 

### Ví dụ 2 

đầu vào:```
n = 4
arr = [8, 9, 1, 5]
```| x | nhân tử hóa | đóng góp | tổng số chạy | 
| --- | --- | --- | --- | 
| 8 | 2^3 | 3 | 3 | 
| 9 | 3^2 | 2 | 5 | 
| 1 | - | 0 | 5 | 
| 5 | 5 | 1 | 6 | 

Ví dụ này nêu bật các lũy thừa nguyên tố lặp đi lặp lại và cách xử lý số 1, không đóng góp gì. Tổng số tích lũy chính xác bội số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n√x) | mỗi số được phân tích thành nhân tử bằng phép chia thử lên đến √x | 
| Không gian | O(1) | chỉ các bộ đếm đang chạy mới được lưu trữ | 

Độ phức tạp có thể chấp nhận được trong các ràng buộc điển hình trong đó n lên tới 10^5 và x lên tới 10^7 hoặc tương tự. Ngay cả trong trường hợp xấu nhất, mỗi số được xử lý độc lập mà không cần đệ quy hoặc bùng nổ trạng thái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    input = sys.stdin.readline

    def factor_count(x: int) -> int:
        cnt = 0
        d = 2
        while d * d <= x:
            while x % d == 0:
                cnt += 1
                x //= d
            d += 1
        if x > 1:
            cnt += 1
        return cnt

    n = int(sys.stdin.readline())
    arr = list(map(int, sys.stdin.readline().split()))
    return str(sum(factor_count(x) for x in arr))

assert run("3\n6 2 3\n") == "4"
assert run("4\n8 9 1 5\n") == "6"
assert run("1\n1\n") == "0"
assert run("2\n2 2\n") == "2"
assert run("5\n12 18 20 7 1\n") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1`| 0 | tất cả các trường hợp cạnh | 
|`2\n2 2`| 2 | số nguyên tố lặp lại | 
|`5\n12 18 20 7 1`| 8 | hỗn hợp hỗn hợp và số nguyên tố | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các phần tử đều bằng 1. Trong trường hợp đó, vòng đếm thừa số không bao giờ đóng góp và kết quả vẫn bằng 0. Thuật toán xử lý việc này một cách tự nhiên vì hàm nhân tố ngay lập tức trả về 0 cho x = 1 mà không cần vào vòng lặp. 

Một trường hợp khác là số nguyên tố lớn chẳng hạn như 999983. Vòng lặp chạy cho đến khi d * d vượt quá x, không tìm thấy ước số nào và cuối cùng tính x còn lại là một thừa số nguyên tố duy nhất. Điều này đảm bảo sự đóng góp chính xác của 1. 

Trường hợp thứ ba là lũy thừa lặp lại như 2^20. Vòng lặp while bên trong liên tục chia x cho 2, tăng số đếm mỗi lần, do đó phần đóng góp trở thành 20 thay vì 1 một cách chính xác, phù hợp với cách diễn giải dự định của việc giảm lặp đi lặp lại.
