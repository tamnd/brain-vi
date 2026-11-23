---
title: "CF 478A - Cược Ban Đầu"
description: "Chúng ta có trạng thái cuối cùng của một hệ thống có năm người tham gia bắt đầu trò chơi theo một quy tắc rất cứng nhắc: mọi người bắt đầu với cùng một số xu dương chưa xác định, gọi nó là $b$. Sau lần khởi tạo này, tiền chỉ được di chuyển giữa những người chơi."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 478
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 273 (Div. 2)"
rating: 1100
weight: 478
solve_time_s: 59
verified: true
draft: false
---

[CF 478A - Đặt cược ban đầu](https://codeforces.com/problemset/problem/478/A) 

**Đánh giá:** 1100 
**Thẻ:** triển khai 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có trạng thái cuối cùng của một hệ thống có năm người tham gia bắt đầu trò chơi theo một quy tắc rất cứng nhắc: mọi người đều bắt đầu với cùng một số xu dương chưa xác định, hãy gọi nó là$b$. Sau lần khởi tạo này, tiền chỉ được di chuyển giữa những người chơi. Không có đồng tiền nào được tạo ra hoặc bị phá hủy trong quá trình này; mọi thao tác chỉ đơn giản là chuyển một đồng xu từ người này sang người khác. 

Cuối cùng, chúng tôi quan sát xem mỗi người trong số năm người chơi có bao nhiêu xu. Từ lần phân phối cuối cùng này, chúng tôi cần xác định xem liệu có khả năng tất cả người chơi ban đầu có cùng số xu dương hay không và nếu có thì hãy khôi phục giá trị ban đầu đó$b$. Nếu không có cách nào giải thích được trạng thái cuối cùng theo những quy tắc đó thì chúng ta phải báo cáo rằng điều đó là không thể. 

Hạn chế cơ cấu chính là bảo tồn tổng số tiền. Vì việc chuyển tiền không làm thay đổi tổng số xu nên tổng giá trị cuối cùng phải bằng tổng giá trị ban đầu, tức là$5b$. Điều này ngay lập tức ngụ ý rằng tổng tổng của mảng cuối cùng phải chia hết cho 5 và thương sẽ là giá trị ứng cử viên của$b$. 

Các ràng buộc cực kỳ nhỏ: mỗi số cuối cùng tối đa là 100 và chỉ có năm giá trị. Điều này loại bỏ mọi nhu cầu về cấu trúc dữ liệu phức tạp hoặc tối ưu hóa. Tính toán trực tiếp là đủ. 

Sự tinh tế chính nằm ở việc phân biệt phân phối lại hợp lệ với phân phối lại không hợp lệ. Ví dụ, một trạng thái như$1, 1, 1, 1, 1$hợp lệ với$b = 1$. Một trạng thái như$2, 2, 2, 2, 2$hợp lệ với$b = 2$. Nhưng một trạng thái như$1, 1, 1, 1, 2$có tổng số là 6, không chia hết cho 5 và do đó không thể có giá trị ban đầu bằng nhau bất kể chuyển nhượng. 

Một trường hợp dễ gây hiểu lầm hơn là trường hợp tổng chia hết cho 5 nhưng lý luận tiêu cực có thể cho thấy điều đó là không thể. Ví dụ,$0, 0, 0, 0, 5$có tổng 5, vậy$b = 1$. Điều này hợp lệ: một người chơi có thể quyên góp tất cả tiền xu. Bất kỳ cách tiếp cận nào cố gắng suy luận về tính khả thi của từng cá nhân mà không sử dụng tổng rủi ro bảo toàn sẽ làm phức tạp quá mức những gì về cơ bản là một điều kiện số học duy nhất. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả các giá trị ban đầu có thể$b$từ 1 đến 100 và mô phỏng xem liệu chúng ta có thể phân phối lại tiền xu từ trạng thái ban đầu gồm năm cọc bằng nhau để đạt được cấu hình mục tiêu hay không. Mặc dù điều này đơn giản về mặt khái niệm nhưng nó ẩn chứa một vấn đề con khó khăn: xác định khả năng tiếp cận trong quá trình chuyển giao tùy ý. Vì tiền xu có thể được di chuyển tự do giữa hai người chơi bất kỳ nên không gian trạng thái lớn nhưng chúng tôi không bị hạn chế bởi cấu trúc biểu đồ hoặc giới hạn di chuyển. Do đó, mô phỏng sẽ yêu cầu xây dựng lại trình tự chuyển giao, điều này là không cần thiết. 

Sự đơn giản hóa quan trọng là nhận ra rằng các giao dịch chuyển tiền sẽ bảo toàn tổng số tiền một cách chính xác. Điều này thu gọn vấn đề từ một câu hỏi về khả năng tiếp cận động thành một điều kiện số học tĩnh. Cấu hình cuối cùng hợp lệ khi và chỉ khi tổng của nó bằng$5b$, Ở đâu$b$phải là số nguyên dương. 

Do đó, toàn bộ vấn đề quy về việc tính tổng và kiểm tra tính chia hết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(trạng thái hoặc không xác định) | O(1) | Quá mức cần thiết / không cần thiết | 
| Kiểm tra tổng và chia hết | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc năm số nguyên biểu thị số xu cuối cùng của mỗi người chơi. 
2. Tính tổng số tiền của chúng. Tổng này đại diện cho tổng số xu trong hệ thống, không bao giờ thay đổi trong suốt trò chơi. 
3. Kiểm tra xem tổng có chia hết cho 5 hay không. Nếu không, không thể phân phối ban đầu bằng nhau vì$5b$tổng phải bằng nhau. 
4. Nếu chia hết thì tính$b = \frac{\text{sum}}{5}$. 
5. Đảm bảo$b > 0$. Nếu tổng bằng 0, tất cả người chơi đều kết thúc với số tiền bằng 0, điều này mâu thuẫn với yêu cầu đặt cược ban đầu phải hoàn toàn dương. 
6. Đầu ra$b$nếu hợp lệ, nếu không thì xuất ra -1. 

### Tại sao nó hoạt động 

Bất biến là bảo toàn tổng số tiền xu. Mọi hoạt động trong trò chơi đều là sự chuyển giao đơn vị giữa những người chơi, giúp bảo toàn tổng số tiền. Do đó tổng cuối cùng phải bằng tổng ban đầu một cách chính xác. Vì cấu hình ban đầu là năm giá trị giống hệt nhau$b$, tổng số phải là$5b$. Điều này xác định duy nhất$b$bất cứ khi nào một giải pháp tồn tại và bất kỳ vi phạm nào về quy tắc chia hết hoặc tích cực ngay lập tức chứng tỏ là không thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    c = list(map(int, input().split()))
    total = sum(c)

    if total % 5 != 0:
        print(-1)
        return

    b = total // 5

    if b <= 0:
        print(-1)
        return

    print(b)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp tuân theo lý luận dựa trên bất biến. Tính toán duy nhất là tổng của năm giá trị. Việc kiểm tra tính chia hết đảm bảo tính nhất quán với mô hình phân phối ban đầu bằng nhau. Việc kiểm tra mức độ tích cực là cần thiết vì vấn đề rõ ràng yêu cầu đặt cược ban đầu là dương chứ không phải bằng 0. 

Một sai lầm phổ biến là bỏ qua điều kiện tích cực. Không có nó, một đầu vào như`0 0 0 0 0`sẽ mang lại kết quả không chính xác$b = 0$, mặc dù các quy tắc cấm đặt cược ban đầu bằng 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 5 4 0 4
```Tổng số tiền là 15, vì vậy ứng viên$b = 3$. 

| Bước | Tổng hợp | Chia hết cho 5 | b | hợp lệ | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 15 | vâng | 3 | vâng | 

Đầu ra là 3. 

Điều này xác nhận sự phân phối lại nhất quán tồn tại vì tổng số tiền khớp với 5 cọc ban đầu bằng nhau. 

### Ví dụ 2 

đầu vào:```
1 1 1 1 2
```Tổng số là 6, không chia hết cho 5. 

| Bước | Tổng hợp | Chia hết cho 5 | b | hợp lệ | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 6 | không | - | không | 

Đầu ra là -1. 

Điều này chứng tỏ rằng mặc dù các giá trị trông “gần” đồng đều, việc bảo toàn khối lượng toàn phần ngay lập tức loại trừ nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có năm số nguyên được xử lý một lần | 
| Không gian | O(1) | Không có cấu trúc dữ liệu phụ trợ ngoài một vài biến | 

Các ràng buộc nhỏ đến mức ngay cả việc đánh giá lặp lại cũng sẽ không đáng kể, nhưng lời giải theo thời gian không đổi phù hợp trực tiếp với cấu trúc bất biến của bài toán. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# provided sample
assert run("2 5 4 0 4\n") == "3", "sample 1"

# all equal minimal valid
assert run("1 1 1 1 1\n") == "1", "all equal"

# impossible due to sum
assert run("1 1 1 1 2\n") == "-1", "sum not divisible"

# all zeros (invalid because b must be positive)
assert run("0 0 0 0 0\n") == "-1", "zero case"

# boundary-ish valid
assert run("0 0 0 0 5\n") == "1", "single pile concentration"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 5 4 0 4 | 3 | trường hợp hợp lệ tiêu chuẩn | 
| 1 1 1 1 2 | -1 | tổng không chia hết | 
| 0 0 0 0 0 | -1 | hạn chế tích cực | 
| 0 0 0 0 5 | 1 | phân phối lại cực đoan | 

## Vỏ cạnh 

Một trường hợp khó nhận thấy là khi tất cả người chơi kết thúc với số tiền bằng 0. Tổng bằng 0, chia hết cho 5, nhưng nó ngụ ý$b = 0$. Vì vấn đề yêu cầu đặt cược ban đầu là hoàn toàn tích cực nên điều này phải bị từ chối. Thuật toán xử lý việc này bằng cách kiểm tra rõ ràng$b > 0$, đảm bảo rằng số 0 không đi qua như một nghiệm hợp lệ. 

Một trường hợp khác là phân phối có độ lệch cao như`0 0 0 0 5`. Ở đây tổng là 5, cho$b = 1$. Bất biến đảm bảo tính hợp lệ vì năm đồng xu ban đầu đều có thể được chuyển cho một người chơi theo thời gian, do đó thuật toán sẽ chấp nhận nó một cách chính xác. 

Cuối cùng, bất kỳ đầu vào nào mà tổng không chia hết cho 5 sẽ bị từ chối ngay lập tức, bao gồm một phần lớn các trạng thái không hợp lệ mà không cần lý do gì thêm.
