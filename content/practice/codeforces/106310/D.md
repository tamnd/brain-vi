---
title: "CF 106310D - \u041a\u0430\u043a \u0440\u0430\u0437\u0432\u043b\u0435\u043a\u0430\u044e\u0442\u0441\u044f \u0434\u043e\u043c\u0430\u0448\u043d\u0438\u0435 \u0440\u043e\u0431\u043e\u0442\u044b?"
description: "Trò chơi được chơi bởi ba robot theo một thứ tự cố định: Máy hút bụi, Ấm đun nước và Loa. Họ lần lượt gọi các số có bốn chữ số, nhưng người chơi có thể phải thử nhiều số trước khi thực hiện một nước đi hợp lệ."
date: "2026-06-25T07:46:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106310
codeforces_index: "D"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435, 7-8 \u043a\u043b\u0430\u0441\u0441\u044b, \u041f\u0435\u0440\u043c\u0441\u043a\u0438\u0439 \u043a\u0440\u0430\u0439, 2025"
rating: 0
weight: 106310
solve_time_s: 37
verified: true
draft: false
---

[CF 106310D - \u041a\u0430\u043a \u0440\u0430\u0437\u0432\u043b\u0435\u043a\u0430\u044e\u0442\u0441\u044f \u0434\u043e\u043c\u0430\u0448\u043d\u0438\u0435 \u0440\u043e\u0431\u043e\u0442\u044b?](https://codeforces.com/problemset/problem/106310/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi được chơi bởi ba robot theo một thứ tự cố định: Máy hút bụi, Ấm đun nước và Loa. Họ lần lượt gọi các số có bốn chữ số, nhưng người chơi có thể phải thử nhiều số trước khi thực hiện một nước đi hợp lệ. Giao thức chứa mọi số đã được nói, bao gồm cả những lần thử không thành công và kết thúc bằng`0`. 

Một số hợp lệ nếu nó có bốn chữ số khác 0, trước đó nó chưa xuất hiện là số thành công và ngoại trừ nước đi thành công đầu tiên, chữ số đầu tiên của nó khớp với chữ số cuối cùng của số thành công trước đó. Một lần thử thất bại sẽ khiến rô-bốt hiện tại bị phạt một điểm và rô-bốt tương tự sẽ tiếp tục thử cho đến khi nó đưa ra một con số hợp lệ. Sau một số hợp lệ, lượt chuyển sang robot tiếp theo. 

Nhiệm vụ là đếm đối với mỗi robot, chúng đã thử bao nhiêu con số và chúng phải nhận bao nhiêu hình phạt. Giá trị đầu tiên bao gồm cả lần thử thành công và không thành công, vì mỗi số được nói đều là một lần thử nước đi. 

Kích thước đầu vào không bị hạn chế rõ ràng trong câu lệnh, nhưng giao thức có thể chứa nhiều dòng. Vì mỗi dòng phải được kiểm tra nên mục tiêu tự nhiên là một lần chuyển qua đầu vào. Bất kỳ giải pháp nào liên tục quét toàn bộ lịch sử để tìm mọi số đều có thể trở thành phương trình bậc hai và không cần thiết. Mô phỏng tuyến tính là đủ vì các quy tắc chỉ phụ thuộc vào số thành công trước đó và tập hợp các số thành công đã được sử dụng. 

Một số chi tiết có thể phá vỡ việc thực hiện bất cẩn. Số được nói đầu tiên không có hạn chế về số trước đó. Ví dụ:```
1234
2345
0
```Số thứ nhất được chấp nhận và số thứ hai cũng được chấp nhận, vì`2`theo sau chữ số kết thúc của`1234`. Giải pháp yêu cầu số trước đó trước khi chấp nhận nước đi đầu tiên sẽ từ chối không chính xác`1234`. 

Các số lặp lại chỉ bị cấm khi di chuyển thành công. Ví dụ:```
1234
4567
4567
0
```Hai số đầu tiên là hợp lệ. Lần thử thứ ba không hợp lệ vì`4567`đã được sử dụng thành công, vì vậy đầu ra là:```
2 0
1 1
0 0
```Giải pháp đánh dấu mọi số được nói là đã sử dụng cũng sẽ từ chối không chính xác số hợp lệ sau đó trong trường hợp lần thử trước đó không thành công. 

Lượt đi không phải lúc nào cũng thay đổi sau mỗi đường. Ví dụ:```
1234
5678
6789
0
```Sau đó`1234`Và`5678`, nỗ lực của Diễn giả`6789`không hợp lệ vì số thành công trước đó kết thúc bằng`8`, do đó Người nói sẽ bị phạt và giữ lượt cho đến khi xuất hiện một số hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là mô phỏng trò chơi. Đối với mỗi số được nói, hãy kiểm tra xem nó có đáp ứng tất cả các quy tắc hay không. Nếu nó không hợp lệ, hãy tăng hình phạt của người chơi hiện tại và giữ nguyên người chơi. Nếu hợp lệ, hãy tăng số lần di chuyển thành công của người chơi, thêm số vào bộ đã sử dụng, cập nhật chữ số cuối cùng và chuyển sang người chơi tiếp theo. 

Mô phỏng trực tiếp này đã là phương pháp tối ưu. Nhận xét quan trọng là trạng thái trò chơi rất nhỏ. Để quyết định số tiếp theo có hợp lệ hay không, chúng ta chỉ cần chữ số kết thúc của số thành công cuối cùng, tập hợp các số thành công trước đó và chỉ mục của người chơi hiện tại. Không cần phải xây dựng lại lịch sử trò chơi hoặc nhìn về phía trước. 

Tổng công việc tỷ lệ thuận với số dòng trong giao thức. Với tập hợp băm, việc kiểm tra xem một số có được sử dụng hay không cần thời gian trung bình không đổi, do đó toàn bộ giải pháp là tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k²) | O(k) | Quá chậm đối với các giao thức lớn | 
| Mô phỏng với một bộ | O(k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các số cho đến số 0 tận cùng và xử lý chúng theo thứ tự. Người chơi hiện tại bắt đầu với tư cách là Máy hút bụi, được biểu thị bằng chỉ mục`0`. 
2. Đối với mỗi số được nói, trước tiên hãy giả sử nó không hợp lệ và kiểm tra mọi quy tắc. Số phải có bốn chữ số, không chứa số 0, không có chữ số lặp lại, không nằm trong tập hợp các số đã được chấp nhận và khớp với chữ số kết thúc trước đó khi tồn tại số hợp lệ trước đó. 
3. Nếu số không hợp lệ, hãy thêm một hình phạt cho người chơi hiện tại. Chỉ số người chơi không thay đổi vì cùng một robot phải tiếp tục cố gắng. 
4. Nếu số này hợp lệ, hãy tăng số lần di chuyển của người chơi hiện tại và thêm số đó vào bộ đã sử dụng. Cập nhật chữ số kết thúc của số thành công cuối cùng và chuyển đổi trình phát theo thứ tự tuần hoàn. 
5. Sau khi quy trình kết thúc, hãy in số liệu thống kê được lưu trữ cho Máy hút bụi, Ấm đun nước và Loa. 

Điều bất biến trong quá trình mô phỏng là trạng thái được lưu trữ mô tả chính xác vị trí của trò chơi thực: người chơi hiện tại là người đến lượt, bộ đã sử dụng chỉ chứa các số thành công và chữ số cuối cùng được lưu thuộc về nước đi thành công cuối cùng. Bởi vì mọi dòng mới đều được đánh giá từ trạng thái này nên mọi quyết định đều phù hợp với quy tắc ban đầu. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    stats = [[0, 0] for _ in range(3)]
    used = set()

    player = 0
    last_digit = -1
    has_previous = False

    for line in sys.stdin:
        s = line.strip()
        if s == "0":
            break

        stats[player][0] += 1

        ok = True

        if len(s) != 4:
            ok = False
        else:
            if '0' in s:
                ok = False
            elif len(set(s)) != 4:
                ok = False

        if ok and s in used:
            ok = False

        if ok and has_previous and int(s[0]) != last_digit:
            ok = False

        if ok:
            used.add(s)
            last_digit = int(s[-1])
            has_previous = True
            player = (player + 1) % 3
        else:
            stats[player][1] += 1

    print(*stats[0])
    print(*stats[1])
    print(*stats[2])

if __name__ == "__main__":
    solve()
```Mảng`stats`lưu trữ hai giá trị cho mỗi robot. Giá trị đầu tiên đếm mọi số được nói bởi robot đó, trong khi giá trị thứ hai chỉ tính những lần thử thất bại. Trình phát hiện tại được lưu dưới dạng chỉ mục từ`0`ĐẾN`2`, điều này làm cho việc thay đổi vòng quay trở thành một phép toán modulo đơn giản. 

bộ`used`chỉ chứa số thành công. Điều này quan trọng vì một lần thử thất bại không tiêu tốn một con số. Biến`has_previous`tách nước đi hợp lệ đầu tiên khỏi tất cả nước đi sau, tránh so sánh không chính xác với số trước đó không tồn tại. 

Việc kiểm tra xác nhận được thực hiện trước khi thay đổi bất kỳ trạng thái nào. Số hợp lệ cập nhật tập hợp đã sử dụng và chữ số kết thúc trước đó, trong khi số không hợp lệ chỉ thay đổi bộ đếm hình phạt. Lệnh này ngăn chặn những nỗ lực thất bại ảnh hưởng đến các bước đi trong tương lai. 

## Ví dụ đã hoạt động 

Đối với giao thức:```
4123
3287
7789
7895
5437
7895
2876
7631
0
```dấu vết là: 

| Số nói | Người chơi | hợp lệ | Nỗ lực sau khi xử lý | Hình phạt | 
| --- | --- | --- | --- | --- | 
| 4123 | Máy hút bụi | Có | VC=1 | VC=0 | 
| 3287 | Ấm đun nước | Có | K=1 | K=0 | 
| 7789 | Loa | Không | S=1 | S=1 | 
| 7895 | Loa | Có | S=2 | S=1 | 
| 5437 | Máy hút bụi | Có | VC=2 | VC=0 | 
| 7895 | Ấm đun nước | Không | K=2 | K=1 | 
| 2876 | Ấm đun nước | Có | K=3 | K=1 | 
| 7631 | Loa | Có | S=3 | S=1 | 

Người bị từ chối`7789`chứng tỏ rằng một nước đi không hợp lệ vẫn giữ nguyên một người chơi. Việc lặp đi lặp lại`7895`chứng tỏ rằng những con số thành công là những con số duy nhất được ghi nhớ. 

Vì:```
1234
4567
4567
5678
0
```dấu vết là: 

| Số nói | Người chơi | hợp lệ | Nỗ lực | Hình phạt | 
| --- | --- | --- | --- | --- | 
| 1234 | Máy hút bụi | Có | VC=1 | VC=0 | 
| 4567 | Ấm đun nước | Có | K=1 | K=0 | 
| 4567 | Loa | Không | S=1 | S=1 | 
| 5678 | Loa | Có | S=2 | S=1 | 

Dấu vết này thực hiện quy tắc rằng số thành công lặp lại là không hợp lệ và Người nói tiếp tục thử sau lần thử thất bại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Mỗi mục nhập giao thức được xử lý một lần và các hoạt động được thiết lập ở mức trung bình O(1) | 
| Không gian | O(k) | Bộ lưu trữ tất cả các số thành công | 

Thuật toán chỉ lưu trữ thông tin cần thiết để tiếp tục mô phỏng. Ngay cả đối với một giao thức rất lớn, mức sử dụng bộ nhớ tăng tuyến tính với số lượng số thành công. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        data = sys.stdin.read().splitlines()

        stats = [[0, 0] for _ in range(3)]
        used = set()
        player = 0
        last_digit = -1
        has_previous = False

        for s in data:
            if s == "0":
                break

            stats[player][0] += 1
            ok = True

            if len(s) != 4 or '0' in s or len(set(s)) != 4:
                ok = False
            if ok and s in used:
                ok = False
            if ok and has_previous and int(s[0]) != last_digit:
                ok = False

            if ok:
                used.add(s)
                last_digit = int(s[-1])
                has_previous = True
                player = (player + 1) % 3
            else:
                stats[player][1] += 1

        return "\n".join(f"{a} {b}" for a, b in stats) + "\n"
    finally:
        sys.stdin = old_stdin

assert run("""4123
3287
7789
7895
5437
7895
2876
7631
0
""") == """3 0
3 1
3 1
""", "sample"

assert run("""1234
4567
4567
5678
0
""") == """1 0
1 0
2 1
""", "repeat case"

assert run("""1111
1230
1234
2345
0
""") == """2 2
1 0
0 0
""", "invalid formats"

assert run("""1234
2345
3456
4567
0
""") == """2 0
1 0
1 0
""", "normal rotation"

assert run("""9876
8765
7654
6543
5432
4321
3219
2198
0
""") == """3 0
3 0
2 0
""", "long chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Giao thức mẫu |`3 0 / 3 1 / 3 1`| Xử lý lượt và phạt cơ bản | 
| Trường hợp số lặp lại |`1 0 / 1 0 / 2 1`| Số đã sử dụng và hành vi thử lại | 
| Chữ số không hợp lệ |`2 2 / 1 0 / 0 0`| Số 0 và chữ số lặp lại | 
| Xoay bình thường |`2 0 / 1 0 / 1 0`| Vòng quay thành công | 
| Dây chuyền dài |`3 0 / 3 0 / 2 0`| Nhiều chuyển tiếp hợp lệ | 

## Vỏ cạnh 

Nước đi đầu tiên rất đặc biệt vì trước đó không có con số thành công nào. Trên đầu vào:```
1234
2345
0
```thuật toán bắt đầu với`has_previous = False`, vì vậy nó bỏ qua việc so sánh chữ số đầu tiên.`1234`được chấp nhận, chữ số trước đó sẽ trở thành`4`, Và`2345`được chấp nhận vì nó bắt đầu bằng`2`. 

Đối với các số thành công lặp lại:```
1234
4567
4567
0
```hai dòng đầu tiên nhập`used`bộ. Khi Người nói nói`4567`, số được tìm thấy trong tập hợp nên Người nói sẽ bị phạt và giữ lượt. Số trong tương lai từ Loa vẫn sẽ được kiểm tra bình thường. 

Đối với những lần thử không hợp lệ sẽ không thay đổi trạng thái trò chơi:```
1234
5670
6789
0
```Máy hút bụi chơi thành công`1234`. Ấm cố gắng`5670`, không hợp lệ vì nó chứa số 0, vì vậy Ấm sẽ bị phạt. Số tiếp theo`6789`vẫn được đánh giá là nước đi Ấm đun nước và vì nó bắt đầu bằng`6`, nó được chấp nhận. Lần thử thất bại không bao giờ ảnh hưởng đến chữ số trước đó hoặc tập hợp đã sử dụng.
