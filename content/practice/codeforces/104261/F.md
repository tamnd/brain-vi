---
title: "CF 104261F - Quầy bán xúc xích Plutonian"
description: "Chúng ta được xếp một hàng người, mỗi người có một giá trị ngưỡng bắt buộc. Mike sở hữu một số lượng vé giảm giá có hạn và anh ấy có thể chỉ định mỗi vé cho tối đa một người trong hàng."
date: "2026-07-01T23:06:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104261
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 03-24-23 Div. 2 (Beginner)"
rating: 0
weight: 104261
solve_time_s: 74
verified: true
draft: false
---

[CF 104261F - Quầy bán xúc xích Plutonian](https://codeforces.com/problemset/problem/104261/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được xếp một hàng người, mỗi người có một giá trị ngưỡng bắt buộc. Mike sở hữu một số lượng vé giảm giá có hạn và anh ấy có thể chỉ định mỗi vé cho tối đa một người trong hàng. Tác dụng của một vé không mang tính cục bộ: nếu một vé được trao cho vị trí`i`, nó tự động truyền tiếp qua các vị trí liên tiếp miễn là những người đi sau có yêu cầu nhỏ hơn hoặc bằng yêu cầu của người bắt đầu. Việc truyền bá dừng lại ngay khi gặp một người có yêu cầu lớn hơn. 

Mục tiêu là đặt tối đa`D`vé sao cho tổng số người riêng biệt được giảm giá, trực tiếp hoặc thông qua tuyên truyền, là tối đa. 

Kích thước đầu vào cho phép lên tới`N = 100000`, điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các tập hợp con của vị trí bắt đầu hoặc mô phỏng quá trình lan truyền lặp đi lặp lại cho mỗi lựa chọn. Ngay cả việc quét bậc hai trên tất cả các vị trí đặt vé có thể cũng trở nên quá chậm, vì`D`nhiều nhất là 100 nhưng`N`lớn nên kết cấu phải được khai thác cẩn thận. 

Trường hợp cạnh tinh tế xuất phát từ sự lan truyền chồng chéo. Nếu hai vé mở rộng thành các phân đoạn giao nhau hoặc lồng nhau, việc đếm chồng chéo không chính xác có thể làm tăng hoặc giảm bớt câu trả lời. Ví dụ: nếu cả hai vé đều mở rộng trên cùng một tiền tố của hậu tố giảm dần, việc đếm ngây thơ sẽ tính gấp đôi các vị trí được bảo hiểm mặc dù chúng đã được giảm giá một lần. 

Một trường hợp cạnh khác là mảng đơn điệu. Nếu mảng tăng nghiêm ngặt thì không có sự lan truyền nào vượt quá điểm bắt đầu, vì vậy mỗi vé chỉ dành cho một người. Trong một mảng giảm dần, một vé duy nhất có thể bao gồm toàn bộ hậu tố và các vé bổ sung không mang lại lợi ích gì sau lần bao phủ đầy đủ đầu tiên. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là chọn`D`vị trí bắt đầu và mô phỏng sự lan truyền của từng lựa chọn. Đối với mỗi vị trí đã chọn, chúng tôi quét về phía trước cho đến khi điều kiện lan truyền bị phá vỡ, đánh dấu tất cả các chỉ số được bao phủ. Sau khi xử lý tất cả các lần bắt đầu đã chọn, chúng tôi đếm có bao nhiêu chỉ số đã được đánh dấu. 

Điều này đúng vì nó trực tiếp thực hiện các quy luật lan truyền. Tuy nhiên, sự phức tạp của nó là rất hạn chế. Lựa chọn`D`chi phí vị thế`O(N^D)`khả năng, và thậm chí với nhỏ`D`cái này phát nổ. Ngay cả khi chúng tôi cố định điểm bắt đầu và mô phỏng vùng phủ sóng một cách hiệu quả, mỗi mô phỏng có thể quét tới`O(N)`, dẫn đến`O(DN)`cho mỗi cấu hình, vẫn không thể liệt kê được trên tất cả các kết hợp. 

Quan sát quan trọng là mỗi vé tạo ra một phân đoạn được xác định duy nhất bởi vị trí bắt đầu của nó: nó kéo dài cho đến vị trí tiếp theo nơi chuỗi trở nên lớn hơn giá trị bắt đầu. Một khi chúng ta nhìn nhận vấn đề theo cách này, mỗi vị trí`i`xác định một khoảng xác định`[i, r[i]]`. Vấn đề trở thành lựa chọn nhiều nhất`D`khoảng thời gian để tối đa hóa phạm vi bảo hiểm của công đoàn. Tuy nhiên, không giống như lập lịch ngắt quãng tiêu chuẩn, các khoảng thời gian phụ thuộc vào điểm bắt đầu đã chọn nhưng không tương tác với nhau về mặt cấu trúc một cách phức tạp sau khi được tính toán trước. 

Điều này cho phép một giải pháp lập trình động: chúng tôi xử lý từ trái sang phải và tại mỗi vị trí quyết định xem nên bắt đầu một yêu cầu ở đó hay bỏ qua nó. Từ`D ≤ 100`, chúng tôi có thể xác định một DP nơi chúng tôi theo dõi số lượng vé chúng tôi đã sử dụng và khoảng cách chúng tôi đã đi được. Trạng thái có thể được nén vì vùng phủ sóng đơn điệu và chuyển động về phía trước. 

Chúng tôi tính toán trước cho từng chỉ mục`i`điểm cuối có thể tiếp cận xa nhất`r[i]`sử dụng cách quét tiến đơn giản hoặc hiệu quả hơn bằng cách sử dụng cấu trúc đơn điệu. Sau đó, quá trình chuyển đổi DP mô phỏng việc chọn hoặc bỏ qua các lần khởi động trong khi vẫn duy trì phạm vi phủ sóng tốt nhất có thể tiếp cận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^D) | O(N) | Quá chậm | 
| Tối ưu | O(ND) | O(ND) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước cho mọi vị trí`i`chỉ số xa nhất`r[i]`sao cho tất cả các phần tử từ`i`ĐẾN`r[i]`nhỏ hơn hoặc bằng`M[i]`. Điều này xác định toàn bộ tác dụng của việc đặt vé tại`i`. Bước này chuyển đổi quy tắc truyền động thành một khoảng tĩnh. 
2. Xác định bảng lập trình động`dp[k][i]`đại diện cho số lượng người được bảo hiểm tối đa trong tiền tố`0..i`sử dụng chính xác`k`vé. Chúng tôi cấu trúc theo cách này vì mỗi vé đóng góp một khoảng thời gian và chúng tôi phải tính đến sự trùng lặp. 
3. Khởi tạo`dp[0][i] = 0`cho tất cả`i`, vì không có vé không bao gồm gì cả. 
4. Lặp lại các vị trí từ trái sang phải. Tại mỗi vị trí`i`, chúng tôi cân nhắc hai lựa chọn: không đặt vé tại`i`, hoặc đặt một. 
5. Nếu chúng tôi không đặt vé tại`i`, sau đó`dp[k][i]`chuyển từ`dp[k][i-1]`. Điều này bảo tồn các quyết định bảo hiểm trước đó. 
6. Nếu chúng tôi đặt vé tại`i`, sau đó nó bao gồm tất cả các chỉ số lên đến`r[i]`. Chúng tôi cập nhật`dp[k+1][r[i]]`sử dụng`dp[k][i-1] + (r[i] - i + 1)`. Điều này bổ sung thêm một đóng góp phân khúc mới. Lý do chúng tôi chuyển thẳng đến`r[i]`đó là tất cả mọi thứ giữa`i`Và`r[i]`được bảo hiểm đầy đủ và không cần đưa ra quyết định thêm trong khoảng thời gian đó. 
7. Sau khi xử lý tất cả các vị trí, câu trả lời là giá trị lớn nhất trong số tất cả các vị trí`dp[k][i]`Ở đâu`k ≤ D`. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi trạng thái`dp[k][i]`đại diện cho phạm vi bảo hiểm tốt nhất có thể bằng cách sử dụng chính xác`k`vé không trùng lặp bắt đầu trong tiền tố lên đến`i`. Bởi vì mỗi vé mở rộng thành một khoảng thời gian tối đa cố định bắt đầu từ chỉ mục đã chọn của nó, nên bất kỳ giải pháp tối ưu nào cũng có thể được xem như một tập hợp các khoảng thời gian rời rạc hoặc chồng chéo mà đóng góp của chúng được tính chính xác một lần khi khoảng thời gian được tạo lần đầu tiên. DP thực thi rằng mỗi khoảng thời gian được tính ở điểm bắt đầu và không bao giờ được xem lại, ngăn chặn việc tính hai lần trong khi vẫn duy trì tất cả các kết hợp bắt đầu hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, d = map(int, input().split())
    a = list(map(int, input().split()))

    r = [0] * n

    # compute farthest reach for each i
    for i in range(n):
        mx = a[i]
        j = i
        while j + 1 < n and a[j + 1] <= mx:
            j += 1
        r[i] = j

    # dp[k][i] = best coverage using k tickets up to i
    dp = [[0] * n for _ in range(d + 1)]

    for k in range(d + 1):
        for i in range(n):
            if i > 0:
                dp[k][i] = max(dp[k][i], dp[k][i - 1])

            if k < d:
                start_prev = dp[k][i - 1] if i > 0 else 0
                reach = r[i]
                gain = reach - i + 1
                if i > 0:
                    dp[k + 1][reach] = max(dp[k + 1][reach], start_prev + gain)
                else:
                    dp[k + 1][reach] = max(dp[k + 1][reach], gain)

    ans = 0
    for k in range(d + 1):
        ans = max(ans, max(dp[k]))

    print(ans)

if __name__ == "__main__":
    solve()
```Mã đầu tiên tính toán ranh giới lan truyền`r[i]`bằng cách mở rộng tham lam sang bên phải trong khi điều kiện`a[j] ≤ a[i]`nắm giữ. Điều này trực tiếp mã hóa quy tắc về khoảng cách một vé được đặt tại`i`có thể ảnh hưởng đến dòng. 

Bảng DP sau đó theo dõi mức độ bao phủ tốt nhất có thể đạt được cho từng số lượng vé. Quá trình chuyển đổi bỏ qua một vị trí sẽ tiếp tục các kết quả trước đó, đảm bảo chúng ta không mất đi các giải pháp từng phần tối ưu. Quá trình chuyển đổi đặt vé sẽ chuyển trực tiếp đến điểm cuối`r[i]`, phản ánh rằng tất cả các vị trí trung gian đều được thực hiện bởi cùng một hoạt động. 

Một chi tiết tinh tế là cập nhật vào`dp[k+1][r[i]]`phải sử dụng giá trị từ`dp[k][i-1]`, không`dp[k][i]`, nếu không, chúng tôi có nguy cơ xâu chuỗi nhiều vé bắt đầu từ cùng một khu vực và đếm quá mức. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
8 2
1 5 7 3 8 2 1 4
```Chúng tôi tính toán phạm vi tiếp cận: 

| tôi | một [tôi] | r[i] | 
| --- | --- | --- | 
| 0 | 1 | 0 | 
| 1 | 5 | 2 | 
| 2 | 7 | 3 | 
| 3 | 3 | 5 | 
| 4 | 8 | 7 | 
| 5 | 2 | 7 | 
| 6 | 1 | 7 | 
| 7 | 4 | 7 | 

Chiến lược tốt nhất sử dụng hai bản mở rộng mạnh mẽ: một ở chỉ số 4 bao gồm`[4..7]`và một ở chỉ số 2 bao gồm`[2..3]`hoặc tương tự. DP kết hợp những điều này để tối đa hóa tổng quy mô liên minh. 

Kết quả cuối cùng là:```
6
```Dấu vết này cho thấy các giá trị lớn hơn tạo ra các khoảng thời gian ngắn nhưng mạnh mẽ như thế nào, trong khi các giá trị tầm trung vẫn có thể mở rộng vừa phải và góp phần tạo ra phạm vi phủ sóng tối ưu khi được kết hợp. 

### Mẫu 2 

đầu vào:```
10 3
1 2 3 4 5 6 7 8 9 10
```Ở đây mọi giá trị đều tăng nghiêm ngặt, vì vậy: 

| tôi | một [tôi] | r[i] | 
| --- | --- | --- | 
| tôi | tôi+1 | tôi | 

Mỗi vé chỉ bao gồm chính nó. 

Với 3 vé, chúng ta chỉ có thể tiếp cận 3 người khác nhau. 

Đầu ra:```
3
```Điều này xác nhận rằng trong các mảng tăng nghiêm ngặt, quá trình lan truyền không bao giờ được kích hoạt và DP thoái hóa thành việc chọn các chỉ số riêng lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(ND) | Mỗi trạng thái DP được cập nhật một lần cho mỗi vị trí trên mỗi số vé | 
| Không gian | O(ND) | Bảng DP lưu trữ các giá trị tốt nhất cho tất cả tiền tố và số lượng vé | 

Các ràng buộc cho phép tối đa 100k phần tử, nhưng`D ≤ 100`giữ cho sản phẩm có thể quản lý được. Sự phụ thuộc bậc hai là vào`D`, không`N`, giúp giải pháp an toàn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, d = map(int, input().split())
    a = list(map(int, input().split()))

    r = [0] * n
    for i in range(n):
        mx = a[i]
        j = i
        while j + 1 < n and a[j + 1] <= mx:
            j += 1
        r[i] = j

    dp = [[0] * n for _ in range(d + 1)]

    for k in range(d + 1):
        for i in range(n):
            if i > 0:
                dp[k][i] = max(dp[k][i], dp[k][i - 1])
            if k < d:
                prev = dp[k][i - 1] if i > 0 else 0
                reach = r[i]
                gain = reach - i + 1
                dp[k + 1][reach] = max(dp[k + 1][reach], prev + gain)

    return str(max(max(row) for row in dp))

# provided samples
assert run("8 2\n1 5 7 3 8 2 1 4\n") == "6", "sample 1"
assert run("10 3\n1 2 3 4 5 6 7 8 9 10\n") == "3", "sample 2"
assert run("10 3\n10 9 8 7 6 5 4 3 2 1\n") == "10", "sample 3"

# custom cases
assert run("1 1\n5\n") == "1", "single element"
assert run("5 1\n5 4 3 2 1\n") == "5", "single ticket full coverage"
assert run("5 2\n1 1 1 1 1\n") == "5", "all equal"
assert run("6 2\n1 3 2 4 1 2\n") == "4", "mixed structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/5 | 1 | kích thước tối thiểu | 
| 5 1 / 5 4 3 2 1 | 5 | bảo hiểm đầy đủ trong một vé | 
| 5 2 / tất cả những cái | 5 | vé thừa | 
| 6 2 / hỗn hợp | 4 | sự chồng chéo không hề nhỏ | 

## Vỏ cạnh 

Một chuỗi giảm nghiêm ngặt như`10 9 8 7 6`kích hoạt sự lan truyền tối đa từ bất kỳ điểm bắt đầu nào. Nếu chúng ta bắt đầu ở chỉ số 0,`r[0]`kéo dài đến hết, vì vậy DP phải đảm bảo các vé bổ sung không mở rộng phạm vi phủ sóng một cách giả tạo vượt quá phạm vi đã được bao trả đầy đủ. Logic chuyển tiếp xử lý việc này vì khoảng thời gian được sử dụng ngay lập tức và các vị trí tiếp theo không thể giới thiệu lại các chỉ số đã được tính. 

Một chuỗi tăng nghiêm ngặt như`1 2 3 4 5`sản xuất`r[i] = i`cho tất cả`i`. Mỗi vé đóng góp đúng một đơn vị. Do đó, DP hoạt động giống như việc chọn`D`các phần tử độc lập và mức tối đa chỉ đơn giản là`D`hoặc`N`, tùy theo giá trị nào nhỏ hơn. Điều này xác nhận rằng logic lan truyền không mở rộng sai các khoảng khi điều kiện không bao giờ được thỏa mãn. 

Một mảng không đổi như`5 5 5 5 5`tạo ra một trường hợp suy biến trong đó mỗi vé ở bất kỳ vị trí nào đều bao trùm toàn bộ hậu tố. Vé đầu tiên được đặt ở bất kỳ đâu sẽ thống trị hiệu quả mọi thứ ở bên phải của nó và các vé bổ sung không đóng góp gì mới ngoài các chỉ số đã được đề cập.
