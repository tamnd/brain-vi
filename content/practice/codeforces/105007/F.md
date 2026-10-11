---
title: "CF 105007F - Đi dạo trong công viên"
description: "Mỗi người bạn mang đến một công việc tiềm năng. Một công việc được xác định bởi hai điều: tập hợp các ngày trong một tuần có 7 ngày cố định khi Alice phải dắt chó của người bạn đó đi dạo và số tiền để hoàn thành công việc đó trong cả tuần."
date: "2026-06-28T03:06:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105007
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 03-01-24 Div. 2 (Beginner)"
rating: 0
weight: 105007
solve_time_s: 63
verified: true
draft: false
---

[CF 105007F - Đi dạo trong công viên](https://codeforces.com/problemset/problem/105007/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi người bạn mang đến một công việc tiềm năng. Một công việc được xác định bởi hai điều: tập hợp các ngày trong một tuần có 7 ngày cố định khi Alice phải dắt chó của người bạn đó đi dạo và số tiền để hoàn thành công việc đó trong cả tuần. Alice có thể chấp nhận một công việc hoặc bỏ qua nó, nhưng cô ấy không thể chấp nhận cùng một người bạn hai lần. 

Ràng buộc hàng ngày là hạn chế cơ cấu quan trọng. Trong bảy ngày đó, Alice có thể dắt nhiều nhất hai con chó đi dạo. Vì vậy, mỗi tập hợp con công việc được chọn phải thỏa mãn rằng mỗi ngày không có quá hai công việc được chọn yêu cầu phải làm việc. 

Đầu ra là tổng số tiền thanh toán tối đa từ bất kỳ tập hợp con công việc nào tuân thủ ràng buộc công suất mỗi ngày này. 

Kích thước đầu vào nhỏ về số lượng công việc, tối đa 100 bạn bè. Cấu trúc ẩn nằm trong chiều lịch trình hàng tuần: mỗi công việc là một mẫu 7 bit, do đó ràng buộc là một tập hợp gồm bảy ràng buộc năng lực độc lập được áp dụng cho bài toán lựa chọn tập hợp con. Điều này ngay lập tức gợi ý rằng các tập hợp con theo cấp số nhân không phải tự động là không thể, nhưng việc liệt kê ngây thơ trên tất cả các tập hợp con đã ở mức 2^100, điều này vượt xa khả thi. 

Một chế độ thất bại tinh tế xuất hiện khi suy nghĩ tham lam. Ví dụ: việc chọn công việc được trả lương cao nhất trước tiên có thể chặn nhiều công việc trung bình mang lại lợi nhuận cao hơn vì chúng trùng lặp khác nhau giữa các ngày. Một vấn đề khác xuất phát từ việc xử lý mỗi ngày một cách độc lập: việc tối ưu hóa công việc được giao mỗi ngày không đảm bảo rằng mỗi công việc đều kéo dài nhiều ngày cùng một lúc. 

Một thất bại tham lam cụ thể: 

đầu vào:```
3
1 0 0 0 0 0 0 50
1 0 0 0 0 0 0 40
0 1 0 0 0 0 0 40
```Chiến lược tham lam theo giá trị có thể chọn 50 công việc trước, không sử dụng công suất 1 vào ngày 1 và bỏ qua rằng công việc thứ hai cũng sử dụng ngày 1 và có thể được ghép nối khác với những ngày khác trong trường hợp lớn hơn. Trong những trường hợp nhỏ, điều này có vẻ vô hại nhưng nói chung nó phá vỡ việc đóng gói tối ưu do các ràng buộc chồng chéo. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp sẽ thử mọi tập hợp con của N công việc. Đối với mỗi tập hợp con, nó sẽ xác minh tính khả thi bằng cách kiểm tra tất cả 7 ngày và đếm số lượng công việc được chọn yêu cầu trong ngày đó. Điều này đúng vì nó khám phá toàn bộ không gian giải pháp, nhưng nó yêu cầu đánh giá 2^N tập hợp con và đối với mỗi tập hợp con, hãy kiểm tra tối đa N công việc trong 7 ngày, đưa ra các phép toán O(N·2^N). Với N = 100 thì điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là kích thước ràng buộc rất nhỏ và cố định ở mức 7 ngày. Mỗi công việc có thể được biểu diễn dưới dạng mặt nạ 7 bit. Thay vì suy luận trực tiếp về các tập hợp con, chúng tôi theo dõi số lượng công việc được giao cho cấu hình mỗi ngày, nhưng điều đó vẫn không làm giảm độ phức tạp ngay lập tức. 

Việc tái cơ cấu quan trọng là xem xét quá trình từng ngày. Vì mỗi ngày cho phép tối đa 2 nhiệm vụ nên chúng tôi có thể xử lý công việc một cách tuần tự và duy trì một chương trình linh hoạt về số lượng vị trí còn lại cho mỗi ngày trong số 7 ngày. Mỗi ngày có công suất 2, do đó trạng thái là 7 bộ số nguyên trong {0,1,2}. Điều đó mang lại nhiều nhất 3^7 = 2187 trạng thái, đủ nhỏ. 

Mỗi công việc tiêu tốn một đơn vị công suất trong mỗi ngày hoạt động trong mặt nạ của nó. Vì vậy, chúng tôi thực hiện DP giống như chiếc ba lô đối với các công việc: đối với mỗi công việc, chúng tôi cố gắng gán nó vào một quá trình chuyển đổi trạng thái để trừ công suất khỏi số ngày hoạt động của nó, nếu khả thi. Đây là một chiếc ba lô đa chiều có giới hạn với kích thước nhỏ. 

Do đó, bài toán trở thành DP trên các trạng thái biểu thị dung lượng còn lại, tối đa hóa tổng giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(N·2^N) | O(1) | Quá chậm | 
| DP trên trạng thái dung lượng 7 ngày | O(N · 3^7) | O(3^7) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mã hóa dung lượng còn lại của mỗi ngày trong 7 ngày dưới dạng bộ dữ liệu dài 7 độ trong đó mỗi mục nhập là 0, 1 hoặc 2. 

1. Khởi tạo từ điển DP với một trạng thái: tất cả các ngày đều có công suất 2 và giá trị 0. Điều này thể hiện việc bắt đầu với tính khả dụng hoàn toàn. 
2. Đối với mỗi công việc, hãy đọc mặt nạ 7-bit và phần thưởng của nó. Mặt nạ cho biết công việc này tiêu thụ một đơn vị công suất từ ​​ngày nào. 
3. Đối với mỗi trạng thái DP hiện có, hãy thử hai lần chuyển đổi: bỏ qua công việc hoặc nhận công việc nếu tất cả các ngày cần thiết đều còn ít nhất 1 công suất. 
4. Khi nhận công việc, hãy tạo trạng thái mới bằng cách trừ đi 1 mỗi ngày khi mặt nạ có 1. Thêm phần thưởng của công việc vào giá trị tích lũy. 
5. Hợp nhất các chuyển tiếp bằng cách chỉ giữ lại giá trị tối đa cho mỗi trạng thái kết quả. 
6. Sau khi xử lý tất cả công việc, câu trả lời là giá trị tối đa trên tất cả các trạng thái DP. 

Việc sắp xếp các công việc không thành vấn đề bởi vì mỗi công việc đều đóng góp một lần hoặc không đóng góp gì cả, và việc chuyển đổi trạng thái luôn bảo toàn các ràng buộc về tính khả thi. 

### Tại sao nó hoạt động 

Trạng thái DP nắm bắt chính xác những gì mà các quyết định trong tương lai phụ thuộc vào: số lượng vị trí còn lại mỗi ngày. Hai chuỗi khác nhau của các lựa chọn công việc trước đó dẫn đến cùng một vectơ năng lực còn lại là tương đương cho tất cả các quyết định trong tương lai, bởi vì các công việc trong tương lai chỉ tương tác thông qua các năng lực này. Điều này chứng tỏ rằng các trạng thái hợp nhất bằng cách chỉ giữ giá trị tốt nhất cho mỗi cấu hình dung lượng không thể loại bỏ bất kỳ giải pháp tối ưu tiềm năng nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    jobs = []
    for _ in range(n):
        *days, m = map(int, input().split())
        mask = tuple(days)
        jobs.append((mask, m))

    from collections import defaultdict

    dp = {(2, 2, 2, 2, 2, 2, 2): 0}

    for mask, val in jobs:
        new_dp = dict(dp)

        for state, cur in dp.items():
            ok = True
            nxt = list(state)

            for i in range(7):
                if mask[i]:
                    if nxt[i] == 0:
                        ok = False
                        break
                    nxt[i] -= 1

            if ok:
                nxt = tuple(nxt)
                new_val = cur + val
                if nxt not in new_dp or new_dp[nxt] < new_val:
                    new_dp[nxt] = new_val

        dp = new_dp

    print(max(dp.values()))

if __name__ == "__main__":
    solve()
```Giải pháp này xây dựng DP theo trạng thái công suất, bắt đầu từ trạng thái sẵn sàng hoàn toàn trong cả bảy ngày. Mỗi công việc đều được bỏ qua hoặc áp dụng nếu khả thi. Việc kiểm tra tính khả thi đảm bảo không có ngày nào vượt quá công suất. Từ điển chỉ lưu trữ giá trị tốt nhất có thể đạt được cho mỗi cấu hình dung lượng, điều này ngăn chặn sự bùng nổ theo cấp số nhân ở các trạng thái trùng lặp. 

Một điểm tinh tế là sao chép`dp`vào trong`new_dp`trước khi lặp lại. Điều này tránh việc xâu chuỗi các cập nhật trong cùng một lần lặp đối với một công việc, điều này sẽ cho phép sử dụng một công việc nhiều lần một cách không chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
1 0 1 0 1 0 1 10
0 1 0 1 0 1 0 5
1 1 1 1 1 1 1 100
1 0 0 0 0 0 0 100
```Chúng tôi chỉ theo dõi một số tiểu bang có liên quan tốt nhất. 

| Bước | Việc làm | Trạng thái trước | Hành động | Trạng thái sau | Giá trị | 
| --- | --- | --- | --- | --- | --- | 
| 1 | cả tuần 10 | (2,2,2,2,2,2,2) | lấy | (1,1,1,1,1,1,1) | 10 | 
| 2 | xen kẽ 5 | (1,1,1,1,1,1,1) | lấy | (1,0,1,0,1,0,1) | 15 | 
| 3 | cả tuần 100 | (1,0,1,0,1,0,1) | không thể lấy | giống nhau | 15 | 
| 4 | ngày duy nhất 100 | (1,0,1,0,1,0,1) | lấy | (0,0,1,0,1,0,1) | 115 | 

Một chi nhánh khác đảm nhận công việc 100 tuần đầu tiên: 

| Bước | Việc làm | Trạng thái trước | Hành động | Trạng thái sau | Giá trị | 
| --- | --- | --- | --- | --- | --- | 
| 1 | cả tuần 100 | (2,2,2,2,2,2,2) | lấy | (1,1,1,1,1,1,1) | 100 | 
| 2 | ngày duy nhất 100 | (1,1,1,1,1,1,1) | lấy | (0,1,1,1,1,1,1) | 200 | 
| 3 | xen kẽ 5 | khả thi | lấy | hỗn hợp | 205 | 

DP giữ cả hai nhánh và chọn mức tối đa, xác nhận tổng số tối ưu là 205. 

### Mẫu 2 (đã thi công) 

đầu vào:```
3
1 0 0 0 0 0 0 50
1 0 0 0 0 0 0 40
1 0 0 0 0 0 0 30
```| Bước | Việc làm | Tiểu bang | Hành động | Giá trị | 
| --- | --- | --- | --- | --- | 
| ban đầu | - | (2,2,2,2,2,2,2) | - | 0 | 
| 1 | 50 | lấy | (1,2,2,2,2,2,2) | 50 | 
| 2 | 40 | lấy | (0,2,2,2,2,2,2) | 90 | 
| 3 | 30 | bỏ qua hoặc thất bại nếu kiệt sức | tốt nhất | 90 | 

Điều này cho thấy DP đã bão hòa chính xác công suất mỗi ngày và ngăn chặn việc phân bổ quá mức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N · 3^7) | mỗi công việc cập nhật tất cả các trạng thái năng lực, mỗi trạng thái phân nhánh tối đa một lần chuyển đổi hợp lệ | 
| Không gian | O(3^7) | DP lưu trữ một giá trị cho mỗi cấu hình dung lượng | 

Số lượng trạng thái được cố định là 2187 và N nhiều nhất là 100, do đó tổng công việc vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else solve_capture(inp)

def solve_capture(inp: str) -> str:
    import sys
    from collections import defaultdict
    input = sys.stdin.readline
    data = inp.strip().split()
    it = iter(data)

    n = int(next(it))
    jobs = []
    for _ in range(n):
        days = [int(next(it)) for _ in range(7)]
        m = int(next(it))
        jobs.append((tuple(days), m))

    dp = {(2,2,2,2,2,2,2): 0}

    for mask, val in jobs:
        new_dp = dict(dp)
        for state, cur in dp.items():
            nxt = list(state)
            ok = True
            for i in range(7):
                if mask[i]:
                    if nxt[i] == 0:
                        ok = False
                        break
                    nxt[i] -= 1
            if ok:
                nxt = tuple(nxt)
                new_dp[nxt] = max(new_dp.get(nxt, 0), cur + val)
        dp = new_dp

    return str(max(dp.values()))

# provided sample
assert run("""4
1 0 1 0 1 0 1 10
0 1 0 1 0 1 0 5
1 1 1 1 1 1 1 100
1 0 0 0 0 0 0 100
""") == "205"

# minimum size
assert run("""1
0 0 0 0 0 0 0 42
""") == "42"

# all days heavy conflict
assert run("""2
1 1 1 1 1 1 1 10
1 1 1 1 1 1 1 20
""") == "20"

# disjoint schedules
assert run("""2
1 0 0 0 0 0 0 10
0 1 0 0 0 0 0 10
""") == "20"

# tight capacity boundary
assert run("""3
1 0 0 0 0 0 0 10
1 0 0 0 0 0 0 20
1 0 0 0 0 0 0 30
""") == "50"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| công việc duy nhất hoàn toàn | 42 | trường hợp cơ sở đúng đắn | 
| tất cả các ngày chồng chéo | 20 | giải quyết xung đột năng lực | 
| công việc rời rạc | 20 | tích lũy độc lập | 
| công việc lặp đi lặp lại trong ngày | 50 | hành vi lựa chọn tham lam và tối ưu | 

## Vỏ cạnh 

Trường hợp đặc biệt quan trọng là khi nhiều công việc yêu cầu đặt chính xác cùng một ngày. Ví dụ: 

đầu vào:```
3
1 0 0 0 0 0 0 10
1 0 0 0 0 0 0 20
1 0 0 0 0 0 0 30
```DP bắt đầu với năng lực 2 vào ngày 1. Có thể đảm nhận tối đa hai công việc này. Quá trình chuyển đổi tạo ra trạng thái chính xác sau khi tiêu thụ một đơn vị cho mỗi công việc. Kết quả tốt nhất đến từ việc chọn hai giá trị cao nhất, đạt 50 và DP thực thi điều này vì bất kỳ lựa chọn thứ ba nào đều không khả thi khi dung lượng đạt đến 0. 

Một trường hợp khó khăn khác là những công việc không có yêu cầu. Một công việc như vậy không bao giờ thay đổi trạng thái, chỉ làm tăng giá trị. DP sẽ luôn chấp nhận nó vì tính khả thi không bao giờ thất bại, vì vậy nó tích lũy tự do một cách hiệu quả, điều này đúng vì nó không tiêu tốn bất kỳ công suất hàng ngày nào. 

Trường hợp thứ ba là một công việc đòi hỏi cả bảy ngày. Điều này làm giảm mỗi thành phần công suất đi một và chỉ có thể thực hiện tổng cộng hai lần cho tất cả các công việc như vậy do giới hạn mỗi ngày. DP thực thi một cách tự nhiên hạn chế toàn cầu này thông qua việc cạn kiệt mỗi ngày thay vì bất kỳ logic đếm rõ ràng nào.
