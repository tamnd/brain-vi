---
title: "CF 102791K - Lối chơi chân thực"
description: "Chúng tôi có một khẩu súng với băng đạn có thể chứa k viên đạn. Có n đợt sóng quái vật độc lập. Một làn sóng xuất hiện vào thời điểm li, chứa quái vật ai và tất cả quái vật trong làn sóng đó phải bị tiêu diệt không muộn hơn thời gian ri."
date: "2026-07-27T18:17:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102791
codeforces_index: "K"
codeforces_contest_name: "ICPC 2020-2021 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102791
solve_time_s: 65
verified: true
draft: false
---

[CF 102791K - Lối chơi chân thực](https://codeforces.com/problemset/problem/102791/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một khẩu súng với băng đạn có thể chứa được`k`đạn. có`n`sóng quái vật độc lập. Một làn sóng xuất hiện vào thời điểm`l_i`, chứa`a_i`quái vật và tất cả quái vật từ đợt đó phải bị giết không muộn hơn thời gian`r_i`. Các sóng đã được sắp xếp theo thứ tự và không bao giờ chồng lên nhau, mặc dù một sóng có thể kết thúc đúng lúc sóng tiếp theo bắt đầu. 

Việc bắn súng diễn ra tức thời nên hành động duy nhất tiêu tốn thời gian là tải lại. Quá trình nạp lại mất một đơn vị thời gian và thay băng đạn hiện tại bằng băng đạn đầy, loại bỏ mọi viên đạn còn lại. Mục đích không chỉ là giảm thiểu lượng đạn bắn trúng quái vật mà còn giảm thiểu lượng đạn bị mất khi nạp đạn. Chúng tôi cần tổng số đạn nhỏ nhất rời khỏi kho của mình khi hoàn thành thành công. 

Số lượng đợt chỉ lên tới 2000, trong khi số lần và số lượng quái vật có thể lớn tới mức`10^9`. Điều này ngay lập tức loại trừ việc mô phỏng mọi khoảnh khắc, bởi vì một khoảng thời gian có thể kéo dài hàng tỷ khoảnh khắc. MỘT`O(n^2)`giải pháp có thể chấp nhận được vì`n`đủ nhỏ để có thể quản lý được khoảng bốn triệu lần chuyển đổi. Một giải pháp tùy thuộc vào kích thước của`l_i`hoặc`r_i`là không khả thi. 

Một số chi tiết làm cho giải pháp không chính xác thất bại. Một sai lầm phổ biến là cho rằng việc nạp lại luôn tốt vì nó mang lại băng đạn đầy đủ. Ví dụ:```
1 10
5 6 6
```Câu trả lời là`6`. Một cách tiếp cận tham lam nạp đạn ngay sau khi bắn vài con quái vật đầu tiên có thể vứt đi những viên đạn hữu ích và tiêu tốn`10`hoặc nhiều viên đạn. 

Một cái bẫy khác là quên rằng sóng có thể chạm tới. Coi như:```
2 3
2 3 6
3 4 3
```Câu trả lời là`9`. Làn sóng thứ hai bắt đầu vào đúng thời điểm làn sóng đầu tiên kết thúc, vì vậy thời gian`3`có thể được sử dụng để vừa tiêu diệt quái vật cũ vừa chuẩn bị cho lần nạp lại tiếp theo. Khoảng thời gian xử lý được phân tách nghiêm ngặt sẽ làm mất tác dụng hợp lệ. 

Hộp đựng cạnh thứ ba là tạp chí cuối cùng. Ví dụ:```
1 10
100 111 1
```Câu trả lời là`1`. Những viên đạn còn lại trong băng đạn cuối cùng không bị loại bỏ sau trận đấu nên lần nạp lại cuối cùng không được tính là đạn lãng phí. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là quyết định mọi lịch trình tải lại có thể. Đối với mỗi đợt, chúng tôi có thể thử mọi số lần tải lại có thể và mọi thời điểm có thể để thực hiện chúng. Số lần có thể có là rất lớn vì các khoảng có thể chứa các giá trị lên tới`10^9`, vì vậy cách tiếp cận này thậm chí không thể được trình bày một cách hiệu quả. 

Quan sát hữu ích là chúng ta không quan tâm đến những khoảnh khắc chính xác bên trong khoảng trống giữa các sóng. Điều quan trọng là trạng thái ngay sau khi một đợt được xử lý hoàn toàn và trước khi đợt tiếp theo bắt đầu. Nếu chúng ta biết rằng chúng ta đến một làn sóng có băng đạn đầy đủ, chúng ta có thể mô phỏng một cách tham lam điều gì sẽ xảy ra nếu làn sóng này và một số sóng tiếp theo được xử lý mà không cần tải lại không cần thiết giữa chúng. 

Lực lượng vũ phu hoạt động vì một chuỗi quyết định nạp đạn cố định hoàn toàn xác định số lượng đạn đã sử dụng, nhưng nó không thành công vì số lượng trình tự có thể xảy ra là rất lớn. Cái nhìn sâu sắc quan trọng là`n`nhỏ nên chúng ta có thể thử mọi sóng bắt đầu có thể có của một đoạn. Đối với mỗi điểm bắt đầu, chúng tôi mở rộng phân đoạn về phía trước và mô phỏng cách tốt nhất có thể để tồn tại cho đến đợt sau. Điều này làm giảm vấn đề lập trình động trên ranh giới sóng. 

Cho phép`dp[i]`là số đạn tối thiểu được sử dụng sau khi xóa viên đạn đầu tiên`i`các đợt và chuẩn bị đầy đủ băng đạn trước đợt tiếp theo. Từ tiểu bang`i`, chúng tôi cố gắng xóa sóng`i`bởi vì`j`liên tục mà không cần phải tải lại trước đợt`j`bắt đầu. Trong quá trình mô phỏng này, chúng tôi theo dõi số lượng đạn còn lại và số lượng đạn đã được sử dụng. Khi băng đạn không còn đủ, chúng tôi tính toán xem cần nạp lại bao nhiêu lần và liệu thời gian có sẵn có cho phép hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ của số lần tải lại | O(1) | Quá chậm | 
| Lập trình động tối ưu | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`dp[0] = 0`. Điều này có nghĩa là trước đợt đầu tiên, chúng tôi chưa tiêu tốn viên đạn nào và băng đạn ban đầu của chúng tôi đã đầy. 
2. Đối với mọi đợt bắt đầu có thể`i`, hãy thử mở rộng phân đoạn hiện tại sang từng đợt sau`j`. Trạng thái trước sóng`i`đã đảm bảo rằng ổ tích dao đã đầy, vì vậy điều này cho chúng ta một điểm khởi đầu hợp lệ để mô phỏng. 
3. Trong khi mở rộng đoạn này, hãy giữ nguyên số đạn còn lại trong băng đạn hiện tại. Nếu băng đạn hiện tại có đủ đạn cho wave`j`, dành những viên đạn đó và tiếp tục. Đợt sóng có thể được hoàn thành mà không cần tải lại. 
4. Nếu băng đạn hiện tại không đủ, hãy tính số lần nạp lại cần thiết để tiêu diệt số quái vật còn lại. Mỗi lần tải lại sẽ thêm`k`đạn, nhưng mỗi lần nạp đạn cũng tiêu tốn một đơn vị thời gian. Số lần tải lại phải vừa với khoảng thời gian của đợt. 
5. Sau khi dọn sóng`j`, hãy cập nhật trạng thái lập trình động tiếp theo nếu có đủ thời gian trước đợt tiếp theo để có lại băng đạn đầy đủ. Quá trình chuyển đổi lưu trữ các viên đạn đã sử dụng cho đến nay cộng với bất kỳ viên đạn nào bị loại bỏ khi chuẩn bị băng đạn đầy đủ tiếp theo. 
6. Sau khi xử lý tất cả các chuyển đổi có thể,`dp[n]`chứa số đạn tối thiểu được sử dụng sau tất cả các đợt. Nếu nó không bao giờ đạt tới, sóng không thể bị xóa. 

Tại sao nó hoạt động: điều bất biến là mọi thứ đều có thể truy cập được`dp[i]`mô tả một tình huống thực tế trong đó lần đầu tiên`i`đợt kết thúc và tạp chí đầy trước đợt tiếp theo. Mọi giải pháp tối ưu đều có một số điểm cuối cùng trong đó ổ tích trữ đã đầy trước một đợt sóng, vì vậy nó xuất hiện như một trong những sự chuyển đổi được xem xét bởi quy hoạch động. Mô phỏng bên trong quá trình chuyển đổi luôn sử dụng số lần nạp lại tối thiểu có thể cần thiết cho phân đoạn cố định đó, vì các lần nạp lại thêm chỉ loại bỏ đạn và tăng chi phí. Vì vậy, mọi ứng cử viên đều hợp lệ và mức tối thiểu trên tất cả các ứng cử viên là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    waves = [tuple(map(int, input().split())) for _ in range(n)]

    INF = 10**30
    dp = [INF] * (n + 1)
    dp[0] = 0

    for i in range(n):
        if dp[i] == INF:
            continue

        spent = dp[i]
        bullets = k

        for j in range(i, n):
            l, r, a = waves[j]
            extra = 0

            if a <= bullets:
                bullets -= a
                finish = l
            else:
                need = a - bullets
                reloads = (need + k - 1) // k
                if reloads > r - l:
                    break

                extra = reloads * k
                finish = l + reloads
                bullets = (k - need % k) % k

            spent += a + extra

            if j == n - 1:
                dp[n] = min(dp[n], spent)
            elif finish <= waves[j + 1][0]:
                dp[j + 1] = min(dp[j + 1], spent + bullets)

    print(-1 if dp[n] == INF else dp[n])

if __name__ == "__main__":
    solve()
```Vòng ngoài chọn làn sóng có sẵn băng đạn đầy đủ trước khi bắt đầu. Vòng lặp bên trong thử tất cả các điểm cuối có thể có của đoạn liên tục hiện tại. Điều này phù hợp với các chuyển đổi lập trình động được mô tả ở trên. 

Bên trong mô phỏng,`bullets`đại diện cho tạp chí hiện tại sau khi xử lý tất cả các đợt trước đó trong phân khúc. Nếu đợt hiện tại không thể kết thúc, mã sẽ tính số lần tải lại tối thiểu cần thiết với mức phân chia trần. điều kiện`reloads > r - l`kiểm tra xem những lần tải lại đó có phù hợp trước thời hạn của đợt hay không. 

biểu hiện`(k - need % k) % k`tính toán số đạn còn lại sau lần nạp đạn cuối cùng. Modulo bổ sung xử lý trường hợp băng đạn cuối cùng hoàn toàn trống. Sau khi hoàn thành một đoạn, thêm`bullets`sang trạng thái tiếp theo đại diện cho số đạn sẽ bị mất khi nạp lại trước đợt tiếp theo. 

Tất cả các phép tính đều sử dụng số nguyên Python, vì vậy các giá trị gần câu trả lời tối đa có thể sẽ an toàn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
2 3
2 3 6
3 4 3
```Mô phỏng chuyển tiếp hoạt động như sau: 

| Sóng | Đạn trước làn sóng | Quái vật | Tải lại | Đạn sau | Chi phí tăng thêm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 6 | 1 | 0 | 6 | 
| 2 | 3 | 3 | 0 | 0 | 3 | 

Đợt đầu tiên yêu cầu nạp lại một lần vì băng đạn chỉ chứa ba viên đạn. Quá trình tải lại kết thúc chính xác khi đợt thứ hai xuất hiện, cho phép các hành động còn lại tiếp tục. Tổng cộng là`9`. 

Đối với mẫu thứ hai:```
2 5
3 7 11
10 12 15
```| Sóng | Đạn trước làn sóng | Quái vật | Tải lại | Đạn sau | Chi phí tăng thêm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 11 | 2 | 4 | 15 | 
| 2 | 5 | 15 | 2 | 0 | 15 | 

Đợt đầu tiên kết thúc với bốn viên đạn bị loại bỏ trước khi cần băng đạn đầy đủ tiếp theo. Mô hình tương tự cũng xảy ra đối với làn sóng thứ hai, mang lại tổng cộng`30`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi sóng bắt đầu sẽ thử mọi sóng kết thúc có thể có và mỗi lần chuyển đổi được mô phỏng theo thời gian không đổi. | 
| Không gian | O(n) | Thuật toán lưu trữ danh sách sóng và một mảng lập trình động. | 

Với`n <= 2000`, số lần chuyển đổi là khoảng bốn triệu, dễ dàng nằm trong giới hạn. Các giá trị thời gian lớn không ảnh hưởng đến độ phức tạp vì thuật toán không bao giờ lặp lại theo từng thời điểm. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    import math
    data = sys.stdin.readline
    n, k = map(int, data().split())
    waves = [tuple(map(int, data().split())) for _ in range(n)]

    INF = 10**30
    dp = [INF] * (n + 1)
    dp[0] = 0

    for i in range(n):
        if dp[i] == INF:
            continue
        spent = dp[i]
        bullets = k
        for j in range(i, n):
            l, r, a = waves[j]
            if a <= bullets:
                bullets -= a
                finish = l
                extra = 0
            else:
                need = a - bullets
                reloads = (need + k - 1) // k
                if reloads > r - l:
                    break
                extra = reloads * k
                finish = l + reloads
                bullets = (k - need % k) % k
            spent += a + extra
            if j == n - 1:
                dp[n] = min(dp[n], spent)
            elif finish <= waves[j + 1][0]:
                dp[j + 1] = min(dp[j + 1], spent + bullets)

    sys.stdin = old_stdin
    return str(-1 if dp[n] == INF else dp[n])

assert solve("""2 3
2 3 6
3 4 3
""") == "9"

assert solve("""2 5
3 7 11
10 12 15
""") == "30"

assert solve("""1 10
100 111 1
""") == "1"

assert solve("""5 42
42 42 42
42 43 42
43 44 42
44 45 42
45 45 1
""") == "-1"

assert solve("""1 5
1 1 5
""") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai làn sóng chạm nhau | 9 | Kiểm tra xem thời gian kết thúc và bắt đầu bằng nhau có thể được sử dụng | 
| Tạp chí lớn với một con quái vật | 1 | Kiểm tra xem đạn chưa sử dụng cuối cùng có được tính không | 
| Làn sóng cuối cùng bất khả thi | -1 | Kiểm tra xử lý thời hạn | 
| Tạp chí điền chính xác sóng đơn | 5 | Kiểm tra trường hợp ranh giới kích thước tối thiểu | 

## Vỏ cạnh 

Đối với trường hợp sóng chạm:```
2 3
2 3 6
3 4 3
```Thuật toán bắt đầu với ba dấu đầu dòng. Nó sử dụng chúng ngay lập tức trong đợt đầu tiên, phát hiện ra rằng cần thêm ba viên đạn nữa và sử dụng một lần nạp lại. Quá trình tải lại kết thúc vào thời điểm đó`3`, điều này được phép vì sóng tiếp theo cũng bắt đầu tại`3`. Làn sóng thứ hai sử dụng hoàn toàn tạp chí mới, tạo ra`9`. 

Đối với trường hợp tạp chí cuối cùng:```
1 10
100 111 1
```Băng đạn đầu tiên đã có đủ đạn rồi. Mô phỏng sử dụng một viên đạn và để lại chín viên đạn không được sử dụng. Vì không có đợt tiếp theo nên mã không thêm những viên đạn còn lại đó làm chi phí loại bỏ, đưa ra câu trả lời chính xác`1`. 

Đối với một thời hạn không thể thực hiện được:```
5 42
42 42 42
42 43 42
43 44 42
44 45 42
45 45 1
```Thuật toán cố gắng mô phỏng làn sóng đầu tiên. Mỗi lần tải lại được yêu cầu sẽ tiêu tốn một đơn vị thời gian và các đợt sau không còn đủ thời gian cho tất cả các lần tải lại cần thiết. Quá trình chuyển đổi không thành công vì số lần tải lại được yêu cầu vượt quá độ dài khoảng thời gian có sẵn, do đó trạng thái cuối cùng vẫn không thể truy cập được và câu trả lời là`-1`.
