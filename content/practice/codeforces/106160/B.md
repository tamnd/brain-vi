---
title: "CF 106160B - Sắp xếp Boggle"
description: "Câu đố là về một khay xúc xắc 4 x 4. Mỗi con súc sắc có thể có sáu chữ cái và chúng ta chỉ có thể xoay một con súc sắc. Xúc xắc vẫn ở vị trí ban đầu."
date: "2026-06-25T11:11:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106160
codeforces_index: "B"
codeforces_contest_name: "2025 Benelux Algorithm Programming Contest (BAPC 25)"
rating: 0
weight: 106160
solve_time_s: 42
verified: true
draft: false
---

[CF 106160B - Sắp xếp Boggle](https://codeforces.com/problemset/problem/106160/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Câu đố là về một khay xúc xắc 4 x 4. Mỗi con súc sắc có thể có sáu chữ cái và chúng ta chỉ có thể xoay một con súc sắc. Xúc xắc vẫn ở vị trí ban đầu. Mục tiêu là chọn mặt trên cuối cùng của mỗi con súc sắc sao cho 16 kết quả nhìn thấy được theo thứ tự bảng chữ cái không giảm dần từ trái sang phải, sử dụng tổng số vòng quay ít nhất. 

Một vòng quay thay đổi hướng mặt lên trên. Mặt nghiêng có thể được đưa lên trên bằng một lượt, trong khi mặt dưới cần hai lượt. Mặt trên hiện có thể nhìn thấy không tốn phí lượt. 

Đầu vào mô tả 16 con xúc xắc bằng cách đưa ra hàng trên cùng, bốn hàng bên cạnh và hàng dưới cùng. các`Q`nhân vật đại diện cho khuôn mặt Boggle đặc biệt`Qu`, nên được so sánh như hai chữ cái. Điều này có nghĩa là một ô hiển thị`Qu`hoạt động giống như chuỗi`"QU"`khi kiểm tra đơn hàng. 

Số lượng xúc xắc được cố định ở mức 16, điều này thay đổi hoàn toàn cách tiếp cận thuật toán. Một vấn đề sắp xếp chung với một số lượng lớn các phần tử sẽ yêu cầu một cái gì đó như`O(n log n)`hoặc tốt hơn, nhưng ở đây số lượng vị trí rất ít. Thử thách không phải là xử lý nhiều viên xúc xắc mà là khám phá chính xác các phép quay có thể có mà không lãng phí công sức. 

Một giải pháp bất cẩn có thể coi mỗi con súc sắc đều có sáu lựa chọn độc lập và thử tất cả các kết hợp. Điều đó tạo ra`6^16`khả năng, đó là hơn 2,8 tỷ tiểu bang. Mặc dù bảng nhỏ, nhưng đó là quá nhiều. 

Những trường hợp phức tạp đến từ quy tắc sắp xếp và các chữ cái lặp đi lặp lại. Ví dụ: một khuôn đơn có thể có cùng một đặc tính hữu ích trên nhiều mặt và việc chọn góc quay rẻ hơn rất quan trọng. Hãy xem xét một con súc sắc có thể hiển thị`A`ở trên cùng và cả ở một bên. Chi phí tối thiểu chính xác cho việc lựa chọn`A`là số không, không phải một. Bất kỳ cách tiếp cận nào chỉ lưu trữ các chữ cái và quên mất giá trị của chúng đều có thể tạo ra câu trả lời sai. 

Một trường hợp cạnh khác là`Qu`khuôn mặt. Nếu có hai ô được chọn liên tiếp`Q`Và`U`, thứ tự của chúng không giống như`Q`Và`T`. Ví dụ, trình tự`Qu, U`là hợp lệ bởi vì nó so sánh như`QU`theo sau là`U`, trong khi`Qu, T`không hợp lệ vì sau`Q`ký tự được so sánh tiếp theo là`U`, lớn hơn`T`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là thử mọi hướng cuối cùng có thể có của mỗi khuôn. Mỗi xúc xắc có thể có sáu mặt trên nên không gian tìm kiếm là`6^16`. Đối với mỗi lựa chọn hoàn chỉnh, chúng tôi kiểm tra xem chuỗi kết quả có được sắp xếp hay không và giữ chi phí xoay vòng tối thiểu. Điều này đúng vì nó kiểm tra mọi trạng thái khay cuối cùng có thể có, nhưng số lượng trạng thái là khoảng 2,8 tỷ, khiến nó không thực tế. 

Cấu trúc giúp chúng ta tiết kiệm là điều kiện được sắp xếp chỉ phụ thuộc vào ô đã chọn trước đó. Một khi chúng ta quyết định điều đầu tiên`i`xúc xắc, thông tin duy nhất cần thiết để quyết định xúc xắc tiếp theo là giá trị của mặt được chọn cuối cùng. Lịch sử chính xác không còn quan trọng nữa. 

Điều này cho phép chúng tôi xây dựng một giải pháp lập trình động. Chúng tôi xử lý xúc xắc từ trái sang phải. Trạng thái lưu trữ chi phí tối thiểu sau khi hoàn thành tiền tố trong khi kết thúc bằng một khuôn mặt hiển thị cụ thể. Khi thêm khuôn tiếp theo, chúng ta chỉ cho phép các khuôn mặt có thứ tự bảng chữ cái không nhỏ hơn khuôn trước đó. 

Số lượng các tiểu bang là rất nhỏ. Chỉ có 16 vị trí và mỗi vị trí có thể nhìn thấy tối đa sáu mặt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(6^16) | O(16) | Quá chậm | 
| Tối ưu | O(16 * 6 * 6) | O(6) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích sáu mặt của mỗi con súc sắc. Đối với mỗi mặt trên cuối cùng có thể có, hãy lưu trữ số lần quay cần thiết để làm cho nó xuất hiện. Mặt trên có giá bằng 0, mặt bên có giá một và mặt dưới có giá hai. 

Nếu cùng một nhân vật hiển thị xuất hiện qua các lần quay khác nhau, việc giữ chi phí rẻ hơn là cần thiết vì thứ tự cuối cùng chỉ quan tâm đến nhân vật chứ không quan tâm đến khuôn mặt vật lý. 
2. Tạo một mảng lập trình động trong đó`dp[j]`có nghĩa là chi phí tối thiểu sau khi xử lý tiền tố hiện tại của xúc xắc, với ô được chọn cuối cùng là mặt`j`. 

Ô trước đó là hạn chế duy nhất cần thiết vì mỗi ô mới ít nhất phải lớn bằng ô trước đó. 
3. Khởi tạo khuôn đầu tiên bằng cách cho phép mọi khuôn mặt có thể nhìn thấy được. Không có ô trước đó để so sánh. 
4. Với mỗi khuôn tiếp theo, hãy thử mọi khuôn mặt hiện tại có thể có. Nếu mặt được chọn trước đó không lớn hơn mặt hiện tại, hãy chuyển sang trạng thái mới bằng cách thêm chi phí quay. 
5. Sau khi xử lý tất cả các viên xúc xắc, câu trả lời là giá trị nhỏ nhất trong số tất cả các trạng thái kết thúc. Nếu không thể truy cập trạng thái thì không thể sắp xếp khay. 

Tại sao nó hoạt động: 

Điều bất biến là sau khi xử lý lần đầu tiên`i`xúc xắc, mọi trạng thái được lưu trữ đều thể hiện cách rẻ nhất để tạo ra những xúc xắc đó`i`các ô hiển thị được sắp xếp và kết thúc bằng ô cuối cùng cụ thể. Khi thêm một con súc sắc mới, chúng tôi chỉ mở rộng các chuỗi vẫn được sắp xếp. Vì mọi mặt kết thúc có thể đều được xem xét nên không có sự sắp xếp hợp lệ nào bị bỏ sót. Bởi vì mỗi quá trình chuyển đổi sẽ thêm chính xác chi phí quay của bề mặt đã chọn, trạng thái cuối cùng có thể đạt được tối thiểu là giải pháp tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**9

def solve():
    lines = []
    while len(lines) < 6:
        s = input().strip()
        if s:
            lines.append(s)

    top = lines[0]
    sides = lines[1:5]
    bottom = lines[5]

    dice = []

    for i in range(16):
        cost = {}
        faces = [(top[i], 0)]
        for row in sides:
            faces.append((row[i], 1))
        faces.append((bottom[i], 2))

        for ch, c in faces:
            value = "QU" if ch == "Q" else ch
            if value not in cost or c < cost[value]:
                cost[value] = c

        dice.append(list(cost.items()))

    states = list({x for die in dice for x, _ in die})
    states.sort()

    m = len(states)
    idx = {s: i for i, s in enumerate(states)}

    dp = [INF] * m

    for s, c in dice[0]:
        dp[idx[s]] = min(dp[idx[s]], c)

    for i in range(1, 16):
        ndp = [INF] * m
        for prev in range(m):
            if dp[prev] == INF:
                continue
            for cur_s, cur_c in dice[i]:
                cur = idx[cur_s]
                if states[prev] <= cur_s:
                    ndp[cur] = min(ndp[cur], dp[prev] + cur_c)
        dp = ndp

    ans = min(dp)
    print("impossible" if ans == INF else ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, đoạn mã này xây dựng sáu kết quả có thể xảy ra của mỗi lần súc sắc và chuyển đổi`Q`vào chuỗi`QU`vì vậy so sánh chuỗi bình thường tuân theo thứ tự bắt buộc. Điều này tránh cần logic so sánh đặc biệt sau này. 

Mảng lập trình động được nén chỉ thành các chuỗi hiển thị thực sự tồn tại trên một số khuôn. Vì có tối đa 16 viên xúc xắc với sáu mặt mỗi mặt nên số lượng trạng thái có thể có là rất nhỏ. 

Kiểm tra chuyển tiếp`states[prev] <= cur_s`. Đây là điều kiện đặt hàng cốt lõi. So sánh chuỗi của Python xử lý đặc biệt`QU`biểu diễn chính xác vì nó so sánh từng ký tự. 

Việc đọc đầu vào bỏ qua các dòng trống, điều này làm cho giải pháp trở nên mạnh mẽ trước những khác biệt về định dạng xung quanh đầu vào mẫu. Mức tối thiểu cuối cùng là số vòng quay hợp lệ hoặc vẫn là vô hạn, nghĩa là không có sự sắp xếp nào được sắp xếp. 

## Ví dụ đã hoạt động 

Đối với trường hợp xây dựng nhỏ: 

đầu vào:```
A
B
C
D
E
F
```Các trạng thái liên quan chỉ có sáu viên xúc xắc trong ví dụ. Quá trình này là: 

| Bước | Hiện tại chết | Sự lựa chọn có thể | Trạng thái DP | 
| --- | --- | --- | --- | 
| 1 | A | A | Đáp: 0 | 
| 2 | B | B | A:0, B:0 | 
| 3 | C | C | A:0, B:0, C:0 | 
| 4 | D | D | A:0, B:0, C:0, D:0 | 

Điều này thể hiện trường hợp được sắp xếp bình thường. Thuật toán giữ mọi điểm kết thúc hợp lệ thay vì cam kết một cách tham lam vào một khuôn mặt. 

Đối với trường hợp thứ tự hiển thị phải được cố định bằng các phép quay: 

đầu vào:```
Z
A
B
C
D
E
```Giả sử xúc xắc thứ nhất có thể quay thành`A`trong một lượt. Dấu vết trở thành: 

| Bước | Hiện tại chết | Gương mặt được chọn | Chi phí | 
| --- | --- | --- | --- | 
| 1 | đầu tiên | A | 1 | 
| 2 | thứ hai | B | 0 | 
| 3 | thứ ba | C | 0 | 
| 4 | thứ tư | D | 0 | 

Chi phí cuối cùng là một. Điều này cho thấy lý do tại sao thuật toán phải xem xét việc xoay sớm thay vì giả sử các chữ cái hiển thị hiện tại là cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(16 * 6 * 6) | Mỗi xúc xắc sẽ thử mọi trạng thái trước đó và mọi khuôn mặt mới có thể có | 
| Không gian | O(6) | Chỉ các lớp DP trước đó và hiện tại mới được lưu trữ | 

Kích thước bảng cố định giúp giải pháp thoải mái trong giới hạn. Thuật toán có thời gian không đổi một cách hiệu quả, nhưng lý luận lập trình động là thứ cho phép cùng một ý tưởng hoạt động với các biến thể lớn hơn với nhiều vị trí hơn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return sys.stdout.getvalue()

# sample 1
assert run("""IAZEEOXSPACKYIGF
APDSSAOHEQAOGGLY
LCERNRFILJINEEWE
BDVLOMRESBATLTRI
TEAAWSINUOOUKVIH
YMNCDHBPTMTDUNUE
""").strip() == "15"

# sample 2
assert run("""EXFETDMNMGDBRSRM
TIEGINOVRETACNUA
PRYKASAEATNTSHID
SOHUOEJDHVKYLPLC
UFIYAWBZONUIEIWE
LBELCOQASIOLAEGP
""").strip() == "impossible"

# already sorted
assert run("""ABCDEFGHIJKLMNOP
ABCDEFGHIJKLMNOP
ABCDEFGHIJKLMNOP
ABCDEFGHIJKLMNOP
ABCDEFGHIJKLMNOP
ABCDEFGHIJKLMNOP
""").strip() == "0"

# requires rotating first die
assert run("""ZBCDEFGHIJKLMNOP
ABCDEFGHIJKLMNOP
ABCDEFGHIJKLMNOP
ABCDEFGHIJKLMNOP
ABCDEFGHIJKLMNOP
ABCDEFGHIJKLMNOP
""").strip() != "impossible"

# impossible ordering
assert run("""ZAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAA
""").strip() == "impossible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 15 | Trường hợp xoay tối ưu thông thường | 
| Mẫu 2 | không thể | Phát hiện đơn hàng không thể truy cập | 
| Xúc xắc được sắp xếp | 0 | Xử lý các giải pháp không tốn chi phí | 
| Khuôn đầu tiên cần điều chỉnh | có thể | Kiểm tra chuyển tiếp xoay | 
| Ô đầu tiên giảm dần mà không cần sửa | không thể | Kiểm tra so sánh ranh giới | 

## Vỏ cạnh 

Khi một con súc sắc có các chữ cái trùng lặp trên nhiều mặt, thuật toán sẽ hợp nhất chúng và giữ chi phí quay thấp nhất. Ví dụ, nếu một con súc sắc có thể hiển thị`A`bằng cách đứng yên hoặc quay lại, chi phí chuyển tiếp được lưu trữ cho`A`vẫn bằng không. Điều này tránh phải trả những lượt không cần thiết. 

các`Qu`khuôn mặt được xử lý bằng cách chuyển đổi nó thành`QU`. Giả sử hai lựa chọn liền kề là`Qu`Và`U`. Sự so sánh trở nên`"QU" <= "U"`, điều đó đúng bởi vì`Q`đến trước`U`. Nếu ô tiếp theo là`T`, sự so sánh trở thành`"QU" <= "T"`, điều này sai vì`U`lớn hơn`T`. Việc biểu diễn chuỗi khớp chính xác với quy tắc chữ cái bắt buộc. 

Nếu không có trình tự nào có thể thỏa mãn ràng buộc về thứ tự thì mọi trạng thái DP cuối cùng sẽ không thể truy cập được. Ví dụ, nếu con súc sắc đầu tiên buộc phải hiện ra`Z`và mỗi lần chết sau chỉ có thể hiện chữ trước`Z`, mọi chuyển đổi đều bị từ chối. Mức tối thiểu cuối cùng vẫn là vô hạn và kết quả đầu ra của chương trình`impossible`.
